---
title: aaaMonitor ett Azure Container Service-kluster med Sysdig | Microsoft Docs
description: "Övervaka ett Azure Container Service-kluster med Sysdig."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 72f2d3d6f6885f9876fa158b88aae58b84a4610f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Övervaka ett Azure Container Service-kluster med Sysdig
I den här artikeln ska vi distribuera Sysdig agenter tooall hello agent noder i Azure Container Service-kluster. Du behöver ett konto hos Sysdig för den här konfigurationen. 

## <a name="prerequisites"></a>Krav
[Distribuera](container-service-deployment.md) och [anslut](../container-service-connect.md) ett kluster som konfigurerats av Azure Container Service. Utforska hello [Marathon-Gränssnittet](container-service-mesos-marathon-ui.md). Gå för[http://app.sysdigcloud.com](http://app.sysdigcloud.com) tooset en Sysdig moln-konto. 

## <a name="sysdig"></a>Sysdig
Sysdig är en övervakningstjänsten som gör att du toomonitor din behållare i klustret. Sysdig är känt toohelp med felsökning har bara din grundläggande övervakning mått för CPU-, nätverks-, minnes- och i/o. Sysdig gör det enkelt toosee som fungerar som behållare hello hardest eller i stort sett använder hello de flesta minne och processor. Den här vyn är i hello ”Overview” avsnittet, vilket är för närvarande i betaversion. 

![Sysdig-gränssnitt](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurera en Sysdig-distribution med Marathon
Dessa steg visar hur tooconfigure och distribuera Sysdig program tooyour kluster med Marathon. 

Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/) en gång i hello DC/OS-Gränssnittet går toohello ”universum” som finns på hello nedre vänster och sedan söka efter ”Sysdig”.

![Sysdig i DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig1.png)

Nu toocomplete hello konfiguration du behöver en Sysdig moln konto eller ett kostnadsfritt utvärderingskonto. När du är inloggad på toohello Sysdig moln webbplats klickar du på ditt användarnamn och på sidan hello bör du se din ”åtkomstnyckeln”. 

![API-nyckel för Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

Bredvid ange din åtkomstnyckel i hello Sysdig konfiguration i hello DC/OS Universe. 

![Sysdig konfigurationen i hello DC/OS Universe](./media/container-service-monitoring-sysdig/sysdig3.png)

Ange hello instanser too10000000 så när en ny nod läggs toohello klustret Sysdig automatiskt distribuerar en agent toothat nya noden. Det här är en tillfällig lösning toomake att Sysdig distribuerar tooall nya agenter inom hello kluster. 

![Sysdig konfigurationen i hello DC/OS Universe-instanser](./media/container-service-monitoring-sysdig/sysdig4.png)

När du har installerat paketet hello navigera tillbaka toohello Sysdig UI och du kan tooexplore hello olika användningsstatistik för hello behållare inom klustret. 

Du kan även installera Mesos- och Marathon-specifika instrumentpaneler via [den nya instrumentpanelsguiden](https://app.sysdigcloud.com/#/dashboards/new).
