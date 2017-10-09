---
title: "aaaDeploy behållare med Helm i Azure Kubernetes | Microsoft Docs"
description: "Använd hello Helm paketering verktyget toodeploy behållare på ett Kubernetes kluster i Azure Container Service"
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: c7bd780afe00084ebe4e3a14873e1e340a29d144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-helm-toodeploy-containers-on-a-kubernetes-cluster"></a><span data-ttu-id="d2bef-103">Använd Helm toodeploy behållare på ett Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="d2bef-103">Use Helm toodeploy containers on a Kubernetes cluster</span></span> 

<span data-ttu-id="d2bef-104">[Helm](https://github.com/kubernetes/helm/) är en öppen källkod paketering verktyg som hjälper dig att installera och hantera hello livscykeln för Kubernetes program.</span><span class="sxs-lookup"><span data-stu-id="d2bef-104">[Helm](https://github.com/kubernetes/helm/) is an open-source packaging tool that helps you install and manage hello lifecycle of Kubernetes applications.</span></span> <span data-ttu-id="d2bef-105">Liknande tooLinux paketet chefer som lgh get och Yum, Helm är används toomanage Kubernetes diagram som är paket med förkonfigurerade Kubernetes resurser.</span><span class="sxs-lookup"><span data-stu-id="d2bef-105">Similar tooLinux package managers such as Apt-get and Yum, Helm is used toomanage Kubernetes charts, which are packages of preconfigured Kubernetes resources.</span></span> <span data-ttu-id="d2bef-106">Den här artikeln visar hur toowork med Helm på ett Kubernetes kluster distribueras i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="d2bef-106">This article shows you how toowork with Helm on a Kubernetes cluster deployed in Azure Container Service.</span></span>

<span data-ttu-id="d2bef-107">Helm består av två komponenter:</span><span class="sxs-lookup"><span data-stu-id="d2bef-107">Helm has two components:</span></span> 
* <span data-ttu-id="d2bef-108">Hej **Helm CLI** är en klient som körs på datorn lokalt eller i molnet hello</span><span class="sxs-lookup"><span data-stu-id="d2bef-108">hello **Helm CLI** is a client that runs on your machine locally or in hello cloud</span></span>  

* <span data-ttu-id="d2bef-109">**Rorkulten** är en server som körs på hello Kubernetes klustret och hanterar hello livscykeln för Kubernetes-program</span><span class="sxs-lookup"><span data-stu-id="d2bef-109">**Tiller** is a server that runs on hello Kubernetes cluster and manages hello lifecycle of your Kubernetes applications</span></span> 
 
## <a name="prerequisites"></a><span data-ttu-id="d2bef-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d2bef-110">Prerequisites</span></span>

* <span data-ttu-id="d2bef-111">[Skapa ett kluster med Kubernetes](container-service-kubernetes-walkthrough.md) i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="d2bef-111">[Create a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>

* <span data-ttu-id="d2bef-112">[Installera och konfigurera `kubectl` ](../container-service-connect.md) på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="d2bef-112">[Install and configure `kubectl`](../container-service-connect.md) on a local computer</span></span>

* <span data-ttu-id="d2bef-113">[Installera Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="d2bef-113">[Install Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) on a local computer</span></span>

## <a name="helm-basics"></a><span data-ttu-id="d2bef-114">Helm grunderna</span><span class="sxs-lookup"><span data-stu-id="d2bef-114">Helm basics</span></span> 

<span data-ttu-id="d2bef-115">tooview information om hello Kubernetes klustret att du installerar rorkulten och distribuerar program för att ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2bef-115">tooview information about hello Kubernetes cluster that you are installing Tiller and deploying your applications to, type hello following command:</span></span>

```bash
kubectl cluster-info 
```
![kubectl klusterinformation](./media/container-service-kubernetes-helm/clusterinfo.png)
 
<span data-ttu-id="d2bef-117">När du har installerat Helm installera rorkulten på Kubernetes klustret genom att skriva följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="d2bef-117">After you have installed Helm, install Tiller on your Kubernetes cluster by typing hello following command:</span></span>

```bash
helm init --upgrade
```
<span data-ttu-id="d2bef-118">När den är klar kan du se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="d2bef-118">When it completes successfully, you see output like hello following:</span></span>

![Rorkulten installation](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
<span data-ttu-id="d2bef-120">tooview alla hello Helm diagram tillgängliga i hello-lagringsplatsen, typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2bef-120">tooview all hello Helm charts available in hello repository, type hello following command:</span></span>

```bash 
helm search 
```

<span data-ttu-id="d2bef-121">Du kan se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="d2bef-121">You see output like hello following:</span></span>

![Helm sökning](./media/container-service-kubernetes-helm/helm-search.png)
 
<span data-ttu-id="d2bef-123">tooupdate hello diagram tooget hello senaste versioner, skriver du:</span><span class="sxs-lookup"><span data-stu-id="d2bef-123">tooupdate hello charts tooget hello latest versions, type:</span></span>

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a><span data-ttu-id="d2bef-124">Distribuera ett Nginx ingång controller diagram</span><span class="sxs-lookup"><span data-stu-id="d2bef-124">Deploy an Nginx ingress controller chart</span></span> 
 
<span data-ttu-id="d2bef-125">toodeploy en Nginx ingång controller diagram, skriver du ett enda kommando:</span><span class="sxs-lookup"><span data-stu-id="d2bef-125">toodeploy an Nginx ingress controller chart, type a single command:</span></span>

```bash
helm install stable/nginx-ingress 
```
![Distribuera ingång-styrenhet](./media/container-service-kubernetes-helm/nginx-ingress.png)

<span data-ttu-id="d2bef-127">Om du anger `kubectl get svc` tooview alla tjänster som körs på hello klustret kan du se att en IP-adress har tilldelats toohello ingång domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="d2bef-127">If you type `kubectl get svc` tooview all services that are running on hello cluster, you see that an IP address is assigned toohello ingress controller.</span></span> <span data-ttu-id="d2bef-128">(Under hello tilldelning pågår visas `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="d2bef-128">(While hello assignment is in progress, you see `<pending>`.</span></span> <span data-ttu-id="d2bef-129">Det tar några minuter toocomplete.)</span><span class="sxs-lookup"><span data-stu-id="d2bef-129">It takes a couple of minutes toocomplete.)</span></span> 

<span data-ttu-id="d2bef-130">Navigera toohello värdet för hello externa IP-adress toosee hello Nginx backend körs när hello IP-adress har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="d2bef-130">After hello IP address is assigned, navigate toohello value of hello external IP address toosee hello Nginx backend running.</span></span> 
 
![Ingång IP-adress](./media/container-service-kubernetes-helm/ingress-ip-address.png)


<span data-ttu-id="d2bef-132">toosee en lista över diagram installeras på klustret, typ:</span><span class="sxs-lookup"><span data-stu-id="d2bef-132">toosee a list of charts installed on your cluster, type:</span></span>

```bash
helm list 
```

<span data-ttu-id="d2bef-133">Du kan förkorta hello kommandot för`helm ls`.</span><span class="sxs-lookup"><span data-stu-id="d2bef-133">You can abbreviate hello command too`helm ls`.</span></span>
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a><span data-ttu-id="d2bef-134">Distribuera ett MariaDB diagram och klient</span><span class="sxs-lookup"><span data-stu-id="d2bef-134">Deploy a MariaDB chart and client</span></span>

<span data-ttu-id="d2bef-135">Nu distribuera MariaDB diagram och en MariaDB klienten tooconnect toohello databas.</span><span class="sxs-lookup"><span data-stu-id="d2bef-135">Now deploy a MariaDB chart and a MariaDB client tooconnect toohello database.</span></span>

<span data-ttu-id="d2bef-136">toodeploy hello MariaDB diagram, typen hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2bef-136">toodeploy hello MariaDB chart, type hello following command:</span></span>

```bash
helm install --name v1 stable/mariadb
```

<span data-ttu-id="d2bef-137">där `--name` är en tagg som används för versioner.</span><span class="sxs-lookup"><span data-stu-id="d2bef-137">where `--name` is a tag used for releases.</span></span>

> [!TIP]
> <span data-ttu-id="d2bef-138">Om hello distributionen misslyckas kör `helm repo update` och försök igen.</span><span class="sxs-lookup"><span data-stu-id="d2bef-138">If hello deployment fails, run `helm repo update` and try again.</span></span>
>
 
 
<span data-ttu-id="d2bef-139">tooview alla hello diagram som har distribuerats på klustret, typ:</span><span class="sxs-lookup"><span data-stu-id="d2bef-139">tooview all hello charts deployed on your cluster, type:</span></span>

```bash 
helm list
```
 
<span data-ttu-id="d2bef-140">tooview skriver du alla distributioner som körs på klustret:</span><span class="sxs-lookup"><span data-stu-id="d2bef-140">tooview all deployments running on your cluster, type:</span></span>

```bash
kubectl get deployments 
``` 
 
 
<span data-ttu-id="d2bef-141">Slutligen toorun baljor tooaccess hello klienten, skriver du:</span><span class="sxs-lookup"><span data-stu-id="d2bef-141">Finally, toorun a pod tooaccess hello client, type:</span></span>

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
<span data-ttu-id="d2bef-142">tooconnect toohello klienten, typen hello följande kommando, där du ersätter `v1-mariadb` med hello namn för din distribution:</span><span class="sxs-lookup"><span data-stu-id="d2bef-142">tooconnect toohello client, type hello following command, replacing `v1-mariadb` with hello name of your deployment:</span></span>

```bash
sudo mysql –h v1-mariadb
```
 
 
<span data-ttu-id="d2bef-143">Du kan nu använda standard toocreate databaser för SQL-kommandon, tabeller osv. Till exempel `Create DATABASE testdb1;` skapar en tom databas.</span><span class="sxs-lookup"><span data-stu-id="d2bef-143">You can now use standard SQL commands toocreate databases, tables, etc. For example, `Create DATABASE testdb1;` creates an empty database.</span></span> 
 
 
 
## <a name="next-steps"></a><span data-ttu-id="d2bef-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2bef-144">Next steps</span></span>

* <span data-ttu-id="d2bef-145">Mer information om hur du hanterar Kubernetes diagram finns hello [Helm dokumentationen](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span><span class="sxs-lookup"><span data-stu-id="d2bef-145">For more information about managing Kubernetes charts, see hello [Helm documentation](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span></span> 

