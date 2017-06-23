---
title: Sharing Azure Remote Desktop Certificates for Deployment
tags:
  - AngularJS
  - asp.net
  - asp.net mvc
  - Azure
  - 'C#'
  - cmd
  - css
  - IIS
  - javascript
  - JSON
  - SQL
  - SSAS
  - Testing
  - URL Rewrite
  - web api
  - Windows 7
  - Windows Phone
  - XML
  - Windows Server
  - NTP
---


$cert = New-SelfSignedCertificate -Type Custom -KeySpec KeyExchange -DnsName clickpointsoftwaredeployment.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 4096 -NotAfter (Get-Date).AddYears(10)
$password = ConvertTo-SecureString -String "YourCertificatePasswordGoesHere" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\clickpointSharedDeploymentAndRDPCert.pfx" -Password $password

Install the Cert into your personal store with default options

from http://www.runeibsen.dk/?p=517

ls cert:/CurrentUser/My


[Reflection.Assembly]::LoadWithPartialName("System.Security")
$pass = [Text.Encoding]::UTF8.GetBytes("YourRDPUsersPasswordGoesHere")
$content = new-object Security.Cryptography.Pkcs.ContentInfo –argumentList (,$pass)
$env = new-object Security.Cryptography.Pkcs.EnvelopedCms $content
$env.Encrypt((new-object System.Security.Cryptography.Pkcs.CmsRecipient(gi cert:\CurrentUser\My\SHA1HashFromLSCommandAbove)))
cls
[Convert]::ToBase64String($env.Encode())

<Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="ValueFromCommandAbove" />

<Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="SHA1HashFromLSCommandAbove" thumbprintAlgorithm="sha1" />