# 2. Input Validation

You may remember that in the integration description we provided an example input that included a required `email` field and optional `limitQuery` and `limitEmail` fields. If the requestor doesn't provide an email address in their input, how would we know where to send the digest? Or, what if the requestor provides invalid limit values (which should be positive integers)?

In this case we want the integration to fail gracefully and respond to the requestor with a failure status and indicate which field(s) need to be fixed. This is where **Validation Rules (VRs)** come in to play.

#### Adding Validation Rules to the integration <a href="#step2-inputvalidation-addingvalidationrulestotheintegration" id="step2-inputvalidation-addingvalidationrulestotheintegration"></a>

1. On the **Build** page, with **Integration** selected under **Component** at the top, right-click the row with `hackernews_email_digest` and select **Go To > Validation Rule**.
2. Click the **\[➕ADD ROW]** button 2 times to add 2 new rows. Enter the following values for each row:
   1. First row:
      * **Type**: `INPUT`
      * **Field**: `input.payload.email`
      *   **Rule**: (copy and paste the below Python code)

          ```
          import re

          pattern = r"^\w[\w&'\+\-\.]*\b@[a-zA-Z\d][a-zA-Z\d\-]*\b\.[a-zA-Z\d]+[a-zA-Z\d\-]*[a-zA-Z\d]$"
          retvalue = _ and isinstance(_, str) and re.match(pattern, _)
          ```
   2. Second row:
      * **Type**: `INPUT`
      * **Field**: `input.payload.limitQuery`
      *   **Rule**: (copy and paste the below Python code)

          ```
          not _ or (isinstance(_, int) and _ > 0)
          ```
   3. Select the second row and click the **\[DUPLICATE ROW]** button at the bottom and make the following edits to the new row:
      * **Field**: `input.payload.limitEmail`
   4. Click the **\[**&#xD83D;� **Save]** button at the bottom.

{% hint style="info" %}
If you are familiar with Python, you may have an idea of what is going on here.\
(`_` represents the value of the field named in the **Field** column)

* In the first validation rule, we ensure that the provided email exists, is a string, and a properly formatted email using a regex expression.
* In the second and third, we ensure that, the limits are either not provided or are positive integers
{% endhint %}



#### Testing the Validation Rules <a href="#step2-inputvalidation-testingthevalidationrules" id="step2-inputvalidation-testingthevalidationrules"></a>

Now we’re going to validate that our new Validation Rules work correctly.

1. Navigate to the **Run History** page. Select our most recent run, and click the **curved arrow symbol** on it. The **Run Integration** dialog window should appear.
   1.  Copy and paste this into the **PAYLOAD**:

       ```
       {
         "email": "user@email.net",
         "limitQuery": 10
       }
       ```
   2. Click **RUN INTEGRATION** at the bottom. It should run successfully.
2. On the **Run History** page, click the most recent integration and then the curved arrow again.
   1.  This time, provide some invalid values in our payload:

       ```
       {
         "email": "invalid%email###@@123",
         "limitEmail": -1
       }
       ```
   2. Re-run the integration again. It should fail.
   3. Click the most recent run (once it appears- it may take a couple seconds). There should be two **VALIDATION FAILURE** records in the middle **STEPS** column. Clicking on them will provide information.

{% hint style="info" %}
It's strongly encouraged to play around a little before continuing!&#x20;

* Try triggering the integration with various valid or invalid inputs.
* Practice viewing them on the **Run History** page.
{% endhint %}
