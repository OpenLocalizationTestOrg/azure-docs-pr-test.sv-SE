---
title: aaaMonitor Azure Kubernetes kluster - Verksamhetsstyrning | Microsoft Docs
description: "Övervaka Kubernetes kluster i Azure Container Service med hjälp av Microsoft Operations Management Suite"
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
ms.openlocfilehash: 7474ee1571134ffe43ff8e4041cf5a64f5635bb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a>Övervaka ett Azure Container Service-kluster med Microsoft Operations Management Suite (OMS)

## <a name="prerequisites"></a>Krav
Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).

Det förutsätts även att du har hello `az` Azure cli och `kubectl` verktygen som installeras.

Du kan testa om du har hello `az` installerat genom att köra verktyget:

```console
$ az --version
```

Om du inte har hello `az` verktyget är installerat, det finns instruktioner [här](https://github.com/azure/azure-cli#installation).  
Du kan också använda [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), som har hello `az` Azure cli och `kubectl` verktyg som redan har installerat för du.  

Du kan testa om du har hello `kubectl` installerat genom att köra verktyget:

```console
$ kubectl version
```

Om du inte har `kubectl` installerat, kan du köra:
```console
$ az acs kubernetes install-cli
```

tootest om du har kubernetes nycklar som installeras i kubectl-verktyg som du kan köra:
```console
$ kubectl get nodes
```

Om hello ovan kommandot fel ut, behöver du tooinstall kubernetes klustret nycklar till kubectl-verktyget. Du kan göra det med hello följande kommando:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a>Övervakning av behållare med Operations Management Suite (OMS)

Microsoft Operations Management (OMS) är Microsofts molnbaserade IT lösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur. Behållaren lösningen är en lösning i OMS logganalys som du kan visa hello behållaren lager, prestanda och loggar på en enda plats. Du kan granska, Felsök behållare genom att visa hello loggar på central plats och hitta störningar förbrukar överdriven behållare på en värd.

![](media/container-service-monitoring-oms/image1.png)

Mer information om behållaren lösning hittar toothe [behållare lösning logganalys](../../log-analytics/log-analytics-containers.md).

## <a name="installing-oms-on-kubernetes"></a>Installera OMS på Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Hämta ditt arbetsyte-ID och nyckel
Hello OMS tootalk toohello-agenttjänsten måste toobe som konfigurerats med en arbetsyte-id och en arbetsyta. tooget hello arbetsyte-id och du behöver toocreate en OMS-kontot på <https://mms.microsoft.com>. Följ hello steg toocreate ett konto. När du är klar skapar hello-konto måste tooobtain ditt id och nyckel genom att klicka på **inställningar**, sedan **anslutna källor**, och sedan **Linux-servrar**, enligt nedan.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-hello-oms-agent-using-a-daemonset"></a>Installera hello OMS-agent med en DaemonSet
DaemonSets används av Kubernetes toorun en instans av en behållare på varje värd i hello kluster.
Det är perfekt för att köra övervakningsagenter.

Här är hello [DaemonSet YAML filen](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Spara den tooa filen namnet `oms-daemonset.yaml` och Ersätt hello platshållare värden för `WSID` och `KEY` med ditt arbetsyte-id och nyckel i hello-filen.

När du har lagt till ditt arbetsyte-ID och nyckel toohello DaemonSet konfiguration, kan du installera hello OMS-agent på ditt kluster med hello `kubectl` kommandoradsverktyget:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-hello-oms-agent-using-a-kubernetes-secret"></a>Installera med hjälp av en Kubernetes hemlighet hello OMS-agent
tooprotect din OMS arbetsyte-ID och du kan använda Kubernetes hemlighet som en del av DaemonSet YAML-fil.

 - Kopiera hello skript, hemliga mallfilen och hello DaemonSet YAML-fil (från [databasen](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) och kontrollera att de är på hello samma katalog. 
      - Hemligt genererar skript - hemlighet gen.sh
      - Hemlig mall - hemlighet template.yaml
   - Filen DaemonSet YAML - omsagent-ds-secrets.yaml
 - Kör skriptet hello. hello skriptet begär hello OMS arbetsyte-ID och primärnyckel. In sätt som och hello skriptet skapar en hemlig yaml-fil så att du kan köra den.   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - Skapa hello hemligheter baljor genom att köra hello följande:``` kubectl create -f omsagentsecret.yaml ```
 
   - toocheck kör hello följande: 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - Skapa din omsagent daemon set genom att köra``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Slutsats
Klart! Efter några minuter bör du kunna toosee data som flödar tooyour OMS-instrumentpanelen.
