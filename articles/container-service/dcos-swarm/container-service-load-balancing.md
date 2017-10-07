---
title: "aaaLoad saldo behållare i Azure DC/OS-kluster | Microsoft Docs"
description: "Belastningen över flera behållare i ett Azure Container Service DC/OS-kluster."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, Micro-tjänster, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 2249cb06880cdb7e9a3aa94c0750c6a27316d349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="9ee92-104">Läsa in saldo behållare i ett Azure Container Service DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="9ee92-104">Load balance containers in an Azure Container Service DC/OS cluster</span></span>
<span data-ttu-id="9ee92-105">I den här artikeln förklarar vi hur toocreate en intern belastningsutjämnare i DC/OS hanteras Azure Container Service med Marathon-LB.</span><span class="sxs-lookup"><span data-stu-id="9ee92-105">In this article, we explore how toocreate an internal load balancer in a DC/OS managed Azure Container Service using Marathon-LB.</span></span> <span data-ttu-id="9ee92-106">Den här konfigurationen kan du tooscale dina program vågrätt.</span><span class="sxs-lookup"><span data-stu-id="9ee92-106">This configuration enables you tooscale your applications horizontally.</span></span> <span data-ttu-id="9ee92-107">Du kan också tootake nytta av hello offentliga och privata agent kluster genom att placera din belastningsutjämnare på hello offentliga och programmet behållarna på hello privata klustret.</span><span class="sxs-lookup"><span data-stu-id="9ee92-107">It also allows you tootake advantage of hello public and private agent clusters by placing your load balancers on hello public cluster and your application containers on hello private cluster.</span></span> <span data-ttu-id="9ee92-108">I den här kursen har du:</span><span class="sxs-lookup"><span data-stu-id="9ee92-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ee92-109">Konfigurera en belastningsutjämnare för Marathon</span><span class="sxs-lookup"><span data-stu-id="9ee92-109">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="9ee92-110">Distribuera ett program med hello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ee92-110">Deploy an application using hello load balancer</span></span>
> * <span data-ttu-id="9ee92-111">Konfigurera och Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ee92-111">Configure and Azure load balancer</span></span>

