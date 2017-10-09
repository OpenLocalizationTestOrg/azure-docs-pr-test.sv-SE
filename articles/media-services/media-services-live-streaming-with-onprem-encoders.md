---
title: "aaaStream live med lokala kodare som skapar dataströmmar i multibithastighet - Azure | Microsoft Docs"
description: "Det här avsnittet beskrivs hur tooset in en kanal som tar emot flera bithastigheter direktsänd dataström från en lokala kodare. hello dataströmmen kan sedan levereras tooclient uppspelning program via en eller flera strömningsslutpunkter, med någon av följande anpassningsbar strömning protokoll hello: HLS, Smooth Streaming STRECK."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 04/12/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 00709cecfc3b5b5dcfaa8f1e4b25bcf9d470d50b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="live-streaming-with-on-premises-encoders-that-create-multi-bitrate-streams"></a>Direktsänd strömning med lokala kodare som skapar dataströmmar i multibithastighet
## <a name="overview"></a>Översikt
I Azure Media Services en *kanal* representerar en pipeline för bearbetning av live-streaming innehåll. En kanal som tar emot live indata i ett av två sätt:

* En lokal livekodare skickar en multibithastighet RTMP eller Smooth Streaming (fragmenterad MP4) dataströmmen toohello kanal som inte är aktiverad tooperform live encoding med Media Services. hello inhämtas strömmarna passerar kanaler utan vidare bearbetning. Den här metoden anropas *direkt*. Du kan använda följande livekodare som har flera bithastigheter Smooth Streaming som utdata hello: Media Excel, Ateme, anta kommunikation, Envivio, Cisco och Elemental. hello följande livekodare har RTMP som utdata: Adobe Flash Live mediekodare, Telestream Wirecast, Haivision, Teradek och TriCaster. En livekodare kan även skicka en enkel bithastighet dataströmmen tooa kanal som inte är aktiverad för live encoding, men vi rekommenderar inte som. Hello dataströmmen toocustomers som begär den levererar Media Services.

  > [!NOTE]
  > Genomströmningsmetoden är hello mest ekonomiska sättet toodo live strömning.


* En lokal livekodare skickar en enkel bithastighet dataströmmen toohello kanal som är aktiverad tooperform live encoding med Media Services i något av hello följande format: RTP (MPEG-TS) RTMP eller Smooth Streaming (fragmenterad MP4). hello kanalen utför sedan live encoding av hello inkommande dataströmmen tooa flera bithastigheter (anpassningsbar) video dataström med enkel bithastighet. Hello dataströmmen toocustomers som begär den levererar Media Services.

Från och med hello Media Services 2.10 versionen när du skapar en kanal kan ange du hur du vill Indataströmmen din kanal tooreceive hello. Du kan också ange om du vill att hello kanal tooperform live encoding av strömmen. Du kan välja mellan två alternativ:

* **Gå igenom**: Ange det här värdet om du planerar toouse en lokal livekodare som har en dataström med multibithastighet (en direkt dataström) som utdata. I det här fallet passerar hello inkommande dataström toohello utdata utan kodning. Detta är hello beteendet för en kanal innan hello 2.10 versionen. Det här avsnittet ger information om att arbeta med kanaler av den här typen.
* **Live kodning**: Välj det här värdet om du planerar toouse Media Services tooencode din tooa multibithastighet dataström med enkel bithastighet direktsänd dataström. Tänk som lämnar en direktsänd kodning kanaler i en **kör** tillstånd kommer avgifter faktureringsinformation. Vi rekommenderar att du stoppar du omedelbart dina kanaler som körs efter live-streaming-händelse är klar tooavoid timvis extra kostnader. Hello dataströmmen toocustomers som begär den levererar Media Services.

> [!NOTE]
> Det här avsnittet beskrivs attributen för kanaler som inte är aktiverade tooperform live encoding. För information om att arbeta med kanaler som är aktiverat tooperform live encoding finns [direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar](media-services-manage-live-encoder-enabled-channels.md).
>
>

följande diagram hello representerar en live-streaming arbetsflöde som använder en lokal livekodare toohave multibithastighet RTMP eller fragmenterad MP4 (Smooth Streaming) dataströmmar som utdata.

![Live-arbetsflöde][live-overview]

## <a id="scenario"></a>Vanligt scenario för live-direktuppspelning
hello beskriver följande steg uppgifter som ingår i att skapa vanliga live-streaming-program.

