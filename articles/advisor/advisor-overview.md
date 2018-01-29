---
title: Introduktion till Azure Advisor | Microsoft Docs
description: "Använd Azure Advisor för att optimera din Azure-distributioner."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 906450f75557820bb27762707c3328b08b23cccb
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/08/2017
---
# <a name="introduction-to-azure-advisor"></a>Introduktion till Azure Advisor

Lär dig mer om de viktigaste funktionerna i Azure Advisor och få svar på vanliga frågor och svar.

## <a name="what-is-advisor"></a>Vad är Advisor?
Advisor är en anpassad cloud konsult som hjälper dig att följa de bästa metoderna för att optimera din Azure-distributioner. Den analyserar dina resurskonfigurationen och telemetri och rekommenderar lösningar som hjälper dig att förbättra kostnadseffektivitet, prestanda, hög tillgänglighet och säkerhet för dina Azure-resurser.

Med Advisor kan du:
* Hämta proaktiv, tillförlitlig och anpassade rekommendationer och metodtips. 
* Förbättra prestanda, säkerhet och hög tillgänglighet för dina resurser, som du identifiera möjligheter att minska den totala Azure tillbringar.
* Få rekommendationer med infogad föreslagna åtgärder.

Du kan komma åt Advisor i den [Azure-portalen](https://aka.ms/azureadvisordashboard). Logga in på den [portal](https://portal.azure.com), leta upp **Advisor** i navigeringsmenyn eller Sök efter den i den **fler tjänster** menyn.

Advisor-instrumentpanelen innehåller anpassad rekommendationer för alla prenumerationer.  Du kan använda filter för att visa rekommendationer för specifika prenumerationer och resurstyper.  Rekommendationerna delas in i fyra kategorier: 

* **Hög tillgänglighet**: att säkerställa och förbättra kontinuiteten i dina verksamhetskritiska program. Mer information finns i [hög tillgänglighet för Advisor-rekommendationer](advisor-high-availability-recommendations.md).
* **Säkerhet**: identifiera hot och säkerhetsrisker som kan leda till säkerhetsintrång. Mer information finns i [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md).
* **Prestanda**: att förbättra hastighet för dina program. Mer information finns i [Advisor-rekommendationer](advisor-performance-recommendations.md).
* **Kostnad**: för att optimera och minska den totala Azure utgifter. Mer information finns i [kostnaden för Advisor-rekommendationer](advisor-cost-recommendations.md).

  ![Typer för Advisor-rekommendationer](./media/advisor-overview/advisor-dashboard.png)

> [!NOTE]
> Du använder Azure Advisor med en prenumeration på en prenumeration *ägare* får starta Advisor-instrumentpanelen.  Den här åtgärden registrerar prenumerationen med Advisor.  Från den punkten på någon prenumeration *ägare*, *deltagare*, eller *Reader* kan komma åt Advisor-rekommendationer för prenumerationen. 

Du kan klicka på en kategori om du vill visa en lista över rekommendationerna i den kategorin Välj en rekommendation om du vill veta mer om den  Du kan också information om åtgärder som du kan utföra för att dra nytta av en affärsmöjlighet eller lösa ett problem.

![Advisor rekommendation kategori](./media/advisor-overview/advisor-ha-category-example.png) 

Välj den rekommenderade åtgärden för en rekommendation att implementera denna rekommendation.  Ett enkelt gränssnitt öppnas där du kan följa rekommendationen eller avser dokumentation som hjälper dig med implementering.  När du implementerar en rekommendation, kan det ta upp till en dag för Advisor att identifiera som.

Om du inte tänker vidta omedelbara åtgärder på en rekommendation du viloläge för en angiven tidsperiod eller stänga den.  Om du inte vill få rekommendationer för en viss prenumeration eller resursgrupp kan du konfigurera Advisor för att endast skapa rekommendationer för angivna prenumerationer och resursgrupper.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="how-do-i-access-advisor"></a>Hur kommer jag åt Advisor?
Du kan komma åt Advisor i den [Azure-portalen](https://aka.ms/azureadvisordashboard). Logga in på den [portal](https://portal.azure.com), leta upp **Advisor** i navigeringsmenyn eller Sök efter den i den **fler tjänster** menyn.

Du kan också visa Advisor-rekommendationer via gränssnittet virtuella resursen. Välj en virtuell dator och bläddrar sedan till Advisor-rekommendationer i menyn. 

### <a name="what-permissions-do-i-need-to-access-advisor"></a>Vilka behörigheter som behöver att komma åt Advisor?

Du måste först registrera prenumerationen med Advisor för att erhålla Advisor-rekommendationer för en prenumeration. En prenumeration registreras när en prenumeration *ägare* startar Advisor-instrumentpanelen. Det här är en gång. När prenumerationen har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Vilka resurser Advisor ger rekommendationer för?

Advisor innehåller rekommendationer för virtuella datorer, tillgänglighetsuppsättningar, programgatewayer, Apptjänster, SQL-servrar, SQL-databaser och Redis-Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Kan jag viloläge eller ignorera en rekommendation?

Viloläge eller ignorera en rekommendation genom att klicka på den **viloläge** länk. Du kan ange en tid för viloläge period eller välja **aldrig** stänga rekommendationen.

## <a name="next-steps"></a>Nästa steg

Mer information om Advisor-rekommendationer finns:

* [Kom igång med Advisor](advisor-get-started.md)
* [Advisor-rekommendationer för hög tillgänglighet](advisor-high-availability-recommendations.md)
* [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md)
* [Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Kostnad Advisor-rekommendationer](advisor-cost-recommendations.md)
