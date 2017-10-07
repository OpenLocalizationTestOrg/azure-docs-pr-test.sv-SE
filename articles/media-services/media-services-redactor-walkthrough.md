---
title: "aaaRedact personerna bakom Azure Media Analytics genomgången | Microsoft Docs"
description: "Det här avsnittet innehåller steg-för-steg-instruktioner om hur toorun en fullständig bortredigering arbetsflöde med hjälp av Azure Media Services Explorer (AMSE) och Azure Media Redactor Visualizer (Verktyg för öppen källkod)."
services: media-services
documentationcenter: 
author: Lichard
manager: cfowler
editor: 
ms.assetid: d6fa21b8-d80a-41b7-80c1-ff1761bc68f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/03/2017
ms.author: rli; juliako;
ms.openlocfilehash: ab28f4052b73fdb74fcd5766235eab35402a0c9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a>Redigera bort personerna bakom Azure Media Analytics genomgång

## <a name="overview"></a>Översikt

**Azure Media Redactor** är en [Azure Media Analytics](media-services-analytics-overview.md) medieprocessor (HP) som erbjuder skalbara ansikte bortredigering i hello molnet. Framsidan bortredigering aktiverar du toomodify videon i ordning tooblur ytor valda enskilda användare. Du kanske vill toouse hello ansikte bortredigering service i offentliga säkerhet och nyheter media scenarier. Några minuter med material som innehåller flera ytor kan ta timmar tooredact manuellt, men med den här tjänsten hello ansikte bortredigering processen tar bara några få enkla steg. Mer information finns i [detta](https://azure.microsoft.com/blog/azure-media-redactor/) blogg.

Mer information om **Azure Media Redactor**, se hello [ansikte bortredigering översikt](media-services-face-redaction.md) avsnittet.

Det här avsnittet innehåller steg-för-steg-instruktioner om hur toorun en fullständig bortredigering arbetsflöde med hjälp av Azure Media Services Explorer (AMSE) och Azure Media Redactor Visualizer (Verktyg för öppen källkod).

Hej **Azure Media Redactor** MP är för närvarande under förhandsgranskning. Den är tillgänglig i alla offentliga Azure-regioner som tillhör amerikanska myndigheter och Kina datacenter. Den här förhandsgranskningen är för närvarande kostnadsfritt. Hello aktuella versionen, att det finns en gräns för 10 minuter på bearbetade video längd.

Mer information finns i [detta](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blogg.

## <a name="azure-media-services-explorer-workflow"></a>Azure Media Services Explorer-arbetsflöde

hello enklaste sättet tooget igång med Redactor är toouse hello öppen källkod AMSE-verktyget på github. Du kan köra en förenklad arbetsflöde via **kombinerade** läge om du inte behöver komma åt toohello anteckningen json eller hello ansikte jpg-bilder.

### <a name="download-and-setup"></a>Hämtning och installation

1. Hämta hello AMSE-verktyget från [här](https://github.com/Azure/Azure-Media-Services-Explorer).
1. Logga in tooyour Media Services-konto med nyckeln för tjänsten.

    tooobtain hello kontonamn och nyckelinformation, gå toohello [Azure-portalen](https://portal.azure.com/) och välj AMS-kontot. Välj Inställningar > nycklar. hello hantera nycklar windows visar hello kontonamn och hello primära och sekundära nycklarna. Kopiera värdena för hello kontonamn och hello primärnyckel.

![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a>Först skickar – analysera läge

1. Överför mediefiler via tillgången –> överföra, eller via dra och släpp. 
1. Högerklicka och bearbeta filen media med hjälp av Media Analytics –> Azure Media Redactor –> analysera läge. 


![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

hello utdata innehåller en anteckningar json-fil med min platsdata, samt jpg av varje identifierad står inför. 

![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a>Andra skicka – redigera bort läge

1. Ladda upp din ursprungliga videotillgång toohello utdata från hello första steget och ange som en primär tillgång. 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. (Valfritt) Överför en 'Dance_idlist.txt'-fil som innehåller en ny rad avgränsad lista över hello ID: N som du vill tooredact. 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. (Valfritt) Gör eventuella ändringar toohello annotations.json fil, till exempel öka hello omgivande gränserna. 
4. Högerklicka på hello utdatatillgången från hello första pass hello Redactor och välj kör med hello **Redact** läge. 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. Hämta eller dela hello omarbetade slutversionen tillgången. 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a>Azure Media Redactor Visualizer öppen källkod verktyget

En öppen källkod [visualizer verktyget](https://github.com/Microsoft/azure-media-redactor-visualizer) är utformad toohelp utvecklare som precis har startats med hello anteckningar format med hjälp av hello utdata och parsning.

När du klonar hello lagringsplatsen i ordning toorun hello-projektet måste toodownload FFMPEG från deras [officiella platsen](https://ffmpeg.org/download.html).

Om du utvecklar försök tooparse hello JSON anteckningsdata, kan du titta på Models.MetaData exempel kodexempel.

### <a name="set-up-hello-tool"></a>Ställa in hello-verktyg

1.  Hämta och skapa hello hela lösningen. 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  Hämta FFMPEG från [här](https://ffmpeg.org/download.html). Det här projektet ursprungligen utvecklades med version be1d324 (2016-10-04) med statisk länkning. 
3.  Kopiera ffmpeg.exe och ffprobe.exe toohello samma utdatamapp som AzureMediaRedactor.exe. 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. Kör AzureMediaRedactor.exe. 

### <a name="use-hello-tool"></a>Verktyget hello

1. Bearbeta videon i Azure Media Services-kontot med hello Redactor MP på Analysera läge. 
2. Hämta både hello ursprungliga videofil och hello utdata från hello bortredigering - analysera jobb. 
3. Kör hello visualizer programmet och välj hello filerna ovan. 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. Förhandsgranska din fil. Välj vilka står du vill tooblur via hello sidopanelen på hello rätt. 
    
    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  hello nedre textfält uppdateras med hello ansikte ID: N. Skapa en fil med namnet ”idlist.txt” med dessa ID: N som en ny rad avgränsad lista. 

    >[!NOTE]
    > Hej idlist.txt ska sparas i ANSI. Du kan använda anteckningar toosave i ANSI.
    
6.  Överför den här filen toohello utdatatillgången från steg 1. Överför hello ursprungliga video toothis tillgången samt och ange som primär tillgång. 
7.  Kör bortredigering jobb på tillgången med ”Redact” läge tooget hello slutliga omarbetade video. 

## <a name="next-steps"></a>Nästa steg 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Relaterade länkar
[Azure Media Services Analytics-översikt](media-services-analytics-overview.md)

[Azure Media Analytics demonstrationer](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[Om ansikte bortredigering för Azure Media Analytics](https://azure.microsoft.com/blog/azure-media-redactor/)
