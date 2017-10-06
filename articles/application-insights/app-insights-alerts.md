---
title: aaaSet aviseringar i Azure Application Insights | Microsoft Docs
description: "Håll dig informerad om långa svarstider, undantag, andra prestanda och användning av ändringar i ditt webbprogram."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: f8ebde72-f819-4ba5-afa2-31dbd49509a5
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: e160cb173e68fda2e6d97f29da342c46b7ac4f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-alerts-in-application-insights"></a>Ställ in aviseringar i Application Insights
[Azure Application Insights] [ start] kan meddela dig toochanges i prestanda eller användningsområden mätvärden i ditt webbprogram. 

Application Insights övervakar din aktiva app på en [flera olika plattformar] [ platforms] toohelp du diagnostisera prestandaproblem och förstå användningsmönster.

Det finns tre typer av aviseringar:

* **Mått aviseringar** berättar när ett mått överskrider ett tröskelvärde för vissa punkt - till exempel svarstider, antalet undantag, CPU-användning eller sidvisningar. 
* [**Webbtester** ] [ availability] berättar när platsen är inte tillgänglig på hello internet eller svarar långsamt. [Lär dig mer][availability].
* [**Proaktiv diagnostik** ](app-insights-proactive-diagnostics.md) konfigureras automatiskt toonotify du om ovanliga prestandamönster.

Vi fokusera på mått aviseringar i den här artikeln.

## <a name="set-a-metric-alert"></a>Ange en avisering om mått
Öppna hello Varningsregler bladet och Använd hello knappen Lägg till. 

![Välj Lägg till avisering i hello Varningsregler bladet. Ange din app som hello resurs toomeasure, ange ett namn för hello avisering och väljer ett mått.](./media/app-insights-alerts/01-set-metric.png)

* Ange hello resursen innan hello andra egenskaper. **Välj hello ”(komponenter)” resurs** om du vill tooset aviseringar om prestanda- eller mått.
* hello-namn som du ger toohello avisering måste vara unika inom hello resursgrupp (inte bara program).
* Vara försiktig toonote hello enheter där du uppmanas tooenter hello tröskelvärdet.
* Om du kryssrutan hello ”e-ägare...”, skickas aviseringar med e-tooeveryone som har åtkomst toothis resursgruppen. tooexpand detta in personer, lägga till dem toohello [resursgrupp eller prenumeration](app-insights-resources-roles-access-control.md) (inte hello resurs).
* Om du anger ”ytterligare e-postmeddelanden” skickas aviseringar toothose enskilda användare eller grupper (om du har markerat hello ”e-ägare...” rutan). 
* Ange en [webhook adress](../monitoring-and-diagnostics/insights-webhooks-alerts.md) om du har skapat ett webbprogram som svarar tooalerts. Det kallas både när hello avisering aktiveras och när den är löst. (Men Observera att för närvarande, frågeparametrar skickas inte via webhook-egenskaper.)
* Du kan inaktivera eller aktivera hello varning: Se hello knappar hello överst i hello-bladet.

*Hello Lägg till avisering om visas inte.* 

* Använder du ett organisationskonto? Du kan ställa in varningar om resurser programmet toothis för ägare eller deltagare. Ta en titt på hello Access Control-bladet. [Lär dig mer om åtkomstkontroll][roles].

> [!NOTE]
> I hello aviseringar bladet du se att det finns redan en avisering uppsättning: [Proactive Diagnostics](app-insights-proactive-failure-diagnostics.md). hello automatisk avisering övervakar ett viss mått, misslyckandegrad. Om du inte toodisable hello proaktiv avisering behöver du inte tooset aviseringen på misslyckandegrad. 
> 
> 

## <a name="see-your-alerts"></a>Se aviseringar
Du får ett e-postmeddelande när en avisering ändrar tillstånd mellan inaktivt och aktivt. 

hello aktuell status för varje avisering visas i hello Varningsregler bladet.

Det finns en sammanfattning av de senaste aktivitet i hello aviseringar listrutan:

![Aviseringar listrutan](./media/app-insights-alerts/010-alert-drop.png)

