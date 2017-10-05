---
title: "Azure RemoteApp - testa din användandet av nätverkets bandbredd med några vanliga scenarier | Microsoft Docs"
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
ms.openlocfilehash: 8ad172a06e34cd0647079d787097cb2696cf116e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - testa din användandet av nätverkets bandbredd med några vanliga scenarier
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Enligt beskrivningen i [uppskattning Azure RemoteApp nätverksbandbredd](remoteapp-bandwidth.md), det bästa sättet att ta reda på vad effekten av Azure RemoteApp till ditt nätverk är att köra vissa tester för användning. Kör dessa tester för en viss tidsperiod och mäta bandbredd som krävs för varje scenario. Om du har möjlighet, kan du mäta i nätverket paket dataförlust och nätverk jitter för att förstå de nätverk mönster som kommer att skapas i din miljö.

Kom ihåg att användningen varierar mellan olika användare inom ditt företag när du utvärderar bandbreddsanvändningen. Till exempel använder text läsare och skrivare vanligtvis mindre bandbredd än användare som arbetar med video. För bästa resultat bör studera dina egna behov och skapa en blandning av följande scenarier som bäst representerar användare i företaget. Kom ihåg att [granska de faktorer som påverkar användningen av nätverksbandbredd och användaren upplever](remoteapp-bandwidthexperience.md) -som hjälper dig att identifiera bästa testerna.

Läsa om testerna, Välj din blandning och kör dem sedan. Du kan använda tabellen nedan för att spåra prestanda.

> [!NOTE]
> Om du inte kan göra egen testning av nätverket eller om du har inte tid att göra det, Kolla in våra [grundläggande bandbredd beräknar/nätverksrekommendationer](remoteapp-bandwidthguidelines.md). Din hittills utfört kan variera, men det om du *kan* köra dina egna tester bör du.
> 
> 

## <a name="the-usage-tests"></a>Användning-test
Var och en av dessa tester kör för olika mycket tid och testa olika funktioner och funktioner som använder nätverksbandbredden. Kom ihåg att välja blandning av test som bäst matchar din enskilda företagsanvändare.

### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Verkställande/komplexa PowerPoint - köra 900 1000 sekunder
En användare anger mellan 45 50 hög återgivning bilder med hjälp av Microsoft Office PowerPoint i helskärmsläge. Bilderna ska innehålla bilder, övergångar (med animering) och spelarbakgrunder med färgskalan som är vanliga för ditt företag. Användaren bör ägna minst 20 sekunder på varje bild.

Det här scenariot skapar burst-trafik när en bild övergår till nästa bild i presentationen.

### <a name="simple-powerpoint---run-for-620-seconds"></a>Enkel PowerPoint - kör ~ 620 sekunder
En användare anger en enkel PowerPoint-fil med cirka 30 bilder med hjälp av Microsoft Office PowerPoint i helskärmsläge. Bilderna är mer text-intensiva än i scenariot verkställande/komplexa PowerPoint och har enklare bakgrund och bilder (svart diagram). 

### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - kör ~ 250 sekunder
En användare bläddrar på webben med hjälp av Internet Explorer. Användaren bläddrar och bläddrar genom en blandning av text, fysiska bilder och vissa diagram. De webbsidor som lagras på den lokala diskenheten på servern som värd för fjärrskrivbordssession (RD Session Host) en. MHT-fil. Användaren rullar med olika intervall för varje nyckel/Bläddra PGUP, PGDN uppåt och nedåt nycklar:

    - Ned - 250 tangenttryckningar mycket 500 ms
    - Page Up - 36 tangenttryckningar varje 1 000 ms
    - Ned - 75 tangenttryckningar var 100 ms
    - Page Down - 20 tangenttryckningar varje 500 ms
    - Upp - 120 tangenttryckningar var 300 ms

### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-dokument - enkelt - kör ~ 610 sekunder
En användare läser och söker ett PDF-dokument på olika sätt med hjälp av Adobe Acrobat Reader. Dokumentet består av tabeller, diagram enkla och flera teckensnitt. Dokumentet är 35-40 sidor långa. Användaren bläddrar genom bakåtkompatibilitet med två olika hastigheter och vidarebefordrar på fyra olika zoom-storlekar (Anpassa till sidan, anpassa till bredd, 100% och en annan väljer). Funktionen garanterar att text (teckensnitt) återger olika storlekar. Rullning är avstängd med olika intervall för varje rulla PGUP uppåt och nedåt nycklar, PGDN.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>PDF-dokument - blandat - körning för ~ 320 sekunder
En användare läser och söker ett PDF-dokument på olika sätt med hjälp av Adobe Acrobat Reader. Dokumentet består av hög kvalitet avbildningar (inklusive fotografier), tabeller, enkla diagram och flera textteckensnitt. Användaren bläddrar genom bakåtkompatibilitet med två olika hastigheter och vidarebefordrar på fyra olika zoom-storlekar (Anpassa till sidan, anpassa till bredd, 100% och en annan väljer). Funktionen garanterar att text (teckensnitt) återger olika storlekar. Rullning är avstängd med olika intervall för varje rulla PGUP uppåt och nedåt nycklar, PGDN.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash videouppspelning - kör ~ 180 sekunder
En användare visar ett Adobe Flash-kodad video inbäddat i en webbsida. Sidan lagras i den lokala hårddisken på värdservern för fjärrskrivbordssessioner. Videon spelas av ett inbäddat player plugin-program i Internet Explorer.

