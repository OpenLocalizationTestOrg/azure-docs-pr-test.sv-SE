---
title: "aaaScheduler hög tillgänglighet och tillförlitlighet"
description: "Schemaläggaren hög tillgänglighet och tillförlitlighet"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 5c9efb333eb42b393adc5deea657ca99206d425e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-high-availability-and-reliability"></a>Schemaläggaren hög tillgänglighet och tillförlitlighet
## <a name="azure-scheduler-high-availability"></a>Azure Scheduler hög tillgänglighet
Som en kärna Azure-plattformstjänsten Azure Schemaläggaren har hög tillgänglighet och funktioner både geo-redundant service-distributionen och geo regionala jobbet replikering.

### <a name="geo-redundant-service-deployment"></a>GEO-redundant tjänstdistribution
Azure Schemaläggaren är tillgänglig via hello Användargränssnittet i nästan alla geografisk region som är i Azure idag. hello listan över regioner som Azure Schemaläggaren finns i är [som listas här](https://azure.microsoft.com/regions/#services). Om ett datacenter i en värdbaserad region återges inte tillgänglig, är hello redundans funktionerna i Azure Schemaläggaren så att hello-tjänsten är tillgänglig från en annan datacenter.

### <a name="geo-regional-job-replication"></a>GEO-regionala jobbet replikering
Inte bara är hello Azure Scheduler frontend tillgänglig för av hanteringsbegäranden, men din egen jobbet är också georeplikerad. När det finns ett avbrott i en region, Azure Scheduler växlas över och garanterar hello jobbet körs från en annan datacenter i hello parad geografiskt område.

Till exempel om du har skapat ett jobb i södra centrala USA, replikerar Azure Scheduler automatiskt det jobbet i norra centrala USA. När det finns ett fel i södra centrala USA, säkerställer Azure Scheduler hello jobbet körs från norra centrala USA. 

![][1]

Därför garanterar Azure Scheduler att dina data förblir inom hello samma bredare geografiska region om ett fel uppstår på Azure. Därför kan du inte behöver duplicera din jobbet bara tooadd hög tillgänglighet – Azure Scheduler innehåller automatiskt funktioner för hög tillgänglighet för dina jobb.

## <a name="azure-scheduler-reliability"></a>Azure Scheduler tillförlitlighet
Azure Scheduler garanterar sin egen hög tillgänglighet och använder en annan metod toouser skapade jobb. Jobbet kan till exempel anropa en HTTP-slutpunkt som inte är tillgänglig. Azure Scheduler ändå försöker tooexecute ditt jobb, genom att ge dig alternativ toodeal med fel. Azure Scheduler gör detta på två sätt:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigurerbar försök princip via ”retryPolicy”
Azure Schemaläggaren kan du tooconfigure en återförsöksprincip. Som standard om ett jobb misslyckas försöker Schemaläggaren hello jobbet igen fyra gånger, med 30 sekunders intervall. Du kan konfigurera den här försök princip toobe mer aggressivt (till exempel tio gånger, med 30 sekunders mellanrum) eller glesare (till exempel två gånger med daglig intervall.)

Som ett exempel på när det kan hjälpa dig, kan du skapa ett jobb som körs en gång i veckan och anropar en HTTP-slutpunkt. Om hello HTTP-slutpunkten är igång på några timmar när jobbet körs kan kanske du inte vill toowait en mer vecka för hello jobbet toorun igen eftersom även hello försök standardprincipen misslyckas. I sådana fall måste du kanske konfigurera om hello standard försök princip tooretry alla tre timmar (till exempel) i stället för med 30 sekunders mellanrum.

hur tooconfigure en återförsöksprincip finns för toolearn[retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Den alternativa slutpunkten konfigurationsmöjligheter via ”errorAction”
Hello mål slutpunkten för din Azure Scheduler-jobbet är inte kan nås, faller Azure Scheduler tillbaka toohello alternativa felhantering slutpunkten när du har följt av dess återförsöksprincip. Om en annan felhantering slutpunkt har konfigurerats, anropar det Azure Schemaläggaren. Med en annan slutpunkt är egna jobb hög tillgänglighet i hello framsidan för felet.

Exempelvis i diagrammet nedan som hello följer Azure Scheduler dess försök princip toohit en New York-webbtjänst. När hello återförsök misslyckas, kontrollerar om det finns ett alternativ. Sedan går vidare och startar begäranden toohello alternativ med hello samma försök princip.

![][2]

Observera att hello samma återförsöksprincip gäller tooboth hello ursprungliga åtgärd och hello alternativa felåtgärd. Det är också möjligt toohave hello alternativa fel åtgärdens åtgärdstyp skilja sig från hello huvudåtgärden åtgärdstyp. Till exempel när hello huvudåtgärden kan anropa en HTTP-slutpunkt, kanske hello felåtgärd i stället queue storage, service bus-kö och service bus avsnittet åtgärd som har fel-loggning.

hur tooconfigure en annan slutpunkt finns för toolearn[errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Se även
 [Vad är Scheduler?](scheduler-intro.md)

 [Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler](scheduler-concepts-terms.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren](scheduler-advanced-complexity.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Azure Scheduler](scheduler-powershell-reference.md)

 [Gränser, standardinställningar och felkoder i Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Utgående autentisering i Azure Scheduler](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
