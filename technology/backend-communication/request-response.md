# Request Response

Simple communication model out there

Server parses the request that is recieved from the client. This is an expensive process. That is one of the main reasons people shifted from SOAP (XML) based API to REST (JSON) based APIs.&#x20;

Even JSON parsing is slow, there are better alternatives like protcol buffers

* The cost of using JSON is, it is readable but the size is large.
* The cost of using protocol buffers is, it is small in size but not readable

Request response model is used almost every where:

* Web
* DNS
* SSH
* HTTP
* RPC
* SQL and Database protocols
* APIs (REST, SOAP, GraphQL)

{% hint style="info" %}
When it comes to backend, never trust the order. Rely on unique IDs
{% endhint %}
