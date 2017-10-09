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
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a>Övervaka ett Azure Container Service-kluster med ELK
I den här artikeln visar hur toodeploy hello ELK (Elasticsearch Logstash, Kibana) stacken på ett DC/OS-kluster i Azure Container Service. 

## <a name="prerequisites"></a>Krav
[Distribuera](container-service-deployment.md) och [ansluta](../container-service-connect.md) ett DC/OS-kluster som är konfigurerat med Azure Container Service. Utforska hello instrumentpanelen för DC/OS och Marathon tjänster [här](container-service-mesos-marathon-ui.md). Även installera hello [Marathon belastningsutjämnaren](container-service-load-balancing.md).


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch Logstash, Kibana)
ELK stacken är en kombination av Elasticsearch Logstash, och Kibana som ger en end tooend stapel som kan använda toomonitor och analysera loggar i klustret.

## <a name="configure-hello-elk-stack-on-a-dcos-cluster"></a>Konfigurera hello ELK stack på ett DC/OS-kluster
Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/) en gång i hello DC/OS-Gränssnittet navigera för**Universe**. Söka efter och installera Elasticsearch och Logstash Kibana från hello DC/OS Universe och i den specifika ordningen. Du kan lära dig mer om konfiguration om du går toohello **Installation i Avancerat** länk.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

En gång Hej ELK behållare och är igång, behöver du tooenable Kibana toobe nås via Marathon-LB. Navigera för **Services** > **kibana**, och klicka på **redigera** enligt nedan.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


Växla för**JSON-läget** och rulla ned toohello etiketter avsnitt.
Du behöver tooadd en `"HAPROXY_GROUP": "external"` post här som visas nedan.
När du klickar på **distribuera ändringar**, omstarter av behållare.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Om du vill tooverify som Kibana registreras som en tjänst i hello HAPROXY instrumentpanelen måste du tooopen port 9090 på hello agent klustret som HAPROXY körs på port 9090.
Som standard det öppna portarna 80, 8080 och 443 i hello DC/OS-klustret för agenten.
Instruktioner tooopen en port och ange offentliga utvärdera tillhandahålls [här](container-service-enable-public-access.md).

tooaccess hello HAPROXY instrumentpanelen, öppna hello Marathon-LB administratörsgränssnittet på: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.
När du navigerar toohello URL bör du se hello HAPROXY instrumentpanelen som visas nedan och du bör se en för Kibana.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


tooaccess hello Kibana instrumentpanelen, som har distribuerats på port 5601, du behöver tooopen port 5601. Följ anvisningarna [här](container-service-enable-public-access.md). Öppna hello Kibana instrumentpanelen på: `http://localhost:5601`.

## <a name="next-steps"></a>Nästa steg

* För system- och logg vidarebefordran och inställningar, se [hantering i DC/OS med ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* toofilter loggar finns [filtrering loggar med ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

