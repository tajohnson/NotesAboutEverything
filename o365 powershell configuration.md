# Office365 Configuration Powershell

o 365 o365



Download latest powershell https://github.com/PowerShell/PowerShell/releases/tag/v7.4.5
Or Windows Store

```
pwsh

 Install-Module ExchangeOnlineManagement

 Connect-ExchangeOnline -UserPrincipalName tj@mailroutecompliance.net -ExchangeEnvironmentName O365USGovGCCHigh


 New-InboundConnector -Name "MailRoute Inbound" -ConnectorType Partner -SenderDomains * -RestrictDomainsToCertificate $true -TlsSenderCertificateName *.mailroute.net -RequireTls $true -EFSkipIPS 199.89.0.0/21



 New-OutboundConnector -Name "MailRoute Outbound" -ConnectorType Partner -RecipientDomains * -SmartHosts outbound.mailroute.net -UseMXRecord $false -TlsSettings DomainValidation -TlsDomain mailroute.net


Get-InboundConnector -Identity "MailRoute Inbound" | Format-List

Get-OutboundConnector -Identity "MailRoute Outbound" | Format-List

Enable-OrganizationCustomization

Set-ArcConfig -Identity Default -ArcTrustedSealers "mailroute.net"



```