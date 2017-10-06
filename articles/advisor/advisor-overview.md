---
title: aaaIntroduction tooAzure Advisor | Microsoft Docs
description: "Använd Azure Advisor toooptimize Azure-distributioner."
services: advisor
documentationcenter: NA
author: kumudd
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
ms.openlocfilehash: 5d796fc06366221efdb6f1bda39ab3fb676abfd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-advisor"></a>Introduktion tooAzure Advisor

Lär dig mer om Azure Advisor och de viktigaste funktionerna och få svar toofrequently frågor och svar.

## <a name="what-is-advisor"></a>Vad är Advisor?
Advisor är personliga molnet konsult som hjälper dig att följa bästa praxis toooptimize Azure-distributioner. Den analyserar dina resurskonfigurationen och telemetri och rekommenderar lösningar som hjälper dig att förbättra hello kostnadseffektivitet, prestanda, hög tillgänglighet och säkerhet för dina Azure-resurser.

Med Advisor kan du:
* Hämta proaktiv, tillförlitlig och anpassade rekommendationer och metodtips. 
* Förbättra hello prestanda, säkerhet och hög tillgänglighet för dina resurser, när du identifierar affärsmöjligheter tooreduce din övergripande Azure tillbringar.
* Få rekommendationer med infogad föreslagna åtgärder.

Du kan komma åt Advisor via hello [Azure-portalen](https://aka.ms/azureadvisordashboard). Logga in toohello [portal](https://portal.azure.com)väljer **Bläddra**, och bläddrar sedan för**Azure Advisor**. hello Advisor instrumentpanelen visar anpassade rekommendationer för den valda prenumerationen. 

hello rekommendationer delas in i fyra kategorier: 

* **Hög tillgänglighet**: tooensure och förbättra hello kontinuiteten i dina verksamhetskritiska program. Mer information finns i [hög tillgänglighet för Advisor-rekommendationer](advisor-high-availability-recommendations.md).

* **Säkerhet**: toodetect hot och säkerhetsrisker som kan leda till toosecurity överträdelser. Mer information finns i [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md).

* **Prestanda**: tooimprove hello hastigheten för dina program. Mer information finns i [Advisor-rekommendationer](advisor-performance-recommendations.md).

* **Kostnad**: toooptimize och minska din övergripande Azure tillbringar. Mer information finns i [kostnaden för Advisor-rekommendationer](advisor-cost-recommendations.md).

  ![Typer för Advisor-rekommendationer](./media/advisor-overview/advisor-all-tab-examples.png)

> [!NOTE]
> tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor. En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen. Det här är en *engångsåtgärd*. När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.

Du kan klicka på en rekommendation toolearn mer om den. Du kan också information om åtgärder som du kan utföra tootake nytta av en möjlighet eller lösa ett problem. 

Advisor-rekommendationer med infogad åtgärder eller dokumentationen länkar. Klicka på en infogad åtgärd tar dig igenom en ”interaktiv användare resa” tooimplement den. Klicka på en länk i dokumentationen pekar du toodocumentation som beskriver hur toomanually implementerar hello-åtgärd. 

Advisor uppdaterar rekommendationer varje timme. Om du inte planerar tootake omedelbara åtgärder på en rekommendation, kan viloläge för en angiven tidsperiod eller stänga den. 

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="how-do-i-access-advisor"></a>Hur kommer jag åt Advisor?
Du kan komma åt Advisor via hello [Azure-portalen](https://aka.ms/azureadvisordashboard). Logga in toohello [portal](https://portal.azure.com)väljer **Bläddra**, och bläddrar sedan för**Azure Advisor**. hello Advisor instrumentpanelen visar anpassade rekommendationer för den valda prenumerationen. 

Du kan också visa Advisor-rekommendationer via hello resursbladet för virtuell dator. Välj en virtuell dator och bläddrar sedan tooAdvisor rekommendationer hello-menyn. 

### <a name="what-permissions-do-i-need-tooaccess-advisor"></a>Behörigheter gör jag behöver tooaccess Advisor?

tooaccess Advisor-rekommendationer, måste du först *registrera prenumerationen* med Advisor. En prenumeration registreras när en *prenumeration ägare* startar hello Advisor instrumentpanelen och klickar på hello **få rekommendationer** knappen. Det här är en *engångsåtgärd*. När hello prenumeration har registrerats kan du komma åt Advisor-rekommendationer som *ägare*, *deltagare*, eller *Reader* för en prenumeration, resursgrupp eller en viss resurs.

### <a name="how-often-are-advisor-recommendations-updated"></a>Hur ofta uppdateras Advisor-rekommendationer uppdateras?

Advisor-rekommendationer uppdateras varje timme.

### <a name="what-resources-does-advisor-provide-recommendations-for"></a>Vilka resurser Advisor ger rekommendationer för?

Advisor innehåller rekommendationer för virtuella datorer, tillgänglighetsuppsättningar, programgatewayer, Apptjänster, SQL-servrar, SQL-databaser och Redis-Cache.

### <a name="can-i-snooze-or-dismiss-a-recommendation"></a>Kan jag viloläge eller ignorera en rekommendation?

toosnooze eller ignorera en rekommendation klickar du på hello **viloläge** knappen eller länken. Du kan ange en tid för viloläge period eller välja **aldrig** toodismiss hello rekommendation.

## <a name="next-steps"></a>Nästa steg

toolearn mer om Advisor-rekommendationer finns:

* [Kom igång med Advisor](advisor-get-started.md)
* [Advisor-rekommendationer för hög tillgänglighet](advisor-high-availability-recommendations.md)
* [Advisor säkerhetsrekommendationer](advisor-security-recommendations.md)
* [Advisor-rekommendationer](advisor-performance-recommendations.md)
* [Kostnad Advisor-rekommendationer](advisor-cost-recommendations.md)
