---
title: "aaaPerform avancerade kodning genom att anpassa MES förinställningar | Microsoft Docs"
description: "Det här avsnittet visar hur tooperform avancerade kodning genom att anpassa Media Encoder Standard uppgiften förinställningar."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: juliako
ms.openlocfilehash: 9caa68fafacaf51f91f0554c5bafe491928d8c77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a>Utföra avancerade encoding genom att anpassa MES förinställningar 

## <a name="overview"></a>Översikt

Det här avsnittet visar hur toocustomize Media Encoder Standard förinställningar. Hej [Encoding med Media Encoder Standard med hjälp av anpassade förinställningar](media-services-custom-mes-presets-with-dotnet.md) avsnittet visas hur toouse .NET toocreate kodning uppgift och ett jobb som utför den här uppgiften. När du anpassar en förinställning, ange hello anpassade förinställningar toohello kodning aktivitet. 

>[!NOTE]
>Om du använder en XML-förinställning, göra att toopreserve hello elementordning, som visas i XML-exempel (till exempel KeyFrameInterval måste föregå SceneChangeDetection).
>

I det här avsnittet hello anpassade förinställningar som utför hello efter kodning uppgifter har visat.

## <a name="support-for-relative-sizes"></a>Stöd för relativa storleken

När du genererar miniatyrer, behöver du inte ange tooalways utdata bredd och höjd i bildpunkter. Du kan ange dem i procent i hello intervallet [% 1,..., 100%].

### <a name="json-preset"></a>JSON förinställda
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a>XML-förinställda
    <Width>100%</Width>
    <Height>100%</Height>

## <a id="thumbnails"></a>Generera miniatyrbilder

Det här avsnittet visas hur toocustomize en förinställning som genererar miniatyrer. hello innehåller förinställning som anges nedan information om hur du vill att tooencode filen samt information som behövs för toogenerate miniatyrerna. Du kan vidta någon av hello MES förinställningar dokumenterade [detta](media-services-mes-presets-overview.md) och lägger till kod som genererar miniatyrer.  

> [!NOTE]
> Hej **SceneChangeDetection** inställningen i hello följande förinställda kan endast anges tootrue om du kodar tooa enkel bithastighet video. Om du kodar tooa multibithastighet video och ange **SceneChangeDetection** tootrue, hello kodare returnerar ett fel.  
>
>

Mer information om schemat finns [detta](media-services-mes-schema.md) avsnittet.

Se till att tooreview hello [överväganden](#considerations) avsnitt.

### <a id="json"></a>JSON förinställda
    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"

            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a id="xml"></a>XML-förinställda
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

### <a name="considerations"></a>Överväganden

det gäller hello följande överväganden:

* hello användningen av explicita tidsstämplar för Startintervall-steg förutsätter att hello Indatakällan är minst 1 minut.
* BmpImage-jpg/Png-element har Start, steg, och intervallet strängattribut – dessa kan tolkas som:

  * RAM-numret om de är icke-negativa heltal, till exempel ”Start”: ”120”
  * Relativa toosource varaktighet om uttryckt %-suffixet, till exempel ”Start”: ”15%”, eller
  * Tidsstämpel om uttryckt: mm: ss... Formatera, till exempel ”Start” ”: 00: 01:00”

    Du kan blanda och matcha här ska du ange.

    Dessutom Start har också stöd för ett särskilt makro: {bästa}, som försöker toodetermine hello första ”intressant” bildrutan hello innehåll Obs: (steg och intervallet ignoreras när Start är inställd för {bästa})
  * Standardvärden: Starta: {bästa}
* Utdataformat måste uttryckligen anges för varje bildformat toobe: BmpFormat-Jpg/Png. När matchar MES JpgVideo tooJpgFormat och så vidare. OutputFormat introducerar ett nytt specifika bilden codec makro: {Index}, som måste toobe finns (en gång och bara en gång) för bildformat för utdata.

## <a id="trim_video"></a>Ta bort en video (urklippet)
Det här avsnittet pratar om hur du ändrar hello kodare förinställningar tooclip eller trim hello indatavideo där hello-indata är en s.k. mezzaninfil eller på begäran-fil. hello kodare kan också använda tooclip eller rensa en tillgång, som har hämtats eller arkiverade från en direktsänd dataström – hello information för detta finns i [bloggen](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

tootrim filmer, du kan vidta någon av hello MES förinställningar dokumenterade [detta](media-services-mes-presets-overview.md) avsnittet och ändra hello **källor** element (som visas nedan). hello-värdet för StartTime måste toomatch hello absolut tidsstämplar hello inkommande video. Till exempel om hello första bildrutan i hello indatavideo har en tidsstämpel för 12:00:10.000 och StartTime bör vara minst 12:00:10.000 och större. I hello exemplet nedan förutsätter att hello videoinmatning har en första tidsstämpel noll. **Källor** ska placeras hello början av hello förinställda.

### <a id="json"></a>JSON förinställda
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="xml-preset"></a>XML-förinställda
tootrim filmer, du kan vidta någon av hello MES förinställningar dokumenterade [här](media-services-mes-presets-overview.md) och ändra hello **källor** element (som visas nedan).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

## <a id="overlay"></a>Skapa ett överlägg

hello Media Encoder Standard kan du toooverlay en bild till en befintlig video. För närvarande hello följande format stöds: png, jpg, gif, bmp och. hello är förinställning som anges nedan ett grundläggande exempel på en videoöverlägg.

Dessutom toodefining en förinställd fil du har också toolet Media Services vet vilken fil i hello tillgången är hello överlägget avbildningen och vilken fil är hello källa video där du vill toooverlay hello bilden. hello videofil har toobe hello **primära** fil.

Om du använder .NET, lägger du till hello följande två funktioner toohello .NET-exempel som definierats i [detta](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) avsnittet. Hej **UploadMediaFilesFromFolder** funktionen Överför filer från en mapp (till exempel BigBuckBunny.mp4 och Image001.png) och anger hello mp4 toobe hello primära fil i hello tillgången. Hej **EncodeWithOverlay** använder funktionen hello anpassade förvalda som skickades tooit (till exempel hello förinställningen som följer) toocreate hello kodning aktivitet.


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // hello following code assumes 
            // you have an input folder with one MP4 and one overlay image file.
            if (af.Name.Contains(".mp4"))
                af.IsPrimary = true;
            else
                af.IsPrimary = false;
    
            af.Update();
        }
    
        return asset;
    }

    static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("Media Encoder Standard Job");
        // Get a media processor reference, and pass tooit hello name of hello 
        // processor toouse for hello specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load hello XML (or JSON) from hello local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify hello input assets toobe encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset toocontain hello results of hello job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> Aktuella begränsningar:
