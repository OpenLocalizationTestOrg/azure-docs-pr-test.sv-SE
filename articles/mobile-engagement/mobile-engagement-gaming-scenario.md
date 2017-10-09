---
title: "aaaAzure Mobile Engagement-implementering för Spelappen"
description: Spel app scenariot tooimplement Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2cafc044-4902-4058-8037-49399bf6bf7f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b82b4a868a33f42e5b759e43e66103556c097f9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-mobile-engagement-with-gaming-app"></a>Implementerar Mobile Engagement med Spelappen
## <a name="overview"></a>Översikt
En spel Startup har startas en ny fiske baserat role play/strategi spel app. hello spelet har varit igång i sex månader. Spelet är mycket stora lyckas, och den har miljontals hämtningar och hello är mycket hög jämfört med tooother uppstart spel appar. Vid hello kvartalsvis granskningsmöte accepterar intressenter de behöver tooincrease genomsnittliga intäkter per användare (ARPU). Premium i spelet paket är tillgängliga som specialerbjudanden. Dessa spel Pack Tillåt användare tooupgrade hello utseende och prestanda på deras fiske rader och bete eller tackles i hello spel. Paketet försäljning är dock mycket låg. Så att de bestämma med ett webbanalysverktyg för till första tooanalyze hello customer experience och sedan toodevelop en engagement program tooincrease försäljning med avancerade segmentering.

Baserat på hello [Azure Mobile Engagement – komma igång med bästa praxis](mobile-engagement-getting-started-best-practices.md) de skapar en strategi.

## <a name="objectives-and-kpis"></a>Mål och KPI: er
Viktiga intressenter för hello spel uppfylla. Komma överens om en Huvudsyftet - tooincrease premium paketet försäljningar efter 15%. De skapar toomeasure (Business Key Performance Indicator) och enheten detta mål

* På vilken nivå av hello spelet köps paketen?
* Vad är hello intäkter per användare, sessioner, per vecka och per månad?
* Vad är hello favorit köp typer?

Del 1 av hello [komma igång](mobile-engagement-getting-started-best-practices.md) förklarar hur toodefine hello mål och KPI: er. 

Med hello nu definierat KPI: er, skapar hello Mobile produkten Manager Engagement KPI: er toodetermine nya användare trender och lagring.

* Övervaka kvarhållning och använda på hello följande intervall: varje dag, varje 2 dagar, varje vecka, månad och var tredje månad
* Aktiva användare
* Hej appklassificering i hello lagra

Baserat på rekommendationer från hello IT-teamet hello följande tekniska KPI: er har lagts till tooanswer hello följande frågor:

* Vad är Mina användare sökväg (vilken sida besökta, hur mycket tid användare spendera på den)
* Antal krascher eller buggar påträffade per session
* Vilka operativsystemversioner körs Mina användare?
* Vad är hello genomsnittlig storlek på skärmen för Mina användare?
* Vilken typ av internet-anslutning har Mina användare?

För varje KPI anger hello Mobile produkten Manager hello data hon och var den finns i sitt playbook.

## <a name="engagement-program-and-integration"></a>Program för användarinteraktion och integrering
Innan du skapar en avancerad engagement-programmet ska hello Mobile projektet Director ansvarig hello projektet ha en djupgående förståelse för hur och när produkter förbrukas av hello användare.

Efter tre månader hello Mobile projektet Director har samlat in tillräckligt med data tooenhance sin app push notification försäljning. Han Lär som:

* hello första inköpet sker vanligtvis på hello nivå 14. Hello inköp är 90% av de fall nya legendariska vapen för $3.
* Användare som har gjort ett inköp fortsätta med hello produkt och göra mer inköp i 80% av de fallen.
* Användare som har klarat hello nivå 20, starta toospend mer än 10 $ i veckan.
* Användare tenderar toobuy premium-paket på nivå 16, 24 och 32.

Tack toothis analys hello Mobile projektet Director beslutar toocreate vissa push notification-sekvenser tooincrease i appen försäljning. Han skapar tre push-sekvenser som han kallar: Välkommen program, försäljning Program och inaktiva Program. Mer information finns i toohello [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
