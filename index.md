# About Dataflect Search

Dataflect Search is available as a free Splunk application that allows users to easily integrate with third-party APIs directly from the Splunk ecosystem. The free license currently allows for 150 searches per month. If you would like to purchase additional license you can do so by contacting [sales@dataflect.com][mailto:sales@dataflect.com].

There are additional capabilities available with a premium version of Dataflect that allows for no-code enrichment of your logs in Splunk, and no-code splunk alert alert actions that interact with 3rd party APIs. If you are interested in a demo of these capabilities contact [sales@dataflect.com][mailto:sales@dataflect.com].

Dataflect Search provides the following high level capabilities:

- Perform searches, aggregations, and analytics over data returned via API using Splunk's powerful capabilities.
- Ingest output from any API into Splunk without the need for a developer to create a scripted input.
- Retrieve remote data and store as a lookup in your Splunk environment.

# Dataflect Search License

To obtain your free Dataflect Search license contact us directly at [sales@dataflect.com](mailto:sales@dataflect.com).
The free Dataflect Search license is limited to 150 searches 

# Dataflect Search Support

Dataflect Search with a free license is a developer supported Splunkbase application. If you encounter issues using the application please contact [support@dataflect.com][mailto:support@dataflect.com] for assistance.

# Disclaimer

Dataflect Search is in no way associated with Splunk, Inc. or any of its affiliates. Dataflect Search is a third party developed and maintained Splunk Application.

Dataflect Search
Copyright (C) 2025 Dataflect LLC All Rights Reserved.

This software is provided by the copyright holder "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed. In no event shall the copyright owner be liable for any direct, indirect, incidental, special, exemplary, or consequential damages (including, but not limited to, procurement of substitute goods or services; loss of use, data, or profits; or business interruption) however caused and on any theory of liability, whether in contract, strict liability, or tort (including negligence or otherwise) arising in any way out of the use of this software, even if advised of the possibility of such damage.

You may not modify, distribute, sublicense, or sell this program or derivative works based upon it. The user is not allowed to distribute the software and must use it only for personal, non-commercial purposes. Use for any other purpose is expressly forbidden, and may result in severe civil and criminal penalties.

***

# Installation

Dataflect Search can be installed in Splunk Cloud environments and should be installed directly from Splunkbase via self-service install or with the help of Splunk Support.

***

# Dataflect Search Roles

Dataflect Search uses the standard access control system integrated with the Splunk platform. Dataflect allows for strict Role Based Access Control by restricting functionality and interaction with third party APIs on a per domain basis. 

In order to facilitate this, Dataflect adds three roles to any pre-existing roles within your Splunk deployment:

| Role    |                                                                                                                                                   Description                                                                                                                                                  |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| dfadmin | Installs and configures the Dataflect app within your Splunk deployment. Users must be assigned this role in order to manage Dataflect settings, configure allowed domains, and manage custom commands created with Dataflect Search. Users with this role will have visibility into the Monitoring dashboard. |
| dfpower | Is able to create custom dataflect search commands and is able to view the Monitoring dashboard (user will also require visibility into the _internal index for this dashboard to function properly) |
| dfuser  | Can execute the dfsearch command. |

***

## Adding users to Dataflect roles

After installation, you must assign at least one user to the _dfadmin_ role. You may assign as many additional users as applicable to the other roles listed above. In order to accomplish this:

1. From the system bar, click **Settings** > **Users**
2. Search for the user you wish to modify
3. Next to the desired user, in the **Actions** column, select the dropdown and pick **Edit**
4. In the pop-up, find the appropriate role next to **Assign roles** under the **Availablle item(s)** column (e.g. dfadmin)
   1. Click the desired role to add it to **Selected item(s)**
5. Click **Save**

# Configuring Dataflect Settings

Once a user has been added to the _dfadmin_ role, they will have visibility into the _Settings_ page (Configure --> Settings) within the Dataflect application. The settings that can be configured are:

## Enforce verification?

- Default Behavior: By default, verify is set to True unless configured otherwise in a search (e.g. \| dfsearch url=api.dataflect.com/api/v2/endpoint verify=false). This means that when you make an HTTPS request, requests will automatically verify the server’s SSL certificate against a set of trusted Certificate Authorities (CAs).
- What Verification Entails:
   - Certificate Validation: Ensures that the server’s SSL certificate is valid, not expired, and has been issued by a trusted CA.
   - Hostname Matching: Confirms that the certificate’s hostname matches the server’s hostname you’re trying to reach.
   - Chain of Trust: Verifies the chain of trust from the server’s certificate up to a root CA.
- Security Importance: Enabling verification (verify=True) helps prevent man-in-the-middle (MITM) attacks by ensuring that you’re communicating with the legitimate server and that the connection is secure.
- When this is set to true, users will be unable to override the default setting of True.

## Enforce allowed domains?

The Allowed Domains list allows an administrator to provide granular permissions to interact with an API. Using this list you can configure per domain permissions:

- **Domain (FQDN)**: The Fully Qualified Domain Name (FQDN) to which the permissions apply (e.g. api.example.com).
  - **NOTE**: Permissions apply to the FQDN only, not any sub-domains.
- When this is set to true, requests made to FQDNs not in list will fail to execute. When this is set to false users can make requests to any FQDN.

# Configuring Dataflect Credentials

On the _Credentials_ page (Configure --> Credentials) an administrator can create API credentials that can be used to authenticate Datflect search commands. When creating credentials you may choose from the following supported types:

