---
title: "aaaGet igång med Azure Apptjänst Mobilappar för Xamarin.iOS-appar | Microsoft Docs"
description: "Följ den här självstudiekursen tooget igång med Mobilappar för Xamarin.iOS-utveckling."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: syntaxc4
ms.openlocfilehash: 524c5ac4d8a29d7cb858f74132aad5d6e2201d02
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinios-app"></a>Skapa en Xamarin.iOS-app
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur tooadd en molnbaserad serverdel tjänsten tooa Xamarin.iOS-mobilapp med hjälp av en mobilappsserverdel i Azure.  Du skapar både en ny mobilapp-serverdel och en enkel *Att göra-lista* Xamarin.iOS-app som lagrar app-data i Azure.

Den här kursen är en förutsättning för alla andra Xamarin.iOS-kurs om hur du använder funktionen för hello Mobile Apps i Azure App Service.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande krav:

* Ett aktivt Azure-konto. Om du inte har ett konto kan registrera dig för en utvärderingsversion av Azure och få upp too10 mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio med Xamarin. Instruktioner finns i avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* En Mac med Xcode v7.0 eller senare och Xamarin Studio Community installerat. Se avsnittet om [konfiguration och installation av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) och om [konfiguration, installation och verifieringar för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-an-azure-mobile-app-backend"></a>Skapa en mobilapp-serverdel i Azure
Följ dessa steg toocreate en mobilappsserverdel.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-hello-server-project"></a>Konfigurera hello serverprojekt
Du har nu skapat en mobilsappsserverdel i Azure som kan användas av dina mobilklientprogram. Därefter ladda ned ett serverprojekt för en enkel todo-list ”serverdel och publicera den tooAzure.

Följ följande steg tooconfigure hello server projektet toouse hello antingen hello Node.js eller .NET-serverdel.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinios-app"></a>Hämta och köra hello Xamarin.iOS-app
1. Öppna hello [Azure-portalen] i ett webbläsarfönster.
2. Klicka på hello inställningsbladet för Mobilappen **Kom igång** > **Xamarin.iOS**. Vid steg 3, klickar du på **Skapa en ny app** om det inte redan är valt.  Klicka sedan på hello **hämta** knappen.

      Ett klientprogram som ansluter tooyour mobila serverdel hämtas. Spara hello komprimerade projektfilen lokalt på datorn och notera var du sparar den.
3. Extrahera hello-projekt som du laddade ned och öppna det i Xamarin Studio (eller Visual Studio).

    ![][9]

    ![][8]
4. Tryck på hello F5 viktiga toobuild hello projektet och starta hello app i hello iPhone-emulatorn.
5. I hello app, ange en beskrivande text som *Läs om Xamarin*, och klicka sedan på hello  **+**  knappen.

    ![][10]

    Data från hello begäran infogas i hello TodoItem-tabell. Objekt som lagras i hello tabellen returneras av hello mobilappsserverdel och data visas i hello-listan.

> [!NOTE]
> Du kan granska hello koden som ansluter till din mobila app backend tooquery och infoga data i hello C#-filen QSTodoService.cs.
>
>

## <a name="next-steps"></a>Nästa steg
* [Lägg till Offline Sync tooyour app](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Lägg till autentisering tooyour app](app-service-mobile-xamarin-ios-get-started-users.md)
* [Lägg till push-meddelanden tooyour Xamarin.Android-app](app-service-mobile-xamarin-ios-get-started-push.md)
* [Hur toouse hello hanterade klienten för Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-portalen]: https://portal.azure.com/
