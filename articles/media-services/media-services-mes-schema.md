---
title: aaaMedia Encoder Standard schemat | Microsoft Docs
description: "hello avsnittet ger en översikt över hello Media Encoder Standard schemat."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 4c060062-8ef2-41d9-834e-e81e8eafcf2e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: juliako
ms.openlocfilehash: 82bad27b9546f75557ac691ff148b46990647632
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard schema
Det här avsnittet beskrivs några av hello element och typer av hello XML-schema som [Media Encoder Standard förinställningar](media-services-mes-presets-overview.md) baseras. hello avsnittet ger förklaring av element och sina giltiga värden. hello fullständig schema kommer att publiceras vid ett senare tillfälle.  

## <a name="Preset"></a>Förinställda (rotelementet)
Definierar en kodning förinställning.  

### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Kodning** |[Kodning](media-services-mes-schema.md#Encoding) |Rotelementet, indikerar att hello indatakällor toobe kodad. |
| **Utdata** |[Utdata](media-services-mes-schema.md#Output) |Samling av önskade utdatafilerna. |

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Version**<br/><br/> Krävs |**Xs:decimal** |hello förinställda version. hello gäller följande begränsningar: xs:fractionDigits värde = ”1” och xs:minInclusive value = ”1” till exempel **version = ”1.0”**. |

## <a name="Encoding"></a>Kodning
Innehåller en sekvens med hello följande element.  

### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Inställningar för H.264-kodning av video. |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Inställningar för AAC kodning av ljud. |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Inställningar för bitmappsbild. |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Inställningar för Png-bild. |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Inställningar för Jpg-bild. |

## <a name="H264Video"></a>H264Video
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs = ”0” |**Xs:Boolean** |För närvarande stöds endast en pass-kodning. |
| **KeyFrameInterval**<br/><br/> minOccurs = ”0”<br/><br/> **standard = ”00: 00:02”** |**Xs: Time** |Anger hello fast avstånd mellan IDR ramar i antal sekunder. Kallas även tooas hello GOP varaktighet. Se **SceneChangeDetection** (nedan) för att styra om hello kodare kan avvika från det här värdet. |
| **SceneChangeDetection**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”false” |**Xs:Boolean** |Om set tootrue, kodare försök toodetect scen ändra i hello videon och infogar en IDR ram. |
| **Komplexitet**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”balanserad” |**Xs:String** |Kontroller hello säkerhetsaspekten koda hastigheten och video. Kan vara någon av följande värden hello: **hastighet**, **balanserad**, eller **kvalitet**<br/><br/> Standard: **belastningsutjämnade** |
| **SyncMode**<br/><br/> minOccurs = ”0” | |Funktionen kommer att exponeras i framtida versioner. |
| **H264Layers**<br/><br/> minOccurs = ”0” |[H264Layers](media-services-mes-schema.md#H264Layers) |Samling av utdata video lager. |

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Villkor** |**Xs:String** | När hello indata har ingen bild, kanske du vill tooforce hello kodare tooinsert monokromt video spår. toodo som använder villkoret = ”InsertBlackIfNoVideoBottomLayerOnly” (tooinsert en video på endast hello lägsta bithastighet) eller villkor = ”InsertBlackIfNoVideo” (tooinsert en video på alla utdata bithastighet). Mer information finns i [detta](media-services-advanced-encoding-with-mes.md#no_video) avsnitt.|

## <a name="H264Layers"></a>H264Layers

Som standard om du skickar en inkommande toohello kodare som innehåller endast ljud och ingen bild innehåller hello utdatatillgången filer med ljud data. Vissa spelare kanske inte toohandle sådana utdataströmmar. Du kan använda hello H264Video **InsertBlackIfNoVideo** attributet inställningen tooforce hello kodare tooadd en video spåra toohello utdata i scenariot. Mer information finns i [detta](media-services-advanced-encoding-with-mes.md#no_video) avsnitt.
              
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs = ”0” maxOccurs = ”unbounded” |[H264Layer](media-services-mes-schema.md#H264Layer) |En samling H264 lager. |

## <a name="H264Layer"></a>H264Layer
> [!NOTE]
> Video gränsvärdena baseras på hello värden beskrivs i hello [H264 nivåer](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) tabell.  
> 
> 

### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”Auto” |**Xs:String** |Kan vara av en av följande hello **xs:string** värden: **automatisk**, **baslinjen**, **Main**, **hög**. |
| **Nivå**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”Auto” |**Xs:String** | |
| **Bithastighet**<br/><br/> minOccurs = ”0” |**Xs:int** |hello bithastighet används för den här videon skikt som angetts i kbit/s. |
| **MaxBitrate**<br/><br/> minOccurs = ”0” |**Xs:int** |hello maximala bithastighet används för den här videon skikt som angetts i kbit/s. |
| **BufferWindow**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”00: 00:05” |**Xs: Time** |Hello video buffertens längd. |
| **Bredd**<br/><br/> minOccurs = ”0” |**Xs:int** |Bredden på hello utdata video ram, i bildpunkter.<br/><br/> Observera att för närvarande måste du ange både bredd och höjd. hello bredd och höjd måste toobe jämnt tal. |
| **Höjd**<br/><br/> minOccurs = ”0” |**Xs:int** |Höjden på hello utdata video ram, i bildpunkter.<br/><br/> Observera att för närvarande måste du ange både bredd och höjd. hello bredd och höjd måste toobe jämnt tal.|
| **BFrames**<br/><br/> minOccurs = ”0” |**Xs:int** |Antal B ramar mellan referens ramar. |
| **ReferenceFrames**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”3” |**Xs:int** |Antal bildrutor referens i en GOP. |
| **EntropyMode**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”Cabac” |**Xs:String** |Kan vara någon av följande värden hello: **Cabac** och **Cavlc**. |
| **Ramhastighet**<br/><br/> minOccurs = ”0” |rationellt tal |Anger hello bildfrekvens hello utdata video. Använd standardvärdet ”0-1” toolet hello encoder använda hello samma ram video gradera som hello indata. Tillåtna värden är förväntade toobe vanliga bildruta priser som visas nedan. Men alla giltiga rationell tillåts. Till exempel 1/1 skulle vara 1 fps och är giltigt.<br/><br/> -12-1 (12 fps)<br/><br/> -15-1 (15 fps)<br/><br/> -24-1 (24 fps)<br/><br/> 24000/1001 (23.976 fps)<br/><br/> -25-1 (25 fps)<br/><br/>  -30-1 (30 fps)<br/><br/> 30000/1001 (29,97 fps) <br/> <br/>**Obs** om du skapar en egen förinställning för flera bithastigheter kodning sedan alla lager i hello förinställningen **måste** Använd hello samma värdet för ramhastighet.|
| **AdaptiveBFrame**<br/><br/> minOccurs = ”0” |**Xs:Boolean** |Kopiera från Azure media-kodaren |
| **Segment**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”0” |**Xs:int** |Anger hur många segment som en ram är uppdelad i. Du bör använda standard. |

## <a name="AACAudio"></a>AACAudio
 Innehåller en sekvens med hello följande element och grupper.  

 Läs mer om AAC [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs = ”0”<br/><br/> standard = ”AACLC” |**Xs:String** |Kan vara någon av följande värden hello: **AACLC**, **HEAACV1**, eller **HEAACV2**. |

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Villkor** |**Xs:String** |tooforce hello kodare tooproduce en tillgång som innehåller en tyst ljud spåra när indata har inget ljud ange hello ”InsertSilenceIfNoAudio” värde.<br/><br/> Som standard om du skickar en inkommande toohello kodare som bara innehåller video, och inget ljud innehåller sedan hello utdatatillgången filer som innehåller endast video data. Vissa spelare kanske inte toohandle sådana utdataströmmar. Du kan använda den här inställningen tooforce hello kodare tooadd en tyst ljud spåra toohello utdata i scenariot. |

### <a name="groups"></a>Grupper
| Referens | Beskrivning |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs = ”0” |Se beskrivningen av [AudioGroup](media-services-mes-schema.md#AudioGroup) tooknow hello lämpligt antal kanaler, samplingsfrekvensen och bithastighet som kan anges för varje profil. |

## <a name="AudioGroup"></a>AudioGroup
Mer information om vilka värden är giltiga för varje profil finns i hello ”ljud codec information” i tabellen nedan.  

### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Kanaler**<br/><br/> minOccurs = ”0” |**Xs:int** |hello antal ljud kanaler kodad. hello följande är giltiga alternativ: 1, 2, 5, 6, 8.<br/><br/> Standard: 2. |
| **SamplingRate**<br/><br/> minOccurs = ”0” |**Xs:int** |hello ljud samplingsfrekvensen, anges i Hz. |
| **Bithastighet**<br/><br/> minOccurs = ”0” |**Xs:int** |hello bithastighet används när kodning hello ljud anges i kbit/s. |

### <a name="audio-codec-details"></a>Ljud-codec information
Ljud-Codec|Information  
-----------------|---  
**AACLC**|1:<br/><br/> -11025: 8 &lt;= bithastighet &lt; 16<br/><br/> -12000: 8 &lt;= bithastighet &lt; 16<br/><br/> -16000: 8 &lt;= bithastighet &lt;32<br/><br/>-22050: 24 &lt;= bithastighet &lt; 32<br/><br/> -24000: 24 &lt;= bithastighet &lt; 32<br/><br/> -32000: 32 &lt;= bithastighet &lt;= 192<br/><br/> -44100: 56 &lt;= bithastighet &lt;= 288<br/><br/> -48000: 56 &lt;= bithastighet &lt;= 288<br/><br/> -88200: 128 &lt;= bithastighet &lt;= 288<br/><br/> -96000: 128 &lt;= bithastighet &lt;= 288<br/><br/> 2:<br/><br/> -11025: 16 &lt;= bithastighet &lt; 24<br/><br/> -12000: 16 &lt;= bithastighet &lt; 24<br/><br/> -16000: 16 &lt;= bithastighet &lt; 40<br/><br/> -22050: 32 &lt;= bithastighet &lt; 40<br/><br/> -24000: 32 &lt;= bithastighet &lt; 40<br/><br/> -32000: 40 &lt;= bithastighet &lt;= 384<br/><br/> -44100: 96 &lt;= bithastighet &lt;= 576<br/><br/> -48000: 96 &lt;= bithastighet &lt;= 576<br/><br/> -88200: 256 &lt;= bithastighet &lt;= 576<br/><br/> -96000: 256 &lt;= bithastighet &lt;= 576<br/><br/> 5/6:<br/><br/> -32000: 160 &lt;= bithastighet &lt;= 896<br/><br/> -44100: 240 &lt;= bithastighet &lt;= 1024<br/><br/> -48000: 240 &lt;= bithastighet &lt;= 1024<br/><br/> -88200: 640 &lt;= bithastighet &lt;= 1024<br/><br/> -96000: 640 &lt;= bithastighet &lt;= 1024<br/><br/> 8:<br/><br/> -32000: 224 &lt;= bithastighet &lt;= 1024<br/><br/> -44100: 384 &lt;= bithastighet &lt;= 1024<br/><br/> -48000: 384 &lt;= bithastighet &lt;= 1024<br/><br/> -88200: 896 &lt;= bithastighet &lt;= 1024<br/><br/> -96000: 896 &lt;= bithastighet &lt;= 1024  
**HEAACV1**|1:<br/><br/> -22050: bithastighet = 8<br/><br/> -24000: 8 &lt;= bithastighet &lt;= 10<br/><br/> -32000: 12 &lt;= bithastighet &lt;= 64<br/><br/> -44100: 20 &lt;= bithastighet &lt;= 64<br/><br/> -48000: 20 &lt;= bithastighet &lt;= 64<br/><br/> -88200: bithastighet = 64<br/><br/> 2:<br/><br/> -32000: 16 &lt;= bithastighet &lt;= 128<br/><br/> -44100: 16 &lt;= bithastighet &lt;= 128<br/><br/> -48000: 16 &lt;= bithastighet &lt;= 128<br/><br/> -88200: 96 &lt;= bithastighet &lt;= 128<br/><br/> -96000: 96 &lt;= bithastighet &lt;= 128<br/><br/> 5/6:<br/><br/> -32000: 64 &lt;= bithastighet &lt;= 320<br/><br/> -44100: 64 &lt;= bithastighet &lt;= 320<br/><br/> -48000: 64 &lt;= bithastighet &lt;= 320<br/><br/> -88200: 256 &lt;= bithastighet &lt;= 320<br/><br/> -96000: 256 &lt;= bithastighet &lt;= 320<br/><br/> 8:<br/><br/> -32000: 96 &lt;= bithastighet &lt;= 448<br/><br/> -44100: 96 &lt;= bithastighet &lt;= 448<br/><br/> -48000: 96 &lt;= bithastighet &lt;= 448<br/><br/> -88200: 384 &lt;= bithastighet &lt;= 448<br/><br/> -96000: 384 &lt;= bithastighet &lt;= 448  
**HEAACV2**|2:<br/><br/> -22050: 8 &lt;= bithastighet &lt;= 10<br/><br/> -24000: 8 &lt;= bithastighet &lt;= 10<br/><br/> -32000: 12 &lt;= bithastighet &lt;= 64<br/><br/> -44100: 20 &lt;= bithastighet &lt;= 64<br/><br/> -48000: 20 &lt;= bithastighet &lt;= 64<br/><br/> -88200: 64 &lt;= bithastighet &lt;= 64  
  

## <a name="Clip"></a>Clip
### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **StartTime** |**Duration** |Anger hello starttiden för en presentation. hello-värdet för StartTime måste toomatch hello absolut tidsstämplar hello inkommande video. Till exempel om hello första bildrutan i hello indatavideo har en tidsstämpel för 12:00:10.000 och StartTime bör vara minst 12:00:10.000 eller större. |
| **Varaktighet** |**Duration** |Anger hello varaktigheten för en presentation (till exempel utseendet på ett överlägg i hello video). |

## <a name="Output"></a>Utdata
### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Filnamn** |**Xs:String** |hello namnet på utdatafilen hello.<br/><br/> Du kan använda makron som beskrivs i hello efter tabellen toobuild hello utdata-filnamn. Exempel:<br/><br/> **”Utdata”: [{”FileName” ”: {Basename}*{upplösning}*{bithastighet} .mp4”, ”Format”: {”typ”: ”MP4Format”}}] ** |

### <a name="macros"></a>Makron
| Makrot | Beskrivning |
| --- | --- |
| **{Basename}** |Om du gör VoD kodning, är hello {Basename} hello först 32 tecken på hello AssetFile.Name egenskap hello primärfilen i hello inkommande tillgången.<br/><br/> Om hello indata är ett live-Arkiv, sedan härleds hello {Basename} från hello trackName attribut i hello server manifest. Om du skickar ett underklipp jobb med hjälp av hello TopBitrate, som i ”: < VideoStream\>TopBitrate < / VideoStream\>”, och hello utdatafilen innehåller video och sedan hello {Basename} är hello hello trackName av hello första 32 tecken video lager med hello högsta bithastighet.<br/><br/> Om du skickar i stället ett underklipp jobb med hjälp av alla hello inkommande bithastighet, exempelvis ”< VideoStream\>* < / VideoStream\>”, och hello utdatafilen innehåller video och sedan {Basename} är hello först 32 tecken för hello trackName av hello motsvarande video lager. |
| **{Codec}** |Mappar för ”H264” video och ”AAC” för ljud. |
| **{Bithastighet}** |hello önskad video bithastighet om hello utdatafilen innehåller video och ljud eller mål ljud bithastighet om hello utdatafilen innehåller endast ljud. hello-värde som används är bithastighet hello i kbit/s. |
| **{Kanal}** |Antal ljud kanaler om hello-filen innehåller ljud. |
| **{Bredd}** |Bredden på hello video, i bildpunkter i hello utdatafilen om hello-filen innehåller video. |
| **{Höjd}** |Höjden på hello video, i bildpunkter i hello utdatafilen om hello-filen innehåller video. |
| **{Tillägg}** |Ärver från hello ”typ”-egenskapen för hello utdatafilen. namnet hello utdatafilen har ett tillägg som är en av: ”mp4”, ”ts”, ”jpg”, ”png” eller ”bmp”. |
| **{Index}** |Obligatorisk för miniatyr. Ska endast finnas en gång. |

## <a name="Video"></a>Video (komplex typ ärver från Codec)
### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Börja** |**Xs:String** | |
| **Steg** |**Xs:String** | |
| **Intervallet** |**Xs:String** | |
| **PreserveResolutionAfterRotation** |**Xs:Boolean** |Mer detaljerad förklaring finns hello efter avsnittet: [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation) |

### <a name="PreserveResolutionAfterRotation"></a>PreserveResolutionAfterRotation
Det rekommenderas toouse hello PreserveResolutionAfterRotation flaggan i kombination med upplösningar uttryckt i procent (Width = ”100%”, höjd = ”100%”).  

Som standard koda hello upplösning (bredden och höjden) i hello Media Encoder Standard (MES) förinställningar riktas mot videor med rotation 0 grader. Till exempel om din indatavideo är minst 1 280 x 720 med noll grad rotation, sedan hello standardförinställningar se till att hello utdata har hello samma upplösning. Se bilden nedan.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Men innebär det att om hello indatavideo har spelats in med noll kan rotera (t.ex. en smartphone eller surfplatta hålls lodrätt), och sedan MES som standard gäller hello koda upplösning inställningar (bredden och höjden) toohello indata video och sedan kompensera för hello rotation. Se exempelvis hello bilden nedan. hello förinställda använder Width = ”100%”, höjd = ”100%”, som MES tolkas som att kräva hello utdata toobe minst 1 280 bildpunkter och 720 bildpunkter. När du roterar hello video, sedan krymper hello bild toofit till det fönstret inledande toopillar-områdena på hello vänster och höger.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Om hello ovan är inte hello önskat beteende, så kan du använda hello PreserveResolutionAfterRotation flaggan och ange den för ”true” (standardvärdet är ”false”). Så om din förinställda har Width = ”100%”, höjd = ”100%” och värdet för ”SANT”, en indatavideo som är minst 1 280 bildpunkter och 720 bildpunkter med 90-gradiga rotation genererar utdata med noll grad rotation, men 720 bildpunkter bred och 1280 PreserveResolutionAfterRotation bildpunkter. Se hello bilden nedan.  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="FormatGroup"></a>FormatGroup (grupp)
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="BmpLayer"></a>BmpLayer
### <a name="element"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Bredd**<br/><br/> minOccurs = ”0” |**Xs:int** | |
| **Höjd**<br/><br/> minOccurs = ”0” |**Xs:int** | |

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Villkor** |**Xs:String** | |

## <a name="PngLayer"></a>PngLayer
### <a name="element"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Bredd**<br/><br/> minOccurs = ”0” |**Xs:int** | |
| **Höjd**<br/><br/> minOccurs = ”0” |**Xs:int** | |

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Villkor** |**Xs:String** | |

## <a name="JpgLayer"></a>JpgLayer
### <a name="element"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Bredd**<br/><br/> minOccurs = ”0” |**Xs:int** | |
| **Höjd**<br/><br/> minOccurs = ”0” |**Xs:int** | |
| **Kvalitet**<br/><br/> minOccurs = ”0” |**Xs:int** |Giltiga värden: 1(worst)-100(best) |

### <a name="attributes"></a>Attribut
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **Villkor** |**Xs:String** | |

## <a name="PngLayers"></a>PngLayers
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs = ”0” maxOccurs = ”unbounded” |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="BmpLayers"></a>BmpLayers
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs = ”0” maxOccurs = ”unbounded” |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="JpgLayers"></a>JpgLayers
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs = ”0” maxOccurs = ”unbounded” |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="BmpImage"></a>BmpImage (komplex typ ärver från Video)
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = ”0” |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lager |

## <a name="JpgImage"></a>JpgImage (komplex typ ärver från Video)
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = ”0” |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lager |

## <a name="PngImage"></a>PngImage (komplex typ ärver från Video)
### <a name="elements"></a>Element
| Namn | Typ | Beskrivning |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs = ”0” |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-lager |

## <a name="examples"></a>Exempel
Finns exempel på XML-förinställda som skapas utifrån det här schemat finns [aktivitet förinställningar för MES (Media Encoder Standard)](media-services-mes-presets-overview.md).

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

