---
title: aaaIndexing mediefiler med Azure Media indexeraren
description: "Azure Media Indexer kan du toomake innehållet i mediefiler sökbara och toogenerate en fulltext betyg för stängd textning och nyckelord. Det här avsnittet visar hur toouse Media indexeraren."
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: adsolank;juliako;johndeu
ms.openlocfilehash: c1bed774e302e61ca3510668645dc2015b434a0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Indexering mediefiler med Azure Media indexeraren
Azure Media Indexer kan du toomake innehållet i mediefiler sökbara och toogenerate en fulltext betyg för stängd textning och nyckelord. Du kan bearbeta en mediefil eller flera mediefiler i en batch.  

> [!IMPORTANT]
> När indexering av innehåll, se till att toouse mediefiler med tydliga tal (utan bakgrundsmusik, ljud, effekter eller mikrofon hiss). Några exempel på rätt innehåll är: registreras möten, Föreläsningar och presentationer. hello följande innehåll är inte lämpliga för indexering: filmer, TV-program, allt med blandade ljud och visuella effekter dåligt registreras innehåll med bakgrundsljud (hiss).
> 
> 

En indexering jobb kan generera hello följande utdata:

* Stängd bildtexten filer i hello följande format: **SAMI**, **TTML**, och **WebVTT**.
  
    Dold textning filer innehåller en tagg som kallas Recognizability, vilka resultat som en indexering jobb baserat på hur okänt hello tal i hello källa videon är.  Du kan använda hello värdet för Recognizability tooscreen utdatafilerna för användbarhet. En låg poäng betyder dåliga indexering resultat på grund av tooaudio kvalitet.
* Nyckelordsfil (XML).
* Ljud indexering blob-fil (AIB) för användning med SQLServer.
  
    Mer information finns i [med AIB filer med Azure Media Indexer och SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).

Det här avsnittet visar hur toocreate indexering jobb för**Index för en tillgång** och **flera indexfiler**.

