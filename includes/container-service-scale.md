# <a name="scale-agent-nodes-in-a-container-service-cluster"></a><span data-ttu-id="a5776-101">Skala agentnoder i ett behållartjänstkluster</span><span class="sxs-lookup"><span data-stu-id="a5776-101">Scale agent nodes in a Container Service cluster</span></span>
<span data-ttu-id="a5776-102">När du har [distribuerat ett Azure Container Service-kluster](../articles/container-service/dcos-swarm/container-service-deployment.md) kanske du behöver ändra antalet agentnoder.</span><span class="sxs-lookup"><span data-stu-id="a5776-102">After [deploying an Azure Container Service cluster](../articles/container-service/dcos-swarm/container-service-deployment.md), you might need to change the number of agent nodes.</span></span> <span data-ttu-id="a5776-103">Till exempel kan du behöva fler agenter så att du kan köra flera behållarprogram eller instanser.</span><span class="sxs-lookup"><span data-stu-id="a5776-103">For example, you might need more agents so you can run more container applications or instances.</span></span> 

<span data-ttu-id="a5776-104">Du kan ändra antalet agentnoder i ett DC/OS-, Docker Swarm- eller Kubernetes-kluster med Azure Portal eller Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="a5776-104">You can change the number of agent nodes in a DC/OS, Docker Swarm, or Kubernetes cluster by using the Azure portal or the Azure CLI 2.0.</span></span> 

## <a name="scale-with-the-azure-portal"></a><span data-ttu-id="a5776-105">Skala med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a5776-105">Scale with the Azure portal</span></span>

