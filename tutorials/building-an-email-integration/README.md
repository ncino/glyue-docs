# Building an Email Integration

This tutorial aims to guide a first-time Integration Gateway user through the process of building a real integration, step by step, from start to finish. This integration will take an email address as input, then query the [Hacker News API](https://github.com/HackerNews/API) for articles, and finally email a list of article URLs to the provided address.

Upon completion of the tutorial, the reader will understand:

* integrations, as well as how to build one,
* how to use the Admin site to configure Integration Gateway components,
* how to validate the data that passes through Integration Gateway,
* how to visualize that data at every step,
* how to trigger integrations,

and more generally, the _awesome power of_ Integration Gateway!

{% hint style="warning" %}
The reader will need the following before starting:

* A Integration Gateway user account with admin privileges
* A Gmail/Google account
* A basic understanding of JSON and HTTP (helpful but not 100% required)
* Basic skills in the Python programming language (helpful but not 100% required)
{% endhint %}



