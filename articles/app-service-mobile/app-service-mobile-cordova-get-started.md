---
title: "Skapa en Cordova-app i Azure Apptjänst Mobile Apps | Microsoft Docs"
description: "Följ den här kursen och kom igång med att använda mobilappserverdelar i Azure för Apache Cordova-utveckling"
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
ms.openlocfilehash: b620465cdc3cfa04933dc6e70163fc32aa9a839b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-cordova-app"></a><span data-ttu-id="b31a9-104">Skapa en Apache Cordova-app</span><span class="sxs-lookup"><span data-stu-id="b31a9-104">Create an Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="b31a9-105">Översikt
</span><span class="sxs-lookup"><span data-stu-id="b31a9-105">Overview</span></span>
<span data-ttu-id="b31a9-106">I den här kursen får du lära dig hur du lägger till en molnbaserad serverdelstjänst i en Apache Cordova-mobilapp med hjälp av en mobilappserverdel i Azure.</span><span class="sxs-lookup"><span data-stu-id="b31a9-106">This tutorial shows you how to add a cloud-based backend service to an Apache Cordova mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="b31a9-107">Du skapar både en ny serverdel för mobilappen och en enkel Apache Cordova-app med en *att göra-lista* som lagrar appdata i Azure.</span><span class="sxs-lookup"><span data-stu-id="b31a9-107">You create both a new mobile app backend and a simple *Todo list* Apache Cordova app that stores app data in Azure.</span></span>

