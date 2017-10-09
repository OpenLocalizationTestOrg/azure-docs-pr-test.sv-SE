Innan du påbörjar den här konfigurationen måste du logga in tooyour Azure-konto. hello cmdlet efterfrågar hello inloggningsuppgifterna för ditt Azure-konto. När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell. Mer information finns i [Använda Windows PowerShell med Resource Manager](../articles/powershell-azure-resource-manager.md).

toolog, öppna PowerShell-konsol med utökade privilegier och Anslut tooyour konto. Använd hello följande exempel toohelp du ansluta:

```powershell
Login-AzureRmAccount
```

Om du har flera Azure-prenumerationer, kontrollera hello prenumerationer för hello-kontot.

```powershell
Get-AzureRmSubscription
```

Ange hello prenumeration som du vill toouse.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
 ```