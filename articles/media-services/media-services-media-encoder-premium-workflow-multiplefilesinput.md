---
title: "aaaMultiple inkommande filer och egenskaperna för komponenten med Premium-kodare - Azure | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toouse setRuntimeProperties toouse indata från flera filer och skicka medieprocessor för anpassade data toohello Media Encoder Premium arbetsflöde."
services: media-services
documentationcenter: 
author: xpouyat
manager: cfowler
editor: 
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: xpouyat;anilmur;juliako
ms.openlocfilehash: e14d10fbf9669e0b88e5ba1c519f1ba5e0bafdd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Använda flera indatafiler och egenskaperna för komponenten med Premium-kodare
## <a name="overview"></a>Översikt
Det finns scenarier där du kan behöva toocustomize komponentegenskaper ange Clip listan XML-innehåll eller skicka flera indatafiler när du skickar en uppgift med hello **Media Encoder Premium arbetsflöde** medieprocessor. Några exempel är:

* Ovanpå text på video och ange hello textvärde (till exempel hello aktuellt datum) vid körning för varje inkommande video.
* Anpassa hello Clip listan XML (toospecify en eller flera källfiler, med eller utan trimning osv.).
* Ovanpå en logotyp på hello indatavideo medan kodas hello video.
* Flera språk för ljud-kodning.

toolet hello **Media Encoder Premium arbetsflöde** vet att du ändrar vissa egenskaper i hello arbetsflödet när du skapar hello uppgift eller skicka flera indatafiler har du toouse konfigurationssträngen som innehåller  **setRuntimeProperties** och/eller **transcodeSource**. Det här avsnittet beskrivs hur toouse dem.

## <a name="configuration-string-syntax"></a>Strängsyntaxen konfiguration
hello configuration sträng tooset i hello kodning aktiviteten använder ett XML-dokument som ser ut så här:

```xml
<?xml version="1.0" encoding="utf-8"?>
<transcodeRequest>
  <transcodeSource>
  </transcodeSource>
  <setRuntimeProperties>
    <property propertyPath="Media File Input/filename" value="VideoFileName.mp4" />
  </setRuntimeProperties>
</transcodeRequest>
```

hello följer hello C#-kod som läser hello XML-konfigurationen från en fil genom att uppdatera den med hello höger video filnamn och skickar den toohello aktivitet i ett jobb:

