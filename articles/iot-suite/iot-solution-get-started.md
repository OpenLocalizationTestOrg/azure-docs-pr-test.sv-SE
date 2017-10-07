---
title: 'MyDriving Azure IoT-exempel: Snabbstart | Microsoft Docs'
description: "Kom igång med en app som är en omfattande demonstration av hur tooarchitect en IoT-system med hjälp av Microsoft Azure, inklusive Stream Analytics, Machine Learning och Händelsehubbar."
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
ms.openlocfilehash: 411b9a992deb22b915f8291d8559e2917d976b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT system: Snabbstart
MyDriving är ett system som visar hello designen och implementeringen av en typisk [Sakernas Internet](iot-suite-overview.md) (IoT)-lösning som samlar in telemetri från enheter, bearbetar data i molnet hello och tillämpar maskininlärning tooprovide ett anpassningsbar svar. hello demonstration loggar data om din bil resor, med hjälp av data från både din mobiltelefon och ett kort som samlar in information från en bil kontrollsystem. Den använder denna data tooprovide feedback på formatmallen intresseväckande jämförelse tooother användare.

hello verkliga syftet med MyDriving är tooget som du startade i att skapa en egen IoT-lösning. Men innan som ska få igång med hello MyDriving själva appen--som medlem i vår test användare team. Detta ger dig en upplevelse av hello app och hello system bakom det som en konsument innan du ger dig i hello-arkitekturen. Introducerar även du tooHockeyApp kall sätt för att hantera hello alpha och beta distribution av appar tootest användarna.

## <a name="use-hello-mobile-experience"></a>Använd hello mobila upplevelsen
Du kan använda hello MyDriving app om du har en Android, iOS eller Windows 10-enhet.

### <a name="android-and-windows-10-mobile-installation"></a>Android och Windows 10 Mobile-installation
På din enhet:

1. Tillåt att utveckla appar:
   
   * Android: I **inställningar** > **säkerhet**, Tillåt appar från **okända källor**.
   * Windows 10: I **inställningar** > **uppdateringar** > **för utvecklare**, ange **utvecklarläge**.
