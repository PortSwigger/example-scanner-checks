# Sample Burp Suite extension: custom scanner checks

In the [custom scan insertion points
example](//github.com/PortSwigger/example-custom-scan-insertion-points), we saw
how an extension could be used to provide custom insertion points for use by
Burp Scanner, enabling you to run the Scanner's built-in checks against entry
points within serialized data or other formats that Burp does not natively
support. In this example, we'll see how an extension can be used to futher
extend the Scanner's behavior, by providing custom checks for passive and active
scanning.

Custom scan checks are tightly integrated within Burp's scanning engine, and are
invoked at the relevant stage for each base request and insertion point that the
user sends for scanning. They can perform arbitrary processing, issue their own
requests (when actively scanning), and report their own custom scan issues.

For the sake of this example, we've updated the demo serialized input
application to contain two fictitious vulnerabilities that our extension can
check for:
- An information leakage vulnerability where a content management system is
  copying sensitive data into some application responses.
- An input vulnerability where submitting the pipe character results in a
  distinctive error message, indicating an exploitable condition.

The sample extension demonstrates the following techniques:
- Registering a custom scanner check.
- Performing passive and active scanning when initiated by the user.
- Using the Burp-provided IScannerInsertionPoint to construct requests for
  active scanning using specified payloads, without needing to understand how
  the insertion point works.
- Using a Burp helper method to search responses for relevant match strings.
- Highlighting relevant portions of requests and responses, in line with Burp's
  natively-generated scan issues.
- Synchronously reporting custom scan issues in response to the relevant checks.
- Guiding Burp on when to consolidate duplicated issues at the same URL (e.g.,
  when the user has scanned the same item multiple times).

If you want to run this extension, you'll need to use the included server (for
ASP.NET and NodeJS), and also install the [custom scan insertion points
example](//github.com/PortSwigger/example-custom-scan-insertion-points), so that
the active scan payload is inserted correctly into the serialized request.

This repository includes source code for Java, Python and Ruby. It also includes
a server (for ASP.NET and NodeJS) that extends the [serialization
example](//github.com/PortSwigger/example-custom-editor-tab) to add some
fictitious bugs so that you can test the custom scanner check and see that the
issues are reported. Note: the sample server uses the JavaScript btoa() function
to perform Base64-encoding on the client side. This function is not supported by
Internet Explorer, but works on most other browsers.
