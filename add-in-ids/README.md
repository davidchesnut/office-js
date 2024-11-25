# Find Outlook add-ins that use legacy Exchange Online tokens

Legacy Exchange Online tokens are deprecated and will begin being turned off across Microsoft 365 tenants in February 2025. If you are a Microsoft 365 administrator, you should check if any Outlook add-ins deployed in your tenant are using legacy Exchange Online tokens. If they are, we recommend you reach out to publishers and developers of those add-ins to ensure they are migrating their add-ins away from legacy Exchange Online tokens. Add-ins that continue to use legacy tokens will eventually break when the tokens are turned off beginning February 2025.

For more information about deprecation of legacy Exchange Online tokens, see [Nested app authentication and Outlook legacy tokens deprecation FAQ](https://aka.ms/NAAFAQ).

The following procedures in this README show how to find Outlook add-ins deployed in your tenant that are potentially using legacy Exchange Online tokens.

## List of Outlook add-ins deployed from the Microsoft store

This folder contains the **add-ins-using-exchange-tokens.xlsx** spreadsheet. The spreadsheet contains a list of all Outlook add-ins published in the Microsoft store that were using legacy Exchange Online tokens as of October 2024. You can use the spreadsheet to look up any add-in by its **AppId**.

## Connect to Exchange Online PowerShell

Use the following steps to connect to Exchange Online PowerShell. For more information about connecting to Exchange Online, see [Connect to Exchange Online PowerShell](https://learn.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell)

1. Open PowerShell.
1. Run the command `Import-Module ExchangeOnlineManagement`. For more information about this command, see [O365CentralizedAddInDeployment 3.0.2](https://www.powershellgallery.com/packages/O365CentralizedAddInDeployment/3.0.2).
1. Run the command `Connect-ExchangeOnline`. Sign in with your Microsoft 365 administrator credentials.

## Get a list of all add-ins deployed at the organization level

Admins can deploy add-ins for the entire organization from the Microsoft store or through central deployment. Run the following command in PowerShell to get a list of all add-ins deployed at the organization level in your tenant.

- `Get-App -OrganizationApp | ft DisplayName, Enabled, AppVersion, AppId, ProviderName`

The command lists all add-ins you deployed on your tenant. Use the **AppId** column to search the `add-ins-using-exchange-tokens.xlsx` spreadsheet. If the add-in is in the spreadsheet then it uses legacy Exchange Online tokens (as of October 2024). We recommend you contact the publisher as soon as possible to confirm they have a plan and a timeline for moving off of legacy Exchange Online tokens.

**Note:** If the **ProviderName** is Microsoft, those add-ins are already being migrated and will be updated by February 2025.

## Get a list of all add-ins deployed by users

Users can also deploy add-ins from the Microsoft store, or sideload add-ins. Run the following command in PowerShell to get a list of all add-ins deployed by users in your tenant. The command loops through all user mailboxes so it may take some time to complete.

- `Get-Mailbox -ResultSize Unlimited | ForEach-Object {Get-App -Mailbox $_.identity | Select-Object -Property DisplayName, Enabled, AppVersion, AppId, MarketplaceAssetID} | Sort-Object -Property DisplayName | Get-Unique -AsString | Export-Csv add-in-list-export.csv -NoTypeInformation -Append`

The command exports a `add-in-list-export.csv` file with a list of all add-ins found for all users. For more information about this command including how to build reports around add-ins in your organization, see [Report Office Store Web Add-ins (apps) usage in your organization](https://www.howto-outlook.com/howto/report-office-store-app-usage.htm)

Open the `add-in-list-export.csv` file in Excel. If the list doesn't open with the correct columns, go to the **Data** tab, in the **Get & Transform Data group**, and choose **From Text/CSV**.

The **MarketplaceAssetID** column will list an asset ID if the add-in was deployed from the Microsoft store. Use the **AppId** for add-ins from the Microsoft store to search the **add-ins-using-exchange-tokens.xlsx** spreadsheet. If the add-in is in the spreadsheet then it uses legacy Exchange Online tokens (as of October 2024). We recommend you contact the publisher as soon as possible to confirm they have a plan and a timeline for moving off of legacy Exchange Online tokens.

Add-ins that don't have a **MarketplaceAssetID** were deployed by the user from the organization or by sideloading. These are not listed in the spreadsheet and are either line-of-business add-ins or 3rd party add-ins. We recommend you reach out to the developers of these add-ins as soon as possible to confirm they have a plan and a timeline for moving off of legacy Exchange Online tokens.

## More resources

For more information about deprecation of legacy Exchange Online tokens, and administrator questions, see [Nested app authentication and Outlook legacy tokens deprecation FAQ](https://aka.ms/NAAFAQ).
