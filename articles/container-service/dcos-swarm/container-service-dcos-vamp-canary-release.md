---
title: "Kanarieöarna version med Vamp på Azure DC/OS-kluster | Microsoft Docs"
description: "Hur du använder Vamp till Kanarieöarna versionen tjänster och tillämpa smart trafik filtrering på ett Azure Container Service DC/OS-kluster"
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 4a20091b59f2643ea71cce99c159a5075706e35d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="30bc7-103">Kanarieöarna versionen mikrotjänster med Vamp på ett Azure Container Service DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="30bc7-103">Canary release microservices with Vamp on an Azure Container Service DC/OS cluster</span></span>

<span data-ttu-id="30bc7-104">I den här genomgången ska konfigurera vi Vamp på Azure Container Service med ett DC/OS-kluster.</span><span class="sxs-lookup"><span data-stu-id="30bc7-104">In this walkthrough, we set up Vamp on Azure Container Service with a DC/OS cluster.</span></span> <span data-ttu-id="30bc7-105">Vi Kanarieöarna släpp Vamp demo tjänsten ”sava” och sedan matcha inkompatibilitet med Firefox genom att använda smart trafikfiltrering.</span><span class="sxs-lookup"><span data-stu-id="30bc7-105">We canary release the Vamp demo service "sava", and then resolve an incompatibility of the service with Firefox by applying smart traffic filtering.</span></span> 

> [!TIP] 
> <span data-ttu-id="30bc7-106">I den här genomgången Vamp körs på en DC/OS-klustret, men du kan också använda Vamp med Kubernetes som orchestrator.</span><span class="sxs-lookup"><span data-stu-id="30bc7-106">In this walkthrough Vamp runs on a DC/OS cluster, but you can also use Vamp with Kubernetes as the orchestrator.</span></span>
>

## <a name="about-canary-releases-and-vamp"></a><span data-ttu-id="30bc7-107">Om Kanarieöarna versioner och Vamp</span><span class="sxs-lookup"><span data-stu-id="30bc7-107">About canary releases and Vamp</span></span>


