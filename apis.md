# ![](https://raw.githubusercontent.com/SWAN-community/swan/main/images/swan.128.pxls.100.dpi.png)

# Secured Web Addressability Network (SWAN) – APIs

Draft: Work in Progress

## Who should read this document?

-   Developers seeking to create User Interfaces for SWAN data (CMPs), or
    retrieve the information for use in advertising.

-   Developers porting SWAN to server-side programming languages other than Go.

-   Developers creating SWAN compatible APIs in other projects.

## Introduction

This document describes the SWAN Operator end points used by User Interface
Providers (UIPs) and consumers of the SWAN data.

The document assumes the reader is familiar with the SWAN concepts.

The term User Interface Provider (UIP) is used to signify a caller with the
purpose of retrieving raw SWAN data to provide a User Interface. In practice
this will typically be a CMP but could be any party with the requisite access to
the SWAN Network.

## Actions

The following actions are described in this document.

-   [Fetch](#fetch) - retrieving data from SWAN

-   [Update](#update) - updating SWAN data

-   [Decrypt](#decrypt) - transforming encrypted data for use in advertising

-   [Decrypt-Raw](#decrypt-raw) - transforming encrypted data use in user
    interfaces

-   [Home-Node](#home-node) - the home node for the web browser

-   [Stop](#stop) - stopping adverts based on domain name

-   [CreateSWID](#createswid) - generating new SWIDs

## Server Side

The APIs are designed to comply with REpresentational State Transfer (REST)
principles and are accessible via the HTTPS protocol. They are designed to be
called from a server environment because they include a mandatory access key for
access control which must never be distributed to web browsers.

To make consumption of the APIs easier for server side programmers separate
wrappers are available in some languages. They are contained in companion
repositories named swan-x where x is the popular name of the associated
programming language. For example; the Go Lang server side wrapper would be
named swan-go.

As SWAN is more widely deployed it is expected engineers will port the Go
reference wrapper to other languages.

## Access Control

All the SWAN endpoints require the mandatory parameter `accessKey` unless
specified otherwise. This is a unique string provided by the SWAN Operator to
the caller. The `accessKey` must never be shared with any other party. For this
reason, these APIs should only ever be used from server-side web requests.

SWAN’s access control mechanism should block requests that contain HTTP headers
commonly associated with web browsers but that would not present from a
server-to-server request in order to prevent accidental leakage of the
accessKey.

In any case the sharing of a SWAN accessKey outside the entity that is entitled
to use it would be considered a violation of the SWAN agreement and likely
result in the entity being banned from participation in SWAN.

## Open Web IDs (OWIDs)

Almost all the data held in SWAN is encoded as Open Web IDs (OWIDs). This
ensures the source of the data is verifiable to recipients of the data and that
the data has a verifiable timestamp associated with its capture which can be
used to resolve conflicts when duplicate values are encountered. OWIDs also form
the core of the SWAN audit process. To find out more about OWIDs see the [OWID
project](https://github.com/SWAN-community/owid).

## SWAN Data

Seven fields form the SWAN data model.

| **Name** | **Format**    | **Description**                                                                                                                                                                                                                                   |
|----------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Email    | String        | The original email address the user entered. Only available for the decrypt-raw end point for the purposes of providing a User Interface by the User Interface Provider. Can not be made available any other party or used for any other purpose. |
| Pref     | OWID          | The preferences chosen by the user. If missing or empty the rest of the data can not be used until a specific value has been set by the user.                                                                                                     |
| SID      | OWID          | The Signed in Identifier generated from the Email using a hash algorithm by the SWAN Operator.                                                                                                                                                    |
| Stop     | String        | A carriage and line feed separated list of advert identifiers that should not be used in response for advertising.                                                                                                                                |
| SWID     | OWID          | The globally unique identifier for the web browser generated by the SWAN Operator.                                                                                                                                                                |
| Val      | String (Date) | A date RFC3339 form date and time string returned from some operations to indicate when the date returned should be revalidated with SWAN.                                                                                                        |
| tcString | OWID          | The [Transparency and Control Framework](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework) string as defined by IAB Europe and IAB Tech Lab encoded as an OWID.                                            |

## General

### End points

All end points are prefixed with the `/swan/api/v1/` path prefix. The path
segment that follows describes the action that the end point will perform. For
example; to create a URL that will direct the web browser to fetch SWAN data the
[fetch](#fetch) action is used resulting in the path `/swan/api/v1/fetch`.

All end points support both HTTPS GET and POST methods for the provision of
parameters. Example URLs in this document are shown with the GET method.

### Defaults

The SWAN Operator is free to define default values for optional parameters. Each
SWAN Operator will be responsible for communicating these defaults and they are
not covered in this document.

### Timeouts

Some operations result in data that must be used with in a limited period of
time. An action like fetch or update that result in a URL require the browser to
be navigated to that URL within a few seconds of it being returned.

Actions that take encrypted data like [decrypt](#decrypt) or
[decrypt-raw](#decrypt-raw) will require the encrypted data to have been
generated just before the request is performed.

These limits prevent data from being played back sometime after the event. For
example; log files that contain full URL paths will no longer contain useful
data because the URL will no longer result in useful information.

HTTP 400 error codes indicating bad request are returned when data has timed out
and can no longer be used. The body of the response will contain any additional
information.

### Web browser detection

If fields that are commonly sent from web browsers but can be removed by
server-side clients, or are commonly not sent from server side clients, are
present in the request then the APIs will respond with an error indicating that
the request is likely to be coming from a web browser and as such is rejected.
This technique reduces the probability of a developer exposing a valid access
key publicly.

As such it is not possible to experiment with SWAN APIs from a web browser
environment.

## Actions

The following actions are available with SWAN Operators and the access nodes
they provide.

### Fetch

SWAN’s data is only stored with the web browser. The fetch action is used to
generate a URL that the caller should navigate the web browser to. The web
browser will return to a URL provided by the caller with the encrypted SWAN data
appended. The URL will typically result in one, but on first use and at other
times more, redirects before the browser returns to the URL provided by the
caller.

#### Input

| **Parameter**         | **Validation** | **Description**                                                                                                                                                                                                                                                                                                                                                   | **Example**                                       |
|-----------------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| accessKey             | Mandatory      | The secret access key known only to the caller and the SWAN Operator.                                                                                                                                                                                                                                                                                             | `123`                                             |
| accessNode            | Optional       | The access node of the SWAN Operator that will be used to perform the decrypt action when control returns to the caller. If this is not provided, then the same SWAN Operator access node as the one being called will be used. This parameter is provided so that UIPs can pass an encrypted response to a publisher where they use different SWAN Operators.    | ap2.swan-operator.com                             |
| backgroundColor       | Optional       | The HTML color for the background of the progress page.                                                                                                                                                                                                                                                                                                           | \#111111                                          |
| displayUserInterface  | Optional       | Boolean indicator for the display of the progress user interface. Defaults to true.                                                                                                                                                                                                                                                                               | true                                              |
| javascript            | Optional       | If set to true then the response to the URL will be JavaScript code that will in turn add more JavaScript code to the page until all the SWAN nodes have been visited and the SWAN data is available for decryption. Used when third party cookies are available. Defaults to false.                                                                              | false                                             |
| message               | Optional       | The message to display above the progress indicator if enabled during the operation. Along with the title this attribute enables callers to communicate with the user and set expectations.                                                                                                                                                                       | Hang tight. We’re just fetching your preferences. |
| messageColor          | Optional       | The HTML color to use for the message text.                                                                                                                                                                                                                                                                                                                       | Blue                                              |
| nodeCount             | Optional       | An integer number greater than 1 to indicate the number of nodes that should be used for the operation.                                                                                                                                                                                                                                                           | 15                                                |
| postMessageOnComplete | Optional       | Boolean indicator to determine if a window.postMessages should be used to return the encrypted response from the storage operation. If set to false then the web browsers navigation returns to the URL. Defaults to false.                                                                                                                                       | false                                             |
| pref                  | Optional       | The current preferences OWID being used by the caller. Used if the data has been removed from the SWAN network, perhaps by tracking prevention techniques.                                                                                                                                                                                                        | `Nx09ZGRCrt8kByjl_HKA`                            |
| progressColor         | Optional       | The HTML color for the progress indicator.                                                                                                                                                                                                                                                                                                                        | Red                                               |
| remoteAddr            | Optional       | The IP address of the web browser as observed by the requesting server. Used to calculate the home node and reduce the number of redirects.                                                                                                                                                                                                                       | 23.245.23.65                                      |
| returnUrl             | Mandatory      | The complete URL that the web browser should return to when the operation completes. The encrypted data will be appended to this URL. If the postMessageOnComplete parameter is true then the value will be used to post the message to the correct window.                                                                                                       | `https://publisher.com/article`                   |
| state                 | Optional       | Optional array of values expressed via duplicate keys to pass information between the caller and the return url.                                                                                                                                                                                                                                                  | “Custom messages”                                 |
| stop                  | Optional       | The current stopped advert identifiers being used by the caller. Used if the data has been removed from the SWAN network, perhaps by tracking prevention techniques.                                                                                                                                                                                              | `cool-bikes.uk` `cool-cars.uk`                    |
| swid                  | Optional       | The current SWID OWID known to the caller. Used if the data has been removed from the SWAN network, perhaps by tracking prevention techniques.                                                                                                                                                                                                                    | `Nx09ZGRCrt8kByjl_HKA`                            |
| tcString              | Optional       | The TCF string encoded in an OWID if provided by the publisher. Used if the data has been removed from the SWAN network, perhaps by tracking prevention techniques.                                                                                                                                                                                               | `Nx09ZGRCrt8kByjl_HKA`                            |
| title                 | Optional       | The title to display in the browser window during the operation.                                                                                                                                                                                                                                                                                                  | Getting your preferences                          |
| useHomeNode           | Optional       | Used to prevent the SWAN operator from using the home node even if it contains a current copy of the data. True to use the home node, false to ignore home nodes. Should be set to false when the caller does not want to rely on the data held in the home node alone and wants to force more nodes to be consulted before returning the data. Defaults to true. | true                                              |
| X-Forwarded-For       | Optional       | The HTTP header value for X-Forwarded-For as provided to the requesting web server. Used to calculate the home node and reduce the number of redirects.                                                                                                                                                                                                           | `23.245.23.65`                                    |

#### Validation

The operation will fail with a bad request or not authorized response under the
following conditions.

-   The access key is invalid.

-   The request is made from a web browser.

-   Mandatory fields are missing.

#### SWAN data

If the optional `SWID`, `pref` or `tcString` parameters provided are not valid
OWIDs that can be verified by the SWAN Operator with the creator, they are
ignored. The `SWID` must also have been created by a SWAN access node that is
known to the SWAN access node being called. This approach provides SWAN
participants that contract with SWAN Operators the option of helping the network
retain data over a longer period of time when used with web browsers that seek
to interfere with the cookie persistence settings of domains used by the SWAN
network.

As OWIDs are encoded with time information this time information is used to
establish which version of the value should be used in the event of a conflict
with existing data. If a caller passes values to the API that are older than the
current values held in SWAN then these values will be ignored. The caller will
receive the newer values and update their values accordingly.

The email address or salt is not available as an optional parameter because it
will never be known to the caller that is seeking to retrieve the SWAN data for
the purposes of supporting advertising. The email and salt are only ever shared
with the User Interface Provider and only for the purposes of facilitating user
management of that data.

#### Example

The following GET HTTP requests to a SWAN Operator using the domain
swan-operator.com for the access node, will result in a URL being returned.

`https://swan-operator.com/swan/api/v1/fetch?returnUrl=http%3A%2F%2Fnew-pork-limes.uk%2F&accessKey=[YOUR
ACCESS KEY]`

`https://swan-operator.com/swan/api/v1/fetch?backgroundColor=%23fff&displayUserInterface=true&message=Getting+things+ready.+New+Pork+Limes+rocks%21&messageColor=%23343a40&postMessageOnComplete=false&progressColor=%236c757d&remoteAddr=127.0.0.1%3A63639&returnUrl=http%3A%2F%2Fnew-pork-limes.uk%2F&title=Your+Preference+Management&accessKey=[YOUR
ACCESS KEY]`

Assuming the `accessKey` is valid, and a return URL is provided, and the request
is made from a server and not a web browser, the response will be a URL that
must be used within the next few seconds. The domain used in the response will
be chosen by the SWAN Operator and is likely to be the home node for the SWAN
network for the specific web browser dependent on the `X-Forwarded-For` and
`remoteAddr` optional parameters provided. The following is an example of such a
URL.

`http://swan-home-node.com/MTYuNTFkYi51azE2...uUXCQttStd0-vfNw`

The URL can be used in one of three ways depending on the input parameters.

#### Primary redirection

If `javascript` and `postMessageOnComplete` attributes are false.

The URL will display a user interface that should be used to redirect the
primary navigation of the browser to. The browser will return to the URL
provided in the returnUrl parameter with the encrypted SWAN data appended. Only
the SWAN access node associated with either the called SWAN Operator or
specified in the accessNode parameter can be used to decrypt the encrypted data.

#### Popup window

If the `postMessageOnComplete` attribute is set to true then the process will
complete with a post message to the opening window. Rather than navigating the
web browser to the return URL the caller should open a new window to perform the
SWAN operation. The following code provides an example.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
var w = 500;
var h = 625;
var x = screen.width/2 - w/2;
var y = screen.height/2 - h/2;
swanWindow = window.open(
    [URL returned from fetch],
    "swan",
    "width=" + w + ",height=" + h + ",left=" + x + ",top=" + y +
    ",directories=0,titlebar=0,toolbar=0,location=0,status=0," +
    "menubar=0,scrollbars=no,resizable=no");
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When the SWAN operation completes in that window the following JavaScript will
be used to communicate to the caller the encrypted SWAN data as a base 64
string.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
window.opener.postMessage("[Encrypted SWAN string]","[ReturnURL]");
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If is the responsibility of the caller to open the window and handle closing the
window when this post message is received. The returnUrl provided should match
the URL of the opening window.

#### Javascript

If the `javascript` parameter is set to true then the URL will return JavaScript
code which will run immediately. This can be included on the page server side as
an HTML script element such as the following.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
<script src="[URL returned from fetch]" type="text/javascript"></script>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Alternatively other JavaScript code could be used to insert the element after
the page has rendered. The following example shows how this could be achieved.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
var s = document.createElement("script");
s.src ="[URL returned from fetch]";
document.currentScript.parentNode.appendChild(s);
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The caller must ensure there is a global function titled swanComplete to the
encrypted SWAN data. This function will be responsible for decrypting the SWAN
data, possibly by making a call to a custom server-side endpoint.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
function swanComplete(encrypted) {
    // Decrypt the encrypted data.
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The encrypted data must be decrypted using the [decrypt](#decrypt) or
[decrypt-raw](#decrypt-raw) actions.

### Decrypt

The data takes an encrypted base 64 string, returned to the caller via another
method such as in a URL path segment, in the content of a client side
postMessage or via a JavaScript call to the global method swanComplete. The
[fetch](#fetch) action described previously is usually called prior to the
decrypt action.

A decrypted version of the input for use by the caller under the terms of the
[SWAN model terms](model-terms-explainer.md) is returned.

The encrypted input data must be generated specifically with the intention of
the SWAN Operator being called decrypting it. The access node used for the
decrypt operation should match the `accessNode` parameter value provided to the
preceding [fetch](#fetch) or [update](#update) actions. The operation will fail
otherwise.

#### Input

| **Parameter** | **Validation** | **Example**                                            |
|---------------|----------------|--------------------------------------------------------|
| accessKey     | Mandatory      | `123`                                                  |
| encrypted     | Mandatory      | `5dq6LhxaSBF…Nx09ZGRCrt8kByjl_HKA_1DscE7d4xLXVbz0_l1c` |

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

<https://swan-operator.com/swan/api/v1/decrypt?accessKey=123&encrypted=5dq6LhxaSBF…Nx09ZGRCrt8kByjl_HKA_1DscE7d4xLXVbz0_l1c>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#### Validation

The operation will fail with a Bad Request response under the following
conditions.

-   The access key is invalid.

-   The request is made from a web browser.

-   The encrypted data has been corrupted and cannot be decoded.

-   The encrypted data was intended for a different SWAN Operator and can
    therefore not be decrypted by the one called. See the accessNode parameter
    of the fetch or update action.

-   The time period for the validity of the data has elapsed.

#### Fields

Five SWAN fields are returned. Each field has four attributes associated with
them that are described in the following table.

| **Attribute** | **Format**                            | **Description**                                                                                                                                                                              |
|---------------|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Key           | The SWAN data field name              | String either swid, sid, pref, tcString or val.                                                                                                                                              |
| Created       | UTC date and time string              | The date and time when the data was created in the SWAN network.                                                                                                                             |
| Expires       | UTC date and time string              | The data and time after which any copies of the data should no longer be used and the value revalidated with SWAN. Typically used by the caller when setting the expiry time of any cookies. |
| Value         | Base 64 encoded string with the value |                                                                                                                                                                                              |

#### Example

The SWAN data items available for readonly operation and distribution
exclusively to SWAN participants bound by the model terms are returned.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[

{

"Key": "sid",

"Created": "2021-03-28T00:00:00Z",

"Expires": "2021-03-28T11:33:56.4466785Z",

"Value": "AjUxZGEudWsAJfEJAB… df9/uXPU="

},

{

"Key": "swid",

"Created": "2021-03-28T00:00:00Z",

"Expires": "2021-03-28T11:33:56.4466785Z",

"Value": "AjUxZGEudWsAo/AJ…X6rpasa6xjJFg=="

},

{

"Key": "stop",

"Created": "2021-03-28T00:00:00Z",

"Expires": "2021-03-28T11:33:56.4466785Z",

"Value": null

},

{

"Key": "pref",

"Created": "2021-03-28T00:00:00Z",

"Expires": "2021-03-28T11:33:56.4466785Z",

"Value": "AmNtcC5zd2FuLWRlbW8…AkY/HAZI="

},

{

"Key": "val",

"Created": "2021-03-28T00:00:00Z",

"Expires": "2021-03-28T11:33:56.4466785Z",

"Value": "2006-01-02T15:04:05Z07:00"

}

] 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#### Examples

The publisher receives a URL of the following form.

`http://publisher.com/5dq6LhxaSBF…Nx09ZGRCrt8kByjl_HKA_1DscE7d4xLXVbz0_l1c`

The last segment of the path is the encrypted SWAN data that needs to be
decrypted and turned into usable SWAN data. The publisher is responsible for
extracting the input data and passing it to the decrypt action.

### Decrypt Raw

The decrypt-raw action takes the same parameters as the decrypt operation.
However, it returns the information that a User Interface Provider needs to
display the user interface along with the data needed to use when calling the
[update](#_Update) action to update the SWAN network and return the web browser
to the caller.

**The data returned from** `decrypt-raw` **can never be used for any purpose
other than to provision a user interface. This is a strict requirement of the
model terms.**

#### Returned Fields

| **Attribute**                                                                                                                                   | **Format**                                                                  | **Description**                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| email                                                                                                                                           | OWID                                                                        | The email address as an OWID if available. The payload is the email string.               |
| pref                                                                                                                                            | OWID                                                                        | The preferences as an OWID if available. The payload will be the string “on” or “off”.    |
| salt                                                                                                                                            | OWID                                                                        | The salt as an OWID if available. The payload is the base 64 encoded version of the salt. |
| swid                                                                                                                                            | OWID                                                                        | The SWID as an OWID if available. The payload is a GUID associated with the web browser.  |
| tcString                                                                                                                                        | OWID                                                                        | The TCF string as an OWID if available. The payload is the TCF string.                    |
| accessNode backgroundColor displayUserInterface message messageColor postMessageOnComplete javaScript useHomeNode progressColor returnUrl title | The same meaning as those provided as input to the [fetch](#_Fetch) action. |                                                                                           |

#### Example

[

{

"accessNode": "swan-acess-node.com",

"backgroundColor": "\#f5f5f5",

"displayUserInterface": "true",

"email": "test@email.com",

"message": "Hang tight. We're getting things ready.",

"messageColor": "darkslategray",

"postMessageOnComplete": "false",

"pref": "on",

"progressColor": "darkgreen",

"returnUrl": "http://new-pork-limes.uk/",

"swid":
"AjUxZGEudWsAf/AJABAAAABELOODXltP4q2r0aTO0v4GlzOu0WiLOi+6Av4QMgwv9ZL/yIQvmY0CCBhKIFKduS/2e2QQDoFIHrOGga4cHM27Tskgjx+JlITydAckF2b3Ug",

"title": "SWAN Demo"

}

]

### Update

User Interface Providers (UIPs) can use this update action to update the SWAN
network. The following fields are passed to the transaction.

Only the SWAN Operators can create SWIDs. See the method
[CreateSWID](#_CreateSWID) to generate new SWIDs.

All the values for `email`, `pref`, `salt` and `tcString` must the turned into
OWIDs before passing them in the update request.

#### Input

| **Attribute**                                                                                                                                   | **Format**                                                                  | **Description**                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| email                                                                                                                                           | OWID                                                                        | The raw email address contained in the payload of an OWID generated by the UIP.               |
| pref                                                                                                                                            | OWID                                                                        | The preference string contained in the payload of an OWID generated by the UIP.               |
| swid                                                                                                                                            | OWID                                                                        | The SWID as an OWID generated by the SWAN Operator that created it via a call to create SWID. |
| salt                                                                                                                                            | OWID                                                                        | The salt as a base 64 string contained in the payload of an OWID generated by the UIP.        |
| tcString                                                                                                                                        | OWID                                                                        | The TCF string contained in the payload of an OWID generated by the UIP.                      |
| accessNode backgroundColor displayUserInterface message messageColor postMessageOnComplete javaScript useHomeNode progressColor returnUrl title | The same meaning as those provided as input to the [fetch](#_Fetch) action. |                                                                                               |

#### Validation

The operation will fail with a Bad Request response under the following
conditions.

-   The access key is invalid.

-   The request is made from a web browser.

-   Mandatory fields are missing.

-   The OWID format parameters are not valid OWIDs where they are provided.

Once the URL is returned it behaves in the same manner as the [fetch](#_Fetch)
action.

### Stop

This action is similar to update and fetch in that it will write data to the
SWAN network.

Unlike other SWAN data the stop attribute is a list of domains stored in plain
text. No OWIDs are used. The caller provides the `host` they wish to stop in the
host parameter. The `accessKey` and other parameters are identical to the
[fetch](#_Fetch) action.

#### Input

| **Parameter**                                                                                                                                   | **Validation**                                                              | **Example**          |
|-------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|----------------------|
| accessKey                                                                                                                                       | Mandatory                                                                   | `123`                |
| host                                                                                                                                            | Mandatory                                                                   | `domain-to-stop.com` |
| accessNode backgroundColor displayUserInterface message messageColor postMessageOnComplete javaScript useHomeNode progressColor returnUrl title | The same meaning as those provided as input to the [fetch](#_Fetch) action. |                      |

#### Validation

The operation will fail with a Bad Request response under the following
conditions.

-   The access key is invalid.

-   The request is made from a web browser.

-   Host field is missing.

#### Example

The following request will return a URL that should be used as described in the
[fetch](#fetch) example.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://swan-operator.com/swan/api/v1/stop?accessKey=123\&host=domain-to-stop.com 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### CreateSWID

Passed an access key and returns a byte array containing a SWID OWID generated
by the SWAN Operator.

#### Validation

The operation will fail with an authentication error response under the
following conditions.

-   The access key is invalid.

#### Example

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://swan-operator.com/swan/api/v1/create-swid?accessKey=123
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Home Node

Passed an access key, and the optional parameters X-Forwarded-For and remoteAddr
to return the home node domain as text string for the web browser associated
with the data.

#### Validation

The operation will fail with an authentication error response under the
following conditions.

-   The access key is invalid.

#### Example

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
https://swan-operator.com/swan/api/v1/home-node?accessKey=123
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
