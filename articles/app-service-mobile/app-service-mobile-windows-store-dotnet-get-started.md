---
title: "aaaCreate en universella Windowsplattformen (UWP) som använder på Mobile Apps | Microsoft Docs"
description: "Följ den här självstudiekursen tooget igång med att använda mobilappserverdelar för utveckling av universella Windowsplattformen (UWP) appar i C#, Visual Basic eller JavaScript."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: d0f57bca5a8195b8b0461b8f7a0d8516371d56a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-app"></a>Skapa en Windows-app
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooa universella Windowsplattformen (UWP) app. Mer information om Mobile Apps finns [här](app-service-mobile-value-prop.md). hello nedan visas skärmdumpar från hello slutförts app:

![Färdig skrivbordsapp](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Visad på ett skrivbord

![Färdig mobilapp](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Visad på en telefon

Du måste slutföra den här kursen innan du går någon annan kurs om Mobilappar för UWP-appar.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. Om du inte har ett konto kan du registrera dig för en utvärderingsversion av Azure och få upp too10 mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio Community 2015] eller senare version.

## <a name="create-a-new-azure-mobile-app-backend"></a>Skapa en ny mobilappsserverdel i Azure
Följ dessa steg toocreate en ny mobilappsserverdel.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Du har nu skapat en mobilsappsserverdel i Azure som kan användas av dina mobilklientprogram. Därefter måste du ladda ned ett serverprojekt för en enkel todo-list ”serverdel och publicera den tooAzure.

## <a name="configure-hello-server-project"></a>Konfigurera hello serverprojekt
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-client-project"></a>Hämta och kör klientprojektet hello
När du har konfigurerat serverdelen för Mobilappen kan du antingen skapa en ny klientapp eller ändra en befintlig app tooconnect tooAzure. I det här avsnittet kan du ladda ned en UWP-appsmall som är anpassade tooconnect tooyour mobilappsserverdel.

1. Tillbaka i hello **Snabbstart** klickar du på bladet för din mobilappsserverdel **skapar en ny app** > **hämta**, extrahera hello komprimerade projektfilerna tooyour lokal dator.

    ![Hämta ett snabbstartsprojekt för Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)
2. (Valfritt) Lägg till hello UWP-app-projekt toohello samma lösning som serverprojektet hello. Detta gör det enklare toodebug och testa både hello appen och hello backend i hello samma Visual Studio-lösning om du väljer toodo så. tooadd en UWP-app-projekt toohello lösning, måste du använda Visual Studio 2015 eller en senare version.
3. Hej UWP-appen som Startprojekt hello, tryck på hello F5 viktiga toodeploy och kör hello app.
4. I hello app, ange en beskrivande text som *fullständig hello kursen*, i hello **infoga en TodoItem** textrutan och klicka sedan på **spara**.

    ![Färdig Windows-snabbstartsapp på skrivbord](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Detta skickar en POST-begäran toohello ny mobilappsserverdel som finns i Azure.
5. (Valfritt) Stoppa hello appen och starta om den på en annan enhet eller mobilemulator.

    ![Färdig Windows-snabbstartsapp på telefon](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Observera att data som sparades hello föregående steg läses från Azure när hello UWP-appen startar.

## <a name="next-steps"></a>Nästa steg
* [Lägg till autentisering tooyour app](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Lär dig hur tooauthenticate användare i appen med en identitetsprovider.
* [Lägg till push-meddelanden tooyour app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobilapp backend toouse Azure Notification Hubs toosend push-meddelanden.
* [Aktivera offlinesynkronisering av appen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel. Offlinesynkronisering kan slutanvändarna toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
