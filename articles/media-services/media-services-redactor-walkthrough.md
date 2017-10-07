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
# <a name="redact-faces-with-azure-media-analytics-walkthrough"></a><span data-ttu-id="16df7-103">Redigera bort personerna bakom Azure Media Analytics genomgång</span><span class="sxs-lookup"><span data-stu-id="16df7-103">Redact faces with Azure Media Analytics walkthrough</span></span>

## <a name="overview"></a><span data-ttu-id="16df7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="16df7-104">Overview</span></span>

<span data-ttu-id="16df7-105">**Azure Media Redactor** är en [Azure Media Analytics](media-services-analytics-overview.md) medieprocessor (HP) som erbjuder skalbara ansikte bortredigering i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="16df7-105">**Azure Media Redactor** is an [Azure Media Analytics](media-services-analytics-overview.md) media processor (MP) that offers scalable face redaction in hello cloud.</span></span> <span data-ttu-id="16df7-106">Framsidan bortredigering aktiverar du toomodify videon i ordning tooblur ytor valda enskilda användare.</span><span class="sxs-lookup"><span data-stu-id="16df7-106">Face redaction enables you toomodify your video in order tooblur faces of selected individuals.</span></span> <span data-ttu-id="16df7-107">Du kanske vill toouse hello ansikte bortredigering service i offentliga säkerhet och nyheter media scenarier.</span><span class="sxs-lookup"><span data-stu-id="16df7-107">You may want toouse hello face redaction service in public safety and news media scenarios.</span></span> <span data-ttu-id="16df7-108">Några minuter med material som innehåller flera ytor kan ta timmar tooredact manuellt, men med den här tjänsten hello ansikte bortredigering processen tar bara några få enkla steg.</span><span class="sxs-lookup"><span data-stu-id="16df7-108">A few minutes of footage that contains multiple faces can take hours tooredact manually, but with this service hello face redaction process will require just a few simple steps.</span></span> <span data-ttu-id="16df7-109">Mer information finns i [detta](https://azure.microsoft.com/blog/azure-media-redactor/) blogg.</span><span class="sxs-lookup"><span data-stu-id="16df7-109">For  more information, see [this](https://azure.microsoft.com/blog/azure-media-redactor/) blog.</span></span>

<span data-ttu-id="16df7-110">Mer information om **Azure Media Redactor**, se hello [ansikte bortredigering översikt](media-services-face-redaction.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="16df7-110">For details about  **Azure Media Redactor**, see hello [Face redaction overview](media-services-face-redaction.md) topic.</span></span>

<span data-ttu-id="16df7-111">Det här avsnittet innehåller steg-för-steg-instruktioner om hur toorun en fullständig bortredigering arbetsflöde med hjälp av Azure Media Services Explorer (AMSE) och Azure Media Redactor Visualizer (Verktyg för öppen källkod).</span><span class="sxs-lookup"><span data-stu-id="16df7-111">This topic shows step by step instructions on how toorun a full redaction workflow using Azure Media Services Explorer (AMSE) and Azure Media Redactor Visualizer (open source tool).</span></span>

<span data-ttu-id="16df7-112">Hej **Azure Media Redactor** MP är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="16df7-112">hello **Azure Media Redactor** MP is currently in Preview.</span></span> <span data-ttu-id="16df7-113">Den är tillgänglig i alla offentliga Azure-regioner som tillhör amerikanska myndigheter och Kina datacenter.</span><span class="sxs-lookup"><span data-stu-id="16df7-113">It is available in all public Azure regions as well as US Government and China data centers.</span></span> <span data-ttu-id="16df7-114">Den här förhandsgranskningen är för närvarande kostnadsfritt.</span><span class="sxs-lookup"><span data-stu-id="16df7-114">This preview is currently free of charge.</span></span> <span data-ttu-id="16df7-115">Hello aktuella versionen, att det finns en gräns för 10 minuter på bearbetade video längd.</span><span class="sxs-lookup"><span data-stu-id="16df7-115">In hello current release, there is a 10 minute limit on processed video length.</span></span>

<span data-ttu-id="16df7-116">Mer information finns i [detta](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blogg.</span><span class="sxs-lookup"><span data-stu-id="16df7-116">For more information, see [this](https://azure.microsoft.com/en-us/blog/redaction-preview-available-globally) blog.</span></span>

## <a name="azure-media-services-explorer-workflow"></a><span data-ttu-id="16df7-117">Azure Media Services Explorer-arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="16df7-117">Azure Media Services Explorer workflow</span></span>

<span data-ttu-id="16df7-118">hello enklaste sättet tooget igång med Redactor är toouse hello öppen källkod AMSE-verktyget på github.</span><span class="sxs-lookup"><span data-stu-id="16df7-118">hello easiest way tooget started with Redactor is toouse hello open source AMSE tool on github.</span></span> <span data-ttu-id="16df7-119">Du kan köra en förenklad arbetsflöde via **kombinerade** läge om du inte behöver komma åt toohello anteckningen json eller hello ansikte jpg-bilder.</span><span class="sxs-lookup"><span data-stu-id="16df7-119">You can run a simplified workflow via **combined** mode if you don’t need access toohello annotation json or hello face jpg images.</span></span>

### <a name="download-and-setup"></a><span data-ttu-id="16df7-120">Hämtning och installation</span><span class="sxs-lookup"><span data-stu-id="16df7-120">Download and setup</span></span>

1. <span data-ttu-id="16df7-121">Hämta hello AMSE-verktyget från [här](https://github.com/Azure/Azure-Media-Services-Explorer).</span><span class="sxs-lookup"><span data-stu-id="16df7-121">Download hello AMSE tool from [here](https://github.com/Azure/Azure-Media-Services-Explorer).</span></span>
1. <span data-ttu-id="16df7-122">Logga in tooyour Media Services-konto med nyckeln för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="16df7-122">Log in tooyour Media Services account using your service key.</span></span>

    <span data-ttu-id="16df7-123">tooobtain hello kontonamn och nyckelinformation, gå toohello [Azure-portalen](https://portal.azure.com/) och välj AMS-kontot.</span><span class="sxs-lookup"><span data-stu-id="16df7-123">tooobtain hello account name and key information, go toohello [Azure portal](https://portal.azure.com/) and select your AMS account.</span></span> <span data-ttu-id="16df7-124">Välj Inställningar > nycklar.</span><span class="sxs-lookup"><span data-stu-id="16df7-124">Then, select Settings > Keys.</span></span> <span data-ttu-id="16df7-125">hello hantera nycklar windows visar hello kontonamn och hello primära och sekundära nycklarna.</span><span class="sxs-lookup"><span data-stu-id="16df7-125">hello Manage keys windows shows hello account name and hello primary and secondary keys is displayed.</span></span> <span data-ttu-id="16df7-126">Kopiera värdena för hello kontonamn och hello primärnyckel.</span><span class="sxs-lookup"><span data-stu-id="16df7-126">Copy values of hello account name and hello primary key.</span></span>

![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough001.png)

### <a name="first-pass--analyze-mode"></a><span data-ttu-id="16df7-128">Först skickar – analysera läge</span><span class="sxs-lookup"><span data-stu-id="16df7-128">First pass – analyze mode</span></span>

1. <span data-ttu-id="16df7-129">Överför mediefiler via tillgången –> överföra, eller via dra och släpp.</span><span class="sxs-lookup"><span data-stu-id="16df7-129">Upload your media file through Asset –> Upload, or via drag and drop.</span></span> 
1. <span data-ttu-id="16df7-130">Högerklicka och bearbeta filen media med hjälp av Media Analytics –> Azure Media Redactor –> analysera läge.</span><span class="sxs-lookup"><span data-stu-id="16df7-130">Right click and process your media file using Media Analytics –> Azure Media Redactor –> Analyze mode.</span></span> 


![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough002.png)

![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough003.png)

<span data-ttu-id="16df7-133">hello utdata innehåller en anteckningar json-fil med min platsdata, samt jpg av varje identifierad står inför.</span><span class="sxs-lookup"><span data-stu-id="16df7-133">hello output will include an annotations json file with face location data, as well as a jpg of each detected face.</span></span> 

![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough004.png)

###<a name="second-pass--redact-mode"></a><span data-ttu-id="16df7-135">Andra skicka – redigera bort läge</span><span class="sxs-lookup"><span data-stu-id="16df7-135">Second pass – redact mode</span></span>

1. <span data-ttu-id="16df7-136">Ladda upp din ursprungliga videotillgång toohello utdata från hello första steget och ange som en primär tillgång.</span><span class="sxs-lookup"><span data-stu-id="16df7-136">Upload your original video asset toohello output from hello first pass and set as a primary asset.</span></span> 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough005.png)

2. <span data-ttu-id="16df7-138">(Valfritt) Överför en 'Dance_idlist.txt'-fil som innehåller en ny rad avgränsad lista över hello ID: N som du vill tooredact.</span><span class="sxs-lookup"><span data-stu-id="16df7-138">(Optional) Upload a 'Dance_idlist.txt' file which includes a newline delimited list of hello IDs you wish tooredact.</span></span> 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough006.png)

3. <span data-ttu-id="16df7-140">(Valfritt) Gör eventuella ändringar toohello annotations.json fil, till exempel öka hello omgivande gränserna.</span><span class="sxs-lookup"><span data-stu-id="16df7-140">(Optional) Make any edits toohello annotations.json file such as increasing hello bounding box boundaries.</span></span> 
4. <span data-ttu-id="16df7-141">Högerklicka på hello utdatatillgången från hello första pass hello Redactor och välj kör med hello **Redact** läge.</span><span class="sxs-lookup"><span data-stu-id="16df7-141">Right click hello output asset from hello first pass, select hello Redactor, and run with hello **Redact** mode.</span></span> 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough007.png)

5. <span data-ttu-id="16df7-143">Hämta eller dela hello omarbetade slutversionen tillgången.</span><span class="sxs-lookup"><span data-stu-id="16df7-143">Download or share hello final redacted output asset.</span></span> 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough008.png)

##<a name="azure-media-redactor-visualizer-open-source-tool"></a><span data-ttu-id="16df7-145">Azure Media Redactor Visualizer öppen källkod verktyget</span><span class="sxs-lookup"><span data-stu-id="16df7-145">Azure Media Redactor Visualizer open source tool</span></span>

<span data-ttu-id="16df7-146">En öppen källkod [visualizer verktyget](https://github.com/Microsoft/azure-media-redactor-visualizer) är utformad toohelp utvecklare som precis har startats med hello anteckningar format med hjälp av hello utdata och parsning.</span><span class="sxs-lookup"><span data-stu-id="16df7-146">An open source [visualizer tool](https://github.com/Microsoft/azure-media-redactor-visualizer) is designed toohelp developers just starting with hello annotations format with parsing and using hello output.</span></span>

<span data-ttu-id="16df7-147">När du klonar hello lagringsplatsen i ordning toorun hello-projektet måste toodownload FFMPEG från deras [officiella platsen](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="16df7-147">After you clone hello repo, in order toorun hello project, you will need toodownload FFMPEG from their [official site](https://ffmpeg.org/download.html).</span></span>

<span data-ttu-id="16df7-148">Om du utvecklar försök tooparse hello JSON anteckningsdata, kan du titta på Models.MetaData exempel kodexempel.</span><span class="sxs-lookup"><span data-stu-id="16df7-148">If you are a developer trying tooparse hello JSON annotation data, look inside Models.MetaData for sample code examples.</span></span>

### <a name="set-up-hello-tool"></a><span data-ttu-id="16df7-149">Ställa in hello-verktyg</span><span class="sxs-lookup"><span data-stu-id="16df7-149">Set up hello tool</span></span>

1.  <span data-ttu-id="16df7-150">Hämta och skapa hello hela lösningen.</span><span class="sxs-lookup"><span data-stu-id="16df7-150">Download and build hello entire solution.</span></span> 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough009.png)

2.  <span data-ttu-id="16df7-152">Hämta FFMPEG från [här](https://ffmpeg.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="16df7-152">Download FFMPEG from [here](https://ffmpeg.org/download.html).</span></span> <span data-ttu-id="16df7-153">Det här projektet ursprungligen utvecklades med version be1d324 (2016-10-04) med statisk länkning.</span><span class="sxs-lookup"><span data-stu-id="16df7-153">This project was originally developed with version be1d324 (2016-10-04) with static linking.</span></span> 
3.  <span data-ttu-id="16df7-154">Kopiera ffmpeg.exe och ffprobe.exe toohello samma utdatamapp som AzureMediaRedactor.exe.</span><span class="sxs-lookup"><span data-stu-id="16df7-154">Copy ffmpeg.exe and ffprobe.exe toohello same output folder as AzureMediaRedactor.exe.</span></span> 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough010.png)

4. <span data-ttu-id="16df7-156">Kör AzureMediaRedactor.exe.</span><span class="sxs-lookup"><span data-stu-id="16df7-156">Run AzureMediaRedactor.exe.</span></span> 

### <a name="use-hello-tool"></a><span data-ttu-id="16df7-157">Verktyget hello</span><span class="sxs-lookup"><span data-stu-id="16df7-157">Use hello tool</span></span>

1. <span data-ttu-id="16df7-158">Bearbeta videon i Azure Media Services-kontot med hello Redactor MP på Analysera läge.</span><span class="sxs-lookup"><span data-stu-id="16df7-158">Process your video in your Azure Media Services account with hello Redactor MP on Analyze mode.</span></span> 
2. <span data-ttu-id="16df7-159">Hämta både hello ursprungliga videofil och hello utdata från hello bortredigering - analysera jobb.</span><span class="sxs-lookup"><span data-stu-id="16df7-159">Download both hello original video file and hello output of hello Redaction - Analyze job.</span></span> 
3. <span data-ttu-id="16df7-160">Kör hello visualizer programmet och välj hello filerna ovan.</span><span class="sxs-lookup"><span data-stu-id="16df7-160">Run hello visualizer application and choose hello files above.</span></span> 

    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough011.png)

4. <span data-ttu-id="16df7-162">Förhandsgranska din fil.</span><span class="sxs-lookup"><span data-stu-id="16df7-162">Preview your file.</span></span> <span data-ttu-id="16df7-163">Välj vilka står du vill tooblur via hello sidopanelen på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="16df7-163">Select which faces you'd like tooblur via hello sidebar on hello right.</span></span> 
    
    ![Ansiktsredigering](./media/media-services-redactor-walkthrough/media-services-redactor-walkthrough012.png)

5.  <span data-ttu-id="16df7-165">hello nedre textfält uppdateras med hello ansikte ID: N.</span><span class="sxs-lookup"><span data-stu-id="16df7-165">hello bottom text field will update with hello face IDs.</span></span> <span data-ttu-id="16df7-166">Skapa en fil med namnet ”idlist.txt” med dessa ID: N som en ny rad avgränsad lista.</span><span class="sxs-lookup"><span data-stu-id="16df7-166">Create a file called "idlist.txt" with these IDs as a newline delimited list.</span></span> 

    >[!NOTE]
    > <span data-ttu-id="16df7-167">Hej idlist.txt ska sparas i ANSI.</span><span class="sxs-lookup"><span data-stu-id="16df7-167">hello idlist.txt should be saved in ANSI.</span></span> <span data-ttu-id="16df7-168">Du kan använda anteckningar toosave i ANSI.</span><span class="sxs-lookup"><span data-stu-id="16df7-168">You can use notepad toosave in ANSI.</span></span>
    
6.  <span data-ttu-id="16df7-169">Överför den här filen toohello utdatatillgången från steg 1.</span><span class="sxs-lookup"><span data-stu-id="16df7-169">Upload this file toohello output asset from step 1.</span></span> <span data-ttu-id="16df7-170">Överför hello ursprungliga video toothis tillgången samt och ange som primär tillgång.</span><span class="sxs-lookup"><span data-stu-id="16df7-170">Upload hello original video toothis asset as well and set as primary asset.</span></span> 
7.  <span data-ttu-id="16df7-171">Kör bortredigering jobb på tillgången med ”Redact” läge tooget hello slutliga omarbetade video.</span><span class="sxs-lookup"><span data-stu-id="16df7-171">Run Redaction job on this asset with "Redact" mode tooget hello final redacted video.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="16df7-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16df7-172">Next steps</span></span> 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="16df7-173">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="16df7-173">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a><span data-ttu-id="16df7-174">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="16df7-174">Related links</span></span>
[<span data-ttu-id="16df7-175">Azure Media Services Analytics-översikt</span><span class="sxs-lookup"><span data-stu-id="16df7-175">Azure Media Services Analytics Overview</span></span>](media-services-analytics-overview.md)

[<span data-ttu-id="16df7-176">Azure Media Analytics demonstrationer</span><span class="sxs-lookup"><span data-stu-id="16df7-176">Azure Media Analytics demos</span></span>](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

[<span data-ttu-id="16df7-177">Om ansikte bortredigering för Azure Media Analytics</span><span class="sxs-lookup"><span data-stu-id="16df7-177">Announcing Face Redaction for Azure Media Analytics</span></span>](https://azure.microsoft.com/blog/azure-media-redactor/)
