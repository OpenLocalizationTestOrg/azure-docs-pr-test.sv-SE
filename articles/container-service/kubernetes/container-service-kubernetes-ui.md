---
title: "aaaManage Azure Kubernetes kluster med webbgränssnittet | Microsoft Docs"
description: "Med hjälp av hello webbgränssnitt Kubernetes i Azure Container Service"
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
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e24ea0b82c94d2fd4610e4442699ef756590e6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-kubernetes-web-ui-with-azure-container-service"></a>Med hjälp av hello webbgränssnitt Kubernetes med Azure Container Service

## <a name="prerequisites"></a>Krav
Den här genomgången förutsätter att du har [skapas ett Kubernetes-kluster med Azure Container Service](container-service-kubernetes-walkthrough.md).


Det förutsätts även att du har hello Azure CLI 2.0 och `kubectl` verktygen som installeras.

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

## <a name="overview"></a>Översikt

### <a name="connect-toohello-web-ui"></a>Ansluta toohello webbgränssnittet
Du kan starta hello Kubernetes webbgränssnittet genom att köra:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Detta ska öppna en webbläsare konfigurerats tootalk tooa säker webbproxy ansluta din lokala dator toohello Kubernetes webbgränssnittet.

### <a name="create-and-expose-a-service"></a>Skapa och visa en tjänst
1. Klicka på hello Kubernetes webbgränssnittet, **skapa** knappen i hello övre högra fönstret.

    ![Kubernetes skapa gränssnitt](./media/container-service-kubernetes-ui/create.png)

    En öppnas dialogrutan där du kan börja skapa ditt program.

2. Namnge den hello `hello-nginx`. Använd hello [ `nginx` behållare från Docker](https://hub.docker.com/_/nginx/) och distribuera tre repliker av den här webbtjänsten.

    ![Dialogrutan Skapa i Kubernetes baljor](./media/container-service-kubernetes-ui/nginx.png)

3. Under **Service**väljer **externa** och ange port 80.

    Den här inställningen belastningsutjämnas trafik toohello tre repliker.

    ![Dialogrutan Skapa i Kubernetes Service](./media/container-service-kubernetes-ui/service.png)

4. Klicka på **distribuera** toodeploy dessa behållare och tjänster.

    ![Distribuera Kubernetes](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Visa behållarna
När du klickar på **distribuera**, hello UI visas en vy över din tjänst som distribueras:

![Kubernetes Status](./media/container-service-kubernetes-ui/status.png)

Du kan se hello status för varje Kubernetes objekt i hello cirkel hello vänster på användargränssnittet under **skida**. Om det är en delvis fylld cirkel distribuera hello objektet fortfarande. När ett objekt har distribuerats helt, visas en grön bock:

![Kubernetes distribueras](./media/container-service-kubernetes-ui/deployed.png)

När allt körs klickar du på något av skida toosee detaljer om hello som kör webbtjänst.

![Kubernetes skida](./media/container-service-kubernetes-ui/pods.png)

I hello **skida** kan du se information om hello behållare i hello baljor samt hello CPU och minne resurser som används av dessa behållare:

![Kubernetes resurser](./media/container-service-kubernetes-ui/resources.png)

Om du inte ser hello resurser, kan du behöva toowait några minuter för hello övervakning data toopropagate.

toosee hello loggar för din behållaren, klicka på **visa loggar**.

![Kubernetes loggar](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Visa din tjänst
I tillägg toorunning din behållare hello Kubernetes UI har skapat en extern `Service` som etablerar en belastningen belastningsutjämnaren toobring trafik toohello behållare i klustret.

Hello vänstra navigeringsfönstret, klicka på **Services** tooview alla tjänster (det ska bara finnas ett).

![Kubernetes tjänster](./media/container-service-kubernetes-ui/service-deployed.png)

Du bör se den externa slutpunkten (IP-adress) som har allokerats tooyour service i denna vy.
Om du klickar på att IP-adress, bör du se din Nginx-behållaren som körs bakom belastningsutjämnaren.

![nginx-vyn](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Ändra storlek på tjänsten
I tillägg tooviewing Hej dina objekt i Användargränssnittet, kan du redigera och uppdatera hello Kubernetes API-objekt.

Klicka först på **distributioner** i hello lämnas fönstret navigering toosee hello distributionen för tjänsten.

När du är i vyn klickar du på hello replikuppsättningen och klicka sedan på **redigera** i hello övre navigeringsfältet:

![Redigera Kubernetes](./media/container-service-kubernetes-ui/edit.png)

Redigera hello `spec.replicas` fältet toobe `2`, och klicka på **uppdatering**.

Detta medför hello antal repliker toodrop tootwo genom att ta bort en av dina skida.

 

