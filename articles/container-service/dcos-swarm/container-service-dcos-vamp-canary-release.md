---
title: "aaaCanary version med Vamp på Azure DC/OS-kluster | Microsoft Docs"
description: "Hur toouse Vamp toocanary versionen tjänster och tillämpa smart trafik filtrering på ett Azure Container Service DC/OS-kluster"
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
ms.openlocfilehash: e7b8658a161a7cddcf718e3e1c12a889a330d3d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="274cd-103">Kanarieöarna versionen mikrotjänster med Vamp på ett Azure Container Service DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="274cd-103">Canary release microservices with Vamp on an Azure Container Service DC/OS cluster</span></span>

<span data-ttu-id="274cd-104">I den här genomgången ska konfigurera vi Vamp på Azure Container Service med ett DC/OS-kluster.</span><span class="sxs-lookup"><span data-stu-id="274cd-104">In this walkthrough, we set up Vamp on Azure Container Service with a DC/OS cluster.</span></span> <span data-ttu-id="274cd-105">Vi Kanarieöarna släpp hello Vamp demo service ”sava” och sedan matcha inkompatibilitet hello tjänsten med Firefox genom att använda smart trafikfiltrering.</span><span class="sxs-lookup"><span data-stu-id="274cd-105">We canary release hello Vamp demo service "sava", and then resolve an incompatibility of hello service with Firefox by applying smart traffic filtering.</span></span> 

> [!TIP] 
> <span data-ttu-id="274cd-106">I den här genomgången Vamp körs på en DC/OS-klustret, men du kan också använda Vamp med Kubernetes som hello orchestrator.</span><span class="sxs-lookup"><span data-stu-id="274cd-106">In this walkthrough Vamp runs on a DC/OS cluster, but you can also use Vamp with Kubernetes as hello orchestrator.</span></span>
>

## <a name="about-canary-releases-and-vamp"></a><span data-ttu-id="274cd-107">Om Kanarieöarna versioner och Vamp</span><span class="sxs-lookup"><span data-stu-id="274cd-107">About canary releases and Vamp</span></span>


