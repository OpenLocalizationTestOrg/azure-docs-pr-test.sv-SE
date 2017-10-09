---
title: aaaMonitor ett Azure DC/OS-kluster - stacken ELK | Microsoft Docs
description: "Övervaka ett DC/OS-kluster i Azure Container Service-kluster med ELK (Elasticsearch Logstash och Kibana)."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Behållare med DC/OS, Azure, övervakning, elk"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 8d81c5342616ec14880d38803cdf95f5845a669b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a><span data-ttu-id="ea995-104">Övervaka ett Azure Container Service-kluster med ELK</span><span class="sxs-lookup"><span data-stu-id="ea995-104">Monitor an Azure Container Service cluster with ELK</span></span>
<span data-ttu-id="ea995-105">I den här artikeln visar hur toodeploy hello ELK (Elasticsearch Logstash, Kibana) stacken på ett DC/OS-kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ea995-105">In this article, we demonstrate how toodeploy hello ELK (Elasticsearch, Logstash, Kibana) stack on a DC/OS cluster in Azure Container Service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ea995-106">Krav</span><span class="sxs-lookup"><span data-stu-id="ea995-106">Prerequisites</span></span>
<span data-ttu-id="ea995-107">[Distribuera](container-service-deployment.md) och [ansluta](../container-service-connect.md) ett DC/OS-kluster som är konfigurerat med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="ea995-107">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a DC/OS cluster configured by Azure Container Service.</span></span> <span data-ttu-id="ea995-108">Utforska hello instrumentpanelen för DC/OS och Marathon tjänster [här](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="ea995-108">Explore hello DC/OS dashboard and Marathon services [here](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="ea995-109">Även installera hello [Marathon belastningsutjämnaren](container-service-load-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="ea995-109">Also install hello [Marathon Load Balancer](container-service-load-balancing.md).</span></span>


## <a name="elk-elasticsearch-logstash-kibana"></a><span data-ttu-id="ea995-110">ELK (Elasticsearch Logstash, Kibana)</span><span class="sxs-lookup"><span data-stu-id="ea995-110">ELK (Elasticsearch, Logstash, Kibana)</span></span>
<span data-ttu-id="ea995-111">ELK stacken är en kombination av Elasticsearch Logstash, och Kibana som ger en end tooend stapel som kan använda toomonitor och analysera loggar i klustret.</span><span class="sxs-lookup"><span data-stu-id="ea995-111">ELK stack is a combination of Elasticsearch, Logstash, and Kibana that provides an end tooend stack that can be used toomonitor and analyze logs in your cluster.</span></span>

## <a name="configure-hello-elk-stack-on-a-dcos-cluster"></a><span data-ttu-id="ea995-112">Konfigurera hello ELK stack på ett DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="ea995-112">Configure hello ELK stack on a DC/OS cluster</span></span>
<span data-ttu-id="ea995-113">Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/) en gång i hello DC/OS-Gränssnittet navigera för**Universe**.</span><span class="sxs-lookup"><span data-stu-id="ea995-113">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in hello DC/OS UI navigate too**Universe**.</span></span> <span data-ttu-id="ea995-114">Söka efter och installera Elasticsearch och Logstash Kibana från hello DC/OS Universe och i den specifika ordningen.</span><span class="sxs-lookup"><span data-stu-id="ea995-114">Search and install Elasticsearch, Logstash, and Kibana from hello DC/OS Universe and in that specific order.</span></span> <span data-ttu-id="ea995-115">Du kan lära dig mer om konfiguration om du går toohello **Installation i Avancerat** länk.</span><span class="sxs-lookup"><span data-stu-id="ea995-115">You can learn more about configuration if you go toohello **Advanced Installation** link.</span></span>

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

<span data-ttu-id="ea995-119">En gång Hej ELK behållare och är igång, behöver du tooenable Kibana toobe nås via Marathon-LB.</span><span class="sxs-lookup"><span data-stu-id="ea995-119">Once hello ELK containers and are up and running, you need tooenable Kibana toobe accessed through Marathon-LB.</span></span> <span data-ttu-id="ea995-120">Navigera för **Services** > **kibana**, och klicka på **redigera** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="ea995-120">Navigate too **Services** > **kibana**, and click **Edit** as shown below.</span></span>

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


<span data-ttu-id="ea995-122">Växla för**JSON-läget** och rulla ned toohello etiketter avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ea995-122">Toggle too**JSON mode** and scroll down toohello labels section.</span></span>
<span data-ttu-id="ea995-123">Du behöver tooadd en `"HAPROXY_GROUP": "external"` post här som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ea995-123">You need tooadd a `"HAPROXY_GROUP": "external"` entry here as shown below.</span></span>
<span data-ttu-id="ea995-124">När du klickar på **distribuera ändringar**, omstarter av behållare.</span><span class="sxs-lookup"><span data-stu-id="ea995-124">Once you click **Deploy changes**, your container restarts.</span></span>

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


<span data-ttu-id="ea995-126">Om du vill tooverify som Kibana registreras som en tjänst i hello HAPROXY instrumentpanelen måste du tooopen port 9090 på hello agent klustret som HAPROXY körs på port 9090.</span><span class="sxs-lookup"><span data-stu-id="ea995-126">If you want tooverify that Kibana is registered as a service in hello HAPROXY dashboard, you need tooopen port 9090 on hello agent cluster as HAPROXY runs on port 9090.</span></span>
<span data-ttu-id="ea995-127">Som standard det öppna portarna 80, 8080 och 443 i hello DC/OS-klustret för agenten.</span><span class="sxs-lookup"><span data-stu-id="ea995-127">By default, we open ports 80, 8080, and 443 in hello DC/OS agent cluster.</span></span>
<span data-ttu-id="ea995-128">Instruktioner tooopen en port och ange offentliga utvärdera tillhandahålls [här](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="ea995-128">Instructions tooopen a port and provide public assess are provided [here](container-service-enable-public-access.md).</span></span>

<span data-ttu-id="ea995-129">tooaccess hello HAPROXY instrumentpanelen, öppna hello Marathon-LB administratörsgränssnittet på: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span><span class="sxs-lookup"><span data-stu-id="ea995-129">tooaccess hello HAPROXY dashboard, open hello Marathon-LB admin interface at: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span></span>
<span data-ttu-id="ea995-130">När du navigerar toohello URL bör du se hello HAPROXY instrumentpanelen som visas nedan och du bör se en för Kibana.</span><span class="sxs-lookup"><span data-stu-id="ea995-130">Once you navigate toohello URL, you should see hello HAPROXY dashboard as shown below and you should see a service entry for Kibana.</span></span>

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


<span data-ttu-id="ea995-132">tooaccess hello Kibana instrumentpanelen, som har distribuerats på port 5601, du behöver tooopen port 5601.</span><span class="sxs-lookup"><span data-stu-id="ea995-132">tooaccess hello Kibana dashboard, which is deployed on port 5601, you need tooopen port 5601.</span></span> <span data-ttu-id="ea995-133">Följ anvisningarna [här](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="ea995-133">Follow instructions [here](container-service-enable-public-access.md).</span></span> <span data-ttu-id="ea995-134">Öppna hello Kibana instrumentpanelen på: `http://localhost:5601`.</span><span class="sxs-lookup"><span data-stu-id="ea995-134">Then open hello Kibana dashboard at: `http://localhost:5601`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea995-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ea995-135">Next steps</span></span>

* <span data-ttu-id="ea995-136">För system- och logg vidarebefordran och inställningar, se [hantering i DC/OS med ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span><span class="sxs-lookup"><span data-stu-id="ea995-136">For system and application log forwarding and setup, see [Log Management in DC/OS with ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span></span>

* <span data-ttu-id="ea995-137">toofilter loggar finns [filtrering loggar med ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span><span class="sxs-lookup"><span data-stu-id="ea995-137">toofilter logs, see [Filtering Logs with ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span></span> 

 

