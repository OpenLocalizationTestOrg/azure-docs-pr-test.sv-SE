---
title: "aaaGet igång med Mobile Apps med hjälp av Xamarin.Forms"
description: "Följ den här självstudiekursen toostart med Mobilappar för Xamarin.Forms-utveckling"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: af6b1c1ce4cf91c397552aa3d8ee40728129238c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinforms-app"></a>Skapa en Xamarin.Forms-app
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur tooadd en molnbaserad serverdelstjänst tooa Xamarin.Forms-mobilapp med hjälp av hello Mobile Apps-funktionen i Azure App Service som hello serverdel. Du skapar både en ny Mobile Apps-serverdel och en enkel ”todo list”-app med Xamarin.Forms där appdata lagras i Azure.

Du måste slutföra den här kursen innan du börjar någon annan kurs om Mobilappar för Xamarin.Forms-appar.

## <a name="prerequisites"></a>Krav
toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. Om du inte har ett konto kan du registrera dig för en utvärderingsversion av Azure och få upp too10 mobilappar utan kostnad som du kan fortsätta att använda även efter utvärderingsperiodens slut. Mer information finns i [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio med Xamarin. Mer information finns i hello [ställa upp och installera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) sidan.

* En Mac med Xcode v7.0 eller senare och Xamarin Studio Community installerat. Information finns på sidan för [installation och konfiguration av Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) och på sidan för [konfiguration, installation och verifiering för Mac-användare](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-mobile-apps-back-end"></a>Skapa en ny Mobile Apps-serverdel

toocreate en ny Mobile Apps tillbaka sluta hello följande:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Nu har du konfigurerat en Mobile Apps-serverdel som du kan använda för dina mobilklientprogram. Därefter ladda ned ett serverprojekt för en serverdel för enkel att göra-lista och sedan publicera tooAzure.

## <a name="configure-hello-server-project"></a>Konfigurera hello serverprojekt

tooconfigure hello server projektet toouse Hej Node.js eller .NET-serverdel, hello följande:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinforms-solution"></a>Hämta och köra hello Xamarin.Forms-lösningen

Du kan hämta hello lösning på två sätt. Hämta den tooa Mac och öppna det i Xamarin Studio eller hämta den tooa Windows-dator och öppna den i Visual Studio med hjälp av en nätverksansluten Mac för att skapa hello iOS-app. Mer information finns på sidan för att [installera och konfigurera Visual Studio och Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

På en Mac- eller Windows-dator, hello följande:

1. Gå toohello [Azure-portalen].

2. På hello **inställningar** bladet för din mobila app under **Mobile**väljer **Kom igång** > **Xamarin.Forms**. Välj **Skapa en ny app** under **steg 3** och välj sedan **Ladda ned**.

   Den här åtgärden hämtar ett projekt som innehåller ett klientprogram som är anslutna tooyour mobila appar. Spara hello komprimerade projektet filen tooyour lokala datorn och notera var du sparar den.

3. Extrahera hello-projekt som du laddade ned och öppna det i Xamarin Studio (Mac) eller Visual Studio (Windows).

   ![Extraherat projekt i Xamarin Studio][9]

   ![Extraherat projekt i Visual Studio][8]

## <a name="optional-run-hello-ios-project"></a>(Valfritt) Kör hello iOS-projekt
I det här avsnittet kan du köra hello Xamarin iOS-projektet för iOS-enheter. Du kan hoppa över det här avsnittet om du inte arbetar med iOS-enheter.

#### <a name="in-xamarin-studio"></a>I Xamarin Studio
1. Högerklicka på hello iOS-projektet och välj sedan **Set As Startup Project**.

2. På hello **kör** väljer du **Start Debugging** toobuild hello projektet och starta hello app i hello iPhone-emulatorn.

#### <a name="in-visual-studio"></a>I Visual Studio 2013
1. Högerklicka på hello iOS-projektet och välj sedan **Set as StartUp Project**.

2. På hello **skapa** väljer du **Configuration Manager**.

3. I hello **Configuration Manager** dialogrutan, Välj hello **skapa** och **distribuera** kryssrutorna nästa toohello iOS-projekt.

4. toobuild hello projektet och starta hello app i hello iPhone-emulatorn väljer hello **F5** nyckel.

   > [!NOTE]
   > Om du har problem i bygget hello projektet kör hello NuGet package manager och uppdaterar toohello senaste versionen av hello Xamarin paket. Snabbstartsprojekt kan vara långsam tooupdate toohello senaste versionerna.    
   >
   >

5. I hello app, ange en beskrivande text som *Läs om Xamarin*, och sedan väljer hello plustecken (**+**).

    ![][10]

    Den här åtgärden skickar en post-begäran toohello nya Mobile Apps serverdel som finns i Azure. Data från hello begäran infogas i hello TodoItem-tabell. Objekt som lagras i hello tabellen returneras av hello Mobile Apps tillbaka slut och hello data visas i hello lista.

    > [!NOTE]
    > Du hittar hello koden som ansluter till din Mobile Apps-serverdel i hello TodoItemManager.cs C#-filen hello portabla klassbiblioteksprojektet i lösningen.
    >
    >

## <a name="optional-run-hello-android-project"></a>(Valfritt) Kör hello Android-projekt
I det här avsnittet kan köra du hello Xamarin droid-projektet för Android. Du kan hoppa över det här avsnittet om du inte arbetar med Androidenheter.

#### <a name="in-xamarin-studio"></a>I Xamarin Studio

1. Högerklicka på hello Android-projekt och välj sedan **Set As Startup Project**.

2. toobuild hello projektet och starta hello app i en androidemulator på hello **kör** väljer du **Start Debugging**.

#### <a name="in-visual-studio"></a>I Visual Studio 2013

1. Högerklicka på hello Android (Droid)-projektet och välj sedan **Set as StartUp Project**.

2. På hello **skapa** väljer du **Configuration Manager**.

3. I hello **Configuration Manager** dialogrutan, Välj hello **skapa** och **distribuera** kryssrutorna nästa toohello Android-projekt.

4. toobuild hello projektet och starta hello app i en androidemulator väljer hello **F5** nyckel.

   > [!NOTE]
   > Om du har problem i bygget hello projektet kör hello NuGet package manager och uppdaterar toohello senaste versionen av hello Xamarin paket. Snabbstartsprojekt kan vara långsam tooupdate toohello senaste versionerna.    
   >
   >

5. I hello app, ange en beskrivande text som *Läs om Xamarin*, och sedan väljer hello plustecken (**+**).

    ![][11]
    
    Den här åtgärden skickar en post-begäran toohello nya Mobile Apps serverdel som finns i Azure. Data från hello begäran infogas i hello TodoItem-tabell. Objekt som lagras i hello tabellen returneras av hello Mobile Apps tillbaka slut och hello data visas i hello lista.
    
    > [!NOTE]
    > Du hittar hello koden som ansluter till din Mobile Apps-serverdel i hello TodoItemManager.cs C#-filen hello portabla klassbiblioteksprojektet i lösningen.
    >
    >

## <a name="optional-run-hello-windows-project"></a>(Valfritt) Kör Windows hello-projekt

I det här avsnittet kan du köra hello Xamarin WinApp-projektet för Windows-enheter. Du kan hoppa över det här avsnittet om du inte arbetar med Windowsenheter.

#### <a name="in-visual-studio"></a>I Visual Studio 2013

1. Högerklicka på något av windowsprojekten hello och välj sedan **Set as StartUp Project**.

2. På hello **skapa** väljer du **Configuration Manager**.

3. I hello **Configuration Manager** dialogrutan, Välj hello **skapa** och **distribuera** kryssrutorna nästa toohello Windows-projekt som du har valt.

4. toobuild hello projektet och starta hello app i en Windows-emulator, Välj hello **F5** nyckel.

   > [!NOTE]
   > Om du har problem i bygget hello projektet kör hello NuGet package manager och uppdaterar toohello senaste versionen av hello Xamarin paket. Snabbstartsprojekt kan vara långsam tooupdate toohello senaste versionerna.    
   >
   >

5. I hello app, ange en beskrivande text som *Läs om Xamarin*, och sedan väljer hello plustecken (**+**).

    Den här åtgärden skickar en post-begäran toohello nya Mobile Apps serverdel som finns i Azure. Data från hello begäran infogas i hello TodoItem-tabell. Objekt som lagras i hello tabellen returneras av hello Mobile Apps tillbaka slut och hello data visas i hello lista.
    
    ![][12]
    
    > [!NOTE]
    > Du hittar hello koden som ansluter till din Mobile Apps-serverdel i hello TodoItemManager.cs C#-filen hello portabla klassbiblioteksprojektet i lösningen.
    >
    >

## <a name="next-steps"></a>Nästa steg

* [Lägg till autentisering tooyour app](app-service-mobile-xamarin-forms-get-started-users.md)  
  Lär dig hur tooauthenticate användare i appen med en identitetsprovider.

* [Lägg till push-meddelanden tooyour app](app-service-mobile-xamarin-forms-get-started-push.md)  
  Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobile Apps serverdel toouse Azure Notification Hubs toosend hello push-meddelanden.

* [Aktivera offlinesynkronisering av appen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Lär dig hur tooadd offline stöd för din app genom att använda en Mobile Apps-serverdel. Med offlinesynkronisering kan du visa, lägga till eller ändra mobilappens data, även när det inte finns någon nätverksanslutning.

* [Använd hello hanterad klient för Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Lär dig hur toowork med hello hanterade klient-SDK i Xamarin-app.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-portalen]: https://portal.azure.com/
