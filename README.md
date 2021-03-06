# PowerAutomateUsers
A standalone Power Automate flow to extract all Power Automate users from a Power Platform tenant, using the standard admin connectors.
It extract all flows within a Power Platform environment and report the main attributes in an Excel Online workbook.

These are the connectors used by the flow:
- https://docs.microsoft.com/en-us/connectors/powerplatformforadmins/
- https://docs.microsoft.com/en-us/connectors/flowmanagement/
- https://docs.microsoft.com/en-us/connectors/microsoftflowforadmins/
- https://docs.microsoft.com/en-us/connectors/excelonlinebusiness/

Every flow is exported with its list of owners, co-owners, read-only users and delegated users through SharePoint lists.

## How to install
Follow these steps to import the flow in your environment:

### Prerequisites
1. An AAD account with Global Admin or Power Platform Admin role
2. A OneDrive For Business drive

### Set-up
1. Download the repository in your local drive and unzip it.
2. Using the admin account, upload the Book.xlsx file to the root folder of the admin's own OneDrive For Business. This will be the workbook used by the flow to generate the report.
3. Using the admin account, log into your preferred Power Platform environment at https://flow.microsoft.com/
4. Within the "My flows" section, click Import to import the GetAllFlowUsersinTenant_20220503165342.zip file containing the flow.
5. Create a connection using the admin account for each one of the four connection references required by the import.

   The following connections must be created manually:
   - Power Platform for Admins
   - Power Automate Management
   - Excel Online (Business)
   - Power Automate for Admins
7. Import.
8. Back to the "My flows" section, turn on the flow and run it. Depending on the number of flows and users in your tenant, it may take a while to complete.

# Output
The data collected by the flow are stored in the Book.xlsx workbook on OneDrive For Business in the following format:

Attribute | Cardinality | Description | Notes
--- | --- | --- | ---
**Environment ID** | 1 per flow | GUID of the Power Platform environment | na
**Environment Name** | 1 per flow | Display Name of the Power Platform environment | na
**Flow ID** | 1 per flow | GUID of the Power Automate flow | na
**Flow Name** | 1 per flow | Display Name of the Power Automate flow | na
**Flow Type** | 1 per flow | \[Instant, Automated, Scheduled\] | na
**Flow Tier** | 1 per flow | \[Standard, Premium\] | NB: AI Builder actions are marked as Premium, even though the Power Automate portal does not highlight it
**Is in Solution** | 1 per flow | \[Yes, No\] - Whether or not the flow is part of a solution (i.e. stored as a Dataverse record) | NB: Users of a solution-aware flow cannot be exported with the standard administrative connector. All remaining fields are marked as "Not visibile"
**Share type** | 1 or many per flow | \[Owner, CanEdit, CanView, Not visible\] | Owner is the creator of the flow. CanEdit are the co-owners. CanView are the read-only users. Not visibile if the flow is solution-aware
**Permission Type** | 1 or many per flow | \[Principal, AuthorizationDelegate, Not visibile\] | Principal if the flow has been shared with a user. AuthorizationDelegate if the flow has been shared through SharePoint lists. Not visibile if the flow is solution-aware
**Principal ID** | 1 or many per flow | GUID of the Principal to whom the flow has been shared | Starts with "DELEGATED-" if Permission Type = AuthorizationDelegate. Not visibile if the flow is solution-aware
**Delegation Service** | 1 or many per flow | \[SharePoint, "", Not visible\] | na
**Delegated Collection** | 1 or many per flow | URL of the SharePoint site that contain the list to which the flow has been shared | na
**Delegated Resource ID** | 1 or many per flow | GUID of the SharePoint list to whom the flow has been shared | na

# What's next
You can easily extend the flow with missing pieces of information. For example, you cound include the Office 365 Users connector to retrieve the identity of each Power Automate user starting from the Principal ID GUID.

https://docs.microsoft.com/en-us/connectors/office365users/

