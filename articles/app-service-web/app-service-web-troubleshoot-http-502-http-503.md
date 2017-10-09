---
title: "aaaFix 502 Felaktig gateway, 503-tjänsten inte är tillgänglig | Microsoft Docs"
description: "Felsöka 502 Felaktig gateway och 503 tjänsten tillgänglig fel i webbappen i Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: top-support-issue
keywords: "502 Felaktig gateway 503-tjänsten tillgänglig, fel 503, fel 502"
ms.assetid: 51cd331a-a3fa-438f-90ef-385e755e50d5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: d9d8dcddaac930967a2e8d2bfd8cad09e6824c17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-http-errors-of-502-bad-gateway-and-503-service-unavailable-in-your-azure-web-apps"></a>Felsök HTTP-fel ”502 Felaktig gateway” och ”503 inte tillgänglig” i Azure-webbappar
”502 Felaktig gateway” och ”503 inte tillgänglig” är vanliga fel i ditt webbprogram som finns i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Den här artikeln hjälper dig att felsöka felen.

Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/). Alternativt kan du även filen en incident i Azure-supporten. Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och klicka på **Get Support**.

## <a name="symptom"></a>Symtom
När du bläddrar toohello webbappen returnerar ett HTTP ”502 felaktig Gateway” fel eller en HTTP-fel ”503 inte tillgänglig”.

## <a name="cause"></a>Orsak
Det här problemet beror ofta på nivån programproblem som:

* begäranden som tar lång tid
* program med hög minne/processor
* programmet kraschar på grund av tooan undantag.

## <a name="troubleshooting-steps-toosolve-502-bad-gateway-and-503-service-unavailable-errors"></a>Felsökning av steg toosolve ”502 Felaktig gateway” och ”503 inte tillgänglig” fel
Felsökning kan delas in i tre olika uppgifter i sekventiell ordning:

