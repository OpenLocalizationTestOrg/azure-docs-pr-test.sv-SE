---
title: aaaUse utkast med Azure Container Service och Azure Container register | Microsoft Docs
description: "Skapa ett Kubernetes ACS-kluster och ett Azure Container registret toocreate ditt första program i Azure med utkastet."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: Docker, Containers, microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: f5e21cda01e5e8452bf86a5c8fa458904d89f451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-toobuild-and-deploy-an-application-tookubernetes"></a><span data-ttu-id="bd0c5-104">Använd utkast med Azure Container Service och Azure Container registret toobuild och distribuera ett program tooKubernetes</span><span class="sxs-lookup"><span data-stu-id="bd0c5-104">Use Draft with Azure Container Service and Azure Container Registry toobuild and deploy an application tooKubernetes</span></span>

<span data-ttu-id="bd0c5-105">[Utkast](https://aka.ms/draft) är ett nytt verktyg för öppen källkod som gör det enkelt toodevelop behållaren-baserade program och distribuera dem tooKubernetes kluster utan att känna till mycket om Docker och Kubernetes-- eller installera dem även.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-105">[Draft](https://aka.ms/draft) is a new open-source tool that makes it easy toodevelop container-based applications and deploy them tooKubernetes clusters without knowing much about Docker and Kubernetes -- or even installing them.</span></span> <span data-ttu-id="bd0c5-106">Med hjälp av verktyg som utkast gör att du och ditt team fokus bygga hello program med Kubernetes, inte betalar så mycket uppmärksamhet tooinfrastructure.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-106">Using tools like Draft let you and your teams focus on building hello application with Kubernetes, not paying as much attention tooinfrastructure.</span></span>

<span data-ttu-id="bd0c5-107">Du kan använda Draft med alla Docker-avbildningsregister och Kubernetes-kluster, även lokalt.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-107">You can use Draft with any Docker image registry and any Kubernetes cluster, including locally.</span></span> <span data-ttu-id="bd0c5-108">Den här kursen visar hur toouse ACS med Kubernetes, ACR och Azure DNS toocreate live CI/CD-utvecklare pipeline med utkastet.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-108">This tutorial shows how toouse ACS with Kubernetes, ACR, and Azure DNS toocreate a live CI/CD developer pipeline using Draft.</span></span>


## <a name="create-an-azure-container-registry"></a><span data-ttu-id="bd0c5-109">Skapa ett Azure Container Registry</span><span class="sxs-lookup"><span data-stu-id="bd0c5-109">Create an Azure Container Registry</span></span>
<span data-ttu-id="bd0c5-110">Du kan enkelt [skapa en ny Azure-behållare registernyckel](../../container-registry/container-registry-get-started-azure-cli.md), men hello stegen är som följer:</span><span class="sxs-lookup"><span data-stu-id="bd0c5-110">You can easily [create a new Azure Container Registry](../../container-registry/container-registry-get-started-azure-cli.md), but hello steps are as follows:</span></span>

1. <span data-ttu-id="bd0c5-111">Skapa en Azure-resurs grupp toomanage ACR registret och hello Kubernetes klustret i ACS.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-111">Create a Azure resource group toomanage your ACR registry and hello Kubernetes cluster in ACS.</span></span>
      ```azurecli
      az group create --name draft --location eastus
      ```

2. <span data-ttu-id="bd0c5-112">Skapa ett ACR-avbildningsregister med hjälp av [az acr create](/cli/azure/acr#create)</span><span class="sxs-lookup"><span data-stu-id="bd0c5-112">Create an ACR image registry using [az acr create](/cli/azure/acr#create)</span></span>
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a><span data-ttu-id="bd0c5-113">Skapa en Azure Container Service med Kubernetes</span><span class="sxs-lookup"><span data-stu-id="bd0c5-113">Create an Azure Container Service with Kubernetes</span></span>

<span data-ttu-id="bd0c5-114">Nu är du redo toouse [az acs skapa](/cli/azure/acs#create) toocreate en ACS-kluster med hjälp av Kubernetes som hello `--orchestrator-type` värde.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-114">Now you're ready toouse [az acs create](/cli/azure/acs#create) toocreate an ACS cluster using Kubernetes as hello `--orchestrator-type` value.</span></span>
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> <span data-ttu-id="bd0c5-115">Eftersom Kubernetes inte hello standardtypen för orchestrator, bör du använda hello `--orchestrator-type kubernetes` växla.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-115">Because Kubernetes is not hello default orchestrator type, be sure you use hello `--orchestrator-type kubernetes` switch.</span></span>

<span data-ttu-id="bd0c5-116">hello utdata när lyckade verkar liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-116">hello output when successful looks similar toohello following.</span></span>

```json
waiting for AAD role toopropagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

<span data-ttu-id="bd0c5-117">Nu när du har ett kluster, kan du importera hello autentiseringsuppgifter med hello [az acs kubernetes get-autentiseringsuppgifter](/cli/azure/acs/kubernetes#get-credentials) kommando.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-117">Now that you have a cluster, you can import hello credentials by using hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="bd0c5-118">Nu har du en lokal fil för klustret, vilket är vilka Helm och utkast måste tooget sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-118">Now you have a local configuration file for your cluster, which is what Helm and Draft need tooget their work done.</span></span>

## <a name="install-and-configure-draft"></a><span data-ttu-id="bd0c5-119">Installera och konfigurera Draft</span><span class="sxs-lookup"><span data-stu-id="bd0c5-119">Install and configure draft</span></span>
<span data-ttu-id="bd0c5-120">hello Installationsinstruktioner för utkast finns i hello [utkast databasen](https://github.com/Azure/draft/blob/master/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="bd0c5-120">hello installation instructions for Draft are in hello [Draft repository](https://github.com/Azure/draft/blob/master/docs/install.md).</span></span> <span data-ttu-id="bd0c5-121">De är relativt enkla men kräver viss konfiguration eftersom det beror på [Helm](https://aka.ms/helm) toocreate och distribuera ett Helm diagram i hello Kubernetes kluster.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-121">They are relatively simple, but do require some configuration, as it depends on [Helm](https://aka.ms/helm) toocreate and deploy a Helm chart into hello Kubernetes cluster.</span></span>

1. <span data-ttu-id="bd0c5-122">[Ladda ned och installera Helm](https://aka.ms/helm#install).</span><span class="sxs-lookup"><span data-stu-id="bd0c5-122">[Download and install Helm](https://aka.ms/helm#install).</span></span>
2. <span data-ttu-id="bd0c5-123">Använd Helm toosearch för och installera `stable/traefik`, och meddelanden om ingångs-styrenhet tooenable inkommande begäranden för din versioner.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-123">Use Helm toosearch for and install `stable/traefik`, and ingress controller tooenable inbound requests for your builds.</span></span>
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    <span data-ttu-id="bd0c5-124">Ange en klockan på hello `ingress` domänkontrollant toocapture hello externa IP-värdet när den distribueras.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-124">Now set a watch on hello `ingress` controller toocapture hello external IP value when it is deployed.</span></span> <span data-ttu-id="bd0c5-125">Den här IP-adressen blir hello en [mappas tooyour distribution domän](#wire-up-deployment-domain) i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-125">This IP address will be hello one [mapped tooyour deployment domain](#wire-up-deployment-domain) in hello next section.</span></span>

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    <span data-ttu-id="bd0c5-126">I det här fallet hello extern IP för hello distribution domän är `13.64.108.240`.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-126">In this case, hello external IP for hello deployment domain is `13.64.108.240`.</span></span> <span data-ttu-id="bd0c5-127">Nu kan du mappa dina toothat IP-domän.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-127">Now you can map your domain toothat IP.</span></span>

## <a name="wire-up-deployment-domain"></a><span data-ttu-id="bd0c5-128">Ställa in distributionsdomän</span><span class="sxs-lookup"><span data-stu-id="bd0c5-128">Wire up deployment domain</span></span>

<span data-ttu-id="bd0c5-129">Draft skapar en version för varje Helm-diagram som skapas (varje program du arbetar med).</span><span class="sxs-lookup"><span data-stu-id="bd0c5-129">Draft creates a release for each Helm chart it creates -- each application you are working on.</span></span> <span data-ttu-id="bd0c5-130">Var och en hämtar ett genererat namn som används av ett utkast till som en _underdomän_ ovanpå hello rot _distribution domän_ som du bestämmer.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-130">Each one gets a generated name that is used by draft as a _subdomain_ on top of hello root _deployment domain_ that you control.</span></span> <span data-ttu-id="bd0c5-131">(I det här exemplet använder vi `squillace.io` som hello distribution domän.) tooenable problemet underdomän måste du skapa en A-post för `'*'` i din DNS-posterna för din distribution domän så att alla genererade underdomän är routade toohello Kubernetes klustrets ingång-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-131">(In this example, we use `squillace.io` as hello deployment domain.) tooenable this subdomain behavior, you must create an A record for `'*'` in your DNS entries for your deployment domain, so that each generated subdomain is routed toohello Kubernetes cluster's ingress controller.</span></span>

<span data-ttu-id="bd0c5-132">En egen domän-provider har egna sätt tooassign DNS-servrar; för[Delegera din domän nameservers tooAzure DNS](../../dns/dns-delegate-domain-azure-dns.md), du vidta hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd0c5-132">Your own domain provider has their own way tooassign DNS servers; too[delegate your domain nameservers tooAzure DNS](../../dns/dns-delegate-domain-azure-dns.md), you take hello following steps:</span></span>

1. <span data-ttu-id="bd0c5-133">Skapa en resursgrupp för din zon.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-133">Create a resource group for your zone.</span></span>
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. <span data-ttu-id="bd0c5-134">Skapa en DNS-zon för din domän.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-134">Create a DNS zone for your domain.</span></span>
<span data-ttu-id="bd0c5-135">Använd hello [az nätverket DNS-zon skapa](/cli/azure/network/dns/zone#create) kommandot tooobtain hello nameservers toodelegate DNS styra tooAzure DNS för din domän.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-135">Use hello [az network dns zone create](/cli/azure/network/dns/zone#create) command tooobtain hello nameservers toodelegate DNS control tooAzure DNS for your domain.</span></span>
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. <span data-ttu-id="bd0c5-136">Lägg till hello DNS-servrar du ges toohello domän provider för din distribution domän där du toouse Azure DNS toorepoint din domän som du vill.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-136">Add hello DNS servers you are given toohello domain provider for your deployment domain, which enables you toouse Azure DNS toorepoint your domain as you want.</span></span>
4. <span data-ttu-id="bd0c5-137">Skapa en A-postuppsättningen post för din distribution domän mappning toohello `ingress` IP från steg 2 av hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-137">Create an A record-set entry for your deployment domain mapping toohello `ingress` IP from step 2 of hello previous section.</span></span>
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
<span data-ttu-id="bd0c5-138">hello utdata ser ut ungefär så:</span><span class="sxs-lookup"><span data-stu-id="bd0c5-138">hello output looks something like:</span></span>
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. <span data-ttu-id="bd0c5-139">Konfigurera utkast toouse registret och skapa underdomäner för varje Helm diagram skapas.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-139">Configure Draft toouse your registry and create subdomains for each Helm chart it creates.</span></span> <span data-ttu-id="bd0c5-140">tooconfigure utkast, behöver du:</span><span class="sxs-lookup"><span data-stu-id="bd0c5-140">tooconfigure Draft, you need:</span></span>
  - <span data-ttu-id="bd0c5-141">ditt Azure Container Registry-namn (`draft` i det här exemplet)</span><span class="sxs-lookup"><span data-stu-id="bd0c5-141">your Azure Container Registry name (in this example, `draft`)</span></span>
  - <span data-ttu-id="bd0c5-142">din registernyckel, eller ditt lösenord, från `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-142">your registry key, or password, from `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span></span>
  - <span data-ttu-id="bd0c5-143">hello distribution rotdomänen som du har konfigurerat toomap toohello Kubernetes ingång extern IP-adress (här, `squillace.io`)</span><span class="sxs-lookup"><span data-stu-id="bd0c5-143">hello root deployment domain that you have configured toomap toohello Kubernetes ingress external IP address (here, `squillace.io`)</span></span>

  <span data-ttu-id="bd0c5-144">Anropa `draft init` och hello konfigurationsprocessen efterfrågar hello värdena ovan.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-144">Call `draft init` and hello configuration process prompts you for hello values above.</span></span> <span data-ttu-id="bd0c5-145">hello processen ser ut ungefär som följande hello hello första gången du kör den.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-145">hello process looks something like hello following hello first time you run it.</span></span>
 ```bash
    $ draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order tooinstall Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

<span data-ttu-id="bd0c5-146">Nu är du redo toodeploy ett program.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-146">Now you're ready toodeploy an application.</span></span>


## <a name="build-and-deploy-an-application"></a><span data-ttu-id="bd0c5-147">Skapa och distribuera ett program</span><span class="sxs-lookup"><span data-stu-id="bd0c5-147">Build and deploy an application</span></span>

<span data-ttu-id="bd0c5-148">I hello utkast lagringsplatsen [sex enkelt exempelprogram](https://github.com/Azure/draft/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="bd0c5-148">In hello Draft repo are [six simple example applications](https://github.com/Azure/draft/tree/master/examples).</span></span> <span data-ttu-id="bd0c5-149">Klona lagringsplatsen hello och vi ska använda hello [Python exempel](https://github.com/Azure/draft/tree/master/examples/python).</span><span class="sxs-lookup"><span data-stu-id="bd0c5-149">Clone hello repo and let's use hello [Python example](https://github.com/Azure/draft/tree/master/examples/python).</span></span> <span data-ttu-id="bd0c5-150">Ändra till katalogen för hello exempel/Python och skriv `draft create` toobuild hello program.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-150">Change into hello examples/Python directory, and type `draft create` toobuild hello application.</span></span> <span data-ttu-id="bd0c5-151">Det bör se ut som följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-151">It should look like hello following example.</span></span>
```bash
$ draft create
--> Python app detected
--> Ready toosail
```

<span data-ttu-id="bd0c5-152">hello utdata innehåller en Dockerfile och ett Helm diagram.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-152">hello output includes a Dockerfile and a Helm chart.</span></span> <span data-ttu-id="bd0c5-153">toobuild och distribuera kan du börja skriva `draft up`.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-153">toobuild and deploy, you just type `draft up`.</span></span> <span data-ttu-id="bd0c5-154">hello-utdata är omfattande, men börjar som hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-154">hello output is extensive, but begins like hello following example.</span></span>
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

<span data-ttu-id="bd0c5-155">och när lyckade avslutas med något liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-155">and when successful ends with something similar toohello following example.</span></span>
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying tooKubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io tooaccess your application

Watching local files for changes...
```

<span data-ttu-id="bd0c5-156">Oavsett ditt diagram namnet är, kan du nu `curl http://gangly-bronco.squillace.io` tooreceive hello svar `Hello World!`.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-156">Whatever your chart's name is, you can now `curl http://gangly-bronco.squillace.io` tooreceive hello reply, `Hello World!`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd0c5-157">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd0c5-157">Next steps</span></span>

<span data-ttu-id="bd0c5-158">Nu när du har ett Kubernetes ACS-kluster kan du undersöka med [Azure Container registret](../../container-registry/container-registry-intro.md) toocreate mer och olika distributioner av det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-158">Now that you have an ACS Kubernetes cluster, you can investigate using [Azure Container Registry](../../container-registry/container-registry-intro.md) toocreate more and different deployments of this scenario.</span></span> <span data-ttu-id="bd0c5-159">Du kan till exempel skapa en DNS-postuppsättning för draft._basedomain.toplevel_ som styr saker från en djupare underdomän för specifika ACS-distributioner.</span><span class="sxs-lookup"><span data-stu-id="bd0c5-159">For example, you can create a draft._basedomain.toplevel_ domain DNS record-set that controls things off of a deeper subdomain for specific ACS deployments.</span></span>






