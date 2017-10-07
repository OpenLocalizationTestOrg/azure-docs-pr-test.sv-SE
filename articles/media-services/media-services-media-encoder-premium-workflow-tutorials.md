---
title: "aaaAvanced Media Encoder Premium arbetsflödet självstudier"
description: "Det här dokumentet innehåller genomgång som visar hur tooperform avancerade uppgifter med Media Encoder Premium arbetsflöde och även hur toocreate komplexa arbetsflöden med Arbetsflödesdesignern."
services: media-services
documentationcenter: 
author: xstof
manager: cfowler
editor: 
ms.assetid: 1ba52865-b4a8-4ca0-ac96-920d55b9d15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: christoc;xpouyat;juliako
ms.openlocfilehash: 641bd1be3bd795b5e138fa7ffdf299ff6710904e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-media-encoder-premium-workflow-tutorials"></a>Avancerade Media Encoder Premium arbetsflödet självstudier
## <a name="overview"></a>Översikt
Det här dokumentet innehåller genomgång som visar hur toocustomize arbetsflöden med **Arbetsflödesdesignern**. Du kan hitta hello faktiska Arbetsflödesfiler [här](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

## <a name="toc"></a>INNEHÅLLSFÖRTECKNING
hello följande avsnitt beskrivs:

* [Kodning MXF till en MP4 med enkel bithastighet](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
  * [Starta ett nytt arbetsflöde](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new)
  * [Med hjälp av hello Media filen indata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
  * [Kontrollera dataströmmar media](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
  * [Lägger till en videokodare för. Generera filen för MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
  * [Kodning hello ljudström](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
  * [Multiplexering ljud och Video strömmar till en MP4-behållare](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
  * [Skrivning hello MP4-fil](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
  * [Skapa ett Media Services tillgångsinformation från hello utdatafil](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
  * [Testa hello klar arbetsflödet lokalt](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
* [Kodning MXF till multibitrate MP4s - dynamisk paketering aktiverad](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
  * [Lägga till en eller flera ytterligare MP4-utdata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
  * [Konfigurera hello utdata-filnamn](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
  * [Lägga till ett separat spår för ljud](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
  * [Lägger till hello. ISM SMIL-fil](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
* [Kodning MXF till multibitrate MP4 - förbättrad utkast](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
  * [Översikt över tooenhance för arbetsflöde](#workflow-overview-to-enhance)
  * [Filen namngivningskonventioner](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
  * [Publicering av egenskaperna för komponenten på hello arbetsflödet rot](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
  * [Har genererat utdatafilen namn är beroende av publicerade egenskapsvärden](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
* [Lägger till miniatyrbilder toomultibitrate MP4-utdata](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
  * [Arbetsflödet översikt tooadd miniatyrer](#workflow-overview-to-add-thumbnails-to)
  * [Lägga till JPG kodning](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
  * [Hantera färg utrymme konvertering](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
  * [Skrivning hello miniatyrer](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
  * [Identifiering av fel i ett arbetsflöde](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
  * [Klar arbetsflöde](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
* [Tidsbaserade trimning multibitrate MP4-utdata](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
  * [Arbetsflödet översikt toostart lägger till trimning till](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
  * [Med hjälp av hello dataströmmen Trimmer](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
  * [Klar arbetsflöde](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
* [Introduktion till hello skripta komponent](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
  * [Skript i ett arbetsflöde: hello world](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
* [RAM-baserade trimning multibitrate MP4-utdata](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
  * [Utkast översikt toostart lägger till trimning till](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
  * [Med hjälp av hello Clip listan XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
  * [När du ändrar hello clip listan från en skript-komponent](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
  * [Lägga till en ClippingEnabled bekvämlighet egenskap](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

## <a id="MXF_to_MP4"></a>Kodning MXF till en MP4 med enkel bithastighet
I den här genomgången ska vi skapa en enkel bithastighet. MP4-fil med AAC HAN kodat ljud från en. MXF indatafilen.

### <a id="MXF_to_MP4_start_new"></a>Starta ett nytt arbetsflöde
Öppna Workflow Designer och väljer ”fil”-”ny arbetsyta”-”Omkoda modell”

arbetsflöde för ny hello visar 3 element:

* Primär källfilen
* Beskär List-XML
* Filen/Utdatatillgången  

![Arbetsflöde för ny kodning](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Arbetsflöde för ny kodning*

### <a id="MXF_to_MP4_with_file_input"></a>Med hjälp av hello Media filen indata
I ordning tooaccept våra inkommande mediefilen startar med att lägga till en Media filen inkommande komponent. tooadd ett komponenten toohello arbetsflöde, leta efter den i sökrutan för hello databasen och dra hello önskad post till hello designern rutan. Gör detta för hello Media filen inkommande och ansluta hello primära källfilen komponenten toohello Filename inkommande PIN-kod från hello Media filen indata.

![Anslutna mediafilen indata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Anslutna mediafilen indata*

Innan vi kan göra mycket annan först behöver vi tooindicate toohello Arbetsflödesdesignern vilka exempelfilen vill vi toouse toodesign våra arbetsflöde med. toodo så klicka på hello designern rutan Bakgrund och leta efter hello primära källfilen egenskapen i hello egenskapen högra fönstret. Klicka på hello mappikon och välj hello önskad filen tootest hello arbetsflöde med. När det är klart hello Media filen indata komponenten inspektera hello-filen och fylla i sina PIN-koder tooreflect hello utdatafilen det kontrolleras.

![Ifyllda mediafilen indata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Ifyllda mediafilen indata*

När detta anger med vad indata vill vi toowork med den tala inte ännu där hello kodade utdata ska gå att. Liknande toohow hello primära källfilen har konfigurerats kan du nu konfigurera hello utdata mappen variabeln egenskapen under den.

![Konfigurerade ingående och utgående egenskaper](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Konfigurerade ingående och utgående egenskaper*

### <a id="MXF_to_MP4_streams"></a>Kontrollera dataströmmar media
Ofta är den önskade tooknow hur hello dataströmmen som de som ser ut som flödar genom hello arbetsflöde. tooinspect en dataström när som helst i arbetsflöde hello, bara på en utgående eller inkommande PIN-kod på någon av hello komponenter. I det här fallet ska du använda kommandot på hello okomprimerade Video utdata PIN-kod från våra Media filen indata. En dialogruta öppnas som tillåter tooinspect hello utgående video.

![Kontrollera hello utdata okomprimerade Video PIN-kod](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Kontrollera hello utdata okomprimerade Video PIN-kod*

I vårt fall meddelar oss till exempel att vi hantera en 1 920 x 1 080 inmatning med 24 bildrutor per sekund i 4:2:2 provtagning en video om nästan 2 minuter.

### <a id="MXF_to_MP4_file_generation"></a>Lägger till en videokodare för. Generera filen för MP4
Observera att nu, en okomprimerad Video och flera utdata PIN-koder för okomprimerade ljud är tillgängliga för användning på vår Media filen indata. I ordning tooencode hello inkommande video måste vi komponenten kodning – i det här fallet för att generera. MP4-filer.

tooencode hello video-ström tooH.264, Lägg till hello AVC-videokodare komponenten toohello designytan. Den här komponenten tar en video Expandera-dataström som indata och levererar en AVC komprimerade video ström på dess utdata PIN-kod.

![Ansluten AVC-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Ansluten AVC-kodaren*

Egenskaperna avgör hur hello kodning exakt händer. Låt oss ta en titt på några av hello viktigare inställningar:

* Utdata bredd och höjd för utdata: dessa avgör hello lösning av hello kodad video. I vårt fall vi väljer 640 x 360
* Bildfrekvens: när set toopassthrough visas bara antar hello källa bildfrekvens, är det möjligt toooverride detta dock. Observera att sådana ramhastighet konvertering inte rörelse-ersätts.
* Profil och nivå: dessa avgör hello AVC-profil och nivå. tooconveniently få mer information om hello olika nivåer och profiler, klickar du på hello frågetecken på hello AVC Video Encoder komponent och hello hjälpsidan ska visa mer information om varje hello nivåer. För exemplet lägger vi väljer Main profil på nivå 3.2 (hello standard).
* Betygsätt läge och bithastighet (kbps): i vårt scenario vi välja en konstant bithastighet (CBR) som utdata med 1200 kbit/s
* Video Format: Detta är om hello VUI (Video användbarhet Information) som skrivs till hello H.264 dataström (sida information som kan användas av en avkodarens tooenhance hello visning men är inte nödvändigt toocorrectly avkoda):
* NTSC (typisk för USA eller Japan med 30 fps)
* PAL (typisk för Europa, med 25 bildrutor per sekund)
* GOP storleksläget: vi konfigurerar fast GOP storlek för våra ändamål med ett intervall med nyckel 2 sekunder med stängd GOPs. Detta garanterar kompatibilitet med hello dynamisk paketering Azure Media Services tillhandahåller.

toofeed våra AVC-kodaren ansluta hello okomprimerade Video utdata PIN-kod från hello Media filen inkommande komponenten toohello okomprimerade Video inkommande PIN-kod från hello AVC-kodaren.

![Anslutna AVC-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Anslutna AVC Main-kodaren*

### <a id="MXF_to_MP4_audio"></a>Kodning hello ljudström
Nu har vi kodad video men hello ursprungliga okomprimerade ljudström måste fortfarande toobe komprimeras. För den här ska vi gå med AAC kodning av hello AAC-kodare (Dolby) komponent. Lägga till den toohello arbetsflöde.

![Ansluten AVC-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Ansluten AAC kodaren*

Nu finns det en inkompatibilitet: det finns bara en enda okomprimerade ljud inkommande PIN-kod från hello AAC kodare när fler än sannolikt hello Media filen indata har två olika okomprimerade ljudström som är tillgängliga: en för hello vänster ljud kanal och en för hello höger. (Om du hantera surroundljud, som är 6 kanaler). Så det inte går ansluta toodirectly hello ljud från hello Media filen Indatakällan till hello AAC ljud kodare. hello AAC komponenten förväntar sig en så kallad ”överlagrad” ljudström: en enda ström som har både hello vänster och hello rätt kanaler överlagrat med varandra. När vi vet från våra media källfil tilldelas vilka ljud spår på vilken position i hello källan vi generera sådana överlagrad ljudström med hello korrekt talare positioner för vänster och höger.

Först ska en toogenerated en överlagrad dataström från hello krävs källa ljud kanaler. hello ljud dataströmmen Interleaver komponenten hantera det för oss. Lägg till den toohello arbetsflödet och ansluta hello ljud utdata från hello Media filen indata till den.

![Ansluten ljudström Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Ansluten ljudström Interleaver*

Nu när vi har en överlagrad ljudström vi fortfarande inte har angett där tooassign hello åt vänster eller höger talare positioner åt. I ordning toospecify detta, kan vi utnyttja hello talare Position du.

![Lägga till en talare Position du](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Lägga till en talare Position du*

Konfigurera hello talare Position du för användning med en stereo Indataströmmen via en kodare förinställda Filter för ”anpassad” och hello kanal förinställningen kallas ”2.0 (L, R)”. (Detta tilldelar hello vänstra högtalaren position toochannel 1 och hello rätt talare position toochannel 2.)

Ansluta hello utdata från hello talare Position du toohello indata för hello AAC kodare. Sedan anger du hello AAC kodare toowork med en ”2.0 (L, R)” kanal förinställningen så att den vet toodeal med stereoljud som indata.

### <a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexering ljud och Video strömmar till en MP4-behållare
Beroende på vår AVC kodade video-ström och våra AAC kodade ljudström, vi kan samla in båda till en. MP4-behållare. hello kallas blandar olika dataströmmar i en enda ”multiplexering” (eller ”muxing”). I det här fallet vi interfoliering hello ljud- och videoströmmar för hello i en enda konsekvent. MP4-paketet. hello-komponent som samordnar för en. MP4-behållaren kallas hello multiplexor ISO MPEG-4. Lägg till en toohello designytan och ansluta både hello AVC Video Encoder och hello AAC kodare tooits indata.

![Anslutna MPEG4 multiplexor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Anslutna MPEG4 multiplexor*

### <a id="MXF_to_MP4_writing_mp4"></a>Skrivning hello MP4-fil
När du skriver en utdatafil används hello utdata från komponenten. Vi kan ansluta den här toohello utdata från hello ISO MPEG-4 multiplexor så att dess utdata skrivs till toodisk. toodo, ansluta hello behållare (MPEG-4) utdata PIN-kod toohello skrivåtgärder inkommande PIN-koden för hello utdata.

![Ansluten utdata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Ansluten utdata*

hello filnamn som kommer att användas bestäms av hello egenskapen. Egenskapen kan vara hårdkodad tooa angivet värde, troligen en ska tooset det via ett uttryck i stället.

toohave hello arbetsflödet automatiskt avgöra hello utdata filen namnegenskapen från ett uttryck, klickar på hello buton nästa toohello filnamn (nästa toohello mappikon visas). Från hello listrutan och välj sedan ”uttryck”. Appskärmen hello uttrycksredigerare. Radera hello hello Editor först.

![Tom uttrycksredigerare](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Tom uttrycksredigerare*

hello uttrycksredigerare kan tooenter alla litteralvärde blandas med en eller flera variabler. Variabler börja med ett dollartecken. Eftersom du träffar hello $ nyckel, visas hello editor listrutan med ett urval av tillgängliga variabler. I vårt fall använder vi en kombination av hello output directory och hello grundläggande indatafilen namnet variabel:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Fylls ut uttrycksredigerare](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Fylls ut uttrycksredigerare*

> [!NOTE]
> Du måste ange ett värde i hello Uttrycksredigerare, i ordning toosee hittar en utdatafil kodning jobbet i Azure.
>
>

När du bekräftar hello uttryck genom att träffa ok kommer hello egenskapen fönstret Förhandsgranska toowhat värdet hello filen egenskapen matchar vid denna tidpunkt.

![Fil-uttrycket matchar utdata dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Fil-uttrycket matchar utdata dir*

### <a id="MXF_to_MP4_asset_from_output"></a>Skapa ett Media Services tillgångsinformation från hello utdatafil
När vi har skapat en MP4-utdatafil vi behöver du fortfarande tooindicate att den här filen tillhör toohello utdata tillgången där media services genererar ett resultat av körning av arbetsflödet. toothis syfte hello fil/Utdatatillgången nod på hello arbetsflödet arbetsytan används. Alla inkommande filer till den här noden gör en del av hello Azure Media Services tillgång.

Ansluta hello utdata från komponenten toohello fil/Utdatatillgången komponenten toofinish hello arbetsflöde.

![Klar arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Klar arbetsflöde*

### <a id="MXF_to_MP4_test"></a>Testa hello klar arbetsflödet lokalt
tootest hello lokalt, arbetsflöde nådde hello uppspelningsknappen i hello verktygsfältet hello överst. När du är klar hello arbetsflödet körs inspektera hello utdata som genereras i hello konfigurerats utdatamapp. Du ser hello klar MP4 utdatafiler som kodats från hello MXF inkommande källfil.

## <a id="MXF_to_MP4_with_dyn_packaging"></a>Kodning MXF till MP4 - multibitrate dynamisk paketering aktiverad
I den här genomgången skapar vi en uppsättning flera MP4-filer med AAC kodade ljud från en enda. MXF indatafilen.

När en multibithastighet tillgången utdata önskas för användning i kombination med hello dynamisk paketering funktioner som erbjuds av Azure Media Services hanterar flera GOP-justerad MP4-filer för var och en annan bithastighet och upplösning behöver toobe genereras. toodo så hello [kodning MXF i en enkel bithastighet MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) genomgången ger oss en bra utgångspunkt.

![Starta arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Starta arbetsflöde*

### <a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Lägga till en eller flera ytterligare MP4-utdata
Alla MP4-filer i vårt Azure Media Services tillgång stöder en annan bithastighet och upplösning. Lägg till en eller flera MP4 utgående filer toohello arbetsflöde.

toomake att vi har alla våra video kodare som skapats med Hej samma inställningar, enklaste tooduplicate hello redan befintliga AVC-videokodare och konfigurera en annan kombination av upplösning och bithastighet (Lägg till en 960 x 540 med 25 bildrutor per sekund på 2,5 Mbit/s). tooduplicate hello befintliga kodare, kopiera klistra in den på hello designytan.

Ansluta hello okomprimerade Video utdata PIN-kod hello Media filen indata till vår nya AVC-komponent.

![Andra AVC-kodaren som är ansluten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Andra AVC-kodaren som är ansluten*

Anpassa nu hello konfiguration för vår nya AVC-kodaren toooutput 960 x 540 med 2,5 Mbit/s. (Använd egenskaper ”utdata bredd”, ”utdata höjd” och ”bithastighet (kbps)” för detta.)

Får vi vill toouse hello tillgång tillsammans med Azure Media Services dynamisk paketering, hello strömmande slutpunkten måste toobe kan genereras från dessa MP4-filer HLS/fragmenterad MP4/DASH fragment som är exakt justerade tooeach andra på ett sätt att hämta en enda smooth kontinuerlig ljud och video experience klienter som byter mellan olika bithastighet. toomake som inträffa, behöver vi tooensure att hello GOP (”grupp av bilder”) storlek för båda MP4-filer har angetts i hello-egenskaperna för både AVC-kodare too2 sekunder, vilket gör du genom att:

* Ange hello GOP storleksläget tooFixed GOP storlek och
* hello nyckeln ramsintervall tootwo i sekunder.
* Ange hello GOP IDR kontrollen tooClosed GOP tooensure alla GOP Ständiga på sina egna utan beroenden

toomake våra arbetsflödet praktiskt toounderstand Byt namn på hello första AVC-kodaren för ”AVC Video Encoder 640 x 360 1200 kbit/s” och hello andra AVC-kodaren ”AVC Video Encoder 960 x 540 2500 kbit/s”.

Nu ska du lägga till en andra ISO MPEG-4-multiplexor och en andra fil-utdata. Ansluta hello multiplexor toohello nya AVC-kodare och kontrollera att dess utdata dirigeras till hello utdata. Även ansluta hello AAC ljud kodare utdata toohello nya multiplexor indata. hello utdata i sin tur sedan kan anslutna toohello fil/Utdatatillgången nod tooadd den toohello Media Services tillgång som kommer att skapas.

![Andra Muxer och utdata från anslutna](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Andra Muxer och utdata från anslutna*

Konfigurera hello multiplexor segmentet läge tooGOP antal eller varaktighet för kompatibilitet med Azure Media Services dynamisk paketering och ange hello GOPs per segment too1. (Detta ska vara hello standard.)

![Muxer segment lägen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Muxer segment lägen*

Obs: du kanske vill toorepeat den här processen för alla andra bithastighet och upplösning kombinationer som du vill toohave toohello tillgången utdata har lagts.

### <a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurera hello utdata-filnamn
Vi har mer än en fil tillagda toohello utdatatillgången. Detta ger en behovet toomake att hello filnamn för varje hello utgående filer skiljer sig från varandra och kanske även tillämpa ett filnamn så att det blir tydligt hello filnamn vad du arbetar med.

Utdata filnamngivning kan styras via uttryck i hello designer. Öppna hello-egenskapen för en av hello utdata komponenter och öppna hello uttrycksredigerare för hello egenskapen. Vårt första utdatafilen konfigurerades via hello följande uttryck (se hello kursen för att gå från [MXF tooa enkel bithastighet MP4 utdata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Det innebär att vår filename bestäms av två variabler: hello utdata directory toowrite i och hello källfilens grundläggande namn. hello f.d. visas som en egenskap på hello arbetsflödet rot och hello senare bestäms av inkommande hello-fil. Observera att hello målkatalogen är det du använder för att testa lokala; den här egenskapen åsidosätts av hello arbetsflödesmotorn när hello arbetsflödet körs av hello molnbaserade media processor i Azure Media Services.
toogive båda våra utdatafiler namngivning av konsekvent utdata, ändra hello filen först namngivning uttryck som:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

och andra hello till:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Köra en mellanliggande testkörning toomake att båda utdata MP4-filer skapas korrekt.

### <a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Lägga till ett separat spår för ljud
Som vi ser senare när vi skapar en .ism-fil toogo med våra MP4-filer för utdata visar kräver vi också en ljuddata MP4-fil som hello ljud spår för våra anpassningsbar strömning. toocreate detta filen, lägga till en ytterligare muxer toohello arbetsflöde (ISO-MPEG-4 multiplexor) och ansluta hello AAC kodare utdata PIN-kod med dess inkommande PIN-kod för spåra 1.

![Ljud Muxer som lagts till](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Ljud Muxer som lagts till*

Skapa en tredje filen komponenten toooutput hello utgående utdataström från hello muxer och konfigurera hello filen namngivning uttryck som:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Ljud Muxer skapa filen utdata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Ljud Muxer skapa filen utdata*

### <a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Lägger till hello. ISM SMIL-fil
För hello dynamisk paketering toowork i kombination med både MP4-filer (och hello ljuddata MP4) i vår Media Services tillgången, men vi behöver också en manifestfil (kallas även en ”SMIL”-fil: synkroniserade Multimedia Integration Language). Den här filen anger tooAzure Media Services vilka MP4-filer är tillgängliga för dynamisk paketering och vilken av dessa tooconsider för hello ljud strömning. En typisk manifestfil för en uppsättning MP4's med en enda ljudström ser ut så här:

    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

hello .ism-fil som innehåller inom en switch-instruktion får en referens tooeach av hello enskilda MP4-filer och dessutom toothose också (minst) ljudfil refererar till tooan MP4 som endast innehåller hello ljud.

Genererar hello-manifestfilen för våra uppsättning MP4's kan göras via en komponent som kallas hello ”AMS Manifest Writer”. toouse, drar den till hello yta och ansluter hello ”skriva slutförd” utdata PIN-koder från hello tre utdata komponenter toohello AMS Manifest skrivare som indata. Gör att tooconnect hello utdata från hello AMS Manifest Writer toohello fil/Utdatatillgången.

Precis som med våra andra filen utdata komponenter, konfigurerar du hello .ism filnamn utdata med ett uttryck:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Vår klar arbetsflöde ser ut så hello nedan:

![Klar MXF toomultibitrate MP4-arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Klar MXF toomultibitrate MP4-arbetsflöde*

## <a id="MXF_to__multibitrate_MP4"></a>Kodning MXF till multibitrate MP4 - förbättrad utkast
I hello [tidigare arbetsflöde genomgången](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) vi har sett hur en enda MXF inkommande tillgång kan konverteras till en utdatatillgången med flera bithastigheter MP4-filer, en ljuddata MP4-fil och en manifestfil för användning tillsammans med Azure Media Services dynamisk paketering.

Den här genomgången visar hur vissa aspekter av hello kan förbättras och görs enklare.

### <a id="MXF_to_multibitrate_MP4_overview"></a>Översikt över tooenhance för arbetsflöde
![Multibitrate MP4 arbetsflödet tooenhance](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4 arbetsflödet tooenhance*

### <a id="MXF_to__multibitrate_MP4_file_naming"></a>Filen namngivningskonventioner
I tidigare hello-arbetsflöde anges vi ett enkelt uttryck som hello grunden för att generera utdata-filnamn. Vi har några duplicering men: alla hello hello enskilda utdata filen komponenter angivna sådana uttrycket.

Till exempel har våra filen utdata-komponenten för hello första videofil konfigurerats med det här uttrycket:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Vi har ett uttryck som medan för hello andra utdata video:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Skulle det vara tydligare mindre fel felbenägna och bekvämare om vi kan ta bort några av duplicering och göra det mer konfigureras i stället? Som tur kan vi: hello designer uttryck funktioner i kombination med hello möjlighet toocreate anpassade egenskaper på vår arbetsflödet rot ger oss ett extra lager av bekvämlighet.

Antar vi att vi ska hårddiskkonfiguration filename från hello bithastighet hello enskilda MP4-filer. Dessa bithastighet mål tooconfigure i en central placera (på hello-roten för våra graph) från där användarna ska komma åt tooconfigure och enheten genereringen av filnamnet. toodo kan vi börja med att publicera hello bithastighet egenskap från båda AVC kodare toohello roten för våra arbetsflödet, så att den blir tillgänglig från båda hello rot samt från och med hello AVC kodare. (Även om visas i två olika platser, det är endast ett underliggande värde.)

### <a id="MXF_to__multibitrate_MP4_publishing"></a>Publicering av egenskaperna för komponenten på hello arbetsflödet rot
Öppna hello första AVC-kodaren, gå toohello bithastighet (kbps) egenskap och välj Publicera hello listrutan.

![Publicerade hello bithastighet egenskapen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Publicerade hello bithastighet egenskapen*

Konfigurera hello publicera dialogrutan toopublish toohello roten för våra arbetsflöde diagram med en publicerade namnet ”video1bitrate” och en läsbar visningsnamn ”Video 1 bithastighet”. Konfigurera ett eget gruppnamn kallas ”strömning bithastighet” och klicka på Publicera.

![Publicerade hello bithastighet egenskapen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Publishing dialogrutan för egenskapen bithastighet*

Upprepa hello samma för hello bithastighet-egenskapen för hello andra AVC-kodare och ge den namnet ”video2bitrate” med ett namn för ”Video 2 bithastighet”, i hello samma anpassade gruppen ”strömning bithastighet”.

Om vi nu granska hello arbetsflödesegenskaper rot, se vi vår anpassad grupp med hello två publicerade egenskaper visas. Både reflektion hello värdet för deras respektive AVC-kodaren bithastighet.

![Egenskaper för publicerade bithastighet i rot-arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

När vi vill tooaccess dessa egenskaper från kod eller ett uttryck kan vi göra det så här:

* från infogad kod från en komponent under hello rot: node.getPropertyAsString('.. / video1bitrate', null)
* i ett uttryck: ${ROOT_video1bitrate}

Vi Slutför hello ”strömning bithastighet” grupp genom att publicera våra ljud spåra bithastighet på den också. Sök efter hello bithastighet inställningen i hello egenskaper hello AAC kodare, och välj Publicera hello dropdown nästa tooit. Publicera toohello rot hello diagrammet med namnet ”audio1bitrate” och visningsnamnet ”ljud 1 bithastighet” i vårt anpassade grupp ”strömning bithastighet”.

![Publishing dialogrutan för ljud bithastighet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Publishing dialogrutan för ljud bithastighet*

![Resulterande video och ljud sammanställer på rot](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Resulterande video och ljud sammanställer på rot*

Observera att även om du ändrar något av dessa tre värden konfigurerar igen och ändringar hello värden på respektive hello komponenter som de är kopplade till (och där publicerade från).

### <a id="MXF_to__multibitrate_MP4_output_files"></a>Har genererat utdatafilen namn är beroende av publicerade egenskapsvärden
I stället för hardcoding våra genererade filnamn, vi kan nu ändra våra filename-uttrycket på varje hello utdata komponenter toorely på hello bithastighet egenskaper som vi just publicerad på hello diagrammet rot. Med vår utdata från första början hitta hello filegenskapen och redigera hello uttryck så här:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

hello olika parametrar i det här uttrycket kan nås och anges genom att träffa hello dollartecken på hello tangentbord i hello uttrycksfönster. En av hello tillgängliga parametrar är vår video1bitrate-egenskap som vi tidigare publicerade.

![Åtkomst till parametrar i ett uttryck](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Åtkomst till parametrar i ett uttryck*

Hello samma för hello utdata för våra andra video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

och för hello ljuddata utdata:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Om vi nu ändra bithastighet hello för någon av hello video eller ljudfiler hello respektive kodaren ska konfigureras och hello bithastighet-baserat namn konventionen att användas automatiskt.

## <a id="thumbnails_to__multibitrate_MP4"></a>Lägger till miniatyrbilder toomultibitrate MP4-utdata
Från ett arbetsflöde som genererar [en multibitrate MP4-utdata från ett MXF indata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), vi kommer nu att titta på lägger till miniatyrbilder toohello utdata.

### <a id="thumbnails_to__multibitrate_MP4_overview"></a>Arbetsflödet översikt tooadd miniatyrer
![Multibitrate MP4 arbetsflöde toostart från](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4 arbetsflöde toostart från*

### <a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Lägga till JPG kodning
hello hjärtat i vår miniatyr generation blir hello JPG kodare komponenten kan toooutput JPG-filer.

![JPG-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG-kodaren*

Vi kan inte ansluta våra okomprimerade Video-ström men direkt från hello Media filen indata till hello JPG-kodaren. I stället förväntar toobe lämnas enskilda ramar. Detta kan vi kan göra via hello Video ram Gate-komponent.

![Ansluta en ram gate toohello JPG-kodaren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Ansluta en ram gate toohello JPG-kodaren*

hello ram gate när varje så många sekunder eller ramar kan en bildruta toopass. hello intervall och hello tidsförskjutningen med det här inträffar kan konfigureras i egenskaperna för hello.

![Video ram Gate-egenskaper](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Video ram Gate-egenskaper*

Nu ska vi skapa en miniatyr i minuten genom att ange hello läge tooTime (sekunder) och hello intervall too60.

### <a id="thumbnails_to__multibitrate_MP4_color_space"></a>Hantera färg utrymme konvertering
Medan verkar det logiskt både okomprimerade Video PIN-koder för hello ram-gate och hello Media filen indata kan nu vara ansluten, vi skulle få en varning om vi skulle göra.

![Indatafärgen utrymme fel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Indatafärgen utrymme fel*

Det beror på att hello sätt i vilken färg information visas i vår ursprungliga rådata okomprimerade video-ström, kommer från våra MXF skiljer sig från vilka hello JPG kodaren förväntar sig. Mer specifikt en s.k. ”färg utrymme” av ”RGB” eller ”gråskala” är förväntat tooflow i. Det innebär att hello Video ram Gates inkommande video-ström behöver toohave en konvertering som tillämpas om dess färg utrymme först.

Dra till hello arbetsflödet hello färg utrymme konverteraren - Intel och anslut den tooour ram gate.

![Ansluta en färg utrymme konverteraren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Ansluta en färg utrymme konverteraren*

Välj hello BGR 24 post hello förinställda listan i egenskapsfönstret för hello.

### <a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Skrivning hello miniatyrer
Skiljer sig från våra MP4-video, hello JPG-kodaren komponenten kommer att bli mer än en fil. I ordning toodeal med den här, en komponent som scen Sök JPG-fil skrivaren kan användas: det tar hello inkommande JPG miniatyrer och skriva ut dem varje filnamn som suffixet av ett annat nummer. (hello nummer vanligtvis som indikerar hello antal sekunder/enheter i hello dataströmmen som hello miniatyr ritades från.)

![Introduktion till hello scen Sök JPG-fil skrivare](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Introduktion till hello scen Sök JPG-fil skrivare*

Konfigurera hello utdata mappsökväg med hello uttryck: ${ROOT_outputWriteDirectory}

och hello Filename Prefix egenskap med:

    ${ROOT_sourceFileBaseName}_thumb_

hello prefix avgör hur hello miniatyr filer namnges. De kommer att med suffixet tal som anger hello par position i hello dataströmmen.

![Egenskaper för scen Sök JPG-fil Writer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Egenskaper för scen Sök JPG-fil Writer*

Ansluta hello scen Sök JPG-fil Writer toohello fil/Utdatatillgången nod.

### <a id="thumbnails_to__multibitrate_MP4_errors"></a>Identifiering av fel i ett arbetsflöde
Ansluta hello indata för hello färg utrymme konverteraren toohello raw okomprimerade videoutgång. Nu utföra lokala testa hello arbetsflödet. Det finns ett troligt hello arbetsflöde plötsligt kan avbrytas och ange med en röd ram på hello-komponent som ett fel uppstod:

![Färg utrymme konverteraren fel](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Färg utrymme konverteraren fel*

Klicka hello lite red ”E”-ikonen i hello övre högra hörnet av hello färg utrymme konverteraren komponenten toosee vad är hello orsak hello kodning försök misslyckades.

![Färg utrymme konverteraren feldialogrutan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Färg utrymme konverteraren feldialogrutan*

Det visar sig, som du kan se att hello inkommande färg utrymme standard för hello färg utrymme konverterare har toobe rec601 för våra begärda konvertering av YUV tooRGB. Vår dataströmmen anger uppenbarligen inte dess rec601. (Rek 601 är en standard för kodning sammanflätade analoga video signaler i digital video form. Det anger en aktiva regionen som täcker 720 ljusstyrkan exemplen och 360 krominans prover per rad. hello färg kodning system kallas YCbCr 4:2:2.)

toofix kan vi ska ange hello metadata för våra dataström som vi behandlar rec601 innehåll. toodo så vi använder en Video Data typen Updater-komponent som vi ska placera between våra rådata käll- och hello färg utrymme konvertering komponent. Den här data typen updater tillåter hello manuell uppdatering av vissa video data Typegenskaper. Konfigurera den tooindicate en färg utrymme Standard för ”Rec 601”. Detta innebär att hello Video Data typen Updater tootag hello dataström med hello ”Rec 601” färg utrymme om det fanns ingen färg utrymme har definierats ännu. (Inte åsidosätter eventuella befintliga metadata om hello åsidosättning var markerad.)

![Uppdatera färg utrymme Standard på hello Data typen Updater](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Uppdatera färg utrymme Standard på hello Data typen Updater*

### <a id="thumbnails_to__multibitrate_MP4_finish"></a>Klar arbetsflöde
Nu när våra våra arbetsflödet har slutförts, göra en annan testkörning toosee den skicka.

![Klar arbetsflöde för flera mp4 utdata med miniatyrbilder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Klar arbetsflöde för flera mp4 utdata med miniatyrbilder*

## <a id="time_based_trim"></a>Tidsbaserade trimning multibitrate MP4-utdata
Från ett arbetsflöde som genererar [en multibitrate MP4-utdata från ett MXF indata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), vi kommer nu att titta på trimning hello källa video baserat på tidsstämplar.

### <a id="time_based_trim_start"></a>Arbetsflödet översikt toostart lägger till trimning till
![Starta arbetsflödet tooadd trimning till](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Starta arbetsflödet tooadd trimning till*

### <a id="time_based_trim_use_stream_trimmer"></a>Med hjälp av hello dataströmmen Trimmer
hello kan dataströmmen Trimmer tootrim hello början och slutet på en indataström som baseras på tidsinformation (sekunder, minuter,...). hello trimmer stöder inte ram-baserade trimning.

![Dataströmmen Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Dataströmmen Trimmer*

I stället för att länka hello AVC kodare och talare position du toohello Media filen indata direkt, låter vi mellan dessa hello dataströmmen trimmer. (En för hello video signal och en för hello överlagrad ljud signal.)

![Placera dataströmmen Trimmer mellan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Placera dataströmmen Trimmer mellan*

Nu ska vi konfigurera hello trimmer så att vi endast bearbeta video och ljud mellan 15 sekunder och 60 sekunder i hello videon.

Gå toohello egenskaper för hello Video dataströmmen Trimmer och konfigurera både starttid (15 sek) och sluttid (60 s)-egenskaper. toomake till både våra ljud och video trimmer är alltid konfigurerad toohello samma börja och sluta värden, vi publicerar dessa toohello roten för hello arbetsflöde.

![Publicera start tid egenskap från dataströmmen Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Publicera start tid egenskap från dataströmmen Trimmer*

![Publicera egenskapsdialogrutan för starttid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Publicera egenskapsdialogrutan för starttid*

![Publicera egenskapsdialogrutan för sluttid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Publicera egenskapsdialogrutan för sluttid*

Om vi nu granska hello roten för våra arbetsflödet, blir snyggt visas och konfigureras i båda egenskaperna därifrån.

![Publicerade egenskaper som är tillgängliga på rot](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Publicerade egenskaper som är tillgängliga på rot*

Öppna hello trimning egenskaper från hello ljud trimmer och konfigurera både start- och sluttider med ett uttryck som refererar toohello publicerade egenskaper i vår arbetsflödet hello rot.

För hello ljud trimning starttid:

    ${ROOT_TrimmingStartTime}

och för dess Sluttid:

    ${ROOT_TrimmingEndTime}

### <a id="time_based_trim_finish"></a>Klar arbetsflöde
![Klar arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Klar arbetsflöde*

## <a id="scripting"></a>Introduktion till hello skripta komponent
Skriptbaserade komponenter kan köra godtycklig skript under hello körning av våra arbetsflöde. Det finns fyra olika skript som kan utföras med specifika egenskaper och deras egen plats i hello arbetsflödet livscykel:

* **commandScript**
* **realizeScript**
* **processInputScript**
* **lifeCycleScript**

hello-dokumentationen för hello går skripta komponent i detalj för varje hello ovan. I [hello följande avsnitt](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim), hello **realizeScript** scripting komponenten är används tooconstruct en cliplist xml på hello direkt när hello arbetsflöde startar. Det här skriptet anropas under hello komponenten installationsprogrammet, vilket sker bara en gång i dess livscykel.

### <a id="scripting_hello_world"></a>Skript i ett arbetsflöde: hello world
Dra en skripta komponent till hello designytan och byta namn på den (till exempel ”SetClipListXML”).

![Lägga till en skriptbaserade komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Lägga till en skriptbaserade komponent*

När du undersöker hello egenskaper för hello skripta komponent, hello fyra olika skript typer visas, varje konfigurerbara tooa olika skript.

![Egenskaperna för komponenten för skript](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Egenskaperna för komponenten för skript*

Rensa hello processInputScript och öppna hello Redigeraren för hello realizeScript. Nu vi har konfigurerat och redo toostart skript.

Skript är skrivna i Groovy, ett dynamiskt kompilerade skriptspråk för hello Java platform som behåller kompatibilitet med Java. De flesta Java-kod är faktiskt, giltig Groovy kod.

Dags att skriva skript för groovy en enkel hello world hello tillsammans med vår realizeScript. Ange följande hello i hello editor:

    node.log("hello world");

Nu ska du köra en lokal testkörning. När du kör, inspektera (via fliken hello på hello skripta komponenten) hello loggar egenskapen.

![Hello world loggutdata](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hello world loggutdata*

hello nodobjektet som vi kallar hello Loggmetod, refererar tooour aktuella ”noden” eller hello komponenten vi skript i. Varje komponent har därför hello möjlighet toooutput loggningsdata, tillgängliga via fliken hello. I det här fallet utdata vi hello stränglitteral ”hello world”. Viktiga toounderstand här är att denna funktion kan vara toobe ett ovärderligt felsökning verktyg som ger dig information om vilka hello skriptet faktiskt göra.

Från inom vårt skriptmiljö ha vi även åtkomst tooproperties för andra komponenter. Prova följande:

    //inspect current node:
    def nodepath = node.getNodePath();
    node.log("this node path: " + nodepath);

    //walking up tooother nodes:
    def parentnode = node.getParentNode();
    def parentnodepath = parentnode.getNodePath();
    node.log("parent node path: " + parentnodepath);

    //read properties from a node:
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null );
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null);
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Vår visas i loggfönstret oss hello följande:

![Utdata för att komma åt noden sökvägar](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Utdata för att komma åt noden sökvägar*

## <a id="frame_based_trim"></a>RAM-baserade trimning multibitrate MP4-utdata
Från ett arbetsflöde som genererar [en multibitrate MP4-utdata från ett MXF indata](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging), vi kommer nu att titta på trimning hello källa video baserat på ramen.

### <a id="frame_based_trim_start"></a>Utkast översikt toostart lägger till trimning till
![Arbetsflödet toostart lägger till trimning till](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Arbetsflödet toostart lägger till trimning till*

### <a id="frame_based_trim_clip_list"></a>Med hjälp av hello Clip listan XML
I alla tidigare arbetsflöde självstudier använde vi hello Media filen inkommande komponenten som våra video Indatakällan. För dessa specifika fall men ska vi använda hello Clip listan Källkomponenten i stället. Observera att det inte får vara hello önskad sätt att arbeta. endast använda hello Clip datakälla när det finns en verklig orsak toodo så (precis som i hello nedan fall där vi gör användningen av hello clip listan trimning funktioner).

tooswitch från våra Media filen indata toohello Clip datakälla, dra hello Clip listan Källkomponenten till hello designytan och koppla hello Clip listan XML PIN-kod toohello Clip listan XML-noden av hello workflow designer. Detta ska fylla hello Clip datakälla med utdata PIN-koder, enligt tooour indatavideo. Ansluta nu hello okomprimerade Video- och okomprimerade PIN-koder hello hello Clip datakälla toohello respektive AVC kodare och ljud dataströmmen Interleaver. Nu ska du ta bort hello Media filen indata.

![Ersatt hello Media filen indata med hello Clip datakälla](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Ersatt hello Media filen indata med hello Clip datakälla*

hello Clip listan Källkomponenten tar som indata en ”Clip listan XML”. När du väljer hello källa filen tootest med lokalt, är den här clip listan xml fylls automatiskt åt dig.

![Automatisk fyllts Clip listan XML-egenskapen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatisk fyllts Clip listan XML-egenskapen*

Ser lite närmare toohello xml gör är detta hur det ser ut:

![Clip Listdialogrutan redigera](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Clip Listdialogrutan redigera*

Detta men avspeglar inte hello funktioner hello clip listan XML. Vi kan tooadd elementet ”Rensa” under både hello video och ljud källa, så här:

![Lägga till en trim toohello clip elementlistan](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Lägga till en trim toohello clip elementlistan*

Om du ändrar hello clip listan xml så här ovan och utföra lokala testa ser du hello video korrekt tagits bort mellan 10 och 20 sekunder i hello videon.

Motstridiga toowhat händer när du gör en lokal körning, även om den här samma cliplist xml inte skulle ha hello samma effekt när det används i ett arbetsflöde som körs i Azure Media Services. När Azure Premium-kodare startar, hello cliplist xml genereras varje gång igen, baserat på hello indatafilen hello kodning gavs jobb. Det innebär att alla ändringar som vi gör hello XML-tyvärr skulle åsidosättas.

toocounter hello cliplist xml som rensas när ett kodningsjobb startas vi kan generera den på hello direkt precis efter hello början av våra arbetsflöde. Dessa anpassade åtgärder kan hämtas via något som kallas en ”Skriptfönster Component”. Mer information finns i [introduktion till hello skripta komponenten](media-services-media-encoder-premium-workflow-tutorials.md#scripting).

Dra en skripta komponent till hello designytan och byta namn på den för ”SetClipListXML”.

![Lägga till en skriptbaserade komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Lägga till en skriptbaserade komponent*

När du undersöker hello egenskaper för hello skripta komponent, hello fyra olika skript typer visas, varje konfigurerbara tooa olika skript.

![Egenskaperna för komponenten för skript](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Egenskaperna för komponenten för skript*

### <a id="frame_based_trim_modify_clip_list"></a>När du ändrar hello clip listan från en skript-komponent
Innan vi kan Skriv hello cliplist xml som genereras under starten av arbetsflödet, behöver vi toohave åtkomst toohello cliplist XML-egenskapen och innehållet. Vi kan göra det så här:

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Inkommande clip lista som loggas](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Inkommande clip lista som loggas*

Vi måste först en sätt toodetermine från vilken plats till vilken plats som vi vill tootrim hello video. toomake lämplig toohello tekniskt användaren av hello arbetsflöde publicera två egenskaper toohello rot hello diagrammet. toodo, högerklicka på hello designytan och välj ”Lägg till egenskap”:

* Första egenskapen: ”ClippingTimeStart” av typen: ”TIDSKOD”
* Andra egenskapen: ”ClippingTimeEnd” av typen: ”TIDSKOD”

![Lägg till dialogrutan för egenskaper för urklippet starttid](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Lägg till dialogrutan för egenskaper för urklippet starttid*

![Publicerade urklippet tid sammanställer i rot-arbetsflöde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Publicerade urklippet tid sammanställer i rot-arbetsflöde*

Konfigurera båda egenskaperna tooa passande värde:

![Konfigurera hello urklippet start- och slutdatum egenskaper](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Konfigurera hello urklippet start- och slutdatum egenskaper*

Nu från i vår skript vi kan komma åt båda egenskaperna så här:

    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Log-fönster med början och slutet av Urklipp](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Log-fönster med början och slutet av Urklipp*

Vi parsa hello tidskod strängar till en enklare toouse i form av ett enkelt reguljära uttryck:

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);
    node.log("framerate end is: " + endframerate);

![Fönstret med utdata från parsade tidskod](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Fönstret med utdata från parsade tidskod*

Vi kan nu ändra hello cliplist xml tooreflect hello start och sluttider för hello önskad ram korrekt urklippet av hello film med den här vi information.

![Skriptet tooadd trim kodelement](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Skriptet tooadd trim kodelement*

Detta gjordes via normala sträng manipulering åtgärder. hello resulterande ändrade clip listan xml skrivs tillbaka toohello clipListXML egenskapen hello arbetsflödet roten hello ”setProperty” med metoden. hello log-fönstret när du kör ett annat test skulle visar hello följande:

![Loggning hello clip lista](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Loggning hello clip lista*

Gör en testkörning toosee hur klipps hello video och ljud dataströmmar. När du ska göra mer än en testkörning med olika värden för hello trimning punkter, Lägg märke till att de inte kommer beaktas men! hello anledningen är att hello designer, till skillnad från hello Azure runtime, har inte åsidosättning hello cliplist xml var kör. Det innebär att endast hello första gången som du har angett hello in och ut punkter, resulterar hello xml tootransform, alla hello andra tider våra guard-satsen (om (clipListXML.indexOf (”<trim>”) == -1)) förhindrar att hello-arbetsflödet lägger till en ny trim elementet när det redan finns en.

toomake våra arbetsflödet praktiskt tootest lokalt, vi bästa lägga till dagliga rutiner kod som kontrollerar om en trim elementet fanns redan. I så fall, kan vi ta bort den innan du fortsätter genom att ändra hello xml med hello nya värden. I stället för vanliga strängändringar, är det förmodligen säkrare toodo genom verkliga XML-objekt modellen parsning.

Innan vi kan dock lägga till sådan kod, behöver vi tooadd ett antal importuttryck längst hello början av våra skriptet först:

    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

Sedan kan vi lägga till hello krävs Rensa kod:

    //for local testing: delete any pre-existing trim elements from hello clip list xml by parsing hello xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
     //delete hello trim nodes:
    elementsToDelete.each{
        e -> e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

Den här koden går ovanför hello punkt där vi lägger till hello trim element toohello cliplist xml.

Vi kan nu köra och ändra våra arbetsflödet så mycket som vi vill samtidigt som du har ändringar hello någonsin tid.    

### <a id="frame_based_trim_clippingenabled_prop"></a>Lägga till en ClippingEnabled bekvämlighet egenskap
Du kanske inte alltid vill trimning toohappen, Slutför vi av våra arbetsflöde genom att lägga till en lämplig boolesk flagga som anger om vi vill tooenable trimning / urklippet.

Publicera en ny egenskap toohello rot för våra arbetsflöde kallas ”ClippingEnabled” av typen ”BOOLESKT” före, precis som.

![Publicerade en egenskap för att aktivera Urklipp](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Publicerade en egenskap för att aktivera Urklipp*

Med hello nedan enkla guard-satsen, vi kontrollera om trimning krävs och bestämma om våra clip listan som sådana måste toobe ändras eller inte.

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }


### <a id="code"></a>Fullständiga koden
    import javax.xml.parsers.*;
    import org.xml.sax.*;
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*;
    import javax.xml.transform.*;
    import javax.xml.transform.stream.*;
    import javax.xml.transform.dom.*;

    // get cliplist xml:
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();

    //parse hello start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart);
    startregresult.matches();
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);

    //parse hello end timing:
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches();
    def endtimecode = endregresult.group(1);
    node.log("timecode end is: " + endtimecode);
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);

    //for local testing: delete any pre-existing trim elements
    //from hello clip list xml by parsing hello xml into a DOM:

    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML));
    Document dom=builder.parse(is);

    //find hello trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim";
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection
    Set<Element> elementsToDelete = new HashSet<Element>();
    for (int i = 0; i < trimelems.getLength(); i++) {
        Element e = (Element)trimelems.item(i);
        elementsToDelete.add(e);
    }

    node.log("about toodelete any existing trim nodes");
    //delete hello trim nodes:
    elementsToDelete.each{ e ->
        e.getParentNode().removeChild(e);
    };
    node.log("deleted any existing trim nodes");

    //serialize hello modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString());
    if(clippingrequired == null || clippingrequired == false)
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return;
    }

    //add trim elements toocliplist xml
    if ( clipListXML.indexOf("<trim>") == -1 )
    {
        //trim video
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode +
            " </outPoint>\n </trim> \n");
        //trim audio
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+
            startframerate +"\">" + starttimecode +
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" +
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML );
        node.setProperty("../clipListXml",clipListXML);
    }


## <a name="also-see"></a>Se även
[Introduktion till Premium-kodning i Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Hur tooUse Premium kodning i Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Koda innehåll på begäran med Azure Media Service](media-services-encode-asset.md#media-encoder-premium-workflow)

[Arbetsflödesformat och codecs för Media Encoder Premium](media-services-premium-workflow-encoder-formats.md)

[Exempelfiler för arbetsflöde](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Explorer-verktyget](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