1. [Observera och övervaka programmets beteende](#observe)
2. [Samla in data](#collect)
3. [Minimera hello problemet](#mitigate)

[App Service Web Apps](/services/app-service/web/) ger olika alternativ i varje steg.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Observera och övervaka programmets beteende
#### <a name="track-service-health"></a>Spåra tjänstens hälsa
Microsoft Azure publicizes varje gång en tjänst avbrott eller prestanda försämras. Du kan spåra hello hälsotillståndet för hello-tjänsten på hello [Azure Portal](https://portal.azure.com/). Mer information finns i [spåra tjänstens hälsa](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Övervaka ditt webbprogram
Det här alternativet kan du toofind ut om programmet har med eventuella problem. I din webbapps blad klickar du på hello **begäranden och fel** panelen. Hej **mått** bladet visar alla hello mått som du kan lägga till.

Några av hello mått som du kanske vill toomonitor för webbappen

* Genomsnittlig minne arbetsminne
* Genomsnittlig svarstid
* CPU-tid
* Arbetsminnet för minne
* Begäranden

![övervaka webbprogram ska lösa HTTP-fel 502 Felaktig gateway och 503 inte tillgänglig](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

Mer information finns i:

* [Övervakaren Web Apps i Azure App Service](web-sites-monitor.md)
* [Få varningsmeddelanden](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

<a name="collect" />

### <a name="2-collect-data"></a>2. Samla in data
#### <a name="use-hello-azure-app-service-support-portal"></a>Använd hello Azure App Service stöd Portal
Web Apps ger hello möjlighet tootroubleshoot problem relaterade tooyour webbprogram genom att titta på http-loggar, händelseloggar, process Dumpar och mycket mer. Du kan komma åt den här informationen med hjälp av vår portal Support vid **http://&lt;appens namn >.scm.azurewebsites.net/Support**

hello stöd för Azure App Service portal innehåller tre separata flikar toosupport hello tre steg i ett vanligt scenario för felsökning:

1. Se aktuella beteende
2. Analysera genom att samla in diagnostikinformation och köra hello inbyggda analyzers
3. Minimera

Om hello problemet sker just nu, klickar du på **analysera** > **diagnostik** > **diagnostisera nu** toocreate en diagnostiska session, som samlar in HTTP loggar, loggar i Loggboken, minne Dumpar, PHP felloggarna och PHP bearbeta rapporten.

När hello data samlas in, kommer den även kör en analys på hello data och ger dig en HTML-rapport.

Om du vill toodownload hello data, som standard lagras den i hello D:\home\data\DaaS mapp.

Mer information om hello stöd för Azure App Service portal finns [nya uppdateringar tooSupport plats tillägget för Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-hello-kudu-debug-console"></a>Använd hello Kudu-Felsökningskonsolen
Web Apps levereras med en Felsökningskonsolen som du kan använda för felsökning, utforska, ladda upp filer, samt JSON-slutpunkter för att hämta information om din miljö. Detta kallas hello *Kudu konsolen* eller hello *SCM instrumentpanelen* för ditt webbprogram.

Du kan komma åt den här instrumentpanelen genom att gå toohello länk **https://&lt;appens namn >.scm.azurewebsites.net/**.

Några av hello saker som Kudu tillhandahåller är:

* inställningar för ditt program
* loggström
* diagnostiska dump
* Felsöka konsolen där du kan köra Powershell-cmdlets och grundläggande DOS-kommandon.

En annan användbar funktion i Kudu är att, om ditt program är att första chans undantag, kan du använda Kudu och hello SysInternals verktyget Procdump toocreate minnesdumpar. Dessa minnesdumpar är ögonblicksbilder av hello processen och ofta kan hjälpa dig att felsöka mer komplicerade problem med ditt webbprogram.

Mer information om funktionerna i Kudu finns [Azure Websites online-verktyg som du bör känna till om](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-hello-issue"></a>3. Minimera hello problemet
#### <a name="scale-hello-web-app"></a>Skala hello-webbprogram
I Azure App Service kan för bättre prestanda och genomflöde, du justera hello skala där du kör programmet. Skala upp ett webbprogram omfattar två relaterade åtgärder: ändra din App Service-plan tooa högre prisnivån och konfigurera vissa inställningar när du har växlat toohello högre prisnivå.

Läs mer om att skala [skala en webbapp i Azure App Service](web-sites-scale.md).

Du kan dessutom välja toorun programmet på mer än en instans. Detta inte bara ger mer bearbetningskapacitet, men ger dig även vissa delar av feltolerans. Om hello processen kraschar på en instans fortsätter hello instanser fortfarande fler begäranden.

Du kan ange hello skalning toobe manuell eller automatisk.

#### <a name="use-autoheal"></a>Använd säkert att AutoHeal
Säkert att AutoHeal återanvänds hello arbetsprocessen för din app baserat på dina inställningar (t.ex. konfigurationsändringar begäranden, minnesbaserade gränser eller hello tid behövs tooexecute en begäran). För hello mesta är återvinning hello processen hello snabbaste sättet toorecover från ett problem. Även om du kan alltid starta om hello webbprogrammet från direkt i hello Azure-portalen, gör säkert att AutoHeal det automatiskt åt dig. Allt du behöver toodo är att lägga till vissa utlösare i hello rotens web.config för ditt webbprogram. Observera att de här inställningarna fungerar i hello samma sätt även om programmet inte är en .net en.

Mer information finns i [automatisk återställning Azure webbplatser](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-hello-web-app"></a>Starta om hello-webbprogram
Det här är ofta hello enklaste sättet toorecover engångsproblem. På hello [Azure Portal](https://portal.azure.com/), på bladet för ditt webbprogram har hello alternativ toostop eller starta om din app.

 ![Starta om appen toosolve HTTP-fel 502 Felaktig gateway och 503 inte tillgänglig](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Du kan också hantera ditt webbprogram med hjälp av Azure Powershell. Mer information finns i [Använda Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).

