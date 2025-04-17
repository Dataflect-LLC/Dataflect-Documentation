[Go back](index)

This documentation is for Dataflect version 2.0.2. For older versions of Dataflect documentation contact us at [support@dataflect.com](mailto:support@dataflect.com).

For additional information, visit [dataflect.com](https://dataflect.com).

# Table of Contents

1. [About Dataflect](#about-dataflect)
2. [Dataflect License](#dataflect-license)
3. [Dataflect Support](#dataflect-support)
4. [Disclaimer](#disclaimer)
5. [Installation](#installation)
6. [Dataflect Roles](#dataflect-roles)
   1. [Adding Users to Dataflect Roles](#adding-users-to-dataflect-roles)
7. [Configuring Dataflect Settings](#configuring-dataflect-settings)
   1. [Enforce Allowed Domains?](#enforce-allowed-domains)
8. [Configuring Dataflect Credentials](#configuring-dataflect-credentials)
9. [Using Dataflect](#using-dataflect)
   1. [Basic Examples](#basic-examples)
   2. [Syntax](#syntax)
   3. [Required Arguments](#required-arguments)
   4. [Optional Arguments](#optional-arguments)
10. [Dataflect Query Builder Overview](#dataflect-query-builder-overview)
   1. [Using the Query Builder to Create a Custom Search Command](#using-the-query-builder-to-create-a-custom-search-command)
11. [Normalizing Dataflect Results with Props](#normalizing-dataflect-results-with-props)
    1. [Field Aliases](#field-aliases)
    2. [Field Extractions](#field-extractions)
12. [Dataflect Logging Overview](#dataflect-logging-overview)
13. [Dataflect Monitoring Dashboard](#dataflect-monitoring-dashboard)
14. [Dataflect Modifiers](#dataflect-modifiers)
15. [Dataflect Known Issues](#dataflect-known-issues)

# About Dataflect
[Go to Top](#)

Dataflect is available as a premium Splunk application that allows users to easily integrate with third-party APIs directly from the Splunk ecosystem. If you would like to purchase additional license you can do so by contacting [sales@dataflect.com](mailto:sales@dataflect.com).

Dataflect provides the following high level capabilities:

- Perform searches, aggregations, and analytics over data returned via API using Splunk's powerful capabilities.
- Ingest output from any API into Splunk without the need for a developer to create a scripted input.
- Retrieve remote data and store as a lookup in your Splunk environment.
- No-code custom scripted lookup functionality - enrich your logs with information from any API.
- No-code custom alert action functionality, engage with any API directly from Splunk.

# Dataflect License
[Go to Top](#)

To obtain your Dataflect license contact us directly at [sales@dataflect.com](mailto:sales@dataflect.com).

# Dataflect Support
[Go to Top](#)

Dataflect is a developer supported Splunkbase application. If you encounter issues using the application please contact [support@dataflect.com](mailto:support@dataflect.com) for assistance.

# Disclaimer
[Go to Top](#)

Dataflect is in no way associated with Splunk, Inc. or any of its affiliates. Dataflect is a third party developed and maintained Splunk Application.

Dataflect
Copyright (C) 2025 Dataflect LLC All Rights Reserved.

This software is provided by the copyright holder "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed. In no event shall the copyright owner be liable for any direct, indirect, incidental, special, exemplary, or consequential damages (including, but not limited to, procurement of substitute goods or services; loss of use, data, or profits; or business interruption) however caused and on any theory of liability, whether in contract, strict liability, or tort (including negligence or otherwise) arising in any way out of the use of this software, even if advised of the possibility of such damage.

You may not modify, distribute, sublicense, or sell this program or derivative works based upon it. The user is not allowed to distribute the software and must use it only for personal, non-commercial purposes. Use for any other purpose is expressly forbidden, and may result in severe civil and criminal penalties.

***

# Installation
[Go to Top](#)

Dataflect can be installed in Splunk Cloud environments and should be installed directly from Splunkbase via self-service install or with the help of Splunk Support. Dataflect is also supported in Splunk Enterprise (on-premise) environments.

***

# Dataflect Roles
[Go to Top](#)

Dataflect uses the standard access control system integrated with the Splunk platform. Dataflect allows for strict Role Based Access Control by restricting functionality and interaction with third party APIs on a per domain basis. 

In order to facilitate this, Dataflect leverages the following existing splunk roles:

| Role    |                                                                                                                                                   Description                                                                                                                                                  |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| admin/sc_admin | Installs and configures the Dataflect app within your Splunk deployment. Users must be assigned this role in order to manage Dataflect settings, configure allowed domains, and manage custom commands and custom alert actions created with Dataflect. Users with this role will have visibility into the Monitoring dashboard. Can execute the dfsearch, dfenrich, and dfengage commands. |
| power  | Can execute the dfsearch, dfenrich, and dfengage commands. |

***

## Adding users to Dataflect roles
[Go to Top](#)

After installation Dataflect must be configured by a Splunk user with the admin or sc_admin role.

# Configuring Dataflect Settings
[Go to Top](#)

Users with admin or sc_admin roles will have visibility into the _Settings_ page (Configure --> Settings) within the Dataflect application. The settings that can be configured are:

## Enforce allowed domains?
[Go to Top](#)

The Allowed Domains list allows an administrator to provide granular permissions to interact with an API. Using this list you can configure per domain permissions:

- **Domain (FQDN)**: The Fully Qualified Domain Name (FQDN) to which the permissions apply (e.g. api.example.com).
  - **NOTE**: Permissions apply to the FQDN only, not any sub-domains.
- When this is set to true, requests made to FQDNs not in list will fail to execute. When this is set to false users can make requests to any FQDN.

## Configure License
[Go to Top](#)

Once you have obtained your license from Dataflect, you must enter your license key in the Dataflect Settings page by clicking "Edit" next to License Key and entering your base64 encodced license. Your text should include the header and footer (-----BEGIN LICENSE FILE----- and ----- END LICENSE FILE-----). If you experience any issues contact [support@dataflect.com](mailto:support@dataflect.com).

Your license can only be used on one Splunk Cloud instance.

# Configuring Dataflect Credentials
[Go to Top](#)

On the _Credentials_ page (Configure --> Credentials) an administrator can create API credentials that can be used to authenticate Datflect Search commands. When creating credentials you may choose from the following supported types:

- **API Key (simple)**: A simple text based field that you can use to store an API Key. The value of this credential type must be a string.
- **API Key (headers)**: For more complex use cases, or when additional fields need to be provided (e.g. Credential ID, Credential Value). The value of a credential of this type must be a valid JSON object.
- **Basic Auth**: When authentication to an API key will be accomplished via username and password.
- **oAuth**: When authentication to an API key will be accomplished via oAuth workflow.

Credentials are stored securely using Splunk's native Secrets Storage.

Additional forms of authentication can be configured with a paid Dataflect license, contact [sales@dataflect.com](mailto:sales@dataflect.com) for additional information.

# Using Dataflect
[Go to Top](#)

Dataflect's core functionality is made accessible primarily via three custom search commands that ship with the application.

- **dfsearch**: Flexibly query any API

**NOTE: the dfsearch command can only be run from the Dataflect app. If you want to extend the capability outside of the app you must create custom search commands using the [Query Builder Dashboard](#using-the-query-builder-to-create-a-custom-search-command), and then set the sharing to Global in the Configure --> Commands Page.

## Basic Examples
[Go to Top](#)

```| dfsearch url="https://uselessfacts.jsph.pl/api/v2/facts/today"```

The above example demonstrates basic usage of the dfsearch command, without any additional parameters specified.

## Syntax
[Go to Top](#)

**Simple:**

| dfsearch url="\<_url_>"

**Complete:**

Required syntax is in **bold**.

**\| dfsearch**  
**[url=\<string>]**  
[endpoint=\<string>]  
[parameters=\<string>]  
[credential=\<string>]  
[containing_field=\<string>]  
[timestamp_field=\<string>]  
[timestmp_strf=\<string>]  
[limit=\<int>]  
[data=\<string>]  
[data_format=\<string>]  
[headers=\<string>]  
[rate_limit_calls=\<int>]  
[rate_limit_period=\<int>]  
[offset_field=\<string>]  
[text_line_breaker=\<string>]  
[text_line_ignore=\<string>]  
[text_line_headers=\<string>]  
[ingest=\<bool>]  
[ingest_index=\<string>]  
[ingest_sourcetype=\<string>]  
[include_fields=\<string>]  
[convert_table_array=\<bool>]  
[timeout=\<int>]

## Required arguments
[Go to Top](#)

**url**

- **Syntax:** url="_\<url>_"
- **Description:** The URL for the API that you are reaching out to. Supports only HTTPS. If no protocol is specified this will default to HTTPS. You can include the full URL path here, or leverage the **url** and **endpoint** parameters together to specify the full path. **Note**: Dataflect forces SSL certificate verification for all requests.
  - Examples:
    - <https://api.dataflect.com/v2/api>
    - api.dataflect.com/v2/api
    - api.dataflect.com

## Optional arguments
[Go to Top](#)

**endpoint**

- **Syntax:** endpoint="\<string>"
- **Description:** The API endpoint being targeted, will be appended to the **url** field. Can begin with or without a "/"

**parameters**

- **Syntax:** parameters="\<string>"
- **Description:**The parameters to pass with the API call. Use the name of the variable and the corresponding value, separated by an =. If multiple parameters apply, separate with "&".
  - Examples:
    - parameters="foo=bar&something=this is a string&id=23"
  - There is no need to URL Escape the parameters, this is handled by Dataflect.

**credential**

- **Syntax:** credential="\<string>"
- **Description:** The name of the credential to be used for interacting with the API. Behavior depends on the "Type" of credential this is associated with:
  - **api_key_headers**: The headers set in the credential are automatically added to your request, no action needed.
  - **api_key_simple:** Wherever the key needs to be entered, place "\<\<>>". This can be in either the **parameters** or the **headers**.
    - e.g. parameters="key=\<\<>>"
    - e.g. headers="{'API-KEY': \<\<>>}"
  - **basic:**: The user/pass are automatically passed with your request, no action needed.

**containing_field**

- **Syntax:** containing_field="\<string>"
- **Description:** The field within the API response that contains the information your are interested in viewing. For example, if your API response returns {"events": [{"foo": "bar", "id": "1"},{"foo": "baz", "id": "2"]}, you can specify the **containing_field**=events to return the containing events.
- **Default:** Defaults to "data". You can override this by setting containing_field="none".

**timestamp_field**

- **Syntax:** timestamp_field="\<string>"
- **Description:** The field within the API response that contains the timestamp

**timestamp_strf**

- **Syntax:** timestmp_strf="\<string>"
- **Description:** The stftime format of the timestamp_field (<https://strftime.org/>)
  - e.g. timestamp_strf="%Y-%m-%dT%H:%M:%S"

**limit**

- **Syntax:** limit="\<int>"
- **Description:** The maximum amount of results that you want to return. This will take effect if your API call returns results that are paginated. 
  - - **NOTE**: If the API "limit" parameter is greater than this, you may receive more than this number of results.

**data**

- **Syntax:** data="\<string>"
- **Description:** The "Body" of the request sent to the API. This must be a JSON-like object, using single quotes in place of double quotes.
  - e.g. data="{'foo': 'bar'}"
 
**data_format**

- **Syntax:** data_format="\<string>"
- **Description:** The data_format parameter specifies the format in which the request payload is sent to the server, such as JSON, raw bytes, or serialized data. This determines how the data is encoded and structured, ensuring that the server correctly interprets and processes the information. Common formats include JSON for structured and hierarchical data, raw for binary or custom data types, and serialized formats like Pickle or Protocol Buffers for transmitting complex objects. Selecting the appropriate data_format is crucial for compatibility and effective communication with the target API or web service, as it aligns the client’s data presentation with the server’s expected input.
- **Accepted Values:** json, raw, serialized
- **Default:** json

**headers**

- **Syntax:** headers="\<string>"
- **Description:** The "Header" of the request sent to the API. This must be a JSON-like object, using single quotes in place of double quotes.
  - e.g. headers="{'foo': 'bar'}"

**rate_limit_calls**

- **Syntax:** rate_limit_calls="\<int>"
- **Description:** Used in conjunction with **rate_limit_period**. If your API response requires pagination, you can implement rate limiting for subsequent calls required to return the full data set. The number set here will dictate the number of calls that can be made within the **rate_limit_period**.

**rate_limit_period**

- **Syntax:** rate_limit_period="\<int>"
- **Description:** Used in conjunction with **rate_limit_calls**. If your API response requires pagination, you can implement rate limiting for subsequent calls required to return the full data set. The number set here will dictate the number of seconds within which the **rate_limit_period** number of calls can be made.
  - e.g. rate_limit_calls=15 rate_limit_period=60
    - 15 calls per 60 seconds, after this limit is reached Dataflect will wait to make subsequent calls.

**offset_field**

- **Syntax:** offset_field="\<string>"
- **Description:** The offset field used by the destination API when making calls that require pagination. This will be "offset" by default but can be configured if an API uses a non-standard field.

**text_line_breaker**

- **Syntax:** text_line_breaker="\<regex>"
- **Description:** When the response returned from the API is a large text object, this parameter can be used to break the text into individual events. A capturing group must be defined, this is where the event will be broken. The capturing group may be empty.
  - e.g. text_line_breaker="}(){"
    - This would cause:
      - {"\_raw":"foo"}{"\_raw":"bar"}
    - To be returned as two events:
      - {"\_raw":"foo"}
      - {"\_raw":"bar"}

**text_line_ignore**
- **Syntax:** text_line_ignore="\<string>"
- **Description:** Only honored when parsing raw text response and a value is defined for text_line_breaker. Will ignore any lines that begin with the defined string. Does not accept regex patterns.
   - e.g. text_line_breaker="#"
      - will ignore: #This is a header row

**text_line_headers**
- **Syntax:** text_line_headers="\<string>"
- **Description:** Only honored when parsing raw text response and a value is defined for text_line_breaker. When your data is a comma separated list without headers, you can define the headers here.
   - e.g. if your raw event is:
     - 8.8.8.8,dns.google.com
   - you can set text_line_headers="ip,fqdn", and your events will extract ip="8.8.8.8" and fqdn="dns.google.com"

**ingest**

- **Syntax:** ingest="\<boolean>"
- **Description:** Tells Dataflect whether to ingest the response from the API in addition to returning as search results. Accepts boolean values 1 or 0.

**ingest_index**

- **Syntax:** ingest_index="\<string>"
- **Description:** Used in conjunction with **ingest**. Tells Dataflect what index to send events to, defaults to "main".

**ingest_sourcetype**

- **Syntax:** ingest_sourcetype="\<string>"
- **Description:** Used in conjunction with **ingest**. Tells Dataflect what sourcetype to associate with ingested events, defaults to "stash".

**include_fields**

- **Syntax:** include_fields="\<string>"
- **Description:** A comma separated list of field names to include in the events returned from your API call. **NOTE**: This filtering occurs after normalization from the **props** setting. This means that if you change a field name with **props** you should specify the new name in this list.
  - e.g.  include_fields=""

**convert_table_array**
- **Syntax:** convert_table_array="\<bool>"
- **Description:** If your API call returns a table array this will convert the table array to  JSON object to be handled and parsed by Splunk.
- **Accepted Values:** true, false

**timeout**
- **Syntax:** timeout="\<int>"
- **Description:** The amount of time to wait for a response from the API endpoint.
- **Default Value:** 60

- **dfenrich**: Enrich your logs with information obtained from any API, acts as a no-code custom scripted lookup.

**NOTE: the dfenrich command can only be run from the Dataflect app.

## Basic Examples
[Go to Top](#)

```
index=foo ip=*
| dfenrich url="https://somerandomsite.notadomain/api/v2/ip/lookup/$ip$"
```

The above example demonstrates basic usage of the dfenrich command, without any additional parameters specified. The field to match on in events is passed as the $ip$ variable. Only one field can be passed in from the source events.

## Syntax
[Go to Top](#)

**Simple:**

| dfenrich url="\<_url_>"

**Complete:**

Required syntax is in **bold**.

**\| dfenrich**  
**[url=\<string>]**  
[endpoint=\<string>]  
[parameters=\<string>]  
[credential=\<string>]  
[containing_field=\<string>]  
[output_fields=\<string>]
[data=\<string>]
[data_format=\<string>]
[headers=\<string>]
[method=\<string>]
[rate_limit_calls=\<int>]  
[rate_limit_period=\<int>]  

## Required arguments
[Go to Top](#)

**url**

- **Syntax:** url="_\<url>_"
- **Description:** The URL for the API that you are reaching out to. Supports only HTTPS. If no protocol is specified this will default to HTTPS. You can include the full URL path here, or leverage the **url** and **endpoint** parameters together to specify the full path. **Note**: Dataflect forces SSL certificate verification for all requests.
  - Examples:
    - <https://api.dataflect.com/v2/api>
    - api.dataflect.com/v2/api
    - api.dataflect.com

## Optional arguments
[Go to Top](#)

**endpoint**

- **Syntax:** endpoint="\<string>"
- **Description:** The API endpoint being targeted, will be appended to the **url** field. Can begin with or without a "/"

**parameters**

- **Syntax:** parameters="\<string>"
- **Description:**The parameters to pass with the API call. Use the name of the variable and the corresponding value, separated by an =. If multiple parameters apply, separate with "&".
  - Examples:
    - parameters="foo=bar&something=this is a string&id=23"
  - There is no need to URL Escape the parameters, this is handled by Dataflect.

**credential**

- **Syntax:** credential="\<string>"
- **Description:** The name of the credential to be used for interacting with the API. Behavior depends on the "Type" of credential this is associated with:
  - **api_key_headers**: The headers set in the credential are automatically added to your request, no action needed.
  - **api_key_simple:** Wherever the key needs to be entered, place "\<\<>>". This can be in either the **parameters** or the **headers**.
    - e.g. parameters="key=\<\<>>"
    - e.g. headers="{'API-KEY': \<\<>>}"
  - **basic:**: The user/pass are automatically passed with your request, no action needed.

**containing_field**

- **Syntax:** containing_field="\<string>"
- **Description:** The field within the API response that contains the information your are interested in viewing. For example, if your API response returns {"events": [{"foo": "bar", "id": "1"},{"foo": "baz", "id": "2"]}, you can specify the **containing_field**=events to return the containing events.
- **Default:** Defaults to "data". You can override this by setting containing_field="none".

**output_fields**

- **Syntax:** containing_field="\<string>"
- **Description:** A comma separated list of field names to include in the events returned from your API call.
  - e.g.  include_fields="foo,bar"

**data**

- **Syntax:** data="\<string>"
- **Description:** The "Body" of the request sent to the API. This must be a JSON-like object, using single quotes in place of double quotes.
  - e.g. data="{'foo': 'bar'}"
 
**data_format**

- **Syntax:** data_format="\<string>"
- **Description:** The data_format parameter specifies the format in which the request payload is sent to the server, such as JSON, raw bytes, or serialized data. This determines how the data is encoded and structured, ensuring that the server correctly interprets and processes the information. Common formats include JSON for structured and hierarchical data, raw for binary or custom data types, and serialized formats like Pickle or Protocol Buffers for transmitting complex objects. Selecting the appropriate data_format is crucial for compatibility and effective communication with the target API or web service, as it aligns the client’s data presentation with the server’s expected input.
- **Accepted Values:** json, raw, serialized
- **Default:** json

**headers**

- **Syntax:** headers="\<string>"
- **Description:** The "Header" of the request sent to the API. This must be a JSON-like object, using single quotes in place of double quotes.
  - e.g. headers="{'foo': 'bar'}"

**method**

- **Syntax:** method="\<string>"
- **Description:** The "method" of the request sent to the API. Accepts "get" or "post".
  - e.g. method=post
- **Default:** get

**rate_limit_calls**

- **Syntax:** rate_limit_calls="\<int>"
- **Description:** Used in conjunction with **rate_limit_period**. If your API response requires pagination, you can implement rate limiting for subsequent calls required to return the full data set. The number set here will dictate the number of calls that can be made within the **rate_limit_period**.

**rate_limit_period**

- **Syntax:** rate_limit_period="\<int>"
- **Description:** Used in conjunction with **rate_limit_calls**. If your API response requires pagination, you can implement rate limiting for subsequent calls required to return the full data set. The number set here will dictate the number of seconds within which the **rate_limit_period** number of calls can be made.
  - e.g. rate_limit_calls=15 rate_limit_period=60
    - 15 calls per 60 seconds, after this limit is reached Dataflect will wait to make subsequent calls.

- **dfengage**: Engage with any API. Implement automations and integrations directly from a Splunk search, can be deployed as no-code custom alert actions.

**NOTE: the dfengage command can only be run from the Dataflect app. If you want to extend the capability outside of the app you must create custom alert action using the [Action Builder Dashboard](#using-the-action-builder-to-create-a-custom-alert-action), and then set the sharing to Global in the Configure --> Commands Page.

## Basic Examples
[Go to Top](#)

```
| dfengage url="https://somerandomsite.notadomain/api/v2/user?username=foo&acition=disable"
```

The above example demonstrates basic usage of the dfengage command, without any additional parameters specified. The dfengage command must be the first command in a search string.

## Syntax
[Go to Top](#)

**Simple:**

| dfengage url="\<_url_>"

**Complete:**

Required syntax is in **bold**.

**\| dfenrich**  
**[url=\<string>]**  
[endpoint=\<string>]  
[parameters=\<string>]  
[credential=\<string>]  
[containing_field=\<string>]  
[data=\<string>]
[data_format=\<string>]
[headers=\<string>]
[method=\<string>]

## Required arguments
[Go to Top](#)

**url**

- **Syntax:** url="_\<url>_"
- **Description:** The URL for the API that you are reaching out to. Supports only HTTPS. If no protocol is specified this will default to HTTPS. You can include the full URL path here, or leverage the **url** and **endpoint** parameters together to specify the full path. **Note**: Dataflect forces SSL certificate verification for all requests.
  - Examples:
    - <https://api.dataflect.com/v2/api>
    - api.dataflect.com/v2/api
    - api.dataflect.com

## Optional arguments
[Go to Top](#)

**endpoint**

- **Syntax:** endpoint="\<string>"
- **Description:** The API endpoint being targeted, will be appended to the **url** field. Can begin with or without a "/"

**parameters**

- **Syntax:** parameters="\<string>"
- **Description:**The parameters to pass with the API call. Use the name of the variable and the corresponding value, separated by an =. If multiple parameters apply, separate with "&".
  - Examples:
    - parameters="foo=bar&something=this is a string&id=23"
  - There is no need to URL Escape the parameters, this is handled by Dataflect.

**credential**

- **Syntax:** credential="\<string>"
- **Description:** The name of the credential to be used for interacting with the API. Behavior depends on the "Type" of credential this is associated with:
  - **api_key_headers**: The headers set in the credential are automatically added to your request, no action needed.
  - **api_key_simple:** Wherever the key needs to be entered, place "\<\<>>". This can be in either the **parameters** or the **headers**.
    - e.g. parameters="key=\<\<>>"
    - e.g. headers="{'API-KEY': \<\<>>}"
  - **basic:**: The user/pass are automatically passed with your request, no action needed.

**containing_field**

- **Syntax:** containing_field="\<string>"
- **Description:** The field within the API response that contains the information your are interested in viewing. For example, if your API response returns {"events": [{"foo": "bar", "id": "1"},{"foo": "baz", "id": "2"]}, you can specify the **containing_field**=events to return the containing events.
- **Default:** Defaults to "data". You can override this by setting containing_field="none".

**data**

- **Syntax:** data="\<string>"
- **Description:** The "Body" of the request sent to the API. This must be a JSON-like object, using single quotes in place of double quotes.
  - e.g. data="{'foo': 'bar'}"
 
**data_format**

- **Syntax:** data_format="\<string>"
- **Description:** The data_format parameter specifies the format in which the request payload is sent to the server, such as JSON, raw bytes, or serialized data. This determines how the data is encoded and structured, ensuring that the server correctly interprets and processes the information. Common formats include JSON for structured and hierarchical data, raw for binary or custom data types, and serialized formats like Pickle or Protocol Buffers for transmitting complex objects. Selecting the appropriate data_format is crucial for compatibility and effective communication with the target API or web service, as it aligns the client’s data presentation with the server’s expected input.
- **Accepted Values:** json, raw, serialized
- **Default:** json

**headers**

- **Syntax:** headers="\<string>"
- **Description:** The "Header" of the request sent to the API. This must be a JSON-like object, using single quotes in place of double quotes.
  - e.g. headers="{'foo': 'bar'}"

**method**

- **Syntax:** method="\<string>"
- **Description:** The "method" of the request sent to the API. Accepts "put", "patch", "post", "delete".
  - e.g. method=post
- **Default:** post

# Dataflect Query Builder Overview
[Go to Top](#)

Dataflect ships with a **Query Builder** view that makes it easy for users to interact with the **dfsearch** command. Users can access the **Query Builder** dashboard by navigating to the Dataflect Application and selecting **Query Builder**.  The same options are available which are discussed in this documentation for the dfsearch command.

## Using the Query Builder to Create a Custom Search Command
[Go to Top](#)

Once a user has executed a search using the **Query Builder**, the option to "Create Custom Search Command" appears at the top of the page. Click the link to expand this option.

The expanded section includes the underlying query that has been executed. From here a user can enter an "Custom Search Command Name Name" in the text box under the search and click "Create" to create a custom search command that will execute the underlying search.

Users have the option to enter parameters within the search query using the format $token$. Tokens must be set when the custom search command is executed using the "parameters".

Update permissions as necessary by navigating to Dataflect --> Configure --> Commands

# Dataflect Action Builder Overview
[Go to Top](#)

Dataflect ships with a **Query Builder** view that makes it easy for users to interact with the **dfsearch** command. Users can access the **Query Builder** dashboard by navigating to the Dataflect Application and selecting **Query Builder**.  The same options are available which are discussed in this documentation for the dfsearch command.

## Using the Action Builder to Create a Custom Alert Action
[Go to Top](#)

Once a user has executed a search using the **Action Builder**, the option to "Create Custom Alert Action" appears at the top of the page. Click the link to expand this option.

The expanded section includes the underlying query that has been executed. From here a user can enter an "Custom Alert Action Name" in the text box under the search and click "Create" to create a custom alert action that will execute the underlying search.

Users have the option to enter parameters within the search query using the format $result.somefield$. Tokens will be passed in from the results passed into the alert action.

Update permissions as necessary by navigating to Dataflect --> Configure --> Actions.

# Normalizing Dataflect Results with Props
Dataflect dfsearch results can be normalized using the default Splunk props.conf configurations, with some key caveats.

## Field Aliases
[Go to Top](#)

To create a field alias, you will navigate in Splunk Web to Settings --> Fields --> Field aliases.

- Select "New Field Alias"
- For "Name": dfprops:\<insert the applicable fqdn>  
   - e.g. dfprops:api.coincap.io
- For "Apply To" select sourcetype, and use the same value that you used for "Name"
- For "Field aliases" the field on the left will be the name of the field as originally returned from the API. The field on the right will be the new field name you want to use.
- **NOTE:** The "Overwrite field values" has no impact, the original field name will always be overwritten.

## Field Extractions
[Go to Top](#)

To create a field extraction, you will navigate in Splunk Web to Settings --> Fields --> Field extractions.

- Select "New Field Extraction"
- For "Name": Select the original field name from the API response that you will be extracting the new fields from.
- For "Apply To" select sourcetype, and use dfprops:\<insert the applicable fqdn>  
   - e.g. dfprops:api.coincap.io
- For "Type" select Inline
- For "Extraction/Transform" enter a regex expression with at least one named capturing group.
   - e.g.
      - name: supply
      - Extraction/Transform: ^(?<supply_rounded>\d+)\.(?<supply_decimal>\d+)
      - The fields "supply_rounded" and "supply_decimal" will be extracted from the "supply" field if the regex pattern finds a match.

# Dataflect Logging Overview
[Go to Top](#)

Each of the Dataflect commands are logged to Splunk's \_internal index. These logs can be found by searching:

```index=_internal source=*dataflect.log```

Dataflect logs the user executing each command, as well as information regarding which APIs they are communicating with, the number of calls they are making, and the volume of data that is being sent out of Splunk (egress) and returned back to Splunk (ingress).

# Dataflect Monitoring Dashboard
[Go to Top](#)

To simplify monitoring, Dataflect provides a **Monitoring** Dashboard which provides some key metrics. To access the **Monitoring** Dashboard, navigate to the Dataflect Application, and select **Monitoring** from the navigation menu.

From within the dashboard you can filter based on time, command and/or the domain being communicated with.

The following modifiers can be used with any Dataflect custom search command:

# Dataflect Modifiers
[Go to Top](#)

- Enter current UTC timestamp:
  - insert $STRF:<strf time format>:STRF$
    - <https://strftime.org/>
    ```
    | dfsearch url=<https://example.dataflect.com> credential=dataflect_example headers="{'User-Agent': 'dataflect-demo', 'Accept': '_/\_', 'x-ms-version': '2020-04-08', 'x-ms-date': '$STRF:%a, %d %b %Y %H:%M:%S GMT:STRF$', 'accept-encoding': 'gzip, deflate'}"
    ```

# Dataflect Known Issues
[Go to Top](#)

- When executing a dfsearch command, you cannot pass the results directly into the "anomalydetection" or "outlier" commands. You must first pass the results through the "stats" command. Example:
  - This will not work:
      ```
      | dfsearch url="<https://api.coincap.io/v2/assets">
      | anomalydetection changePercent24Hr
      ```
  - This will work:
      ```
      | dfsearch url="<https://api.coincap.io/v2/assets"> 
      | stats count by symbol changePercent24Hr  
      | anomalydetection changePercent24Hr
      ```
