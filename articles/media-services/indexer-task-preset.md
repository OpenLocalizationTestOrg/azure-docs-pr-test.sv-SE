---
title: "aaaTask förinställningen Azure Media indexer"
description: "Det här avsnittet ger en översikt över aktivitet förinställningen Azure Media indexer."
services: media-services
documentationcenter: 
author: Asolanki
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/03/2017
ms.author: adsolank;juliako;
ms.openlocfilehash: ca0b3e7aa9f6dd9fdecddfc5b3137281ed5cef35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="task-preset-for-azure-media-indexer"></a>Uppgiften förinställningen Azure Media indexer

Azure Media Indexer är en Medieprocessor att du använder tooperform hello följande uppgifter: Se mediefiler och innehåll sökbara, generera stängd textning spårar och nyckelord, index tillgångsfiler som ingår i din tillgång.

Det här avsnittet beskrivs hello uppgiften förinställningen måste toopass tooyour indexering jobb. Komplett exempel finns [indexering mediefiler med Azure Media Indexer](media-services-index-content.md).

## <a name="azure-media-indexer-configuration-xml"></a>Azure Media Indexer konfigurations-XML

hello följande tabell förklarar elementen och attributen i hello konfigurations-XML.

|Namn|Kräv|Beskrivning|
|---|---|---|
|Indata|SANT|Tillgångsinformation fil(er) som du vill tooindex.<br/>Azure Media Indexer stöder hello följande format: MP4, MOV, WMV, MP3, M4A, WMA, AAC, WAV. <br/><br/>Du kan ange hello filnamn (s) i hello **namn** eller **lista** attribut för hello **inkommande** element (som visas nedan). Om du inte anger vilka tillgången filen tooindex plockats hello primära filen. Om inga primära resursfilen anges indexeras hello första filen i hello inkommande tillgången.<br/><br/>tooexplicitly anger hello tillgången filnamn, gör du:<br/>```<input name="TestFile.wmv" />```<br/><br/>Du kan även indexera flera tillgångsfiler samtidigt (upp too10-filer). toodo detta:<br/>-Skapa en textfil (manifestfilen) och ge den ett .lst-tillägg.<br/>-Lägg till en lista över alla hello tillgången filnamn i inkommande tillgången toothis manifestfilen.<br/>-Lägg till (överföringen) thanifest filen toohello tillgången.<br/>-Ange hello namnet på manifestfilen hello i hello indata-attribut.<br/>```<input list="input.lst">```<br/><br/>**Obs:** om du lägger till fler än 10 filer toohello manifestfilen hello indexering jobb misslyckas med felkoden hello 2006.|
|Metadata|FALSKT|Metadata för hello angivna tillgången filer.<br/>```<metadata key="..." value="..." />```<br/><br/>Du kan ange värden för fördefinierade nycklar. <br/><br/>För närvarande stöds hello följande nycklar:<br/><br/>**Rubrik** och **beskrivning** -används taligenkänningen för tooupdate hello språk modellen tooimprove.<br/>```<metadata key="title" value="[Title of hello media file]" /><metadata key="description" value="[Description of hello media file]" />```<br/><br/>**användarnamnet** och **lösenord** – används för autentisering när du hämtar Internetfiler via http eller https.<br/>```<metadata key="username" value="[UserName]" /><metadata key="password" value="[Password]" />```<br/>Hej användarnamn och lösenord gäller tooall media URL: er i hello inkommande manifest.|
|funktioner<br/><br/>Lägga till i version 1.2. Hello stöds endast funktionen är för närvarande taligenkänning (”ASR”).|FALSKT|hello taligenkänning har hello följande inställningar nycklar:<br/><br/>Språk:<br/>-hello naturligt språk toobe identifieras i hello fil.<br/>-Engelska, spanska<br/><br/>CaptionFormats:<br/>-en semikolonavgränsad lista över hello önskad rubrik utdataformat (eventuella)<br/>-ttml; sami; webvtt<br/><br/><br/>GenerateAIB:<br/>-En boolesk flagga som anger huruvida en AIB fil krävs (för användning med SQL Server och hello kunden indexeraren IFilter). Mer information finns i använda AIB filer med Azure Media Indexer och SQL Server.<br/>-SANT. FALSKT<br/><br/>GenerateKeywords:<br/>-En boolesk flagga som anger huruvida en nyckelordet XML-fil måste anges.<br/>-SANT. FALSKT.|

## <a name="azure-media-indexer-configuration-xml-example"></a>Azure Media Indexer configuration XML-exempel

``` 
<?xml version="1.0" encoding="utf-8"?>  
<configuration version="2.0">  
  <input>  
    <metadata key="title" value="[Title of hello media file]" />  
    <metadata key="description" value="[Description of hello media file]" />  
  </input>  
  <settings>  
  </settings>  
  
  <features>  
    <feature name="ASR">    
      <settings>  
        <add key="Language" value="English"/>  
        <add key="CaptionFormats" value="ttml;sami;webvtt"/>  
        <add key="GenerateAIB" value ="true" />  
        <add key="GenerateKeywords" value ="true" />  
      </settings>  
    </feature>  
  </features>  
  
</configuration>  
```
  
## <a name="next-steps"></a>Nästa steg

Se [indexering mediefiler med Azure Media Indexer](media-services-index-content.md).

