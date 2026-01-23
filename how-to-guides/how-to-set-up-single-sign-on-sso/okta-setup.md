# Okta Setup

{% hint style="warning" %}
Before continuing, this user should be an Okta admin for the organization, and a SAML Config must be created in the Integration Gateway Environment.
{% endhint %}

### Create the Integration Gateway application in Okta <a href="#oktasso-basicsetuphowto-createtheglyueapplicationinokta" id="oktasso-basicsetuphowto-createtheglyueapplicationinokta"></a>

From the Okta dashboard, find **Applications** on the left-hand panel and select **Applications** under it.

<figure><img src="../../.gitbook/assets/image (58).png" alt=""><figcaption></figcaption></figure>

Click the **Create App Integration** button.

<div data-full-width="false"><figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure></div>

Select **SAML 2.0** and click **Next**

<figure><img src="../../.gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

Give the app a name. We suggest something like Integration Gateway **DEV**, Integration Gateway **PROD** etc. Click **Next**.

<figure><img src="../../.gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

### Provide Integration Gateway's SAML info to Okta <a href="#oktasso-basicsetuphowto-provideglyuessamlinfotookta" id="oktasso-basicsetuphowto-provideglyuessamlinfotookta"></a>

{% hint style="info" %}
The Integration Gateway environment SAML metadata will be required for the next step.

Integration Gateway always serves its metadata at `https://` (custom domain) `/sso/saml2/metadata/`. If this user is also a Integration Gateway administrator, the metadata URL will be displayed on the **Admin** site under **SAML Configs**.
{% endhint %}

#### **Single sign-on URL** <a href="#oktasso-basicsetuphowto-singlesign-onurl" id="oktasso-basicsetuphowto-singlesign-onurl"></a>

In the XML document, locate an element named `AssertionConsumerService` and grab the URL from its `Location` attribute (do not include the `"`).

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

In most cases this will be `https://`\[domain]`/sso/saml2/acs/`

#### **Audience URI (SP Entity ID)** <a href="#oktasso-basicsetuphowto-audienceuri-spentityid" id="oktasso-basicsetuphowto-audienceuri-spentityid"></a>

In the XML document, locate the first element `EntityDescriptor` and grab the value for its `entityID` attribute. In most cases this will be the same URL of Integration Gateway's metadata (do not include the `"`).

<figure><img src="../../.gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

<div data-full-width="false"><figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure></div>

#### **Attribute Statements** <a href="#oktasso-basicsetuphowto-attributestatements" id="oktasso-basicsetuphowto-attributestatements"></a>

Although the Okta wizard says it is optional, it is actually necessary. Add an attribute called `email`, with `URI Reference` for its **Name format**. Select `user.email` as the **Value**.

<figure><img src="../../.gitbook/assets/image (61).png" alt=""><figcaption></figcaption></figure>

Click **Next,** provide feedback to Okta if so inclined, and click **Finish**.

### Get the Okta metadata for Integration Gateway <a href="#oktasso-basicsetuphowto-gettheoktametadataforglyue" id="oktasso-basicsetuphowto-gettheoktametadataforglyue"></a>

From the **Applications** screen, click the newly created application and go to the **Sign On** tab.

Under **Settings** > **Sign on methods** > **SAML 2.0** > **Metadata details**, grab the **Metadata URL**.

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

This will be needed on the Integration Gateway side, when adding this Okta environment as an IdP. If this user is not an Integration Gateway admin, please provide the URL to a Sandbox Banking employee.

{% hint style="warning" %}
### Lastly… <a href="#oktasso-basicsetuphowto-lastly..." id="oktasso-basicsetuphowto-lastly..."></a>

Don’t forget to assign users or groups to the new Okta application- otherwise they won’t be able to authenticate with Integration Gateway via Okta SSO.
{% endhint %}
