## <a name="set-up-azure-powershell-for-azure-dns"></a>Konfigurera Azure PowerShell för Azure DNS

### <a name="before-you-begin"></a>Innan du börjar

Kontrollera att du har hello följande objekt innan du börjar din konfiguration.

* En Azure-prenumeration. Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).
* Du måste tooinstall hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="sign-in-tooyour-azure-account"></a>Logga in tooyour Azure-konto

Öppna PowerShell-konsolen och Anslut tooyour konto. Mer information finns i [med hjälp av PowerShell med Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).

```powershell
Login-AzureRmAccount
```

### <a name="select-hello-subscription"></a>Välj hello-prenumeration
 
Kontrollera hello prenumerationer för hello-kontot.

```powershell
Get-AzureRmSubscription
```

Välj vilka av dina Azure-prenumerationer toouse.

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Azure Resource Manager kräver att alla resursgrupper anger en plats. Den här platsen används som hello standardplatsen för resurser i resursgruppen. Men eftersom alla DNS-resurser är globala, inte regional, har hello valet av resursgruppens plats ingen inverkan på Azure DNS.

Du kan hoppa över det här steget om du använder en befintlig resursgrupp.

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a>Registrera resursprovider

hello Azure DNS-tjänst som hanteras av hello Microsoft.Network-resursprovidern. Din Azure-prenumeration måste vara registrerade toouse den här resursprovidern innan du kan använda Azure DNS. Det här är en engångsåtgärd för varje prenumeration.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```