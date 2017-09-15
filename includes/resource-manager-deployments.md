## <a name="incremental-and-complete-deployments"></a><span data-ttu-id="35891-101">Inkrementella och fullständiga distributioner</span><span class="sxs-lookup"><span data-stu-id="35891-101">Incremental and complete deployments</span></span>
<span data-ttu-id="35891-102">När du distribuerar dina resurser kan ange du att distributionen är en inkrementell uppdatering eller en fullständig uppdatering.</span><span class="sxs-lookup"><span data-stu-id="35891-102">When deploying your resources, you specify that the deployment is either an incremental update or a complete update.</span></span> <span data-ttu-id="35891-103">Den främsta skillnaden mellan dessa två lägen är hur Resource Manager hanterar befintliga resurser i resursgruppen som inte ingår i mallen:</span><span class="sxs-lookup"><span data-stu-id="35891-103">The primary difference between these two modes is how Resource Manager handles existing resources in the resource group that are not in the template:</span></span>

* <span data-ttu-id="35891-104">I Resource Manager-fullständig läge **tar bort** resurser som finns i resursgruppen men har inte angetts i mallen.</span><span class="sxs-lookup"><span data-stu-id="35891-104">In complete mode, Resource Manager **deletes** resources that exist in the resource group but are not specified in the template.</span></span> 
* <span data-ttu-id="35891-105">I Resource Manager-inkrementell läge **lämnar oförändrat** resurser som finns i resursgruppen men har inte angetts i mallen.</span><span class="sxs-lookup"><span data-stu-id="35891-105">In incremental mode, Resource Manager **leaves unchanged** resources that exist in the resource group but are not specified in the template.</span></span>

<span data-ttu-id="35891-106">Resource Manager försöker etablera alla resurser som har angetts i mallen för båda lägena.</span><span class="sxs-lookup"><span data-stu-id="35891-106">For both modes, Resource Manager attempts to provision all resources specified in the template.</span></span> <span data-ttu-id="35891-107">Åtgärden resulterar i ingen ändring om resursen finns redan i resursgruppen och dess inställningar har inte ändrats.</span><span class="sxs-lookup"><span data-stu-id="35891-107">If the resource already exists in the resource group and its settings are unchanged, the operation results in no change.</span></span> <span data-ttu-id="35891-108">Om du ändrar inställningarna för en resurs kan etableras resursen med de nya inställningarna.</span><span class="sxs-lookup"><span data-stu-id="35891-108">If you change the settings for a resource, the resource is provisioned with those new settings.</span></span> <span data-ttu-id="35891-109">Om du försöker uppdatera platsen eller typ av en befintlig resurs misslyckas distributionen med ett fel.</span><span class="sxs-lookup"><span data-stu-id="35891-109">If you attempt to update the location or type of an existing resource, the deployment fails with an error.</span></span> <span data-ttu-id="35891-110">I stället distribuerar en ny resurs med platsen eller ange att du behöver.</span><span class="sxs-lookup"><span data-stu-id="35891-110">Instead, deploy a new resource with the location or type that you need.</span></span>

<span data-ttu-id="35891-111">Som standard använder Resource Manager inkrementell läge.</span><span class="sxs-lookup"><span data-stu-id="35891-111">By default, Resource Manager uses the incremental mode.</span></span>

<span data-ttu-id="35891-112">Studera följande scenario för att illustrera skillnaden mellan inkrementella och fullständiga lägen.</span><span class="sxs-lookup"><span data-stu-id="35891-112">To illustrate the difference between incremental and complete modes, consider the following scenario.</span></span>

<span data-ttu-id="35891-113">**Befintlig resursgrupp** innehåller:</span><span class="sxs-lookup"><span data-stu-id="35891-113">**Existing Resource Group** contains:</span></span>

* <span data-ttu-id="35891-114">Resurs A</span><span class="sxs-lookup"><span data-stu-id="35891-114">Resource A</span></span>
* <span data-ttu-id="35891-115">Resurs B</span><span class="sxs-lookup"><span data-stu-id="35891-115">Resource B</span></span>
* <span data-ttu-id="35891-116">Resurs C</span><span class="sxs-lookup"><span data-stu-id="35891-116">Resource C</span></span>

<span data-ttu-id="35891-117">**Mallen** definierar:</span><span class="sxs-lookup"><span data-stu-id="35891-117">**Template** defines:</span></span>

* <span data-ttu-id="35891-118">Resurs A</span><span class="sxs-lookup"><span data-stu-id="35891-118">Resource A</span></span>
* <span data-ttu-id="35891-119">Resurs B</span><span class="sxs-lookup"><span data-stu-id="35891-119">Resource B</span></span>
* <span data-ttu-id="35891-120">Resurs D</span><span class="sxs-lookup"><span data-stu-id="35891-120">Resource D</span></span>

<span data-ttu-id="35891-121">När de distribueras i **inkrementella** läge, resursgruppen innehåller:</span><span class="sxs-lookup"><span data-stu-id="35891-121">When deployed in **incremental** mode, the resource group contains:</span></span>

* <span data-ttu-id="35891-122">Resurs A</span><span class="sxs-lookup"><span data-stu-id="35891-122">Resource A</span></span>
* <span data-ttu-id="35891-123">Resurs B</span><span class="sxs-lookup"><span data-stu-id="35891-123">Resource B</span></span>
* <span data-ttu-id="35891-124">Resurs C</span><span class="sxs-lookup"><span data-stu-id="35891-124">Resource C</span></span>
* <span data-ttu-id="35891-125">Resurs D</span><span class="sxs-lookup"><span data-stu-id="35891-125">Resource D</span></span>

<span data-ttu-id="35891-126">När de distribueras i **fullständig** resurs C-läge har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="35891-126">When deployed in **complete** mode, Resource C is deleted.</span></span> <span data-ttu-id="35891-127">Resursgruppen innehåller:</span><span class="sxs-lookup"><span data-stu-id="35891-127">The resource group contains:</span></span>

* <span data-ttu-id="35891-128">Resurs A</span><span class="sxs-lookup"><span data-stu-id="35891-128">Resource A</span></span>
* <span data-ttu-id="35891-129">Resurs B</span><span class="sxs-lookup"><span data-stu-id="35891-129">Resource B</span></span>
* <span data-ttu-id="35891-130">Resurs D</span><span class="sxs-lookup"><span data-stu-id="35891-130">Resource D</span></span>
