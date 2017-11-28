## <a name="set-up-azure-cli-for-azure-dns"></a><span data-ttu-id="be473-101">Konfigurera Azure CLI för Azure DNS</span><span class="sxs-lookup"><span data-stu-id="be473-101">Set up Azure CLI for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="be473-102">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="be473-102">Before you begin</span></span>

<span data-ttu-id="be473-103">Kontrollera att du har följande innan du påbörjar konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="be473-103">Verify that you have the following items before beginning your configuration.</span></span>

* <span data-ttu-id="be473-104">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="be473-104">An Azure subscription.</span></span> <span data-ttu-id="be473-105">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="be473-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="be473-106">Installera den senaste versionen av Azure CLI. Den finns tillgänglig för Windows, Linux och MAC.</span><span class="sxs-lookup"><span data-stu-id="be473-106">Install the latest version of the Azure CLI, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="be473-107">Mer information finns på [Installera Azure CLI](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="be473-107">More information is available at [Install the Azure CLI](../articles/cli-install-nodejs.md).</span></span>

### <a name="sign-in-to-your-azure-account"></a><span data-ttu-id="be473-108">Logga in på ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="be473-108">Sign in to your Azure account</span></span>

<span data-ttu-id="be473-109">Öppna ett konsolfönster och autentisera med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="be473-109">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="be473-110">Mer information finns i [Logga in i Azure från Azure CLI](../articles/xplat-cli-connect.md)</span><span class="sxs-lookup"><span data-stu-id="be473-110">For more information, see [Log in to Azure from the Azure CLI](../articles/xplat-cli-connect.md)</span></span>

```azurecli
azure login
```

### <a name="switch-cli-mode"></a><span data-ttu-id="be473-111">Växla CLI-läge</span><span class="sxs-lookup"><span data-stu-id="be473-111">Switch CLI mode</span></span>

<span data-ttu-id="be473-112">Azure DNS använder Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="be473-112">Azure DNS uses Azure Resource Manager.</span></span> <span data-ttu-id="be473-113">Se till att du byter CLI-läge så att du kan använda Azure Resource Manager-kommandon.</span><span class="sxs-lookup"><span data-stu-id="be473-113">Make sure you switch CLI mode to use Azure Resource Manager commands.</span></span>

```azurecli
azure config mode arm
```

### <a name="select-the-subscription"></a><span data-ttu-id="be473-114">Välja prenumerationen</span><span class="sxs-lookup"><span data-stu-id="be473-114">Select the subscription</span></span>

<span data-ttu-id="be473-115">Kontrollera prenumerationerna för kontot.</span><span class="sxs-lookup"><span data-stu-id="be473-115">Check the subscriptions for the account.</span></span>

```azurecli
azure account list
```

<span data-ttu-id="be473-116">Välj vilka av dina Azure-prenumerationer som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="be473-116">Choose which of your Azure subscriptions to use.</span></span>

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="be473-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="be473-117">Create a resource group</span></span>

<span data-ttu-id="be473-118">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="be473-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="be473-119">Detta används som standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="be473-119">This is used as the default location for resources in that resource group.</span></span> <span data-ttu-id="be473-120">Men eftersom alla DNS-resurser är globala, inte regionala, så påverkar inte valet av resursgruppens plats Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="be473-120">However, because all DNS resources are global, not regional, the choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="be473-121">Du kan hoppa över det här steget om du använder en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="be473-121">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="be473-122">Registrera resursprovider</span><span class="sxs-lookup"><span data-stu-id="be473-122">Register resource provider</span></span>

<span data-ttu-id="be473-123">Azure DNS-tjänsten hanteras av Microsoft.Network-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="be473-123">The Azure DNS service is managed by the Microsoft.Network resource provider.</span></span> <span data-ttu-id="be473-124">Din Azure-prenumeration måste vara registrerad att använda den här resursprovidern innan du kan använda Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="be473-124">Your Azure subscription must be registered to use this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="be473-125">Det här är en engångsåtgärd för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="be473-125">This is a one-time operation for each subscription.</span></span>

```azurecli
azure provider register --namespace Microsoft.Network
```