<span data-ttu-id="9ee92-112">Du behöver en ACS-DC /-OS klustret toocomplete hello stegen i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="9ee92-112">You need an ACS DC/OS cluster toocomplete hello steps in this tutorial.</span></span> <span data-ttu-id="9ee92-113">Om det behövs, [detta skriptexempel](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kan skapa en åt dig.</span><span class="sxs-lookup"><span data-stu-id="9ee92-113">If needed, [this script sample](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) can create one for you.</span></span>

<span data-ttu-id="9ee92-114">Den här kursen kräver hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9ee92-114">This tutorial requires hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="9ee92-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="9ee92-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="9ee92-116">Om du behöver tooupgrade finns [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9ee92-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a><span data-ttu-id="9ee92-117">Översikt för belastningsutjämning</span><span class="sxs-lookup"><span data-stu-id="9ee92-117">Load balancing overview</span></span>

<span data-ttu-id="9ee92-118">Det finns två lager för belastningsutjämning i ett Azure Container Service DC/OS-kluster:</span><span class="sxs-lookup"><span data-stu-id="9ee92-118">There are two load-balancing layers in an Azure Container Service DC/OS cluster:</span></span> 

<span data-ttu-id="9ee92-119">**Azure belastningsutjämnare** ger offentliga startpunkterna (hello viktiga att slutanvändare åtkomst till).</span><span class="sxs-lookup"><span data-stu-id="9ee92-119">**Azure Load Balancer** provides public entry points (hello ones that end users access).</span></span> <span data-ttu-id="9ee92-120">Ett Azure LB tillhandahålls automatiskt med Azure Container Service och är som standard konfigurerade tooexpose port 80 och 443 8080.</span><span class="sxs-lookup"><span data-stu-id="9ee92-120">An Azure LB is provided automatically by Azure Container Service and is, by default, configured tooexpose port 80, 443 and 8080.</span></span>

<span data-ttu-id="9ee92-121">**Hej Marathon belastningsutjämnare (marathon-lb)** vägar inkommande begäranden toocontainer instanser som dessa tjänstbegäranden.</span><span class="sxs-lookup"><span data-stu-id="9ee92-121">**hello Marathon Load Balancer (marathon-lb)** routes inbound requests toocontainer instances that service these requests.</span></span> <span data-ttu-id="9ee92-122">Som vi skalanpassar hello behållarna som tillhandahåller våra webbtjänsten hello marathon-lb anpassas efter dynamiskt.</span><span class="sxs-lookup"><span data-stu-id="9ee92-122">As we scale hello containers providing our web service, hello marathon-lb dynamically adapts.</span></span> <span data-ttu-id="9ee92-123">Denna belastningsutjämning har inte angetts som standard i Container Service, men det är enkelt tooinstall.</span><span class="sxs-lookup"><span data-stu-id="9ee92-123">This load balancer is not provided by default in your Container Service, but it is easy tooinstall.</span></span>

## <a name="configure-marathon-load-balancer"></a><span data-ttu-id="9ee92-124">Konfigurera Marathon belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ee92-124">Configure Marathon Load Balancer</span></span>

<span data-ttu-id="9ee92-125">Marathon belastningsutjämnare automatiskt dynamiskt baserat på hello-behållare som du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="9ee92-125">Marathon Load Balancer dynamically reconfigures itself based on hello containers that you've deployed.</span></span> <span data-ttu-id="9ee92-126">Det är också flexibel toohello förlust av en behållare eller agent - om detta inträffar, Apache Mesos startar hello behållaren någon annanstans och marathon-lb anpassas efter dina behov.</span><span class="sxs-lookup"><span data-stu-id="9ee92-126">It's also resilient toohello loss of a container or an agent - if this occurs, Apache Mesos restarts hello container elsewhere and marathon-lb adapts.</span></span>

<span data-ttu-id="9ee92-127">Hello kör följande kommando tooinstall hello marathon belastningsutjämnaren på hello offentlig agent klustret.</span><span class="sxs-lookup"><span data-stu-id="9ee92-127">Run hello following command tooinstall hello marathon load balancer on hello public agent's cluster.</span></span>

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a><span data-ttu-id="9ee92-128">Distribuera program för Utjämning av nätverksbelastning</span><span class="sxs-lookup"><span data-stu-id="9ee92-128">Deploy load balanced application</span></span>

<span data-ttu-id="9ee92-129">Nu när vi har hello marathon-lb-paketet kan vi distribuera en behållare för program som vi vill tooload saldo.</span><span class="sxs-lookup"><span data-stu-id="9ee92-129">Now that we have hello marathon-lb package, we can deploy an application container that we wish tooload balance.</span></span> 

<span data-ttu-id="9ee92-130">Först hämta hello FQDN för hello offentligt exponeras agenter.</span><span class="sxs-lookup"><span data-stu-id="9ee92-130">First, get hello FQDN of hello publicly exposed agents.</span></span>

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

<span data-ttu-id="9ee92-131">Skapa sedan en fil med namnet *hello web.json* och kopiera i hello efter innehållet.</span><span class="sxs-lookup"><span data-stu-id="9ee92-131">Next, create a file named *hello-web.json* and copy in hello following contents.</span></span> <span data-ttu-id="9ee92-132">Hej `HAPROXY_0_VHOST` etikett måste toobe uppdateras med hello FQDN för hello DC/OS-agenterna.</span><span class="sxs-lookup"><span data-stu-id="9ee92-132">hello `HAPROXY_0_VHOST` label needs toobe updated with hello FQDN of hello DC/OS agents.</span></span> 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

<span data-ttu-id="9ee92-133">Använda hello DC/OS CLI toorun hello programmet.</span><span class="sxs-lookup"><span data-stu-id="9ee92-133">Use hello DC/OS CLI toorun hello application.</span></span> <span data-ttu-id="9ee92-134">Som standard distribuerar Marathon hello hello programobjekt toohello privata kluster.</span><span class="sxs-lookup"><span data-stu-id="9ee92-134">By default Marathon deploys hello hello applicaton toohello private cluster.</span></span> <span data-ttu-id="9ee92-135">Detta innebär att hello ovan distribution är endast tillgänglig via din belastningsutjämnare, som vanligtvis är hello önskat beteende.</span><span class="sxs-lookup"><span data-stu-id="9ee92-135">This means that hello above deployment is only accessible via your load balancer, which is usually hello desired behavior.</span></span>

```azurecli-interactive
dcos marathon app add hello-web.json
```

<span data-ttu-id="9ee92-136">Bläddra toohello FQDN för hello klustret tooview belastningsutjämnade Agentprogrammet när hello programmet har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="9ee92-136">Once hello application has been deployed, browse toohello FQDN of hello agent cluster tooview load balanced application.</span></span>

![Bild av belastningsutjämnade program](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a><span data-ttu-id="9ee92-138">Konfigurera Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ee92-138">Configure Azure Load Balancer</span></span>

<span data-ttu-id="9ee92-139">Som standard exponerar Azure Load Balancer portarna 80, 8080 och 443.</span><span class="sxs-lookup"><span data-stu-id="9ee92-139">By default, Azure Load Balancer exposes ports 80, 8080, and 443.</span></span> <span data-ttu-id="9ee92-140">Om du använder en av de här tre portarna (som vi gör i hello ovanstående exempel) och det inte finns något du behöver toodo.</span><span class="sxs-lookup"><span data-stu-id="9ee92-140">If you're using one of these three ports (as we do in hello above example), then there is nothing you need toodo.</span></span> <span data-ttu-id="9ee92-141">Du bör vara kan toohit agent läsa in belastningsutjämnings FQDN och varje gång du uppdaterar du tryck något av de tre webbservrarna resursallokering överskrids.</span><span class="sxs-lookup"><span data-stu-id="9ee92-141">You should be able toohit your agent load balancer's FQDN, and each time you refresh, you'll hit one of your three web servers in a round-robin fashion.</span></span> 

<span data-ttu-id="9ee92-142">Om du använder en annan port, behöver du tooadd en resursallokering regel och en avsökning på hello belastningsutjämnare för hello-porten som du använde.</span><span class="sxs-lookup"><span data-stu-id="9ee92-142">If you use a different port, you need tooadd a round-robin rule and a probe on hello load balancer for hello port that you used.</span></span> <span data-ttu-id="9ee92-143">Du kan göra detta från hello [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), med hello kommandon `azure network lb rule create` och `azure network lb probe create`.</span><span class="sxs-lookup"><span data-stu-id="9ee92-143">You can do this from hello [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), with hello commands `azure network lb rule create` and `azure network lb probe create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ee92-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ee92-144">Next steps</span></span>

<span data-ttu-id="9ee92-145">I kursen får du lärt dig om belastningsutjämning i ACS med både hello Marathon och Azure belastningen belastningsutjämnare inklusive hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="9ee92-145">In this tutorial, you learned about load balancing in ACS with both hello Marathon and Azure load balancers including hello following actions:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9ee92-146">Konfigurera en belastningsutjämnare för Marathon</span><span class="sxs-lookup"><span data-stu-id="9ee92-146">Configure a Marathon Load Balancer</span></span>
> * <span data-ttu-id="9ee92-147">Distribuera ett program med hello belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ee92-147">Deploy an application using hello load balancer</span></span>
> * <span data-ttu-id="9ee92-148">Konfigurera och Azure belastningsutjämnare</span><span class="sxs-lookup"><span data-stu-id="9ee92-148">Configure and Azure load balancer</span></span>

<span data-ttu-id="9ee92-149">Avancera toohello nästa självstudiekurs toolearn om integreringen med Azure storage med DC/OS i Azure.</span><span class="sxs-lookup"><span data-stu-id="9ee92-149">Advance toohello next tutorial toolearn about integrating Azure storage with DC/OS in Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="9ee92-150">Montera Azure-filresursen i DC/OS-klustret</span><span class="sxs-lookup"><span data-stu-id="9ee92-150">Mount Azure File Share in DC/OS cluster</span></span>](container-service-dcos-fileshare.md)