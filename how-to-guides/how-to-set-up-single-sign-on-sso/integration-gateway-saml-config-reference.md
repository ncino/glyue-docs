# Integration Gateway SAML Config Reference

While the default SAML config settings will suffice for most use cases, changes may be necessary depending on the IdP and/or customer’s requirements.

{% hint style="warning" %}
If changes are made to an existing SAML Config, Integration Gateway's SAML metadata will change. All connected IdPs will need the updated metadata.
{% endhint %}

### All SAML Config options <a href="#samlconfigreference-allsamlconfigoptions" id="samlconfigreference-allsamlconfigoptions"></a>

#### Attribute map <a href="#samlconfigreference-attributemap" id="samlconfigreference-attributemap"></a>

default: `{"email": "email"}`

A mapping table (in the format of a JSON object) that maps IdP user attributes (keys) to Integration Gateway user attributes (values). If the name of the attribute for the user’s email address in the IdP is something other than `email` (such as `emailAddress`), this JSON object should be extended to include that mapping:

```
{
  "email": "email",
  "emailAddress": "email"
}
```

{% hint style="info" %}
The user email address in Integration Gateway is simply called `email`.
{% endhint %}

{% hint style="info" %}
Some IdPs allow for mapping user attributes on their end as well.
{% endhint %}

{% hint style="warning" %}
Be sure to add the new attribute name to **Required attributes**.
{% endhint %}

#### Http client timeout <a href="#samlconfigreference-httpclienttimeout" id="samlconfigreference-httpclienttimeout"></a>

default: `10`

Seconds to wait for a response from an IdP before giving up.

#### Logout http binding <a href="#samlconfigreference-logouthttpbinding" id="samlconfigreference-logouthttpbinding"></a>

default: `REDIRECT`

How Integration Gateway should send logout requests to the IdP (user wants to logout of Integration Gateway _and their IdP_). This is usually disabled by the IdP by default and is also not commonly used.



#### Cert file <a href="#samlconfigreference-certfileoptionalgreen" id="samlconfigreference-certfileoptionalgreen"></a>

Optional. A custom certificate and public key for Integration Gateway to use for SAML communication.

#### Key file  <a href="#samlconfigreference-keyfileoptionalgreen" id="samlconfigreference-keyfileoptionalgreen"></a>

Optional. The private key corresponding to the above cert.

{% hint style="info" %}
By default, Integration Gateway uses HTTPS and requires that IdPs sign their SAML assertions, and this is generally considered to be plenty secure. A key/cert pair is only needed if _additional security, on top of https and SAML signature verification,_ is desired.
{% endhint %}

#### Required attributes <a href="#samlconfigreference-requiredattributes" id="samlconfigreference-requiredattributes"></a>

default: `email`

A comma-separated list of user attributes Integration Gateway will require from the IdP. If a custom user attribute is added to the **Attribute map** it should also be added here.

#### Want assertions or response signed <a href="#samlconfigreference-wantassertionsorresponsesigned" id="samlconfigreference-wantassertionsorresponsesigned"></a>

default: `True`

Integration Gateway will accept as valid all SAML assertions (login response) from trusted IdPs as long as either the assertion or the outer response containing it, is signed. This is considered secure in SAML standards and should suffice for most cases. If set to True, this overrides the next two settings.

#### Want assertions signed <a href="#samlconfigreference-wantassertionssigned" id="samlconfigreference-wantassertionssigned"></a>

default: `False`

Integration Gateway will require SAML assertions to be signed. Automatically set to `False` if **Want assertions or response signed** is `True`.

#### Want response signed <a href="#samlconfigreference-wantresponsesigned" id="samlconfigreference-wantresponsesigned"></a>

default: `False`

Integration Gateway will require the outer SAML response to be signed. Automatically set to `False` if **Want assertions or response signed** is `True`.

#### Authn requests signed <a href="#samlconfigreference-authnrequestssigned" id="samlconfigreference-authnrequestssigned"></a>

default: `False`

Integration Gateway will sign its login requests to the IdP, using the cert/key pair. Generally not required.

#### Force authn <a href="#samlconfigreference-forceauthn" id="samlconfigreference-forceauthn"></a>

default: `False`

Requires IdP to authenticate the user directly rather than rely on a previous security context.