<span data-ttu-id="274cd-108">[Kanarieöarna släppa](https://martinfowler.com/bliki/CanaryRelease.html) är en smart distributionsstrategi antas av innovativa organisationer som Netflix, Facebook och Spotify.</span><span class="sxs-lookup"><span data-stu-id="274cd-108">[Canary releasing](https://martinfowler.com/bliki/CanaryRelease.html) is a smart deployment strategy adopted by innovative organizations like Netflix, Facebook, and Spotify.</span></span> <span data-ttu-id="274cd-109">Det är en lösning som passar eftersom det minskar problem, introducerar säkerhet nät och ökar innovation.</span><span class="sxs-lookup"><span data-stu-id="274cd-109">It’s an approach that makes sense, because it reduces issues, introduces safety-nets, and increases innovation.</span></span> <span data-ttu-id="274cd-110">Så varför inte alla företag som använder den?</span><span class="sxs-lookup"><span data-stu-id="274cd-110">So why aren’t all companies using it?</span></span> <span data-ttu-id="274cd-111">Utöka en CI/CD-pipeline tooinclude Kanarieöarna strategier lägger till komplexitet och kräver omfattande devops kunskap och erfarenhet.</span><span class="sxs-lookup"><span data-stu-id="274cd-111">Extending a CI/CD pipeline tooinclude canary strategies adds complexity, and requires extensive devops knowledge and experience.</span></span> <span data-ttu-id="274cd-112">Som är tillräckligt med tooblock mindre företag och företag som är både innan de även komma igång.</span><span class="sxs-lookup"><span data-stu-id="274cd-112">That’s enough tooblock smaller companies and enterprises alike before they even get started.</span></span> 

<span data-ttu-id="274cd-113">[Vamp](http://vamp.io/) är en öppen källkod system utformade tooease övergången och ta Kanarieöarna släppa funktioner tooyour önskade behållaren Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="274cd-113">[Vamp](http://vamp.io/) is an open-source system designed tooease this transition and bring canary releasing features tooyour preferred container scheduler.</span></span> <span data-ttu-id="274cd-114">Vamps Kanarieöarna funktioner går utöver procent-baserade installationer.</span><span class="sxs-lookup"><span data-stu-id="274cd-114">Vamp’s canary functionality goes beyond percentage-based rollouts.</span></span> <span data-ttu-id="274cd-115">Trafik kan filtreras och dela på en mängd olika villkor, till exempel tootarget specifika användare, IP-adressintervall eller enheter.</span><span class="sxs-lookup"><span data-stu-id="274cd-115">Traffic can be filtered and split on a wide range of conditions, for example tootarget specific users, IP-ranges, or devices.</span></span> <span data-ttu-id="274cd-116">Vamp spårar och analyserar prestandamått, så att för automatisering som bygger på verkliga data.</span><span class="sxs-lookup"><span data-stu-id="274cd-116">Vamp tracks and analyzes performance metrics, allowing for automation based on real-world data.</span></span> <span data-ttu-id="274cd-117">Du kan ställa in automatisk återställning vid fel eller skala enskild tjänst varianter utifrån belastningen eller svarstid.</span><span class="sxs-lookup"><span data-stu-id="274cd-117">You can set up automatic rollback on errors, or scale individual service variants based on load or latency.</span></span>

## <a name="set-up-azure-container-service-with-dcos"></a><span data-ttu-id="274cd-118">Konfigurera Azure Container Service med DC/OS</span><span class="sxs-lookup"><span data-stu-id="274cd-118">Set up Azure Container Service with DC/OS</span></span>



1. <span data-ttu-id="274cd-119">[Distribuera ett DC/OS-kluster](container-service-deployment.md) med en överordnad och två agenterna av standardstorleken.</span><span class="sxs-lookup"><span data-stu-id="274cd-119">[Deploy a DC/OS cluster](container-service-deployment.md) with one master and two agents of default size.</span></span> 

2. <span data-ttu-id="274cd-120">[Skapa en SSH-tunnel](../container-service-connect.md) tooconnect toohello DC/OS-klustret.</span><span class="sxs-lookup"><span data-stu-id="274cd-120">[Create an SSH tunnel](../container-service-connect.md) tooconnect toohello DC/OS cluster.</span></span> <span data-ttu-id="274cd-121">Den här artikeln förutsätter att du tunnel toohello klustret på lokal port 80.</span><span class="sxs-lookup"><span data-stu-id="274cd-121">This article assumes that you tunnel toohello cluster on local port 80.</span></span>


## <a name="set-up-vamp"></a><span data-ttu-id="274cd-122">Ställ in Vamp</span><span class="sxs-lookup"><span data-stu-id="274cd-122">Set up Vamp</span></span>

<span data-ttu-id="274cd-123">Nu när du har ett DC/OS-kluster som körs, kan du installera Vamp från hello DC/OS-Gränssnittet (http://localhost:80).</span><span class="sxs-lookup"><span data-stu-id="274cd-123">Now that you have a running DC/OS cluster, you can install Vamp from hello DC/OS UI (http://localhost:80).</span></span> 

![DC/OS-gränssnitt:](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

<span data-ttu-id="274cd-125">Installationen sker i två steg:</span><span class="sxs-lookup"><span data-stu-id="274cd-125">Installation is done in two stages:</span></span>

1. <span data-ttu-id="274cd-126">**Distribuera Elasticsearch**.</span><span class="sxs-lookup"><span data-stu-id="274cd-126">**Deploy Elasticsearch**.</span></span>

2. <span data-ttu-id="274cd-127">Sedan **distribuera Vamp** genom att installera hello Vamp DC/OS universe paketet.</span><span class="sxs-lookup"><span data-stu-id="274cd-127">Then **deploy Vamp** by installing hello Vamp DC/OS universe package.</span></span>

### <a name="deploy-elasticsearch"></a><span data-ttu-id="274cd-128">Distribuera Elasticsearch</span><span class="sxs-lookup"><span data-stu-id="274cd-128">Deploy Elasticsearch</span></span>

<span data-ttu-id="274cd-129">Vamp kräver Elasticsearch för insamling av mätvärden och aggregering.</span><span class="sxs-lookup"><span data-stu-id="274cd-129">Vamp requires Elasticsearch for metrics collection and aggregation.</span></span> <span data-ttu-id="274cd-130">Du kan använda hello [magneticio Docker bilder](https://hub.docker.com/r/magneticio/elastic/) toodeploy en kompatibel Vamp Elasticsearch stack.</span><span class="sxs-lookup"><span data-stu-id="274cd-130">You can use hello [magneticio Docker images](https://hub.docker.com/r/magneticio/elastic/) toodeploy a compatible Vamp Elasticsearch stack.</span></span>

1. <span data-ttu-id="274cd-131">Hello DC/OS-Gränssnittet, gå för**Services** och på **distribuera tjänst**.</span><span class="sxs-lookup"><span data-stu-id="274cd-131">In hello DC/OS UI, go too**Services** and click **Deploy Service**.</span></span>

2. <span data-ttu-id="274cd-132">Välj **JSON-läget** från hello **distribuera nya tjänster** popup.</span><span class="sxs-lookup"><span data-stu-id="274cd-132">Select **JSON mode** from hello **Deploy New Service** pop-up.</span></span>

  ![Välj JSON-läge](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. <span data-ttu-id="274cd-134">Klistra in i hello följande JSON.</span><span class="sxs-lookup"><span data-stu-id="274cd-134">Paste in hello following JSON.</span></span> <span data-ttu-id="274cd-135">Den här konfigurationen kör hello behållare med 1 GB RAM-minne och grundläggande hälsokontrollen på hello Elasticsearch port.</span><span class="sxs-lookup"><span data-stu-id="274cd-135">This configuration runs hello container with 1 GB of RAM and a basic health check on hello Elasticsearch port.</span></span>
  
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
  

3. <span data-ttu-id="274cd-136">Klicka på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="274cd-136">Click **Deploy**.</span></span>

  <span data-ttu-id="274cd-137">DC/OS distribuerar hello Elasticsearch behållare.</span><span class="sxs-lookup"><span data-stu-id="274cd-137">DC/OS deploys hello Elasticsearch container.</span></span> <span data-ttu-id="274cd-138">Du kan följa förloppet på hello **Services** sidan.</span><span class="sxs-lookup"><span data-stu-id="274cd-138">You can track progress on hello **Services** page.</span></span>  

  ![distribuera e? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a><span data-ttu-id="274cd-140">Distribuera Vamp</span><span class="sxs-lookup"><span data-stu-id="274cd-140">Deploy Vamp</span></span>

<span data-ttu-id="274cd-141">När Elasticsearch rapporterar som **kör**, kan du lägga till hello Vamp DC/OS Universe paketet.</span><span class="sxs-lookup"><span data-stu-id="274cd-141">Once Elasticsearch reports as **Running**, you can add hello Vamp DC/OS Universe package.</span></span> 

1. <span data-ttu-id="274cd-142">Gå för**Universe** och Sök efter **vamp**.</span><span class="sxs-lookup"><span data-stu-id="274cd-142">Go too**Universe** and search for **vamp**.</span></span> 
  <span data-ttu-id="274cd-143">![Vamp på DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span><span class="sxs-lookup"><span data-stu-id="274cd-143">![Vamp on DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)</span></span>

2. <span data-ttu-id="274cd-144">Klicka på **installera** nästa toohello vamp paketet och välj **Installation i Avancerat**.</span><span class="sxs-lookup"><span data-stu-id="274cd-144">Click **install** next toohello vamp package, and choose **Advanced Installation**.</span></span>

3. <span data-ttu-id="274cd-145">Bläddra nedåt och ange hello följande elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span><span class="sxs-lookup"><span data-stu-id="274cd-145">Scroll down and enter hello following elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`.</span></span> 

  ![Ange Elasticsearch URL](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. <span data-ttu-id="274cd-147">Klicka på **granska och installera**, klicka på **installera** toostart hello distribution.</span><span class="sxs-lookup"><span data-stu-id="274cd-147">Click **Review and Install**, then click **Install** toostart hello deployment.</span></span>  

  <span data-ttu-id="274cd-148">DC/OS distribuerar alla nödvändiga komponenter för Vamp.</span><span class="sxs-lookup"><span data-stu-id="274cd-148">DC/OS deploys all required Vamp components.</span></span> <span data-ttu-id="274cd-149">Du kan följa förloppet på hello **Services** sidan.</span><span class="sxs-lookup"><span data-stu-id="274cd-149">You can track progress on hello **Services** page.</span></span>
  
  ![Distribuera Vamp som universe paket](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. <span data-ttu-id="274cd-151">När distributionen är klar, kan du komma åt hello Vamp UI:</span><span class="sxs-lookup"><span data-stu-id="274cd-151">Once deployment has completed, you can access hello Vamp UI:</span></span>

  ![Vamp-tjänsten på DC/OS](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp UI](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a><span data-ttu-id="274cd-154">Distribuera din första tjänst</span><span class="sxs-lookup"><span data-stu-id="274cd-154">Deploy your first service</span></span>

<span data-ttu-id="274cd-155">Nu att Vamp är igång kan du distribuera en tjänst från ett utkast.</span><span class="sxs-lookup"><span data-stu-id="274cd-155">Now that Vamp is up and running, deploy a service from a blueprint.</span></span> 

<span data-ttu-id="274cd-156">I sin enklaste form är en [Vamp modell](http://vamp.io/documentation/using-vamp/blueprints/) beskriver hello slutpunkter (gateway), kluster och tjänster toodeploy.</span><span class="sxs-lookup"><span data-stu-id="274cd-156">In its simplest form, a [Vamp blueprint](http://vamp.io/documentation/using-vamp/blueprints/) describes hello endpoints (gateways), clusters, and services toodeploy.</span></span> <span data-ttu-id="274cd-157">Vamp använder kluster toogroup olika varianter av hello samma tjänst i logiska grupper för Kanarieöarna släppa eller A / B-testning.</span><span class="sxs-lookup"><span data-stu-id="274cd-157">Vamp uses clusters toogroup different variants of hello same service into logical groups for canary releasing or A/B testing.</span></span>  

<span data-ttu-id="274cd-158">Det här scenariot använder ett monolitisk exempelprogram som kallas [ **sava**](https://github.com/magneticio/sava), vilket är version 1.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-158">This scenario uses a sample monolithic application called [**sava**](https://github.com/magneticio/sava), which is at version 1.0.</span></span> <span data-ttu-id="274cd-159">Hej monolitvolym paketeras i en dockerbehållare med som finns i Docker under magneticio/sava:1.0.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-159">hello monolith is packaged in a Docker container, which is in Docker Hub under magneticio/sava:1.0.0.</span></span> <span data-ttu-id="274cd-160">hello appen körs normalt på port 8080, men du vill tooexpose den under port 9050 i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="274cd-160">hello app normally runs on port 8080, but you want tooexpose it under port 9050 in this case.</span></span> <span data-ttu-id="274cd-161">Distribuera hello appen via Vamp med en enkel modell.</span><span class="sxs-lookup"><span data-stu-id="274cd-161">Deploy hello app through Vamp using a simple blueprint.</span></span>

1. <span data-ttu-id="274cd-162">Gå för**distributioner**.</span><span class="sxs-lookup"><span data-stu-id="274cd-162">Go too**Deployments**.</span></span>

2. <span data-ttu-id="274cd-163">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="274cd-163">Click **Add**.</span></span>

3. <span data-ttu-id="274cd-164">Klistra in följande hello utkast YAML.</span><span class="sxs-lookup"><span data-stu-id="274cd-164">Paste in hello following blueprint YAML.</span></span> <span data-ttu-id="274cd-165">Det här utkastet innehåller ett kluster med en enda service variant som vi ändras i ett senare steg:</span><span class="sxs-lookup"><span data-stu-id="274cd-165">This blueprint contains one cluster with only one service variant, which we change in a later step:</span></span>

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster toocreate
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. <span data-ttu-id="274cd-166">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="274cd-166">Click **Save**.</span></span> <span data-ttu-id="274cd-167">Vamp initierar hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="274cd-167">Vamp initiates hello deployment.</span></span>

<span data-ttu-id="274cd-168">hello distribution visas på hello **distributioner** sidan.</span><span class="sxs-lookup"><span data-stu-id="274cd-168">hello deployment is listed on hello **Deployments** page.</span></span> <span data-ttu-id="274cd-169">Klicka på hello distribution toomonitor dess status.</span><span class="sxs-lookup"><span data-stu-id="274cd-169">Click hello deployment toomonitor its status.</span></span>

![Vamp gränssnitt – distribuera sava](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![sava service i Användargränssnittet för Vamp](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

<span data-ttu-id="274cd-172">Två gateways skapas, som visas på hello **Gateways** sidan:</span><span class="sxs-lookup"><span data-stu-id="274cd-172">Two gateways are created, which are listed on hello **Gateways** page:</span></span>

* <span data-ttu-id="274cd-173">en stabil endpoint tooaccess hello som kör tjänsten (9050-port)</span><span class="sxs-lookup"><span data-stu-id="274cd-173">a stable endpoint tooaccess hello running service (port 9050)</span></span> 
* <span data-ttu-id="274cd-174">en Vamp hanteras internt gateway (mer om den här gatewayen senare).</span><span class="sxs-lookup"><span data-stu-id="274cd-174">a Vamp-managed internal gateway (more on this gateway later).</span></span> 

![Vamp UI - sava gateways](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

<span data-ttu-id="274cd-176">Hej sava tjänsten nu har distribuerats, men du kan inte komma åt den externt eftersom hello Azure belastningsutjämnare inte vet tooforward trafik tooit ännu.</span><span class="sxs-lookup"><span data-stu-id="274cd-176">hello sava service has now deployed, but you can’t access it externally because hello Azure Load Balancer doesn’t know tooforward traffic tooit yet.</span></span> <span data-ttu-id="274cd-177">tooaccess hello tjänst update hello Azure nätverkskonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="274cd-177">tooaccess hello service, update hello Azure networking configuration.</span></span>


## <a name="update-hello-azure-network-configuration"></a><span data-ttu-id="274cd-178">Uppdatera hello Azure nätverkskonfiguration</span><span class="sxs-lookup"><span data-stu-id="274cd-178">Update hello Azure network configuration</span></span>

<span data-ttu-id="274cd-179">Vamp distribuerade hello sava-tjänsten på noder hello DC/OS-agenten, exponera en stabil slutpunkt på port 9050.</span><span class="sxs-lookup"><span data-stu-id="274cd-179">Vamp deployed hello sava service on hello DC/OS agent nodes, exposing a stable endpoint at port 9050.</span></span> <span data-ttu-id="274cd-180">tooaccess hello tjänst från utanför hello DC/OS-klustret, att Hej efter ändringar toohello Azure nätverkskonfigurationen i kluster-distributionen:</span><span class="sxs-lookup"><span data-stu-id="274cd-180">tooaccess hello service from outside hello DC/OS cluster, make hello following changes toohello Azure network configuration in your cluster deployment:</span></span> 

1. <span data-ttu-id="274cd-181">**Konfigurera hello Azure belastningsutjämnare** för hello-agenter (hello resurs med namnet **DC/OS-agent-lb-xxxx**) med en hälsoavsökningen och en regel tooforward trafik på port 9050 toohello sava instanser.</span><span class="sxs-lookup"><span data-stu-id="274cd-181">**Configure hello Azure Load Balancer** for hello agents (hello resource named **dcos-agent-lb-xxxx**) with a health probe and a rule tooforward traffic on port 9050 toohello sava instances.</span></span> 

2. <span data-ttu-id="274cd-182">**Uppdatera hello nätverkssäkerhetsgruppen** för hello offentliga agenter (hello resurs med namnet **XXXX-agent-offentliga-nsg-XXXX**) tooallow trafik på port 9050.</span><span class="sxs-lookup"><span data-stu-id="274cd-182">**Update hello network security group** for hello public agents (hello resource named **XXXX-agent-public-nsg-XXXX**) tooallow traffic on port 9050.</span></span>

<span data-ttu-id="274cd-183">För detaljerade anvisningar toocomplete hello dessa uppgifter med hjälp av Azure portal, se [aktiverar offentlig åtkomst tooan Azure Container Service-programmet](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="274cd-183">For detailed steps toocomplete these tasks using hello Azure portal, see [Enable public access tooan Azure Container Service application](container-service-enable-public-access.md).</span></span> <span data-ttu-id="274cd-184">Ange port 9050 för alla portinställningar.</span><span class="sxs-lookup"><span data-stu-id="274cd-184">Specify port 9050 for all port settings.</span></span>


<span data-ttu-id="274cd-185">När allt har skapats, gå toohello **översikt** bladet för hello DC/OS-agentens belastningsutjämnare (hello resurs med namnet **DC/OS-agent-lb-xxxx**).</span><span class="sxs-lookup"><span data-stu-id="274cd-185">Once everything has been created, go toohello **Overview** blade of hello DC/OS agent load balancer (hello resource named **dcos-agent-lb-xxxx**).</span></span> <span data-ttu-id="274cd-186">Hitta hello **offentliga IP-adressen**, och använder hello adress tooaccess sava på port 9050.</span><span class="sxs-lookup"><span data-stu-id="274cd-186">Find hello **Public IP address**, and use hello address tooaccess sava at port 9050.</span></span>

![Azure portal – hämta offentlig IP-adress](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a><span data-ttu-id="274cd-189">Kör en version av Kanarieöarna</span><span class="sxs-lookup"><span data-stu-id="274cd-189">Run a canary release</span></span>

<span data-ttu-id="274cd-190">Anta att du har en ny version av det här programmet som du vill toocanary övergång till produktion.</span><span class="sxs-lookup"><span data-stu-id="274cd-190">Suppose you have a new version of this application that you want toocanary release into production.</span></span> <span data-ttu-id="274cd-191">Du har den behållare som magneticio/sava:1.1.0 och är redo toogo.</span><span class="sxs-lookup"><span data-stu-id="274cd-191">You have it containerized as magneticio/sava:1.1.0 and are ready toogo.</span></span> <span data-ttu-id="274cd-192">Vamp kan du enkelt lägga till nya tjänster toohello kör distribution.</span><span class="sxs-lookup"><span data-stu-id="274cd-192">Vamp lets you easily add new services toohello running deployment.</span></span> <span data-ttu-id="274cd-193">Tjänsterna ”kopplade” distribuerats tillsammans med befintliga hello-tjänster i hello kluster, och tilldelas en vikt på 0%.</span><span class="sxs-lookup"><span data-stu-id="274cd-193">These "merged" services are deployed alongside hello existing services in hello cluster, and assigned a weight of 0%.</span></span> <span data-ttu-id="274cd-194">Ingen trafik är routade tooa nyligen samman tjänsten tills du justera hello trafikfördelning.</span><span class="sxs-lookup"><span data-stu-id="274cd-194">No traffic is routed tooa newly merged service until you adjust hello traffic distribution.</span></span> <span data-ttu-id="274cd-195">hello vikt skjutreglaget i hello Vamp UI ger fullständig kontroll över hello distribution, så att för inkrementell justeringar (Kanarieöarna utgåva) eller en omedelbar återställning.</span><span class="sxs-lookup"><span data-stu-id="274cd-195">hello weight slider in hello Vamp UI gives you complete control over hello distribution, allowing for incremental adjustments (canary release) or an instant rollback.</span></span>

### <a name="merge-a-new-service-variant"></a><span data-ttu-id="274cd-196">Koppla en ny service-variant</span><span class="sxs-lookup"><span data-stu-id="274cd-196">Merge a new service variant</span></span>

<span data-ttu-id="274cd-197">toomerge hello nya sava 1.1 service med hello kör distribution:</span><span class="sxs-lookup"><span data-stu-id="274cd-197">toomerge hello new sava 1.1 service with hello running deployment:</span></span>

1. <span data-ttu-id="274cd-198">I hello Vamp UI, klickar du på **ritningarna**.</span><span class="sxs-lookup"><span data-stu-id="274cd-198">In hello Vamp UI, click **Blueprints**.</span></span>

2. <span data-ttu-id="274cd-199">Klicka på **Lägg till** och klistra in följande hello utkast YAML: den här utkast som beskriver en ny service variant (sava: 1.1.0) toodeploy inom hello befintligt kluster (sava_cluster).</span><span class="sxs-lookup"><span data-stu-id="274cd-199">Click **Add** and paste in hello following blueprint YAML: This blueprint describes a new service variant (sava:1.1.0) toodeploy within hello existing cluster (sava_cluster).</span></span>

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster tooupdate
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint tooupdate
  ```
  
3. <span data-ttu-id="274cd-200">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="274cd-200">Click **Save**.</span></span> <span data-ttu-id="274cd-201">hello utkast lagras och visas på hello **ritningarna** sidan.</span><span class="sxs-lookup"><span data-stu-id="274cd-201">hello blueprint is stored and listed on hello **Blueprints** page.</span></span>

4. <span data-ttu-id="274cd-202">Öppna hello Åtgärd-menyn i hello sava: 1.1 plan och klickar på **koppla till**.</span><span class="sxs-lookup"><span data-stu-id="274cd-202">Open hello action menu on hello sava:1.1 blueprint and click **Merge to**.</span></span>

  ![Vamp UI - ritningarna](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. <span data-ttu-id="274cd-204">Välj hello **sava** distribution och klickar på **sammanfoga**.</span><span class="sxs-lookup"><span data-stu-id="274cd-204">Select hello **sava** deployment and click **Merge**.</span></span>

  ![Vamp UI - sammanfoga utkast toodeployment](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

<span data-ttu-id="274cd-206">Vamp distribuerar hello ny sava: 1.1.0 service variant beskrivs i hello utkast tillsammans med sava: 1.0.0 i hello **sava_cluster** av hello kör distribution.</span><span class="sxs-lookup"><span data-stu-id="274cd-206">Vamp deploys hello new sava:1.1.0 service variant described in hello blueprint alongside sava:1.0.0 in hello **sava_cluster** of hello running deployment.</span></span> 

![Vamp UI - uppdaterade sava distribution](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

<span data-ttu-id="274cd-208">Hej **webport-sava/sava_cluster** gateway (hello klusterslutpunkten) har också uppdaterats, lägga till en väg toohello nyligen distribuerade sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-208">hello **sava/sava_cluster/webport** gateway (hello cluster endpoint) is also updated, adding a route toohello newly deployed sava:1.1.0.</span></span> <span data-ttu-id="274cd-209">Nu ingen trafik dirigeras här (hello **vikt** anges too0%).</span><span class="sxs-lookup"><span data-stu-id="274cd-209">At this point, no traffic is routed here (hello **WEIGHT** is set too0%).</span></span>

![Vamp UI - klustret gateway](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a><span data-ttu-id="274cd-211">Kanarieöarna versionen</span><span class="sxs-lookup"><span data-stu-id="274cd-211">Canary release</span></span>

<span data-ttu-id="274cd-212">Med båda versionerna av sava distribuerats i hello samma kluster, justera hello fördelningen av trafiken mellan dem genom att flytta hello **vikt** skjutreglaget.</span><span class="sxs-lookup"><span data-stu-id="274cd-212">With both versions of sava deployed in hello same cluster, adjust hello distribution of traffic between them by moving hello **WEIGHT** slider.</span></span>

1. <span data-ttu-id="274cd-213">Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) nästa för**vikt**.</span><span class="sxs-lookup"><span data-stu-id="274cd-213">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next too**WEIGHT**.</span></span>

2. <span data-ttu-id="274cd-214">Ange hello vikten Distributionsplatsens too50%/50% och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="274cd-214">Set hello weight distribution too50%/50% and click **Save**.</span></span>

  ![Vamp UI - gateway vikt skjutreglaget](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. <span data-ttu-id="274cd-216">Gå tillbaka tooyour webbläsare och uppdatera hello sava sidan några gånger.</span><span class="sxs-lookup"><span data-stu-id="274cd-216">Go back tooyour browser and refresh hello sava page a few more times.</span></span> <span data-ttu-id="274cd-217">Hej sava programmet nu växlar mellan en sava: 1.0 och en sava: 1.1-sidan.</span><span class="sxs-lookup"><span data-stu-id="274cd-217">hello sava application now switches between a sava:1.0 page and a sava:1.1 page.</span></span>

  ![Alternerande sava1.0 och sava1.1 tjänster](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > <span data-ttu-id="274cd-219">Den här växlingar hello sidan fungerar bäst med hello ”Incognito” eller ”anonym” läge i webbläsaren på grund av hello cachelagring av statiska tillgångar.</span><span class="sxs-lookup"><span data-stu-id="274cd-219">This alternation of hello page works best with hello "Incognito" or “Anonymous” mode of your browser because of hello caching of static assets.</span></span>
  >

### <a name="filter-traffic"></a><span data-ttu-id="274cd-220">Filtrera trafik</span><span class="sxs-lookup"><span data-stu-id="274cd-220">Filter traffic</span></span>

<span data-ttu-id="274cd-221">Anta att efter distributionen du identifierade inkompatibilitet i sava: 1.1.0 att visas i Firefox webbläsare.</span><span class="sxs-lookup"><span data-stu-id="274cd-221">Suppose after deployment that you discovered an incompatibility in sava:1.1.0 that causes display problems in Firefox browsers.</span></span> <span data-ttu-id="274cd-222">Du kan ange Vamp toofilter inkommande trafik och dirigera alla Firefox användare tillbaka toohello kända stabil sava: 1.0.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-222">You can set Vamp toofilter incoming traffic and direct all Firefox users back toohello known stable sava:1.0.0.</span></span> <span data-ttu-id="274cd-223">Det här filtret bättre direkt matchar hello avbrott för Firefox användare, medan alla andra fortsätter tooenjoy hello fördelarna med hello sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-223">This filter instantly resolves hello disruption for Firefox users, while everybody else continues tooenjoy hello benefits of hello improved sava:1.1.0.</span></span>

<span data-ttu-id="274cd-224">Vamp använder **villkor** toofilter trafik mellan vägar i en gateway.</span><span class="sxs-lookup"><span data-stu-id="274cd-224">Vamp uses **conditions** toofilter traffic between routes in a gateway.</span></span> <span data-ttu-id="274cd-225">Trafik filtreras först och dirigeras bl.a toohello villkor som gäller tooeach vägen.</span><span class="sxs-lookup"><span data-stu-id="274cd-225">Traffic is first filtered and directed according toohello conditions applied tooeach route.</span></span> <span data-ttu-id="274cd-226">Alla återstående trafiken fördelas enligt toohello gateway vikt inställningen.</span><span class="sxs-lookup"><span data-stu-id="274cd-226">All remaining traffic is distributed according toohello gateway weight setting.</span></span>

<span data-ttu-id="274cd-227">Du kan skapa ett villkor toofilter alla Firefox användare och dirigera dem toohello gamla sava: 1.0.0:</span><span class="sxs-lookup"><span data-stu-id="274cd-227">You can create a condition toofilter all Firefox users and direct them toohello old sava:1.0.0:</span></span>

1. <span data-ttu-id="274cd-228">På hello sava/sava_cluster/webport **Gateways** klickar du på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd en **VILLKORET** toohello väg sava/sava_cluster/sava:1.0.0/webport.</span><span class="sxs-lookup"><span data-stu-id="274cd-228">On hello sava/sava_cluster/webport **Gateways** page, click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd a **CONDITION** toohello route sava/sava_cluster/sava:1.0.0/webport.</span></span> 

2. <span data-ttu-id="274cd-229">Ange villkor för hello **användaragent == Firefox** och på ![Vamp UI - spara](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span><span class="sxs-lookup"><span data-stu-id="274cd-229">Enter hello condition **user-agent == Firefox** and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).</span></span>

  <span data-ttu-id="274cd-230">Vamp lägger till hello villkor med en standardvärdet 0%.</span><span class="sxs-lookup"><span data-stu-id="274cd-230">Vamp adds hello condition with a default strength of 0%.</span></span> <span data-ttu-id="274cd-231">Filtrera trafik på toostart, behöver du tooadjust hello villkoret styrka.</span><span class="sxs-lookup"><span data-stu-id="274cd-231">toostart filtering traffic, you need tooadjust hello condition strength.</span></span>

3. <span data-ttu-id="274cd-232">Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **styrkan** tillämpas toohello villkor.</span><span class="sxs-lookup"><span data-stu-id="274cd-232">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **STRENGTH** applied toohello condition.</span></span>
 
4. <span data-ttu-id="274cd-233">Ange hello **styrkan** too100% och klickar på ![Vamp UI - spara](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.</span><span class="sxs-lookup"><span data-stu-id="274cd-233">Set hello **STRENGTH** too100% and click ![Vamp UI - save](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.</span></span>

  <span data-ttu-id="274cd-234">Vamp nu skickar all trafik som matchar hello villkor (alla Firefox användare) toosava:1.0.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-234">Vamp now sends all traffic matching hello condition (all Firefox users) toosava:1.0.0.</span></span>

  ![Vamp UI - använda villkoret toogateway](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. <span data-ttu-id="274cd-236">Slutligen justera hello gateway vikt toosend alla återstående trafik (alla användare med icke-Firefox) toohello nya sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-236">Finally, adjust hello gateway weight toosend all remaining traffic (all non-Firefox users) toohello new sava:1.1.0.</span></span> <span data-ttu-id="274cd-237">Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) nästa för**vikt** och ange hello viktfördelningen så 100% är riktat toohello väg sava/sava_cluster/sava:1.1.0/webport.</span><span class="sxs-lookup"><span data-stu-id="274cd-237">Click ![Vamp UI - edit](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) next too**WEIGHT** and set hello weight distribution so 100% is directed toohello route sava/sava_cluster/sava:1.1.0/webport.</span></span>

  <span data-ttu-id="274cd-238">All trafik som inte filtreras genom hello villkoret är nu dirigerad toohello nya sava: 1.1.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-238">All traffic not filtered by hello condition is now directed toohello new sava:1.1.0.</span></span>

6. <span data-ttu-id="274cd-239">toosee hello filter i åtgärden, öppna två olika webbläsare (en Firefox och en annan webbläsare) och hello sava tjänsten från både.</span><span class="sxs-lookup"><span data-stu-id="274cd-239">toosee hello filter in action, open two different browsers (one Firefox and one other browser) and access hello sava service from both.</span></span> <span data-ttu-id="274cd-240">Alla Firefox begäranden skickas toosava:1.0.0, medan alla andra webbläsare som är riktade toosava:1.1.0.</span><span class="sxs-lookup"><span data-stu-id="274cd-240">All Firefox requests are sent toosava:1.0.0, while all other browsers are directed toosava:1.1.0.</span></span>

  ![Vamp UI - filtrera trafik](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a><span data-ttu-id="274cd-242">Sammanfattningsvis</span><span class="sxs-lookup"><span data-stu-id="274cd-242">Summing up</span></span>

<span data-ttu-id="274cd-243">Den här artikeln är en snabb introduktion tooVamp på ett DC/OS-kluster.</span><span class="sxs-lookup"><span data-stu-id="274cd-243">This article was a quick introduction tooVamp on a DC/OS cluster.</span></span> <span data-ttu-id="274cd-244">Börja du har fått Vamp och körs på din Azure Container Service DC/OS-kluster distribuerat en tjänst med en Vamp modell och används på hello exponeras slutpunkt (gateway).</span><span class="sxs-lookup"><span data-stu-id="274cd-244">For starters, you got Vamp up and running on your Azure Container Service DC/OS cluster, deployed a service with a Vamp blueprint, and accessed it at hello exposed endpoint (gateway).</span></span>

<span data-ttu-id="274cd-245">Vi också har använts på vissa kraftfulla funktioner i Vamp: Koppla en ny service variant toohello kör distribution och introduktion till inkrementellt sedan filtrera trafik tooresolve kända kompatibilitetsproblem.</span><span class="sxs-lookup"><span data-stu-id="274cd-245">We also touched on some powerful features of Vamp:  merging a new service variant toohello running deployment and introducing it incrementally, then filtering traffic tooresolve a known incompatibility.</span></span>


## <a name="next-steps"></a><span data-ttu-id="274cd-246">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="274cd-246">Next steps</span></span>

* <span data-ttu-id="274cd-247">Lär dig mer om hur du hanterar Vamp åtgärder via hello [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span><span class="sxs-lookup"><span data-stu-id="274cd-247">Learn about managing Vamp actions through hello [Vamp REST API](http://vamp.io/documentation/api/api-reference/).</span></span>

* <span data-ttu-id="274cd-248">Skapa Vamp automatiseringsskript i Node.js och köra dem som [Vamp arbetsflöden](http://vamp.io/documentation/tutorials/create-a-workflow/).</span><span class="sxs-lookup"><span data-stu-id="274cd-248">Build Vamp automation scripts in Node.js and run them as [Vamp workflows](http://vamp.io/documentation/tutorials/create-a-workflow/).</span></span>

* <span data-ttu-id="274cd-249">Se ytterligare [VAMP självstudier](http://vamp.io/documentation/tutorials/overview/).</span><span class="sxs-lookup"><span data-stu-id="274cd-249">See additional [VAMP tutorials](http://vamp.io/documentation/tutorials/overview/).</span></span>

