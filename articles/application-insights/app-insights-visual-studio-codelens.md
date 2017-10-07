---
title: aaaApplication insikter telemetri i Visual Studio CodeLens | Microsoft Docs
description: "Kom snabbt åt din Application Insights-begäran och undantagstelemetri med CodeLens i Visual Studio."
services: application-insights
documentationcenter: .net
author: numberbycolors
manager: carmonm
ms.assetid: 93559e44-23cb-4b9d-8425-60f7f0d0a82c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: e812aa48f2a67eea860e7ecde341855763bb8a8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-telemetry-in-visual-studio-codelens"></a>Application Insights Telemetry i Visual Studio CodeLens
Metoderna i hello-koden för ditt webbprogram kan förses med telemetri om undantag för körning och begära svarstider. Om du installerar [Azure Application Insights](app-insights-overview.md) hello telemetri visas i ditt program i Visual Studio [CodeLens](https://msdn.microsoft.com/library/dn269218.aspx) -hello anteckningar hello överst i varje funktion där du använt tooseeing användbart information, till exempel hello antalet platser hello funktionen refereras eller hello senaste personen som redigerade den.

![CodeLens](./media/app-insights-visual-studio-codelens/codelens-overview.png)

> [!NOTE]
> Application Insights i CodeLens är tillgänglig i Visual Studio 2015 Update 3 och senare, eller med hello senaste versionen av [Analytics utvecklingsverktyg tillägget](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a). CodeLens är tillgänglig i hello Enterprise och Professional-versioner av Visual Studio.
> 
> 

## <a name="where-toofind-application-insights-data"></a>Där toofind Application Insights-data
Leta efter Application Insights telemetri i hello CodeLens indikatorer för hello offentliga metodbegäranden för ditt webbprogram. CodeLens-indikatorer visas ovanför metod och andra deklarationer i C#- och Visual Basic-kod. Om Application Insights-data finns tillgängliga för en metod, kommer du att se indikatorer för begäranden och undantag, till exempel "100 begäranden, 1 % misslyckades" eller "10 undantag." Klicka på en CodeLens-indikator för mer information. 

> [!TIP]
> Application Insights begära och undantag indikatorer kan ta några sekunder för extra tooload när andra CodeLens indikatorer visas.
> 
> 

## <a name="exceptions-in-codelens"></a>Undantag i CodeLens
![TBD](./media/app-insights-visual-studio-codelens/codelens-exceptions.png)

hello undantag CodeLens indikator visar hello antalet undantag som uppstått i hello senaste 24 timmarna från hello 15 de flesta ofta återkommande undantag i ditt program under den perioden under bearbetning av begäran hello hanteras av hello-metoden.

toosee mer information, klickar du på hello undantag CodeLens indikator:

* hello procentandel förändring av antalet undantag från hello de senaste 24 timmarna relativa toohello föregående 24 timmarna
* Välj **gå toocode** toonavigate toohello källkoden för hello-funktionen att hello undantag
* Välj **Sök** tooquery alla instanser av det här undantaget som har inträffat i hello senaste 24 timmarna
* Välj **Trend** tooview en trend visualisering för förekomster av det här undantaget i hello senaste 24 timmarna
* Välj **visa alla undantag i den här appen** tooquery alla undantag som uppstått i hello senaste 24 timmarna
* Välj **utforska undantagstrender** tooview en visualiseringen trend för alla undantag som uppstått i hello senaste 24 timmarna. 

> [!TIP]
> Om du ser ”0 undantag” i CodeLens men du vet att det ska vara undantag, kontrollera att hello rätt Application Insights-resursen är markerad i CodeLens toomake. tooselect en annan resurs, högerklicka på ditt projekt i hello Solution Explorer och välja **Programinsikter > Välj källa för telemetri**. CodeLens visas bara för hello 15 de flesta ofta återkommande undantag i ditt program i hello senaste 24 timmarna, så om ett undantag är hello 16 mest ofta eller mindre, ser du ”0 undantag”. Undantag från ASP.NET vyer kanske inte visas på hello controller metoder som genereras av dessa vyer.
> 
> [!TIP]
> Om du ser "? undantag ”i CodeLens, behöver du tooassociate ditt Azure-konto med Visual Studio eller dina autentiseringsuppgifter för Azure-konto kan ha gått ut. I bägge fallen klickar du på "? undantag ”och välj **Lägg till ett konto...**  tooenter dina autentiseringsuppgifter.
> 
> 

## <a name="requests-in-codelens"></a>Begäranden i CodeLens
![TBD](./media/app-insights-visual-studio-codelens/codelens-requests.png)

hello begäran CodeLens indikatorn visar hello antal HTTP-begäranden som har underhålls av en metod i hello efter 24 timmar, plus hello procentandel av dessa begäranden som misslyckats.

toosee mer information Klicka hello begär CodeLens indikator:

* hello absoluta och procentuella ändringar i antalet begäranden, misslyckade begäranden och genomsnittliga svarstider över hello senaste 24 timmarna jämfört toohello föregående 24 timmarna
* hello tillförlitlighet hello-metoden, beräknat som hello procentandelen förfrågningar som inte fel i hello senaste 24 timmarna
* Välj **Sök** för begäranden och misslyckade begäranden tooquery alla hello (misslyckade) förfrågningar som uppstått i hello senaste 24 timmarna
* Välj **Trend** tooview en trend visualisering för begäranden, misslyckade begäranden eller genomsnittlig svarstid för gånger på hello senaste 24 timmarna.
* Välj hello namnet på hello Application Insights-resurs i hello övre vänstra hörnet hello CodeLens information visa toochange vilken resurs som är hello som källa för CodeLens data.

## <a name="next"></a>Nästa steg
|  |  |
| --- | --- |
| **[Arbeta med Application Insights i Visual Studio](app-insights-visual-studio.md)**<br/>Sök i telemetri, visa data i CodeLens och konfigurera Application Insights. Allt i Visual Studio. |![Högerklicka på hello-projektet och välj Application Insights sökning](./media/app-insights-visual-studio-codelens/34.png) |
| **[Lägga till mer information](app-insights-asp-net-more.md)**<br/>Övervaka användning, tillgänglighet, beroenden och undantag. Integrera spårningar från loggningsramverk. Skriv anpassad telemetri. |![Visual Studio](./media/app-insights-visual-studio-codelens/64.png) |
| **[Arbeta med hello Application Insights-portalen](app-insights-dashboards.md)**<br/>Instrumentpaneler, kraftfulla verktyg för diagnostik och analys, aviseringar, live-mappning över beroenden för din app och telemetriexport. |![Visual Studio](./media/app-insights-visual-studio-codelens/62.png) |