1. <span data-ttu-id="a5776-106">I [Azure Portal](https://portal.azure.com) bläddrar du till **Behållartjänster** och klickar sedan på den behållartjänst du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="a5776-106">In the [Azure portal](https://portal.azure.com), browse for **Container services**, and then click the container service that you want to modify.</span></span>
2. <span data-ttu-id="a5776-107">På bladet **Behållartjänst** klickar du på **Agenter**.</span><span class="sxs-lookup"><span data-stu-id="a5776-107">In the **Container service** blade, click **Agents**.</span></span>
3. <span data-ttu-id="a5776-108">In **Antal virtuella datorer** anger du önskat antal agentnoder.</span><span class="sxs-lookup"><span data-stu-id="a5776-108">In **VM Count**, enter the desired number of agents nodes.</span></span>

    ![Skala en pool i portalen](./media/container-service-scale/container-service-scale-portal.png)

4. <span data-ttu-id="a5776-110">Spara konfigurationen genom att klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a5776-110">To save the configuration, click **Save**.</span></span>

## <a name="scale-with-the-azure-cli-20"></a><span data-ttu-id="a5776-111">Skala med Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a5776-111">Scale with the Azure CLI 2.0</span></span>

<span data-ttu-id="a5776-112">Kontrollera att den senaste versionen av Azure CLI 2.0 är [installerad](/cli/azure/install-az-cli2) och att du är inloggad på ett Azure-konto (`az login`).</span><span class="sxs-lookup"><span data-stu-id="a5776-112">Make sure that you [installed](/cli/azure/install-az-cli2) the latest Azure CLI 2.0 and logged in to an azure account (`az login`).</span></span>

### <a name="see-the-current-agent-count"></a><span data-ttu-id="a5776-113">Visa det aktuella antalet agenter</span><span class="sxs-lookup"><span data-stu-id="a5776-113">See the current agent count</span></span>
<span data-ttu-id="a5776-114">För att se antalet agenter i klustret kör du kommandot `az acs show`.</span><span class="sxs-lookup"><span data-stu-id="a5776-114">To see the number of agents currently in the cluster, run the `az acs show` command.</span></span> <span data-ttu-id="a5776-115">Detta visar klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a5776-115">This shows the cluster configuration.</span></span> <span data-ttu-id="a5776-116">Följande kommando visar till exempel konfigurationen av behållartjänsten som heter `containerservice-myACSName` i resursgruppen `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="a5776-116">For example, the following command shows the configuration of the container service named `containerservice-myACSName` in the resource group `myResourceGroup`:</span></span>

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

<span data-ttu-id="a5776-117">Kommandot returnerar antalet agenter i `Count`-värdet under `AgentPoolProfiles`.</span><span class="sxs-lookup"><span data-stu-id="a5776-117">The command returns the number of agents in the `Count` value under `AgentPoolProfiles`.</span></span>

### <a name="use-the-az-acs-scale-command"></a><span data-ttu-id="a5776-118">Använda kommandot az acs scale</span><span class="sxs-lookup"><span data-stu-id="a5776-118">Use the az acs scale command</span></span>
<span data-ttu-id="a5776-119">Om du vill ändra antalet agentnoder kör du kommandot `az acs scale` och anger **resursgrupp**, **namnet på behållartjänsten** och önskat **nytt agentantal**.</span><span class="sxs-lookup"><span data-stu-id="a5776-119">To change the number of agent nodes, run the `az acs scale` command and supply the **resource group**, **container service name**, and the desired **new agent count**.</span></span> <span data-ttu-id="a5776-120">Om du använder ett lägre eller högre antal kan du skala ned eller upp.</span><span class="sxs-lookup"><span data-stu-id="a5776-120">By using a smaller or higher number, you can scale down or up, respectively.</span></span>

<span data-ttu-id="a5776-121">Om du till exempel vill ändra antalet agenter i föregående kluster till 10 skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a5776-121">For example, to change the number of agents in the previous cluster to 10, type the following command:</span></span>

```azurecli
az acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

<span data-ttu-id="a5776-122">Azure CLI 2.0 returnerar en JSON-sträng som representerar den nya konfigurationen av behållartjänsten, inklusive det nya agentantalet.</span><span class="sxs-lookup"><span data-stu-id="a5776-122">The Azure CLI 2.0 returns a JSON string representing the new configuration of the container service, including the new agent count.</span></span>

<span data-ttu-id="a5776-123">Om du vill ha fler kommandoalternativ kör du `az acs scale --help`.</span><span class="sxs-lookup"><span data-stu-id="a5776-123">For more command options, run `az acs scale --help`.</span></span>

## <a name="scaling-considerations"></a><span data-ttu-id="a5776-124">Att tänka på vid skalning</span><span class="sxs-lookup"><span data-stu-id="a5776-124">Scaling considerations</span></span>

* <span data-ttu-id="a5776-125">Antalet agentnoder måste vara mellan 1 och 100.</span><span class="sxs-lookup"><span data-stu-id="a5776-125">The number of agent nodes must be between 1 and 100, inclusive.</span></span> 

* <span data-ttu-id="a5776-126">Din kärnkvot kan begränsa antalet agentnoder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="a5776-126">Your cores quota can limit the number of agent nodes in a cluster.</span></span>

* <span data-ttu-id="a5776-127">Skalningsåtgärder för agentnoder tillämpas på en skalningsuppsättning för en virtuell Azure-dator som innehåller agentpoolen.</span><span class="sxs-lookup"><span data-stu-id="a5776-127">Agent node scaling operations are applied to an Azure virtual machine scale set that contains the agent pool.</span></span> <span data-ttu-id="a5776-128">Endast agentnoder i den privata poolen skalas i ett DC/OS-kluster med åtgärder som visas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a5776-128">In a DC/OS cluster, only agent nodes in the private pool are scaled by the operations shown in this article.</span></span>

* <span data-ttu-id="a5776-129">Beroende på vilken initierare du distribuerar i ditt kluster kan du separat skala antalet instanser i en behållare som körs på klustret.</span><span class="sxs-lookup"><span data-stu-id="a5776-129">Depending on the orchestrator you deploy in your cluster, you can separately scale the number of instances of a container running on the cluster.</span></span> <span data-ttu-id="a5776-130">I exempelvis ett DC/OS-kluster kan du använda [gränssnittet i Marathon](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) för att ändra antalet instanser i ett behållarprogram.</span><span class="sxs-lookup"><span data-stu-id="a5776-130">For example, in a DC/OS cluster, use the [Marathon UI](../articles/container-service/dcos-swarm/container-service-mesos-marathon-ui.md) to change the number of instances of a container application.</span></span>

* <span data-ttu-id="a5776-131">För närvarande går det inte att autoskala agentnoder i ett behållartjänstkluster.</span><span class="sxs-lookup"><span data-stu-id="a5776-131">Currently, autoscaling of agent nodes in a container service cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a5776-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a5776-132">Next steps</span></span>
* <span data-ttu-id="a5776-133">Se [fler exempel](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) på att använda kommandon i Azure CLI 2.0 med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="a5776-133">See [more examples](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) of using Azure CLI 2.0 commands with Azure Container Service.</span></span>
* <span data-ttu-id="a5776-134">Läs mer om [DC/OS-agentpooler](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="a5776-134">Learn more about [DC/OS agent pools](../articles/container-service/dcos-swarm/container-service-dcos-agents.md) in Azure Container Service.</span></span>

