
## <a name="start-your-powershell-session"></a>Starta din PowerShell-session
Du bör först ha hello senaste Azure PowerShell installerade och körs. Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Många nya funktioner i SQL-databas stöds endast när du använder hello [Azure Resource Manager-distributionsmodellen](../articles/azure-resource-manager/resource-group-overview.md), så exemplen använder hello [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) för Resource Manager . distributionsmodell för hello service management (klassisk) [Azure SQL Database Service Management-cmdlets](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) stöds för bakåtkompatibilitet, men vi rekommenderar att du använder hello Resource Manager-cmdletar.
> 
> 

Kör hello [ **Add-AzureRmAccount** ](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) cmdlet, och du kommer att visas en inloggningssida tooenter dina autentiseringsuppgifter. Använd hello samma autentiseringsuppgifter som du använder toosign i toohello Azure-portalen.

```PowerShell
Add-AzureRmAccount
```

Om du har flera prenumerationer använder hello [ **Set-AzureRmContext** ](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) cmdlet tooselect vilken prenumeration som din PowerShell-session ska använda. toosee vilken prenumeration hello aktuella PowerShell sessionen använder kör [ **Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx). toosee dina prenumerationer kör [ **Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx).

```PowerShell
Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'
```
