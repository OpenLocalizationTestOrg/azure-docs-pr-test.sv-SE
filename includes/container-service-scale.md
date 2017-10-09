# <a name="scale-agent-nodes-in-a-container-service-cluster"></a><span data-ttu-id="f1589-101">Skala agentnoder i ett behållartjänstkluster</span><span class="sxs-lookup"><span data-stu-id="f1589-101">Scale agent nodes in a Container Service cluster</span></span>
<span data-ttu-id="f1589-102">Efter [distribuera ett Azure Container Service-kluster](../articles/container-service/dcos-swarm/container-service-deployment.md), måste du kanske toochange hello antalet agent noder.</span><span class="sxs-lookup"><span data-stu-id="f1589-102">After [deploying an Azure Container Service cluster](../articles/container-service/dcos-swarm/container-service-deployment.md), you might need toochange hello number of agent nodes.</span></span> <span data-ttu-id="f1589-103">Till exempel kan du behöva fler agenter så att du kan köra flera behållarprogram eller instanser.</span><span class="sxs-lookup"><span data-stu-id="f1589-103">For example, you might need more agents so you can run more container applications or instances.</span></span> 

<span data-ttu-id="f1589-104">Du kan ändra hello antalet agent noder i ett kluster för DC/OS, Docker Swarm eller Kubernetes med hello Azure-portalen eller hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="f1589-104">You can change hello number of agent nodes in a DC/OS, Docker Swarm, or Kubernetes cluster by using hello Azure portal or hello Azure CLI 2.0.</span></span> 

## <a name="scale-with-hello-azure-portal"></a><span data-ttu-id="f1589-105">Skala med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f1589-105">Scale with hello Azure portal</span></span>

