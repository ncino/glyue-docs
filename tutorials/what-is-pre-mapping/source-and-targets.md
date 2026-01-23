# Source and Targets

Now we are looking at a **Field Mapping** in the **Build** page. The **Build** page is designed to resemble and function similarly to a spreadsheet. Much like a spreadsheet, there are several headers at the top. The columns we will focus on are under the headers `Source Record Type`, `Source Field Name`, `Target Record Type` and `Target Field Name`

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

These columns are sort of like labels for the integration template that's been installed.&#x20;

{% hint style="info" %}
The Source and Target columns are merely descriptors of the particular field mapping and do not set the mapping.
{% endhint %}

The `Source` columns relate to the system your data is coming from such as a Loan Origination System (LOS). The `Target` columns refer to the destination system your data is going to such as your banking Core or a data warehouse.

* `Source Record Type`: This is the specific record type for this **Field Mapping** such as a Loan booking, a Credit Card application, or an Account.
  * &#x20;NOTE: Sometimes the API name for the field is used which may be slightly different than how the field appears in the source system.
* `Source Field Name`: This is the name of the specific field such as "Account Number", "First Name" or "Street Address"
* `Target Record Type`: This is your destination system's record type which will be something like Account, Collateral, or CIF Record
* `Target Field Name`: This is the field name in your destination system which will be something like "Account Number", "First Name" or "TID"

Your primary goal in **Pre-Mapping** will simply be to evaluate these **Source** and **Target** columns and determine whether or not the referenced pairs match your business process, if they need to be changed, if they need to be added, or if you have questions about a particular mapping.

***

Through the **Pre-Mapping** and **Mapping** phases, we are going to be spending a lot of time on this page. Let's start to get acquainted with this page with a few simple exercises:

1.  The columns in the build page are somewhat narrow by default and may be too narrow to see the full entries. Hover your mouse cursor over the boundary between columns, click and drag to the right to expand the width of the column to your liking

    <figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>
2. You can order each column's values by numeric or alphabetical order or reverse numeric or alphabetical order. Try clicking on the header of your choosing and order the values back and forth.
3.  At the moment, we only care about the 4 columns mentioned plus the `Comments` and `Status` columns (we'll cover these in the next sections). There are a lot of other columns and we won't need to pay attention to most of them until later in the project. We can decide which columns to show and hide by using the **Show Columns** drop-down:<br>

    <figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

    NOTE: The **Field Mapping** page(s) will be filtered by default when accessing them through the **Bookmarks**.
4. Let's click on the **Show Columns** drop-down and select `Comments`, `Source Record Type`, `Source Field Name`, `Target Record Type` and `Target Field Name`, `Status` so that it looks like this:

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

5.  Now that we're looking at the columns we need to see, go ahead and expand each of the rows as we had done in Step 1:

    <figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>
6. Again, the goal of **Pre-Mapping** is to identify and evaluate the mappings for fields coming from your **Source** system going into your **Target** or destination system.
