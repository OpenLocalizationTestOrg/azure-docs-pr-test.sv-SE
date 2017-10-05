---
title: "Azure Resource health vanliga frågor och svar | Microsoft Docs"
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
ms.openlocfilehash: 41522a85cac05304b3ae60c45b48920eefbe8f5c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-health-faq"></a>Azure Resource health vanliga frågor och svar
Lär dig svar på vanliga frågor om Azure-resurs hälsotillstånd.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
* [Vad är Azure Resource Health?](#what-is-azure-resource-health)
* [Vad är resource health avsedd för?](#what-is-the-resource-health-intended-for)
* [Vilka kontroll utförs av resource health?](#what-health-checks-are-performed-by-resource-health)
* [Vad innebär varje hälsostatus?](#what-does-each-of-the-health-status-mean)
* [Vad innebär okänd status? Något är fel med min resurs?](#what-does-the-unknown-status-mean-is-something-wrong-with-my-resource)
* [Hur kan jag få hjälp för en resurs som inte är tillgänglig?](#how-can-i-get-help-for-a-resource-that-is-unavailable)
* [Resurshälsa skilja mellan inte finns alltid i av plattform problem jämfört med något som jag gjorde?](#does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did)
* [Kan jag integrera resurshälsa med min övervakningsverktyg?](#can-i-integrate-resource-health-with-my-monitoring-tools)
* [Var hittar jag resurshälsa](#where-do-i-find-resource-health)
* [Är resurshälsa tillgängligt för alla typer av resurser?](#is-resource-health-available-for-all-resource-types)
* [Vad gör jag om min resurs visas tillgängliga men jag tror att det inte är?](#what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not)
* [Är resurshälsa tillgängligt för alla Azure-regioner?](#is-resource-health-available-for-all-azure-regions)
* [Hur skiljer sig resurshälsa från Hälsoinstrumentpanelen eller Azure portal tjänstmeddelanden?](#how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications)
* [Behöver jag aktivera resurshälsa för varje resurs?](#do-i-need-to-activate-resource-health-for-each-resource)
* [Vi behöver du aktivera resurshälsa för min organisation?](#do-we-need-to-enable-resource-health-for-my-organization)
* [Finns resurshälsa kostnadsfritt?](#is-resource-health-available-free-of-charge)
* [Vilka är de rekommendationer som resource health tillhandahåller?](#what-are-the-recommendations-that-resource-health-provides)


## <a name="what-is-azure-resource-health"></a>Vad är Azure resource health?
Resource Health hjälper dig att diagnostisera och få support när ett problem med Azure påverkar dina resurser. Det informerar dig om det aktuella och tidigare hälsotillståndet för dina resurser och hjälper dig att åtgärda problem. Resource Health ger teknisk support när du behöver hjälp med problem med Azure-tjänster.  

## <a name="what-is-the-resource-health-intended-for"></a>Vad är resource health avsedd för?
När ett problem med en resurs har identifierats hjälper resource health dig att diagnostisera orsaken. Det ger hjälpa till att minska problem och teknisk support om du behöver mer hjälp med problem med Azure-tjänsten.

## <a name="what-health-checks-are-performed-by-resource-health"></a>Vilka kontroll utförs av resource health?
Resurshälsa kontrollerar olika baserat på de [resurstypen](resource-health-checks-resource-types.md). Dessa kontroller är utformade för att implementera tre typer av problem: 
1. Oplanerad händelser, till exempel en oväntade värden omstart
2. Planerad händelser, t.ex. uppdateringar schemalagda värdens operativsystem
3. Händelser som utlösts av användaråtgärder, till exempel en användare startar om en virtuell dator

## <a name="what-does-each-of-the-health-status-mean"></a>Vad innebär varje hälsostatus?
Det finns tre olika hälsotillstånd statusar:
1. Tillgänglig: Det finns inga kända problem i Azure-plattformen som kan påverka den här resursen
2. Inte tillgänglig: Resource health har upptäckt problem som påverkar resursen
3. Okänd: Resource health kan inte fastställa hälsotillståndet för en resurs eftersom den har stoppats får information om den. 

## <a name="what-does-the-unknown-status-mean-is-something-wrong-with-my-resource"></a>Vad innebär okänd status? Något är fel med min resurs?
Hälsostatus anges till okänd när resurshälsa stoppar tar emot information om en viss resurs. Denna status är inte en slutgiltig indikation på tillståndet för resursen i fall där du får problem, kan det indikera det har uppstått ett problem med Azure.

## <a name="how-can-i-get-help-for-a-resource-that-is-unavailable"></a>Hur kan jag få hjälp för en resurs som inte är tillgänglig?
Du kan skicka en begäran från resursbladet för hälsotillstånd. Du behöver inte ett supportavtal med Microsoft för att öppna en begäran när resursen är inte tillgänglig eftersom plattformen händelser.

## <a name="does-resource-health-differentiate-between-unavailability-cased-by-platform-problems-versus-something-i-did"></a>Resurshälsa skilja mellan inte finns alltid i av plattform problem jämfört med något som jag gjorde?
När en resurs är tillgänglig, identifierar resurshälsa Ja, den grundläggande orsaken i en av dessa kategorier: 
1.  Användarinitierad åtgärd
2.  Planerad åtgärd 
3.  Oplanerad händelse

I portalen visas användarinitierad åtgärder med en blå meddelandeikonen medan planerade och oplanerade händelser visas med en röd varningsikon. Mer information finns i den [hälsa Resursöversikt](Resource-health-overview.md).  

## <a name="can-i-integrate-resource-health-with-my-monitoring-tools"></a>Kan jag integrera resurshälsa med min övervakningsverktyg?
Resurshälsotillståndet är en tjänst som hjälper dig att diagnostisera och åtgärda problem med Azure-tjänsten som påverkar dina resurser. Medan du kan använda resurshälsa API för att genom programmering få hälsostatus, rekommenderar vi att du använder mått för att övervaka dina resurser. När ett problem upptäcks resource health hjälper dig att avgöra den bakomliggande orsaken och hjälper dig att åtgärder för att behandla dem. Besök [Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) lära dig mer om hur du kan använda mått för att kontrollera dina resurser.

## <a name="where-do-i-find-resource-health"></a>Var hittar jag resurshälsa
När du loggar in på Azure-portalen, finns det flera olika sätt som du kan komma åt resurshälsa:
1. Gå till din resurs. I det vänstra navigeringsfönstret klickar du på **Resource health**.
2. Gå till Azure-Monitor-bladet.  I det vänstra navigeringsfönstret klickar du på **Resource health**.
3. Öppna den **hjälp + stöd** bladet genom att klicka på frågetecken i det övre högra hörnet av portalen och sedan välja **hjälp + stöd**. När det öppnas bladet, klickar du på **resurshälsa**

Du kan också använda resurshälsa API för att hämta information om hälsotillståndet för dina resurser.

## <a name="is-resource-health-available-for-all-resource-types"></a>Är resurshälsa tillgängligt för alla typer av resurser?
Listan över hälsokontroller och resurstyper som stöds via resurshälsa hittar [här](resource-health-checks-resource-types.md)

## <a name="what-should-i-do-if-my-resource-is-showing-available-but-i-believe-it-is-not"></a>Vad gör jag om min resurs visas tillgängliga men jag tror att det inte är ”?
Kontrollerar hälsotillståndet för en resurs direkt under hälsostatus du klicka på **Rapportera felaktig hälsostatus**. Innan du skickar rapporten har du möjlighet att ange ytterligare information om varför du tror att det aktuella hälsotillståndet är felaktig.

## <a name="is-resource-health-available-for-all-azure-regions"></a>Är resurshälsa tillgängligt för alla Azure-regioner? 
Resurshälsa finns tillgänglig i alla regioner som Azure förutom följande områden:
* Virginia (USA-förvaltad region)
* Iowa (USA-förvaltad region)
* US DoD, östra
* US DoD, centrala
* Centrala Tyskland
* Nordöstra Tyskland
* Östra Kina
* Norra Kina

## <a name="how-is-resource-health-different-from-the-service-health-dashboard-or-the-azure-portal-service-notifications"></a>Hur skiljer sig resurshälsa från Hälsoinstrumentpanelen eller Azure portal tjänstmeddelanden?
Information som tillhandahålls av resurshälsa är mer specifik än vad som tillhandahålls av Azure Hälsoinstrumentpanelen.

Medan [Azure Status](https://status.azure.com) och företagsportalen service-meddelanden får du information om tjänsten problem som påverkar ett stort antal kunder (till exempel en Azure-region), resurshälsa exponerar mer detaljerade händelser som är relevanta för endast den viss resurs. Om en värd oväntat startar om aviseringar resurshälsa kunder vars virtuella datorer körs på värden.

Det är viktigt att Observera att för att ge dig en fullständig överblick över händelser som påverkar dina resurser, resource health tillhandahåller även händelser som har publicerats i tjänstmeddelanden och Hälsoinstrumentpanelen.

## <a name="do-i-need-to-activate-resource-health-for-each-resource"></a>Behöver jag aktivera resurshälsa för varje resurs?
Nej, hälsoinformation är tillgängliga för alla resurstyper som är tillgängliga via resurshälsa. 

## <a name="do-we-need-to-enable-resource-health-for-my-organization"></a>Vi behöver du aktivera resurshälsa för min organisation?
Nej.  Azure-resurs hälsotillstånd är tillgänglig i Azure-portalen utan att alla krav för nätverksinstallation.

## <a name="is-resource-health-available-free-of-charge"></a>Finns resurshälsa kostnadsfritt?
Ja.  Hälsotillståndet för Azure-resurs är kostnadsfritt.

## <a name="what-are-the-recommendations-that-resource-health-provides"></a>Vilka är de rekommendationer som resource health tillhandahåller?
Baserat på hälsostatus ger resurshälsa dig rekommendationer med målet att minska tid som du har använt för felsökning. Fokus rekommendationer om hur du löser de flesta vanliga problem kunderna får för tillgängliga resurser. Om resursen inte är tillgänglig på grund av en Azure oplanerade händelse kommer fokus att bistå du under och efter återställningen. 

## <a name="next-steps"></a>Nästa steg

Checka ut dessa resurser om du vill veta mer om resurshälsa:
-  [Översikt över Azure-resurs-hälsa](Resource-health-overview.md)
-  [Resurstyper och hälsokontroller finns tillgängliga genom Azure Resource Health](resource-health-checks-resource-types.md)
