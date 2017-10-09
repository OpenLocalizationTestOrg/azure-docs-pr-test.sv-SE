---
title: aaaMonitoring i Microsoft Azure | Microsoft Docs
description: "Alternativ när du vill toomonitor allt annat i Microsoft Azure. Azure-Monitor, Application Insights logganalys"
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: f5a4f32525c52613f01a913e09a9fe3fbcbaeb21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-monitoring-in-microsoft-azure"></a>Översikt över övervakning i Microsoft Azure
Den här artikeln innehåller en översikt över verktyg som är tillgängligt för övervakning av Microsoft Azure. Den gäller för
- övervakning av program som körs i Microsoft Azure 
- Verktyg/tjänster som körs utanför Azure som kan övervaka objekt i Azure. 

Den behandlar hello olika produkter och tjänster som är tillgängliga och hur de fungerar tillsammans. Det kan hjälpa dig att toodetermine vilka verktyg är mest lämpliga för du i vilket fall.  

## <a name="why-use-monitoring-and-diagnostics"></a>Varför använda övervaknings- och diagnostikfunktionerna?

Prestandaproblem i din molnapp kan påverka din verksamhet. Med flera sammankopplade komponenter och ofta versioner kan degradations inträffa när som helst. Och om du utvecklar en app användarna identifiera vanligtvis problem som du inte kan hitta vid testning. Du bör känna till de här problemen omedelbart och har verktyg för att diagnostisera och åtgärda problem med hello. Microsoft Azure har en uppsättning verktyg för att identifiera problemen.

## <a name="how-do-i-monitor-my-azure-cloud-apps"></a>Hur övervakar jag min Azure-molnappar?

Det finns ett antal verktyg för övervakning av Azure-program och tjänster. Några av sina funktioner överlappar varandra. Detta är delvis historiska skäl och delvis på grund av toohello oskärpa mellan utveckling och drift av ett program. 

Här följer hello huvudsakliga verktyg:

-   **Azure-Monitor** är grundläggande verktyg för att övervaka tjänster som körs på Azure. Den ger dig infrastrukturnivå data om hello genomflödet av en tjänst och hello omgivande miljö. Om du hanterar dina appar i Azure, bestämmer om tooscale uppåt eller nedåt resurser sedan Azure-Monitor ger dig vad du använder toostart.

-   **Application Insights** kan användas för utveckling och som en produktion övervakningslösning. Det fungerar genom att installera ett paket i din app och så får du en mer interna vy över vad som händer. Data innehåller svarstider för beroenden, undantag spårning, felsökning ögonblicksbilder, körning av profiler. Den innehåller kraftfulla smarta verktyg för analys all telemetri båda toohelp du felsöka en app och toohelp du förstår vad användarna gör med den. Du kan se om en topp i antal svarstider förfaller toosomething i en app eller vissa externa resourcing problemet. Om du använder Visual Studio och hello appen är fel, kan du tas rätt toohello problemet raderna i koden så att du kan åtgärda den.  

-   **Logga Analytics** för den som behöver tootune prestanda och planera Underhåll på program som körs i produktion. Den är baserad i Azure. Den samlar in och samlar in data från flera källor, men med en fördröjning på 10 minuter för too15. Det ger en heltäckande lösning för IT för Azure, lokalt och från tredje part molnbaserad infrastruktur (till exempel Amazon Web Services). Det ger bättre verktyg tooanalyze data över flera källor, tillåter komplexa frågor över alla loggar och proaktivt kan meddela på angivna villkor.  Du kan även samla in anpassade data till den centrala databasen så kan fråga och visualisera den. 

-   **System Center Operations Manager (SCOM)** är för att hantera och övervaka stora molnet installationer. Du kanske redan känner till den som ett hanteringsverktyg för lokal Windows Sever och baserat Hyper-V-moln, men den kan även integreras med och hantera Azure-appar. Bland annat installera den Application Insights på befintliga live appar.  Om en app kraschar, om det i sekunder. Observera att logganalys inte ersätter SCOM. Det fungerar bra tillsammans med den.  


## <a name="accessing-monitoring-in-hello-azure-portal"></a>Åtkomst till övervakning i hello Azure-portalen
Alla övervakningstjänster för Azure finns nu i en enda UI-fönstret. Mer information om hur tooaccess det här området finns [Kom igång med Azure-Monitor](monitoring-get-started.md). 

Du kan också komma åt övervakning funktioner för specifika resurser genom att markera resurserna och gå nedåt i sina övervakningsalternativ. 

## <a name="examples-of-when-toouse-which-tool"></a>Exempel på när toouse som verktyget 

hello följande avsnitt visar några grundläggande scenarier och vilka verktyg som ska användas tillsammans. 

