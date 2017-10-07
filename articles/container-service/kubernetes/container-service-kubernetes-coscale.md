---
title: aaaMonitor en Azure-Kubernetes kluster med CoScale | Microsoft Docs
description: "Övervaka ett Kubernetes kluster i Azure Container Service med hjälp av CoScale"
services: container-service
documentationcenter: 
author: fryckbos
manager: 
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f835e82d2be3afe1d85070bd0bf69649cc6dd2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Övervaka ett Azure Container Service Kubernetes kluster med CoScale

I den här artikeln visar vi dig hur toodeploy hello [CoScale](https://www.coscale.com/) agent toomonitor alla noder och behållare i din Kubernetes kluster i Azure Container Service. Du behöver ett konto med CoScale för den här konfigurationen. 


## <a name="about-coscale"></a>Om CoScale 

CoScale är en övervakning plattform som samlar in mätvärden och -händelser från alla behållare i flera orchestration-plattformar. CoScale ger fullständig stack övervakning för Kubernetes miljöer. Den innehåller visualiseringar och analytics för alla lager i hello stacken: hello OS, Kubernetes, Docker och program som körs på din behållare. CoScale erbjuder flera inbyggda övervakning instrumentpaneler, och den har inbyggda avvikelseidentifiering identifiering tooallow operatorer och utvecklare toofind infrastruktur- och problem för snabb.

![CoScale UI](./media/container-service-kubernetes-coscale/coscale.png)

Som det visas i den här artikeln, kan du installera agenter på en Kubernetes klustret toorun CoScale som ett SaaS-lösningen. Om du vill tookeep dina data på plats, är CoScale också tillgängligt för lokal installation.


## <a name="prerequisites"></a>Krav

Du måste först för[skapa ett konto för CoScale](https://www.coscale.com/free-trial).

Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).

Det förutsätts även att du har hello `az` Azure CLI och `kubectl` verktygen som installeras.

Du kan testa om du har hello `az` installerat genom att köra verktyget:

```azurecli
az --version
```

Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](/cli/azure/install-azure-cli).

Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:

```bash
kubectl version
```

Om du inte har `kubectl` installerat, kan du köra:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-hello-coscale-agent-with-a-daemonset"></a>Hej CoScale agent installeras med en DaemonSet
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) är används av Kubernetes toorun en instans av en behållare på varje värd i klustret hello.
Det är perfekt för att köra övervakningsagenter, till exempel hello CoScale agent.

När du loggar in tooCoScale går toohello [agent sidan](https://app.coscale.com/) tooinstall CoScale agenter på klustret med hjälp av en DaemonSet. Hej CoScale Användargränssnittet innehåller interaktiv configuration steg toocreate en agent och starta övervakning fullständig Kubernetes klustret.

![CoScale agentkonfiguration](./media/container-service-kubernetes-coscale/installation.png)

toostart hello agent på hello klustret kör hello angivna kommando:

![Starta hello CoScale agent](./media/container-service-kubernetes-coscale/agent_script.png)

Klart! När hello agenter är igång kan bör du se data i hello-konsolen på några minuter. Besök hello [agent sidan](https://app.coscale.com/) toosee en sammanfattning av klustret, utföra ytterligare konfigurationssteg och se instrumentpaneler, till exempel hello **Kubernetes kluster översikt**.

![Översikt över kluster Kubernetes](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

Hej CoScale agenten distribueras automatiskt på nya datorer i hello kluster. hello-agenten uppdateras automatiskt när en ny version släpps.


## <a name="next-steps"></a>Nästa steg

Se hello [CoScale dokumentationen](http://docs.coscale.com/) och [blogg](https://www.coscale.com/blog) för mer information om CoScale övervakningslösningar. 

