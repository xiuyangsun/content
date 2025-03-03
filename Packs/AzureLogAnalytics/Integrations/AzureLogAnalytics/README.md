Log Analytics is a service that helps you collect and analyze data generated by resources in your cloud and on-premises environments.
This integration was integrated and tested with version 2021-06-01 of Azure Log Analytics.

## Authorize Cortex XSOAR for Azure Log Analytics

You need to grant Cortex XSOAR authorization to access Azure Log Analytics.

**Note**: The Azure account must have permission to manage applications in Azure Active Directory (Azure AD). Any of the following Azure AD roles include the required permissions:

- Application administrator
- Application developer
- Cloud application administrator

In addition, the user needs to be assigned the **Log Analytics Reader** role.
To add the role, see the [Microsoft article](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps#assign-users-and-groups-to-roles).

1. Access the [authorization flow](https://oproxy.demisto.ninja/ms-azure-log-analytics). 
2. Click **Start Authorization Process**.
 You will be prompted to grant Cortex XSOAR permissions for your Azure Service Management. 
3. Click **Accept**.
You will receive your ID, token, and key. You need to enter these when you configure the Azure Log Analytics integration instance in Cortex XSOAR.

## Authorize Cortex XSOAR for Azure Log Analytics (Self-Deployed Configuration)
To use a self-configured Azure application, you need to add a new Azure App Registration in the Azure Portal. To add the registration, see the [Microsoft article](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app).

### Required permissions
- Azure Service Management - permission `user_impersonation` of type `Delegated`
- Log Analytics API - permission `Data.Read` of type `Delegated`

In the self-deployed mode you can authenticate, by using one of the following flows:

- Authentication code flow
- Client Credentials flow

### Authentication Code flow

---
1. Enter your client ID in the **ID\Client ID** parameter (credentials username). 
2. Enter your client secret in the **password** parameter (credentials password).
3. Enter your tenant ID in the **Token** parameter.
4. Enter your redirect URI in the **Redirect URI** parameter.
5. Save the integration settings.
6. Run the `!azure-log-analytics-generate-login-url` command in the War Room and follow the instruction.
### Authorize Cortex XSOAR for Azure Log Analytics (Client-Credentials Configuration)

---
Follow these steps for client-credentials configuration.

1. In the instance configuration, select the **client-credentials** checkbox.
2. Enter your Client ID in the **ID/Client ID** parameter (credentials username). 
3. Enter your Client Secret in the **password** parameter (credentials password).
4. Enter your Tenant ID in the **Tenant ID** parameter.
5. Run the ***azure-log-analytics-test*** command to test the connection and the authorization process.
9. Enter your redirect URI in the **Redirect URI** parameter.

## Get the additional instance parameters

To get the **Subscription ID**, **Workspace Name**, **Workspace ID** and **Resource Group** parameters, navigate in the Azure Portal to **Azure Sentinel > YOUR-WORKSPACE > Settings** and click the **Workspace Settings** tab.

## Configure Azure Log Analytics on Cortex XSOAR

1. Navigate to **Settings** > **Integrations** > **Servers & Services**.
2. Search for Azure Log Analytics.
3. Click **Add instance** to create and configure a new integration instance.

| **Parameter** | **Description** | **Required** |
| --- | --- | --- |
| credentials - identifier | ID\Client ID \(received from the authorization step or from the self-deployed configuration process - see Detailed Instructions \(?\) section\) | True |
| refresh_token | Token\tenant_id \(received from the authorization step or from the self-deployed configuration process - see Detailed Instructions \(?\) section\) | True |
| credentials - password | Key\Client secret \(received from the authorization step - see Detailed Instructions \(?\) section\) | False |
| Certificate Thumbprint | Used for certificate authentication. As appears in the "Certificates & secrets" page of the app. | False |
| Private Key | Used for certificate authentication. The private key of the registered certificate. | False |
| Use Azure Managed Identities | Relevant only if the integration is running on Azure VM. If selected, authenticates based on the value provided for the Azure Managed Identities Client ID field. If no value is provided for the Azure Managed Identities Client ID field, authenticates based on the System Assigned Managed Identity. For additional information, see the Help tab. | False |
| Azure Managed Identities Client ID | The Managed Identities client ID for authentication - relevant only if the integration is running on Azure VM. | False |
| self_deployed | Use a self-deployed Azure application. | False |
| Use Client Credentials Authorization Flow | Use a self-deployed Azure application and authenticate using the Client Credentials flow. | False |
| redirect_uri | Application redirect URI \(for self-deployed mode\) | False |
| auth_code | Authorization code \(received from the authorization step - see Detailed Instructions \(?\) section\) | False |
| subscriptionID | Subscription ID | True |
| resourceGroupName | Resource Group Name | True |
| workspaceName | Workspace Name | True |
| workspaceID | Workspace ID \(the UUID of the workspace, e.g., `123e4567-e89b-12d3-a456-426614174000`\) | True |
| insecure | Trust any certificate \(not secure\) | False |
| proxy | Use system proxy settings | False |

1. Click **Test** to validate the URLs, token, and connection.
## Commands
You can execute these commands from the Cortex XSOAR CLI, as part of an automation, or in a playbook.
After you successfully execute a command, a DBot message appears in the War Room with the command details.
### azure-log-analytics-execute-query
***
Executes an Analytics query for data.


#### Base Command

`azure-log-analytics-execute-query`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| query | The query to execute. | Required | 
| timespan | The timespan over which to query data. This is an ISO8601 time period value. This timespan is applied in addition to any timespans specified in the query expression. | Optional | 
| timeout | The amount of time (in seconds) that a request will wait for the query response before a timeout occurs. | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| AzureLogAnalytics.Query.Query | String | The executed query. | 
| AzureLogAnalytics.Query.TableName | String | The name of the query table. | 


#### Command Example
```!azure-log-analytics-execute-query query="Usage | take 10" workspace_id=WORKSPACE_ID```

#### Human Readable Output
## Query Results
### PrimaryResult
|Tenant Id|Computer|Time Generated|Source System|Start Time|End Time|Resource Uri|Data Type|Solution|Batches Within Sla|Batches Outside Sla|Batches Capped|Total Batches|Avg Latency In Seconds|Quantity|Quantity Unit|Is Billable|Meter Id|Linked Meter Id|Type|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TENANT_ID | Deprecated field: see http://aka.ms/LA-Usage | 2020-07-30T04:00:00Z | OMS | 2020-07-30T03:00:00Z | 2020-07-30T04:00:00Z | /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP/providers/microsoft.operationalinsights/workspaces/WORKSPACE_NAME | Operation | LogManagement | 0 | 0 | 0 | 0 | 0 | 0.00714 | MBytes | false | METER_ID | 00000000-0000-0000-0000-000000000000 | Usage |
| TENANT_ID | Deprecated field: see http://aka.ms/LA-Usage | 2020-07-30T04:00:00Z | OMS | 2020-07-30T03:00:00Z | 2020-07-30T04:00:00Z | /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP/providers/microsoft.operationalinsights/workspaces/WORKSPACE_NAME | SigninLogs | LogManagement | 0 | 0 | 0 | 0 | 0 | 0.012602 | MBytes | true | METER_ID | 00000000-0000-0000-0000-000000000000 | Usage |
| TENANT_ID | Deprecated field: see http://aka.ms/LA-Usage | 2020-07-30T05:00:00Z | OMS | 2020-07-30T04:00:00Z | 2020-07-30T05:00:00Z | /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP/providers/microsoft.operationalinsights/workspaces/WORKSPACE_NAME | OfficeActivity | Office365/SecurityInsights | 0 | 0 | 0 | 0 | 0 | 0.00201499908978072 | MBytes | false | METER_ID | 00000000-0000-0000-0000-000000000000 | Usage |
| TENANT_ID | Deprecated field: see http://aka.ms/LA-Usage | 2020-07-30T05:00:00Z | OMS | 2020-07-30T04:00:00Z | 2020-07-30T05:00:00Z | /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP/providers/microsoft.operationalinsights/workspaces/WORKSPACE_NAME | SigninLogs | LogManagement | 0 | 0 | 0 | 0 | 0 | 0.009107 | MBytes | true | METER_ID | 00000000-0000-0000-0000-000000000000 | Usage |


### azure-log-analytics-list-saved-searches
***
Gets the saved searches of the Log Analytics workspace.


#### Base Command

`azure-log-analytics-list-saved-searches`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| limit | The maximum number of saved searches to return. Default is 50. | Optional | 
| page | The page number from which to start a search. | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| AzureLogAnalytics.SavedSearch.id | String | The ID of the saved search. | 
| AzureLogAnalytics.SavedSearch.etag | String | The ETag of the saved search. | 
| AzureLogAnalytics.SavedSearch.category | String | The category of the saved search. This helps users quickly find a saved search. | 
| AzureLogAnalytics.SavedSearch.displayName | String | Display name of the saved search. | 
| AzureLogAnalytics.SavedSearch.functionAlias | String | The function alias if the query serves as a function. | 
| AzureLogAnalytics.SavedSearch.functionParameters | String | The optional function parameters if the query serves as a function. Value should be in the following format: 'param-name1:type1 = default_value1, param-name2:type2 = default_value2'. For more examples and proper syntax please refer to https://docs.microsoft.com/en-us/azure/kusto/query/functions/user-defined-functions | 
| AzureLogAnalytics.SavedSearch.query | String | The query expression for the saved search. | 
| AzureLogAnalytics.SavedSearch.tags | String | The tags attached to the saved search. | 
| AzureLogAnalytics.SavedSearch.version | Number | The version number of the query language. The current version and default is 2. | 
| AzureLogAnalytics.SavedSearch.type | String | The resource type, e.g., Microsoft.Compute/virtualMachines or Microsoft.Storage/storageAccounts. | 


#### Command Example
```!azure-log-analytics-list-saved-searches limit=3```

#### Human Readable Output
### Saved searches
|Etag|Id|Category|Display Name|Function Alias|Function Parameters|Query|Tags|Version|Type|
|---|---|---|---|---|---|---|---|---|---|
| W/"datetime'2020-07-05T13%3A38%3A41.053438Z'" | test2 | category1 | test2 | heartbeat_func | a:int=1 | Heartbeat \| summarize Count() by Computer \| take a | {'name': 'Group', 'value': 'Computer'} | 2 | Microsoft.OperationalInsights/savedSearches |
| W/"datetime'2020-07-28T18%3A43%3A56.8625448Z'" | test123 | Saved Search Test Category | test123 | heartbeat_func | a:int=1 | Heartbeat \| summarize Count() by Computer \| take a | {'name': 'Group', 'value': 'Computer'} | 2 | Microsoft.OperationalInsights/savedSearches |
| W/"datetime'2020-07-30T11%3A41%3A35.1459664Z'" | test1234 | test | test |  |  | 	SecurityAlert<br/>\| summarize arg_max(TimeGenerated, *) by SystemAlertId<br/>\| where SystemAlertId in("TEST_SYSTEM_ALERT_ID") |  | 2 | Microsoft.OperationalInsights/savedSearches |


### azure-log-analytics-get-saved-search-by-id
***
Gets the specified saved search from the Log Analytics workspace.


#### Base Command

`azure-log-analytics-get-saved-search-by-id`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| saved_search_id | The ID of the saved search. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| AzureLogAnalytics.SavedSearch.id | String | The ID of the saved search. | 
| AzureLogAnalytics.SavedSearch.etag | String | The ETag of the saved search. | 
| AzureLogAnalytics.SavedSearch.category | String | The category of the saved search. This helps users quickly find a saved search. | 
| AzureLogAnalytics.SavedSearch.displayName | String | The display name of the saved search. | 
| AzureLogAnalytics.SavedSearch.functionAlias | String | The function alias if the query serves as a function. | 
| AzureLogAnalytics.SavedSearch.functionParameters | String | The optional function parameters if the query serves as a function. Value should be in the following format: 'param-name1:type1 = default_value1, param-name2:type2 = default_value2'. For more examples and proper syntax see the Microsoft documention, https://docs.microsoft.com/en-us/azure/kusto/query/functions/user-defined-functions | 
| AzureLogAnalytics.SavedSearch.query | String | The query expression for the saved search. | 
| AzureLogAnalytics.SavedSearch.tags | String | The tags attached to the saved search. | 
| AzureLogAnalytics.SavedSearch.version | Number | The version number of the query language. The current version and default is 2. | 
| AzureLogAnalytics.SavedSearch.type | String | The resource type, e.g., Microsoft.Compute/virtualMachines or Microsoft.Storage/storageAccounts. | 


#### Command Example
```!azure-log-analytics-get-saved-search-by-id saved_search_id=test1234```

#### Human Readable Output
### Saved search `test1234` properties
|Etag|Id|Category|Display Name|Query|Version|
|---|---|---|---|---|---|
| W/"datetime'2020-07-30T12%3A21%3A05.3197505Z'" | test1234 | test | test | SecurityAlert &#124; summarize arg_max(TimeGenerated, *) by SystemAlertId &#124; where SystemAlertId in("TEST_SYSTEM_ALERT_ID") | 2 |


### azure-log-analytics-create-or-update-saved-search
***
Creates or updates a saved search from the Log Analytics workspace.


#### Base Command

`azure-log-analytics-create-or-update-saved-search`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| saved_search_id | The ID of the saved search. | Required | 
| etag | The ETag of the saved search. This argument is required for updating an existing saved search. | Optional | 
| category | The category of the saved search. This helps users quickly find a saved search. | Required | 
| display_name | The display name of the saved search. | Required | 
| function_alias | The function alias if the query serves as a function. | Optional | 
| function_parameters | The optional function parameters if the query serves as a function. Value should be in the following format: 'param-name1:type1 = default_value1, param-name2:type2 = default_value2'. For more examples and proper syntax please refer to https://docs.microsoft.com/en-us/azure/kusto/query/functions/user-defined-functions. | Optional | 
| query | The query expression for the saved search. | Required | 
| tags | The tags attached to the saved search. Value should be in the following format: 'name=value;name=value' | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| AzureLogAnalytics.SavedSearch.id | String | The ID of the saved search. | 
| AzureLogAnalytics.SavedSearch.etag | String | The ETag of the saved search. | 
| AzureLogAnalytics.SavedSearch.category | String | The category of the saved search. This helps users quickly find a saved search. | 
| AzureLogAnalytics.SavedSearch.displayName | String | The display name of the saved search. | 
| AzureLogAnalytics.SavedSearch.functionAlias | String | The function alias if the query serves as a function. | 
| AzureLogAnalytics.SavedSearch.functionParameters | String | The optional function parameters if the query serves as a function. Value should be in the following format: 'param-name1:type1 = default_value1, param-name2:type2 = default_value2'. For more examples and proper syntax please refer to https://docs.microsoft.com/en-us/azure/kusto/query/functions/user-defined-functions | 
| AzureLogAnalytics.SavedSearch.query | String | The query expression for the saved search. | 
| AzureLogAnalytics.SavedSearch.tags | String | The tags attached to the saved search. | 
| AzureLogAnalytics.SavedSearch.version | Number | The version number of the query language. The current version and default is 2. | 
| AzureLogAnalytics.SavedSearch.type | String | The resource type, e.g., Microsoft.Compute/virtualMachines or Microsoft.Storage/storageAccounts. | 


#### Command Example
```
!azure-log-analytics-create-or-update-saved-search saved_search_id="test1234" category="test" display_name="new display name test" query=`SecurityAlert
| summarize arg_max(TimeGenerated, *) by SystemAlertId
| where SystemAlertId in("TEST_SYSTEM_ALERT_ID")
```

#### Human Readable Output
### Saved search `test1234` properties
|Etag|Id|Category|Display Name|Query|Version|
|---|---|---|---|---|---|
| W/"datetime'2020-07-30T12%3A21%3A05.3197505Z'" | test1234 | test | new display name test | SecurityAlert &#124; summarize arg_max(TimeGenerated, *) by SystemAlertId &#124; where SystemAlertId in("TEST_SYSTEM_ALERT_ID") | 2 |


### azure-log-analytics-delete-saved-search
***
Deletes a specified saved search in the Log Analytics workspace.


#### Base Command

`azure-log-analytics-delete-saved-search`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| saved_search_id | The ID of the saved search. | Required | 


#### Context Output

There is no context output for this command.

#### Command Example
```!azure-log-analytics-delete-saved-search saved_search_id=test1234```

#### Human Readable Output
Successfully deleted the saved search test1234.


### azure-log-analytics-test
***
Tests connectivity to Azure Log Analytics.

#### Base Command

`azure-log-analytics-test`
#### Input

There are no input arguments for this command.

#### Context Output

There is no context output for this command.

#### Command Example
```!azure-log-analytics-test```

#### Human Readable Output
```✅ Success!```


### azure-log-analytics-generate-login-url
***
Generate the login url used for Authorization code flow.

#### Base Command

`azure-log-analytics-generate-login-url`
#### Input

There are no input arguments for this command.

#### Context Output

There is no context output for this command.

#### Command Example
```azure-log-analytics-generate-login-url```

#### Human Readable Output

>### Authorization instructions
>1. Click on the [login URL]() to sign in and grant Cortex XSOAR permissions for your Azure Service Management.
You will be automatically redirected to a link with the following structure:
```REDIRECT_URI?code=AUTH_CODE&session_state=SESSION_STATE```
>2. Copy the `AUTH_CODE` (without the `“code=”` prefix, and the `session_state` parameter)
and paste it in your instance configuration under the **Authorization code** parameter.