- **API Key (simple)**: A simple text based field that you can use to store an API Key. The value of this credential type must be a string.
- **API Key (headers)**: For more complex use cases, or when additional fields need to be provided (e.g. Credential ID, Credential Value). The value of a credential of this type must be a valid JSON object.
- **Basic Auth**: When authentication to an API key will be accomplished via username and password.
- **oAuth**: When authentication to an API key will be accomplished via oAuth workflow.

Credentials are stored securely using Splunk's native Secrets Storage.

# Using Dataflect Search

Dataflect Search's core functionality is made accessible primarily via a custom search command that ship with the Application.

- **dfsearch**: Flexibly query any API

## Basic Examples

```Text SPL
| dfsearch url="https://uselessfacts.jsph.pl/api/v2/facts/today"
```

The above example demonstrates basic usage of the dfsearch command, without any additional parameters specified.

## Syntax

**Simple:**

| dfsearch url="\<_url_>"

**Complete:**

Required syntax is in **bold**.

**| dfsearch**  
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
[verify=\<bool>]  
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
[search_filter=\<string>]  
[convert_table_array=\<bool>]
[timeout=\<int>]

## Required arguments

**url**

- **Syntax:** url="_\<url>_"
- **Description:** The URL for the API that you are reaching out to. Supports HTTP and HTTPS. If no protocol is specified this will default to HTTPS. You can include the full URL path here, or leverage the **url** and **endpoint** parameters together to specify the full path. 
  - Examples:
    - <https://api.dataflect.com/v2/api>
    - api.dataflect.com/v2/api
    - api.dataflect.com

## Optional arguments

**endpoint**

- **Syntax:** endpoint="\<string>"
- **Description:** The API endpoint being targeted, will be appended to the **url** field. Can begin with or without a "/"

**parameters**

- **Syntax:** parameters="\<string>"
- **Description:**The parameters to pass with the API call. Use the name of the variable and the corresponding value, separated by an =. If multiple parameters apply, separate with "&".
  - Examples:
    - foo=bar&something=this is a string&id=23
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

Enter

**headers**

- **Syntax:** headers="\<string>"
- **Description:** The "Header" of the request sent to the API. This must be a JSON-like object, using single quotes in place of double quotes.
  - e.g. headers="{'foo': 'bar'}"

**verify**

- **Syntax:** verify="\<boolean>"
- **Description:** Whether or not to verify the server certificate with the request. **NOTE:** If Dataflect settings have set "Force HTTPS Verify" to true this setting will be ignored.

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

**text_line_headers**

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

**search_filter**

- **Syntax:** search_filter="\<string>"
- **Description:** Provides the ability to filter out results from the API response before returning them as events. Accepts a comma separated list of criteria with the following comparisons: =, >, >=, \<, \<=. Filters are combined using AND logic. **NOTE**: This filtering occurs after normalization from the **props** setting. This means that if you change a field name with **props** you should specify the new name in this list.
  - e.g. price>1,currency=USD
    - Evaluated as price greater than 1 AND currency equals USD.


**convert_table_array**

**timeout**


# Dataflect Search Query Builder Overview

Dataflect Search ships with a **Query Builder** view that makes it easy for users to interact with the **dfsearch** command. Users can access the **Query Builder** dashboard by navigating to the Dataflect Search Application and selecting **Query Builder**.  The same options are available which are discussed in this documentation for the dfsearch command.

## Using the Query Builder to Create a Custom Search Command

Once a user has executed a search using the **Query Builder**, the option to "Create Custom Search Command" appears at the top of the page. Click the link to expand this option.

The expanded section includes the underlying query that has been executed. From here a user can enter an "Custom Search Command Name Name" in the text box under the search and click "Create" to create a custom search command that will execute the underlying search.

Users have the option to enter parameters within the search query using the format $token$. Tokens must be set when the custom search command is executed using the "parameters".

Update permissions as necessary by navigating to Dataflect Search --> Configure --> Commands

### Dataflect Search Logging Overview

Each of the Dataflect commands are logged to Splunk's \_internal index. These logs can be found by searching:

```Text SPL
index=_internal sourcetype=dataflect:log
```

Dataflect logs the user executing each command, as well as information regarding which APIs they are communicating with, the number of calls they are making, and the volume of data that is being sent out of Splunk (egress) and returned back to Splunk (ingress).

## Dataflect Search Monitoring Dashboard

To simplify monitoring, Dataflect provides a **Monitoring** Dashboard which provides some key metrics. To access the **Monitoring** Dashboard, navigate to the Dataflect Application, and select **Monitoring** from the navigation menu.

From within the dashboard you can filter based on time, command and/or the domain being communicated with.

The following modifiers can be used with any Dataflect custom search command:

# Dataflect Search Modifiers

- Enter current UTC timestamp:
  - insert $STRF:<strf time format>:STRF$
    - <https://strftime.org/>
  - | dfsearch url=<https://example.dataflect.com> credential=dataflect_example headers="{'User-Agent': 'dataflect-demo', 'Accept': '_/\_', 'x-ms-version': '2020-04-08', 'x-ms-date': '$STRF:%a, %d %b %Y %H:%M:%S GMT:STRF$', 'accept-encoding': 'gzip, deflate'}"

# Dataflect Search Known Issues

- When executing a dfsearch command, you cannot pass the results directly into the "anomalydetection" or "outlier" commands. You must first pass the results through the "stats" command. Example:
  - This will not work:
    - | dfsearch url="<https://api.coincap.io/v2/assets">
      | anomalydetection changePercent24Hr
  - This will work:
    - | dfsearch url="<https://api.coincap.io/v2/assets"> 
      | stats count by symbol changePercent24Hr  
      | anomalydetection changePercent24Hr
