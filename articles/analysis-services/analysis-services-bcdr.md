---
title: "aaaAzure hög tillgänglighet för Analysis Services | Microsoft Docs"
description: "Säkerställa hög tillgänglighet för Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 6e09536c5bd7dc7f88f9b662e1a9f42d2b8c969b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analysis-services-high-availability"></a>Hög tillgänglighet för Analysis Services
Den här artikeln beskriver garantera hög tillgänglighet för Azure Analysis Services-servrar. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Säkerställa hög tillgänglighet vid ett avbrott i tjänsten
När det är sällsynt, kan ett Azure-Datacenter ha ett avbrott. När ett avbrott inträffar gör en business-avbrott som kan senaste några minuter eller kan senaste timmar. Hög tillgänglighet uppnås ofta med server redundans. Du kan uppnå redundans med Azure Analysis Services genom att skapa ytterligare sekundära servrar i en eller flera regioner. När du skapar redundanta servrar, tooassure hello data och metadata på dessa servrar är synkroniserad med hello-server i en region som är offline, kan du:

* Distribuera modeller tooredundant servrar i andra regioner. Den här metoden kräver databearbetning på både primära servern och redundanta servrar i parallellt, så alla servrar är synkroniserad.

* Säkerhetskopiera databaser från den primära servern och återställning på redundanta servrar. Du kan till exempel automatisera säkerhetskopieringar tooAzure lagring och återställa tooother redundanta servrar i andra regioner. 

Om den primära servern skulle få ett avbrott i båda fallen måste du ändra hello anslutningssträngar i rapportserver klienter tooconnect toohello i en annan regionala datacenter. Den här ändringen ska betraktas som en sista utväg och endast om ett oåterkalleligt regionala data center avbrott inträffar. Det är troligt att en data center avbrott som värd för den primära servern skulle komma online igen innan du kan uppdatera anslutningar på alla klienter. 



## <a name="related-information"></a>Relaterad information
[Säkerhetskopiering och återställning](analysis-services-backup.md)   
[Hantera Azure Analysis Services](analysis-services-manage.md) 

