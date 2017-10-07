---
title: "aaaAzure behållaren registret databaser | Microsoft Docs"
description: "Hur toouse Azure Container registret databaser för Docker bilder"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 06172a63465838a78a607f268da116d8158789ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-registry-repositories"></a>Azure-behållaren registret databaser

Azure Container register är kompatibla med en mängd olika tjänster och orchestrators. toomake den enklare tootrack hello källa tjänster och agenter som ACR används, har vi igång med hello Docker huvudfältet i hello Docker.config fil.



## <a name="viewing-repositories-in-hello-portal"></a>Visa databaser i hello Portal

Hej ACR huvuden följer hello format:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Molnet: Azure, Azure Stack eller andra myndigheter eller landsspecifika Azure-moln. Även om Azure-stacken och offentliga moln inte stöds för närvarande, kan den här parametern framtida stöd.
* Tjänst: namnet på hello-tjänst.
* Optionalservicename: valfri parameter för tjänster med subservices eller toospecify en SKU (ex: webbappar motsvarar Azure/app-/-webbappar).

Partnertjänster och orchestrators är rekommenderar toouse-specifikt huvud värden toohelp med våra telemetri. Användare kan också ändra hello-värdet som skickas toohello huvudet om de så önskar.

hello-värden som vi vill ACR partners toouse toopopulate hello ”X-Meta-käll-klient” fältet är nedan:

| Tjänstnamn              | Huvudet                                |
| ------------------------- | ------------------------------------- |
| Azure Container Service   | Azure/beräkning/azure-behållaren-tjänst |
| App Service - Webbappar    | Azure/app-/-webbappar            |
| Apptjänst - Logic Apps  | Azure/app-tjänsten /-logikappar          |
| Batch                     | batch-Azure/beräkning                   |
| Cloud-konsolen             | Azure-cloud-konsolen                   |
| Funktioner                 | Azure-beräknings-funktioner               |
| Internet saker - hubb  | Azure-iot-hubb                         |
| HDInsight                 | hdinsight-Azure/data                  |
| Jenkins                   | Azure/jenkins                         |
| Machine Learning          | Azure/data/machile-utbildning           |
| Service Fabric            | Azure/beräkning/service-infrastrukturresurser          |
| VSTS                      | Azure/vsts                            |


## <a name="next-steps"></a>Nästa steg
[Mer information om register och hello stöds tjänster och orchestrators](container-registry-intro.md)
