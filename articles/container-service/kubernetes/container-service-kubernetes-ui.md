---
title: "aaaManage Azure Kubernetes kluster med webbgränssnittet | Microsoft Docs"
description: "Med hjälp av hello webbgränssnitt Kubernetes i Azure Container Service"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e24ea0b82c94d2fd4610e4442699ef756590e6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-kubernetes-web-ui-with-azure-container-service"></a><span data-ttu-id="7defc-103">Med hjälp av hello webbgränssnitt Kubernetes med Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="7defc-103">Using hello Kubernetes web UI with Azure Container Service</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7defc-104">Krav</span><span class="sxs-lookup"><span data-stu-id="7defc-104">Prerequisites</span></span>
<span data-ttu-id="7defc-105">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="7defc-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>


<span data-ttu-id="7defc-106">Det förutsätts även att du har hello Azure CLI 2.0 och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="7defc-106">It also assumes that you have hello Azure CLI 2.0 and `kubectl` tools installed.</span></span>

<span data-ttu-id="7defc-107">Du kan testa om du har hello `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="7defc-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="7defc-108">Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="7defc-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="7defc-109">Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="7defc-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="7defc-110">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="7defc-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a><span data-ttu-id="7defc-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="7defc-111">Overview</span></span>

### <a name="connect-toohello-web-ui"></a><span data-ttu-id="7defc-112">Ansluta toohello webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="7defc-112">Connect toohello web UI</span></span>
<span data-ttu-id="7defc-113">Du kan starta hello Kubernetes webbgränssnittet genom att köra:</span><span class="sxs-lookup"><span data-stu-id="7defc-113">You can launch hello Kubernetes web UI by running:</span></span>

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

<span data-ttu-id="7defc-114">Detta ska öppna en webbläsare konfigurerats tootalk tooa säker webbproxy ansluta din lokala dator toohello Kubernetes webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="7defc-114">This should open a web browser configured tootalk tooa secure proxy connecting your local machine toohello Kubernetes web UI.</span></span>

### <a name="create-and-expose-a-service"></a><span data-ttu-id="7defc-115">Skapa och visa en tjänst</span><span class="sxs-lookup"><span data-stu-id="7defc-115">Create and expose a service</span></span>
1. <span data-ttu-id="7defc-116">Klicka på hello Kubernetes webbgränssnittet, **skapa** knappen i hello övre högra fönstret.</span><span class="sxs-lookup"><span data-stu-id="7defc-116">In hello Kubernetes web UI, click **Create** button in hello upper right window.</span></span>

    ![Kubernetes skapa gränssnitt](./media/container-service-kubernetes-ui/create.png)

    <span data-ttu-id="7defc-118">En öppnas dialogrutan där du kan börja skapa ditt program.</span><span class="sxs-lookup"><span data-stu-id="7defc-118">A dialog box opens where you can start creating your application.</span></span>

2. <span data-ttu-id="7defc-119">Namnge den hello `hello-nginx`.</span><span class="sxs-lookup"><span data-stu-id="7defc-119">Give it hello name `hello-nginx`.</span></span> <span data-ttu-id="7defc-120">Använd hello [ `nginx` behållare från Docker](https://hub.docker.com/_/nginx/) och distribuera tre repliker av den här webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7defc-120">Use hello [`nginx` container from Docker](https://hub.docker.com/_/nginx/) and deploy three replicas of this web service.</span></span>

    ![Dialogrutan Skapa i Kubernetes baljor](./media/container-service-kubernetes-ui/nginx.png)

3. <span data-ttu-id="7defc-122">Under **Service**väljer **externa** och ange port 80.</span><span class="sxs-lookup"><span data-stu-id="7defc-122">Under **Service**, select **External** and enter port 80.</span></span>

    <span data-ttu-id="7defc-123">Den här inställningen belastningsutjämnas trafik toohello tre repliker.</span><span class="sxs-lookup"><span data-stu-id="7defc-123">This setting load-balances traffic toohello three replicas.</span></span>

    ![Dialogrutan Skapa i Kubernetes Service](./media/container-service-kubernetes-ui/service.png)

4. <span data-ttu-id="7defc-125">Klicka på **distribuera** toodeploy dessa behållare och tjänster.</span><span class="sxs-lookup"><span data-stu-id="7defc-125">Click **Deploy** toodeploy these containers and services.</span></span>

    ![Distribuera Kubernetes](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a><span data-ttu-id="7defc-127">Visa behållarna</span><span class="sxs-lookup"><span data-stu-id="7defc-127">View your containers</span></span>
<span data-ttu-id="7defc-128">När du klickar på **distribuera**, hello UI visas en vy över din tjänst som distribueras:</span><span class="sxs-lookup"><span data-stu-id="7defc-128">After you click **Deploy**, hello UI shows a view of your service as it deploys:</span></span>

![Kubernetes Status](./media/container-service-kubernetes-ui/status.png)

<span data-ttu-id="7defc-130">Du kan se hello status för varje Kubernetes objekt i hello cirkel hello vänster på användargränssnittet under **skida**.</span><span class="sxs-lookup"><span data-stu-id="7defc-130">You can see hello status of each Kubernetes object in hello circle on hello left-hand side of the UI, under **Pods**.</span></span> <span data-ttu-id="7defc-131">Om det är en delvis fylld cirkel distribuera hello objektet fortfarande.</span><span class="sxs-lookup"><span data-stu-id="7defc-131">If it is a partially full circle, then hello object is still deploying.</span></span> <span data-ttu-id="7defc-132">När ett objekt har distribuerats helt, visas en grön bock:</span><span class="sxs-lookup"><span data-stu-id="7defc-132">When an object is fully deployed, it displays a green check mark:</span></span>

![Kubernetes distribueras](./media/container-service-kubernetes-ui/deployed.png)

<span data-ttu-id="7defc-134">När allt körs klickar du på något av skida toosee detaljer om hello som kör webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="7defc-134">Once everything is running, click one of your pods toosee details about hello running web service.</span></span>

![Kubernetes skida](./media/container-service-kubernetes-ui/pods.png)

<span data-ttu-id="7defc-136">I hello **skida** kan du se information om hello behållare i hello baljor samt hello CPU och minne resurser som används av dessa behållare:</span><span class="sxs-lookup"><span data-stu-id="7defc-136">In hello **Pods** view, you can see information about hello containers in hello pod as well as hello CPU and memory resources used by those containers:</span></span>

![Kubernetes resurser](./media/container-service-kubernetes-ui/resources.png)

<span data-ttu-id="7defc-138">Om du inte ser hello resurser, kan du behöva toowait några minuter för hello övervakning data toopropagate.</span><span class="sxs-lookup"><span data-stu-id="7defc-138">If you don't see hello resources, you may need toowait a few minutes for hello monitoring data toopropagate.</span></span>

<span data-ttu-id="7defc-139">toosee hello loggar för din behållaren, klicka på **visa loggar**.</span><span class="sxs-lookup"><span data-stu-id="7defc-139">toosee hello logs for your container, click **View logs**.</span></span>

![Kubernetes loggar](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a><span data-ttu-id="7defc-141">Visa din tjänst</span><span class="sxs-lookup"><span data-stu-id="7defc-141">Viewing your service</span></span>
<span data-ttu-id="7defc-142">I tillägg toorunning din behållare hello Kubernetes UI har skapat en extern `Service` som etablerar en belastningen belastningsutjämnaren toobring trafik toohello behållare i klustret.</span><span class="sxs-lookup"><span data-stu-id="7defc-142">In addition toorunning your containers, hello Kubernetes UI has created an external `Service` which provisions a load balancer toobring traffic toohello containers in your cluster.</span></span>

<span data-ttu-id="7defc-143">Hello vänstra navigeringsfönstret, klicka på **Services** tooview alla tjänster (det ska bara finnas ett).</span><span class="sxs-lookup"><span data-stu-id="7defc-143">In hello left navigation pane, click **Services** tooview all services (there should be only one).</span></span>

![Kubernetes tjänster](./media/container-service-kubernetes-ui/service-deployed.png)

<span data-ttu-id="7defc-145">Du bör se den externa slutpunkten (IP-adress) som har allokerats tooyour service i denna vy.</span><span class="sxs-lookup"><span data-stu-id="7defc-145">In that view, you should see an external endpoint (IP address) that has been allocated tooyour service.</span></span>
<span data-ttu-id="7defc-146">Om du klickar på att IP-adress, bör du se din Nginx-behållaren som körs bakom belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="7defc-146">If you click that IP address, you should see your Nginx container running behind the load balancer.</span></span>

![nginx-vyn](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a><span data-ttu-id="7defc-148">Ändra storlek på tjänsten</span><span class="sxs-lookup"><span data-stu-id="7defc-148">Resizing your service</span></span>
<span data-ttu-id="7defc-149">I tillägg tooviewing Hej dina objekt i Användargränssnittet, kan du redigera och uppdatera hello Kubernetes API-objekt.</span><span class="sxs-lookup"><span data-stu-id="7defc-149">In addition tooviewing your objects in hello UI, you can edit and update hello Kubernetes API objects.</span></span>

<span data-ttu-id="7defc-150">Klicka först på **distributioner** i hello lämnas fönstret navigering toosee hello distributionen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7defc-150">First, click **Deployments** in hello left navigation pane toosee hello deployment for your service.</span></span>

<span data-ttu-id="7defc-151">När du är i vyn klickar du på hello replikuppsättningen och klicka sedan på **redigera** i hello övre navigeringsfältet:</span><span class="sxs-lookup"><span data-stu-id="7defc-151">Once you are in that view, click on hello replica set, and then click **Edit** in hello upper navigation bar:</span></span>

![Redigera Kubernetes](./media/container-service-kubernetes-ui/edit.png)

<span data-ttu-id="7defc-153">Redigera hello `spec.replicas` fältet toobe `2`, och klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="7defc-153">Edit hello `spec.replicas` field toobe `2`, and click **Update**.</span></span>

<span data-ttu-id="7defc-154">Detta medför hello antal repliker toodrop tootwo genom att ta bort en av dina skida.</span><span class="sxs-lookup"><span data-stu-id="7defc-154">This causes hello number of replicas toodrop tootwo by deleting one of your pods.</span></span>

 

