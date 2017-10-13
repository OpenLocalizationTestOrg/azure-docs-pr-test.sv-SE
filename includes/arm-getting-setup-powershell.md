## <a name="setting-up-powershell-for-resource-manager-templates"></a>Konfigurera PowerShell för Resource Manager-mallar
Innan du kan använda Azure PowerShell med Resource Manager, behöver du ha behörigheten Windows PowerShell och Azure PowerShell-versioner.

### <a name="verify-powershell-versions"></a>Kontrollera att PowerShell-versioner
Kontrollera att du har Windows PowerShell version 3.0 eller 4.0. Skriv följande kommando i Kommandotolken för Windows PowerShell för att hitta versionen av Windows PowerShell.

    $PSVersionTable

Du får följande typ av information:

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2


Kontrollera att värdet för **PSVersion** 3.0 eller 4.0. Om inte, se [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) eller [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

### <a name="set-your-azure-account-and-subscription"></a>Konfigurera Azure-kontot och Azure-prenumerationen
Om du inte redan har en Azure-prenumeration, kan du aktivera din [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).

Öppna ett Azure PowerShell-Kommandotolken och logga in på Azure med det här kommandot.

    Login-AzureRmAccount

Om du har flera Azure-prenumerationer, kan du visa din Azure-prenumerationer med det här kommandot.

    Get-AzureRmSubscription

Du får följande typ av information:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName :
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Du kan ange den aktuella Azure-prenumerationen genom att köra dessa kommandon i Kommandotolken Azure PowerShell. Ersätt allt inom citattecken, inklusive den < och > tecken, med rätt namn.

    $subscr="<SubscriptionName from the display of Get-AzureRmSubscription>"
    Select-AzureRmSubscription -SubscriptionName $subscr -Current

Läs mer om Azure-prenumerationer och konton [så här: ansluta till din prenumeration](/powershell/azureps-cmdlets-docs#step-3-connect).

