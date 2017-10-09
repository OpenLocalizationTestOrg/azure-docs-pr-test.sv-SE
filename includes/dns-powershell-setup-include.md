## <a name="set-up-azure-powershell-for-azure-dns"></a><span data-ttu-id="c18fb-101">Konfigurera Azure PowerShell för Azure DNS</span><span class="sxs-lookup"><span data-stu-id="c18fb-101">Set up Azure PowerShell for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="c18fb-102">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c18fb-102">Before you begin</span></span>

<span data-ttu-id="c18fb-103">Kontrollera att du har hello följande objekt innan du börjar din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c18fb-103">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="c18fb-104">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c18fb-104">An Azure subscription.</span></span> <span data-ttu-id="c18fb-105">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c18fb-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="c18fb-106">Du måste tooinstall hello senaste versionen av hello Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c18fb-106">You need tooinstall hello latest version of hello Azure Resource Manager PowerShell cmdlets.</span></span> <span data-ttu-id="c18fb-107">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="c18fb-107">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="c18fb-108">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="c18fb-108">Sign in tooyour Azure account</span></span>

<span data-ttu-id="c18fb-109">Öppna PowerShell-konsolen och Anslut tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="c18fb-109">Open your PowerShell console and connect tooyour account.</span></span> <span data-ttu-id="c18fb-110">Mer information finns i [med hjälp av PowerShell med Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="c18fb-110">For more information, see [Using PowerShell with Resource Manager](../articles/azure-resource-manager/powershell-azure-resource-manager.md).</span></span>

```powershell
Login-AzureRmAccount
```

### <a name="select-hello-subscription"></a><span data-ttu-id="c18fb-111">Välj hello-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c18fb-111">Select hello subscription</span></span>
 
<span data-ttu-id="c18fb-112">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="c18fb-112">Check hello subscriptions for hello account.</span></span>

```powershell
Get-AzureRmSubscription
```

<span data-ttu-id="c18fb-113">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="c18fb-113">Choose which of your Azure subscriptions toouse.</span></span>

```powershell
Select-AzureRmSubscription -SubscriptionName "your_subscription_name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="c18fb-114">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c18fb-114">Create a resource group</span></span>

<span data-ttu-id="c18fb-115">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="c18fb-115">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="c18fb-116">Den här platsen används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="c18fb-116">This location is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="c18fb-117">Men eftersom alla DNS-resurser är globala, inte regional, har hello valet av resursgruppens plats ingen inverkan på Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="c18fb-117">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="c18fb-118">Du kan hoppa över det här steget om du använder en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="c18fb-118">You can skip this step if you are using an existing resource group.</span></span>

```powershell
New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="c18fb-119">Registrera resursprovider</span><span class="sxs-lookup"><span data-stu-id="c18fb-119">Register resource provider</span></span>

<span data-ttu-id="c18fb-120">hello Azure DNS-tjänst som hanteras av hello Microsoft.Network-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="c18fb-120">hello Azure DNS service is managed by hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="c18fb-121">Din Azure-prenumeration måste vara registrerade toouse den här resursprovidern innan du kan använda Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="c18fb-121">Your Azure subscription must be registered toouse this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="c18fb-122">Det här är en engångsåtgärd för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c18fb-122">This is a one-time operation for each subscription.</span></span>

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```