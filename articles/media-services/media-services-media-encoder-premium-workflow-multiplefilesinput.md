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
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a><span data-ttu-id="afeec-103">Använda flera indatafiler och egenskaperna för komponenten med Premium-kodare</span><span class="sxs-lookup"><span data-stu-id="afeec-103">Using multiple input files and component properties with Premium Encoder</span></span>
## <a name="overview"></a><span data-ttu-id="afeec-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="afeec-104">Overview</span></span>
<span data-ttu-id="afeec-105">Det finns scenarier där du kan behöva toocustomize komponentegenskaper ange Clip listan XML-innehåll eller skicka flera indatafiler när du skickar en uppgift med hello **Media Encoder Premium arbetsflöde** medieprocessor.</span><span class="sxs-lookup"><span data-stu-id="afeec-105">There are scenarios in which you might need toocustomize component properties, specify Clip List XML content, or send multiple input files when you submit a task with hello **Media Encoder Premium Workflow** media processor.</span></span> <span data-ttu-id="afeec-106">Några exempel är:</span><span class="sxs-lookup"><span data-stu-id="afeec-106">Some examples are:</span></span>

* <span data-ttu-id="afeec-107">Ovanpå text på video och ange hello textvärde (till exempel hello aktuellt datum) vid körning för varje inkommande video.</span><span class="sxs-lookup"><span data-stu-id="afeec-107">Overlaying text on video and setting hello text value (for example, hello current date) at runtime for each input video.</span></span>
* <span data-ttu-id="afeec-108">Anpassa hello Clip listan XML (toospecify en eller flera källfiler, med eller utan trimning osv.).</span><span class="sxs-lookup"><span data-stu-id="afeec-108">Customizing hello Clip List XML (toospecify one or several source files, with or without trimming, etc.).</span></span>
* <span data-ttu-id="afeec-109">Ovanpå en logotyp på hello indatavideo medan kodas hello video.</span><span class="sxs-lookup"><span data-stu-id="afeec-109">Overlaying a logo image on hello input video while hello video is encoded.</span></span>
* <span data-ttu-id="afeec-110">Flera språk för ljud-kodning.</span><span class="sxs-lookup"><span data-stu-id="afeec-110">Multiple audio language encoding.</span></span>

<span data-ttu-id="afeec-111">toolet hello **Media Encoder Premium arbetsflöde** vet att du ändrar vissa egenskaper i hello arbetsflödet när du skapar hello uppgift eller skicka flera indatafiler har du toouse konfigurationssträngen som innehåller  **setRuntimeProperties** och/eller **transcodeSource**.</span><span class="sxs-lookup"><span data-stu-id="afeec-111">toolet hello **Media Encoder Premium Workflow** know that you are changing some properties in hello workflow when you create hello task or send multiple input files, you have toouse a configuration string that contains **setRuntimeProperties** and/or **transcodeSource**.</span></span> <span data-ttu-id="afeec-112">Det här avsnittet beskrivs hur toouse dem.</span><span class="sxs-lookup"><span data-stu-id="afeec-112">This topic explains how toouse them.</span></span>

## <a name="configuration-string-syntax"></a><span data-ttu-id="afeec-113">Strängsyntaxen konfiguration</span><span class="sxs-lookup"><span data-stu-id="afeec-113">Configuration string syntax</span></span>
<span data-ttu-id="afeec-114">hello configuration sträng tooset i hello kodning aktiviteten använder ett XML-dokument som ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="afeec-114">hello configuration string tooset in hello encoding task uses an XML document that looks like this:</span></span>

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

<span data-ttu-id="afeec-115">hello följer hello C#-kod som läser hello XML-konfigurationen från en fil genom att uppdatera den med hello höger video filnamn och skickar den toohello aktivitet i ett jobb:</span><span class="sxs-lookup"><span data-stu-id="afeec-115">hello following is hello C# code that reads hello XML configuration from a file, update it with hello right video filename and passes it toohello task in a job:</span></span>

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

## <a name="customizing-component-properties"></a><span data-ttu-id="afeec-116">Anpassa egenskaperna för komponenten</span><span class="sxs-lookup"><span data-stu-id="afeec-116">Customizing component properties</span></span>
### <a name="property-with-a-simple-value"></a><span data-ttu-id="afeec-117">Egenskapen med ett enkelt värde</span><span class="sxs-lookup"><span data-stu-id="afeec-117">Property with a simple value</span></span>
<span data-ttu-id="afeec-118">I vissa fall är det användbart toocustomize en komponent egenskap tillsammans med hello-arbetsflödesfil som ska toobe som utförs av Media Encoder Premium arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="afeec-118">In some cases, it is useful toocustomize a component property together with hello workflow file that is going toobe executed by Media Encoder Premium Workflow.</span></span>

<span data-ttu-id="afeec-119">Anta att du skapat ett arbetsflöde överlägg texten på videor och hello texten (till exempel hello aktuellt datum) ska toobe set vid körning.</span><span class="sxs-lookup"><span data-stu-id="afeec-119">Suppose you designed a workflow that overlays text on your videos, and hello text (for example, hello current date) is supposed toobe set at runtime.</span></span> <span data-ttu-id="afeec-120">Du kan göra detta genom att skicka hello text toobe ange hello nytt värde för hello textegenskap hello överlägget komponenten från hello kodning aktivitet.</span><span class="sxs-lookup"><span data-stu-id="afeec-120">You can do this by sending hello text toobe set as hello new value for hello text property of hello overlay component from hello encoding task.</span></span> <span data-ttu-id="afeec-121">Du kan använda den här mekanismen toochange andra egenskaper för en komponent i hello arbetsflöde (till exempel hello position eller färgen på hello överlägget hello bithastighet hello AVC-kodaren osv.).</span><span class="sxs-lookup"><span data-stu-id="afeec-121">You can use this mechanism toochange other properties of a component in hello workflow (such as hello position or color of hello overlay, hello bitrate of hello AVC encoder, etc.).</span></span>

