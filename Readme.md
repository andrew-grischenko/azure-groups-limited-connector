
## Azure Groups Limited Connector
This connector is built on top of [Microsoft Azure Active Directory Connector](https://github.com/microsoft/PowerPlatformConnectors/tree/master/certified-connectors/AzureAD), but limits it's scope and operations to only those with groups and members of groups (see below). The Grap API scopes required for this connector are:
* Group.ReadWrite.All
* GroupMember.ReadWrite.All    
* offline_access 

This connector exposes operations to be used in Microsoft Flow and PowerApps.

## Supported Operations
The connector supports the following operations:
* `Create Security group`: Create a security group in your AAD tenant
* `Find group`: Find and get group details by name 
* `Get group`: Get details for a group by id
* `Get group members`: Get the users who are members of a group
* `Remove Member From Group`: Remove Member From Group
* `Add user to group`: Add a user to a group in this AAD tenant

## Pre-requisites
You will need the following to proceed:
* A Microsoft PowerApps or Microsoft Flow plan with custom connector feature
* An Azure subscription
* The [Microsoft Power Platform Connectors CLI](https://learn.microsoft.com/en-us/connectors/custom-connectors/paconn-cli)

## Building the connector 
Since the APIs used by the connector are secured by Azure Active Directory (AD), we first need to set up a few thing in Azure AD for connector to securely access  them.  After this setup, you can create and test the connector.

### Set up an Azure AD application for your custom connector
Since the connector uses OAuth as authentication type, we first need to register an application in Azure AD.  This application will be used to get the authorization token required to invoke rest APIs used by the connector.  You can read more about this [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/authentication-scenarios) and follow the steps below:

1. Create an Azure AD application
This can be done using [Azure Portal] (https://portal.azure.com), by following the steps [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app).  Once created, note down the value of Application (Client) ID. You will need this later.

2. Configure (Update) your Azure AD application to access the Azure Active Directory API
This step will ensure that your application can successfully retrieve an access token to invoke Azure Active Directory rest APIs on behalf of your users.  To do this, follow the steps [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-configure-app-access-web-apis).
    - For redirect URI, use "https://global.consent.azure-apim.net/redirect"
    - For the credentials, use a client secret (and not certificates).  Remember to note the secret down, you will need this later and it is shown only once.
    - For API permissions, use "Microsoft Graph" and "Application" type permissions "Group.ReadWrite.All" and "GroupMember.ReadWrite.All"
   
At this point, we now have a valid Azure AD application that can be used to get permissions as service principal and access Microsoft Graph API.  The next step for us is to create a custom connector.

### Deploying the connector via CLI
Run the following commands and follow the prompts:

```paconn
paconn login

paconn create --api-def apiDefinition.swagger.json --api-prop apiProperties.json --secret <client_secret>
```

## Helpful documentation used
* [Microsoft Graph API explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)
* [Build a custom connector for Microsoft Graph API](https://powerusers.microsoft.com/t5/Power-Automate-Community-Blog/Build-a-custom-connector-for-Microsoft-Graph-API/ba-p/647492)
* [Microsoft Graph API documentation - Group resource](https://learn.microsoft.com/en-us/graph/api/resources/group?view=graph-rest-1.0)
* [Service principal authentication](https://powerapps.microsoft.com/en-us/blog/public-preview-of-new-custom-connector-enhancements/)
* [Streamlining Integration: Using Service Principal authentication on Custom connectors with Microsoft Graph Application Permissions](https://ashiqf.com/2023/10/28/streamlining-integration-using-service-principal-authentication-on-custom-connectors-with-microsoft-graph-application-permissions/)
* [Microsoft Power Platform Connectors CLI](https://learn.microsoft.com/en-us/connectors/custom-connectors/paconn-cli)
* [Microsoft Azure Active Directory Connector on GitHub](https://github.com/microsoft/PowerPlatformConnectors/tree/master/certified-connectors/AzureAD)
