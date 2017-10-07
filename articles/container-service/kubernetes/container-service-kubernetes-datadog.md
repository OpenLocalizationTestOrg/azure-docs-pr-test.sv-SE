---
title: aaaMonitor Azure Kubernetes kluster med Datadog | Microsoft Docs
description: "Övervaka Kubernetes kluster i Azure Container Service med hjälp av Datadog"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: bccd8b59a048e0f791172fcfc4eeba6370dafcc0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Övervaka ett Azure Container Service-kluster med DataDog

## <a name="prerequisites"></a>Krav
Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).

Det förutsätts även att du har hello `az` Azure cli och `kubectl` verktygen som installeras.

Du kan testa om du har hello `az` installerat genom att köra verktyget:

```console
$ az --version
```

Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).

Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:

```console
$ kubectl version
```

Om du inte har `kubectl` installerat, kan du köra:

```console
$ az acs kubernetes install-cli
```

## <a name="datadog"></a>DataDog
Datadog är en övervakningstjänsten som samlar in övervakningsdata från din behållare i Azure Container Service-kluster. Datadog har en Docker-integrering instrumentpanel där du kan se specifika mått i en behållare. Mätvärden som samlats in från behållarna ordnas efter CPU, minne, nätverk och i/o. Datadog delar mått i behållare och bilder.

Du måste först för[skapa ett konto](https://www.datadoghq.com/lpg/)

## <a name="installing-hello-datadog-agent-with-a-daemonset"></a>Installera hello Datadog Agent med en DaemonSet
DaemonSets används av Kubernetes toorun en instans av en behållare på varje värd i hello kluster.
Det är perfekt för att köra övervakningsagenter.

När du har loggat in på Datadog, du kan följa hello [Datadog instruktioner](https://app.datadoghq.com/account/settings#agent/kubernetes) tooinstall Datadog agenter på klustret med hjälp av en DaemonSet.

## <a name="conclusion"></a>Slutsats
Klart! När hello agenter är igång och kör du bör se data i hello-konsolen på några minuter. Du besöker hello integrerad [kubernetes instrumentpanelen](https://app.datadoghq.com/screen/integration/kubernetes) toosee en sammanfattning av klustret.
