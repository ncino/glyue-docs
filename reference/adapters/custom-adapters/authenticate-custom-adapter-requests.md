# How to Authenticate Custom Adapter Requests

This guide shows how to authenticate the outbound requests a custom adapter makes. Custom adapters perform no authentication on your behalf. For background, see [Custom Adapters](README.md). You implement authentication in the adapter code with values from the config schema.

Each example shows the config schema fields and the adapter code for one authentication style. Adjust the field names to suit your API. Inside the adapter, `self.config` is a plain dictionary of the resolved config values, and the platform has already decrypted the `encrypted_string` and `file` fields.

## Authenticate with a Username and Password

Use this for HTTP Basic authentication. It is stateless — there is no token to fetch, store, or refresh.

**Config Schema**

<table><thead><tr><th>Field</th><th>Type</th></tr></thead><tbody><tr><td><code>base_url</code></td><td><code>string</code></td></tr><tr><td><code>username</code></td><td><code>string</code></td></tr><tr><td><code>password</code></td><td><code>encrypted_string</code></td></tr></tbody></table>

**Adapter Code**

```python
import requests


class CustomAdapter(BaseAdapter):
    def execute(self, request):
        response = AdapterResponse()
        response.success = False
        try:
            r = requests.request(
                method=request.payload.get("method", "GET"),
                url=f"{self.config['base_url']}{request.payload.get('path', '')}",
                auth=(self.config["username"], self.config["password"]),
                json=request.payload.get("body"),
                timeout=self.config.get("timeout", 30),
            )
            r.raise_for_status()
            response.payload = r.json()
            response.success = True
        except Exception as e:
            response.add_message_and_stack_trace(MessageTypes.UNCATEGORIZED_ERROR, str(e))
        return response

    def validate_config(self):
        if not self.config.get("username") or not self.config.get("password"):
            raise Exception("username and password are required")
```

When you pass `auth=(username, password)`, the `requests` library builds the `Authorization` header. The `encrypted_string` type keeps the password encrypted at rest.

## Authenticate with an API Key or Token

Use this when the API issues a long-lived key or token in advance. Store it in an `encrypted_string` field and send it on every request. The default adapter template uses this pattern.

**Config Schema**

<table><thead><tr><th>Field</th><th>Type</th></tr></thead><tbody><tr><td><code>base_url</code></td><td><code>string</code></td></tr><tr><td><code>api_key</code></td><td><code>encrypted_string</code></td></tr></tbody></table>

**Adapter Code**

```python
import requests


class CustomAdapter(BaseAdapter):
    def execute(self, request):
        response = AdapterResponse()
        response.success = False
        try:
            headers = {
                # Bearer token:
                "Authorization": f"Bearer {self.config['api_key']}",
                # Or an API-key header, if the API expects one instead:
                # "X-API-Key": self.config["api_key"],
                "Content-Type": "application/json",
            }
            r = requests.request(
                method=request.payload.get("method", "POST"),
                url=f"{self.config['base_url']}{request.payload.get('path', '')}",
                headers=headers,
                json=request.payload.get("body"),
                timeout=self.config.get("timeout", 30),
            )
            r.raise_for_status()
            response.payload = r.json()
            response.success = True
        except Exception as e:
            response.add_message_and_stack_trace(MessageTypes.UNCATEGORIZED_ERROR, str(e))
        return response

    def validate_config(self):
        if not self.config.get("api_key"):
            raise Exception("api_key is required")
```

## Authenticate with OAuth Client Credentials

{% hint style="warning" %}
A custom adapter cannot use the platform's built-in OAuth handling — the token retrieval, storage, and refresh that built-in adapters perform. You implement the flow in the adapter code. A custom adapter currently has no per-config token store. The config values it receives are a copy, and it cannot write a token back to its config at runtime, so the example that follows requests a new access token on each run. This works well for low-volume use. At high volume, every call incurs an extra token request. The authorization-code flow is not practical in a custom adapter, because it requires an interactive user redirect and callback, and an unattended server-side adapter run cannot present one.
{% endhint %}

This example implements the OAuth 2.0 client-credentials flow: it exchanges a client ID and secret for a short-lived access token, then uses that token on the request.

**Config Schema**

<table><thead><tr><th>Field</th><th>Type</th></tr></thead><tbody><tr><td><code>base_url</code></td><td><code>string</code></td></tr><tr><td><code>token_url</code></td><td><code>string</code></td></tr><tr><td><code>client_id</code></td><td><code>string</code></td></tr><tr><td><code>client_secret</code></td><td><code>encrypted_string</code></td></tr><tr><td><code>scope</code></td><td><code>string</code></td></tr></tbody></table>

**Adapter Code**

```python
import requests


class CustomAdapter(BaseAdapter):
    def _get_access_token(self):
        data = {
            "grant_type": "client_credentials",
            "client_id": self.config["client_id"],
            "client_secret": self.config["client_secret"],
        }
        if self.config.get("scope"):
            data["scope"] = self.config["scope"]

        r = requests.post(
            self.config["token_url"],
            data=data,
            headers={"Accept": "application/json"},
            timeout=self.config.get("timeout", 30),
        )
        r.raise_for_status()
        token = r.json().get("access_token")
        if not token:
            raise Exception("No access_token in token response")
        return token

    def execute(self, request):
        response = AdapterResponse()
        response.success = False
        try:
            token = self._get_access_token()
            r = requests.request(
                method=request.payload.get("method", "GET"),
                url=f"{self.config['base_url']}{request.payload.get('path', '')}",
                headers={"Authorization": f"Bearer {token}"},
                json=request.payload.get("body"),
                timeout=self.config.get("timeout", 30),
            )
            r.raise_for_status()
            response.payload = r.json()
            response.success = True
        except Exception as e:
            response.add_message_and_stack_trace(MessageTypes.UNCATEGORIZED_ERROR, str(e))
        return response

    def validate_config(self):
        for field in ("token_url", "client_id", "client_secret"):
            if not self.config.get(field):
                raise Exception(f"{field} is required")
```

Some authorization servers expect the client credentials in an HTTP Basic `Authorization` header rather than in the request body. In that case, build the header from the injected `base64` module instead of sending `client_id` and `client_secret` in `data`:

```python
creds = base64.b64encode(
    f"{self.config['client_id']}:{self.config['client_secret']}".encode()
).decode()
headers = {"Authorization": f"Basic {creds}", "Accept": "application/json"}
```

## Notes

* You implement all authentication in adapter code; the platform performs none of it on the adapter's behalf.
* Store any secret, such as a password, API key, or client secret, in an `encrypted_string` field so the platform encrypts it at rest and masks it in the interface and API.
* A custom adapter currently has no per-config token store: the config values it receives are a copy, and it cannot persist changes back to its config at runtime. The platform does not support flows that depend on storing and reusing a token across runs, such as caching an access token until it expires or the OAuth authorization-code flow; these must re-request the token each run.