1. Anslut en videokamera tooa dator. Starta och konfigurera en lokal livekodare som har en multibithastighet RTMP eller fragmenterad MP4 (Smooth Streaming) dataströmmen som utdata. Mer information finns i [Support och livekodare för Azure Media Services RTMP](http://go.microsoft.com/fwlink/?LinkId=532824).

    Du kan också utföra det här steget när du har skapat din kanal.
2. Skapa och starta en kanal.

3. Hämta hello kanal infognings-URL.

    hello live Encoding använder hello infognings-URL: en toosend hello dataströmmen toohello kanal.
4. Hämta hello kanalens förhandsgransknings-URL.

    Använd den här URL: en tooverify att din kanal är tar emot hello direktsänd dataström.
5. Skapa ett program.

    Det skapas även en tillgång till när du använder hello Azure-portalen för att skapa ett program.

    När du använder hello .NET SDK eller REST behöver toocreate en tillgång och ange toouse tillgången när du skapar ett program.
6. Publicera hello tillgång som är kopplad till programmet hello.   

    >[!NOTE]
    >När Azure Media Services-konto skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. Hej strömningsslutpunkt från vilken du vill att toostream innehåll har toobe i hello **kör** tillstånd.

7. Starta hello programmet när du är klar toostart strömning och arkivering.

8. Du kan också kan hello livekodare vara signalerat toostart en annons. hello annonsen infogas i utdataströmmen hello.

9. Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen.

10. Ta bort programmet hello (och ta eventuellt bort tillgången hello).     

## <a id="channel"></a>Beskrivning av en kanal och dess relaterade komponenter
### <a id="channel_input"></a>Kanalen indata (infognings) konfigurationer
#### <a id="ingest_protocols"></a>Strömmande infogningsprotokollet
Media Services stöder vill föra in levande feeds med flera bithastigheter fragmenterad MP4 och flera bithastigheter RTMP som strömningsprotokoll. När infognings-hello RTMP strömning protokoll har valts, två infognings-(indata) slutpunkter har skapats för hello kanalen:

* **Primär URL**: Anger hello fullständiga URL: en för hello kanal primära RTMP infognings-slutpunkten.
* **Sekundär URL** (valfritt): Anger hello fullständiga URL: en för hello kanal sekundära RTMP infognings-slutpunkten.

Använd hello sekundära URL Om du vill tooimprove hello hållbarhet och feltolerans för din infognings dataström (samt kodare redundans och feltolerans), särskilt för hello följande scenarier:

- Enskild kodare double-skicka tooboth primära och sekundära webbadresser:

    Hej Huvudsyftet med det här scenariot är tooprovide mer återhämtning toonetwork variationer och skakningar. Vissa RTMP kodare inte hantera nätverksavbrott bra. När det sker ett kopplas från, kan en kodare stoppa kodning och sedan inte skicka hello buffrade data när en återanslutning sker. Detta leder till avbrott och förlorade data. Nätverksavbrott kan inträffa på grund av ett felaktigt nätverk eller underhåll på hello Azure sida. Primära och sekundära URL: er minska hello nätverksproblem och ger en kontrollerad uppgraderingsprocessen. Varje gång som sker en schemalagd nätverk koppla från Media Services hanterar hello primära och sekundära kopplar ifrån och ger en fördröjd koppla mellan hello två. Kodare sedan har tid tookeep skicka data och ansluta igen. hello ordning hello kopplas från kan vara slumpmässiga, men det kommer alltid att vara en fördröjning mellan primära och sekundära eller sekundär eller den primära URL-adresser. I det här scenariot är hello kodare fortfarande hello enskild felpunkt.

- Flera kodare med varje encoder trycka tooa dedikerad punkt:

    Det här scenariot ger både kodare och infognings-redundans. I det här scenariot encoder1 pushes toohello primära URL och encoder2 skickar toohello sekundära URL. När en kodare misslyckas kan hello andra kodare hålla skickar data. Dataredundans kan upprätthållas eftersom Media Services inte kopplar primära och sekundära URL: er på hello samtidigt. Det här scenariot förutsätter att kodare tid synkroniseras och ange exakt hello samma data.  

- Flera kodare double-skicka tooboth primära och sekundära webbadresser:

    I det här scenariot push både kodare data tooboth primära och sekundära URL: er. Detta ger hello bästa tillförlitlighet och feltolerans samt dataredundans. Det här scenariot kan tolerera fel i båda kodaren och kopplar, även om en kodare slutar fungera. Det förutsätts att kodare tid synkroniseras och ange exakt hello samma data.  

Information om RTMP livekodare finns [Azure Media Services RTMP Support och direktsända kodare](http://go.microsoft.com/fwlink/?LinkId=532824).

#### <a name="ingest-urls-endpoints"></a>Infognings-URL: er (slutpunkter)
En kanal som tillhandahåller en slutpunkt för indata (infognings-URL) som du anger i hello livekodaren, så kan push-hello kodare strömmar tooyour kanaler.   

Du kan hämta hello infognings-URL: er när du skapar hello-kanal. Du tooget dessa URL: er hello kanalen inte är toobe i hello **kör** tillstånd. När du är klar toostart trycka toohello datakanal hello kanal måste vara i hello **kör** tillstånd. När hello kanal har startat mata in data kan du förhandsgranska dataströmmen via hello förhandsgransknings-URL.

Du har ett alternativ för vill föra in fragmenterad MP4 (Smooth Streaming) direktsänd dataström via en SSL-anslutning. tooingest via SSL, se till att tooupdate hello infognings-URL: en tooHTTPS. Du kan för närvarande mata in RTMP via SSL.

#### <a id="keyframe_interval"></a>Keyframe intervall
När du använder en lokal livekodare toogenerate multibithastighet dataström anger hello keyframe intervall hello varaktigheten för hello grupp med bilder (GOP) som används av den externa kodaren. När hello kanal tar emot den här inkommande dataström, kan du leverera din liveströmning tooclient uppspelning program i något av följande format hello: Smooth Streaming, dynamiska anpassningsbar strömning via HTTP-(bindestreck) och HTTP Live Streaming (HLS). När du utför direktsänd strömning paketeras HLS alltid dynamiskt. Som standard beräknas automatiskt Media Services hello HLS segment paketering förhållandet mellan (fragment per segment) baserat på hello keyframe intervall som tas emot från hello livekodaren.

hello följande tabell visar hur hello segment varaktighet beräknas:

| Keyframe intervall | HLS segment paketering förhållandet (FragmentsPerSegment) | Exempel |
| --- | --- | --- |
| Mindre än eller lika med too3 sekunder |3:1 |Om KeyFrameInterval (eller GOP) är 2 sekunder är hello standard HLS segment paketering kvoten 3 too1. Detta skapar ett 6 sekund HLS segment. |
| 3 too5 sekunder |2:1 |Om KeyFrameInterval (eller GOP) är 4 sekunder, är hello standard HLS segment paketering förhållandet 2 too1. Detta skapar ett 8-sekund HLS segment. |
| Större än 5 sekunder |1:1 |Om KeyFrameInterval (eller GOP) är 6 sekunder är hello standard HLS segment paketering kvoten 1 too1. Detta skapar ett 6 sekund HLS segment. |

Du kan ändra hello fragment per segment förhållandet genom att konfigurera hello kanal utdata och inställningen FragmentsPerSegment på ChannelOutputHls.

Du kan också ändra hello keyframe intervall genom att ange hello KeyFrameInterval egenskapen på ChanneInput. Om du uttryckligen ställa in KeyFrameInterval hello HLS segment paketering förhållandet FragmentsPerSegment beräknas via hello regler som beskrivs ovan.  

Om du uttryckligen ställa in både KeyFrameInterval och FragmentsPerSegment använder Media Services hello-värden som du anger.

#### <a name="allowed-ip-addresses"></a>Tillåtna IP-adresser
Du kan definiera hello IP-adresser som är tillåtna toopublish video toothis kanal. Tillåtna IP-adressen kan anges som en av följande hello:

* En IP-adress (t.ex, 10.0.0.1)
* Ett IP-adressintervall som använder en IP-adress och nätmask CIDR (till exempel 10.0.0.1/22)
* Ett IP-adressintervall som använder en IP-adress och en prickad decimal nätmask (till exempel 10.0.0.1(255.255.252.0))

Om inga IP-adresser har angetts och det finns inga Regeldefinitionen tillåts ingen IP-adress. tooallow IP-adresser, skapa en regel och ange 0.0.0.0/0.

### <a name="channel-preview"></a>Kanal preview
#### <a name="preview-urls"></a>URL: er för förhandsgranskning
Kanaler ger en förhandsgranskningsslutpunkten (förhandsgranskning URL) om du använder toopreview och verifiera strömmen innan ytterligare bearbetning och leverans.

Du kan hämta hello förhandsgransknings-URL när du skapar hello-kanal. För du tooget hello URL hello kanal har inte toobe i hello **kör** tillstånd. När hello kanal startar mata in data, kan du förhandsgranska dataströmmen.

För närvarande hello Förhandsgranska dataströmmen levereras endast i fragmenterad MP4 (Smooth Streaming), oavsett hello angivet Indatatyp. Du kan använda hello [Smooth Streaming hälsoövervakning](http://smf.cloudapp.net/healthmonitor) player tootest hello smooth stream. Du kan också använda en spelare som har hosted i hello Azure portal tooview strömmen.

#### <a name="allowed-ip-addresses"></a>Tillåtna IP-adresser
Du kan definiera hello IP-adresser som är tillåtna tooconnect toohello förhandsgranskningsslutpunkten. Om inga IP-adresser anges, får alla IP-adresser. Tillåtna IP-adressen kan anges som en av följande hello:

* En IP-adress (t.ex, 10.0.0.1)
* Ett IP-adressintervall som använder en IP-adress och nätmask CIDR (till exempel 10.0.0.1/22)
* Ett IP-adressintervall som använder en IP-adress och en prickad decimal nätmask (till exempel 10.0.0.1(255.255.252.0))

### <a name="channel-output"></a>Kanal-utdata
Information om kanalen utdata finns hello [Keyframe intervall](#keyframe_interval) avsnitt.

### <a name="channel-managed-programs"></a>Kanal-hanterade program
En kanal är associerad med program som du kan använda toocontrol hello publicering och lagring av segment i en direktsänd dataström. Kanaler hanterar program. hello är relationen mellan kanal och program mycket lik tootraditional media där en kanal har en konstant ström av innehåll och ett program är begränsade toosome tidsinställd händelse på kanalen.

Du kan ange hello antal timmar som du vill att tooretain hello registreras innehåll för programmet hello genom att ange hello **Arkivfönster** längd. Det här värdet kan anges från minst 5 minuter tooa högst 25 timmar. Längd avgör också hello hur lång tid som klienter kan söka bakåt i tiden från hello aktuella direktsända positionen. Program kan köras under hello angiven tidsperiod, men innehåll som ligger bakom hello längd ignoreras kontinuerligt. Det här värdet för den här egenskapen avgör också hur länge hello klient manifest kan växa.

Varje program som är associerad med en tillgång som lagrar hello strömmas innehåll. En tillgång är mappade tooa block blob-behållaren i hello Azure storage-konto och hello i hello tillgången filer som blobbar i behållaren. toopublish hello programmet så att kunderna kan visa hello stream, måste du skapa en positionerare för hello associerade tillgången. Du kan använda den här lokaliserare toobuild en strömnings-URL som du kan tillhandahålla tooyour klienter.

En kanal har stöd för upp toothree program som körs samtidigt så du kan skapa flera Arkiv för hello samma inkommande dataström. Du kan publicera och arkivera olika delar av en händelse efter behov. Anta exempelvis att dina affärsbehov är tooarchive 6 timmar av ett program, men toobroadcast endast hello sista 10 minuter. tooaccomplish detta, behöver du toocreate två program som körs samtidigt. Ett program ställs in tooarchive 6 timmar av hello händelse, men hello programmet publiceras inte. hello kan inte ange tooarchive i 10 minuter och det här programmet har publicerats.

Du bör inte återanvända befintliga program för nya händelser. I stället skapa ett nytt program för varje händelse. Starta hello programmet när du är klar toostart strömning och arkivering. Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen.

toodelete arkiverat innehåll, stoppa och ta bort programmet hello och ta sedan bort hello associerade tillgången. En tillgång kan inte tas bort om den används av ett program. hello programmet måste tas bort först.

När du stoppar och ta bort programmet hello kan användare strömma ditt arkiverade innehåll som en video på begäran, tills du tar bort hello tillgången. Om du vill tooretain hello arkiverat innehåll, men inte har den tillgänglig för strömning, tar du bort hello strömning lokaliserare.

## <a id="states"></a>Kanal tillstånd och fakturering
Möjliga värden för hello aktuell status för en kanal är:

* **Stoppats**: Detta är hello inledningsvis i hello kanal när skapandet. I det här tillståndet hello kanal egenskaper kan uppdateras men strömning tillåts inte.
* **Starta**: hello kanal startas. Inga uppdateringar eller strömning tillåts i det här tillståndet. Om ett fel inträffar hello kanal returnerar toohello **stoppad** tillstånd.
* **Kör**: hello kanal kan bearbeta live dataströmmar.
* **Stoppa**: hello kanal stoppas. Inga uppdateringar eller strömning tillåts i det här tillståndet.
* **Ta bort**: hello kanal tas bort. Inga uppdateringar eller strömning tillåts i det här tillståndet.

hello följande tabell visar hur kanal tillstånd kartan toohello fakturerings-läge.

| Kanaltillstånd | Portalen UI-indikatorer | Fakturerad? |
| --- | --- | --- | --- |
| **Starta** |**Starta** |Nej (övergångsläge) |
| **Kör** |**Redo** (inget program som körs)<p><p>eller<p>**Strömning** (minst ett aktivt program) |Ja |
| **Stoppa** |**Stoppa** |Nej (övergångsläge) |
| **Stoppades** |**Stoppades** |Nej |

## <a id="cc_and_ads"></a>Stängd textning och ad infogning
hello följande tabell visar stöds standarder för dold textning och ad infogas.

| Standard | Anteckningar |
| --- | --- |
| CEA 708 och EIA 608 (708/608) |CEA 708 och EIA 608 är textning standarder för hello USA och Kanada.<p><p>För närvarande stöds textning endast om transporteras i hello kodade Indataströmmen. Du måste toouse en aktiva mediekodare som kan infoga 608 eller 708 beskrivningar i hello kodade dataströmmen som har skickats tooMedia tjänster. Hello innehåll med infogade bildtexter tooyour visningsprogram levererar Media Services. |
| TTML i .ismt (Smooth Streaming textspår) |Media Services dynamisk paketering gör det möjligt för klienter toostream innehållet i något av följande format hello: DASH, HLS eller Smooth Streaming. Om du vill mata in fragmenterad MP4 (Smooth Streaming) med etiketter i .ismt (Smooth Streaming textspår) kan du leverera hello dataströmmen tooonly Smooth Streaming-klienter. |
| SCTE 35 |SCTE 35 är ett digitalt signaling system som har använt toocue reklam infogning. Underordnade mottagare använda hello signal toosplice reklam till hello dataström för hello tilldelad tid. SCTE 35 måste skickas som ett null-optimerade spår i hello Indataströmmen.<p><p>För närvarande hello stöds endast Indataströmmen format som överför ad signaler fragmenterad MP4 (Smooth Streaming). hello stöds endast utdataformatet är också Smooth Streaming. |

## <a id="considerations"></a>Överväganden
När du använder en lokal livekodare toosend en kanal med flera bithastigheter dataströmmen tooa gäller hello följande villkor:

* Kontrollera att du har tillräckligt ledigt Internet anslutning toosend data toohello mata in punkter.
* Med hjälp av en sekundär infognings-URL krävs ytterligare bandbredd.
* hello inkommande dataström i multibithastighet kan ha högst 10 bildkvaliteten nivåer (lager) och högst 5 ljud spår.
* högsta genomsnittliga bithastighet hello för någon av hello bildkvaliteten nivåerna bör vara mindre än 10 Mbit/s.
* hello måste aggregering av hello genomsnittlig bithastighet för alla hello video och ljud dataströmmar ha färre än 25 Mbit/s.
* Du kan inte ändra hello indataprotokollet när hello kanalen eller dess associerade program körs. Om du behöver olika protokoll får du skapa separata kanaler för varje indataprotokoll.
* Du kan mata in en enkel bithastighet i din kanal. Men eftersom hello kanal inte bearbetar hello stream, hello klientprogram även ta emot en dataström med enkel bithastighet. (Vi rekommenderar inte det här alternativet.)

Här följer andra relaterade tooworking överväganden kanaler och relaterade komponenter:

* Varje gång du konfigurera om hello livekodaren anropa hello **återställa** metod på hello-kanal. Innan du återställer hello kanal har toostop hello program. Starta om programmet hello när du har återställt hello-kanal.
* En kanal kan stoppas endast när den är i hello **kör** tillstånd och alla program på hello kanalen har stoppats.
* Som standard kan du lägga till endast 5 kanaler tooyour Media Services-konto. Mer information finns i [kvoter och begränsningar](media-services-quotas-and-limitations.md).
* Du debiteras endast när din kanal är i hello **kör** tillstånd. Mer information finns i toohello [tillstånd och fakturering kanaler](media-services-live-streaming-with-onprem-encoders.md#states) avsnitt.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Relaterade ämnen
[Azure Media Services fragmenterad MP4 live infognings-specifikationen](media-services-fmp4-live-ingest-overview.md)

[Översikt över Azure Media Services och vanliga scenarier](media-services-overview.md)

[Media Services-koncepten](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
