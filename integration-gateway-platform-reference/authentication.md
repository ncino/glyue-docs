# Authentication

Integration Gateway supports multiple authentication methods to facilitate calling integrations in a variety of situations.&#x20;

* [Basic Auth (username / password)](authentication.md#basic-auth)
* [OAuth 2.0 Authorization Code](authentication.md#oauth-2.0-authorization-code)
* [OAuth 2.0 Client Credentials](authentication.md#oauth-2.0-client-credentials)

## Basic Auth

Basic Auth uses a user’s credentials (username and password) that are encoded and sent with each request. While any valid user credential will work, Integration Gateway strongly encourages using a dedicated [service account](permissions/service-accounts.md) for each external service that is calling an integration.

Service accounts adhere to the principle of least privilege; they have limited abilities within the app, cannot modify integrations, and can have tightly scoped [integration permissions](permissions/#integration-permissions).

## OAuth 2.0 Authorization Code

OAuth 2.0 Authorization Code flows are used to provide access to Integration Gateway integrations on behalf of a user in another system. This typically manifests as the external system redirecting to Integration Gateway's login, which redirects back to the external system after the user logs into Integration Gateway. Permissions are governed by the logged-in user's Integration Gateway account.

Setting up an Authorization Code based flow has two parts: configuring within Integration Gateway, and configuring in the external system.

#### Integration Gateway Setup <a href="#admin-setup-glyue" id="admin-setup-glyue"></a>

1. From the admin page, find the _OAuth2.0 section._ Click on _Applications,_ then _Add Application+_
2. Save the generated _Client ID_ (top of page) and _Client secret_ (toward bottom) in a secure location. After this step, these values will not be visible again.
3. In the _Redirect URIs_ field, enter the full redirect URI (including `https://`)  from the external app.
4. Set _Client Type_ to `Confidential`
5. Set _Authorization Grant Type_ to `Authorization Code`
6. Enter the _Name_ of the external app or 3rd party
7. Save

**External Application Setup**

Specific steps differ between applications, but they will all require the following information:

* The _Client ID_ and _Client Secret_ from above
* Integration Gateway's authorization endpoint: `yourdomain.sandboxbanking.com/o/authorize/`
  * Response type: `code`
* Integration Gateway's token endpoint: `yourdomain.sandboxbanking.com/o/token/`
  * Grant type: `authorization_code`&#x20;



## OAuth 2.0 Client Credentials

OAuth 2.0 Client Credential flows are used for server-to-server communication where no user is directly involved. In contrast to the Authorization Code flow, the external application _itself_ is authorized in Integration Gateway, rather than being authorization _on behalf of_ a user.

For audit trail purposes, Integration Gateway requires that a service account is associated with each Client Credential configuration. The [integration permissions](permissions/#integration-permissions) on this service account will govern which integrations the external app is allowed to execute.

Setting up an Client Credential based flow has two parts: configuring within Integration Gateway, and configuring in the external system.

#### Integration Gateway Setup <a href="#admin-setup-glyue" id="admin-setup-glyue"></a>

1. From the admin page, find the _OAuth2.0 section._ Click on _Applications,_ then _Add Application+_
2. Select a service account from the _User_ dropdown.
3. Save the generated _Client ID_ (top of page) and _Client secret_ (toward bottom) in a secure location. After this step, these values will not be visible again.
4. Set _Client Type_ to `Confidential`
5. Set _Authorization Grant Type_ to `Client Credentials`
6. Enter the _Name_ of the external app or 3rd party
7. Save

**External Application Setup**

Specific steps differ between applications, but they will all require the following information:

* The _Client ID_ and _Client Secret_ from above
* Access Token request details
  * Path:  `yourdomain.sandboxbanking.com/o/token/`
  * Method: `POST`
  * Headers:
    * `Authorization: Basic {base64(client_id:client_secret)}`
    * `Content-Type: x-www-form-urlencoded`
  * Body > `grant_type`  : `client_credentials`&#x20;

After the external system calls Integration Gateway with the above details, Integration Gateway will respond with an access token (also known as a "bearer token"). Include that token in the authorization header of subsequent requests to integrations.&#x20;
