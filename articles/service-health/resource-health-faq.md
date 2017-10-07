---
title: "aaaAzure Resource Health vanliga frågor och svar | Microsoft Docs"
description: "Översikt över Azure Resource Health"
services: Resource health
documentationcenter: dev-center-name
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/05/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: fe29b2df1f9fee4b77d0100c7d872df837dc4b4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-faq"></a>Azure Resource Health vanliga frågor och svar
Lär dig hello svar toocommon frågor om Azure Resource Health.

## <a name="what-is-azure-resource-health"></a>Vad är Azure Resource Health?
Resource Health hjälper dig att diagnostisera och få support när ett problem med Azure påverkar dina resurser. Det informerar om hello aktuella och tidigare dina resursers hälsa och hjälper dig att minska problem. Resource Health ger teknisk support när du behöver hjälp med problem med Azure-tjänster.  

## <a name="what-is-hello-resource-health-intended-for"></a>Vad är hello Resource Health avsedd för?
När ett problem med en resurs har identifierats hjälper Resource Health dig att diagnostisera hello orsaken. Den innehåller hjälp toomitigate hello problemet och teknisk support om du behöver mer hjälp med problem med Azure-tjänsten.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Vilka kontroll utförs av Resource Health?
Resurshälsa kontrollerar olika baserat på hello [resurstypen](resource-health-checks-resource-types.md). Dessa kontroller är utformad tooimplement tre typer av problem: 
- Oplanerad händelser, till exempel en oväntade värden omstart
- Planerad händelser, t.ex. uppdateringar schemalagda värdens operativsystem
- Händelser som utlösts av användaråtgärder, till exempel en användare startar om en virtuell dator

## <a name="what-does-each-of-hello-health-status-mean"></a>Vad innebär varje hello hälsostatus?
Det finns tre olika hälsotillstånd statusar:
- Tillgänglig: Det finns inga kända problem i hello Azure-plattformen som kan påverka den här resursen
- Inte tillgänglig: Resource health har upptäckt problem som påverkar hello resurs
- Okänd: Resource health kan inte fastställa hello hälsotillståndet för en resurs eftersom den har stoppats får information om den. 

## <a name="what-does-hello-unknown-status-mean-is-something-wrong-with-my-resource"></a>Vad hello okänd status medelvärdet? Något är fel med min resurs?
hello hälsostatus anges toounknown när Resource Health stoppar tar emot information om en viss resurs. Denna status är inte en slutgiltig indikation på hello tillståndet för hello resurs i fall där du får problem, kan den indikera det har uppstått ett problem med Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hur kan jag få hjälp för en resurs som inte är tillgänglig?
Du kan skicka en begäran från hello Resource Health-bladet. Du behöver inte ett supportavtal med Microsoft tooopen en begäran när hello resursen är inte tillgänglig eftersom plattformen händelser.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Resource Health skilja mellan inte finns alltid i av plattform problem jämfört med något som jag gjorde?
Ja, identifierar Resource Health hello grundorsaken inom någon av dessa kategorier när en resurs inte är tillgänglig: 
-   Användarinitierad åtgärd
-   Planerad åtgärd 
-   Oplanerad händelse

