# 5. Sending the Email

Now we’re ready for the final step. We’re going to take each of those story responses and collate them into an email body and then send it off to our recipient.

#### Service Request: `send_email` <a href="#step6-theemail-servicerequest-send_email" id="step6-theemail-servicerequest-send_email"></a>

Navigate to the **Build** page, go to the Service Requests for our integration, and add a new row. Enter the following values:

* **Sequence:** `200`
* **System:** `EMAIL_SMTP`
* **Service Name:** `n/a`
* **Formula Variable:** `send_email`
*   **Before Prepare Request Hook:** copy and paste the code below.\
    This is the most complicated Python code we have shown to the reader thus far. Basically, we are creating a standard message for each story we received, and writing it to a file. We will later read this file for the body of the email.

    ```
    from datetime import datetime

    stories = [sr.response.payload for sr in get_story_by_id]
    stories.sort(key=lambda story: story.score, reverse=True)

    query_ct = 500 if not input.payload.limitQuery else input.payload.limitQuery
    email_limit = 50 if not input.payload.limitEmail else input.payload.limitEmail

    with open_glyuefile("email_body.txt", "w") as file:
        file.write(f"Here are the top {email_limit} of the latest {query_ct} articles from Hacker news:\n\n")
        for story in stories[:email_limit]:
            time = datetime.fromtimestamp(story.time).strftime("%m/%d/%Y, %H:%M")
            file.writelines([
                f"{story.title}\n",
                f"{story.url}\n",
                f"Posted by [{story.by}] on {time} ({story.score} points, {story.descendants} comments)\n",
                f"Discussion: https://news.ycombinator.com/item?id={story.id}\n\n"
            ])
    ```

#### Field Mappings for SR: `send_email` <a href="#step6-theemail-fieldmappingsforsr-send_email" id="step6-theemail-fieldmappingsforsr-send_email"></a>

Right-click the newly-created SR and go to Field Mappings. Add 4 rows with these values and then save.

* First row
  * **Field:** `from_address`
  * **Value:** `"email@address.net"` (use the same email as was entered for the Email SMTP Adapter Config, and don’t forget the quotes)
  * **Value Type:** `str`
* Second row
  * **Field:** `to_addresses`
  * **Value:** `[input.payload.email]`
  * **Value Type:** `list`
* Third row
  * **Field:** `subject`
  * **Value:** `"Top HackerNews Stories"`
  * **Value Type:** `str`
* Fourth row
  * **Field:** `body`
  * **Value:** `open_glyuefile("email_body.txt").read()`
  * **Value Type:** `str`

{% hint style="warning" %}
Don’t forget to save!
{% endhint %}