Hello Azure Media Indexer uppdaterad finns [Media Services bloggar](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Med hjälp av konfigurations-och manifest för indexering uppgifter
Du kan ange mer information för dina indexering aktiviteter med hjälp av en konfiguration för aktiviteten. Exempelvis kan du ange vilka metadata toouse för media-fil. Dessa metadata som används av hello språk motorn tooexpand dess ordförråd och ger betydligt bättre hello taligenkänningen.  Du är också kan toospecify önskade utgående filer.

Du kan också bearbeta flera filer samtidigt med hjälp av en manifestfil.

Mer information finns i [aktivitet förinställningen Azure Media indexer](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Index för en tillgång
hello följande metod överför en mediefil som en tillgång och skapar ett jobb tooindex hello tillgång.

Observera att om ingen konfigurationsfil anges hello mediefilen indexeras med alla standardinställningar.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload hello input media file toostorage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference toohello Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with hello encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input asset toobe indexed.
        task.InputAssets.Add(asset);

        // Add an output asset toocontain hello results of hello job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use hello following event handler toocheck job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch hello job.
        job.Submit();

        // Check job execution and wait for job toofinish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, hello event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due toojob error.");
            return false;
        }

        // Download hello job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
<!-- __ -->
### <a id="output_files"></a>Utgående filer
Som standard genererar ett indexering jobb hello följande utgående filer. hello filer kommer att lagras i hello första utdatatillgången.

När det finns fler än en inkommande mediefilen är genererar indexeraren en manifestfil hello jobbet utdata, med namnet 'JobResult.txt'. För varje inkommande mediefilen är hello resulterande AIB, SAMI, TTML, WebVTT och nyckelordet filer sekventiellt numrerade och namnges med hello ”Alias”.

| Filnamn | Beskrivning |
| --- | --- |
| **InputFileName.aib** |Ljud indexering blob-fil. <br/><br/> Ljudfilen indexering Blob (AIB) är en binär fil som kan sökas i Microsoft SQL server med fulltextsökning.  hello AIB filen är mer kraftfulla än hello enkel etikett filer, eftersom den innehåller alternativ för varje ord, så att en mycket rikare upplevelse för sökning. <br/> <br/>Det kräver hello installation av hello indexeraren SQL-tillägg på en dator kör Microsoft SQL server 2008 eller senare. Söker hello AIB med hjälp av Microsoft SQL ger server-fulltextsökning exaktare sökresultat än söker hello stängd bildtexten filer som genererats av WAMI. Det beror på att hello AIB innehåller word alternativ ljud liknande medan hello stängd bildtexten filer som innehåller hello högsta förtroende ord för varje del av hello ljud. Om du söker efter talade ord är upmost viktigt, rekommenderas toouse hello AIB i tillsammans med Microsoft SQL Server.<br/><br/> toodownload Hej tillägg, klickar du på <a href="http://aka.ms/indexersql">Azure Media Indexer SQL tillägget</a>. <br/><br/>Det är också möjligt tooutilize andra sökmotorer som Apache Lucene/Solr toosimply index hello video utifrån hello stängd beskrivning och nyckelordet XML-filer, men detta resulterar i mindre exakt sökresultat. |
| **InputFileName.smi**<br/>**InputFileName.ttml**<br/>**InputFileName.vtt** |Stängd beskrivning (CC) filer i SAMI, TTML och WebVTT format.<br/><br/>De kan använda toomake ljud och video filer tillgänglig toopeople med hörsel funktionshinder.<br/><br/>Stängd bildtexten filer innehåller en tagg som kallas <b>Recognizability</b> som poäng en indexering jobb baserat på hur okänt hello tal i hello källa videon är.  Du kan använda hello värdet för <b>Recognizability</b> tooscreen utdatafiler för användbarhet. En låg poäng betyder dåliga indexering resultat på grund av tooaudio kvalitet. |
| **InputFileName.kw.xml<br/>InputFileName.info** |Information om nyckelord och filer. <br/><br/>Nyckelordet filen är en XML-fil som innehåller nyckelord som extraheras från hello tal innehåll med frekvens och offset-information. <br/><br/>Information om filen är en oformaterad textfil som innehåller detaljerad information om varje term som identifieras. hello första raden är speciellt och innehåller hello Recognizability poäng. Varje efterföljande rad är en tabbavgränsade lista över hello följande data: starta tid, sluttid, /-fras, förtroende. hello tiderna anges i sekunder och hello förtroende ges som ett tal mellan 0-1. <br/><br/>Exempel rad: ”1,20 1,45 word 0.67” <br/><br/>Dessa filer kan användas för flera olika syften, till exempel tooperform tal analytics eller exponerade toosearch motorer, till exempel Bing eller Google, Microsoft SharePoint toomake hello mediefiler mer synliga eller även använda toodeliver mer relevant annonser. |
| **JobResult.txt** |Utdata manifestet, visas bara om indexering flera filer, som innehåller hello följande information:<br/><br/><table border="1"><tr><th>Indatafil</th><th>Alias</th><th>MediaLength</th><th>Fel</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Om inte alla inkommande mediefiler indexeras har, hello indexering jobb misslyckas med felkoden 4000. Mer information finns i [felkoder](#error_codes).

## <a name="index-multiple-files"></a>Index flera filer
hello följa metoden Överför flera mediefiler som en tillgång och skapar ett jobb tooindex alla filer i en batch.

Manifestfilen med hello .lst tillägg har skapats och ladda upp till hello tillgång. hello Manifestfilen innehåller hello lista över alla hello tillgångsfiler. Mer information finns i [aktivitet förinställningen Azure Media indexer](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload toostorage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all hello asset file names and upload toostorage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference toohello Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with hello encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input asset toobe indexed.
        task.InputAssets.Add(asset);

        // Add an output asset toocontain hello results of hello job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use hello following event handler toocheck job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch hello job.
        job.Submit();

        // Check job execution and wait for job toofinish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, hello event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due toojob error.");
            return false;
        }

        // Download hello job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Delvis har slutförts jobb
Om inte alla inkommande mediefiler indexeras har, hello indexering jobb misslyckas med felkoden 4000. Mer information finns i [felkoder](#error_codes).

Hej samma utdata (som har slutförts jobb) genereras. Du kan se toohello utdata manifestfilen toofind reda på vilka indatafiler kunde, enligt toohello fel kolumnvärdena. Hej resulterande AIB, SAMI, TTML, WebVTT för inkommande filer som misslyckades och nyckelordet filer skapas.

### <a id="preset"></a>Uppgiften förinställda Azure Media indexer
hello bearbetning från Azure Media Indexer kan anpassas genom att tillhandahålla en frivillig uppgift förinställningen tillsammans med hello-aktivitet.  hello nedan beskrivs hello format för den här xml-konfigurationsfilen.

| Namn | Kräv | Beskrivning |
| --- | --- | --- |
| **indata** |FALSKT |Tillgångsinformation fil(er) som du vill tooindex.</p><p>Azure Media Indexer stöder hello följande format: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Du kan ange hello filnamn (s) i hello **namn** eller **lista** attribut för hello **inkommande** element (som visas nedan). Om du inte anger vilka tillgången filen tooindex plockats hello primära filen. Om inga primära resursfilen anges indexeras hello första filen i hello inkommande tillgången.</p><p>tooexplicitly anger hello tillgången filnamn, gör du:<br/>`<input name="TestFile.wmv">`<br/><br/>Du kan även indexera flera tillgångsfiler samtidigt (upp too10-filer). toodo detta:<br/><br/><ol class="ordered"><li><p>Skapa en textfil (manifestfilen) och ge det ett .lst-tillägg. </p></li><li><p>Lägga till en lista över alla hello tillgången filnamn i inkommande tillgången toothis manifestfilen. </p></li><li><p>Lägg till (överföringen) thanifest filen toohello tillgången.  </p></li><li><p>Ange hello namnet på manifestfilen hello i hello indata-attribut.<br/>`<input list="input.lst">`</li></ol><br/><br/>Obs: Om du lägger till fler än 10 filer toohello manifestfilen hello indexering jobb misslyckas med felkoden för hello 2006. |
| **metadata** |FALSKT |Metadata för hello angivna tillgången filer som används för ordförråd anpassning.  Användbar tooprepare indexeraren toorecognize standardmässiga ordförråd ord som namn.<br/>`<metadata key="..." value="..."/>` <br/><br/>Du kan ange **värden** för fördefinierade **nycklar**. Följande nycklar hello stöds för närvarande:<br/><br/>”title” och ”beskrivning” - används för ordförråd anpassning tootweak hello språk modellen för jobbet och förbättra taligenkänningen.  hello värden dirigera Internet sökningar toofind sammanhang relevanta textdokument, använder hello innehållet tooaugment hello interna ordlista för hello varaktighet för indexering uppgiften.<br/>`<metadata key="title" value="[Title of hello media file]" />`<br/>`<metadata key="description" value="[Description of hello media file] />"` |
| **funktioner** <br/><br/> Lägga till i version 1.2. Hello stöds endast funktionen är för närvarande taligenkänning (”ASR”). |FALSKT |hello taligenkänning har hello följande inställningar nycklar:<table><tr><th><p>Nyckel</p></th>        <th><p>Beskrivning</p></th><th><p>Exempelvärde</p></th></tr><tr><td><p>Språk</p></td><td><p>hello naturligt språk toobe identifieras i hello fil.</p></td><td><p>Engelska, spanska</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>en semikolonavgränsad lista över hello önskad rubrik utdataformat (eventuella)</p></td><td><p>ttml; sami; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>En boolesk flagga som anger huruvida en AIB fil krävs (för användning med SQL Server och hello kunden indexeraren IFilter).  Mer information finns i <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">med AIB filer med Azure Media Indexer och SQL Server</a>.</p></td><td><p>SANT. FALSKT</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>En boolesk flagga som anger huruvida en nyckelordet XML-fil måste anges.</p></td><td><p>SANT. FALSKT. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>En boolesk flagga som anger huruvida tooforce fullständig bildtexter (oavsett konfidensnivån).  </p><p>Standardvärdet är false, i vilket fall ord och fraser som har mindre än 50% konfidensnivå utelämnas från hello sista etiketten utdata och ersättas med ellipserna (”...”).  hello ellipserna är användbara för rubriken kvalitetskontroll och granskning.</p></td><td><p>SANT. FALSKT. </p></td></tr></table> |

### <a id="error_codes"></a>Felkoder
Hello gäller ett fel, Azure Media Indexer ska rapportera tillbaka en hello följande felkoder:

| Kod | Namn | Möjliga orsaker |
| --- | --- | --- |
| 2000 |Ogiltig konfiguration |Ogiltig konfiguration |
| 2001 |Ogiltiga indata tillgångar |Saknar indata tillgångar eller tom tillgång. |
| 2002 |Ogiltigt manifest |Manifestet är tom eller manifest innehåller ogiltiga objekt. |
| 2003 |Misslyckade toodownload mediefil |Ogiltig URL i manifestfilen. |
| 2004 |Protokoll som inte stöds |Protokollet för media URL stöds inte. |
| 2005 |Typ som inte stöds |Mediafiltypen stöds inte. |
| 2006 |För många inkommande filer |Det finns fler än 10 filer i hello inkommande manifest. |
| 3000 |Misslyckade toodecode mediefil |Codec för stöds inte <br/>eller<br/> Skadat mediefil <br/>eller<br/> Inga ljudström i mediet. |
| 4000 |Delvis lyckades batch indexering |Vissa av hello mediet filer är misslyckades toobe indexeras. Mer information finns i <a href="#output_files">utdatafiler</a>. |
| övrigt |Internt fel |Kontakta supportteamet. indexer@microsoft.com |

## <a id="supported_languages"></a>Språk som stöds
Hello engelska och spanska språk stöds för närvarande. Mer information finns i [hello version 1.2 versionen blogginlägget](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Azure Media Services Analytics-översikt](media-services-analytics-overview.md)

[Med hjälp av AIB filer med Azure Media Indexer och SQLServer](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indexering mediefiler med Azure Media Indexer 2 förhandsgranskning](media-services-process-content-with-indexer2.md)

