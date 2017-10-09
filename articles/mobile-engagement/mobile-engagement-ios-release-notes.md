---
title: aaaAzure Mobile Engagement iOS SDK viktig information | Microsoft Docs
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: ae29d200ebb1784357b29edbd1f66b71df0778cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Azure Mobile Engagement iOS SDK viktig information

## <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Fast Aktivitetsikoner avmarkerad i bakgrunden.
* Fast varningar på XCode 9 om API: er som inte anropas i huvudkön.
* Fast en minnesläcka i Reach-avsökningar.
* Bort stöd för iOS 6.X. Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 7.

## <a name="401-12132016"></a>4.0.1 (12/13/2016)
* Bättre loggen leverans i bakgrunden.

## <a name="400-09122016"></a>4.0.0 (09/12/2016)
* Fast meddelande inte åtgärdade på iOS 10-enheter.
* Föråldrad XCode 7.

## <a name="324-06302016"></a>3.2.4 (06/30/2016)
* Fast aggregering mellan tekniska loggar och andra loggar.

## <a name="323-06072016"></a>3.2.3 (06/07/2016)
* Fast hello bugg där leverans av feedback inte har rapporterats om appen hello bakgrund.
* Optimerad hello skickar tekniska loggar.

## <a name="322-04072016"></a>3.2.2 (04/07/2016)
* Fast programfel på http-begäran annullering, vilket ibland leder toocrash.

## <a name="321-12112015"></a>3.2.1 (12/11/2015)
* Fast hello fördröjning när en ny instans av app ska utlösas av ett meddelande med djuplänkar

## <a name="320-10082015"></a>3.2.0 (10/08/2015)
* Aktivera Bitcode i hello SDK toomake den fungerar med **Xcode 7**.
* Fast buggar relaterade tooin appen meddelanden.
* Gjort hello i appen meddelanden mer tillförlitlig vid låg batterinivå och andra sådana scenarier.
* Ta bort extra konsolen loggar som genereras av 3 part-biblioteket.

## <a name="310-08262015"></a>3.1.0 (08/26/2015)
* Åtgärda fel för iOS 9-kompatibilitet med tredjeparts-biblioteket. Den orsakar krascher medan skickar avsöker resultat, information om programmet eller extra data.

## <a name="300-06192015"></a>3.0.0 (06/19/2015)
* Mobile Engagement använder tysta Push-meddelanden.
* Bort stöd för iOS 4.X. Från och med den här versionen hello distributionsmålet för ditt program måste vara minst iOS 6.

## <a name="220-05212015"></a>2.2.0 (05/21/2015)
* hello Mobile Engagement enhets-id för enheter < iOS 6 nu baserat på ett GUID som genereras vid tidpunkten för installationen.

## <a name="210-04242015"></a>2.1.0 (04/24/2015)
* Tillagda Swift kompatibilitet.
* När du klickar på ett meddelande köra hello åtgärd URL: en är nu höger när hello-programmet öppnas.
* Tillagda saknas huvudfilen i SDK-paketet.
* Ett problem har åtgärdats när hello Mobile Engagement krasch rapport har inaktiverats.

## <a name="200-02172015"></a>2.0.0 (02/17/2015)
* Första versionen av Azure Mobile Engagement
* konfiguration av appId/sdkKey ersätts av en sträng anslutningskonfiguration.
* Ta bort API toosend och ta emot godtycklig XMPP meddelanden från valfri XMPP entiteter.
* Ta bort API toosend och ta emot meddelanden mellan enheter.
* Förbättringar av säkerhet.
* Spårning av SmartAd tas bort.
