# Frontends

## Using React with Frontends

When using React to build your Frontend, ensure that the "public path" or "base directory" of your React app is set to `frontends/example-frontend` to ensure all linked resources are loaded correctly.

#### Using Vite

If you're using [Vite](https://vitejs.dev/guide/static-deploy.html) to build your React app, you can edit the `vite.config.js` file (or `vite.config.ts` if you're using TypeScript) to set the public path, like so:

```js
import { defineConfig } from "vite"
import react from "@vitejs/plugin-react"

export default defineConfig({
  plugins: [react()],
  base: "/frontends/example_dashboard" <--
})
```

#### Using Create-React-App

If you bootstrapped you're React app using [Create-React-App](https://create-react-app.dev/docs/deployment), you can specify the `homepage` key in your project's `package.json` file to set the public path, for example:

```json
"homepage": "https://glyueprod.examplebank.sandboxbanking.com/frontends/example_dashboard"
```

#### Using Webpack

If you're using [Webpack](https://webpack.js.org/guides/public-path/) to bundle your React app, you'll just need to modify `output.publicPath` in your Webpack configuration file (most likely `webpack.config.js`), like so:

```js
export default {
  output: {
    publicPath: "https://glyueprod.examplebank.sandboxbanking.com/frontends/example_dashboard"
  }
}
```



***

## Query Parameters

Frontends accept query parameters. As an example,

```
glyueprod.mybank.sandboxbanking.com/frontends/example-dashboard?customer_id=1234&branch=west
```

To access these query parameters in your frontend's Javascript, use the `window` object's `location` property. For example:

```javascript
const params = new URLSearchParams(window.location.search);
const customerId = params.get('customer_id'); // 1234
const branch = params.get('branch'); // west
```

***

## Code Snippets

Below are provided code snippets for common actions within a Frontend.

### Getting the CSRF Token

For Glyue to accept any `POST` requests made to its API endpoints, you must provide the `X-CSRFToken` header using the value of the `csrftoken` cookie with your request.

**Using `js-cookie`**

If you choose to use an external library with your Frontend, [js-cookie](https://github.com/js-cookie/js-cookie) provides a simple way to get the value of cookies:

```js
import Cookies from "js-cookie"

const CSRFToken = Cookies.get("csrftoken")
```

**Using `document.cookie`**

If you're unable to use an external library, below is sample code for getting the value of cookies using the built-in `document.cookie`:

```js
function getCookie(name) {
  const cookies = document.cookie.split("; ")

  return cookies
    .map((cookie) => cookie.split("="))
    .find(([key, _]) => key === name)
    ?.[1] ?? null
}
```

### Logging Out

While authentication for Frontends is built-in and enforced automatically, explicit logout is not. The following function logs a user out of a Frontend.

```js
async function logout() {
  await fetch(
    "/integrations/services/auth/logout",
    {
      method: "POST",
      headers: {
        "X-CSRFToken": getCookie("csrftoken"),
      }
    }
  )
}
```

### Executing an Integration

To execute an integration, make a `POST` request to `/integrations/execute/{pathName}` with its JSON payload and the `X-CSRFToken` header set. You may also use the integration's [webservice endpoints](../reference/web-service-endpoints.md) if set up.

The integration will run as the logged-in user. The logged-in must have `execute` [permissions](permissions/#integration-permissions) on the integration.

```js
async function executeIntegration(pathName, payload) {
  const response = await fetch(
    `/integrations/execute/${pathName}`,
    {
      body: JSON.stringify(payload),
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "X-CSRFToken": getCookie("csrftoken"),
      }
    }
  )

  if (response.ok) {
    return await response.json()
  } else {
    return null
  }
}
```