I hello portal visas användarinitierad åtgärder med en blå meddelandeikonen medan planerade och oplanerade händelser visas med en röd varningsikon. Mer information finns i hello [Resource Health översikt](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kan jag integrera Resource Health med min övervakningsverktyg?
Resurshälsotillståndet är en syftar toohelp du diagnostisera och åtgärda problem med Azure-tjänsten som påverkar dina resurser. Du kan använda hello Resource Health API tooprogrammatically hämta hello hälsostatus rekommenderar vi du använder mått toomonitor dina resurser. När ett problem upptäcks Resource Health hjälper dig att avgöra orsaken hello och hjälper dig att åtgärder tooaddress dem. Besök [Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) toolearn mer om hur du kan använda mått toocheck dina resurser.

## <a name="where-do-i-find-resource-health"></a>Var hittar Resource Health?
När du loggar in toohello Azure-portalen, finns det flera sätt som du kan komma åt Resource Health:
- Navigera tooyour resurs. I hello vänstra navigeringsfönstret, Välj **resurshälsa**
- Gå toohello Azure-Monitor-bladet.  I hello vänstra navigeringsfönstret, Välj **Resource health**.
- Öppna hello **hjälp + stöd** bladet genom att välja hello frågetecken i hello övre högra hörnet av hello-portalen och sedan välja **hjälp + stöd**. Då öppnas hello blad, Välj **resurshälsa**

Du kan också använda hello Resource Health API tooobtain information om hello hälsotillstånd för dina resurser.

## <a name="is-resource-health-available-for-all-resource-types"></a>Är Resource Health tillgängligt för alla typer av resurser?
hello lista över hälsokontroller och resurstyper som stöds via Resource Health finns [här](resource-health-checks-resource-types.md).

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Vad gör jag om min resurs visas tillgängliga men jag tror att det inte är ”?
När du kontrollerar hello hälsotillståndet för en resurs direkt under hello hälsostatus du kan klicka på **Rapportera felaktig hälsostatus**. Innan du skickar hello rapporten har hello möjlighet att med ytterligare information om varför du anser hello aktuella status är felaktig.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Är Resource Health tillgängligt för alla Azure-regioner? 
Resurshälsa finns tillgänglig i alla regioner som Azure förutom hello följande områden:
- Virginia (USA-förvaltad region)
- Iowa (USA-förvaltad region)
- US DoD, östra
- US DoD, centrala
- Centrala Tyskland
- Nordöstra Tyskland
- Östra Kina
- Norra Kina

## <a name="how-is-resource-health-different-from-hello-service-health-dashboard-or-hello-azure-portal-service-notifications"></a>Hur skiljer sig Resource Health från hello Hälsoinstrumentpanelen eller hello Azure portal tjänstmeddelanden?
hello information som tillhandahålls av Resource Health är mer specifik än vad som tillhandahålls av hello Azure Hälsoinstrumentpanelen.

Medan [Azure Status](https://status.azure.com) och hello portal tjänstmeddelanden får du information om tjänsten problem som påverkar ett stort antal kunder (till exempel en Azure-region), Resource Health exponerar mer detaljerade händelser som endast är relevanta toohello viss resurs. Om en värd oväntat startar om aviseringar Resource Health kunder vars virtuella datorer körs på värden.

Det är viktigt toonotice som du har slutfört synligheten för händelser som påverkar dina resurser, Resource Health också hämtar händelser publiceras i tjänstmeddelanden tooprovide och hello Hälsoinstrumentpanelen.

## <a name="do-i-need-tooactivate-resource-health-for-each-resource"></a>Behöver jag tooactivate Resource Health för varje resurs?
Nej, hälsoinformation är tillgängliga för alla resurstyper som är tillgängliga via Resource Health. 

## <a name="do-we-need-tooenable-resource-health-for-my-organization"></a>Behöver vi tooenable Resource Health för min organisation?
Nej.  Azure Resource Health är tillgänglig hello Azure-portalen utan att alla krav för nätverksinstallation.

## <a name="is-resource-health-available-free-of-charge"></a>Finns Resource Health kostnadsfritt?
Ja.  Azure Resource Health är kostnadsfritt.

## <a name="what-are-hello-recommendations-that-resource-health-provides"></a>Vad är hello rekommendationer som Resource Health tillhandahåller?
Baserat på hello hälsostatus ger Resource Health dig rekommendationer med hello målet för att minska hello som du har använt för felsökning. Tillgängliga resurser i hello rekommendationer fokuserar på hur toosolve hello de vanligaste problem uppstår. Om hello resursen är inte tillgänglig på grund av tooan Azure oplanerade händelse hello fokus är på hjälper dig under och efter hello återställningsprocessen. 

## <a name="next-steps"></a>Nästa steg

Läs mer om Resource Health:
-  [Översikt över Azure Resource Health](Resource-health-overview.md)
-  [Resurstyper och hälsokontroller är tillgängliga genom Azure Resource Health](resource-health-checks-resource-types.md)
