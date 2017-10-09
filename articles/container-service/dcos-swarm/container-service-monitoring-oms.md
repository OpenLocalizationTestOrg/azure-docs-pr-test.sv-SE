---
title: aaaMonitor Azure DC/OS-kluster - Verksamhetsstyrning | Microsoft Docs
description: "Övervaka ett Azure Container Service DC/OS-kluster med Microsoft Operations Management Suite."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 0ebfa3ba3cef8f0205b15731b0e91f5b304bc8fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a>Övervaka ett Azure Container Service DC/OS-kluster med Operations Management Suite

Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda din lokala och molnbaserade infrastruktur. Behållaren lösningen är en lösning i OMS logganalys som du kan visa hello behållaren lager, prestanda och loggar på en enda plats. Du kan granska, Felsök behållare genom att visa hello loggar på central plats och hitta störningar förbrukar överdriven behållare på en värd.

![](media/container-service-monitoring-oms/image1.png)

Mer information om behållaren lösning hittar toothe [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-hello-dcos-universe"></a>Konfigurera OMS från hello DC/OS universe


Den här artikeln förutsätter att du har skapat ett DC/OS och har distribuerat enkla behållaren webbprogram på hello klustret.

### <a name="pre-requisite"></a>Förhandskrav
- [Microsoft Azure-prenumeration](https://azure.microsoft.com/free/) -du kan få detta kostnadsfritt.  
- Installationsprogrammet för Microsoft OMS-arbetsytan - finns i ”steg 3” nedan
- [DC/OS CLI](https://dcos.io/docs/1.8/usage/cli/install/) installerad.

1. Hello DC/OS-instrumentpanelen, klicka på Universe och Sök efter ”OMS-enligt nedan.

![](media/container-service-monitoring-oms/image2.png)

2. Klicka på **Installera**. Du ser en pop upp med hello OMS versionsinformation och en **installera paket** eller **Installation i Avancerat** knappen. När du klickar på **Installation i Avancerat**, som leder dig toohello **OMS specifika konfigurationsegenskaper** sidan.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Här kan du bli ombedd tooenter hello `wsid` (hello OMS arbetsyte-ID) och `wskey` (hello OMS primär nyckel för hello arbetsyte-id). tooget båda `wsid` och `wskey` toocreate måste en OMS-konto på <https://mms.microsoft.com>. Följ hello steg toocreate ett konto. När du är klar skapar hello-konto måste tooobtain din `wsid` och `wskey` genom att klicka på **inställningar**, sedan **anslutna källor**, och sedan **Linux-servrar** , enligt nedan.

 ![](media/container-service-monitoring-oms/image5.png)

4. Välj hello tal du OMS-instanser som du vill använda och klicka hello, granska och installera'. Vanligtvis vill du toohave hello antalet OMS instanser lika toohello Virtuella datorer i klustret för agenten. OMS-Agent för Linux är en installation som enskilda behållare på varje virtuell dator att vill toocollect information för övervakning och loggning.

## <a name="setting-up-a-simple-oms-dashboard"></a>Konfigurera en enkel OMS-instrumentpanel

När du har installerat hello OMS-Agent för Linux på hello virtuella datorer, är nästa steg tooset in hello OMS-instrumentpanelen. Det finns två sätt toodo detta: OMS-portalen och Azure-portalen.

### <a name="oms-portal"></a>OMS-portalen 

Logga in toohello OMS-portalen (<https://mms.microsoft.com>) och gå toohello **lösning galleriet**.

![](media/container-service-monitoring-oms/image6.png)

När du är i hello **lösning galleriet**väljer **behållare**.

![](media/container-service-monitoring-oms/image7.png)

När du har valt hello behållaren lösning visas hello panelen på hello OMS översikt instrumentpanelssida. När hello inhämtas behållardata indexeras visas hello sida vid sida med information om lösningen visa paneler.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure Portal 

Inloggningen tooAzure portalen på <https://portal.microsoft.com/>. Gå till **Marketplace**väljer **övervakning + management** och på **finns alla**. Skriv `containers` i sökningen. Du ser ”behållare” i hello sökresultat. Välj **behållare** och på **skapa**.

![](media/container-service-monitoring-oms/image9.png)

När du klickar på **skapa**, uppmanas du för din arbetsyta. Välj din arbetsyta eller om du inte har någon kan du skapa en ny arbetsyta.

![](media/container-service-monitoring-oms/image10.PNG)

När du har valt din arbetsyta, klickar du på **skapa**.

![](media/container-service-monitoring-oms/image11.png)

Mer information om hello OMS behållaren lösning Se toothe [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).

### <a name="how-tooscale-oms-agent-with-acs-dcos"></a>Hur tooscale OMS-Agent med ACS DC/OS 

Om du behöver toohave installerat OMS-agent lite hello faktiska nodsantalet eller du proportionellt VMSS genom att lägga till fler Virtuella du göra det genom att skala hello `msoms` service.

Du kan gå tooMarathon eller hello DC/OS UI tjänstefliken och skala upp din antalet noder.

![](media/container-service-monitoring-oms/image12.PNG)

Det här distribuerar tooother noder som ännu inte har distribuerat hello OMS-agent.

## <a name="uninstall-ms-oms"></a>Avinstallera MS OMS

toouninstall MS OMS ange hello följande kommando:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Berätta för oss!
Vad fungerar? Vad som saknas? Vad behöver du för den här toobe användbara för dig? Berätta för oss på <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Nästa steg

 Nu när du har ställt in OMS toomonitor din behållare[finns i instrumentpanelen behållaren](../../log-analytics/log-analytics-containers.md).
