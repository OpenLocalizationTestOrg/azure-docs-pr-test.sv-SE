---
title: "aaaGet igång med Azure Mobilappar för Xamarin.Android-appar"
description: "Följ den här självstudiekursen tooget igång med Azure Mobilappar för Xamarin Android-utveckling"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 38710660d9328fe3c068efca972f76aa8b6e049b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinandroid-app"></a>Skapa en Xamarin.Android-app
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooa Xamarin.Android-app. Mer information om Mobile Apps finns [här](app-service-mobile-value-prop.md).

En skärmbild från hello slutförts app understiger:

![][0]

Du måste slutföra den här kursen innan du börjar någon annan kurs om Mobile Apps för Xamarin.Android-appar.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande krav:

* Ett aktivt Azure-konto. Om du inte har ett konto kan registrera dig för en utvärderingsversion av Azure och få upp too10 ledigt Mobile Apps. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio med Xamarin. Instruktioner finns i avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

## <a name="create-an-azure-mobile-app-backend"></a>Skapa en mobilapp-serverdel i Azure
Följ dessa steg toocreate en mobilappsserverdel.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Du har nu skapat en mobilsappsserverdel i Azure som kan användas av dina mobilklientprogram. Därefter ladda ned ett serverprojekt för en enkel todo-list ”serverdel och publicera den tooAzure.

## <a name="configure-hello-server-project"></a>Konfigurera hello serverprojekt
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinandroid-app"></a>Hämta och köra hello Xamarin.Android-app
1. Under **ladda ned och kör ditt Xamarin.Android-projekt**, klicka på hello **hämta** knappen.

      Spara hello komprimerade projektet filen tooyour lokala datorn och notera var du sparar den.
2. Tryck på hello **F5** nyckeln toobuild hello projektet och starta hello app.
3. I hello app, ange en beskrivande text som *fullständig hello kursen* och klicka sedan på hello **Lägg till** knappen.

    ![][10]

    Data från hello begäran infogas i hello TodoItem-tabell. Objekt som lagras i hello tabellen returneras av hello mobilappsserverdel och informationen visas i hello lista.

   > [!NOTE]
   > Du kan granska hello koden som ansluter till din mobila app backend tooquery och infoga data som finns i hello C#-filen ToDoActivity.cs.
   >
   >

## <a name="next-steps"></a>Nästa steg
* [Lägg till Offline Sync tooyour app](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Lägg till autentisering tooyour app](app-service-mobile-xamarin-android-get-started-users.md)
* [Lägg till push-meddelanden tooyour Xamarin.Android-app](app-service-mobile-xamarin-android-get-started-push.md)
* [Hur toouse hello hanterade klienten för Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
