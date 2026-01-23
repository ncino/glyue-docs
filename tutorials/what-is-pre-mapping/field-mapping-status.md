# Field Mapping Status

Since the goal of **Pre-Mapping** is to review each of the **Field Mappings**, the **Status** column is where we are going to mark each **Field Mapping** to signify that it's been reviewed. The objective is simply to take a first pass at the **Field Mappings** which doesn't mean they have to be fully approved or worked out. If you don't understand a particular **Field Mapping**, there's a **Status** option to mark it as such and you can revisit it in the **Mapping** phase with your Solution Engineer.

**Here's a list of the different Status Options:**

* **Not Started** - This is the default **Status** which means the **Field Mapping** has NOT been reviewed&#x20;
* **Not Used** - This means the **Field** for the **Target System** isn't used at all and this **Field Mapping** isn't necessary
* **Change Requested** - This means your team is requesting that this **Field Mapping** be mapped differently, either to a different **Target Field** or from a different **Source Field** (or even if additional logic is needed)
* **In Progress** - This means your organization uses this **Field Mapping** but needs to decide how it should be mapped to fit your business purposes
* **Waiting for Support** - This means you have questions or you're unsure of how to move forward with this **Field Mapping** and you need assistance from our team
* **Waiting for Customer** - Our team will use this when we answer one of your questions or have further questions for your team
* **Needs Testing** - This means you believe this **Field Mapping** to be correct but that it needs testing before being ready for production.
* **Approved** - This means that mapping is complete and the **Field Mapping** has been fully tested and is ready for production. <mark style="background-color:blue;">NOTE: This status will not be used until the User Acceptance Testing (UAT) phase.</mark>

![](<../../.gitbook/assets/image (19).png>)

{% hint style="info" %}
**Pre-Mapping** is complete when EVERY **Field Mapping Status** has been transitioned from **Not Started** to **Needs Testing, Not Used, Waiting for Support, or Change Requested**
{% endhint %}

**Some Clarifications:**

* Changing a **Field Mapping's Status** doesn't mean you have to take immediate action on it, you're just marking it for now so that all the **Field Mappings** that need to be actioned will be collated for a more efficient **Mapping** phase
* The difference between **Change Requested** and **Waiting for Support** is that **Change Requested** means you know how the field should be mapped and are waiting on our team to implement and **Waiting for Support** means you have questions about the **Field Mapping** for our team that you need answered to understand and decide how it should be mapped
* The difference between **In Progress** and **Waiting for Support** is that **In Progress** means it's your organization's action to decide how to move forward and **Waiting for Support** puts the **Field Mapping** in our team's court to answer any questions

For the following exercises, you can pick up where you left off from [**Bookmarks**](bookmarks.md) or follow the steps in [**Source and Targets**](source-and-targets.md) to open up the **Field Mappings** for any **Bookmark**.

1. Find a very basic **Field Mapping** with "First Name" in the **Source Field Name** and "First Name" in the **Target Field Name**. If you need help from the Solutions Engineer assigned to your project, they'll be happy to help. If your integration isn't for personal entities, feel free to select the appropriate name field for an organization entity.

In most cases, fields like First Name, Last Name, or Organization Name should be straightforward. Verify that this field from the **Source System** should arrive in the **Target System** corresponding to the **Field Mapping**.

1. If you agree with the Field Mapping, change the Status to **Needs Testing**
2. Now, look down the list of **Target Field Names** and try to find a field in your **Target System** that you don't use at all.
3. Update that Status to **Not Used**

We are only going to focus on **Needs Testing** or **Not Used** for this portion of the tutorial. For the other Statuses, we're going to use Field Mapping level Comments to communicate questions or changes as part of the **Change Requested**, **In Progress, Waiting for Customer,** and **Waiting for Support** Statuses.
