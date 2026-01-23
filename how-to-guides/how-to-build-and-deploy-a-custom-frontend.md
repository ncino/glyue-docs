# How to Build and Deploy a Custom Frontend

## What is a Frontend?

Frontends are lightweight applications hosted on Integration Gateway. They can be used to support a wide range of use cases, including creating dashboards, giving easy access to run integrations, or simply as a fast way to host an internal application.

## Accessing a Frontend

Frontends can be given custom subaddresses on your Integration Gateway domain (e.g. `glyueprod.examplebank.sandboxbanking.com/frontends/example_dashboard`) making them easy to bookmark, link, and distribute.

All Frontends have built-in Integration Gateway authentication; users must first log in with their Integration Gateway account before they can load a Frontend.&#x20;

## Creating a Frontend

Frontends can be created by uploading a `.zip` bundle that contains the `.html`, `.js`, and `.css` files for your web application, along with any other assets that may be required.&#x20;

Frontends require a file named `index.html` to be present, which will be used as the entrypoint into the application. Relative references to pages within the frontend (e.g. `/resources/logo.png`) will work as expected.

<figure><img src="../.gitbook/assets/Screenshot 2024-07-15 at 4.51.27 PM.png" alt=""><figcaption><p>Create a Frontend by giving it a name, path, and uploading the .zip</p></figcaption></figure>

For details on more advanced Frontend features and sample code snippest, see the [reference documentation](../integration-gateway-platform-reference/frontends.md).

## Editing a Frontend

Once created, a Frontend can be modified by replacing the contents of the uploaded `.zip` with an updated version. Upon save, the new version will be deployed.

The source code of the currently-served Frontend can be downloaded from within the _Edit_ page of the Frontend.