Det här scenariot emulerar användare visa omfattande innehåll webbsidor som innehåller multimedia. De flesta data ska bo via VOBR.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Word fjärråtkomst att skriva – kör ~ 1 800 sekunder
Användaren anger ett dokument via en RDP-session. Tecknen skickas från klienten via RDP-session till ett dokument i Microsoft Word körs i fjärrsessionen. Skriv frekvensen är ett tecken varje 250 ms (totalt 7050 tecken). 

Detta är en av de vanligaste scenarierna för arbetar. Det här scenariot testar svarstiderna för en användare i en modern arbete processor. Det här scenariot är känsligt för även små förändringar i bandbreddsanvändningen.

## <a name="tracking-the-test-results"></a>Spårning av testresultaten
Du kan använda följande tabell för att utvärdera scenarier i din miljö. Informationen nedan gäller bara för bild - frågeprestanda avsevärt annorlunda från du se. 

För enkelhetens skull förutsätter vi att alla scenarier som testas med samma 1 920 x 1 080 bildpunkter skärmupplösningen och TCP transporter i ett nätverk med svarstid (fördröjning) under 200 ms och nätverket jitter i 120 ms + Markera ca 1%.

Om tabellen:

* **Genomsnittlig upplevelse** innehåller nätverksbandbredd där produktivitet påverkas inte avsevärt men utesluter inte tillfällig video eller ljud problem. Systemet kan återhämta sig snabbt genom att dra nytta av dynamisk logiken. Nätverksbandbredd beräknar försöket att garantera kvaliteten på användarupplevelsen.
  * **Märkbar problem (break punkt)** innehåller nätverksbandbredd där användare kan märka viktiga problem i sina erfarenheter och deras produktivitet påverkas för mätbara tidsperioder. Nu är kämpar RDP-algoritmer och kan inte garantera användarens kvalitet på grund av otillräcklig bandbredd.
  * **Rekommenderade** innehåller den nätverksbandbredd som rekommenderas för bra eller utmärkt upplevelse. Det är vanligtvis ett steg som är högre än värdet i motsvarande **genomsnittlig upplevelse** kolumn.
  * **Anteckningar** inkluderar observationer och kommentarer.

| Testa | Genomsnittlig upplevelse | Märkbar problem (break punkt) | Rekommenderade nätverkets bandbredd | Anteckningar |
| --- | --- | --- | --- | --- |
| Verkställande/komplexa PPT |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s rekommenderas |Många animeringar går förlorade vid 1 MB/s |
| Enkel PPT |5 MB/s |256 KB/s |10 MB/s |På 256 KB/sek, läsa in bilder med märkbar fördröjning |
| Internet Explorer |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s rekommenderas |Web videor är suddig på 1 MB/s och konstigt, snabb rullning har problem |
| Enkel PDF |1 MB/s |256 KB/s |5 MB/s |På 256 KB/sek, tar det en stund att läsa in sidan |
| Blandat PDF |1 MB/s |256 KB/s |5 MB/s |Vid 256 KB/sek tar sidan mycket tid att läsa in |
| Flash videouppspelning |10 MB/s |1 MB/s |> 10 MB/s, 100 MB/s rekommenderas |Vid 1 MB/s videon är kornigt och vissa ramar släpps |
| Word fjärråtkomst att skriva |256 KB/s |128 KB/s |1 MB/s |På 256 KB/sek hända användaren tiden mellan tangenttryckningar |

Skapa en blandning av ovanstående scenarier och motsvarande andel av nätverksbandbredd krävs för att utvärdera nätverkets bandbredd per användare. Välj det högsta antal som krävs för dina scenarier. Eftersom användarna använder nästan aldrig systemet enbart, Överväg vissa reservera för användare som arbetar samtidigt på samma nätverk.

## <a name="learn-more"></a>Läs mer
* [Beräkna Azure RemoteApp användningen av nätverksbandbredd](remoteapp-bandwidth.md)
* [Azure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp-nätverkets bandbredd – allmänna riktlinjer (om du kan testa din egen)](remoteapp-bandwidthguidelines.md)

