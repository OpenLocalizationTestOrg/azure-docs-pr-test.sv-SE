---
title: "Distribuera behållare med Helm i Azure Kubernetes | Microsoft Docs"
description: "Verktyget Helm paketering ska distribuera behållare på ett Kubernetes kluster i Azure Container Service"
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
ms.openlocfilehash: 3cfcc5abbee03ca8fbbec4e4eae711e7c2d9deae
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-helm-to-deploy-containers-on-a-kubernetes-cluster"></a><span data-ttu-id="af271-103">Använda Helm för att distribuera behållare på ett Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="af271-103">Use Helm to deploy containers on a Kubernetes cluster</span></span> 

<span data-ttu-id="af271-104">[Helm](https://github.com/kubernetes/helm/) är en öppen källkod paketering verktyg som hjälper dig att installera och hantera livscykeln för Kubernetes program.</span><span class="sxs-lookup"><span data-stu-id="af271-104">[Helm](https://github.com/kubernetes/helm/) is an open-source packaging tool that helps you install and manage the lifecycle of Kubernetes applications.</span></span> <span data-ttu-id="af271-105">Liknar Linux paketet chefer som lgh get och Yum, Helm används för att hantera Kubernetes diagram som är paket med förkonfigurerade Kubernetes resurser.</span><span class="sxs-lookup"><span data-stu-id="af271-105">Similar to Linux package managers such as Apt-get and Yum, Helm is used to manage Kubernetes charts, which are packages of preconfigured Kubernetes resources.</span></span> <span data-ttu-id="af271-106">Den här artikeln visar hur du arbetar med Helm på ett Kubernetes kluster som distribueras i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="af271-106">This article shows you how to work with Helm on a Kubernetes cluster deployed in Azure Container Service.</span></span>

<span data-ttu-id="af271-107">Helm består av två komponenter:</span><span class="sxs-lookup"><span data-stu-id="af271-107">Helm has two components:</span></span> 
* <span data-ttu-id="af271-108">Den **Helm CLI** är en klient som körs på datorn lokalt eller i molnet</span><span class="sxs-lookup"><span data-stu-id="af271-108">The **Helm CLI** is a client that runs on your machine locally or in the cloud</span></span>  

* <span data-ttu-id="af271-109">**Rorkulten** är en server som körs på klustret Kubernetes och hanterar livscykeln för Kubernetes-program</span><span class="sxs-lookup"><span data-stu-id="af271-109">**Tiller** is a server that runs on the Kubernetes cluster and manages the lifecycle of your Kubernetes applications</span></span> 
 
## <a name="prerequisites"></a><span data-ttu-id="af271-110">Krav</span><span class="sxs-lookup"><span data-stu-id="af271-110">Prerequisites</span></span>

* <span data-ttu-id="af271-111">[Skapa ett kluster med Kubernetes](container-service-kubernetes-walkthrough.md) i Azure Container Service</span><span class="sxs-lookup"><span data-stu-id="af271-111">[Create a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>

* <span data-ttu-id="af271-112">[Installera och konfigurera `kubectl` ](../container-service-connect.md) på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="af271-112">[Install and configure `kubectl`](../container-service-connect.md) on a local computer</span></span>

* <span data-ttu-id="af271-113">[Installera Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) på en lokal dator</span><span class="sxs-lookup"><span data-stu-id="af271-113">[Install Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) on a local computer</span></span>

## <a name="helm-basics"></a><span data-ttu-id="af271-114">Helm grunderna</span><span class="sxs-lookup"><span data-stu-id="af271-114">Helm basics</span></span> 

<span data-ttu-id="af271-115">Om du vill visa information om Kubernetes klustret att du installerar rorkulten och distribuera ditt program till, skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af271-115">To view information about the Kubernetes cluster that you are installing Tiller and deploying your applications to, type the following command:</span></span>

```bash
kubectl cluster-info 
```
![kubectl klusterinformation](./media/container-service-kubernetes-helm/clusterinfo.png)
 
<span data-ttu-id="af271-117">När du har installerat Helm installera rorkulten på Kubernetes klustret genom att skriva följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af271-117">After you have installed Helm, install Tiller on your Kubernetes cluster by typing the following command:</span></span>

```bash
helm init --upgrade
```
<span data-ttu-id="af271-118">När den är klar kan du se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="af271-118">When it completes successfully, you see output like the following:</span></span>

![Rorkulten installation](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
<span data-ttu-id="af271-120">Om du vill visa alla Helm-diagram som finns tillgängliga i databasen, skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af271-120">To view all the Helm charts available in the repository, type the following command:</span></span>

```bash 
helm search 
```

<span data-ttu-id="af271-121">Du kan se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="af271-121">You see output like the following:</span></span>

![Helm sökning](./media/container-service-kubernetes-helm/helm-search.png)
 
<span data-ttu-id="af271-123">Om du vill uppdatera diagram om du vill hämta den senaste versionen, skriver du:</span><span class="sxs-lookup"><span data-stu-id="af271-123">To update the charts to get the latest versions, type:</span></span>

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a><span data-ttu-id="af271-124">Distribuera ett Nginx ingång controller diagram</span><span class="sxs-lookup"><span data-stu-id="af271-124">Deploy an Nginx ingress controller chart</span></span> 
 
<span data-ttu-id="af271-125">Om du vill distribuera ett Nginx ingång controller diagram, skriver du ett enda kommando:</span><span class="sxs-lookup"><span data-stu-id="af271-125">To deploy an Nginx ingress controller chart, type a single command:</span></span>

```bash
helm install stable/nginx-ingress 
```
![Distribuera ingång-styrenhet](./media/container-service-kubernetes-helm/nginx-ingress.png)

<span data-ttu-id="af271-127">Om du anger `kubectl get svc` om du vill visa alla tjänster som körs på klustret som du ser att en IP-adress har tilldelats ingång-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="af271-127">If you type `kubectl get svc` to view all services that are running on the cluster, you see that an IP address is assigned to the ingress controller.</span></span> <span data-ttu-id="af271-128">(När tilldelningen pågår kan du se `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="af271-128">(While the assignment is in progress, you see `<pending>`.</span></span> <span data-ttu-id="af271-129">Det tar några minuter att slutföra.)</span><span class="sxs-lookup"><span data-stu-id="af271-129">It takes a couple of minutes to complete.)</span></span> 

<span data-ttu-id="af271-130">Efter IP-adress tilldelas, navigerar till värdet för externa IP-adressen finns Nginx-backend som körs.</span><span class="sxs-lookup"><span data-stu-id="af271-130">After the IP address is assigned, navigate to the value of the external IP address to see the Nginx backend running.</span></span> 
 
![Ingång IP-adress](./media/container-service-kubernetes-helm/ingress-ip-address.png)


<span data-ttu-id="af271-132">Om du vill se en lista över diagram som installerats på klustret, skriver du:</span><span class="sxs-lookup"><span data-stu-id="af271-132">To see a list of charts installed on your cluster, type:</span></span>

```bash
helm list 
```

<span data-ttu-id="af271-133">Du kan förkorta kommandot till `helm ls`.</span><span class="sxs-lookup"><span data-stu-id="af271-133">You can abbreviate the command to `helm ls`.</span></span>
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a><span data-ttu-id="af271-134">Distribuera ett MariaDB diagram och klient</span><span class="sxs-lookup"><span data-stu-id="af271-134">Deploy a MariaDB chart and client</span></span>

<span data-ttu-id="af271-135">Nu distribuera MariaDB diagram och en MariaDB klient ansluta till databasen.</span><span class="sxs-lookup"><span data-stu-id="af271-135">Now deploy a MariaDB chart and a MariaDB client to connect to the database.</span></span>

<span data-ttu-id="af271-136">Om du vill distribuera MariaDB diagram, skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="af271-136">To deploy the MariaDB chart, type the following command:</span></span>

```bash
helm install --name v1 stable/mariadb
```

<span data-ttu-id="af271-137">där `--name` är en tagg som används för versioner.</span><span class="sxs-lookup"><span data-stu-id="af271-137">where `--name` is a tag used for releases.</span></span>

> [!TIP]
> <span data-ttu-id="af271-138">Om distributionen av misslyckas kör `helm repo update` och försök igen.</span><span class="sxs-lookup"><span data-stu-id="af271-138">If the deployment fails, run `helm repo update` and try again.</span></span>
>
 
 
<span data-ttu-id="af271-139">Om du vill visa alla diagram som distribuerats på klustret, skriver du:</span><span class="sxs-lookup"><span data-stu-id="af271-139">To view all the charts deployed on your cluster, type:</span></span>

```bash 
helm list
```
 
<span data-ttu-id="af271-140">Om du vill visa alla distributioner som körs på klustret, skriver du:</span><span class="sxs-lookup"><span data-stu-id="af271-140">To view all deployments running on your cluster, type:</span></span>

```bash
kubectl get deployments 
``` 
 
 
<span data-ttu-id="af271-141">Slutligen, om du vill köra en baljor för att komma åt klienten, skriver du:</span><span class="sxs-lookup"><span data-stu-id="af271-141">Finally, to run a pod to access the client, type:</span></span>

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
<span data-ttu-id="af271-142">För att ansluta till klienten, Skriv följande kommando ersätter `v1-mariadb` med namnet på din distribution:</span><span class="sxs-lookup"><span data-stu-id="af271-142">To connect to the client, type the following command, replacing `v1-mariadb` with the name of your deployment:</span></span>

```bash
sudo mysql –h v1-mariadb
```
 
 
<span data-ttu-id="af271-143">Du kan nu använda standard SQL-kommandon för att skapa databaser, tabeller osv. Till exempel `Create DATABASE testdb1;` skapar en tom databas.</span><span class="sxs-lookup"><span data-stu-id="af271-143">You can now use standard SQL commands to create databases, tables, etc. For example, `Create DATABASE testdb1;` creates an empty database.</span></span> 
 
 
 
## <a name="next-steps"></a><span data-ttu-id="af271-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af271-144">Next steps</span></span>

* <span data-ttu-id="af271-145">Mer information om hur du hanterar Kubernetes diagram finns det [Helm dokumentationen](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span><span class="sxs-lookup"><span data-stu-id="af271-145">For more information about managing Kubernetes charts, see the [Helm documentation](https://github.com/kubernetes/helm/blob/master/docs/index.md).</span></span> 

