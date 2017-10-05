---
title: "Hantera Azure Kubernetes kluster med webbgränssnittet | Microsoft Docs"
description: "Med hjälp av Kubernetes webbgränssnittet i Azure Container Service"
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
ms.openlocfilehash: e31f90d61fc61f17582372fe9f491a1e21f628b0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-kubernetes-web-ui-with-azure-container-service"></a><span data-ttu-id="300d2-103">Med Azure Container Service Kubernetes webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="300d2-103">Using the Kubernetes web UI with Azure Container Service</span></span>

## <a name="prerequisites"></a><span data-ttu-id="300d2-104">Krav</span><span class="sxs-lookup"><span data-stu-id="300d2-104">Prerequisites</span></span>
<span data-ttu-id="300d2-105">Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="300d2-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>


<span data-ttu-id="300d2-106">Det förutsätts även att du har Azure CLI 2.0 och `kubectl` verktygen som installeras.</span><span class="sxs-lookup"><span data-stu-id="300d2-106">It also assumes that you have the Azure CLI 2.0 and `kubectl` tools installed.</span></span>

<span data-ttu-id="300d2-107">Du kan testa om du har den `az` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="300d2-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="300d2-108">Om du inte har den `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="300d2-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="300d2-109">Du kan testa om du har den `kubectl` installerat genom att köra verktyget:</span><span class="sxs-lookup"><span data-stu-id="300d2-109">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="300d2-110">Om du inte har `kubectl` installerat, kan du köra:</span><span class="sxs-lookup"><span data-stu-id="300d2-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a><span data-ttu-id="300d2-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="300d2-111">Overview</span></span>

### <a name="connect-to-the-web-ui"></a><span data-ttu-id="300d2-112">Ansluta till webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="300d2-112">Connect to the web UI</span></span>
<span data-ttu-id="300d2-113">Du kan starta Kubernetes webbgränssnittet genom att köra:</span><span class="sxs-lookup"><span data-stu-id="300d2-113">You can launch the Kubernetes web UI by running:</span></span>

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

<span data-ttu-id="300d2-114">Detta ska öppna en webbläsare som konfigurerats för att kommunicera med en säker proxy ansluta din lokala dator till Kubernetes webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="300d2-114">This should open a web browser configured to talk to a secure proxy connecting your local machine to the Kubernetes web UI.</span></span>

### <a name="create-and-expose-a-service"></a><span data-ttu-id="300d2-115">Skapa och visa en tjänst</span><span class="sxs-lookup"><span data-stu-id="300d2-115">Create and expose a service</span></span>
1. <span data-ttu-id="300d2-116">Klicka på Kubernetes webbgränssnittet **skapa** knappen i det övre högra fönstret.</span><span class="sxs-lookup"><span data-stu-id="300d2-116">In the Kubernetes web UI, click **Create** button in the upper right window.</span></span>

    ![Kubernetes skapa gränssnitt](./media/container-service-kubernetes-ui/create.png)

    <span data-ttu-id="300d2-118">En öppnas dialogrutan där du kan börja skapa ditt program.</span><span class="sxs-lookup"><span data-stu-id="300d2-118">A dialog box opens where you can start creating your application.</span></span>

