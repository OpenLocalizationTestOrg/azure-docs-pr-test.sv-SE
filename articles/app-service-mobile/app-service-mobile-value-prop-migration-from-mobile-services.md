---
title: "aaaI använder Mobile Services, Apptjänst sätt?"
description: "Lär dig mer om vilken nytta apptjänst tooyour befintliga Mobile Services-projekt."
services: app-service\mobile
documentationcenter: ios
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 26b68a11-8352-4f78-acd2-e4e0ec177781
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 315cc6eedcdca6c3f9f9bb9fd5ec7baf655b7e20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started"> </a>Jag använder Mobile Services. Vad bidrar Apptjänst med?
## <a name="overview"></a>Översikt
Din nuvarande mobiltjänst Mobile Services är säker och kommer att kunna användas även i fortsättningen. Det finns dock antal fördelar hello *Azure App Service* plattformen ger din mobilapp som inte är tillgängliga i Mobile Services:

* enklare och mer kostnadseffektivt utbud av appar för både webb- och mobilklienter
* nya värdfunktioner såsom WebJobs, anpassade CName-poster, bättre övervakning
* nyckelfärdig integrering med Traffic Manager
* Anslutningen tooyour lokala resurser och VPN-anslutningar via VNet i tillägget tooHybrid anslutningar
* övervakning, aviseringar och felsökning av appar med NewRelic eller AppInsights
* Större utbud av hello underliggande beräkningsresurser och prissättning
* inbyggd automatisk skalning, belastningsutjämning och prestandaövervakning
* inbyggd mellanlagring, säkerhetskopiering, återställning och möjlighet till testning i produktionsmiljö

## <a name="new-hosting-features"></a>Nya värdfunktioner
I *Azure App Service* hello *Mobilapp* hello i serverdelen koden körs i samma behållare som Web App och API-App. Därmed kan du dra nytta av alla hello-funktioner i den här behållaren, inklusive några som inte är för närvarande finns i Mobile Services:

* Du kan lägga till serverdelslogik som körs hela tiden via WebJobs.
* Du kan säkerställa att serverdelskoden alltid körs.
* Använd anpassade CNAME-resursposter tooprovide eget och stabil namn tooyour mobila serverdelsslutpunkter.
* Du kan geoskala appen med Traffic Manager.
* Du kan ha med vilka bibliotek och paket du vill.
* (För .NET) Du kan använda alla funktioner i ASP.NET, även MVC.
* (För Node.js) Utnyttja alla bibliotek med endast JavaScript hello Node-ekosystemet, inklusive vanliga MVC-bibliotek.

## <a name="access-on-premises-data-using-vnet"></a>Möjlighet att nå lokala data via VNet
Med Mobile Services dag som du kan använda Hybridanslutningar tooaccess redan lokala resurser. Det finns dock tillfällen då det är bäst med VPN-lösning. Med *Azure Apptjänst* kan du använda Azure VNet för serverdelskoden till mobilappar.

## <a name="use-your-favorite-backend-language"></a>Använd det serverdelsspråk som du gillar bäst
*Azure Apptjänst* erbjudanden bredare och bättre stöd för ASP.NET och Node.js-plattformarna, inklusive åtkomst toohello senaste körningarna.

## <a name="set-up-automatic-scale"></a>Automatisk skalning
Med Mobile Services kördes alla instanser av serverdelskoden på små virtuella datorer. *Azure Apptjänst* kan du tooselect hello storleken på virtuella datorer från en mycket större antal alternativ. Du kan även snabbt skala upp eller ut toohandle alla inkommande kundbelastning utifrån olika prestandamått.

## <a name="be-in-hello-know"></a>Att i hello ”vet”
Reagera tooissues i realtid genom övervakning och aviseringar tooautomatically meddela dig och din grupp. Integrera avancerad och övervakningsfunktioner från New Relic och AppInsights ännu bättre tooget-inblick i hur din mobila app utförs. Med *Azure App Service* kan du nu ställa varningar baserat på flera olika prestandamått, antingen genom att programmera eller via hello Azure-portalen.

## <a name="keep-your-assets-safe"></a>Dina tillgångar är trygga
Serverdelen och databasen kan säkerhetskopieras automatiskt. Kod och data är skyddade vid katastrofer och lätt återställas så att du toorun din verksamhet utan problem.

## <a name="ready-stage-go"></a>Klara, mellanlagra, kör!
Med *Azure Apptjänst* kan du nu skapa flera olika privata testnings- och mellanlagringsmiljöer för dina mobilappar. Använd dem tooperform testning innan du distribuerar. Växla tooproduction utan avbrott. Webbappar är förinstallerade, vilket ger hello bästa möjliga kundupplevelse.

Du kan komma igång och utnyttja alla fördelarna med *Apptjänst* för din befintliga mobiltjänst genom att gå igenom den här [kursen](app-service-mobile-migrating-from-mobile-services.md).