>
> hello överlägget opacitetsinställningen stöds inte.
>
> Din video källfilen och hello överlägget image-filen har toobe i hello samma tillgång och hello videofil behov toobe set som hello primära fil i tillgången.
>
>

### <a name="json-preset"></a>JSON förinställda
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a>XML-förinställda
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


## <a id="silent_audio"></a>Infoga en tyst ljud spåra när indata har inget ljud
Som standard om du skickar en inkommande toohello kodare som bara innehåller video, och inget ljud innehåller hello utdatatillgången filer som innehåller endast video data. Vissa spelare kanske inte toohandle sådana utdataströmmar. Du kan använda den här inställningen tooforce hello kodare tooadd en tyst ljud spåra toohello utdata i scenariot.

tooforce hello kodare tooproduce en tillgång som innehåller en tyst ljud spåra när indata har inget ljud ange hello ”InsertSilenceIfNoAudio” värde.

Du kan vidta någon av hello MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnitt och att Hej ändringar:

### <a name="json-preset"></a>JSON förinställda
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a>XML-förinställda
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <a id="deinterlacing"></a>Inaktivera automatisk Frigör sammanflätning
Kunder behöver inte toodo något om de liksom hello interlace innehållet toobe automatiskt ej sammanflätad. När hello automatiskt Frigör sammanflätning är aktiverad (standard) hello MES hello automatisk identifiering av sammanflätade ramar och frigör interlaces endast ramar som markerats som sammanflätad.

Du kan inaktivera hello automatiskt Frigör sammanflätning. Det här alternativet rekommenderas inte.

### <a name="json-preset"></a>JSON förinställda
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a>XML-förinställda
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <a id="audio_only"></a>Endast ljud förinställningar
Det här avsnittet beskrivs två ljuddata MES förinställningar: AAC ljud- och AAC bra kvalitet ljud.

### <a name="aac-audio"></a>AAC ljud
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="aac-good-quality-audio"></a>AAC god kvalitet ljud
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="concatenate"></a>Sammanfoga två eller flera videofiler

hello följande exempel visar hur du kan skapa en förinställd tooconcatenate två eller fler videofiler. hello vanligaste scenariot är när du vill tooadd ett sidhuvud eller en släpvagn toohello huvudsakliga video. hello avsedd användning är när hello videofiler redigeras tillsammans resursegenskaper (skärmupplösning, bildfrekvens, ljud spåra antalet osv.). Noga inte toomix videor olika hastigheterna eller med olika antal ljud spår.

>[!NOTE]
>hello aktuella designen av hello sammanfogning funktionen förväntar sig att hello indata videoklipp är konsekventa vad gäller upplösning bildfrekvens osv. 

### <a name="requirements-and-considerations"></a>Krav och överväganden

* Inkommande videor endast ha ett ljud spår.
* Ange videor ska ha hello samma bildfrekvens.
* Du måste överföra videor till separata tillgångar och ange hello videor som hello primärfilen i varje tillgång.
* Du måste tooknow hello varaktighet videor.
* hello förinställda exemplen nedan förutsätter att alla hello inkommande videor som börjar med en tidsstämpel noll. Du behöver toomodify hello StartTime värden om hello videor har olika första tidsstämpel som är vanligtvis hello fallet med Direktmigrering Arkiv.
* hello gör JSON förinställda explicita referenser toohello AssetID värdena för hello inkommande tillgångar.
* hello exempelkoden förutsätter att hello JSON förinställningen har sparats tooa lokal fil, till exempel ”C:\supportFiles\preset.json”. Det förutsätts även att två tillgångar har skapats genom att ladda upp två videofiler och att du vet hello gällande AssetID värden.
* Hej kodfragmentet och JSON förinställda visar ett exempel på Sammanfoga två videofiler. Du kan utöka den toomore än två videor av:

  1. Anropar uppgift. InputAssets.Add() upprepade gånger tooadd fler videoklipp i ordning.
  2. Gör motsvarande redigerar toohello ”källor” element i hello JSON, genom att lägga till fler poster i hello samma ordning.

