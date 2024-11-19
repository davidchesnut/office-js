# Add-ins using legacy Exchange online tokens

This folder contains the **add-ins-using-exchange-tokens.xlsx** spreadsheet. The spreadsheet contains a list of all Outlook add-ins published in the Microsoft store that were using legacy Exchange online tokens as of October 2024.

As a Microsoft 365 administrator you can use this spreadsheet to determine if any deployed Outlook add-ins in your tenant are potentially using legacy Exchange online tokens.

## Get a list of all add-ins

Use the following steps in PowerShell To get a list of all deployed add-ins on your tenant.

1. Open PowerShell.
1. Run the command `Install-Module -Name O365CentralizedAddInDeployment`. For more information about this command, see [O365CentralizedAddInDeployment 3.0.2](https://www.powershellgallery.com/packages/O365CentralizedAddInDeployment/3.0.2).
1. Run the command `Import-Module -Name O365CentralizedAddInDeployment`.
1. Run the command `Connect-OrganizationAddInService`. Sign in with your admin credentials.
1. Run the command `Get-OrganizationAddIn`.

This will list all add-ins deployed on your tenant. there will be a **DisplayName** column and **ProductId** column. You can look up any add-ins by using the **ProductId** and searching the spreadsheet. If the add-in is in the spreadsheet then it uses legacy Exchange online tokens (as of October 2024).

## What to do with add-ins using legacy tokens

If you have any deployed add-ins that are listed in the spreadsheet, we recommend that you reach out to those publishers to ensure they get migrated to use Entra ID tokens. Otherwise, when legacy Exchange tokens are eventually turned off, those add-ins will break.

If your tenant doesn’t have any add-ins using legacy Exchange tokens, we recommend you turn off Exchange tokens in your tenant as a security best practice.

## More resources

For more information about deprecation of legacy Exchange online tokens, and administrator questions, see [Nested app authentication and Outlook legacy tokens deprecation FAQ](https://aka.ms/NAAFAQ).
