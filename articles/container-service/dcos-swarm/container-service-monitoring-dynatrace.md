---
title: aaaMonitor Azure DC/OS-kluster - Dynatrace | Microsoft Docs
description: "Övervaka ett Azure Container Service DC/OS-kluster med Dynatrace. Distribuera hello Dynatrace OneAgent med hello DC/OS-instrumentpanelen."
services: container-service
documentationcenter: 
author: MartinGoodwell
manager: 
editor: 
tags: acs, azure-container-service
keywords: "Behållare, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 9e2e2d1c8b454422d1db30dac7c385d31d336853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>Övervaka ett Azure Container Service DC/OS-kluster med Dynatrace SaaS/hanteras
I den här artikeln visar vi dig hur toodeploy hello [Dynatrace](https://www.dynatrace.com/) OneAgent toomonitor alla hello agent noder i Azure Container Service-kluster. Du behöver ett konto med Dynatrace SaaS/hanteras för den här konfigurationen. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS/hanteras
Dynatrace är en moln-intern övervakningslösning för klustret miljöer och mycket dynamisk behållare. Det gör att du toobetter optimera din distribution i behållare och en minnesallokering med hjälp av data i realtid. Den är kapabel att automatiskt lokalisera programmet och infrastruktur problem genom att tillhandahålla automatisk baslinjeuppgifter och problemet korrelation grundorsaken identifiering.

hello följande bild visar hello Dynatrace UI:

![Dynatrace UI](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Krav 
[Distribuera](container-service-deployment.md) och [ansluta](./../container-service-connect.md) tooa kluster som har konfigurerats med Azure Container Service. Utforska hello [Marathon-Gränssnittet](container-service-mesos-marathon-ui.md). Gå för[https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) tooset en Dynatrace SaaS-konto.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Konfigurera Dynatrace distribution med Marathon
Dessa steg visar hur tooconfigure och distribuera Dynatrace program tooyour kluster med Marathon.

1. Åtkomst till ditt DC/OS-Gränssnittet via [http://localhost:80 /](http://localhost:80/). En gång i hello DC/OS-Gränssnittet, navigerar toohello **Universe** fliken och sök sedan efter **Dynatrace**.

    ![Dynatrace i DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. toocomplete hello konfiguration, behöver du ett Dynatrace SaaS-konto eller ett kostnadsfritt utvärderingskonto. När du loggar in hello Dynatrace instrumentpanelen välja **distribuera Dynatrace**.

    ![Dynatrace konfigurera PaaS-integrering](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Välj på sidan hello **ställa in integration PaaS**. 

    ![Dynatrace API-token](./media/container-service-monitoring-dynatrace/api-token.png) 

4. Ange din API-token i hello Dynatrace OneAgent konfiguration i hello DC/OS Universe. 

    ![Dynatrace OneAgent konfigurationen i hello DC/OS Universe](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Ange hello instanser toohello många noder du planerar toorun. Anger en hög siffra också fungerar, men DC/OS kommer fortsätta att försöka toofind nya instanser tills numret faktiskt har uppnåtts. Om du vill kan ange du också tooa värdet som 1000000. I det här fallet när en ny nod läggs toohello kluster, distribuerar Dynatrace automatiskt en agent toothat nya noden till hello pris DC/OS ständigt försök toodeploy ytterligare instanser.

    ![Dynatrace konfigurationen i hello DC/OS Universe-instanser](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Nästa steg

När du har installerat hello paketet kan gå tillbaka toohello Dynatrace instrumentpanelen. Du kan utforska hello olika användningsstatistik för hello behållare i klustret. 
