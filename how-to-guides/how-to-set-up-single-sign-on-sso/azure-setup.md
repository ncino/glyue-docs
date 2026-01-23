# Azure Setup

{% hint style="warning" %}
Before continuing, this user should be an Azure AD admin for the organization, and a SAML Config must be created in the Integration Gateway Environment.
{% endhint %}

### Create the Integration Gateway Enterprise Application <a href="#azuresso-basicsetuphowto-createtheglyueenterpriseapplication" id="azuresso-basicsetuphowto-createtheglyueenterpriseapplication"></a>

From the Azure Active Directory overview page, select **Enterprise applications** from the left-hand panel.

<figure><img src="../../.gitbook/assets/794656786.png" alt=""><figcaption></figcaption></figure>

Click **New Application** at the top.

<figure><img src="../../.gitbook/assets/795213846.png" alt=""><figcaption></figcaption></figure>

Click **Create your own application** at the top.

<figure><img src="../../.gitbook/assets/795312129.png" alt=""><figcaption></figcaption></figure>

Provide a name and select **Integrate any other application you don’t find in the gallery (Non-gallery)**. Click **Create** at the bottom of the page.

<figure><img src="../../.gitbook/assets/795148309.png" alt=""><figcaption></figcaption></figure>

Click **2. Set up single sign on** under **Getting Started**, or select **Single sign-on** from the left-hand panel.

<figure><img src="../../.gitbook/assets/794591255.png" alt=""><figcaption></figcaption></figure>

Select **SAML**.

<figure><img src="../../.gitbook/assets/794722362.png" alt=""><figcaption></figcaption></figure>

### Provide Integration Gateway's metadata to Azure <a href="#azuresso-basicsetuphowto-provideglyuesmetadatatoazure" id="azuresso-basicsetuphowto-provideglyuesmetadatatoazure"></a>

{% hint style="info" %}
The Integration Gateway environment SAML metadata will be required for the next step.

Integration Gateway always serves its metadata at `https://` (custom domain) `/sso/saml2/metadata/`. If this user is also a Integration Gateway administrator, the metadata URL will be displayed on the **Admin** site under **SAML Configs**.
{% endhint %}

At the top, select **Upload metadata file.** Select the appropriate file and upload.

<figure><img src="../../.gitbook/assets/795148332.png" alt=""><figcaption></figcaption></figure>

Click **Save** at the top.

<figure><img src="../../.gitbook/assets/794886183.png" alt=""><figcaption></figcaption></figure>

### Add an email claim <a href="#azuresso-basicsetuphowto-addanemailclaim" id="azuresso-basicsetuphowto-addanemailclaim"></a>

We need to add a claim to the SAML configuration in Azure, so that the user’s email address comes over to Integration Gateway in a field titled “email” instead of something else.

On the application **SAML-based Sign-on** page, in section **2 Attributes & Claims** click **Edit**.

<figure><img src="../../.gitbook/assets/794624036.png" alt=""><figcaption></figcaption></figure>

Click **Add new claim** at the top.

<figure><img src="../../.gitbook/assets/794525715.png" alt=""><figcaption></figcaption></figure>

Enter `email` in the **Name** field. For **Source Attribute** select `user.mail`. Click **Save** in the top-left.

<figure><img src="../../.gitbook/assets/795148338.png" alt=""><figcaption></figcaption></figure>

Depending on the organization’s Azure configuration, the user email field name may differ.

### Get Azure SAML metadata for Integration Gateway <a href="#azuresso-basicsetuphowto-getazuresamlmetadataforglyue" id="azuresso-basicsetuphowto-getazuresamlmetadataforglyue"></a>

On the application **SAML-based Sign-on** page, in section **3** under **SAML Certificates** download the **Federation Metadata XML**.

<figure><img src="../../.gitbook/assets/spaces_1flQ2To8tQpCQWl2Ty9U_uploads_FoW4QsSTMlRCR5eIsRRT_794492954.png" alt=""><figcaption></figcaption></figure>

Alternatively, grab the **App Federation Metadata Url** instead of downloading the file.

This will be needed on the Integration Gateway side, to add Azure as an IdP. If this user is not a Integration Gateway admin, please provide the file/URL to a Sandbox Banking employee.

{% hint style="warning" %}
### Lastly… <a href="#azuresso-basicsetuphowto-lastly..." id="azuresso-basicsetuphowto-lastly..."></a>

Don’t forget to assign users or groups to the new application- otherwise they won’t be able to authenticate with Integration Gateway via SSO.
{% endhint %}