1. <span data-ttu-id="f1589-106">I hello [Azure-portalen](https://portal.azure.com), bläddra efter **behållartjänster**, och klicka sedan på hello behållartjänsten som du vill toomodify.</span><span class="sxs-lookup"><span data-stu-id="f1589-106">In hello [Azure portal](https://portal.azure.com), browse for **Container services**, and then click hello container service that you want toomodify.</span></span>
2. <span data-ttu-id="f1589-107">I hello **behållartjänsten** bladet, klickar du på **agenter**.</span><span class="sxs-lookup"><span data-stu-id="f1589-107">In hello **Container service** blade, click **Agents**.</span></span>
3. <span data-ttu-id="f1589-108">I **antal VM**, ange hello önskat antal agenter noder.</span><span class="sxs-lookup"><span data-stu-id="f1589-108">In **VM Count**, enter hello desired number of agents nodes.</span></span>

    ![Skala en pool i hello-portalen](./media/container-service-scale/container-service-scale-portal.png)

4. <span data-ttu-id="f1589-110">toosave hello konfiguration, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="f1589-110">toosave hello configuration, click **Save**.</span></span>

## <a name="scale-with-hello-azure-cli-20"></a><span data-ttu-id="f1589-111">Skala med hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f1589-111">Scale with hello Azure CLI 2.0</span></span>

<span data-ttu-id="f1589-112">Se till att du [installerat](/cli/azure/install-az-cli2) hello senaste Azure CLI 2.0 och loggas i tooan azure-konto (`az login`).</span><span class="sxs-lookup"><span data-stu-id="f1589-112">Make sure that you [installed](/cli/azure/install-az-cli2) hello latest Azure CLI 2.0 and logged in tooan azure account (`az login`).</span></span>

### <a name="see-hello-current-agent-count"></a><span data-ttu-id="f1589-113">Se hello aktuella agentantal</span><span class="sxs-lookup"><span data-stu-id="f1589-113">See hello current agent count</span></span>
<span data-ttu-id="f1589-114">toosee hello antalet agenter som för närvarande i hello klustret kör hello `az acs show` kommando.</span><span class="sxs-lookup"><span data-stu-id="f1589-114">toosee hello number of agents currently in hello cluster, run hello `az acs show` command.</span></span> <span data-ttu-id="f1589-115">Detta visar hello klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f1589-115">This shows hello cluster configuration.</span></span> <span data-ttu-id="f1589-116">Hej exempelvis följande kommando visar hello configuration hello container Service med namnet `containerservice-myACSName` i hello resursgruppen `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="f1589-116">For example, hello following command shows hello configuration of hello container service named `containerservice-myACSName` in hello resource group `myResourceGroup`:</span></span>

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

<span data-ttu-id="f1589-117">hello kommandot returnerar hello antalet agenter i hello `Count` värde `AgentPoolProfiles`.</span><span class="sxs-lookup"><span data-stu-id="f1589-117">hello command returns hello number of agents in hello `Count` value under `AgentPoolProfiles`.</span></span>

### <a name="use-hello-az-acs-scale-command"></a><span data-ttu-id="f1589-118">Använd hello az acs skala kommando</span><span class="sxs-lookup"><span data-stu-id="f1589-118">Use hello az acs scale command</span></span>
<span data-ttu-id="f1589-119">toochange hello antal agent noder, kör hello `az acs scale` kommandot och varor hello **resursgruppen**, **service behållarnamn**, och hello önskad **nya agentantal**.</span><span class="sxs-lookup"><span data-stu-id="f1589-119">toochange hello number of agent nodes, run hello `az acs scale` command and supply hello **resource group**, **container service name**, and hello desired **new agent count**.</span></span> <span data-ttu-id="f1589-120">Om du använder ett lägre eller högre antal kan du skala ned eller upp.</span><span class="sxs-lookup"><span data-stu-id="f1589-120">By using a smaller or higher number, you can scale down or up, respectively.</span></span>

<span data-ttu-id="f1589-121">Till exempel toochange hello antalet agenter i hello tidigare kluster too10, typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f1589-121">For example, toochange hello number of agents in hello previous cluster too10, type hello following command:</span></span>

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

<span data-ttu-id="f1589-122">hello Azure CLI 2.0 returnerar en JSON-sträng som representerar hello ny konfiguration hello container Service, inklusive antal för nya hello-agenten.</span><span class="sxs-lookup"><span data-stu-id="f1589-122">hello Azure CLI 2.0 returns a JSON string representing hello new configuration of hello container service, including hello new agent count.</span></span>

<span data-ttu-id="f1589-123">Om du vill ha fler kommandoalternativ kör du `az acs scale --help`.</span><span class="sxs-lookup"><span data-stu-id="f1589-123">For more command options, run `az acs scale --help`.</span></span>

## <a name="scaling-considerations"></a><span data-ttu-id="f1589-124">Att tänka på vid skalning</span><span class="sxs-lookup"><span data-stu-id="f1589-124">Scaling considerations</span></span>

* <span data-ttu-id="f1589-125">hello antalet agent noder måste vara mellan 1 och 100.</span><span class="sxs-lookup"><span data-stu-id="f1589-125">hello number of agent nodes must be between 1 and 100, inclusive.</span></span> 

* <span data-ttu-id="f1589-126">Din kärnor kvot kan begränsa hello antalet agent noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="f1589-126">Your cores quota can limit hello number of agent nodes in a cluster.</span></span>

* <span data-ttu-id="f1589-127">Åtgärder för agenten nod skalning är tillämpade tooan Azure virtuella datorns skaluppsättning som innehåller hello agent poolen.</span><span class="sxs-lookup"><span data-stu-id="f1589-127">Agent node scaling operations are applied tooan Azure virtual machine scale set that contains hello agent pool.</span></span> <span data-ttu-id="f1589-128">I ett DC/OS-kluster skalas endast agent noder i hello privata pool av hello åtgärder visas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f1589-128">In a DC/OS cluster, only agent nodes in hello private pool are scaled by hello operations shown in this article.</span></span>

* <span data-ttu-id="f1589-129">Beroende på hello orchestrator som du distribuerar i ditt kluster, kan du separat skala hello antal instanser av en behållare som körs på klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f1589-129">Depending on hello orchestrator you deploy in your cluster, you can separately scale hello number of instances of a container running on hello cluster.</span></span> <span data-ttu-id="f1589-130">Till exempel använda hello i ett DC/OS-kluster [Marathon UI](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello antal instanser av ett program för behållare.</span><span class="sxs-lookup"><span data-stu-id="f1589-130">For example, in a DC/OS cluster, use hello [Marathon UI](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) toochange hello number of instances of a container application.</span></span>

* <span data-ttu-id="f1589-131">För närvarande går det inte att autoskala agentnoder i ett behållartjänstkluster.</span><span class="sxs-lookup"><span data-stu-id="f1589-131">Currently, autoscaling of agent nodes in a container service cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1589-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1589-132">Next steps</span></span>
* <span data-ttu-id="f1589-133">Se [fler exempel](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) på att använda kommandon i Azure CLI 2.0 med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="f1589-133">See [more examples](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) of using Azure CLI 2.0 commands with Azure Container Service.</span></span>
* <span data-ttu-id="f1589-134">Läs mer om [DC/OS-agentpooler](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="f1589-134">Learn more about [DC/OS agent pools](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) in Azure Container Service.</span></span>

