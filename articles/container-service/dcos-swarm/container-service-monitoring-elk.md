---
title: "Övervaka ett Azure DC/OS-kluster - stacken ELK | Microsoft Docs"
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
ms.openlocfilehash: fcfa277cdd0f3cebc0fbbb23e771fb23ffbe2ca6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a><span data-ttu-id="4b352-104">Övervaka ett Azure Container Service-kluster med ELK</span><span class="sxs-lookup"><span data-stu-id="4b352-104">Monitor an Azure Container Service cluster with ELK</span></span>
<span data-ttu-id="4b352-105">I den här artikeln visar vi hur du distribuerar ELK (Elasticsearch, Logstash, Kibana)-stacken på ett DC/OS-kluster i Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="4b352-105">In this article, we demonstrate how to deploy the ELK (Elasticsearch, Logstash, Kibana) stack on a DC/OS cluster in Azure Container Service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="4b352-106">Krav</span><span class="sxs-lookup"><span data-stu-id="4b352-106">Prerequisites</span></span>
<span data-ttu-id="4b352-107">[Distribuera](container-service-deployment.md) och [ansluta](../container-service-connect.md) ett DC/OS-kluster som är konfigurerat med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="4b352-107">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a DC/OS cluster configured by Azure Container Service.</span></span> <span data-ttu-id="4b352-108">Utforska instrumentpanelen för DC/OS och Marathon services [här](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="4b352-108">Explore the DC/OS dashboard and Marathon services [here](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="4b352-109">Installera den [Marathon belastningsutjämnaren](container-service-load-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="4b352-109">Also install the [Marathon Load Balancer](container-service-load-balancing.md).</span></span>


## <a name="elk-elasticsearch-logstash-kibana"></a><span data-ttu-id="4b352-110">ELK (Elasticsearch Logstash, Kibana)</span><span class="sxs-lookup"><span data-stu-id="4b352-110">ELK (Elasticsearch, Logstash, Kibana)</span></span>
<span data-ttu-id="4b352-111">ELK stacken är en kombination av Elasticsearch och Logstash Kibana som tillhandahåller en heltäckande stapel som kan användas för att övervaka och analysera loggar i klustret.</span><span class="sxs-lookup"><span data-stu-id="4b352-111">ELK stack is a combination of Elasticsearch, Logstash, and Kibana that provides an end to end stack that can be used to monitor and analyze logs in your cluster.</span></span>

## <a name="configure-the-elk-stack-on-a-dcos-cluster"></a><span data-ttu-id="4b352-112">Konfigurera ELK stacken på ett DC/OS-kluster</span><span class="sxs-lookup"><span data-stu-id="4b352-112">Configure the ELK stack on a DC/OS cluster</span></span>
<span data-ttu-id="4b352-113">Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/) en gång i DC/OS-Gränssnittet går du till **Universe**.</span><span class="sxs-lookup"><span data-stu-id="4b352-113">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in the DC/OS UI navigate to **Universe**.</span></span> <span data-ttu-id="4b352-114">Söka efter och installera Elasticsearch och Logstash Kibana från DC/OS-Universe och i den specifika ordningen.</span><span class="sxs-lookup"><span data-stu-id="4b352-114">Search and install Elasticsearch, Logstash, and Kibana from the DC/OS Universe and in that specific order.</span></span> <span data-ttu-id="4b352-115">Du kan lära dig mer om konfiguration om du går till den **Installation i Avancerat** länk.</span><span class="sxs-lookup"><span data-stu-id="4b352-115">You can learn more about configuration if you go to the **Advanced Installation** link.</span></span>

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

<span data-ttu-id="4b352-119">När ELK behållare och är igång, måste du aktivera Kibana är tillgängliga via Marathon-LB.</span><span class="sxs-lookup"><span data-stu-id="4b352-119">Once the ELK containers and are up and running, you need to enable Kibana to be accessed through Marathon-LB.</span></span> <span data-ttu-id="4b352-120">Gå till **Services** > **kibana**, och klicka på **redigera** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="4b352-120">Navigate to **Services** > **kibana**, and click **Edit** as shown below.</span></span>

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


<span data-ttu-id="4b352-122">Växla till **JSON-läget** och rulla ned till avsnittet etiketter.</span><span class="sxs-lookup"><span data-stu-id="4b352-122">Toggle to **JSON mode** and scroll down to the labels section.</span></span>
<span data-ttu-id="4b352-123">Du måste lägga till en `"HAPROXY_GROUP": "external"` post här som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="4b352-123">You need to add a `"HAPROXY_GROUP": "external"` entry here as shown below.</span></span>
<span data-ttu-id="4b352-124">När du klickar på **distribuera ändringar**, omstarter av behållare.</span><span class="sxs-lookup"><span data-stu-id="4b352-124">Once you click **Deploy changes**, your container restarts.</span></span>

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


<span data-ttu-id="4b352-126">Om du vill kontrollera att Kibana registreras som en tjänst i HAPROXY instrumentpanelen måste du öppna port 9090 på agenten klustret som HAPROXY körs på port 9090.</span><span class="sxs-lookup"><span data-stu-id="4b352-126">If you want to verify that Kibana is registered as a service in the HAPROXY dashboard, you need to open port 9090 on the agent cluster as HAPROXY runs on port 9090.</span></span>
<span data-ttu-id="4b352-127">Som standard det öppna portarna 80, 8080 och 443 i klustret för DC/OS-agenten.</span><span class="sxs-lookup"><span data-stu-id="4b352-127">By default, we open ports 80, 8080, and 443 in the DC/OS agent cluster.</span></span>
<span data-ttu-id="4b352-128">Instruktioner för att öppna en port och ge offentlig utvärdera [här](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="4b352-128">Instructions to open a port and provide public assess are provided [here](container-service-enable-public-access.md).</span></span>

<span data-ttu-id="4b352-129">Öppna Marathon-LB admin-gränssnittet på för att komma åt instrumentpanelen HAPROXY: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span><span class="sxs-lookup"><span data-stu-id="4b352-129">To access the HAPROXY dashboard, open the Marathon-LB admin interface at: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span></span>
<span data-ttu-id="4b352-130">När du navigerar till URL: en bör du se HAPROXY instrumentpanelen som visas nedan och du bör se en för Kibana.</span><span class="sxs-lookup"><span data-stu-id="4b352-130">Once you navigate to the URL, you should see the HAPROXY dashboard as shown below and you should see a service entry for Kibana.</span></span>

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


<span data-ttu-id="4b352-132">Om du vill komma åt instrumentpanelen Kibana som har distribuerats på port 5601, måste du öppna port 5601.</span><span class="sxs-lookup"><span data-stu-id="4b352-132">To access the Kibana dashboard, which is deployed on port 5601, you need to open port 5601.</span></span> <span data-ttu-id="4b352-133">Följ anvisningarna [här](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="4b352-133">Follow instructions [here](container-service-enable-public-access.md).</span></span> <span data-ttu-id="4b352-134">Öppna instrumentpanelen på Kibana: `http://localhost:5601`.</span><span class="sxs-lookup"><span data-stu-id="4b352-134">Then open the Kibana dashboard at: `http://localhost:5601`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b352-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b352-135">Next steps</span></span>

* <span data-ttu-id="4b352-136">För system- och logg vidarebefordran och inställningar, se [hantering i DC/OS med ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span><span class="sxs-lookup"><span data-stu-id="4b352-136">For system and application log forwarding and setup, see [Log Management in DC/OS with ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span></span>

* <span data-ttu-id="4b352-137">Om du vill filtrera loggarna finns [filtrering loggar med ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span><span class="sxs-lookup"><span data-stu-id="4b352-137">To filter logs, see [Filtering Logs with ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span></span> 

 

