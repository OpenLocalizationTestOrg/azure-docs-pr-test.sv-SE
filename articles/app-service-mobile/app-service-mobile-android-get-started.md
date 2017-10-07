---
title: "aaaCreate en Android-app på Azure Apptjänst Mobilappar | Microsoft Docs"
description: "Följ den här självstudiekursen tooget igång med att använda mobilappserverdelar för Android-utveckling"
services: app-service\mobile
documentationcenter: android
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 0af85a3a4de9fc265976bbe3f34d73effc3807df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-android-app"></a><span data-ttu-id="2f3e2-103">Skapa en Android-app</span><span class="sxs-lookup"><span data-stu-id="2f3e2-103">Create an Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="2f3e2-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2f3e2-104">Overview</span></span>
<span data-ttu-id="2f3e2-105">Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooan Android-mobilapp med hjälp av en mobilappsserverdel i Azure.</span><span class="sxs-lookup"><span data-stu-id="2f3e2-105">This tutorial shows you how tooadd a cloud-based backend service tooan Android mobile app by using an Azure mobile app backend.</span></span>  <span data-ttu-id="2f3e2-106">Du skapar både en ny mobilappsserverdel och en enkel Android-app med en *att göra-lista* som lagrar appdata i Azure.</span><span class="sxs-lookup"><span data-stu-id="2f3e2-106">You will create both a new mobile app backend and a simple *Todo list* Android app that stores app data in Azure.</span></span>

<span data-ttu-id="2f3e2-107">Den här kursen är en förutsättning för alla andra Android självstudier om hur du använder funktionen för hello Mobile Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="2f3e2-107">Completing this tutorial is a prerequisite for all other Android tutorials about using hello Mobile Apps feature in Azure App Service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f3e2-108">Krav</span><span class="sxs-lookup"><span data-stu-id="2f3e2-108">Prerequisites</span></span>
<span data-ttu-id="2f3e2-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="2f3e2-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="2f3e2-110">[Android Developer Tools](https://developer.android.com/sdk/index.html), som innehåller hello Android Studio integrated development environment och hello senaste Android-plattformen.</span><span class="sxs-lookup"><span data-stu-id="2f3e2-110">[Android Developer Tools](https://developer.android.com/sdk/index.html), which includes hello Android Studio integrated development environment, and hello latest Android platform.</span></span>
* <span data-ttu-id="2f3e2-111">Azure Mobile Android SDK, som refereras automatiskt som en del av hello snabbstartsprojekt du hämtar.</span><span class="sxs-lookup"><span data-stu-id="2f3e2-111">Azure Mobile Android SDK, which is automatically referenced as part of hello quickstart project you download.</span></span>
* <span data-ttu-id="2f3e2-112">Ett [aktivt Azure-konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2f3e2-112">An [active Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="create-a-new-azure-mobile-app-backend"></a><span data-ttu-id="2f3e2-113">Skapa en ny mobilappsserverdel i Azure</span><span class="sxs-lookup"><span data-stu-id="2f3e2-113">Create a new Azure mobile app backend</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a><span data-ttu-id="2f3e2-114">Konfigurera hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="2f3e2-114">Configure hello server project</span></span>
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-android-app"></a><span data-ttu-id="2f3e2-115">Hämta och köra hello Android-app</span><span class="sxs-lookup"><span data-stu-id="2f3e2-115">Download and run hello Android app</span></span>
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
