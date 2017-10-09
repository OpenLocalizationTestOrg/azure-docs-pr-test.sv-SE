---
title: "aaaAzure Resource health vanliga frågor och svar | Microsoft Docs"
description: "Översikt över Azure Resource health"
services: Resource health
documentationcenter: dev-center-name
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 06/01/2016
ms.author: BernardoAMunoz
ms.openlocfilehash: 5c5cfa116340094ffb1d6d5b206a11d389a0305a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-faq"></a>Azure Resource health vanliga frågor och svar
Lär dig hello svar toocommon frågor om Azure-resurshälsa.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
* [Vad är Azure Resource Health?](#what-is-azure-resource-health)
* [Vad är hello resurshälsa avsedd för?](#what-is-the-resource-health-intended-for)
* [Vilka kontroll utförs av resource health?](#what-health-checks-are-performed-by-resource-health)
* [Vad innebär varje hello hälsostatus?](#what-does-each-of-the-health-status-mean)
* [Vad hello okänd status medelvärdet? Något är fel med min resurs?](#what-does-the-unknown-status-mean-is-something-wrong-with-my-resource)
* [Hur kan jag få hjälp för en resurs som inte är tillgänglig?](#how-can-i-get-help-for-a-resource-that-is-unavailable)
* [Resurshälsa skilja mellan inte finns alltid i av plattform problem jämfört med något som jag gjorde?](#does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did)
* [Kan jag integrera resurshälsa med min övervakningsverktyg?](#can-i-integrate-resource-health-with-my-monitoring-tools)
* [Var hittar jag resurshälsa](#where-do-i-find-resource-health)
* [Är resurshälsa tillgängligt för alla typer av resurser?](#is-resource-health-available-for-all-resource-types)
* [Vad gör jag om min resurs visas tillgängliga men jag tror att det inte är?](#what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not)
* [Är resurshälsa tillgängligt för alla Azure-regioner?](#is-resource-health-available-for-all-azure-regions)
* [Hur skiljer sig resurshälsa från hello Hälsoinstrumentpanelen eller hello Azure portal tjänstmeddelanden?](#how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications)
* [Behöver jag tooactivate resurshälsa för varje resurs?](#do-i-need-to-activate-resource-health-for-each-resource)
* [Behöver vi tooenable resurshälsa för min organisation?](#do-we-need-to-enable-resource-health-for-my-organization)
* [Finns resurshälsa kostnadsfritt?](#is-resource-health-available-free-of-charge)
* [Vad är hello rekommendationer som resource health tillhandahåller?](#what-are-the-recommendations-that-resource-health-provides)


## <a name="what-is-azure-resource-health"></a>Vad är Azure resource health?
Resource Health hjälper dig att diagnostisera och få support när ett problem med Azure påverkar dina resurser. Det informerar om hello aktuella och tidigare dina resursers hälsa och hjälper dig att minska problem. Resource Health ger teknisk support när du behöver hjälp med problem med Azure-tjänster.  

## <a name="what-is-hello-resource-health-intended-for"></a>Vad är hello resurshälsa avsedd för?
När ett problem med en resurs har identifierats kan resurshälsa hjälpa dig att diagnostisera hello orsaken. Den innehåller hjälp toomitigate hello problemet och teknisk support om du behöver mer hjälp med problem med Azure-tjänsten.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Vilka kontroll utförs av resource health?
Resurshälsa kontrollerar olika baserat på hello [resurstypen](resource-health-checks-resource-types.md). Dessa kontroller är utformad tooimplement tre typer av problem: 
1. Oplanerad händelser, till exempel en oväntade värden omstart
2. Planerad händelser, t.ex. uppdateringar schemalagda värdens operativsystem
3. Händelser som utlösts av användaråtgärder, till exempel en användare startar om en virtuell dator

## <a name="what-does-each-of-hello-health-status-mean"></a>Vad innebär varje hello hälsostatus?
Det finns tre olika hälsotillstånd statusar:
1. Tillgänglig: Det finns inga kända problem i hello Azure-plattformen som kan påverka den här resursen
2. Inte tillgänglig: Resource health har upptäckt problem som påverkar hello resurs
3. Okänd: Resource health kan inte fastställa hello hälsotillståndet för en resurs eftersom den har stoppats får information om den. 

## <a name="what-does-hello-unknown-status-mean-is-something-wrong-with-my-resource"></a>Vad hello okänd status medelvärdet? Något är fel med min resurs?
hello hälsostatus anges toounknown när resurshälsa slutar ta emot information om en viss resurs. Denna status är inte en slutgiltig indikation på hello tillståndet för hello resurs i fall där du får problem, kan den indikera det har uppstått ett problem med Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hur kan jag få hjälp för en resurs som inte är tillgänglig?
Du kan skicka en begäran från hello resursbladet hälsa. Du behöver inte ett supportavtal med Microsoft tooopen en begäran när hello resursen är inte tillgänglig eftersom plattformen händelser.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Resurshälsa skilja mellan inte finns alltid i av plattform problem jämfört med något som jag gjorde?
Ja, identifierar resurshälsa hello grundorsaken inom någon av dessa kategorier när en resurs inte är tillgänglig: 
1.  Användarinitierad åtgärd
2.  Planerad åtgärd 
3.  Oplanerad händelse

I hello portal visas användarinitierad åtgärder med en blå meddelandeikonen medan planerade och oplanerade händelser visas med en röd varningsikon. Mer information finns i hello [hälsa Resursöversikt](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kan jag integrera resurshälsa med min övervakningsverktyg?
Resurshälsotillståndet är en syftar toohelp du diagnostisera och åtgärda problem med Azure-tjänsten som påverkar dina resurser. Du kan använda hello resource health API tooprogrammatically hämta hello hälsostatus rekommenderar vi du använder mått toomonitor dina resurser. När ett problem upptäcks resource health hjälper dig att avgöra orsaken hello och hjälper dig att åtgärder tooaddress dem. Besök [Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) toolearn mer om hur du kan använda mått toocheck dina resurser.

## <a name="where-do-i-find-resource-health"></a>Var hittar jag resurshälsa
När du loggar in toohello Azure-portalen, finns det flera olika sätt som du kan komma åt resursen hälsotillstånd:
1. Navigera tooyour resurs. I hello vänstra navigeringsfönstret klickar du på **Resource health**.
2. Gå toohello Azure-Monitor-bladet.  I hello vänstra navigeringsfönstret klickar du på **Resource health**.
3. Öppna hello **hjälp + stöd** bladet genom att klicka på hello frågetecken i hello övre högra hörnet av hello-portalen och sedan välja **hjälp + stöd**. När hello blad öppnas, klickar du på **resurshälsa**

Du kan också använda hello resurshälsa API tooobtain information om hello hälsotillstånd för dina resurser.

## <a name="is-resource-health-available-for-all-resource-types"></a>Är resurshälsa tillgängligt för alla typer av resurser?
hello lista över hälsokontroller och resurstyper som stöds via resurshälsa hittar [här](resource-health-checks-resource-types.md)

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Vad gör jag om min resurs visas tillgängliga men jag tror att det inte är ”?
När du kontrollerar hello hälsotillståndet för en resurs direkt under hello hälsostatus du kan klicka på **Rapportera felaktig hälsostatus**. Innan du skickar hello rapporten har hello möjlighet att med ytterligare information om varför du anser hello aktuella status är felaktig.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Är resurshälsa tillgängligt för alla Azure-regioner? 
Resurshälsa finns tillgänglig i alla regioner som Azure förutom hello följande områden:
* Virginia (USA-förvaltad region)
* Iowa (USA-förvaltad region)
* US DoD, östra
* US DoD, centrala
* Centrala Tyskland
* Nordöstra Tyskland
* Östra Kina
* Norra Kina

## <a name="how-is-resource-health-different-from-hello-service-health-dashboard-or-hello-azure-portal-service-notifications"></a>Hur skiljer sig resurshälsa från hello Hälsoinstrumentpanelen eller hello Azure portal tjänstmeddelanden?
hello uppgifter som resurshälsa är mer specifik än vad som tillhandahålls av hello Azure Hälsoinstrumentpanelen.

Medan [Azure Status](https://status.azure.com) och hello portal tjänstmeddelanden får du information om tjänsten problem som påverkar ett stort antal kunder (till exempel en Azure-region), resurshälsa exponerar mer detaljerade händelser som endast är relevanta toohello viss resurs. Om en värd oväntat startar om aviseringar resurshälsa kunder vars virtuella datorer körs på värden.

Det är viktigt toonotice som tooprovide som du har slutfört synligheten för händelser som påverkar dina resurser resurshälsa hämtar också händelser som har publicerats i tjänstmeddelanden och hello Hälsoinstrumentpanelen.

## <a name="do-i-need-tooactivate-resource-health-for-each-resource"></a>Behöver jag tooactivate resurshälsa för varje resurs?
Nej, hälsoinformation är tillgängliga för alla resurstyper som är tillgängliga via resurshälsa. 

## <a name="do-we-need-tooenable-resource-health-for-my-organization"></a>Behöver vi tooenable resurshälsa för min organisation?
Nej.  Azure-resurs hälsotillstånd är tillgänglig hello Azure-portalen utan att alla krav för nätverksinstallation.

## <a name="is-resource-health-available-free-of-charge"></a>Finns resurshälsa kostnadsfritt?
Ja.  Hälsotillståndet för Azure-resurs är kostnadsfritt.

## <a name="what-are-hello-recommendations-that-resource-health-provides"></a>Vad är hello rekommendationer som resource health tillhandahåller?
Baserat på hello hälsostatus ger resurshälsa dig rekommendationer med hello målet för att minska hello som du har använt för felsökning. Tillgängliga resurser i hello rekommendationer fokuserar på hur toosolve hello de vanligaste problem uppstår. Om hello resursen är inte tillgänglig på grund av tooan Azure oplanerade händelse hello fokus är på hjälper dig under och efter hello återställningsprocessen. 

## <a name="next-steps"></a>Nästa steg

Checka ut dessa resurser toolearn mer om resurshälsa:
-  [Översikt över Azure-resurs-hälsa](Resource-health-overview.md)
-  [Resurstyper och hälsokontroller finns tillgängliga genom Azure Resource Health](resource-health-checks-resource-types.md)