<span data-ttu-id="b31a9-108">Du måste slutföra den här kursen innan du påbörjar någon annan Apache Cordova-kurs om att använda funktionen Mobile Apps i Azure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="b31a9-108">Completing this tutorial is a prerequisite for all other Apache Cordova tutorials about using the Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b31a9-109">Krav</span><span class="sxs-lookup"><span data-stu-id="b31a9-109">Prerequisites</span></span>
<span data-ttu-id="b31a9-110">För att slutföra den här självstudien, finns följande förhandskrav:</span><span class="sxs-lookup"><span data-stu-id="b31a9-110">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="b31a9-111">En PC-dator med [Visual Studio Community 2017] eller senare.</span><span class="sxs-lookup"><span data-stu-id="b31a9-111">A PC with [Visual Studio Community 2017] or newer.</span></span>
* <span data-ttu-id="b31a9-112">[Visual Studio Tools för Apache Cordova].</span><span class="sxs-lookup"><span data-stu-id="b31a9-112">[Visual Studio Tools for Apache Cordova].</span></span>
* <span data-ttu-id="b31a9-113">Ett [aktivt Azure-konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b31a9-113">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

<span data-ttu-id="b31a9-114">Du kan också kringgå Visual Studio och direkt använda kommandoraden i Apache Cordova.</span><span class="sxs-lookup"><span data-stu-id="b31a9-114">You may also bypass Visual Studio and use the Apache Cordova command line directly.</span></span>  <span data-ttu-id="b31a9-115">Det är praktiskt att använda kommandoraden om du går självstudiekursen på en Mac-dator.</span><span class="sxs-lookup"><span data-stu-id="b31a9-115">Using the command line is useful when completing the tutorial on a Mac computer.</span></span>  <span data-ttu-id="b31a9-116">I den här kursen ingår inte att kompilera klientprogram i Apache Cordova med hjälp av kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="b31a9-116">Compiling Apache Cordova client applications using the command line is not covered by this tutorial.</span></span>

## <a name="create-an-azure-mobile-app-backend"></a><span data-ttu-id="b31a9-117">Skapa serverdelen för en Azure-mobilapp</span><span class="sxs-lookup"><span data-stu-id="b31a9-117">Create an Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[<span data-ttu-id="b31a9-118">Titta på en video som visar liknande steg</span><span class="sxs-lookup"><span data-stu-id="b31a9-118">Watch a video showing similar steps</span></span>](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a><span data-ttu-id="b31a9-119">Konfigurera serverprojektet</span><span class="sxs-lookup"><span data-stu-id="b31a9-119">Configure the server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a><span data-ttu-id="b31a9-120">Hämta och köra Apache Cordova-appen</span><span class="sxs-lookup"><span data-stu-id="b31a9-120">Download and run the Apache Cordova app</span></span>
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a><span data-ttu-id="b31a9-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b31a9-121">Next Steps</span></span>
<span data-ttu-id="b31a9-122">Nu när du har slutfört den här snabbstartskursen går du vidare till en av följande kurser:</span><span class="sxs-lookup"><span data-stu-id="b31a9-122">Now that you completed this quick start tutorial, move on to one of the following tutorials:</span></span>

* <span data-ttu-id="b31a9-123">[Add Offline Data](app-service-mobile-cordova-get-started-offline-data.md) (Lägga till offlinedata) i en Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="b31a9-123">[Add Offline Data](app-service-mobile-cordova-get-started-offline-data.md) to your Apache Cordova app.</span></span>
* <span data-ttu-id="b31a9-124">[Lägg till autentisering](app-service-mobile-cordova-get-started-users.md) i din Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="b31a9-124">[Add Authentication](app-service-mobile-cordova-get-started-users.md) to your Apache Cordova app.</span></span>
* <span data-ttu-id="b31a9-125">[Lägg till push-meddelanden](app-service-mobile-cordova-get-started-push.md) i din Apache Cordova-app.</span><span class="sxs-lookup"><span data-stu-id="b31a9-125">[Add Push Notifications](app-service-mobile-cordova-get-started-push.md) to your Apache Cordova app.</span></span>

<span data-ttu-id="b31a9-126">Läs mer om nyckelbegrepp i Azure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="b31a9-126">Learn more about key concepts with Azure App Service.</span></span>

* <span data-ttu-id="b31a9-127">[Offlinedata]</span><span class="sxs-lookup"><span data-stu-id="b31a9-127">[Offline Data]</span></span>
* <span data-ttu-id="b31a9-128">[Autentisering]</span><span class="sxs-lookup"><span data-stu-id="b31a9-128">[Authentication]</span></span>
* <span data-ttu-id="b31a9-129">[Push-meddelanden]</span><span class="sxs-lookup"><span data-stu-id="b31a9-129">[Push Notifications]</span></span>

<span data-ttu-id="b31a9-130">Lär dig hur du använder SDK: er.</span><span class="sxs-lookup"><span data-stu-id="b31a9-130">Learn how to use the SDKs.</span></span>

* <span data-ttu-id="b31a9-131">[Apache Cordova-SDK]</span><span class="sxs-lookup"><span data-stu-id="b31a9-131">[Apache Cordova SDK]</span></span>
* <span data-ttu-id="b31a9-132">[ASP.NET Server-SDK]</span><span class="sxs-lookup"><span data-stu-id="b31a9-132">[ASP.NET Server SDK]</span></span>
* <span data-ttu-id="b31a9-133">[Node.js Server-SDK]</span><span class="sxs-lookup"><span data-stu-id="b31a9-133">[Node.js Server SDK]</span></span>

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
<span data-ttu-id="b31a9-134">[Visual Studio Community 2017]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="b31a9-134">[Visual Studio Community 2017]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="b31a9-135">[Visual Studio Tools för Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span><span class="sxs-lookup"><span data-stu-id="b31a9-135">[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx</span></span>
<span data-ttu-id="b31a9-136">[Offlinedata]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="b31a9-136">[Offline Data]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="b31a9-137">[Autentisering]: app-service-mobile-auth.md</span><span class="sxs-lookup"><span data-stu-id="b31a9-137">[Authentication]: app-service-mobile-auth.md</span></span>
<span data-ttu-id="b31a9-138">[Push-meddelanden]: ../notification-hubs/notification-hubs-push-notification-overview.md</span><span class="sxs-lookup"><span data-stu-id="b31a9-138">[Push Notifications]: ../notification-hubs/notification-hubs-push-notification-overview.md</span></span>
<span data-ttu-id="b31a9-139">[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span><span class="sxs-lookup"><span data-stu-id="b31a9-139">[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md</span></span>
<span data-ttu-id="b31a9-140">[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="b31a9-140">[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md</span></span>
<span data-ttu-id="b31a9-141">[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span><span class="sxs-lookup"><span data-stu-id="b31a9-141">[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md</span></span>
