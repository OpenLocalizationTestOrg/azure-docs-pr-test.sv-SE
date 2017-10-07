---
title: "aaaIntroduction tooAzure Container Service för Kubernetes | Microsoft Docs"
description: "Azure Container Service för Kubernetes gör det enkelt toodeploy och hantera behållare-baserade program på Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: bfc85a180bdf4a405c9047eb882d3eed01640dd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-container-service-for-kubernetes"></a>Introduktion tooAzure Container Service för Kubernetes
Azure Container Service för Kubernetes gör det enkelt toocreate, konfigurera och hantera kluster för virtuella datorer som är förkonfigurerad toorun av program. Detta gör att du toouse dina befintliga kunskaper eller rita på en stor och växande uppsättning community kunskaper, toodeploy och hantera behållare-baserade program i Microsoft Azure.

Med Azure Container Service kan du dra nytta av hello företagsklass funktioner i Azure, men har ändå programmet överföring via Kubernetes och hello Docker bildformat.

## <a name="using-azure-container-service-for-kubernetes"></a>Använda Azure Container Service för Kubernetes
Vårt mål med Azure Container Service är tooprovide värdmiljön en behållare med öppen källkod verktyg och tekniker som är populärt bland våra kunder idag. toothis slutet kan du använda hello standard Kubernetes API-slutpunkter. Du kan utnyttja all programvara som kan prata tooa Kubernetes kluster med hjälp av dessa slutpunkter som standard. Du kan till exempel välja [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) eller [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service"></a>Skapa ett Kubernetes-kluster med Azure Container Service
toobegin med Azure Container Service distribuera ett Azure Container Service-kluster med hello [Azure CLI 2.0](container-service-kubernetes-walkthrough.md) eller via hello-portal (Sök hello Marketplace för **Azure Container Service**). Om du är en avancerad användare som behöver mer kontroll över hello Azure Resource Manager-mallar kan du använda hello öppen källkod [acs-motorn](https://github.com/Azure/acs-engine) projekt toobuild egna anpassade Kubernetes kluster och distribuera det via hello `az` CLI.

### <a name="using-kubernetes"></a>Använda Kubernetes
Kubernetes automatiserar distributionen, skalningen och hanteringen av program som använder behållare. Det har en omfattande uppsättning funktioner. Till exempel:
* Automatisk paketering
* Självåterställning
* Horisontell skalning
* Tjänstidentifiering och belastningsutjämning
* Automatiserade distributioner och återställningar
* Hemlighets- och konfigurationshantering
* Storage-dirigering
* Batch-körning

Arkitekturdiagram över Kubernetes som distribuerats via Azure Container Service:

![Azure Container Service konfigurerad toouse Kubernetes.](media/acs-intro/kubernetes.png)

## <a name="videos"></a>Videoklipp

Stöd för Kubernetes i Azure Container Service (Azure fredag, januari 2017):

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Kubernetes-Support-in-Azure-Container-Services/player]
>
>

Verktyg för utveckling och distribution av program på Kubernetes (Azure OpenDev, juni 2017):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

## <a name="next-steps"></a>Nästa steg

Utforska hello [Kubernetes Quickstart](container-service-kubernetes-walkthrough.md) toobegin utforska Azure Container Service idag.
