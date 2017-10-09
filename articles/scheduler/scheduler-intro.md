---
title: "aaaWhat är Azure Schemaläggaren? | Microsoft Docs"
description: "Azure Schemaläggaren kan du toodeclaratively beskrivs åtgärder toorun i hello molnet. Tjänsten schemalägger och kör sedan dessa åtgärder automatiskt."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 062e25ae473510264dc0038198c05e7ac1e86210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-scheduler"></a>Vad är Azure Scheduler?
Azure Schemaläggaren kan du toodeclaratively beskrivs åtgärder toorun i hello molnet. Tjänsten schemalägger och kör sedan dessa åtgärder automatiskt.  Schemaläggaren gör detta med hjälp av [hello Azure-portalen](scheduler-get-started-portal.md), kod, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), eller Azure PowerShell.

Scheduler skapar, underhåller och anropar schemalagt arbete.  Scheduler är inte värd för några arbetsbelastningar och kör ingen kod. Tjänsten *anropar* bara kod som finns på en annan plats – i Azure, lokalt eller med en annan provider. Den anropar via HTTP, HTTPS, en lagringskö, en Service Bus-kö eller ett Service Bus-ämne.

Schemaläggaren scheman [jobb](scheduler-concepts-terms.md), sparar en historik över jobb Körningsresultat att någon kan granska och deterministiskt och tillförlitligt scheman arbetsbelastningar toobe kör. Använda Schemaläggaren i bakgrunden hello Azure WebJobs (del av funktionen för hello Web Apps i Azure App Service) och andra funktioner i Azure schemaläggning. Hej [Scheduler REST API](https://msdn.microsoft.com/library/mt629143.aspx) hjälper dig att hantera hello kommunikation för dessa åtgärder. Scheduler har stöd för [komplexa scheman och avancerad upprepning](scheduler-advanced-complexity.md).

Det finns flera scenarier som lämpar sig toohello användning av Schemaläggaren. Exempel:

* *Åtgärder för återkommande program:* Samla in data från Twitter till en feed med jämna mellanrum.
* *Dagligt underhåll:* Daglig loggrensning, säkerhetskopiering och andra underhållsåtgärder. En administratör kan till exempel välja tooback hello databasen klockan 1:00 varje dag för hello nästa nio månader.

Schemaläggaren kan du toocreate, uppdatera, ta bort, visa och hantera jobb och [jobbet samlingar](scheduler-concepts-terms.md) programmässigt med hjälp av skript och hello-portalen.

## <a name="see-also"></a>Se även
 [Begrepp, terminologi och entitetshierarki relaterade till Azure Scheduler](scheduler-concepts-terms.md)

 [Komma igång med Schemaläggaren i hello Azure-portalen](scheduler-get-started-portal.md)

 [Prenumerationer och fakturering i Azure Scheduler](scheduler-plans-billing.md)

 [Hur toobuild komplex schemaläggs och avancerade upprepning med Azure Schemaläggaren](scheduler-advanced-complexity.md)

 [Referens för REST-API:et för Azure Scheduler](https://msdn.microsoft.com/library/mt629143)

 [Referens för PowerShell-cmdlets för Azure Scheduler](scheduler-powershell-reference.md)

 [Hög tillgänglighet och tillförlitlighet i Azure Scheduler](scheduler-high-availability-reliability.md)

 [Gränser, standardinställningar och felkoder i Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Utgående autentisering i Azure Scheduler](scheduler-outbound-authentication.md)

