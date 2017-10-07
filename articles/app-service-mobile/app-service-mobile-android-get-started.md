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
# <a name="create-an-android-app"></a>Skapa en Android-app
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooan Android-mobilapp med hjälp av en mobilappsserverdel i Azure.  Du skapar både en ny mobilappsserverdel och en enkel Android-app med en *att göra-lista* som lagrar appdata i Azure.

Den här kursen är en förutsättning för alla andra Android självstudier om hur du använder funktionen för hello Mobile Apps i Azure App Service.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande:

* [Android Developer Tools](https://developer.android.com/sdk/index.html), som innehåller hello Android Studio integrated development environment och hello senaste Android-plattformen.
* Azure Mobile Android SDK, som refereras automatiskt som en del av hello snabbstartsprojekt du hämtar.
* Ett [aktivt Azure-konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Skapa en ny mobilappsserverdel i Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a>Konfigurera hello serverprojekt
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-android-app"></a>Hämta och köra hello Android-app
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