2. Ansluta till vår beta testgruppen genom att registrera sig med eller logga in på, [HockeyApp](https://rink.hockeyapp.net). HockeyApp gör det enkelt toodistribute tidig versioner av app tootest användarna.
   
   Om du använder Windows 10, använda hello Edge-webbläsaren.
   
   Om du har en Build 2016 deltagare, logga in med hello samma Microsoft-konto e-post som har registrerats för hello konferens med någon av hello Microsoft knappar. Du har redan loggat med HockeyApp.
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
3. Hämta och installera appen hello härifrån:
   
   * [Android](http://rink.io/spMyDrivingAndroid)
   * [Windows 10](http://rink.io/spMyDrivingUWP)
   
   Det finns två objekt. Installera hello certifikat i **betrodda personer**. Installera sedan hello app.

*Eventuella problem som startar hello app på Windows 10 Mobile?* Din telefon kan vara en uppdatering eller två bakom. Kontrollera att du har fått hello senaste uppdateringar eller installera:

* [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a>iOS-installation
Om du var Build 2016 hämta hello appen som en medlem i vår test-teamet på HockeyApp:

1. På iOS-enheten, logga in för[HockeyApp](https://rink.hockeyapp.net).
   Använd en av hello Microsoft inloggning knappar och loggar in med hello samma Microsoft-konto e-post som du har registrerat med hello konferens. (Använd inte hello fälten för e-post och lösenord.)
   
   ![HockeyApp inloggningssida](./media/iot-solution-get-started/image1.png)
2. Välj MyDriving hello HockeyApp instrumentpanelen och ladda ned den.
3. Auktorisera hello betaversion från HockeyApp:
   
   a. Gå för**inställningar** > **allmänna** > **profiler och hantering av enheter.**
   
   b. Förtroende hello **bitars Stadium GmbH** certifikat.

Om du inte tar tag Build 2016, kan du skapa och distribuera hello app själv:

1. Hämta hello koden [från GitHub].
2. Skapa och distribuera med [med Xamarin].

Mer information finns i hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Hämta ett OBD-kort (valfritt)
Detta är en del av hello som gör det en verklig Sakernas Internet system! Du kan använda hello app utan ett, men det är roligt med hello verkligt och de är inte dyrt.

Inbyggd diagnostik (OBD) är hello funktion i bilen som hello garage använder tootune in bilen och diagnostisera udda brus och varning ljus. Om inte bilen är för stor antiquity, hittar du en socket någonstans i hello utrustning vanligtvis bakom en flik under hello instrumentpanelen. Med rätt hello-anslutningen kan du hämta mätvärden för hello motorns prestanda och gör vissa ändringar. En OBD-koppling kan köpas billigt från hello vanliga platser. Ansluter med hjälp av Bluetooth- eller Wi-Fi tooan app på din telefon.

I det här fallet vi tooconnect bilen toohello molnet. hello direkt anslutning från hello OBD är tooyour telefon, men vår app fungerar som ett relä. En bil telemetri skickas raka toohello MyDriving IoT-hubb, där det är bearbetas toolog körda mil och utvärdera din intresseväckande stil.

tooconnect en OBD-enhet:

1. Kontrollera att bilen har en OBD-socket.
2. Hämta ett OBD-nätverkskort:
   
   * Om du använder en Android eller Windows phone måste ett Bluetooth-aktiverade OBD II-kort. Vi använde [BAFX produkter 34t5 Bluetooth OBDII Scan Tool].
   * Om du använder en iOS-telefoner, måste ett Wi-Fi-aktiverade OBD-kort. Vi använde [Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner].
3. Följ instruktionerna för hello som medföljer din OBD-kort tooconnect den tooyour phone. Tänk på följande hello:
   
   * En Bluetooth-adapter måste kombineras med hello telefon på hello **inställningar** sidan.
   * Wi-Fi-kort måste ha en adress i hello intervallet 192.168.xxx.xxx.
4. Om du har flera bilar kan hämta du ett separat nätverkskort för varje (högst tre).

Om du inte har ett OBD-kort, hello appen fortfarande skickar plats och hastighet data från hello phone GPS-mottagare toohello tillbaka avslutas och ber om du vill toosimulate en OBD.

Du hittar mer information om hur hello appen använder data från hello OBD-kort och alternativ för att skapa OBD-enheten i 2.1, ”IoT-enheter” i hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs).

## <a name="use-hello-app"></a>Använda hello app
Starta hello app. Det finns en inledande Quickstart toowalk igenom hur det fungerar.

### <a name="track-your-trips"></a>Spåra dina resor
Knacka hello poster knappen (stort röd cirkel längst hello hello-skärmen) toostart resa och igen på tooend.

![Bild av hello post knapp för resa spårning](./media/iot-solution-get-started/image2.png)

Varje gång du startar en resa, om det finns ingen OBD-enhet, får du en fråga om du vill toouse hello simulatorn.

Hello slutet av en resa får tryck hello stopp-knappen, och du en sammanfattning.

![Exempel på en sammanfattande resa](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Granska dina resor
![Exempel på en tidigare resa](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Granska din profil
![Exempel på en profil för körning-format](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Skicka test-feedback
Eftersom vi skapade MyDriving toohelp rivstart IoT systemen vill vi verkligen toohear från dig om hur det fungerar. Berätta för oss om:

* Du stöter på problem eller utmaningar.
* Det finns en plats för tillägg som gör det lämpligare tooyour scenario.
* Du hittar ett mer effektivt sätt tooaccomplish vissa behov.
* Du har några förslag för att förbättra MyDriving eller den här dokumentationen.

Du kan använda hello inbyggd HockeyApp feedback mekanism i hello MyDriving app sig själv: på iOS och Android bara ge din telefon en shake eller använda hello **Feedback** kommando. Detta ska koppla en skärmbild automatiskt så att vi vet vad du pratar om. Och om det finns några olycklig krascher, HockeyApp samlar in hello krascher loggar tootell oss om dem. Du kan också ge feedback via hello [HockeyApp-portalen].

Du kan också lagra en [problemet på GitHub], eller lämna en kommentar nedan (SV-oss edition).

Vi ser fram emot toohearing från dig!

## <a name="next-steps"></a>Nästa steg
* Utforska hello [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) toounderstand hur vi har utformat och skapat hello hela MyDriving system.
* [Skapa och distribuera ett system med din egen](iot-solution-build-system.md) med hjälp av vår Azure Resource Manager-skript. Hej [MyDriving referenshandboken](http://aka.ms/mydrivingdocs) hjälper dig också att områden där du ska göra hello de flesta anpassningar.

[från GitHub]: https://github.com/Azure-Samples/MyDriving
[med Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[BAFX produkter 34t5 Bluetooth OBDII Scan Tool]: http://www.amazon.com/gp/product/B005NLQAHS
[Sökningsverktyget OBDLink MX Wi-Fi: OBD-kort/diagnostik skanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[HockeyApp-portalen]: https://rink.hockeyapp.org
[problemet på GitHub]: https://github.com/Azure-Samples/MyDriving/issues
