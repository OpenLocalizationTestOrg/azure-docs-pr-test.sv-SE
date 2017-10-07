---
title: "aaaCreate Cordova-app på Azure Apptjänst Mobilappar | Microsoft Docs"
description: "Följ den här självstudiekursen tooget igång med att använda mobilappserverdelar för Apache Cordova-utveckling"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
tags: 
keywords: cordova, javascript, mobil, klient
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: hero-article
ms.date: 07/07/2017
ms.author: glenga
ms.openlocfilehash: 4981cbc0686c15230019ac9f30495f30cbf2c791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-cordova-app"></a>Skapa en Apache Cordova-app
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooan Apache Cordova-mobilapp med hjälp av en mobilappsserverdel i Azure.  Du skapar både en ny serverdel för mobilappen och en enkel Apache Cordova-app med en *att göra-lista* som lagrar appdata i Azure.

Den här kursen är en förutsättning för alla andra Apache Cordova-kurs om att använda funktionen för hello Mobile Apps i Azure App Service.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande krav:

* En PC-dator med [Visual Studio Community 2017] eller senare.
* [Visual Studio Tools för Apache Cordova].
* Ett [aktivt Azure-konto](https://azure.microsoft.com/pricing/free-trial/).

Du kan också kringgå Visual Studio och direkt använda kommandoraden för hello Apache Cordova.  Kommandoraden för hello är användbart när hello kursen på en Mac-dator.  Kompilera klientprogram i Apache Cordova hello kommandoraden omfattas inte av den här kursen.

## <a name="create-an-azure-mobile-app-backend"></a>Skapa serverdelen för en Azure-mobilapp
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Titta på en video som visar liknande steg](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-hello-server-project"></a>Konfigurera hello serverprojekt
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-apache-cordova-app"></a>Hämta och köra hello Apache Cordova-app
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Nästa steg
Nu när du har slutfört den här snabbstartsguide gå vidare tooone av hello följande kurser:

* [Lägg till Offline Data](app-service-mobile-cordova-get-started-offline-data.md) tooyour Apache Cordova-app.
* [Lägg till autentisering](app-service-mobile-cordova-get-started-users.md) tooyour Apache Cordova-app.
* [Lägg till Push-meddelanden](app-service-mobile-cordova-get-started-push.md) tooyour Apache Cordova-app.

Läs mer om nyckelbegrepp i Azure Apptjänst.

* [Offlinedata]
* [Autentisering]
* [Push-meddelanden]

Lär dig hur toouse hello SDK: er.

* [Apache Cordova-SDK]
* [ASP.NET Server-SDK]
* [Node.js Server-SDK]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2017]: http://www.visualstudio.com/
[Visual Studio Tools för Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Offlinedata]: app-service-mobile-offline-data-sync.md
[Autentisering]: app-service-mobile-auth.md
[Push-meddelanden]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