<span data-ttu-id="30bc7-108">[Kanarieöarna släppa](https://martinfowler.com/bliki/CanaryRelease.html) är en smart distributionsstrategi antas av innovativa organisationer som Netflix, Facebook och Spotify.</span><span class="sxs-lookup"><span data-stu-id="30bc7-108">[Canary releasing](https://martinfowler.com/bliki/CanaryRelease.html) is a smart deployment strategy adopted by innovative organizations like Netflix, Facebook, and Spotify.</span></span> <span data-ttu-id="30bc7-109">Det är en lösning som passar eftersom det minskar problem, introducerar säkerhet nät och ökar innovation.</span><span class="sxs-lookup"><span data-stu-id="30bc7-109">It’s an approach that makes sense, because it reduces issues, introduces safety-nets, and increases innovation.</span></span> <span data-ttu-id="30bc7-110">Så varför inte alla företag som använder den?</span><span class="sxs-lookup"><span data-stu-id="30bc7-110">So why aren’t all companies using it?</span></span> <span data-ttu-id="30bc7-111">Utöka en CI/CD-pipeline för att inkludera Kanarieöarna strategier lägger till komplexitet och kräver omfattande devops kunskap och erfarenhet.</span><span class="sxs-lookup"><span data-stu-id="30bc7-111">Extending a CI/CD pipeline to include canary strategies adds complexity, and requires extensive devops knowledge and experience.</span></span> <span data-ttu-id="30bc7-112">Det är tillräckligt för att blockera mindre företag och företag som är både innan de även komma igång.</span><span class="sxs-lookup"><span data-stu-id="30bc7-112">That’s enough to block smaller companies and enterprises alike before they even get started.</span></span> 

<span data-ttu-id="30bc7-113">[Vamp](http://vamp.io/) lanserar ett system för öppen källkod som utformats för att underlätta övergången och sätta Kanarieöarna funktioner till din önskade behållaren Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="30bc7-113">[Vamp](http://vamp.io/) is an open-source system designed to ease this transition and bring canary releasing features to your preferred container scheduler.</span></span> <span data-ttu-id="30bc7-114">Vamps Kanarieöarna funktioner går utöver procent-baserade installationer.</span><span class="sxs-lookup"><span data-stu-id="30bc7-114">Vamp’s canary functionality goes beyond percentage-based rollouts.</span></span> <span data-ttu-id="30bc7-115">Trafik kan filtreras och dela på en mängd olika villkor, till exempel till målet specifika användare, IP-adressintervall eller enheter.</span><span class="sxs-lookup"><span data-stu-id="30bc7-115">Traffic can be filtered and split on a wide range of conditions, for example to target specific users, IP-ranges, or devices.</span></span> <span data-ttu-id="30bc7-116">Vamp spårar och analyserar prestandamått, så att för automatisering som bygger på verkliga data.</span><span class="sxs-lookup"><span data-stu-id="30bc7-116">Vamp tracks and analyzes performance metrics, allowing for automation based on real-world data.</span></span> <span data-ttu-id="30bc7-117">Du kan ställa in automatisk återställning vid fel eller skala enskild tjänst varianter utifrån belastningen eller svarstid.</span><span class="sxs-lookup"><span data-stu-id="30bc7-117">You can set up automatic rollback on errors, or scale individual service variants based on load or latency.</span></span>

## <a name="set-up-azure-container-service-with-dcos"></a><span data-ttu-id="30bc7-118">Konfigurera Azure Container Service med DC/OS</span><span class="sxs-lookup"><span data-stu-id="30bc7-118">Set up Azure Container Service with DC/OS</span></span>



1. <span data-ttu-id="30bc7-119">[Distribuera ett DC/OS-kluster](container-service-deployment.md) med en överordnad och två agenterna av standardstorleken.</span><span class="sxs-lookup"><span data-stu-id="30bc7-119">[Deploy a DC/OS cluster](container-service-deployment.md) with one master and two agents of default size.</span></span> 

2. <span data-ttu-id="30bc7-120">[Skapa en SSH-tunnel](../container-service-connect.md) att ansluta till DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="30bc7-120">[Create an SSH tunnel](../container-service-connect.md) to connect to the DC/OS cluster.</span></span> <span data-ttu-id="30bc7-121">Den här artikeln förutsätter att du tunnel till klustret på lokal port 80.</span><span class="sxs-lookup"><span data-stu-id="30bc7-121">This article assumes that you tunnel to the cluster on local port 80.</span></span>


## <a name="set-up-vamp"></a><span data-ttu-id="30bc7-122">Ställ in Vamp</span><span class="sxs-lookup"><span data-stu-id="30bc7-122">Set up Vamp</span></span>

<span data-ttu-id="30bc7-123">Nu när du har ett DC/OS-kluster som körs kan du installera Vamp från DC/OS-Gränssnittet (http://localhost:80).</span><span class="sxs-lookup"><span data-stu-id="30bc7-123">Now that you have a running DC/OS cluster, you can install Vamp from the DC/OS UI (http://localhost:80).</span></span> 

![DC/OS-gränssnitt:](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

<span data-ttu-id="30bc7-125">Installationen sker i två steg:</span><span class="sxs-lookup"><span data-stu-id="30bc7-125">Installation is done in two stages:</span></span>

1. <span data-ttu-id="30bc7-126">**Distribuera Elasticsearch**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-126">**Deploy Elasticsearch**.</span></span>

2. <span data-ttu-id="30bc7-127">Sedan **distribuera Vamp** genom att installera paketet Vamp DC/OS universe.</span><span class="sxs-lookup"><span data-stu-id="30bc7-127">Then **deploy Vamp** by installing the Vamp DC/OS universe package.</span></span>

### <a name="deploy-elasticsearch"></a><span data-ttu-id="30bc7-128">Distribuera Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="30bc7-128">Deploy Elasticsearch</span></span>

<span data-ttu-id="30bc7-129">Vamp kräver Elasticsearch för insamling av mätvärden och aggregering.</span><span class="sxs-lookup"><span data-stu-id="30bc7-129">Vamp requires Elasticsearch for metrics collection and aggregation.</span></span> <span data-ttu-id="30bc7-130">Du kan använda den [magneticio Docker bilder](https://hub.docker.com/r/magneticio/elastic/) att distribuera en kompatibel Vamp Elasticsearch stack.</span><span class="sxs-lookup"><span data-stu-id="30bc7-130">You can use the [magneticio Docker images](https://hub.docker.com/r/magneticio/elastic/) to deploy a compatible Vamp Elasticsearch stack.</span></span>

1. <span data-ttu-id="30bc7-131">I DC/OS-Gränssnittet går du till **Services** och på **distribuera tjänst**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-131">In the DC/OS UI, go to **Services** and click **Deploy Service**.</span></span>

2. <span data-ttu-id="30bc7-132">Välj **JSON-läget** från den **distribuera nya tjänster** popup.</span><span class="sxs-lookup"><span data-stu-id="30bc7-132">Select **JSON mode** from the **Deploy New Service** pop-up.</span></span>

  ![Välj JSON-läge](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. <span data-ttu-id="30bc7-134">Klistra in följande JSON.</span><span class="sxs-lookup"><span data-stu-id="30bc7-134">Paste in the following JSON.</span></span> <span data-ttu-id="30bc7-135">Den här konfigurationen kör behållaren med 1 GB RAM-minne och grundläggande hälsokontrollen på Elasticsearch port.</span><span class="sxs-lookup"><span data-stu-id="30bc7-135">This configuration runs the container with 1 GB of RAM and a basic health check on the Elasticsearch port.</span></span>
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. <span data-ttu-id="30bc7-136">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-136">Click **Deploy**.</span></span>

  <span data-ttu-id="30bc7-137">DC/OS distribuerar Elasticsearch behållaren.</span><span class="sxs-lookup"><span data-stu-id="30bc7-137">DC/OS deploys the Elasticsearch container.</span></span> <span data-ttu-id="30bc7-138">Du kan följa förloppet på den **Services** sidan.</span><span class="sxs-lookup"><span data-stu-id="30bc7-138">You can track progress on the **Services** page.</span></span>  

  ![distribuera e? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a><span data-ttu-id="30bc7-140">Distribuera Vamp</span><span class="sxs-lookup"><span data-stu-id="30bc7-140">Deploy Vamp</span></span>

<span data-ttu-id="30bc7-141">När Elasticsearch rapporterar som **kör**, du kan lägga till Universe Vamp DC/OS-paketet.</span><span class="sxs-lookup"><span data-stu-id="30bc7-141">Once Elasticsearch reports as **Running**, you can add the Vamp DC/OS Universe package.</span></span> 

1. <span data-ttu-id="30bc7-142">Gå till **Universe** och Sök efter **vamp**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-142">Go to **Universe** and search for **vamp**.</span></span> 
  <span data-ttu-id="30bc7-143">![Vamp på DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span><span class="sxs-lookup"><span data-stu-id="30bc7-143">![Vamp on DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span></span>

2. <span data-ttu-id="30bc7-144">Klicka på **installera** bredvid vamp paketet och välj **Installation i Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-144">Click **install** next to the vamp package, and choose **Advanced Installation**.</span></span>

3. <span data-ttu-id="30bc7-145">Bläddra nedåt och ange följande elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span><span class="sxs-lookup"><span data-stu-id="30bc7-145">Scroll down and enter the following elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span></span> 

  ![Ange Elasticsearch URL](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. <span data-ttu-id="30bc7-147">Klicka på **granska och installera**, klicka på **installera** att starta distributionen.</span><span class="sxs-lookup"><span data-stu-id="30bc7-147">Click **Review and Install**, then click **Install** to start the deployment.</span></span>  

  <span data-ttu-id="30bc7-148">DC/OS distribuerar alla nödvändiga komponenter för Vamp.</span><span class="sxs-lookup"><span data-stu-id="30bc7-148">DC/OS deploys all required Vamp components.</span></span> <span data-ttu-id="30bc7-149">Du kan följa förloppet på den **Services** sidan.</span><span class="sxs-lookup"><span data-stu-id="30bc7-149">You can track progress on the **Services** page.</span></span>
  
  ![Distribuera Vamp som universe paket](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. <span data-ttu-id="30bc7-151">När distributionen är klar kan du komma åt Vamp-Användargränssnittet:</span><span class="sxs-lookup"><span data-stu-id="30bc7-151">Once deployment has completed, you can access the Vamp UI:</span></span>

  ![Vamp-tjänsten på DC/OS](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp UI](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a><span data-ttu-id="30bc7-154">Distribuera din första tjänst</span><span class="sxs-lookup"><span data-stu-id="30bc7-154">Deploy your first service</span></span>

<span data-ttu-id="30bc7-155">Nu att Vamp är igång kan du distribuera en tjänst från ett utkast.</span><span class="sxs-lookup"><span data-stu-id="30bc7-155">Now that Vamp is up and running, deploy a service from a blueprint.</span></span> 

<span data-ttu-id="30bc7-156">I sin enklaste form är en [Vamp modell](http://vamp.io/documentation/using-vamp/blueprints/) beskriver slutpunkter (gateway), kluster och tjänster för att distribuera.</span><span class="sxs-lookup"><span data-stu-id="30bc7-156">In its simplest form, a [Vamp blueprint](http://vamp.io/documentation/using-vamp/blueprints/) describes the endpoints (gateways), clusters, and services to deploy.</span></span> <span data-ttu-id="30bc7-157">Vamp använder kluster för att gruppera olika varianter av samma tjänst i logiska grupper Kanarieöarna släppa eller A / B-testning.</span><span class="sxs-lookup"><span data-stu-id="30bc7-157">Vamp uses clusters to group different variants of the same service into logical groups for canary releasing or A/B testing.</span></span>  

<span data-ttu-id="30bc7-158">Det här scenariot använder ett monolitisk exempelprogram som kallas [ **sava**](https://github.com/magneticio/sava), vilket är version 1.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-158">This scenario uses a sample monolithic application called [**sava**](https://github.com/magneticio/sava), which is at version 1.0.</span></span> <span data-ttu-id="30bc7-159">Monolitvolym paketeras i en dockerbehållare med som finns i Docker under magneticio/sava:1.0.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-159">The monolith is packaged in a Docker container, which is in Docker Hub under magneticio/sava:1.0.0.</span></span> <span data-ttu-id="30bc7-160">Appen körs normalt på port 8080, men du vill exponera under port 9050 i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="30bc7-160">The app normally runs on port 8080, but you want to expose it under port 9050 in this case.</span></span> <span data-ttu-id="30bc7-161">Distribuera appen via Vamp med en enkel modell.</span><span class="sxs-lookup"><span data-stu-id="30bc7-161">Deploy the app through Vamp using a simple blueprint.</span></span>

1. <span data-ttu-id="30bc7-162">Gå till **distributioner**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-162">Go to **Deployments**.</span></span>

2. <span data-ttu-id="30bc7-163">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-163">Click **Add**.</span></span>

3. <span data-ttu-id="30bc7-164">Klistra in i den följande modell YAML.</span><span class="sxs-lookup"><span data-stu-id="30bc7-164">Paste in the following blueprint YAML.</span></span> <span data-ttu-id="30bc7-165">Det här utkastet innehåller ett kluster med en enda service variant som vi ändras i ett senare steg:</span><span class="sxs-lookup"><span data-stu-id="30bc7-165">This blueprint contains one cluster with only one service variant, which we change in a later step:</span></span>

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster to create
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. <span data-ttu-id="30bc7-166">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-166">Click **Save**.</span></span> <span data-ttu-id="30bc7-167">Vamp initierar distributionen.</span><span class="sxs-lookup"><span data-stu-id="30bc7-167">Vamp initiates the deployment.</span></span>

<span data-ttu-id="30bc7-168">Distributionen finns på den **distributioner** sidan.</span><span class="sxs-lookup"><span data-stu-id="30bc7-168">The deployment is listed on the **Deployments** page.</span></span> <span data-ttu-id="30bc7-169">Klicka om du vill övervaka statusen för distributionen.</span><span class="sxs-lookup"><span data-stu-id="30bc7-169">Click the deployment to monitor its status.</span></span>

![Vamp gränssnitt – distribuera sava](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![sava service i Användargränssnittet för Vamp](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

<span data-ttu-id="30bc7-172">Två gateways skapas, som visas på den **Gateways** sidan:</span><span class="sxs-lookup"><span data-stu-id="30bc7-172">Two gateways are created, which are listed on the **Gateways** page:</span></span>

* <span data-ttu-id="30bc7-173">en stabil slutpunkter för åtkomst till tjänsten körs (9050-port)</span><span class="sxs-lookup"><span data-stu-id="30bc7-173">a stable endpoint to access the running service (port 9050)</span></span> 
* <span data-ttu-id="30bc7-174">en Vamp hanteras internt gateway (mer om den här gatewayen senare).</span><span class="sxs-lookup"><span data-stu-id="30bc7-174">a Vamp-managed internal gateway (more on this gateway later).</span></span> 

![Vamp UI - sava gateways](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

<span data-ttu-id="30bc7-176">Tjänsten sava nu har distribuerats, men du kan inte komma åt den externt eftersom Azure belastningsutjämnare inte vet för att vidarebefordra trafik till det ännu.</span><span class="sxs-lookup"><span data-stu-id="30bc7-176">The sava service has now deployed, but you can’t access it externally because the Azure Load Balancer doesn’t know to forward traffic to it yet.</span></span> <span data-ttu-id="30bc7-177">Uppdatera Azure nätverkskonfigurationen för att komma åt tjänsten.</span><span class="sxs-lookup"><span data-stu-id="30bc7-177">To access the service, update the Azure networking configuration.</span></span>


## <a name="update-the-azure-network-configuration"></a><span data-ttu-id="30bc7-178">Uppdatera konfigurationen av Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="30bc7-178">Update the Azure network configuration</span></span>

<span data-ttu-id="30bc7-179">Vamp distribuerade tjänsten sava på DC/OS-agenten noder, exponera en stabil slutpunkt på port 9050.</span><span class="sxs-lookup"><span data-stu-id="30bc7-179">Vamp deployed the sava service on the DC/OS agent nodes, exposing a stable endpoint at port 9050.</span></span> <span data-ttu-id="30bc7-180">Gör följande ändringar i konfigurationen för Azure-nätverk i klustret distributionen för att komma åt tjänsten från utanför DC/OS-klustret:</span><span class="sxs-lookup"><span data-stu-id="30bc7-180">To access the service from outside the DC/OS cluster, make the following changes to the Azure network configuration in your cluster deployment:</span></span> 

1. <span data-ttu-id="30bc7-181">**Konfigurera Azure belastningsutjämnare** för agenter (resursen med namnet **DC/OS-agent-lb-xxxx**) med en hälsoavsökningen och en regel för att vidarebefordra trafik på port 9050 till sava-instanser.</span><span class="sxs-lookup"><span data-stu-id="30bc7-181">**Configure the Azure Load Balancer** for the agents (the resource named **dcos-agent-lb-xxxx**) with a health probe and a rule to forward traffic on port 9050 to the sava instances.</span></span> 

2. <span data-ttu-id="30bc7-182">**Uppdatera nätverkssäkerhetsgruppen** för offentliga agenter (resursen med namnet **XXXX-agent-offentliga-nsg-XXXX**) som tillåter trafik på port 9050.</span><span class="sxs-lookup"><span data-stu-id="30bc7-182">**Update the network security group** for the public agents (the resource named **XXXX-agent-public-nsg-XXXX**) to allow traffic on port 9050.</span></span>

<span data-ttu-id="30bc7-183">Detaljerade steg för att utföra dessa uppgifter med hjälp av Azure portal, finns [aktivera offentlig åtkomst till ett Azure Container Service-program](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="30bc7-183">For detailed steps to complete these tasks using the Azure portal, see [Enable public access to an Azure Container Service application](container-service-enable-public-access.md).</span></span> <span data-ttu-id="30bc7-184">Ange port 9050 för alla portinställningar.</span><span class="sxs-lookup"><span data-stu-id="30bc7-184">Specify port 9050 for all port settings.</span></span>


<span data-ttu-id="30bc7-185">När allt har skapats, gå till den **översikt** bladet för DC/OS-agentens belastningsutjämnare (resursen med namnet **DC/OS-agent-lb-xxxx**).</span><span class="sxs-lookup"><span data-stu-id="30bc7-185">Once everything has been created, go to the **Overview** blade of the DC/OS agent load balancer (the resource named **dcos-agent-lb-xxxx**).</span></span> <span data-ttu-id="30bc7-186">Hitta de **offentliga IP-adressen**, och Använd adress för att komma åt sava på port 9050.</span><span class="sxs-lookup"><span data-stu-id="30bc7-186">Find the **Public IP address**, and use the address to access sava at port 9050.</span></span>

![Azure portal – hämta offentlig IP-adress](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a><span data-ttu-id="30bc7-189">Kör en version av Kanarieöarna</span><span class="sxs-lookup"><span data-stu-id="30bc7-189">Run a canary release</span></span>

<span data-ttu-id="30bc7-190">Anta att du har en ny version av det här programmet som du vill att Kanarieöarna ut i produktion.</span><span class="sxs-lookup"><span data-stu-id="30bc7-190">Suppose you have a new version of this application that you want to canary release into production.</span></span> <span data-ttu-id="30bc7-191">Du har den behållare som magneticio/sava:1.1.0 och är redo att sätta igång.</span><span class="sxs-lookup"><span data-stu-id="30bc7-191">You have it containerized as magneticio/sava:1.1.0 and are ready to go.</span></span> <span data-ttu-id="30bc7-192">Vamp kan du enkelt lägga till nya tjänster körs distributionen.</span><span class="sxs-lookup"><span data-stu-id="30bc7-192">Vamp lets you easily add new services to the running deployment.</span></span> <span data-ttu-id="30bc7-193">Tjänsterna ”kopplade” distribuerats tillsammans med befintliga tjänster i klustret, och tilldelas en vikt på 0%.</span><span class="sxs-lookup"><span data-stu-id="30bc7-193">These "merged" services are deployed alongside the existing services in the cluster, and assigned a weight of 0%.</span></span> <span data-ttu-id="30bc7-194">Ingen trafik dirigeras till en nyligen kopplade tjänst förrän du justera trafik distribution.</span><span class="sxs-lookup"><span data-stu-id="30bc7-194">No traffic is routed to a newly merged service until you adjust the traffic distribution.</span></span> <span data-ttu-id="30bc7-195">Skjutreglaget vikt i Användargränssnittet för Vamp ger fullständig kontroll över distributionen för inkrementell justeringar (Kanarieöarna utgåva) eller en omedelbar återställning.</span><span class="sxs-lookup"><span data-stu-id="30bc7-195">The weight slider in the Vamp UI gives you complete control over the distribution, allowing for incremental adjustments (canary release) or an instant rollback.</span></span>

### <a name="merge-a-new-service-variant"></a><span data-ttu-id="30bc7-196">Koppla en ny service-variant</span><span class="sxs-lookup"><span data-stu-id="30bc7-196">Merge a new service variant</span></span>

<span data-ttu-id="30bc7-197">Att sammanfoga den nya sava 1.1-tjänsten med körs distributionen:</span><span class="sxs-lookup"><span data-stu-id="30bc7-197">To merge the new sava 1.1 service with the running deployment:</span></span>

1. <span data-ttu-id="30bc7-198">I Användargränssnittet för Vamp klickar du på **ritningarna**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-198">In the Vamp UI, click **Blueprints**.</span></span>

2. <span data-ttu-id="30bc7-199">Klicka på **Lägg till** och klistra in i den följande modell YAML: den här utkast som beskriver en ny service-variant (sava: 1.1.0) ska distribueras i det befintliga klustret (sava_cluster).</span><span class="sxs-lookup"><span data-stu-id="30bc7-199">Click **Add** and paste in the following blueprint YAML: This blueprint describes a new service variant (sava:1.1.0) to deploy within the existing cluster (sava_cluster).</span></span>

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster to update
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint to update
  ```
  
3. <span data-ttu-id="30bc7-200">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-200">Click **Save**.</span></span> <span data-ttu-id="30bc7-201">Modell lagras och visas på den **ritningarna** sidan.</span><span class="sxs-lookup"><span data-stu-id="30bc7-201">The blueprint is stored and listed on the **Blueprints** page.</span></span>

4. <span data-ttu-id="30bc7-202">Öppna åtgärdsmenyn på den sava: 1.1-modell och klicka på **koppla till**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-202">Open the action menu on the sava:1.1 blueprint and click **Merge to**.</span></span>

  ![Vamp UI - ritningarna](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. <span data-ttu-id="30bc7-204">Välj den **sava** distribution och klickar på **sammanfoga**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-204">Select the **sava** deployment and click **Merge**.</span></span>

  ![Vamp UI - merge modell som distributionen](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

<span data-ttu-id="30bc7-206">Vamp distribuerar den nya sava: 1.1.0 service varianten som beskrivs i modell tillsammans med sava: 1.0.0 i den **sava_cluster** för distributionen körs.</span><span class="sxs-lookup"><span data-stu-id="30bc7-206">Vamp deploys the new sava:1.1.0 service variant described in the blueprint alongside sava:1.0.0 in the **sava_cluster** of the running deployment.</span></span> 

![Vamp UI - uppdaterade sava distribution](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

<span data-ttu-id="30bc7-208">Den **webport-sava/sava_cluster** gateway (klusterslutpunkten) uppdateras även att lägga till en väg till nyligen distribuerade sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-208">The **sava/sava_cluster/webport** gateway (the cluster endpoint) is also updated, adding a route to the newly deployed sava:1.1.0.</span></span> <span data-ttu-id="30bc7-209">Nu ingen trafik dirigeras här (den **vikt** anges till 0%).</span><span class="sxs-lookup"><span data-stu-id="30bc7-209">At this point, no traffic is routed here (the **WEIGHT** is set to 0%).</span></span>

![Vamp UI - klustret gateway](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a><span data-ttu-id="30bc7-211">Kanarieöarna versionen</span><span class="sxs-lookup"><span data-stu-id="30bc7-211">Canary release</span></span>

<span data-ttu-id="30bc7-212">Med båda versionerna av sava som distribueras i samma kluster, justera fördelningen av trafiken mellan dem genom att flytta den **vikt** skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="30bc7-212">With both versions of sava deployed in the same cluster, adjust the distribution of traffic between them by moving the **WEIGHT** slider.</span></span>

1. <span data-ttu-id="30bc7-213">Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) bredvid **vikt**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-213">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next to **WEIGHT**.</span></span>

2. <span data-ttu-id="30bc7-214">Ange viktfördelningen till 50/50% och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="30bc7-214">Set the weight distribution to 50%/50% and click **Save**.</span></span>

  ![Vamp UI - gateway vikt skjutreglaget](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. <span data-ttu-id="30bc7-216">Gå tillbaka till din webbläsare och uppdatera sidan sava några gånger.</span><span class="sxs-lookup"><span data-stu-id="30bc7-216">Go back to your browser and refresh the sava page a few more times.</span></span> <span data-ttu-id="30bc7-217">Sava programmet nu växlar mellan en sava: 1.0 och en sava: 1.1-sidan.</span><span class="sxs-lookup"><span data-stu-id="30bc7-217">The sava application now switches between a sava:1.0 page and a sava:1.1 page.</span></span>

  ![Alternerande sava1.0 och sava1.1 tjänster](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > <span data-ttu-id="30bc7-219">Den här växlingar på sidan fungerar bäst med ”Incognito” eller ”Anonymous” läge i webbläsaren på grund av cachelagring av statiska tillgångar.</span><span class="sxs-lookup"><span data-stu-id="30bc7-219">This alternation of the page works best with the "Incognito" or “Anonymous” mode of your browser because of the caching of static assets.</span></span>
  >

### <a name="filter-traffic"></a><span data-ttu-id="30bc7-220">Filtrera trafik</span><span class="sxs-lookup"><span data-stu-id="30bc7-220">Filter traffic</span></span>

<span data-ttu-id="30bc7-221">Anta att efter distributionen du identifierade inkompatibilitet i sava: 1.1.0 att visas i Firefox webbläsare.</span><span class="sxs-lookup"><span data-stu-id="30bc7-221">Suppose after deployment that you discovered an incompatibility in sava:1.1.0 that causes display problems in Firefox browsers.</span></span> <span data-ttu-id="30bc7-222">Du kan ange Vamp att filtrera inkommande trafik och dirigera alla Firefox användare tillbaka till kända stabil sava: 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-222">You can set Vamp to filter incoming traffic and direct all Firefox users back to the known stable sava:1.0.0.</span></span> <span data-ttu-id="30bc7-223">Det här filtret löser omedelbart avbrott för Firefox användare, medan alla andra fortsätter att dra nytta av fördelarna med förbättrad sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-223">This filter instantly resolves the disruption for Firefox users, while everybody else continues to enjoy the benefits of the improved sava:1.1.0.</span></span>

<span data-ttu-id="30bc7-224">Vamp använder **villkor** att filtrera trafik mellan vägar i en gateway.</span><span class="sxs-lookup"><span data-stu-id="30bc7-224">Vamp uses **conditions** to filter traffic between routes in a gateway.</span></span> <span data-ttu-id="30bc7-225">Trafiken först filtreras och dirigeras enligt villkor som används för varje flöde.</span><span class="sxs-lookup"><span data-stu-id="30bc7-225">Traffic is first filtered and directed according to the conditions applied to each route.</span></span> <span data-ttu-id="30bc7-226">Alla återstående trafiken fördelas enligt inställningen gateway vikt.</span><span class="sxs-lookup"><span data-stu-id="30bc7-226">All remaining traffic is distributed according to the gateway weight setting.</span></span>

<span data-ttu-id="30bc7-227">Du kan skapa ett villkor för att filtrera alla Firefox användare och dirigera dem till den gamla sava: 1.0.0:</span><span class="sxs-lookup"><span data-stu-id="30bc7-227">You can create a condition to filter all Firefox users and direct them to the old sava:1.0.0:</span></span>

1. <span data-ttu-id="30bc7-228">På sava/sava_cluster/webport **Gateways** klickar du på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) att lägga till en **VILLKORET** till väg sava/sava_cluster/sava:1.0.0/webport.</span><span class="sxs-lookup"><span data-stu-id="30bc7-228">On the sava/sava_cluster/webport **Gateways** page, click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) to add a **CONDITION** to the route sava/sava_cluster/sava:1.0.0/webport.</span></span> 

2. <span data-ttu-id="30bc7-229">Ange villkoret **användaragent == Firefox** och på ![Vamp UI - spara](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span><span class="sxs-lookup"><span data-stu-id="30bc7-229">Enter the condition **user-agent == Firefox** and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span></span>

  <span data-ttu-id="30bc7-230">Vamp lägger till villkor med en standardvärdet 0%.</span><span class="sxs-lookup"><span data-stu-id="30bc7-230">Vamp adds the condition with a default strength of 0%.</span></span> <span data-ttu-id="30bc7-231">Om du vill starta filtrerar trafik, måste du justera styrkan villkor.</span><span class="sxs-lookup"><span data-stu-id="30bc7-231">To start filtering traffic, you need to adjust the condition strength.</span></span>

3. <span data-ttu-id="30bc7-232">Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) att ändra den **styrkan** tillämpas på villkoret.</span><span class="sxs-lookup"><span data-stu-id="30bc7-232">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) to change the **STRENGTH** applied to the condition.</span></span>
 
4. <span data-ttu-id="30bc7-233">Ange den **styrkan** till 100% och klicka på ![Vamp UI - spara](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) att spara.</span><span class="sxs-lookup"><span data-stu-id="30bc7-233">Set the **STRENGTH** to 100% and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) to save.</span></span>

  <span data-ttu-id="30bc7-234">Vamp nu skickar all trafik som matchar villkoret (alla Firefox användare) till sava: 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-234">Vamp now sends all traffic matching the condition (all Firefox users) to sava:1.0.0.</span></span>

  ![Vamp UI - gäller villkor för gateway](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. <span data-ttu-id="30bc7-236">Slutligen justera gateway vikt för att skicka alla återstående trafik (alla användare med icke-Firefox) till den nya sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-236">Finally, adjust the gateway weight to send all remaining traffic (all non-Firefox users) to the new sava:1.1.0.</span></span> <span data-ttu-id="30bc7-237">Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) bredvid **vikt** och ange viktfördelningen så dirigeras till väg sava/sava_cluster/sava:1.1.0/webport 100%.</span><span class="sxs-lookup"><span data-stu-id="30bc7-237">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next to **WEIGHT** and set the weight distribution so 100% is directed to the route sava/sava_cluster/sava:1.1.0/webport.</span></span>

  <span data-ttu-id="30bc7-238">All trafik som inte filtreras efter villkoret omdirigeras nu till den nya sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-238">All traffic not filtered by the condition is now directed to the new sava:1.1.0.</span></span>

6. <span data-ttu-id="30bc7-239">Öppna två olika webbläsare (en Firefox och en annan webbläsare) och tjänsten sava från både om du vill visa filter i åtgärden.</span><span class="sxs-lookup"><span data-stu-id="30bc7-239">To see the filter in action, open two different browsers (one Firefox and one other browser) and access the sava service from both.</span></span> <span data-ttu-id="30bc7-240">Alla Firefox begäranden skickas till sava: 1.0.0, medan alla andra webbläsare dirigeras till sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="30bc7-240">All Firefox requests are sent to sava:1.0.0, while all other browsers are directed to sava:1.1.0.</span></span>

  ![Vamp UI - filtrera trafik](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a><span data-ttu-id="30bc7-242">Sammanfattningsvis</span><span class="sxs-lookup"><span data-stu-id="30bc7-242">Summing up</span></span>

<span data-ttu-id="30bc7-243">Den här artikeln är en snabb introduktion till Vamp på ett DC/OS-kluster.</span><span class="sxs-lookup"><span data-stu-id="30bc7-243">This article was a quick introduction to Vamp on a DC/OS cluster.</span></span> <span data-ttu-id="30bc7-244">Börja du har fått Vamp och körs på din Azure Container Service DC/OS-kluster distribuerat en tjänst med en Vamp modell och används på exponerade slutpunkten (gateway).</span><span class="sxs-lookup"><span data-stu-id="30bc7-244">For starters, you got Vamp up and running on your Azure Container Service DC/OS cluster, deployed a service with a Vamp blueprint, and accessed it at the exposed endpoint (gateway).</span></span>

<span data-ttu-id="30bc7-245">Vi också har använts på vissa kraftfulla funktioner i Vamp: Koppla en ny service variant-distributionen körs och introduktion till inkrementellt sedan filtrerar trafik för att lösa kända kompatibilitetsproblem.</span><span class="sxs-lookup"><span data-stu-id="30bc7-245">We also touched on some powerful features of Vamp:  merging a new service variant to the running deployment and introducing it incrementally, then filtering traffic to resolve a known incompatibility.</span></span>


## <a name="next-steps"></a><span data-ttu-id="30bc7-246">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30bc7-246">Next steps</span></span>

* <span data-ttu-id="30bc7-247">Lär dig mer om hur du hanterar Vamp åtgärder via den [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span><span class="sxs-lookup"><span data-stu-id="30bc7-247">Learn about managing Vamp actions through the [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span></span>

* <span data-ttu-id="30bc7-248">Skapa Vamp automatiseringsskript i Node.js och köra dem som [Vamp arbetsflöden](http://vamp.io/documentation/tutorials/create-a-workflow/).</span><span class="sxs-lookup"><span data-stu-id="30bc7-248">Build Vamp automation scripts in Node.js and run them as [Vamp workflows](http://vamp.io/documentation/tutorials/create-a-workflow/).</span></span>

* <span data-ttu-id="30bc7-249">Se ytterligare [VAMP självstudier](http://vamp.io/documentation/tutorials/overview/).</span><span class="sxs-lookup"><span data-stu-id="30bc7-249">See additional [VAMP tutorials](http://vamp.io/documentation/tutorials/overview/).</span></span>