<span data-ttu-id="afeec-122">**setRuntimeProperties** är används toooverride en egenskap i hello komponenter i hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="afeec-122">**setRuntimeProperties** is used toooverride a property in hello components of hello workflow.</span></span>

<span data-ttu-id="afeec-123">Exempel:</span><span class="sxs-lookup"><span data-stu-id="afeec-123">Example:</span></span>

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

### <a name="property-with-an-xml-value"></a><span data-ttu-id="afeec-124">Egenskapen med ett XML-värde</span><span class="sxs-lookup"><span data-stu-id="afeec-124">Property with an XML value</span></span>
<span data-ttu-id="afeec-125">tooset en egenskap som förväntar sig ett XML-värde som kapslar in med hjälp av `<![CDATA[ and ]]>`.</span><span class="sxs-lookup"><span data-stu-id="afeec-125">tooset a property that expects an XML value, encapsulate by using `<![CDATA[ and ]]>`.</span></span>

<span data-ttu-id="afeec-126">Exempel:</span><span class="sxs-lookup"><span data-stu-id="afeec-126">Example:</span></span>

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
> <span data-ttu-id="afeec-127">Kontrollera att inte tooput radmatning returnera precis efter `<![CDATA[`.</span><span class="sxs-lookup"><span data-stu-id="afeec-127">Make sure not tooput a carriage return just after `<![CDATA[`.</span></span>

### <a name="propertypath-value"></a><span data-ttu-id="afeec-128">propertyPath-värde</span><span class="sxs-lookup"><span data-stu-id="afeec-128">propertyPath value</span></span>
<span data-ttu-id="afeec-129">I föregående exempel hello hello propertyPath var ”/ mediafilen indata/filename” eller ”/ inactiveTimeout” eller ”clipListXml”.</span><span class="sxs-lookup"><span data-stu-id="afeec-129">In hello previous examples, hello propertyPath was "/Media File Input/filename" or "/inactiveTimeout" or "clipListXml".</span></span>
<span data-ttu-id="afeec-130">Detta är i allmänhet hello namnet på hello komponenten och hello namnet på hello-egenskap.</span><span class="sxs-lookup"><span data-stu-id="afeec-130">This is, in general, hello name of hello component, then hello name of hello property.</span></span> <span data-ttu-id="afeec-131">hello sökvägen kan innehålla fler eller färre nivåer, som ”/ primarySourceFile” (eftersom hello-egenskapen är hello roten i hello arbetsflöde) eller ”/ Video bearbetning/bild överlägget/opacitet” (eftersom hello överlägget är i en grupp).</span><span class="sxs-lookup"><span data-stu-id="afeec-131">hello path can have more or fewer levels, like "/primarySourceFile" (because hello property is at hello root of hello workflow) or "/Video Processing/Graphic Overlay/Opacity" (because hello Overlay is in a group).</span></span>    

<span data-ttu-id="afeec-132">toocheck hello sökväg och egenskapen namn, Använd hello åtgärdsknappen som är direkt bredvid varje egenskap.</span><span class="sxs-lookup"><span data-stu-id="afeec-132">toocheck hello path and property name, use hello action button that is immediately beside each property.</span></span> <span data-ttu-id="afeec-133">Du kan klicka på åtgärdsknappen och välja **redigera**.</span><span class="sxs-lookup"><span data-stu-id="afeec-133">You can click this action button and select **Edit**.</span></span> <span data-ttu-id="afeec-134">Detta visar du hello faktiska namn för hello-egenskapen och omedelbart ovanför, hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="afeec-134">This will show you hello actual name of hello property, and immediately above it, hello namespace.</span></span>

