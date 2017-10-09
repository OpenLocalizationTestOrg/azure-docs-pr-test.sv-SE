<span data-ttu-id="3b5e4-101">Med Azure Resource Manager kan du definiera parametrar för värden som du vill toospecify när hello mallen distribueras.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-101">With Azure Resource Manager, you define parameters for values you want toospecify when hello template is deployed.</span></span> <span data-ttu-id="3b5e4-102">hello mallen innehåller ett avsnitt som heter parametrar som innehåller alla hello parametervärden.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-102">hello template includes a section called Parameters that contains all of hello parameter values.</span></span>
<span data-ttu-id="3b5e4-103">Du bör definiera en parameter för de värden som varierar baserat på hello-projekt som du distribuerar eller på hello-miljö som du distribuerar till.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-103">You should define a parameter for those values that will vary based on hello project you are deploying or based on hello environment you are deploying to.</span></span> <span data-ttu-id="3b5e4-104">Definiera inte parametrar för värden som behålls alltid hello samma.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-104">Do not define parameters for values that will always stay hello same.</span></span> <span data-ttu-id="3b5e4-105">Varje parametervärdet används i hello mallen toodefine hello resurser som distribueras.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-105">Each parameter value is used in hello template toodefine hello resources that are deployed.</span></span> 

<span data-ttu-id="3b5e4-106">När du definierar parametrar, använda hello **allowedValues** fältet toospecify som värden en användare kan ange under distributionen.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-106">When defining parameters, use hello **allowedValues** field toospecify which values a user can provide during deployment.</span></span> <span data-ttu-id="3b5e4-107">Använd hello **defaultValue** fältet tooassign värdet toohello parameter, om inget värde anges under distributionen.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-107">Use hello **defaultValue** field tooassign a value toohello parameter, if no value is provided during deployment.</span></span>

<span data-ttu-id="3b5e4-108">Vi beskriver varje parameter i hello mallen.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-108">We will describe each parameter in hello template.</span></span>

### <a name="sitename"></a><span data-ttu-id="3b5e4-109">Platsnamn</span><span class="sxs-lookup"><span data-stu-id="3b5e4-109">siteName</span></span>
<span data-ttu-id="3b5e4-110">hello namnet på hello webbprogram som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-110">hello name of hello web app that you wish toocreate.</span></span>

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a><span data-ttu-id="3b5e4-111">hostingPlanName</span><span class="sxs-lookup"><span data-stu-id="3b5e4-111">hostingPlanName</span></span>
<span data-ttu-id="3b5e4-112">hello namnet på hello Apptjänst planera toouse som värd för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-112">hello name of hello App Service plan toouse for hosting hello web app.</span></span>

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a><span data-ttu-id="3b5e4-113">SKU</span><span class="sxs-lookup"><span data-stu-id="3b5e4-113">sku</span></span>
<span data-ttu-id="3b5e4-114">hello prisnivån för hello som värd för planen.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-114">hello pricing tier for hello hosting plan.</span></span>

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "hello pricing tier for hello hosting plan."
      }
    }

<span data-ttu-id="3b5e4-115">hello mallen definierar hello värden som tillåts för den här parametern och tilldelas ett standardvärde (S1) om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-115">hello template defines hello values that are permitted for this parameter, and assigns a default value (S1) if no value is specified.</span></span>

### <a name="workersize"></a><span data-ttu-id="3b5e4-116">workerSize</span><span class="sxs-lookup"><span data-stu-id="3b5e4-116">workerSize</span></span>
<span data-ttu-id="3b5e4-117">Hej instansstorleken för hello värd plan (liten, medel eller hög).</span><span class="sxs-lookup"><span data-stu-id="3b5e4-117">hello instance size of hello hosting plan (small, medium, or large).</span></span>

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

<span data-ttu-id="3b5e4-118">hello mallen definierar hello värden som tillåts för den här parametern (0, 1 eller 2) och tilldelas ett standardvärde (0) om inget värde anges.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-118">hello template defines hello values that are permitted for this parameter (0, 1, or 2), and assigns a default value (0) if no value is specified.</span></span> <span data-ttu-id="3b5e4-119">hello värden motsvarar toosmall, medelstora och stora.</span><span class="sxs-lookup"><span data-stu-id="3b5e4-119">hello values correspond toosmall, medium and large.</span></span>