2. <span data-ttu-id="300d2-119">Ge den namnet `hello-nginx`.</span><span class="sxs-lookup"><span data-stu-id="300d2-119">Give it the name `hello-nginx`.</span></span> <span data-ttu-id="300d2-120">Använd den [ `nginx` behållare från Docker](https://hub.docker.com/_/nginx/) och distribuera tre repliker av den här webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="300d2-120">Use the [`nginx` container from Docker](https://hub.docker.com/_/nginx/) and deploy three replicas of this web service.</span></span>

    ![Dialogrutan Skapa i Kubernetes baljor](./media/container-service-kubernetes-ui/nginx.png)

3. <span data-ttu-id="300d2-122">Under **Service**väljer **externa** och ange port 80.</span><span class="sxs-lookup"><span data-stu-id="300d2-122">Under **Service**, select **External** and enter port 80.</span></span>

    <span data-ttu-id="300d2-123">Den här inställningen belastningsutjämnas trafik till tre replikerna.</span><span class="sxs-lookup"><span data-stu-id="300d2-123">This setting load-balances traffic to the three replicas.</span></span>

    ![Dialogrutan Skapa i Kubernetes Service](./media/container-service-kubernetes-ui/service.png)

4. <span data-ttu-id="300d2-125">Klicka på **distribuera** att distribuera dessa behållare och tjänster.</span><span class="sxs-lookup"><span data-stu-id="300d2-125">Click **Deploy** to deploy these containers and services.</span></span>

    ![Distribuera Kubernetes](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a><span data-ttu-id="300d2-127">Visa behållarna</span><span class="sxs-lookup"><span data-stu-id="300d2-127">View your containers</span></span>
<span data-ttu-id="300d2-128">När du klickar på **distribuera**, Gränssnittet visas en vy över din tjänst som distribueras:</span><span class="sxs-lookup"><span data-stu-id="300d2-128">After you click **Deploy**, the UI shows a view of your service as it deploys:</span></span>

![Kubernetes Status](./media/container-service-kubernetes-ui/status.png)

<span data-ttu-id="300d2-130">Du kan se status för varje Kubernetes objekt i cirkeln på vänster sida av Användargränssnittet under **skida**.</span><span class="sxs-lookup"><span data-stu-id="300d2-130">You can see the status of each Kubernetes object in the circle on the left-hand side of the UI, under **Pods**.</span></span> <span data-ttu-id="300d2-131">Om det är en delvis fylld cirkel distribuera objektet fortfarande.</span><span class="sxs-lookup"><span data-stu-id="300d2-131">If it is a partially full circle, then the object is still deploying.</span></span> <span data-ttu-id="300d2-132">När ett objekt har distribuerats helt, visas en grön bock:</span><span class="sxs-lookup"><span data-stu-id="300d2-132">When an object is fully deployed, it displays a green check mark:</span></span>

![Kubernetes distribueras](./media/container-service-kubernetes-ui/deployed.png)

<span data-ttu-id="300d2-134">Klicka på en av dina skida att se information om webbtjänsten körs när allt körs.</span><span class="sxs-lookup"><span data-stu-id="300d2-134">Once everything is running, click one of your pods to see details about the running web service.</span></span>

![Kubernetes skida](./media/container-service-kubernetes-ui/pods.png)

<span data-ttu-id="300d2-136">I den **skida** kan du se information om behållare i baljor samt resurserna CPU och minne som används av dessa behållare:</span><span class="sxs-lookup"><span data-stu-id="300d2-136">In the **Pods** view, you can see information about the containers in the pod as well as the CPU and memory resources used by those containers:</span></span>

![Kubernetes resurser](./media/container-service-kubernetes-ui/resources.png)

<span data-ttu-id="300d2-138">Om du inte ser resurserna, kan du behöva vänta några minuter att sprida övervakningsdata.</span><span class="sxs-lookup"><span data-stu-id="300d2-138">If you don't see the resources, you may need to wait a few minutes for the monitoring data to propagate.</span></span>

<span data-ttu-id="300d2-139">Klicka för att visa loggar för din behållaren **visa loggar**.</span><span class="sxs-lookup"><span data-stu-id="300d2-139">To see the logs for your container, click **View logs**.</span></span>

![Kubernetes loggar](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a><span data-ttu-id="300d2-141">Visa din tjänst</span><span class="sxs-lookup"><span data-stu-id="300d2-141">Viewing your service</span></span>
<span data-ttu-id="300d2-142">Förutom att köra behållarna Kubernetes UI har skapat en extern `Service` som etablerar en belastningsutjämnare för att göra trafik behållare i klustret.</span><span class="sxs-lookup"><span data-stu-id="300d2-142">In addition to running your containers, the Kubernetes UI has created an external `Service` which provisions a load balancer to bring traffic to the containers in your cluster.</span></span>

<span data-ttu-id="300d2-143">I det vänstra navigeringsfönstret klickar du på **Services** att visa alla tjänster (det ska bara finnas ett).</span><span class="sxs-lookup"><span data-stu-id="300d2-143">In the left navigation pane, click **Services** to view all services (there should be only one).</span></span>

![Kubernetes tjänster](./media/container-service-kubernetes-ui/service-deployed.png)

<span data-ttu-id="300d2-145">I vyn, bör du se en extern slutpunkt (IP-adress) som har allokerats till din tjänst.</span><span class="sxs-lookup"><span data-stu-id="300d2-145">In that view, you should see an external endpoint (IP address) that has been allocated to your service.</span></span>
<span data-ttu-id="300d2-146">Om du klickar på att IP-adress, bör du se din Nginx-behållaren som körs bakom belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="300d2-146">If you click that IP address, you should see your Nginx container running behind the load balancer.</span></span>

![nginx-vyn](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a><span data-ttu-id="300d2-148">Ändra storlek på tjänsten</span><span class="sxs-lookup"><span data-stu-id="300d2-148">Resizing your service</span></span>
<span data-ttu-id="300d2-149">Förutom att visa objekten i Användargränssnittet, kan du redigera och uppdatera Kubernetes API-objekt.</span><span class="sxs-lookup"><span data-stu-id="300d2-149">In addition to viewing your objects in the UI, you can edit and update the Kubernetes API objects.</span></span>

<span data-ttu-id="300d2-150">Klicka först på **distributioner** i det vänstra navigeringsfönstret för att se distributionen för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="300d2-150">First, click **Deployments** in the left navigation pane to see the deployment for your service.</span></span>

<span data-ttu-id="300d2-151">När du är i vyn klickar du på uppsättningen och klicka sedan på **redigera** i det övre navigeringsfältet:</span><span class="sxs-lookup"><span data-stu-id="300d2-151">Once you are in that view, click on the replica set, and then click **Edit** in the upper navigation bar:</span></span>

![Redigera Kubernetes](./media/container-service-kubernetes-ui/edit.png)

<span data-ttu-id="300d2-153">Redigera den `spec.replicas` fältet ska vara `2`, och klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="300d2-153">Edit the `spec.replicas` field to be `2`, and click **Update**.</span></span>

<span data-ttu-id="300d2-154">Detta gör antalet repliker till två genom att ta bort en av dina skida.</span><span class="sxs-lookup"><span data-stu-id="300d2-154">This causes the number of replicas to drop to two by deleting one of your pods.</span></span>

 

