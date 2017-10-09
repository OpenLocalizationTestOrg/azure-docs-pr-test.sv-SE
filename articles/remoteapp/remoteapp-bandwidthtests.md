---
title: "aaaAzure RemoteApp - testa din användandet av nätverkets bandbredd med några vanliga scenarier | Microsoft Docs"
description: "Lär dig mer om hur vanliga Användningsscenarier som kan hjälpa dig att ta reda på dina nätverksbandbredd behov för Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 06417c75-0c63-4ecf-b9d1-66a7af6b7b91
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 22c1dbb61d956d58d01eb4e11363266168e337e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - testa din användandet av nätverkets bandbredd med några vanliga scenarier
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Enligt beskrivningen i [uppskattning Azure RemoteApp nätverksbandbredd](remoteapp-bandwidth.md), hello bästa sätt toofigure reda på vilka hello effekten av Azure RemoteApp tooyour nätverk är toorun vissa tester för användning. Kör dessa tester för en uppsättning tid period och mäta hello bandbredden som behövs för varje scenario. Om du har hello-funktionen kan kan du mäta hello nätverket paket dataförlust och nätverk jitter toounderstand hello nätverket mönster som kommer att skapas i din miljö.

Kom ihåg att användningen varierar mellan olika användare inom ditt företag när du utvärderar hello bandbreddsanvändning. Till exempel använder text läsare och skrivare vanligtvis mindre bandbredd än användare som arbetar med video. För bästa resultat bör studera dina egna behov och skapa en blandning av hello följande scenarier som bäst representerar hello användare i företaget. Kom ihåg för[granska hello faktorer som påverkar användningen av nätverksbandbredd och användaren upplever](remoteapp-bandwidthexperience.md) -som hjälper dig att identifiera hello perfekt tester.

Först läsa om hello tester, Välj din blandning och kör dem sedan. Du kan använda hello tabellen nedan toohelp spåra prestanda.

> [!NOTE]
> Om du inte kan göra egen testning av nätverket eller behöver du inte hello tid toodo så, Kolla in våra [grundläggande bandbredd beräknar/nätverksrekommendationer](remoteapp-bandwidthguidelines.md). Din hittills utfört kan variera, men det om du *kan* köra dina egna tester bör du.
> 
> 

## <a name="hello-usage-tests"></a>hello användning tester
Var och en av dessa tester kör för olika mycket tid och testa olika funktioner och funktioner som använder nätverksbandbredden. Kom ihåg toochoose hello blandning av test som bäst passar din enskilda företagsanvändare.

### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Verkställande/komplexa PowerPoint - köra 900 1000 sekunder
En användare anger mellan 45 50 hög återgivning bilder med hjälp av Microsoft Office PowerPoint i helskärmsläge. hello bilder ska innehålla bilder, övergångar (med animering) och spelarbakgrunder med färgskalan som är vanliga för ditt företag. hello användaren bör ägna minst 20 sekunder på varje bild.

Det här scenariot skapar burst-trafik när en bild övergångar toohello nästa bild i hello presentation.

### <a name="simple-powerpoint---run-for-620-seconds"></a>Enkel PowerPoint - kör ~ 620 sekunder
En användare anger en enkel PowerPoint-fil med cirka 30 bilder med hjälp av Microsoft Office PowerPoint i helskärmsläge. hello bilder är mer text-intensiva än i hello verkställande/komplexa PowerPoint scenariot och har enklare bakgrund och bilder (svart diagram). 

### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - kör ~ 250 sekunder
En användare bläddrar hello webbprogram med hjälp av Internet Explorer. hello användaren bläddrar och bläddrar genom en blandning av text, fysiska bilder och vissa diagram. Hej webbsidor som lagras på hello lokala diskenheten på hello värd för fjärrskrivbordssession (RD Session Host)-server som en. MHT-fil. hello användaren rullar med olika intervall för varje nyckel/Bläddra PGUP, PGDN uppåt och nedåt nycklar:

    - Ned - 250 tangenttryckningar mycket 500 ms
    - Page Up - 36 tangenttryckningar varje 1 000 ms
    - Ned - 75 tangenttryckningar var 100 ms
    - Page Down - 20 tangenttryckningar varje 500 ms
    - Upp - 120 tangenttryckningar var 300 ms

### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-dokument - enkelt - kör ~ 610 sekunder
En användare läser och söker ett PDF-dokument på olika sätt med hjälp av Adobe Acrobat Reader. hello dokumentet består av tabeller, diagram enkla och flera teckensnitt. hello dokumentet är 35-40 sidor långa. hello användaren bläddrar genom bakåtkompatibilitet med två olika hastigheter och vidarebefordrar på fyra olika zoom-storlekar (passar toopage, anpassa toowidth 100% och en annan väljer). hello zoomning säkerställer att hello text (teckensnitt) återger olika storlekar. Rullning är avstängd med olika intervall för varje rulla hello PGUP, PGDN uppåt och nedåt nycklar.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF-dokument - blandat - körning för ~ 320 sekunder
En användare läser och söker ett PDF-dokument på olika sätt med hjälp av Adobe Acrobat Reader. hello dokumentet består av hög kvalitet avbildningar (inklusive fotografier), tabeller, enkla diagram och flera textteckensnitt. hello användaren bläddrar genom bakåtkompatibilitet med två olika hastigheter och vidarebefordrar på fyra olika zoom-storlekar (passar toopage, anpassa toowidth 100% och en annan väljer). hello zoomning säkerställer att hello text (teckensnitt) återger olika storlekar. Rullning är avstängd med olika intervall för varje rulla hello PGUP, PGDN uppåt och nedåt nycklar.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash videouppspelning - kör ~ 180 sekunder
En användare visar ett Adobe Flash-kodad video inbäddat i en webbsida. webbsidan hello lagras i hello lokala hårddisk hello värdserver för fjärrskrivbordssession. hello video spelas upp i Internet Explorer med en inbäddad player plugin-programmet.

Det här scenariot emulerar användare visa omfattande innehåll webbsidor som innehåller multimedia. De flesta hello data ska bo via VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Word fjärråtkomst att skriva – kör ~ 1 800 sekunder
Användaren anger ett dokument via en RDP-session. Tecknen skickas från hello klientsidan via hello RDP-session tooa dokument i Microsoft Word körs i hello fjärrsessionen. hello skriva hastighet är ett tecken varje 250 ms (totalt 7050 tecken). 

Detta är en hello vanligaste scenarierna för arbetar. Det här scenariot testar hello svarstider för en användare i en modern arbete processor. Det här scenariot är känsliga tooeven små ändringar i bandbreddsanvändningen.

## <a name="tracking-hello-test-results"></a>Spåra hello testresultat
Du kan använda hello efter tabellen tooevaluate hello scenarier i din miljö. hello informationen nedan gäller bara för bild - frågeprestanda avsevärt annorlunda från du se. 

För enkelhetens skull förutsätter vi att alla scenarier som testas med hello samma 1 920 x 1 080 bildpunkter skärmupplösning och TCP transporter i ett nätverk med svarstid (fördröjning) under 200 ms och nätverket jitter i hello 120 ms + Markera ca 1%.

Om hello tabell:

* **Genomsnittlig upplevelse** innehåller hello nätverksbandbredd där produktivitet påverkas inte avsevärt men utesluter inte tillfällig video eller ljud problem. hello system är kan toorecover snabbt genom att dra nytta av dynamisk hello-logik. hello nätverksbandbredd beräknar försök tooguarantee hello kvalitet hello användarupplevelse.
  * **Märkbar problem (break punkt)** innehåller hello nätverksbandbredd där användare kan märka viktiga problem i sina erfarenheter och deras produktivitet påverkas för mätbara tidsperioder. Nu är kämpar hello RDP algoritmer och kan inte garantera hello användarens kvalitet på grund av otillräcklig bandbredd.
  * **Rekommenderade** innehåller hello nätverksbandbredd rekommenderas för bra eller utmärkt upplevelse. Det är vanligtvis ett steg som är högre än hello värdet i motsvarande hello **genomsnittlig upplevelse** kolumn.
  * **Anteckningar** inkluderar observationer och kommentarer.

| Testa | Genomsnittlig upplevelse | Märkbar problem (break punkt) | Rekommenderade nätverkets bandbredd | Anteckningar |
| --- | --- | --- | --- | --- |
| Verkställande/komplexa PPT |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s rekommenderas |Många animeringar går förlorade vid 1 MB/s |
| Enkel PPT |5 MB/s |256 KB/s |10 MB/s |På 256 KB/sek hello bilder att läsa in med märkbar fördröjning |
| Internet Explorer |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s rekommenderas |Web videor är suddig på 1 MB/s och konstigt, snabb rullning har problem |
| Enkel PDF |1 MB/s |256 KB/s |5 MB/s |På 256 KB/sek, det tar en stund tooload hello sida |
| Blandat PDF |1 MB/s |256 KB/s |5 MB/s |På 256 KB/sek tar hello sidan en betydande tid tooload |
| Flash videouppspelning |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s rekommenderas |Vid 1 MB/s hello video är kornigt och vissa ramar släpps |
| Word fjärråtkomst att skriva |256 KB/s |128 KB/s |1 MB/s |På 256 KB/sek hända användaren hello tiden mellan tangenttryckningar |

tooevaluate hello nätverkets bandbredd per användare, skapa en blandning av hello ovan scenarier och hello motsvarande andel av nödvändiga nätverksbandbredd. Välj hello högsta antal som krävs för dina scenarier. Eftersom användarna använder nästan aldrig hello system enbart, Överväg att vissa reservera för användare som arbetar samtidigt på hello samma nätverk.

## <a name="learn-more"></a>Läs mer
* [Beräkna Azure RemoteApp användningen av nätverksbandbredd](remoteapp-bandwidth.md)
* [Azure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp-nätverkets bandbredd – allmänna riktlinjer (om du kan testa din egen)](remoteapp-bandwidthguidelines.md)

