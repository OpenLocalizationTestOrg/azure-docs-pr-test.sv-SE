---
title: 'MyDriving Azure IoT-exempel: Snabbstart | Microsoft Docs'
description: "Kom igång med en app som är en omfattande demonstration av hur du kan skapa en IoT-system med hjälp av Microsoft Azure, inklusive Stream Analytics, Machine Learning och Händelsehubbar."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: f40ea71b-5721-4a6b-a886-53c2e9dffe8f
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 03/25/2016
ms.author: harikm
ms.openlocfilehash: 031b492df1f186087e7b91102cbb44f552999293
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT system: Snabbstart
MyDriving är ett system som visar designen och implementeringen av en typisk [Sakernas Internet](iot-suite-overview.md) (IoT)-lösning som samlar in telemetri från enheter, bearbetar data i molnet och använder maskininlärning att ange en anpassningsbar svar. Demonstrationen av loggar data om din bil resor, med hjälp av data från både din mobiltelefon och ett kort som samlar in information från en bil kontrollsystem. Dessa data används för att ge feedback om formatmallen intresseväckande jämförelse med andra användare.

Det verkliga syftet med MyDriving är att komma igång med att skapa en egen IoT-lösning. Men innan som ska få igång med MyDriving själva appen--som medlem i vår test användare team. Detta ger dig en upplevelse av appen och systemet bakom det som en konsument innan du detaljerad information om arkitekturen. Den också ger en introduktion till HockeyApp, ett kall sätt att hantera dina appar alpha och beta-distributioner att testa användare.

## <a name="use-the-mobile-experience"></a>Använd mobila upplevelsen
Du kan använda appen MyDriving om du har en Android, iOS eller Windows 10-enhet.

### <a name="android-and-windows-10-mobile-installation"></a>Android och Windows 10 Mobile-installation
På din enhet:

1. Tillåt att utveckla appar:
   
   * Android: I **inställningar** > **säkerhet**, Tillåt appar från **okända källor**.
   * Windows 10: I **inställningar** > **uppdateringar** > **för utvecklare**, ange **utvecklarläge**.