hello historik över tillståndsändringar har hello aktivitetsloggen:

![Klicka på inställningar, granskningsloggar på hello översikt bladet](./media/app-insights-alerts/09-alerts.png)

## <a name="how-alerts-work"></a>Så fungerar aviseringar
* En avisering har tre lägen: ”aldrig aktiverats”, ”aktiverad” och ”löst”. Aktiverad innebär hello villkor du anger var true när den senast utvärderades.
* En avisering genereras när en avisering ändrar tillstånd. (Om hello varningsvillkor var redan sant när du skapade hello avisering kan du kanske inte får ett meddelande tills hello villkor går false.)
* Varje meddelande genererar ett e-postmeddelande om du markerat hello e-postmeddelanden rutan eller angivna e-postadresser. Du kan också titta på hello aviseringar listrutan.
* En avisering utvärderas för varje gång ett mått tas emot, men inte på annat sätt.
* hello utvärderingen aggregerar hello mått över hello föregående period och sedan jämförs toohello tröskelvärdet toodetermine hello nya statusen.
* hello period som du anger hello intervallet över vilket mått aggregeras. Det påverkar inte hur ofta hello avisering utvärderas: som beror på hello ofta ankomsten av mått.
* Om inga data tas emot för ett viss mått under en viss tid har hello mellanrummet mellan olika effekter på aviseringen utvärdering och hello diagram i mått explorer. I mått explorer om inga data visas under längre tid än hello diagrammets insamlingsintervall hello diagrammet visar värdet 0. Men en avisering baserat på hello samma mått inte är vara reevaluated och hello aviseringens status förblir oförändrad. 
  
    När data kommer så småningom, hoppar hello diagram tillbaka tooa annat värde än noll. hello avisering utvärderar baserat på hello data är tillgängliga för hello period som du angett. Om hello nya datapunkt är hello endast en tillgänglig i hello period, baseras hello sammanställd bara att datapunkt.
* En avisering flimra ofta mellan varning och felfritt tillstånd, även om du anger en längre tid. Detta kan inträffa om hello måttvärdet håller muspekaren runt hello tröskelvärdet. Det finns inga betraktas i hello tröskelvärdet: hello övergången tooalert sker på samma värde som hello övergången toohealthy hello.

## <a name="what-are-good-alerts-tooset"></a>Vad är bra aviseringar tooset?
Det beror på ditt program. toostart med, det bäst inte tooset för många mått. Lägg ned lite tid tittar på dina mått diagram medan appen körs tooget en känsla för hur den fungerar normalt. Det hjälper dig att hitta sätt tooimprove dess prestanda. Ställa in aviseringar tootell du när hello mått går utanför normal hello-zon. 

Populära aviseringar är:

* [Webbläsaren mått][client], särskilt webbläsare **sidan laddningstider**, är bra för webbprogram. Om sidan har många skript, ska du leta efter **webbläsarundantag**. I ordning tooget dessa mätvärden och aviseringar, har du tooset [webbsida övervakning][client].
* **Serversvarstid** för hello serversidan av webbprogram. Hålla ett öga på den här mått toosee samt att konfigurera aviseringar om den oproportionerligt varierar med hög begäran priser: variationen kan tyda på att din app körs slut på resurser. 
* **Undantag för** -toosee dem du har toodo vissa [ytterligare inställningar](app-insights-asp-net-exceptions.md).

Glöm inte som [proaktiv fel hastighet diagnostik](app-insights-proactive-failure-diagnostics.md) automatiskt övervakaren hello hastighet med vilken din app svarar toorequests med felkoder. 

## <a name="automation"></a>Automation
* [Använd PowerShell tooautomate ställa in aviseringar](app-insights-powershell-alerts.md)
* [Använd webhooks tooautomate svarar tooalerts](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="see-also"></a>Se även
* [Tillgänglighetstester för webbprogram](app-insights-monitor-web-app-availability.md)
* [Automatisera ställa in aviseringar](app-insights-powershell-alerts.md)
* [Proaktiv diagnostik](app-insights-proactive-diagnostics.md) 

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

