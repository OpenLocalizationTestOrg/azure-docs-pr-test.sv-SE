---
title: aaaMonitor Azure DC/OS-kluster - Datadog | Microsoft Docs
description: "Övervaka ett Azure Container Service-kluster med Datadog. Använda hello DC/OS web UI toodeploy hello Datadog agenter tooyour kluster."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Behållare med DC/OS, Docker Swarm Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 10268c04b5c2ef393429e706ed4a467fff80f718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Övervaka ett Azure Container Service DC/OS-kluster med Datadog
I den här artikeln ska vi distribuera Datadog agenter tooall hello agent noder i Azure Container Service-kluster. Du behöver ett konto med Datadog för den här konfigurationen. 

## <a name="prerequisites"></a>Krav
[Distribuera](container-service-deployment.md) och [anslut](../container-service-connect.md) ett kluster som konfigurerats av Azure Container Service. Utforska hello [Marathon-Gränssnittet](container-service-mesos-marathon-ui.md). Gå för[http://datadoghq.com](http://datadoghq.com) tooset en Datadog-konto. 

## <a name="datadog"></a>Datadog
Datadog är en övervakningstjänsten som samlar in övervakningsdata från din behållare i Azure Container Service-kluster. Datadog har en Docker-integrering instrumentpanel där du kan se specifika mått i en behållare. Mätvärden som samlats in från behållarna ordnas efter CPU, minne, nätverk och i/o. Datadog delar mått i behållare och bilder. Ett exempel på vilka hello UI verkar för CPU-användningen är nedan.

![Datadog UI](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurera Datadog distribution med Marathon
Dessa steg visar hur tooconfigure och distribuera Datadog program tooyour kluster med Marathon. 

Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/). En gång i hello DC/OS-Gränssnittet navigera toohello ”universum” som finns på hello nedre vänster och sedan söka efter ”Datadog” och klicka på ”installera”.

![Datadog paketet i hello DC/OS Universe](./media/container-service-monitoring/datadog1.png)

Nu toocomplete hello konfigurationen behöver du ett Datadog konto eller ett kostnadsfritt utvärderingskonto. När du är inloggad titta i toohello Datadog webbplats toohello vänster och gå sedan-tooIntegrations > [API: er](https://app.datadoghq.com/account/settings#api). 

![Datadog API-nyckel](./media/container-service-monitoring/datadog2.png)

Bredvid ange API-nyckel i hello Datadog konfiguration i hello DC/OS Universe. 

![Datadog konfigurationen i hello DC/OS Universe](./media/container-service-monitoring/datadog3.png) 

I hello ovan configuration anges instanser too10000000 så när en ny nod läggs toohello klustret Datadog automatiskt distribuerar en agent toothat nod. Det här är en tillfällig lösning. När du har installerat hello paketet ska du gå tillbaka toohello Datadog webbplats och hitta ”[instrumentpaneler](https://app.datadoghq.com/dash/list)”. Därifrån ser anpassad och integrering instrumentpaneler. Hej [Docker instrumentpanelen](https://app.datadoghq.com/screen/integration/docker) har alla hello behållaren mått som du behöver för att övervaka ditt kluster. 

