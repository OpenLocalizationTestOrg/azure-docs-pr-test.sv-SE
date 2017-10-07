---
title: "aaaDC/OS-agenten pooler för Azure Container Service | Microsoft Docs"
description: "Så här fungerar hello offentliga och privata agent pooler med ett Azure Container Service DC/OS-kluster"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, behållare, Micro-tjänster, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: c7d3889db07cb9908e8b68b668bd8a14ef3c2552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-agent-pools-for-azure-container-service"></a>DC/OS-agenten pooler för Azure Container Service
DC/OS-kluster i Azure Container Service innehålla agent noder i två pooler, en pool med offentliga och privata poolen. Ett program kan distribueras tooeither poolen, påverkar tillgängligheten mellan datorer i container service. hello datorer kan vara synliga toohello internet (offentlig) eller hålls intern (privat). Den här artikeln ger en kort översikt över därför offentliga och privata pooler.


* **Privata agenter**: privat agent noder kör via ett icke-dirigerbara nätverk. Det här nätverket är endast tillgänglig från hello admin-zon eller via hello offentliga zon gränsrouter. Som standard startar DC/OS appar på privata agent noder. 

* **Offentliga agenter**: offentlig agent noder kör DC/OS-appar och tjänster via ett allmänt tillgängligt nätverk. 

Mer information om nätverkssäkerhet för DC/OS finns hello [DC/OS-dokumentationen](https://dcos.io/docs/1.7/administration/securing-your-cluster/).

## <a name="deploy-agent-pools"></a>Distribuera agenten pooler

hello DC/OS-agenten pooler i Azure Container Service skapas på följande sätt:

* Hej **privata poolen** innehåller hello antal agent noder som du anger när du [distribuera hello DC/OS-klustret](container-service-deployment.md). 

* Hej **offentliga poolen** ursprungligen innehåller ett förbestämt antal agent noder. Den här poolen läggs till automatiskt när hello DC/OS-klustret har etablerats.

hello privata poolen och hello offentliga poolen är skalningsuppsättningar i virtuella Azure-datorn. Du kan ändra dessa pooler efter distributionen.

## <a name="use-agent-pools"></a>Använda agent-pooler
Som standard **Marathon** distribuerar några nya program toohello *privata* agent noder. Du måste distribuera hello programmet toohello tooexplicitly *offentliga* noder under hello skapandet av programmet hello. Välj hello **valfritt** och ange **slave_public** för hello **accepterade resursroller** värde. Den här processen dokumenteras [här](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) och i hello [DC/OS](https://dcos.io/docs/1.7/administration/installing/custom/create-public-agent/) dokumentation.

## <a name="next-steps"></a>Nästa steg
* Läs mer om [hantera DC/OS-behållare](container-service-mesos-marathon-ui.md).

* Lär dig hur för[öppna hello brandväggen](container-service-enable-public-access.md) tillhandahålls av Azure tooallow offentlig åtkomst tooyour DC/OS-behållare.

