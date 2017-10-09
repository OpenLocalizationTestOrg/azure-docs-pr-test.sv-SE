---
title: aaaMonitor Azure Kubernetes kluster - Sysdig | Microsoft Docs
description: "Övervaka Kubernetes kluster i Azure Container Service med hjälp av Sysdig"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: a27813d01ee4624b9e993f6185169ad73aeec3a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Övervaka en Kubernetes för Azure Container Service-kluster med hjälp av Sysdig

## <a name="prerequisites"></a>Krav
Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).

Det förutsätts även att du har hello azure cli och kubectl tools har installerats.

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

## <a name="sysdig"></a>Sysdig
Sysdig är en extern övervakning som en tjänst-företag som du kan övervaka behållare i Kubernetes klustret körs i Azure. Med hjälp av Sysdig kräver ett aktivt Sysdig-konto.
Du kan registrera dig för ett konto sina [plats](https://app.sysdigcloud.com).

När du är inloggad på toohello Sysdig moln webbplats klickar du på ditt användarnamn och på sidan hello bör du se din ”åtkomstnyckeln”. 

![API-nyckel för Sysdig](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-hello-sysdig-agents-tookubernetes"></a>Installera hello Sysdig agenter tooKubernetes
toomonitor behållare Sysdig kör en process på varje dator med hjälp av en Kubernetes `DaemonSet`.
DaemonSets är Kubernetes API-objekt som kör en instans av en behållare per dator.
Det är perfekt för att installera verktyg som hello Sysdigs övervakningsagenten.

tooinstall hello Sysdig daemonset bör du först hämta [hello mallen](https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml) från sysdig. Spara filen som `sysdig-daemonset.yaml`.

Du kan köra på Linux och OS X:

```console
$ curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

I PowerShell:

```console
$ Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Redigera bredvid den filen tooinsert snabbtangent, som du fick från ditt konto Sysdig.

Skapa slutligen hello DaemonSet:

```console
$ kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>Visa övervakningen
När du installerade och körs, bör hello agenter pump data tillbaka tooSysdig.  Gå tillbaka toothe [sysdig instrumentpanelen](https://app.sysdigcloud.com) och du bör se information om din behållare.

Du kan också installera Kubernetes-specifika instrumentpaneler via den [guiden Ny instrumentpanel](https://app.sysdigcloud.com/#/dashboards/new).
