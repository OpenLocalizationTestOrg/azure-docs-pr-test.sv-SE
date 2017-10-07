---
title: aaaCreate en ny Azure Application Insights-resurs | Microsoft Docs
description: "Manuellt konfigurera Application Insights-övervakning för ett nytt live program."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: bwren
ms.openlocfilehash: 3aba7045e1f8fe43d473f0be01dd52106ab992a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-insights-resource"></a>Skapa en Application Insights-resurs
Azure Application Insights visar data om ditt program i en Microsoft Azure *resurs*. Skapa en ny resurs är därför en del av [konfigurera Application Insights toomonitor ett nytt program][start]. I många fall kan att skapa en resurs göras automatiskt med hello IDE. Men i vissa fall kan du skapa en resurs manuellt, till exempel toohave separata resurser för utveckling och produktion versioner av programmet.

När du har skapat hello resurs kan du hämta dess instrumentation nyckel och använder som tooconfigure hello SDK i hello program. viktiga hello resurslänkar hello telemetri toohello resurs.

## <a name="sign-up-toomicrosoft-azure"></a>Registrera dig tooMicrosoft Azure
Om du inte har en [Microsoft konto måste du skaffa ett nu](http://live.com). (Om du använder tjänster som Outlook.com, OneDrive, Windows Phone och XBox Live du redan har ett Microsoft-konto.)

Du måste också en prenumeration för[Microsoft Azure](http://azure.com). Om din grupp eller organisation har en Azure-prenumeration, hello ägare kan lägga till dig tooit, med ditt Windows Live ID. Du kan endast debiteras du använder. grundläggande hello standardplanen kan under en viss experiment används utan kostnad.

När du har fått åtkomst tooa prenumeration, logga in tooApplication insikter på [http://portal.azure.com](https://portal.azure.com), och använda Live ID-toologin.

## <a name="create-an-application-insights-resource"></a>Skapa en Application Insights-resurs
I hello [portal.azure.com](https://portal.azure.com), Lägg till Application Insights-resurs:

![Klicka på Nytt, Application Insights](./media/app-insights-create-new-resource/01-new.png)

* **Programtyp** påverkar vad som visas på hello översikt bladet och hello-egenskaper som är tillgängliga i [mått explorer][metrics]. Välj Allmänt om du inte ser typen av app.
* **Prenumerationen** är din betalningskonto i Azure.
* **Resursgruppen** är för att hantera egenskaper i syfte att underlätta som åtkomstkontroll. Om du redan har skapat andra Azure-resurser kan du välja tooput nya resursen i hello samma grupp.
* **Plats** är där vi behåller dina data.
* **PIN-kod toodashboard** placerar en snabb åtkomst-panelen för din resurs på Azure-startsidan. Rekommenderas.

När appen har skapats, öppnas ett nytt blad. Det här bladet är där du ser prestanda- och användningsdata om din app. 

tooget tillbaka tooit nästa gång du loggar in tooAzure, leta efter din app Snabbkurs panelen på hello starta board (startsidan). Eller klicka på Bläddra toofind den.

## <a name="copy-hello-instrumentation-key"></a>Kopiera hello instrumentation nyckeln
hello instrumentation nyckel identifierar hello-resurs som du skapade. Du behöver den toogive toohello SDK.

![Klicka på Essentials, hello Instrumentation nyckeln CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-hello-sdk-in-your-app"></a>Installera hello SDK i din app
Installera hello Application Insights SDK i din app. Det här steget beror kraftigt på hello typ av ditt program. 

Använd hello instrumentation viktiga tooconfigure [hello SDK som du installerar programmet][start].

hello SDK innehåller standard moduler som skickar telemetri utan att behöva toowrite någon kod. tootrack användaråtgärder eller diagnostisera problem i detalj, [använder hello API] [ api] toosend egna telemetri.

## <a name="monitor"></a>Se telemetridata
Stäng hello snabb start bladet tooreturn tooyour programmet bladet i hello Azure-portalen.

Klicka på hello Sök panelen toosee [diagnostiska Sök][diagnostic], där hello första händelser visas. 

Om du väntar mer data, klickar du på **uppdatera** efter några sekunder.

## <a name="creating-a-resource-automatically"></a>Skapa en resurs automatiskt
Du kan skriva en [PowerShell-skript](app-insights-powershell.md) toocreate en resurs automatiskt.

## <a name="next-steps"></a>Nästa steg
* [Skapa en instrumentpanel](app-insights-dashboards.md)
* [Diagnostiksökning](app-insights-diagnostic-search.md)
* [Utforska mått](app-insights-metrics-explorer.md)
* [Skriv analysfrågor](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

