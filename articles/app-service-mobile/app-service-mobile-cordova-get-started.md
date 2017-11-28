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
# <a name="create-an-apache-cordova-app"></a><span data-ttu-id="962ab-104">Skapa en Apache Cordova-app</span><span class="sxs-lookup"><span data-stu-id="962ab-104">Create an Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="962ab-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="962ab-105">Overview</span></span>
<span data-ttu-id="962ab-106">Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooan Apache Cordova-mobilapp med hjälp av en mobilappsserverdel i Azure.</span><span class="sxs-lookup"><span data-stu-id="962ab-106">This tutorial shows you how tooadd a cloud-based backend service tooan Apache Cordova mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="962ab-107">Du skapar både en ny serverdel för mobilappen och en enkel Apache Cordova-app med en *att göra-lista* som lagrar appdata i Azure.</span><span class="sxs-lookup"><span data-stu-id="962ab-107">You create both a new mobile app backend and a simple *Todo list* Apache Cordova app that stores app data in Azure.</span></span>

<span data-ttu-id="962ab-108">Den här kursen är en förutsättning för alla andra Apache Cordova-kurs om att använda funktionen för hello Mobile Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="962ab-108">Completing this tutorial is a prerequisite for all other Apache Cordova tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="962ab-109">Krav</span><span class="sxs-lookup"><span data-stu-id="962ab-109">Prerequisites</span></span>
<span data-ttu-id="962ab-110">toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="962ab-110">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="962ab-111">En PC-dator med [Visual Studio Community 2017] eller senare.</span><span class="sxs-lookup"><span data-stu-id="962ab-111">A PC with [Visual Studio Community 2017] or newer.</span></span>
* <span data-ttu-id="962ab-112">[Visual Studio Tools för Apache Cordova].</span><span class="sxs-lookup"><span data-stu-id="962ab-112">[Visual Studio Tools for Apache Cordova].</span></span>
* <span data-ttu-id="962ab-113">Ett [aktivt Azure-konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="962ab-113">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="962ab-114">Du kan också kringgå Visual Studio och direkt använda kommandoraden för hello Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="962ab-114">You may also bypass Visual Studio and use hello Apache Cordova command line directly.</span></span>  <span data-ttu-id="962ab-115">Kommandoraden för hello är användbart när hello kursen på en Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="962ab-115">Using hello command line is useful when completing hello tutorial on a Mac computer.</span></span>  <span data-ttu-id="962ab-116">Kompilera klientprogram i Apache Cordova hello kommandoraden omfattas inte av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="962ab-116">Compiling Apache Cordova client applications using hello command line is not covered by this tutorial.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="962ab-117">Skapa serverdelen för en Azure-mobilapp</span><span class="sxs-lookup"><span data-stu-id="962ab-117">Create an Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[<span data-ttu-id="962ab-118">Titta på en video som visar liknande steg</span><span class="sxs-lookup"><span data-stu-id="962ab-118">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-hello-server-project"></a><span data-ttu-id="962ab-119">Konfigurera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="962ab-119">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-apache-cordova-app"></a><span data-ttu-id="962ab-120">Hämta och köra hello Apache Cordova-app</span><span class="sxs-lookup"><span data-stu-id="962ab-120">Download and run hello Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a><span data-ttu-id="962ab-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="962ab-121">Next Steps</span></span>
<span data-ttu-id="962ab-122">Nu när du har slutfört den här snabbstartsguide gå vidare tooone av hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="962ab-122">Now that you completed this quick start tutorial, move on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="962ab-123">[Lägg till Offline Data](app-service-mobile-cordova-get-started-offline-data.md) tooyour Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="962ab-123">[Add Offline Data](app-service-mobile-cordova-get-started-offline-data.md) tooyour Apache Cordova app.</span></span>
* <span data-ttu-id="962ab-124">[Lägg till autentisering](app-service-mobile-cordova-get-started-users.md) tooyour Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="962ab-124">[Add Authentication](app-service-mobile-cordova-get-started-users.md) tooyour Apache Cordova app.</span></span>
* <span data-ttu-id="962ab-125">[Lägg till Push-meddelanden](app-service-mobile-cordova-get-started-push.md) tooyour Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="962ab-125">[Add Push Notifications](app-service-mobile-cordova-get-started-push.md) tooyour Apache Cordova app.</span></span>

<span data-ttu-id="962ab-126">Läs mer om nyckelbegrepp i Azure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="962ab-126">Learn more about key concepts with Azure App Service.</span></span>

* <span data-ttu-id="962ab-127">[Offlinedata]</span><span class="sxs-lookup"><span data-stu-id="962ab-127">[Offline Data]</span></span>
* <span data-ttu-id="962ab-128">[Autentisering]</span><span class="sxs-lookup"><span data-stu-id="962ab-128">[Authentication]</span></span>
* <span data-ttu-id="962ab-129">[Push-meddelanden]</span><span class="sxs-lookup"><span data-stu-id="962ab-129">[Push Notifications]</span></span>

<span data-ttu-id="962ab-130">Lär dig hur toouse hello SDK: er.</span><span class="sxs-lookup"><span data-stu-id="962ab-130">Learn how toouse hello SDKs.</span></span>

* <span data-ttu-id="962ab-131">[Apache Cordova-SDK]</span><span class="sxs-lookup"><span data-stu-id="962ab-131">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="962ab-132">[ASP.NET Server-SDK]</span><span class="sxs-lookup"><span data-stu-id="962ab-132">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="962ab-133">[Node.js Server-SDK]</span><span class="sxs-lookup"><span data-stu-id="962ab-133">[Node.js Server SDK]</span></span>

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
