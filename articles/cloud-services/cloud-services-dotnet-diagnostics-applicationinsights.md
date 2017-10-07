---
title: "aaaTroubleshoot molntjänster med hjälp av Application Insights | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Molntjänsten problem med hjälp av Application Insights tooprocess data från Azure-diagnostik."
services: cloud-services
documentationcenter: .net
author: sbtron
manager: timlt
editor: tysonn
ms.assetid: e93f387b-ef29-4731-ae41-0676722accb6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: saurabh
ms.openlocfilehash: 972924d9e6d1fe33d5c19b006d482de52ffb0ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-services-using-application-insights"></a>Felsöka molntjänster med hjälp av Application Insights
Med [Azure SDK 2.8](https://azure.microsoft.com/downloads/) och tillägg för Azure-diagnostik 1.5, du kan skicka data i Azure-diagnostik för din molntjänst direkt tooApplication insikter. Hej loggar samlas in av Azure-diagnostik&mdash;inklusive programloggarna, Windows-händelseloggar, ETW-loggar och prestandaräknare&mdash;kan skickas tooApplication insikter. Sedan kan du visualisera informationen i hello Application Insights-portalen Användargränssnittet. Du kan sedan använda hello Application Insights SDK tooget insikter om mått och loggar som kommer från ditt program, samt hello system- och infrastruktur-nivå som kommer från Azure-diagnostik.

## <a name="configure-azure-diagnostics-toosend-data-tooapplication-insights"></a>Konfigurera Azure-diagnostik toosend data tooApplication insikter
Följ dessa steg tooset upp din cloud service-projekt toosend Azure Diagnostics data tooApplication insikter.

1. Högerklicka på en roll i Visual Studio Solution Explorer och markera **egenskaper** tooopen hello rollen designer.

    ![Solution Explorer rollegenskaper][1]

2. I hello **diagnostik** avsnittet hello rollen Designer, väljer hello **skicka diagnostik data tooApplication insikter** alternativet.

    ![Rollen designer skickar diagnostik data tooapplication insikter][2]

3. Välj hello Application Insights-resurs som du vill toosend hello Azure-diagnostikdata till hello dialogrutan som öppnas. hello dialogrutan kan du tooselect en befintlig Application Insights-resurs från prenumerationen eller toomanually anger du en instrumentation nyckel för Application Insights-resurs. Mer information om hur du skapar en Application Insights-resursen finns [skapar en ny Application Insights-resurs](../application-insights/app-insights-create-new-resource.md).

    ![Välj application insights-resurs][3]

    När du har lagt till hello Application Insights-resurs, hello instrumentation nyckel för den här resursen lagras som en konfigurationsinställning för tjänst med namnet hello **APPINSIGHTS_INSTRUMENTATIONKEY**. Du kan ändra Konfigurationsinställningen för varje tjänstkonfiguration eller miljö. toodo så, Välj en annan konfiguration från hello **tjänstkonfiguration** listan och ange en ny instrumentation nyckel för denna konfiguration.

    ![Välj tjänstkonfiguration][4]

    Hej **APPINSIGHTS_INSTRUMENTATIONKEY** Konfigurationsinställningen används av Visual Studio tooconfigure hello diagnostik tillägg med hello lämplig Application Insights resursinformation vid publicering. hello Konfigurationsinställningen är ett bekvämt sätt att definiera olika instrumentation nycklar för olika konfigurationer. Visual Studio kommer översätta inställningen och infoga i hello diagnostik tilläggets offentliga konfiguration under hello publiceringsprocessen. toosimplify hello konfigureringen av hello diagnostik tillägget med PowerShell, hello paketet utdata från Visual Studio innehåller också hello offentliga konfigurations-XML med hello lämplig Application Insights instrumentation nyckel. hello offentliga config-filer som skapas i hello tilläggsmappen och följer hello mönster *PaaSDiagnostics.&lt; RoleName&gt;. PubConfig.xml*. PowerShell-baserade distributioner kan använda det här mönstret toomap varje configuration tooa roll.

4) tooconfigure Azure diagnostics toosend Felnivån loggar och alla prestandaräknare som samlas in av hello Azure diagnostics agent tooApplication Insights, aktivera hello **skicka diagnostik data tooApplication insikter** alternativet. 

    Om du vill toofurther konfigurera vilka data skickas tooApplication insikter, måste du manuellt redigera hello *diagnostics.wadcfgx* fil för varje roll. Se [konfigurera Azure-diagnostik toosend data tooApplication insikter](#configure-azure-diagnostics-to-send-data-to-application-insights) toolearn mer om att manuellt uppdatera hello konfiguration.

När hello molntjänst är konfigurerade toosend Azure diagnostics data tooapplication insikter, kan du distribuera den tooAzure normalt kontrollerar hello Azure diagnostics tillägget har aktiverats. Mer information finns i [publicering av en tjänst i molnet med hjälp av Visual Studio](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Visa data i Azure-diagnostik i Application Insights
hello Azure diagnostisk telemetri visas i hello Application Insights-resurs som konfigurerats för Molntjänsten.

Azure diagnostics loggen typer mappa tooApplication insikter begrepp på följande sätt:

* Prestandaräknare visas som anpassade mått i Application Insights.
* Windows-händelseloggar visas som spårningar och anpassade händelser i Application Insights.
* Program loggar, ETW-loggar och diagnostik infrastruktur loggar visas som spåren i Application Insights.

tooview data i Azure-diagnostik i Application Insights gör något av följande hello:

* Använd [Metrics explorer](../application-insights/app-insights-metrics-explorer.md) toovisualize eventuella anpassade prestandaräknare eller räknar olika typer av händelser i händelseloggen i Windows.

    ![Anpassade mått i Metrics Explorer][5]

* Använd [Sök](../application-insights/app-insights-diagnostic-search.md) toosearch över hello spårningsloggar som skickas av Azure-diagnostik. Till exempel om ett ohanterat undantag har orsakat hello rollen toocrash och återvinning, information om hello undantag visas i hello *programmet* kanal för *Windows-händelseloggen*. Du kan använda Sök toolook på hello händelseloggen fel och få hello fullständig stack-spårning för hello undantag toohelp hitta hello orsaken till problemet hello.

    ![Sök spårningar][6]

## <a name="next-steps"></a>Nästa steg
* [Lägg till Molntjänsten för hello Application Insights SDK tooyour](../application-insights/app-insights-cloudservices.md) toosend data om begäranden, undantag, beroenden och eventuella anpassade telemetri från ditt program. När den kombineras med hello Azure Diagnostics data, den här informationen som du får en fullständig överblick över dina program och system, hello i samma Application Insights-resurs.  

<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