### <a name="scenario-1--fix-errors-in-an-azure-application-under-development"></a>Scenario 1 – reparera fel i ett Azure-program under utveckling   

**hello bästa alternativet är toouse Application Insights, Azure-Monitor och Visual Studio tillsammans**

Azure tillhandahåller nu hello alla fördelar med hello Visual Studio-felsökaren i hello molnet. Konfigurera Azure-Monitor toosend telemetri tooApplication insikter. Aktivera Visual Studio tooinclude hello Application Insights SDK i ditt program. När i Application Insights, kan du använda hello programavbildningen toodiscover visuellt är vilka delar i tillämpningsprogrammet körs felaktiga eller inte. För de delar som inte är felfri, finns fel och undantag redan för undersökning. Du kan använda hello olika analyser i Application Insights toogo djupare. Om du inte är säker om hello felet, kan du använda hello Visual Studio debugger tootrace i koden och PIN-kod punkt ytterligare problem. 

Mer information finns i [övervakning Web Apps](../application-insights/app-insights-azure-web-apps.md) och toohello innehållsförteckning hello vänster finns anvisningar för olika typer av appar och språk.  

### <a name="scenario-2--debug-an-azure-net-web-application-for-errors-that-only-show-in-production"></a>Scenario 2 – felsöka ett Azure .NET-webbprogram för fel som endast visar i produktion 

> [!NOTE]
> Dessa funktioner finns i förhandsgranskningen. 

**hello bästa alternativet är toouse Application Insights och om möjligt Visual Studio för hello fullständig felsökning upplevelse.**

Använda hello Application Insights ögonblicksbild felsökare toodebug din app. När ett visst tröskelvärde för fel inträffar med olika komponenter fångar hello system automatiskt telemetri i windows tid som kallas ”ögonblicksbilder”. hello avbildas är säkra för produktion moln eftersom den är liten tillräckligt med inte tooaffect prestanda men tillräckligt betydande tooallow spårning.  hello system kan ta flera ögonblicksbilder. Du kan titta på en tidpunkt i hello Azure-portalen eller använda Visual Studio för fullständiga hello-upplevelsen. Med Visual Studio kan utvecklare igenom den ögonblicksbilden som om de felsökning i realtid. Lokala variabler, parametrar, minne och ramar är alla tillgängliga. Utvecklare måste beviljas åtkomst toothis produktionsdata via en RBAC-roll.  

Mer information finns i [ögonblicksbild felsökning](../application-insights/app-insights-snapshot-debugger.md). 

### <a name="scenario-3--debug-an-azure-application-that-uses-containers-or-microservices"></a>Scenario 3 – felsöka ett Azure-program som använder behållare eller mikrotjänster 

**Samma som scenario 1. Använda Application Insights, Azure-Monitor och Visual Studio tillsammans** Application Insights stöder också samla in telemetri från processer som körs i behållare och mikrotjänster (Kubernetes, Docker, Azure Service Fabric). Mer information [finns i den här videon om hur du felsöker behållare och mikrotjänster](https://go.microsoft.com/fwlink/?linkid=848184). 


### <a name="scenario-4--fix-performance-issues-in-your-azure-application"></a>Scenario 4 – korrigera prestandaproblem i ditt Azure-program

Hej [Application Insights profiler](../application-insights/app-insights-profiler.md) är utformad toohelp felsöka dessa typer av problem. Du kan identifiera och felsöka problem med prestanda för program som körs i Apptjänster (Web Apps Logic Apps Mobilappar, API Apps) och andra beräkningsresurser som virtuella datorer, virtuella skaluppsättningar (VMSS), Cloud Services och Service Fabric. 

> [!NOTE]
> Möjlighet tooprofile virtuella datorer, virtuella skaluppsättningar (VMSS), Cloud Services och tjänster Fabric finns i förhandsgranskningen.   

Dessutom meddelas du proaktivt via e-post om vissa typer av fel som långsam sidinläsningstider av hello Smart Detection tool.  Du behöver inte toodo konfiguration på det här verktyget. Mer information finns i [Smart identifiering - Prestandaavvikelser](../application-insights/app-insights-proactive-performance-diagnostics.md) och [Smart identifiering - Prestandaavvikelser](https://azure.microsoft.com/blog/Enhancments-ApplicationInsights-SmartDetection/preview).



## <a name="next-steps"></a>Nästa steg
Lär dig mer om

* [Azure Övervakare i ett videoklipp från Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Komma igång med Azure Övervakare](monitoring-get-started.md)
* [Azure Diagnostics](../azure-diagnostics.md) om du försöker toodiagnose problem i din molntjänst, virtuell dator, virtuella skala anges eller Service Fabric-programmet.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) om du försöker toodiagnostic problem i din App Service Web app.
* [Logga Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) och hello [Operations Management Suite](https://www.microsoft.com/oms/) produktion övervakningslösning