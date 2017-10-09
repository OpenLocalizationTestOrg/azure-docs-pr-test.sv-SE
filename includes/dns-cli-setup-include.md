## <a name="set-up-azure-cli-for-azure-dns"></a><span data-ttu-id="89d5f-101">Konfigurera Azure CLI för Azure DNS</span><span class="sxs-lookup"><span data-stu-id="89d5f-101">Set up Azure CLI for Azure DNS</span></span>

### <a name="before-you-begin"></a><span data-ttu-id="89d5f-102">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="89d5f-102">Before you begin</span></span>

<span data-ttu-id="89d5f-103">Kontrollera att du har hello följande objekt innan du börjar din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="89d5f-103">Verify that you have hello following items before beginning your configuration.</span></span>

* <span data-ttu-id="89d5f-104">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="89d5f-104">An Azure subscription.</span></span> <span data-ttu-id="89d5f-105">Om du inte har någon Azure-prenumeration kan du aktivera dina [MSDN-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) eller registrera dig för ett [kostnadsfritt konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="89d5f-105">If you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) or sign up for a [free account](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="89d5f-106">Installera hello senaste versionen av hello Azure CLI, tillgänglig för Windows, Linux och MAC.</span><span class="sxs-lookup"><span data-stu-id="89d5f-106">Install hello latest version of hello Azure CLI, available for Windows, Linux, or MAC.</span></span> <span data-ttu-id="89d5f-107">Mer information finns på [installera hello Azure CLI](../articles/cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="89d5f-107">More information is available at [Install hello Azure CLI](../articles/cli-install-nodejs.md).</span></span>

### <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="89d5f-108">Logga in tooyour Azure-konto</span><span class="sxs-lookup"><span data-stu-id="89d5f-108">Sign in tooyour Azure account</span></span>

<span data-ttu-id="89d5f-109">Öppna ett konsolfönster och autentisera med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="89d5f-109">Open a console window and authenticate with your credentials.</span></span> <span data-ttu-id="89d5f-110">Mer information finns i [logga in tooAzure från hello Azure CLI](../articles/xplat-cli-connect.md)</span><span class="sxs-lookup"><span data-stu-id="89d5f-110">For more information, see [Log in tooAzure from hello Azure CLI](../articles/xplat-cli-connect.md)</span></span>

```azurecli
azure login
```

### <a name="switch-cli-mode"></a><span data-ttu-id="89d5f-111">Växla CLI-läge</span><span class="sxs-lookup"><span data-stu-id="89d5f-111">Switch CLI mode</span></span>

<span data-ttu-id="89d5f-112">Azure DNS använder Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="89d5f-112">Azure DNS uses Azure Resource Manager.</span></span> <span data-ttu-id="89d5f-113">Kontrollera att du växla CLI läge toouse Azure Resource Manager-kommandon.</span><span class="sxs-lookup"><span data-stu-id="89d5f-113">Make sure you switch CLI mode toouse Azure Resource Manager commands.</span></span>

```azurecli
azure config mode arm
```

### <a name="select-hello-subscription"></a><span data-ttu-id="89d5f-114">Välj hello-prenumeration</span><span class="sxs-lookup"><span data-stu-id="89d5f-114">Select hello subscription</span></span>

<span data-ttu-id="89d5f-115">Kontrollera hello prenumerationer för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="89d5f-115">Check hello subscriptions for hello account.</span></span>

```azurecli
azure account list
```

<span data-ttu-id="89d5f-116">Välj vilka av dina Azure-prenumerationer toouse.</span><span class="sxs-lookup"><span data-stu-id="89d5f-116">Choose which of your Azure subscriptions toouse.</span></span>

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a><span data-ttu-id="89d5f-117">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="89d5f-117">Create a resource group</span></span>

<span data-ttu-id="89d5f-118">Azure Resource Manager kräver att alla resursgrupper anger en plats.</span><span class="sxs-lookup"><span data-stu-id="89d5f-118">Azure Resource Manager requires that all resource groups specify a location.</span></span> <span data-ttu-id="89d5f-119">Detta används som hello standardplatsen för resurser i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="89d5f-119">This is used as hello default location for resources in that resource group.</span></span> <span data-ttu-id="89d5f-120">Men eftersom alla DNS-resurser är globala, inte regional, har hello valet av resursgruppens plats ingen inverkan på Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="89d5f-120">However, because all DNS resources are global, not regional, hello choice of resource group location has no impact on Azure DNS.</span></span>

<span data-ttu-id="89d5f-121">Du kan hoppa över det här steget om du använder en befintlig resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="89d5f-121">You can skip this step if you are using an existing resource group.</span></span>

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a><span data-ttu-id="89d5f-122">Registrera resursprovider</span><span class="sxs-lookup"><span data-stu-id="89d5f-122">Register resource provider</span></span>

<span data-ttu-id="89d5f-123">hello Azure DNS-tjänst som hanteras av hello Microsoft.Network-resursprovidern.</span><span class="sxs-lookup"><span data-stu-id="89d5f-123">hello Azure DNS service is managed by hello Microsoft.Network resource provider.</span></span> <span data-ttu-id="89d5f-124">Din Azure-prenumeration måste vara registrerade toouse den här resursprovidern innan du kan använda Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="89d5f-124">Your Azure subscription must be registered toouse this resource provider before you can use Azure DNS.</span></span> <span data-ttu-id="89d5f-125">Det här är en engångsåtgärd för varje prenumeration.</span><span class="sxs-lookup"><span data-stu-id="89d5f-125">This is a one-time operation for each subscription.</span></span>

```azurecli
azure provider register --namespace Microsoft.Network
```

