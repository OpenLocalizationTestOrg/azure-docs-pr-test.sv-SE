
## <a name="start-your-powershell-session"></a>Starta din PowerShell-session
Du måste först toohave hello senaste [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) installerad och körs. Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Hej exemplen i det här avsnittet använder [Azure Resource Manager-distributionsmodellen](../articles/azure-resource-manager/resource-group-overview.md), så exemplen använder hello [Azure Resource Manager cmdlets](http://msdn.microsoft.com/library/azure/mt125356.aspx). 
> 
> 

Kör hello [ **Add-AzureRmAccount** ](http://msdn.microsoft.com/library/mt619267.aspx) cmdlet och du kommer att visas med en inloggning skärmen tooenter dina autentiseringsuppgifter. Använd hello samma autentiseringsuppgifter som du använder toosign i toohello Azure-portalen.

    Add-AzureRmAccount

Om du har flera prenumerationer använder hello [ **Set-AzureRmContext** ](http://msdn.microsoft.com/library/mt619263.aspx) cmdlet tooselect vilken prenumeration som din PowerShell-session ska använda. toosee vilken prenumeration hello aktuella PowerShell sessionen använder kör [ **Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx). toosee dina prenumerationer kör [ **Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