### <a name="net-code"></a>.NET-kod

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass tooit hello name of the
    // processor toouse for hello specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load hello XML (or JSON) from hello local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify hello input videos toobe concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset toocontain hello results of hello job.
    // This output is specified as AssetCreationOptions.None, which
    // means hello output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a>JSON förinställda

Uppdatera dina anpassade förinställningen med ID: n för hello tillgångar som du vill tooconcatenate och hello lämplig tidpunkt segment för varje video.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a id="crop"></a>Beskär videor med Media Encoder Standard
Se hello [Beskär videor med Media Encoder Standard](media-services-crop-video.md) avsnittet.

## <a id="no_video"></a>Infoga en video spåra när indata har ingen bild

Som standard om du skickar en inkommande toohello kodare som innehåller endast ljud och ingen bild innehåller hello utdatatillgången filer som innehåller endast ljuddata. Vissa spelare, inklusive Azure Media Player (se [detta](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) kanske inte kan toohandle dessa strömmar. Du kan använda den här inställningen tooforce hello kodare tooadd en monokromt video spåra toohello utdata i scenariot.

> [!NOTE]
> Tvinga hello kodare tooinsert en utdata video spåra ökar hello storlek hello utdata tillgången och hello därmed kostnaden för hello kodning aktivitet. Du bör köra testerna tooverify som Storleksökningen gällande har endast en liten inverkan på dina månatliga avgifter.
>

### <a name="inserting-video-at-only-hello-lowest-bitrate"></a>Infoga video på endast hello lägsta bithastighet

Anta att du använder en flera kodning av bithastighet förinställningen som [”H264 Multibithastighet 720p”](media-services-mes-preset-h264-multiple-bitrate-720p.md) tooencode hela din indata katalogen för strömning, som innehåller en blandning av video och ljud-only-filer. I detta scenario när hello indata har ingen bild kanske du vill tooforce hello kodare tooinsert en video monokromt spåra på bara hello lägsta bithastighet, som skillnad från tooinserting video vid varje utdata bithastighet. tooachieve detta, behöver du toouse hello **InsertBlackIfNoVideoBottomLayerOnly** flaggan.

Du kan vidta någon av hello MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnitt och att Hej ändringar:

#### <a name="json-preset"></a>JSON förinställda
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-förinställda

Använd villkor när du använder XML = ”InsertBlackIfNoVideoBottomLayerOnly” som en attributet toohello **H264Video** element och villkor = ”InsertSilenceIfNoAudio” som ett attribut för**AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideoBottomLayerOnly">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .
```

### <a name="inserting-video-at-all-output-bitrates"></a>Infoga video alls utdata bithastighet
Anta att du använder en flera kodning av bithastighet förinställningen som [”H264 Multibithastighet 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) tooencode hela din indata katalogen för strömning, som innehåller en blandning av video och ljud-only-filer. I detta scenario när hello indata har ingen bild kanske du vill tooforce hello kodare tooinsert monokromt video reda på alla hello utdata bithastighet. Detta säkerställer att din utdata tillgångar är alla homogen med avseende toonumber spårar video och ljud spår. tooachieve detta, behöver du toospecify hello ”InsertBlackIfNoVideo” flaggan.

Du kan vidta någon av hello MES förinställningar dokumenterade i [detta](media-services-mes-presets-overview.md) avsnitt och att Hej ändringar:

#### <a name="json-preset"></a>JSON förinställda
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-förinställda

Använd villkor när du använder XML = ”InsertBlackIfNoVideo” som en attributet toohello **H264Video** element och villkor = ”InsertSilenceIfNoAudio” som ett attribut för**AACAudio**.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideo">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .  
```

## <a id="rotate_video"></a>Rotera en video
Hej [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) stöder rotation av vinklar 0/90/180/270. hello standardbeteendet är ”automatisk”, där den försöker toodetect hello rotation metadata i hello inkommande videofil och kompensera för den. Inkludera hello följande **källor** elementet tooone hello förinställningar som definierats i [detta](media-services-mes-presets-overview.md) avsnitt:

### <a name="json-preset"></a>JSON förinställda
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...
### <a name="xml-preset"></a>XML-förinställda
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Se även [detta](media-services-mes-schema.md#PreserveResolutionAfterRotation) avsnittet för mer information om hur hello kodare tolkar hello bredd och höjd inställningar i hello förinställningen när rotation ersättning utlöses.

Du kan använda hello värdet ”0” tooindicate toohello kodare tooignore rotation metadata, om den finns i hello inkommande videon.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Se även
[Media Services Encoding översikt](media-services-encode-asset.md)
