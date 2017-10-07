---
title: "aaaAzure Container Service tutorial – distribuera klustret | Microsoft Docs"
description: "Självstudiekurs för Azure Container Service - distribuera kluster"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c4c8cc95c88d9c2077d0322f57e5d3159e2dd0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Distribuera ett Kubernetes kluster i Azure Container Service

Kubernetes tillhandahåller en distribuerad plattform för av program. Med Azure Container Service är etablering av en klar Kubernetes produktionskluster snabbt och enkelt. I den här självstudiekursen, del 3 i 7, distribueras ett Azure Container Service Kubernetes klustret. Slutfört stegen innefattar:

> [!div class="checklist"]
> * Distribuera en Kubernetes ACS-kluster
> * Installation av hello Kubernetes CLI (kubectl)
> * Konfigurationen av kubectl

I efterföljande självstudiekurser hello Azure rösten program är distribuerat toohello kluster, skalas, uppdateras och Operations Management Suite är konfigurerade toomonitor hello Kubernetes kluster.

## <a name="before-you-begin"></a>Innan du börjar

I föregående självstudiekurser en behållare avbildning har skapats och överföra tooan Azure Container registret instans. Om du inte har gjort dessa steg och vill toofollow längs, returnerar för[kursen 1 – skapa behållaren bilder](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Skapa Kubernetes-kluster

I hello [tidigare kursen](./container-service-tutorial-kubernetes-prepare-acr.md), en resursgrupp med namnet *myResourceGroup* skapades. Om du inte har gjort det, skapa den här resursgruppen nu.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Skapa ett Kubernetes-kluster i Azure Container Service med hello [az acs skapa](/cli/azure/acs#create) kommando. 

hello följande exempel skapar ett kluster med namnet *myK8sCluster* med en Linux master-nod och tre noder för Linux-agenten.

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

Om några minuter hello-kommandot har slutförts och returnerar json-formaterad information om hello ACS-distribution.

## <a name="install-hello-kubectl-cli"></a>Installera hello kubectl CLI

tooconnect toohello Kubernetes kluster från din klientdator, Använd [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes kommandorads-klient. 

Om du använder Azure CloudShell är `kubectl` redan installerad. Om du vill tooinstall den lokalt, använda hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) kommando.

Om du kör i Linux eller macOS behöva toorun med sudo. Windows, se till att gränssnittet har körts som administratör.

```azurecli-interactive 
az acs kubernetes install-cli 
```

På Windows hello standardinstallation är *c:\program files (x86)\kubectl.exe*. Du kan behöva tooadd den här sökvägen toohello Windows. 

## <a name="connect-with-kubectl"></a>Ansluta med kubectl

tooconfigure `kubectl` tooconnect tooyour Kubernetes klustret, kör hello [az acs kubernetes get-autentiseringsuppgifter](/cli/azure/acs/kubernetes#get-credentials) kommando.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

tooverify hello anslutning tooyour klustret, kör hello [kubectl hämta noder](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) kommando.

```azurecli-interactive
kubectl get nodes
```

Resultat:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

I kursen slutförande har du ett Kubernetes ACS-kluster redo för arbetsbelastningar. I efterföljande självstudiekurser är ett program för flera behållare distribuerade toothis kluster, skala ut, uppdateras och övervakas.

## <a name="next-steps"></a>Nästa steg

Ett Azure Container Service Kubernetes kluster har distribuerats i den här självstudiekursen. hello följande steg har slutförts:

> [!div class="checklist"]
> * Distribuera ett Kubernetes ACS-kluster
> * Installerade hello Kubernetes CLI (kubectl)
> * Konfigurerade kubectl

Avancera toohello nästa självstudiekurs toolearn om att köra programmet på hello klustret.

> [!div class="nextstepaction"]
> [Distribuera program i Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)