2. Ansluta till vår beta testgruppen genom att registrera sig med eller logga in på, [HockeyApp](https://rink.hockeyapp.net). HockeyApp gör det enkelt att distribuera tidigare versioner av appen för att testa användare.
   
   Om du använder Windows 10 kan du använda Edge-webbläsaren.
   
   Om du har en Build 2016 deltagare, logga in med samma Microsoft-konto e-postmeddelandet du registrerat för konferens, genom att använda ett Microsoft-knappar. Du har redan loggat med HockeyApp.
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
3. Hämta och installera appen från den här:
   
   * [Android](http://rink.io/spMyDrivingAndroid)
   * [Windows 10](http://rink.io/spMyDrivingUWP)
   
   Det finns två objekt. Installera certifikatet i **betrodda personer**. Installera appen.

*Eventuella problem som startar appen på Windows 10 Mobile?* Din telefon kan vara en uppdatering eller två bakom. Kontrollera att du har de senaste uppdateringarna eller installera:

* [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a>iOS-installation
Om du var Build 2016 hämta appen som en medlem i vår test-teamet på HockeyApp:

1. Logga in på din iOS-enhet till [HockeyApp](https://rink.hockeyapp.net).
   Använd någon av Microsoft-inloggning knapparna och logga in med samma Microsoft-konto e-postmeddelandet som du har registrerat med konferens. (Använd inte fälten e-post och lösenord.)
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
2. Välj MyDriving på HockeyApp-instrumentpanel och ladda ned den.
3. Auktorisera betaversion från HockeyApp:
   
   a. Gå till **inställningar** > **allmänna** > **profiler och hantering av enheter.**
   
   b. Förtroende för den **bitars Stadium GmbH** certifikat.

Om du inte tar tag Build 2016, kan du skapa och distribuera appen själv:

1. Hämta koden [från GitHub].
2. Skapa och distribuera med [med Xamarin].

Mer information i den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Hämta ett OBD-kort (valfritt)
Detta är den del som är detta en verklig Sakernas Internet system! Du kan använda appen utan en, men det är roligt med verkligt och de är inte dyrt.

Inbyggd diagnostik (OBD) är en funktion av bilen garaget använder för att finjustera bilen och diagnostisera udda brus och varning ljus. Om inte bilen är för stor antiquity, hittar du en socket någonstans i utrustning vanligtvis bakom en flik under instrumentpanelen. Med rätt-anslutningen kan du hämta mätvärden för motorns prestanda och gör vissa ändringar. En OBD-koppling kan köpas billigt på vanligt sätt. Ansluter med hjälp av Bluetooth- eller Wi-Fi till en app på din telefon.

I det här fallet är det dags att ansluta bilen till molnet. Direktanslutning från OBD är att din telefon, men vår app fungerar som ett relä. En bil telemetri skickas direkt till MyDriving IoT-hubb där den bearbetas logga körda mil och utvärdera din intresseväckande stil.

Att ansluta en OBD-enhet:

1. Kontrollera att bilen har en OBD-socket.
2. Hämta ett OBD-nätverkskort:
   
   * Om du använder en Android eller Windows phone måste ett Bluetooth-aktiverade OBD II-kort. Vi använde [BAFX produkter 34t5 Bluetooth OBDII Scan Tool].
   * Om du använder en iOS-telefoner, måste ett Wi-Fi-aktiverade OBD-kort. Vi använde [Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner].
3. Följ instruktionerna som medföljer OBD kortet att ansluta till din telefon. Tänk på följande:
   
   * En Bluetooth-adapter måste kombineras med telefonen och på den **inställningar** sidan.
   * En Wi-Fi-nätverkskortet ha en adress i intervallet 192.168.xxx.xxx.
4. Om du har flera bilar kan hämta du ett separat nätverkskort för varje (högst tre).

Om du inte har ett kort OBD-appen kommer fortfarande att skicka data för platsen och hastighet från telefonens GPS-mottagare till serverdelen och ber om du vill simulera en OBD.

Du hittar mer information om hur appen använder data från OBD-kort och alternativ för att skapa OBD-enheten i 2.1, ”IoT-enheter” i den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Använd appen
Starta appen. Det finns en inledande Snabbstart som leder dig igenom hur det fungerar.

### <a name="track-your-trips"></a>Spåra dina resor
Knacka på posten (stort röd cirkel längst ned på skärmen) för att starta en resa och tryck på igen om du vill avsluta.

![Bild av knappen post för resa spårning](./media/iot-solution-get-started/image2.png)

Varje gång du startar en resa, om det finns ingen OBD-enhet, får du en fråga om du vill använda simulatorn.

Klicka på Stopp i slutet av en resa och du får en översikt.

![Exempel på en sammanfattande resa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Granska dina resor
![Exempel på en tidigare resa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Granska din profil
![Exempel på en profil för körning-format](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Skicka test-feedback
Eftersom vi skapade MyDriving att rivstart IoT systemen vill vi verkligen veta hur det fungerar. Berätta för oss om:

* Du stöter på problem eller utmaningar.
* Det finns en plats för tillägg som gör det passar bättre för ditt scenario.
* Du hittar ett mer effektivt sätt att utföra vissa behov.
* Du har några förslag för att förbättra MyDriving eller den här dokumentationen.

Inom MyDriving själva appen, du kan använda den inbyggda HockeyApp feedback mekanismen: på iOS och Android bara ge din telefon en shake eller använda den **Feedback** kommando. Detta ska koppla en skärmbild automatiskt så att vi vet vad du pratar om. Och om det finns några olycklig krascher, HockeyApp samlar in krascher loggarna om du vill berätta om dem. Du kan också ge feedback via den [HockeyApp-portalen].

Du kan också lagra en [problemet på GitHub], eller lämna en kommentar nedan (SV-oss edition).

Vi hoppas att få höra från dig!

## <a name="next-steps"></a>Nästa steg
* Utforska den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) att förstå hur vi har utformat och skapat MyDriving hela systemet.
* [Skapa och distribuera ett system med din egen](iot-solution-build-system.md) med hjälp av vår Azure Resource Manager-skript. Den [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) hjälper dig också att områden där du ska göra de anpassningar.

[från GitHub]: https://github.com/Azure-Samples/MyDriving
[med Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX produkter 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS
[Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[HockeyApp-portalen]: https://rink.hockeyapp.org
[problemet på GitHub]: https://github.com/Azure-Samples/MyDriving/issues