![Åtgärden och redigera](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Egenskap](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a><span data-ttu-id="afeec-137">Flera inkommande filer</span><span class="sxs-lookup"><span data-stu-id="afeec-137">Multiple input files</span></span>
<span data-ttu-id="afeec-138">Varje aktivitet som du skickar toohello **Media Encoder Premium arbetsflöde** kräver två tillgångar:</span><span class="sxs-lookup"><span data-stu-id="afeec-138">Each task that you submit toohello **Media Encoder Premium Workflow** requires two assets:</span></span>

* <span data-ttu-id="afeec-139">hello först en är en *arbetsflöde tillgången* som innehåller ett arbetsflödesfil.</span><span class="sxs-lookup"><span data-stu-id="afeec-139">hello first one is a *Workflow Asset* that contains a workflow file.</span></span> <span data-ttu-id="afeec-140">Du kan utforma Arbetsflödesfiler med hjälp av hello [Arbetsflödesdesignern](media-services-workflow-designer.md).</span><span class="sxs-lookup"><span data-stu-id="afeec-140">You can design workflow files by using hello [Workflow Designer](media-services-workflow-designer.md).</span></span>
* <span data-ttu-id="afeec-141">hello andra är en *Media tillgången* som innehåller hello media fil(er) som du vill tooencode.</span><span class="sxs-lookup"><span data-stu-id="afeec-141">hello second one is a *Media Asset* that contains hello media file(s) that you want tooencode.</span></span>

<span data-ttu-id="afeec-142">När du skickar flera media filer toohello **Media Encoder Premium arbetsflöde** kodare, hello följande villkor gäller:</span><span class="sxs-lookup"><span data-stu-id="afeec-142">When you're sending multiple media files toohello **Media Encoder Premium Workflow** encoder, hello following constraints apply:</span></span>

* <span data-ttu-id="afeec-143">Alla hello media filer måste vara i hello samma *Media tillgången*.</span><span class="sxs-lookup"><span data-stu-id="afeec-143">All hello media files must be in hello same *Media Asset*.</span></span> <span data-ttu-id="afeec-144">Flera mediaresurser stöds inte.</span><span class="sxs-lookup"><span data-stu-id="afeec-144">Using multiple Media Assets is not supported.</span></span>
* <span data-ttu-id="afeec-145">Måste du ange hello primärfilen i tillgången Media (Vi rekommenderar detta är hello huvudsakliga videofil som hello kodare blir ombedd tooprocess).</span><span class="sxs-lookup"><span data-stu-id="afeec-145">You must set hello primary file in this Media Asset (ideally, this is hello main video file that hello encoder is asked tooprocess).</span></span>
* <span data-ttu-id="afeec-146">Det är nödvändigt toopass konfigurationsdata som innehåller hello **setRuntimeProperties** och/eller **transcodeSource** elementet toohello processor.</span><span class="sxs-lookup"><span data-stu-id="afeec-146">It is necessary toopass configuration data that includes hello **setRuntimeProperties** and/or **transcodeSource** element toohello processor.</span></span>
  * <span data-ttu-id="afeec-147">**setRuntimeProperties** används toooverride hello filename-egenskapen eller en annan egenskap i hello komponenter i hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="afeec-147">**setRuntimeProperties** is used toooverride hello filename property or another property in hello components of hello workflow.</span></span>
  * <span data-ttu-id="afeec-148">**transcodeSource** är används toospecify hello Clip listan XML-innehåll.</span><span class="sxs-lookup"><span data-stu-id="afeec-148">**transcodeSource** is used toospecify hello Clip List XML content.</span></span>

<span data-ttu-id="afeec-149">Anslutningar i hello arbetsflöde:</span><span class="sxs-lookup"><span data-stu-id="afeec-149">Connections in hello workflow:</span></span>

* <span data-ttu-id="afeec-150">Om du använder en eller flera Media filen indata komponenter och planera toouse **setRuntimeProperties** toospecify hello filnamn och Anslut inte hello primärfilen komponenten PIN-kod toothem.</span><span class="sxs-lookup"><span data-stu-id="afeec-150">If you use one or several Media File Input components and plan toouse **setRuntimeProperties** toospecify hello file name, then do not connect hello primary file component pin toothem.</span></span> <span data-ttu-id="afeec-151">Kontrollera att det finns ingen anslutning mellan hello primärfilen objekt och hello Media filen Input(s).</span><span class="sxs-lookup"><span data-stu-id="afeec-151">Make sure that there is no connection between hello primary file object and hello Media File Input(s).</span></span>
* <span data-ttu-id="afeec-152">Om du föredrar toouse Clip listan XML och en mediekälla komponent kan sedan du ansluta båda tillsammans.</span><span class="sxs-lookup"><span data-stu-id="afeec-152">If you prefer toouse Clip List XML and one Media Source component, then you can connect both together.</span></span>

![Ingen anslutning från primära källfilen tooMedia filen indata](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

<span data-ttu-id="afeec-154">*Det finns ingen anslutning från hello primärfilen tooMedia filen indata komponenterna om du använder setRuntimeProperties tooset hello filename-egenskapen.*</span><span class="sxs-lookup"><span data-stu-id="afeec-154">*There is no connection from hello primary file tooMedia File Input component(s) if you use setRuntimeProperties tooset hello filename property.*</span></span>

![Anslutningen från Clip listan XML tooClip datakälla](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

<span data-ttu-id="afeec-156">*Du kan ansluta Clip listan XML tooMedia källa och använder transcodeSource.*</span><span class="sxs-lookup"><span data-stu-id="afeec-156">*You can connect Clip List XML tooMedia Source and use transcodeSource.*</span></span>

### <a name="clip-list-xml-customization"></a><span data-ttu-id="afeec-157">Beskär listan XML-anpassning</span><span class="sxs-lookup"><span data-stu-id="afeec-157">Clip List XML customization</span></span>
<span data-ttu-id="afeec-158">Du kan ange hello Clip listan XML i hello arbetsflöde vid körning med hjälp av **transcodeSource** string XML i hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="afeec-158">You can specify hello Clip List XML in hello workflow at runtime by using **transcodeSource** in hello configuration string XML.</span></span> <span data-ttu-id="afeec-159">Hello Clip listan XML PIN-kod toobe anslutna toohello mediekälla komponent i hello arbetsflöde krävs.</span><span class="sxs-lookup"><span data-stu-id="afeec-159">This requires hello Clip List XML pin toobe connected toohello Media Source component in hello workflow.</span></span>

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

<span data-ttu-id="afeec-160">Om du vill toospecify /primarySourceFile toouse den här egenskapen tooname hello utdatafiler med hjälp av 'Uttryck', så rekommenderar vi skickar hello Clip listan XML som en egenskap *när* hello /primarySourceFile egenskapen, tooavoid med hello åsidosättas klipp av hello /primarySourceFile inställning.</span><span class="sxs-lookup"><span data-stu-id="afeec-160">If you want toospecify /primarySourceFile toouse this property tooname hello output files by using 'Expressions', then we recommend passing hello Clip List XML as a property *after* hello /primarySourceFile property, tooavoid having hello Clip List be overridden by hello /primarySourceFile setting.</span></span>

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

<span data-ttu-id="afeec-161">Med ytterligare ram exakt hur:</span><span class="sxs-lookup"><span data-stu-id="afeec-161">With additional frame-accurate trimming:</span></span>

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

## <a name="example-1--overlay-an-image-on-top-of-hello-video"></a><span data-ttu-id="afeec-162">Exempel 1: Överlagra en bild ovanpå hello video</span><span class="sxs-lookup"><span data-stu-id="afeec-162">Example 1 : Overlay an image on top of hello video</span></span>

### <a name="presentation"></a><span data-ttu-id="afeec-163">Presentation</span><span class="sxs-lookup"><span data-stu-id="afeec-163">Presentation</span></span>
<span data-ttu-id="afeec-164">Överväg ett exempel där du vill toooverlay en logotyp på hello indatavideo medan kodas hello video.</span><span class="sxs-lookup"><span data-stu-id="afeec-164">Consider an example in which you want toooverlay a logo image on hello input video while hello video is encoded.</span></span> <span data-ttu-id="afeec-165">I det här exemplet hello indatavideo har namnet ”Microsoft_HoloLens_Possibilities_816p24.mp4” och hello logotypen heter ”logo.png”.</span><span class="sxs-lookup"><span data-stu-id="afeec-165">In this example, hello input video is named "Microsoft_HoloLens_Possibilities_816p24.mp4" and hello logo is named "logo.png".</span></span> <span data-ttu-id="afeec-166">Du bör utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="afeec-166">You should perform hello following steps:</span></span>

* <span data-ttu-id="afeec-167">Skapa ett arbetsflöde tillgång med hello-arbetsflödesfil (se följande exempel hello).</span><span class="sxs-lookup"><span data-stu-id="afeec-167">Create a Workflow Asset with hello workflow file (see hello following example).</span></span>
* <span data-ttu-id="afeec-168">Skapa en tillgång med Media, som innehåller två filer: MyInputVideo.mp4 som hello primära fil- och MyLogo.png.</span><span class="sxs-lookup"><span data-stu-id="afeec-168">Create a Media Asset, which contains two files: MyInputVideo.mp4 as hello primary file and MyLogo.png.</span></span>
* <span data-ttu-id="afeec-169">Skicka en aktivitet toohello Media Encoder Premium arbetsflödet media-processor med hello ovan indata tillgångar och ange hello följande konfigurationssträngen.</span><span class="sxs-lookup"><span data-stu-id="afeec-169">Send a task toohello Media Encoder Premium Workflow media processor with hello above input assets and specify hello following configuration string.</span></span>

<span data-ttu-id="afeec-170">Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="afeec-170">Configuration:</span></span>

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

<span data-ttu-id="afeec-171">I hello-exemplet ovan skickas hello namnet på hello videofil toohello Media filen Input-komponenten och hello primarySourceFile-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="afeec-171">In hello example above, hello name of hello video file is sent toohello Media File Input component and hello primarySourceFile property.</span></span> <span data-ttu-id="afeec-172">hello namnet på hello logotypfilen skickas tooanother Media filen indata som är anslutna toohello grafiska överlägget komponent.</span><span class="sxs-lookup"><span data-stu-id="afeec-172">hello name of hello logo file is sent tooanother Media File Input that is connected toohello graphic overlay component.</span></span>

> [!NOTE]
> <span data-ttu-id="afeec-173">hello videofil namn skickas toohello primarySourceFile egenskapen.</span><span class="sxs-lookup"><span data-stu-id="afeec-173">hello video file name is sent toohello primarySourceFile property.</span></span> <span data-ttu-id="afeec-174">Hej anledningen är toouse den här egenskapen i hello arbetsflöde för att skapa med uttryck, till exempel hello-rätt utdata-filnamn.</span><span class="sxs-lookup"><span data-stu-id="afeec-174">hello reason for this is toouse this property in hello workflow for building hello correct output file name using Expressions, for example.</span></span>

### <a name="step-by-step-workflow-creation"></a><span data-ttu-id="afeec-175">Steg för steg-arbetsflödet skapats</span><span class="sxs-lookup"><span data-stu-id="afeec-175">Step-by-step workflow creation</span></span>
<span data-ttu-id="afeec-176">Här följer hello steg toocreate ett arbetsflöde som använder två filer som indata: en video och en bild.</span><span class="sxs-lookup"><span data-stu-id="afeec-176">Here are hello steps toocreate a workflow that takes two files as input: a video and an image.</span></span> <span data-ttu-id="afeec-177">Det kommer täcker hello bilden ovanpå hello video.</span><span class="sxs-lookup"><span data-stu-id="afeec-177">It will overlay hello image on top of hello video.</span></span>

<span data-ttu-id="afeec-178">Öppna **Arbetsflödesdesignern** och välj **filen** > **ny arbetsyta** > **Omkoda modell**.</span><span class="sxs-lookup"><span data-stu-id="afeec-178">Open **Workflow Designer** and select **File** > **New Workspace** > **Transcode Blueprint**.</span></span>

<span data-ttu-id="afeec-179">arbetsflöde för ny hello visar tre element:</span><span class="sxs-lookup"><span data-stu-id="afeec-179">hello new workflow shows three elements:</span></span>

* <span data-ttu-id="afeec-180">Primär källfilen</span><span class="sxs-lookup"><span data-stu-id="afeec-180">Primary Source File</span></span>
* <span data-ttu-id="afeec-181">Beskär List-XML</span><span class="sxs-lookup"><span data-stu-id="afeec-181">Clip List XML</span></span>
* <span data-ttu-id="afeec-182">Filen/Utdatatillgången</span><span class="sxs-lookup"><span data-stu-id="afeec-182">Output File/Asset</span></span>  

![Arbetsflöde för ny kodning](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

<span data-ttu-id="afeec-184">*Arbetsflöde för ny kodning*</span><span class="sxs-lookup"><span data-stu-id="afeec-184">*New Encoding Workflow*</span></span>

<span data-ttu-id="afeec-185">Börja med att lägga till en Media filen inkommande komponent i ordning tooaccept hello mediet-filen.</span><span class="sxs-lookup"><span data-stu-id="afeec-185">In order tooaccept hello input media file, start with adding a Media File Input component.</span></span> <span data-ttu-id="afeec-186">tooadd ett komponenten toohello arbetsflöde, leta efter den i sökrutan för hello databasen och dra hello önskad post till hello designern rutan.</span><span class="sxs-lookup"><span data-stu-id="afeec-186">tooadd a component toohello workflow, look for it in hello Repository search box and drag hello desired entry onto hello designer pane.</span></span>

<span data-ttu-id="afeec-187">Lägg till hello videofil toobe används för att utforma ditt arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="afeec-187">Next, add hello video file toobe used for designing your workflow.</span></span> <span data-ttu-id="afeec-188">toodo så klicka hello bakgrundspanelen i Arbetsflödesdesignern och leta efter hello primära källfilen egenskapen på hello egenskapen högra rutan.</span><span class="sxs-lookup"><span data-stu-id="afeec-188">toodo so, click hello background pane in Workflow Designer and look for hello Primary Source File property on hello right-hand property pane.</span></span> <span data-ttu-id="afeec-189">Klicka på ikonen för hello-mapp och välj hello lämpliga videofil.</span><span class="sxs-lookup"><span data-stu-id="afeec-189">Click hello folder icon and select hello appropriate video file.</span></span>

![Primär källa](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

<span data-ttu-id="afeec-191">*Primär källa*</span><span class="sxs-lookup"><span data-stu-id="afeec-191">*Primary File Source*</span></span>

<span data-ttu-id="afeec-192">Ange därefter hello videofil i hello Media filen ingång komponent.</span><span class="sxs-lookup"><span data-stu-id="afeec-192">Next, specify hello video file in hello Media File Input component.</span></span>   

![Indatakällan för mediefiler](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

<span data-ttu-id="afeec-194">*Indatakällan för mediefiler*</span><span class="sxs-lookup"><span data-stu-id="afeec-194">*Media File Input Source*</span></span>

<span data-ttu-id="afeec-195">När det är klart hello Media filen indata komponenten inspektera hello-filen och fylla i sina PIN-koder tooreflect hello utdatafiler som det kontrolleras.</span><span class="sxs-lookup"><span data-stu-id="afeec-195">As soon as this is done, hello Media File Input component will inspect hello file and populate its output pins tooreflect hello file that it inspected.</span></span>

<span data-ttu-id="afeec-196">hello nästa steg är tooadd utrymme tooRec.709 en ”Video Data typen Updater” toospecify hello färg.</span><span class="sxs-lookup"><span data-stu-id="afeec-196">hello next step is tooadd a "Video Data Type Updater" toospecify hello color space tooRec.709.</span></span> <span data-ttu-id="afeec-197">Lägg till en ”Video konverteraren” som har angetts tooData Layout/Layout typ = konfigurerbara Planar.</span><span class="sxs-lookup"><span data-stu-id="afeec-197">Add a "Video Format Converter" that is set tooData Layout/Layout type = Configurable Planar.</span></span> <span data-ttu-id="afeec-198">Detta omvandlar hello video-ström tooa format som kan räknas som en källa för hello överlägget komponent.</span><span class="sxs-lookup"><span data-stu-id="afeec-198">This will convert hello video stream tooa format that can be taken as a source of hello overlay component.</span></span>

![video Data typen Updater och konverteraren](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

<span data-ttu-id="afeec-200">*Video Data typen Updater och konverteraren*</span><span class="sxs-lookup"><span data-stu-id="afeec-200">*Video Data Type Updater and Format Converter*</span></span>

![Typ av layout = konfigurerbara Planar](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

<span data-ttu-id="afeec-202">*Typ av layout är konfigurerbar Planar*</span><span class="sxs-lookup"><span data-stu-id="afeec-202">*Layout type is Configurable Planar*</span></span>

<span data-ttu-id="afeec-203">Sedan lägger till en Video överlägg komponent och ansluta hello (okomprimerad) video PIN-kod toohello (okomprimerad) video PIN-koden för hello media filen indata.</span><span class="sxs-lookup"><span data-stu-id="afeec-203">Next, add a Video Overlay component and connect hello (uncompressed) video pin toohello (uncompressed) video pin of hello media file input.</span></span>

<span data-ttu-id="afeec-204">Lägga till en annan Media filen indata (tooload hello logotyp-fil), klickar du på den här komponenten och byta namn på den för ”Media filen indata logotyp” och välj en bild (en PNG-fil till exempel) i egenskapen hello.</span><span class="sxs-lookup"><span data-stu-id="afeec-204">Add another Media File Input (tooload hello logo file), click on this component and rename it too"Media File Input Logo", and select an image (a .png file for example) in hello file property.</span></span> <span data-ttu-id="afeec-205">Ansluta hello okomprimerade bilden PIN-kod toohello okomprimerade bilden PIN-koden för hello överlägget.</span><span class="sxs-lookup"><span data-stu-id="afeec-205">Connect hello Uncompressed image pin toohello Uncompressed image pin of hello overlay.</span></span>

![Källa för överlägget komponenten och avbildning](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

<span data-ttu-id="afeec-207">*Källa för överlägget komponenten och avbildning*</span><span class="sxs-lookup"><span data-stu-id="afeec-207">*Overlay component and image file source*</span></span>

<span data-ttu-id="afeec-208">Om du vill toomodify hello position hello logotyp på hello video (till exempel du kanske vill tooposition den på 10 procent från hello övre vänstra hörnet av hello video) avmarkerar kryssrutan för hello ”manuell inmatning”.</span><span class="sxs-lookup"><span data-stu-id="afeec-208">If you want toomodify hello position of hello logo on hello video (for example, you might want tooposition it at 10 percent off of hello top left corner of hello video), clear hello "Manual Input" check box.</span></span> <span data-ttu-id="afeec-209">Du kan göra detta eftersom du använder en Media filen indata tooprovide hello logotypen filen toohello överlägget komponent.</span><span class="sxs-lookup"><span data-stu-id="afeec-209">You can do this because you are using a Media File Input tooprovide hello logo file toohello overlay component.</span></span>

![Överlägget position](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

<span data-ttu-id="afeec-211">*Överlägget position*</span><span class="sxs-lookup"><span data-stu-id="afeec-211">*Overlay position*</span></span>

<span data-ttu-id="afeec-212">tooencode Hej video-ström tooH.264, lägga till hello AVC-videokodare och AAC kodare komponenter toohello designytan.</span><span class="sxs-lookup"><span data-stu-id="afeec-212">tooencode hello video stream tooH.264, add hello AVC Video Encoder and AAC encoder components toohello designer surface.</span></span> <span data-ttu-id="afeec-213">Ansluta hello PIN-koder.</span><span class="sxs-lookup"><span data-stu-id="afeec-213">Connect hello pins.</span></span>
<span data-ttu-id="afeec-214">Ställ in hello AAC kodare och välj ljud Format konvertering/förinställda: 2.0 (L, R).</span><span class="sxs-lookup"><span data-stu-id="afeec-214">Set up hello AAC encoder and select Audio Format Conversion/Preset : 2.0 (L, R).</span></span>

![Ljud och Video kodare](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

<span data-ttu-id="afeec-216">*Ljud och Video kodare*</span><span class="sxs-lookup"><span data-stu-id="afeec-216">*Audio and Video Encoders*</span></span>

<span data-ttu-id="afeec-217">Lägg nu till hello **ISO Mpeg-4 multiplexor** och **utdata** komponenter och ansluta hello PIN-koder som visas.</span><span class="sxs-lookup"><span data-stu-id="afeec-217">Now add hello **ISO Mpeg-4 Multiplexer** and **File Output** components and connect hello pins as shown.</span></span>

![MP4 multiplexor och utdata](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

<span data-ttu-id="afeec-219">*MP4 multiplexor och utdata*</span><span class="sxs-lookup"><span data-stu-id="afeec-219">*MP4 multiplexer and file output*</span></span>

<span data-ttu-id="afeec-220">Du måste tooset hello namn för hello utdatafilen.</span><span class="sxs-lookup"><span data-stu-id="afeec-220">You need tooset hello name for hello output file.</span></span> <span data-ttu-id="afeec-221">Klicka på hello **utdata** komponent och redigera hello-uttrycket för hello-filen:</span><span class="sxs-lookup"><span data-stu-id="afeec-221">Click hello **File Output** component and edit hello expression for hello file:</span></span>

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Filnamnet för utdata](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

<span data-ttu-id="afeec-223">*Filnamnet för utdata*</span><span class="sxs-lookup"><span data-stu-id="afeec-223">*File output name*</span></span>

<span data-ttu-id="afeec-224">Du kan köra hello arbetsflödet lokalt toocheck körs korrekt.</span><span class="sxs-lookup"><span data-stu-id="afeec-224">You can run hello workflow locally toocheck that it is running correctly.</span></span>

<span data-ttu-id="afeec-225">När den är klar kan du köra det i Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="afeec-225">After it finishes, you can run it in Azure Media Services.</span></span>

<span data-ttu-id="afeec-226">Förbereda en tillgång i Azure Media Services med två filer: hello videofil och hello-logotypen.</span><span class="sxs-lookup"><span data-stu-id="afeec-226">First, prepare an asset in Azure Media Services with two files in it: hello video file and hello logo.</span></span> <span data-ttu-id="afeec-227">Du kan göra detta med hjälp av hello .NET eller REST API.</span><span class="sxs-lookup"><span data-stu-id="afeec-227">You can do this by using hello .NET or REST API.</span></span> <span data-ttu-id="afeec-228">Du kan också göra detta med hjälp av hello Azure-portalen eller [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span><span class="sxs-lookup"><span data-stu-id="afeec-228">You can also do this by using hello Azure portal or [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).</span></span>

<span data-ttu-id="afeec-229">De här självstudierna visar hur toomanage tillgångar med AMSE.</span><span class="sxs-lookup"><span data-stu-id="afeec-229">This tutorial shows you how toomanage assets with AMSE.</span></span> <span data-ttu-id="afeec-230">Det finns två sätt tooadd filer tooan tillgångsinformation:</span><span class="sxs-lookup"><span data-stu-id="afeec-230">There are two ways tooadd files tooan asset:</span></span>

* <span data-ttu-id="afeec-231">Skapa en lokal mapp, kopiera hello två filer, och dra och släppa hello mappen toohello **tillgången** fliken.</span><span class="sxs-lookup"><span data-stu-id="afeec-231">Create a local folder, copy hello two files in it, and drag and drop hello folder toohello **Asset** tab.</span></span>
* <span data-ttu-id="afeec-232">Överföra hello videofil som en tillgång, visa hello tillgångsinformation, gå toohello på fliken filer och ladda upp en fil (logotypen).</span><span class="sxs-lookup"><span data-stu-id="afeec-232">Upload hello video file as an asset, display hello asset information, go toohello files tab, and upload an additional file (logo).</span></span>

> [!NOTE]
> <span data-ttu-id="afeec-233">Gör att tooset en primär fil i hello tillgången (hello video huvudfilen).</span><span class="sxs-lookup"><span data-stu-id="afeec-233">Make sure tooset a primary file in hello asset (hello main video file).</span></span>

![Tillgångsfiler i AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

<span data-ttu-id="afeec-235">*Tillgångsfiler i AMSE*</span><span class="sxs-lookup"><span data-stu-id="afeec-235">*Asset files in AMSE*</span></span>

<span data-ttu-id="afeec-236">Välj hello tillgång och välj tooencode med Premium-kodare.</span><span class="sxs-lookup"><span data-stu-id="afeec-236">Select hello asset and choose tooencode it with Premium Encoder.</span></span> <span data-ttu-id="afeec-237">Överför hello arbetsflödet och markera den.</span><span class="sxs-lookup"><span data-stu-id="afeec-237">Upload hello workflow and select it.</span></span>

<span data-ttu-id="afeec-238">Klicka på hello knappen toopass data toohello processor och Lägg till hello följande XML-tooset hello runtime egenskaper:</span><span class="sxs-lookup"><span data-stu-id="afeec-238">Click hello button toopass data toohello processor, and add hello following XML tooset hello runtime properties:</span></span>

![Premium-kodare i AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

<span data-ttu-id="afeec-240">*Premium-kodare i AMSE*</span><span class="sxs-lookup"><span data-stu-id="afeec-240">*Premium Encoder in AMSE*</span></span>

<span data-ttu-id="afeec-241">Klistra sedan in hello följande XML-data.</span><span class="sxs-lookup"><span data-stu-id="afeec-241">Then, paste hello following XML data.</span></span> <span data-ttu-id="afeec-242">Du behöver toospecify hello namnet på hello videofil för både hello Media filen indata och primarySourceFile.</span><span class="sxs-lookup"><span data-stu-id="afeec-242">You need toospecify hello name of hello video file for both hello Media File Input and primarySourceFile.</span></span> <span data-ttu-id="afeec-243">Ange hello namn hello filnamn för hello logotypen för.</span><span class="sxs-lookup"><span data-stu-id="afeec-243">Specify hello name of hello file name for hello logo too.</span></span>

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

<span data-ttu-id="afeec-245">*setRuntimeProperties*</span><span class="sxs-lookup"><span data-stu-id="afeec-245">*setRuntimeProperties*</span></span>

<span data-ttu-id="afeec-246">Om du använder hello .NET SDK toocreate och köra hello aktivitet har toobe skickas som hello konfigurationssträngen XML-data.</span><span class="sxs-lookup"><span data-stu-id="afeec-246">If you use hello .NET SDK toocreate and run hello task, this XML data has toobe passed as hello configuration string.</span></span>

```c#
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

<span data-ttu-id="afeec-247">När hello jobbet är klart utdata hello MP4-fil i hello tillgångsinformation visar hello överlägget!</span><span class="sxs-lookup"><span data-stu-id="afeec-247">After hello job is complete, hello MP4 file in hello output asset displays hello overlay!</span></span>

![Överlägg på hello video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

<span data-ttu-id="afeec-249">*Överlägg på hello video*</span><span class="sxs-lookup"><span data-stu-id="afeec-249">*Overlay on hello video*</span></span>

<span data-ttu-id="afeec-250">Du kan hämta hello Exempelarbetsflöde från [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span><span class="sxs-lookup"><span data-stu-id="afeec-250">You can download hello sample workflow from [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).</span></span>

## <a name="example-2--multiple-audio-language-encoding"></a><span data-ttu-id="afeec-251">Exempel 2: Flera ljud språkkod</span><span class="sxs-lookup"><span data-stu-id="afeec-251">Example 2 : Multiple audio language encoding</span></span>

<span data-ttu-id="afeec-252">Ett exempel på flera språk för ljud kodning workfkow finns i [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span><span class="sxs-lookup"><span data-stu-id="afeec-252">An example of multiple audio language encoding workfkow is available in [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding).</span></span>

<span data-ttu-id="afeec-253">Den här mappen innehåller ett Exempelarbetsflöde som kan vara används tooencode en MXF filen tooa multi MP4-filer tillgång med flera ljud spår.</span><span class="sxs-lookup"><span data-stu-id="afeec-253">This folder contains a sample workflow which can be used tooencode a MXF file tooa multi MP4 files asset with multiple audio tracks.</span></span>

<span data-ttu-id="afeec-254">Det här arbetsflödet förutsätter hello MXF filen innehåller ett ljud spår; hello ytterligare ljud spårar ska skickas som separata ljudfiler (WAV eller MP4...).</span><span class="sxs-lookup"><span data-stu-id="afeec-254">This workflow assumes that hello MXF file contains one audio track ; hello additional audio tracks should be passed as seperate audio files (WAV or MP4...).</span></span>

<span data-ttu-id="afeec-255">tooencode, följer du dessa anvisningar:</span><span class="sxs-lookup"><span data-stu-id="afeec-255">tooencode, please follow these steps:</span></span>

* <span data-ttu-id="afeec-256">Skapa en tillgång med Media Services med hello MXF fil- och hello ljud-filer (0 too18 ljud).</span><span class="sxs-lookup"><span data-stu-id="afeec-256">Create a Media Services asset with hello MXF file and hello Audio files (0 too18 audio files).</span></span>
* <span data-ttu-id="afeec-257">Kontrollera att filen hello MXF har angetts som en primär fil.</span><span class="sxs-lookup"><span data-stu-id="afeec-257">Make sure that hello MXF file is set as a primary file.</span></span>
* <span data-ttu-id="afeec-258">Skapa ett jobb och en aktivitet med hello Premium-kodare arbetsflödet processor.</span><span class="sxs-lookup"><span data-stu-id="afeec-258">Create a job and a task using hello Premium Workflow Encoder processor.</span></span> <span data-ttu-id="afeec-259">Använd hello arbetsflödet som anges (MultiMP4-1080p-19audio-v1.workflow).</span><span class="sxs-lookup"><span data-stu-id="afeec-259">Use hello workflow provided (MultiMP4-1080p-19audio-v1.workflow).</span></span>
* <span data-ttu-id="afeec-260">Skicka hello setruntime.xml data toohello aktivitet (om du använder Azure Media Services Explorer, använda hello ”skicka toohello arbetsflödet för XML-data” knappen).</span><span class="sxs-lookup"><span data-stu-id="afeec-260">Pass hello setruntime.xml data toohello task (if you use Azure Media Services Explorer, use hello “pass xml data toohello workflow” button).</span></span>
  * <span data-ttu-id="afeec-261">Uppdatera hello XML-data toospecify hello rätt fil namn och språk taggar.</span><span class="sxs-lookup"><span data-stu-id="afeec-261">Please update hello XML data toospecify hello correct file names and languages tags.</span></span>
  * <span data-ttu-id="afeec-262">hello arbetsflöde har ljud komponenter med namnet ljud 1 tooAudio 18.</span><span class="sxs-lookup"><span data-stu-id="afeec-262">hello workflow has audio components named Audio 1 tooAudio 18.</span></span>
  * <span data-ttu-id="afeec-263">RFC5646 stöds för hello språk taggen.</span><span class="sxs-lookup"><span data-stu-id="afeec-263">RFC5646 is supported for hello language tag.</span></span>

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

* <span data-ttu-id="afeec-264">hello kodade tillgången innehåller flera språk ljud spårar och spåren ska väljas i Azure Media Player.</span><span class="sxs-lookup"><span data-stu-id="afeec-264">hello encoded asset will contain multi language audio tracks and these tracks should be selectable in Azure Media Player.</span></span>

## <a name="see-also"></a><span data-ttu-id="afeec-265">Se även</span><span class="sxs-lookup"><span data-stu-id="afeec-265">See also</span></span>
* [<span data-ttu-id="afeec-266">Introduktion till Premium-kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="afeec-266">Introducing Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="afeec-267">Hur toouse Premium kodning i Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="afeec-267">How toouse Premium Encoding in Azure Media Services</span></span>](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [<span data-ttu-id="afeec-268">Koda innehåll på begäran med Azure Media Services</span><span class="sxs-lookup"><span data-stu-id="afeec-268">Encoding on-demand content with Azure Media Services</span></span>](media-services-encode-asset.md#media-encoder-premium-workflow)
* [<span data-ttu-id="afeec-269">Arbetsflöde för Media Encoder Premium format och codec</span><span class="sxs-lookup"><span data-stu-id="afeec-269">Media Encoder Premium Workflow formats and codecs</span></span>](media-services-premium-workflow-encoder-formats.md)
* [<span data-ttu-id="afeec-270">Exempelfiler för arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="afeec-270">Sample workflow files</span></span>](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)
* [<span data-ttu-id="afeec-271">Azure Media Services Explorer-verktyget</span><span class="sxs-lookup"><span data-stu-id="afeec-271">Azure Media Services Explorer tool</span></span>](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a><span data-ttu-id="afeec-272">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="afeec-272">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="afeec-273">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="afeec-273">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
