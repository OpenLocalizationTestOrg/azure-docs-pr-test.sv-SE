---
title: aaaTroubleshoot analyser i Azure Application Insights | Microsoft Docs
description: "Problem med Application Insights analytics? Börja här. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: bwren
ms.openlocfilehash: e3be2fbc0237440d3b8a736484434a9f276296c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a>Felsökningsanalys i Application Insights
Problem med [Application Insights Analytics](app-insights-analytics.md)? Börja här. Analytics är hello kraftfullt sökverktyg av Azure Application Insights.

## <a name="limits"></a>Begränsningar
* För närvarande är frågeresultat begränsad toojust över en vecka med senaste data.
* Webbläsare som vi testa på: senaste versionerna av Chrome, kant och Internet Explorer.

## <a name="known-incompatible-browser-extensions"></a>Kända inkompatibla webbläsartillägg
* Ghostery

Inaktivera tillägget hello eller Använd en annan webbläsare.

## <a name="e-a"></a>”Fel”
![Oväntat felskärmen](./media/app-insights-analytics-troubleshooting/010.png)

Internt fel inträffade under portal runtime-undantag.

* Rensa hello webbläsarens cacheminne. 

## <a name="e-b"></a>403... Försök tooreload
![403... Försök tooreload](./media/app-insights-analytics-troubleshooting/020.png)

En autentisering relaterade fel uppstod (under autentisering eller under åtkomst-token generering). hello-portalen kan ha något sätt återställa för utan att ändra inställningarna för webbläsaren.

* Kontrollera [cookies från tredje part är aktiverade](#cookies) i hello webbläsare. 

## <a name="authentication"></a>403... Kontrollera säkerhetszon
![403.. .verify säkerhetszon](./media/app-insights-analytics-troubleshooting/030.png)

En autentisering relaterade fel uppstod (under autentisering eller under åtkomst-token generering). hello-portalen kan ha något sätt återställa för utan att ändra inställningarna för webbläsaren.

1. Kontrollera [cookies från tredje part är aktiverade](#cookies) i hello webbläsare. 
2. Använde du en favorit, bokmärke eller en sparad länk tooopen hello Analytics portal? Är du loggade in med andra autentiseringsuppgifter än som du använde när du sparade hello länken?
3. Försök med en i-privat/incognito webbläsarfönster (när du stänger alla fönster). Har du tooprovide dina autentiseringsuppgifter. 
4. Öppna ett nytt webbläsarfönster som (vanlig) och gå för[Azure](https://portal.azure.com). Logga ut. Öppna länken och logga in med hello rätt autentiseringsuppgifter.
5. Edge- och Internet Explorer-användare kan också få det här felet när betrodd Zoninställningar inte stöds.
   
    Kontrollera att båda [Analytics-portalen](https://analytics.applicationinsights.io) och [Azure Active Directory-portalen](https://portal.azure.com) i hello samma säkerhetszon:
   
   * I Internet Explorer, öppna **Internetalternativ**, **säkerhet**, **tillförlitliga platser**, **platser**:
     
     ![Dialogrutan Internetalternativ, lägga till en plats tooTrusted platser](./media/app-insights-analytics-troubleshooting/033.png)
     
     I listan över hello-webbplatser, om någon av följande URL: er hello ingår Kontrollera att hello andra ingår också:
     
     https://Analytics.applicationinsights.IO<br/>
     https://login.microsoftonline.com<br/>
     https://login.windows.net

## <a name="e-d"></a>404 ... Resursen hittades inte
![404... resursen hittades inte](./media/app-insights-analytics-troubleshooting/040.png)

Programresursen har tagits bort från Application Insights och är inte tillgänglig längre. Detta kan inträffa om du har sparat hello URL toohello Analytics-sida.

## <a name="e-e"></a>403 ... Ingen åtkomstbehörighet för
![403... inte behörighet](./media/app-insights-analytics-troubleshooting/050.png)

Du har inte behörighet tooopen det här programmet i Analytics.

* Fick du hello länk från någon annan? Be dem att du arbetar i hello toomake [läsare eller deltagare för den här resursgruppen](app-insights-resources-roles-access-control.md).
* Sparades hello-länk med andra autentiseringsuppgifter? Öppna hello [Azure-portalen](https://portal.azure.com), logga ut och försök sedan den här länken anger hello rätt autentiseringsuppgifter.

## <a name="html-storage"></a>403 ... HTML5-lagring
Vår portal använder HTML5 localStorage och sessionStorage.

* Chrome: Inställningar, sekretess, inställningar för innehåll.
* Internet Explorer: Internet-alternativ, fliken Avancerat, säkerhet, aktivera DOM-lagring

![403... Försök tooenable HTML5-lagring](./media/app-insights-analytics-troubleshooting/060.png)

## <a name="e-g"></a>404 ... Det gick inte att hitta prenumerationen
![404 ... Det gick inte att hitta prenumerationen](./media/app-insights-analytics-troubleshooting/070.png)

hello URL är ogiltig. 

* Öppna hello app resurs i [Application Insights-portalen](https://portal.azure.com). Använd sedan hello Analytics-knappen.

## <a name="e-h"></a>404... sidan finns inte
![404 ... Sidan finns inte](./media/app-insights-analytics-troubleshooting/080.png)

hello URL är ogiltig.

* Öppna hello app resurs i [Application Insights-portalen](https://portal.azure.com). Använd sedan hello Analytics-knappen.

## <a name="cookies"></a>Aktivera cookies från tredje part
  Se [hur toodisable tredjeparts-cookies](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), men Observera att vi behöver för**aktivera** dem.


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

