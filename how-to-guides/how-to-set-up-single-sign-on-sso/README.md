# How to Set Up Single Sign On (SSO)

​[Single Sign-On](https://en.wikipedia.org/wiki/Single_sign-on) (SSO) is a demanded feature for many enterprise web applications, as it allows for centralized user access control by the business or organization. Users also do not need to remember passwords, as they only need their SSO login details to access any of their connected applications.

#### Brief Overview of SAML and Integration Gateway SSO <a href="#glyuesso-explanation-briefoverviewofsamlandglyuesso" id="glyuesso-explanation-briefoverviewofsamlandglyuesso"></a>

In a SAML SSO configuration, Integration Gateway acts as the **Service Provider (SP)** and a trusted identity/access platform (such as JumpCloud, Azure AD, Okta, etc.) acts as the **Identity Provider (IdP)**. Integration Gateway SSO is designed to be IdP-agnostic and work with any provider.

{% hint style="info" %}
While expected to work with any IdP, as of the writing of this article, Integration Gateway SSO is verified to be compatible with:

* JumpCloud
* Okta
* Azure AD

More providers will be added as their compatibility is verified.
{% endhint %}

The usual user authentication flow goes like this:

1. User clicks SSO option on the SP login screen. SP redirects the user to the IdP along with a SAML request.
2. User authenticates at the IdP.  IdP redirects user back to the SP along with a signed SAML response, which contains information on the user it just authenticated.
3. SP verifies the signature, reads the response, identifies the correct user in its system, and logs them in.

Before any of that could happen, though, the service and ID providers had to establish a trusted relationship.  The usual flow for that goes like this:

1. An Integration Gateway administrator configures and activates SAML SSO and then provides a metadata file to the other organization.
2. An IdP administrator at the organization adds Integration Gateway as an SP, uploads the metadata, and configures as necessary. They provide another metadata file back to the Glyue admin.
3. The Integration Gateway admin creates the IdP record and uploads its metadata.

Once done, the IdP should appear on the Integration Gateway login screen as an option for users.

## Frequently Asked Questions / FAQ

#### Does Integration Gateway support OIDC (OpenID Connect)?

> No. For a deeper explanation as to the differences between SAML and OIDC, see [this excellent article](https://jumpcloud.com/blog/saml-vs-openid) from [JumpCloud](https://jumpcloud.com/), a respected cloud-based authentication provider and IT asset management platform.

#### Does a user need an account in Integration Gateway before being able to log in via SSO?

> No, by default users signing in via SSO will have an account created at that time automatically.  This feature can be toggled off if asked, which will enforce login only by existing users in Integration Gateway.
>
> On the Identity Provider record on the Integration Gateway Admin site, it can be set to automatically add users to an organization and/or mark as staff.  If the Default Staff Group or Default Non-Staff Group are set on the Global Config, new users will be added to those groups as appropriate depending on staff status.
>
> This way, user onboarding can be completely automated, so new users are added to your Organization (if using Organizations) as well as added to a Group with permissions to certain integrations.

#### Can I restrict users to logging in with SSO only?

> Yes. New users can be restricted to SSO by unchecking "Allow Password Login" on the invite page. If the user already exists, go to Admin > Accounts, select the account, and uncheck "Allow password auth" and save.