```c#
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass tooit hello name of hello 
// processor toouse for hello specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with hello encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify hello input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset toocontain hello results of hello job. 
// This output is specified as AssetCreationOptions.None, which 
// means hello output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a>Anpassa egenskaperna för komponenten
### <a name="property-with-a-simple-value"></a>Egenskapen med ett enkelt värde
I vissa fall är det användbart toocustomize en komponent egenskap tillsammans med hello-arbetsflödesfil som ska toobe som utförs av Media Encoder Premium arbetsflöde.

Anta att du skapat ett arbetsflöde överlägg texten på videor och hello texten (till exempel hello aktuellt datum) ska toobe set vid körning. Du kan göra detta genom att skicka hello text toobe ange hello nytt värde för hello textegenskap hello överlägget komponenten från hello kodning aktivitet. Du kan använda den här mekanismen toochange andra egenskaper för en komponent i hello arbetsflöde (till exempel hello position eller färgen på hello överlägget hello bithastighet hello AVC-kodaren osv.).

**setRuntimeProperties** är används toooverride en egenskap i hello komponenter i hello arbetsflöde.

Exempel:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text tooImage Converter/text" value="Today is Friday hello 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a>Egenskapen med ett XML-värde
tooset en egenskap som förväntar sig ett XML-värde som kapslar in med hjälp av `<![CDATA[ and ]]>`.

Exempel:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

> [!NOTE]
> Kontrollera att inte tooput radmatning returnera precis efter `<![CDATA[`.

### <a name="propertypath-value"></a>propertyPath-värde
I föregående exempel hello hello propertyPath var ”/ mediafilen indata/filename” eller ”/ inactiveTimeout” eller ”clipListXml”.
Detta är i allmänhet hello namnet på hello komponenten och hello namnet på hello-egenskap. hello sökvägen kan innehålla fler eller färre nivåer, som ”/ primarySourceFile” (eftersom hello-egenskapen är hello roten i hello arbetsflöde) eller ”/ Video bearbetning/bild överlägget/opacitet” (eftersom hello överlägget är i en grupp).    

toocheck hello sökväg och egenskapen namn, Använd hello åtgärdsknappen som är direkt bredvid varje egenskap. Du kan klicka på åtgärdsknappen och välja **redigera**. Detta visar du hello faktiska namn för hello-egenskapen och omedelbart ovanför, hello namnområde.

![Åtgärden och redigera](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Egenskap](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Flera inkommande filer
Varje aktivitet som du skickar toohello **Media Encoder Premium arbetsflöde** kräver två tillgångar:

* hello först en är en *arbetsflöde tillgången* som innehåller ett arbetsflödesfil. Du kan utforma Arbetsflödesfiler med hjälp av hello [Arbetsflödesdesignern](media-services-workflow-designer.md).
* hello andra är en *Media tillgången* som innehåller hello media fil(er) som du vill tooencode.

När du skickar flera media filer toohello **Media Encoder Premium arbetsflöde** kodare, hello följande villkor gäller:

* Alla hello media filer måste vara i hello samma *Media tillgången*. Flera mediaresurser stöds inte.
* Måste du ange hello primärfilen i tillgången Media (Vi rekommenderar detta är hello huvudsakliga videofil som hello kodare blir ombedd tooprocess).
* Det är nödvändigt toopass konfigurationsdata som innehåller hello **setRuntimeProperties** och/eller **transcodeSource** elementet toohello processor.
  * **setRuntimeProperties** används toooverride hello filename-egenskapen eller en annan egenskap i hello komponenter i hello arbetsflöde.
  * **transcodeSource** är används toospecify hello Clip listan XML-innehåll.

Anslutningar i hello arbetsflöde:

* Om du använder en eller flera Media filen indata komponenter och planera toouse **setRuntimeProperties** toospecify hello filnamn och Anslut inte hello primärfilen komponenten PIN-kod toothem. Kontrollera att det finns ingen anslutning mellan hello primärfilen objekt och hello Media filen Input(s).
* Om du föredrar toouse Clip listan XML och en mediekälla komponent kan sedan du ansluta båda tillsammans.

![Ingen anslutning från primära källfilen tooMedia filen indata](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Det finns ingen anslutning från hello primärfilen tooMedia filen indata komponenterna om du använder setRuntimeProperties tooset hello filename-egenskapen.*

![Anslutningen från Clip listan XML tooClip datakälla](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Du kan ansluta Clip listan XML tooMedia källa och använder transcodeSource.*

### <a name="clip-list-xml-customization"></a>Beskär listan XML-anpassning
Du kan ange hello Clip listan XML i hello arbetsflöde vid körning med hjälp av **transcodeSource** string XML i hello konfiguration. Hello Clip listan XML PIN-kod toobe anslutna toohello mediekälla komponent i hello arbetsflöde krävs.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <transcodeSource>
      <clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
      </clipList>
    </transcodeSource>
    <setRuntimeProperties>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Om du vill toospecify /primarySourceFile toouse den här egenskapen tooname hello utdatafiler med hjälp av 'Uttryck', så rekommenderar vi skickar hello Clip listan XML som en egenskap *när* hello /primarySourceFile egenskapen, tooavoid med hello åsidosättas klipp av hello /primarySourceFile inställning.

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Med ytterligare ram exakt hur:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

## <a name="example-1--overlay-an-image-on-top-of-hello-video"></a>Exempel 1: Överlagra en bild ovanpå hello video

### <a name="presentation"></a>Presentation
Överväg ett exempel där du vill toooverlay en logotyp på hello indatavideo medan kodas hello video. I det här exemplet hello indatavideo har namnet ”Microsoft_HoloLens_Possibilities_816p24.mp4” och hello logotypen heter ”logo.png”. Du bör utföra hello följande steg:

* Skapa ett arbetsflöde tillgång med hello-arbetsflödesfil (se följande exempel hello).
* Skapa en tillgång med Media, som innehåller två filer: MyInputVideo.mp4 som hello primära fil- och MyLogo.png.
* Skicka en aktivitet toohello Media Encoder Premium arbetsflödet media-processor med hello ovan indata tillgångar och ange hello följande konfigurationssträngen.

Konfiguration:

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

I hello-exemplet ovan skickas hello namnet på hello videofil toohello Media filen Input-komponenten och hello primarySourceFile-egenskapen. hello namnet på hello logotypfilen skickas tooanother Media filen indata som är anslutna toohello grafiska överlägget komponent.

> [!NOTE]
> hello videofil namn skickas toohello primarySourceFile egenskapen. Hej anledningen är toouse den här egenskapen i hello arbetsflöde för att skapa med uttryck, till exempel hello-rätt utdata-filnamn.

### <a name="step-by-step-workflow-creation"></a>Steg för steg-arbetsflödet skapats
Här följer hello steg toocreate ett arbetsflöde som använder två filer som indata: en video och en bild. Det kommer täcker hello bilden ovanpå hello video.

Öppna **Arbetsflödesdesignern** och välj **filen** > **ny arbetsyta** > **Omkoda modell**.

arbetsflöde för ny hello visar tre element:

* Primär källfilen
* Beskär List-XML
* Filen/Utdatatillgången  

![Arbetsflöde för ny kodning](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Arbetsflöde för ny kodning*

Börja med att lägga till en Media filen inkommande komponent i ordning tooaccept hello mediet-filen. tooadd ett komponenten toohello arbetsflöde, leta efter den i sökrutan för hello databasen och dra hello önskad post till hello designern rutan.

Lägg till hello videofil toobe används för att utforma ditt arbetsflöde. toodo så klicka hello bakgrundspanelen i Arbetsflödesdesignern och leta efter hello primära källfilen egenskapen på hello egenskapen högra rutan. Klicka på ikonen för hello-mapp och välj hello lämpliga videofil.

![Primär källa](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primär källa*

Ange därefter hello videofil i hello Media filen ingång komponent.   

![Indatakällan för mediefiler](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Indatakällan för mediefiler*

När det är klart hello Media filen indata komponenten inspektera hello-filen och fylla i sina PIN-koder tooreflect hello utdatafiler som det kontrolleras.

hello nästa steg är tooadd utrymme tooRec.709 en ”Video Data typen Updater” toospecify hello färg. Lägg till en ”Video konverteraren” som har angetts tooData Layout/Layout typ = konfigurerbara Planar. Detta omvandlar hello video-ström tooa format som kan räknas som en källa för hello överlägget komponent.

![video Data typen Updater och konverteraren](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video Data typen Updater och konverteraren*

![Typ av layout = konfigurerbara Planar](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Typ av layout är konfigurerbar Planar*

Sedan lägger till en Video överlägg komponent och ansluta hello (okomprimerad) video PIN-kod toohello (okomprimerad) video PIN-koden för hello media filen indata.

Lägga till en annan Media filen indata (tooload hello logotyp-fil), klickar du på den här komponenten och byta namn på den för ”Media filen indata logotyp” och välj en bild (en PNG-fil till exempel) i egenskapen hello. Ansluta hello okomprimerade bilden PIN-kod toohello okomprimerade bilden PIN-koden för hello överlägget.

![Källa för överlägget komponenten och avbildning](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Källa för överlägget komponenten och avbildning*

Om du vill toomodify hello position hello logotyp på hello video (till exempel du kanske vill tooposition den på 10 procent från hello övre vänstra hörnet av hello video) avmarkerar kryssrutan för hello ”manuell inmatning”. Du kan göra detta eftersom du använder en Media filen indata tooprovide hello logotypen filen toohello överlägget komponent.

![Överlägget position](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Överlägget position*

tooencode Hej video-ström tooH.264, lägga till hello AVC-videokodare och AAC kodare komponenter toohello designytan. Ansluta hello PIN-koder.
Ställ in hello AAC kodare och välj ljud Format konvertering/förinställda: 2.0 (L, R).

![Ljud och Video kodare](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Ljud och Video kodare*

Lägg nu till hello **ISO Mpeg-4 multiplexor** och **utdata** komponenter och ansluta hello PIN-koder som visas.

![MP4 multiplexor och utdata](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexor och utdata*

Du måste tooset hello namn för hello utdatafilen. Klicka på hello **utdata** komponent och redigera hello-uttrycket för hello-filen:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Filnamnet för utdata](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Filnamnet för utdata*

Du kan köra hello arbetsflödet lokalt toocheck körs korrekt.

När den är klar kan du köra det i Azure Media Services.

Förbereda en tillgång i Azure Media Services med två filer: hello videofil och hello-logotypen. Du kan göra detta med hjälp av hello .NET eller REST API. Du kan också göra detta med hjälp av hello Azure-portalen eller [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

De här självstudierna visar hur toomanage tillgångar med AMSE. Det finns två sätt tooadd filer tooan tillgångsinformation:

* Skapa en lokal mapp, kopiera hello två filer, och dra och släppa hello mappen toohello **tillgången** fliken.
* Överföra hello videofil som en tillgång, visa hello tillgångsinformation, gå toohello på fliken filer och ladda upp en fil (logotypen).

> [!NOTE]
> Gör att tooset en primär fil i hello tillgången (hello video huvudfilen).

![Tillgångsfiler i AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Tillgångsfiler i AMSE*

Välj hello tillgång och välj tooencode med Premium-kodare. Överför hello arbetsflödet och markera den.

Klicka på hello knappen toopass data toohello processor och Lägg till hello följande XML-tooset hello runtime egenskaper:

![Premium-kodare i AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium-kodare i AMSE*

Klistra sedan in hello följande XML-data. Du behöver toospecify hello namnet på hello videofil för både hello Media filen indata och primarySourceFile. Ange hello namn hello filnamn för hello logotypen för.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*

Om du använder hello .NET SDK toocreate och köra hello aktivitet har toobe skickas som hello konfigurationssträngen XML-data.

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

När hello jobbet är klart utdata hello MP4-fil i hello tillgångsinformation visar hello överlägget!

![Överlägg på hello video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Överlägg på hello video*

Du kan hämta hello Exempelarbetsflöde från [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).

## <a name="example-2--multiple-audio-language-encoding"></a>Exempel 2: Flera ljud språkkod

Ett exempel på flera språk för ljud kodning workfkow finns i [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).

Den här mappen innehåller ett Exempelarbetsflöde som kan vara används tooencode en MXF filen tooa multi MP4-filer tillgång med flera ljud spår.

Det här arbetsflödet förutsätter hello MXF filen innehåller ett ljud spår; hello ytterligare ljud spårar ska skickas som separata ljudfiler (WAV eller MP4...).

tooencode, följer du dessa anvisningar:

* Skapa en tillgång med Media Services med hello MXF fil- och hello ljud-filer (0 too18 ljud).
* Kontrollera att filen hello MXF har angetts som en primär fil.
* Skapa ett jobb och en aktivitet med hello Premium-kodare arbetsflödet processor. Använd hello arbetsflödet som anges (MultiMP4-1080p-19audio-v1.workflow).
* Skicka hello setruntime.xml data toohello aktivitet (om du använder Azure Media Services Explorer, använda hello ”skicka toohello arbetsflödet för XML-data” knappen).
  * Uppdatera hello XML-data toospecify hello rätt fil namn och språk taggar.
  * hello arbetsflöde har ljud komponenter med namnet ljud 1 tooAudio 18.
  * RFC5646 stöds för hello språk taggen.

```xml
<?xml version="1.0" encoding="utf-16"?>
<transcodeRequest>
  <setRuntimeProperties>
    <property propertyPath="Media File Input Video/filename" value="MainVideo.mxf" />
    <property propertyPath="Language/language_code" value="en" />
    <property propertyPath="/primarySourceFile" value="MainVideo.mxf" />
    <property propertyPath="Audio 1/Media File Input/filename" value="french-audio.wav" />
    <property propertyPath="Audio 1/Language/language_code" value="fr" />
    <property propertyPath="Audio 2/Media File Input/filename" value="german-audio.wav" />
    <property propertyPath="Audio 2/Language/language_code" value="de" />
    <property propertyPath="Audio 3/Media File Input/filename" value="japanese-audio.wav" />
    <property propertyPath="Audio 3/Language/language_code" value="ja" />
  </setRuntimeProperties>
</transcodeRequest>
```

* hello kodade tillgången innehåller flera språk ljud spårar och spåren ska väljas i Azure Media Player.

## <a name="see-also"></a>Se även
* [Introduktion till Premium-kodning i Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Hur toouse Premium kodning i Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [Koda innehåll på begäran med Azure Media Services](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Arbetsflöde för Media Encoder Premium format och codec](media-services-premium-workflow-encoder-formats.md)
* [Exempelfiler för arbetsflöde](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [Azure Media Services Explorer-verktyget](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
