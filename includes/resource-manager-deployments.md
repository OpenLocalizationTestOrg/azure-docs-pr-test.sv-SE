## <a name="incremental-and-complete-deployments"></a><span data-ttu-id="1f993-101">Inkrementella och fullständiga distributioner</span><span class="sxs-lookup"><span data-stu-id="1f993-101">Incremental and complete deployments</span></span>
<span data-ttu-id="1f993-102">När du distribuerar dina resurser kan ange du att hello distribution är en inkrementell uppdatering eller en fullständig uppdatering.</span><span class="sxs-lookup"><span data-stu-id="1f993-102">When deploying your resources, you specify that hello deployment is either an incremental update or a complete update.</span></span> <span data-ttu-id="1f993-103">hello främsta skillnaden mellan dessa två lägen är hur Resource Manager hanterar befintliga resurser i resursgruppen hello som inte ingår i hello mallen:</span><span class="sxs-lookup"><span data-stu-id="1f993-103">hello primary difference between these two modes is how Resource Manager handles existing resources in hello resource group that are not in hello template:</span></span>

* <span data-ttu-id="1f993-104">I Resource Manager-fullständig läge **tar bort** resurser som finns i resursgruppen hello men har inte angetts i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1f993-104">In complete mode, Resource Manager **deletes** resources that exist in hello resource group but are not specified in hello template.</span></span> 
* <span data-ttu-id="1f993-105">I Resource Manager-inkrementell läge **lämnar oförändrat** resurser som finns i resursgruppen hello men har inte angetts i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1f993-105">In incremental mode, Resource Manager **leaves unchanged** resources that exist in hello resource group but are not specified in hello template.</span></span>

<span data-ttu-id="1f993-106">För båda lägena försöker Resource Manager tooprovision alla resurser som har angetts i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="1f993-106">For both modes, Resource Manager attempts tooprovision all resources specified in hello template.</span></span> <span data-ttu-id="1f993-107">Om hello resursen finns redan i hello resursgruppen och dess inställningar är desamma, resulterar hello åtgärden i ingen ändring.</span><span class="sxs-lookup"><span data-stu-id="1f993-107">If hello resource already exists in hello resource group and its settings are unchanged, hello operation results in no change.</span></span> <span data-ttu-id="1f993-108">Om du ändrar hello inställningar för en resurs kan etableras hello resurs med de nya inställningarna.</span><span class="sxs-lookup"><span data-stu-id="1f993-108">If you change hello settings for a resource, hello resource is provisioned with those new settings.</span></span> <span data-ttu-id="1f993-109">Om du försöker tooupdate hello plats eller en typ för en befintlig resurs misslyckas hello distributionen med ett fel.</span><span class="sxs-lookup"><span data-stu-id="1f993-109">If you attempt tooupdate hello location or type of an existing resource, hello deployment fails with an error.</span></span> <span data-ttu-id="1f993-110">I stället distribuerar en ny resurs med hello plats eller ange att du behöver.</span><span class="sxs-lookup"><span data-stu-id="1f993-110">Instead, deploy a new resource with hello location or type that you need.</span></span>

<span data-ttu-id="1f993-111">Som standard använder Resource Manager hello inkrementell läge.</span><span class="sxs-lookup"><span data-stu-id="1f993-111">By default, Resource Manager uses hello incremental mode.</span></span>

<span data-ttu-id="1f993-112">tooillustrate hello skillnaden mellan inkrementella och fullständiga läge, Överväg hello följande scenario.</span><span class="sxs-lookup"><span data-stu-id="1f993-112">tooillustrate hello difference between incremental and complete modes, consider hello following scenario.</span></span>

<span data-ttu-id="1f993-113">**Befintlig resursgrupp** innehåller:</span><span class="sxs-lookup"><span data-stu-id="1f993-113">**Existing Resource Group** contains:</span></span>

* <span data-ttu-id="1f993-114">Resurs A</span><span class="sxs-lookup"><span data-stu-id="1f993-114">Resource A</span></span>
* <span data-ttu-id="1f993-115">Resurs B</span><span class="sxs-lookup"><span data-stu-id="1f993-115">Resource B</span></span>
* <span data-ttu-id="1f993-116">Resurs C</span><span class="sxs-lookup"><span data-stu-id="1f993-116">Resource C</span></span>

<span data-ttu-id="1f993-117">**Mallen** definierar:</span><span class="sxs-lookup"><span data-stu-id="1f993-117">**Template** defines:</span></span>

* <span data-ttu-id="1f993-118">Resurs A</span><span class="sxs-lookup"><span data-stu-id="1f993-118">Resource A</span></span>
* <span data-ttu-id="1f993-119">Resurs B</span><span class="sxs-lookup"><span data-stu-id="1f993-119">Resource B</span></span>
* <span data-ttu-id="1f993-120">Resurs D</span><span class="sxs-lookup"><span data-stu-id="1f993-120">Resource D</span></span>

<span data-ttu-id="1f993-121">När de distribueras i **inkrementella** läge hello resursgruppen innehåller:</span><span class="sxs-lookup"><span data-stu-id="1f993-121">When deployed in **incremental** mode, hello resource group contains:</span></span>

* <span data-ttu-id="1f993-122">Resurs A</span><span class="sxs-lookup"><span data-stu-id="1f993-122">Resource A</span></span>
* <span data-ttu-id="1f993-123">Resurs B</span><span class="sxs-lookup"><span data-stu-id="1f993-123">Resource B</span></span>
* <span data-ttu-id="1f993-124">Resurs C</span><span class="sxs-lookup"><span data-stu-id="1f993-124">Resource C</span></span>
* <span data-ttu-id="1f993-125">Resurs D</span><span class="sxs-lookup"><span data-stu-id="1f993-125">Resource D</span></span>

<span data-ttu-id="1f993-126">När de distribueras i **fullständig** resurs C-läge har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="1f993-126">When deployed in **complete** mode, Resource C is deleted.</span></span> <span data-ttu-id="1f993-127">hello resursgruppen innehåller:</span><span class="sxs-lookup"><span data-stu-id="1f993-127">hello resource group contains:</span></span>

* <span data-ttu-id="1f993-128">Resurs A</span><span class="sxs-lookup"><span data-stu-id="1f993-128">Resource A</span></span>
* <span data-ttu-id="1f993-129">Resurs B</span><span class="sxs-lookup"><span data-stu-id="1f993-129">Resource B</span></span>
* <span data-ttu-id="1f993-130">Resurs D</span><span class="sxs-lookup"><span data-stu-id="1f993-130">Resource D</span></span>
