---
title: "aaaTroubleshooting guide för direktsänd strömning | Microsoft Docs"
description: "Det här avsnittet innehåller förslag på hur tootroubleshoot bor strömmande problem."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 8549bae947ff3b225ce624220d1e48b63f90208c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-live-streaming"></a><span data-ttu-id="de71d-103">Felsökningsguide för liveuppspelning</span><span class="sxs-lookup"><span data-stu-id="de71d-103">Troubleshooting guide for live streaming</span></span>
<span data-ttu-id="de71d-104">Det här avsnittet innehåller förslag på hur tootroubleshoot vissa direktsänd strömning problem.</span><span class="sxs-lookup"><span data-stu-id="de71d-104">This topic gives suggestions on how tootroubleshoot some live streaming problems.</span></span>

## <a name="issues-related-tooon-premises-encoders"></a><span data-ttu-id="de71d-105">Problem med tooon lokala kodare</span><span class="sxs-lookup"><span data-stu-id="de71d-105">Issues related tooon-premises encoders</span></span>
<span data-ttu-id="de71d-106">Det här avsnittet innehåller förslag på hur tootroubleshoot problem relaterade tooon lokala kodare som är konfigurerat toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding.</span><span class="sxs-lookup"><span data-stu-id="de71d-106">This section gives suggestions on how tootroubleshoot problems related tooon-premises encoders that are configured toosend a single bitrate stream tooAMS channels that are enabled for live encoding.</span></span>

### <a name="problem-would-like-toosee-logs"></a><span data-ttu-id="de71d-107">Problem: Vill toosee loggar</span><span class="sxs-lookup"><span data-stu-id="de71d-107">Problem: Would like toosee logs</span></span>
* <span data-ttu-id="de71d-108">**Potentiella problem**: Det går inte att hitta kodare loggar som kan bidra till felsökning av problem.</span><span class="sxs-lookup"><span data-stu-id="de71d-108">**Potential issue**: Can't find encoder logs that might help in debugging issues.</span></span>
  
  * <span data-ttu-id="de71d-109">**Telestream Wirecast**: vanligtvis finns det loggar under C:\Users\{username} \AppData\Roaming\Wirecast\\</span><span class="sxs-lookup"><span data-stu-id="de71d-109">**Telestream Wirecast**: You can usually find logs under C:\Users\{username}\AppData\Roaming\Wirecast\\</span></span> 
  * <span data-ttu-id="de71d-110">**Grundämne Live**: du kan hitta har länkar toologs på hello-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="de71d-110">**Elemental Live**: You can find has links toologs on hello management portal.</span></span> <span data-ttu-id="de71d-111">Klicka på **Stats**, sedan **loggar**.</span><span class="sxs-lookup"><span data-stu-id="de71d-111">Click on **Stats**, then **Logs**.</span></span> <span data-ttu-id="de71d-112">På hello **loggfiler** sidan visas en lista över loggar för alla hello LiveEvent objekt; Välj hello en matchande den aktuella sessionen.</span><span class="sxs-lookup"><span data-stu-id="de71d-112">On hello **Log Files** page, you will see a list of logs for all hello LiveEvent items; select hello one matching your current session.</span></span> 
  * <span data-ttu-id="de71d-113">**Flash Live mediekodare**: du kan hitta hello **loggkatalogen...**  genom att gå toohello **kodning loggen** fliken.</span><span class="sxs-lookup"><span data-stu-id="de71d-113">**Flash Media Live Encoder**: You can find hello **Log Directory...** by navigating toohello **Encoding Log** tab.</span></span>

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a><span data-ttu-id="de71d-114">Problem: Det finns inget alternativ för att mata ut en progressiv dataström</span><span class="sxs-lookup"><span data-stu-id="de71d-114">Problem: There is no option for outputting a progressive stream</span></span>
* <span data-ttu-id="de71d-115">**Potentiella problem**: hello kodare används inte automatiskt Ej sammanflätning.</span><span class="sxs-lookup"><span data-stu-id="de71d-115">**Potential issue**: hello encoder being used doesn't automatically deinterlace.</span></span> 
  
    <span data-ttu-id="de71d-116">**Felsökningssteg**: Leta efter en de-interlacing alternativ i hello kodare gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="de71d-116">**Troubleshooting steps**: Look for a de-interlacing option within hello encoder interface.</span></span> <span data-ttu-id="de71d-117">Kontrollera igen för inställningar för progressiv utdata när Frigör sammanflätning har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="de71d-117">Once de-interlacing is enabled, check again for progressive output settings.</span></span> 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-tooconnect"></a><span data-ttu-id="de71d-118">Problem: Försökte flera kodare utdata inställningar och fortfarande inte tooconnect.</span><span class="sxs-lookup"><span data-stu-id="de71d-118">Problem: Tried several encoder output settings and still unable tooconnect.</span></span>
* <span data-ttu-id="de71d-119">**Potentiella problem**: Azure kodning kanal har inte återställts korrekt.</span><span class="sxs-lookup"><span data-stu-id="de71d-119">**Potential issue**: Azure encoding channel was not properly reset.</span></span> 
  
    <span data-ttu-id="de71d-120">**Felsökningssteg**: se till att hello kodare inte längre trycka tooAMS, stoppa och återställa hello-kanalen.</span><span class="sxs-lookup"><span data-stu-id="de71d-120">**Troubleshooting steps**: Make sure hello encoder is no longer pushing tooAMS, stop and reset hello channel.</span></span> <span data-ttu-id="de71d-121">Kör en gång igen och försök att ansluta din kodare med hello nya inställningar.</span><span class="sxs-lookup"><span data-stu-id="de71d-121">Once running again, try connecting your encoder with hello new settings.</span></span> <span data-ttu-id="de71d-122">Om du fortfarande inte kan lösa problemet hello, försök att skapa en ny kanal helt, ibland kanaler kan bli skadad efter flera misslyckade försök.</span><span class="sxs-lookup"><span data-stu-id="de71d-122">If this still does not correct hello issue, try creating a new channel entirely, sometimes channels can become corrupt after several failed attempts.</span></span>  
* <span data-ttu-id="de71d-123">**Potentiella problem**: hello GOP storlek eller nyckelbilden inställningar inte är optimala.</span><span class="sxs-lookup"><span data-stu-id="de71d-123">**Potential issue**: hello GOP size or key frame settings are not optimal.</span></span> 
  
    <span data-ttu-id="de71d-124">**Felsökningssteg**: rekommenderas GOP storlek eller keyframe är 2 sekunder.</span><span class="sxs-lookup"><span data-stu-id="de71d-124">**Troubleshooting steps**: Recommended GOP size or keyframe interval is 2 seconds.</span></span> <span data-ttu-id="de71d-125">Vissa kodare beräkna den här inställningen i antal bildrutor, medan andra använder sekunder.</span><span class="sxs-lookup"><span data-stu-id="de71d-125">Some encoders calculate this setting in number of frames, while others use seconds.</span></span> <span data-ttu-id="de71d-126">Exempel: när mata ut 30fps hello GOP storlek är 60 ramar som är likvärdiga too2 sekunder.</span><span class="sxs-lookup"><span data-stu-id="de71d-126">For example: When outputting 30fps, hello GOP size would be 60 frames, which is equivalent too2 seconds.</span></span>  
* <span data-ttu-id="de71d-127">**Potentiella problem**: stängda portar blockerar hello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="de71d-127">**Potential issue**: Closed ports are blocking hello stream.</span></span> 
  
    <span data-ttu-id="de71d-128">**Felsökningssteg**: när direktuppspelning via RTMP, kontrollera brandväggen och/eller proxy inställningar tooconfirm att utgående portar 1935 och 1936 är öppna.</span><span class="sxs-lookup"><span data-stu-id="de71d-128">**Troubleshooting steps**: When streaming via RTMP, check firewall and/or proxy settings tooconfirm that outbound ports 1935 and 1936 are open.</span></span> <span data-ttu-id="de71d-129">När du använder RTP strömning, bekräfta att utgående port 2010 är öppen.</span><span class="sxs-lookup"><span data-stu-id="de71d-129">When using RTP streaming, confirm that outbound port 2010 is open.</span></span> 

### <a name="problem-when-configuring-hello-encoder-toostream-with-hello-rtp-protocol-there-is-no-place-tooenter-a-host-name"></a><span data-ttu-id="de71d-130">Problem: När du konfigurerar hello kodare toostream med hello RTP-protokollet är tooenter ett värdnamn.</span><span class="sxs-lookup"><span data-stu-id="de71d-130">Problem: When configuring hello encoder toostream with hello RTP protocol, there is no place tooenter a host name.</span></span>
* <span data-ttu-id="de71d-131">**Potentiella problem**: Tillåt inte många RTP kodare för värdnamn och en IP-adress måste toobe förvärvade.</span><span class="sxs-lookup"><span data-stu-id="de71d-131">**Potential issue**: Many RTP encoders do not allow for host names, and an IP address will need toobe acquired.</span></span>  
  
    <span data-ttu-id="de71d-132">**Felsökningssteg**: toofind Hej IP-adress, öppna en kommandotolk på varje dator.</span><span class="sxs-lookup"><span data-stu-id="de71d-132">**Troubleshooting steps**: toofind hello IP address, open a command prompt on any computer.</span></span> <span data-ttu-id="de71d-133">toodo detta i Windows, öppna hello kör programstart (WIN + R) och Skriv ”cmd” tooopen.</span><span class="sxs-lookup"><span data-stu-id="de71d-133">toodo this in Windows, open hello Run launcher (WIN + R) and type “cmd” tooopen.</span></span>  
  
    <span data-ttu-id="de71d-134">När hello kommandotolk är öppen skriver du ”Ping [AMS värdnamn]”.</span><span class="sxs-lookup"><span data-stu-id="de71d-134">Once hello command prompt is open, type "Ping [AMS Host Name]".</span></span> 
  
    <span data-ttu-id="de71d-135">hello värdnamn kan härledas genom att utelämna hello portnumret från hello Azure infognings-URL, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="de71d-135">hello host name can be derived by omitting hello port number from hello Azure Ingest URL, as highlighted in hello following example:</span></span> 
  
    <span data-ttu-id="de71d-136">RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 /</span><span class="sxs-lookup"><span data-stu-id="de71d-136">rtp://test2-amstest009.rtp.channel.mediaservices.windows.net:2010/</span></span> 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> <span data-ttu-id="de71d-138">Skicka ett supportärende med hello Azure-portalen om du efter följande hello du fortfarande inte kan kunna strömma för felsökning.</span><span class="sxs-lookup"><span data-stu-id="de71d-138">If after following hello troubleshooting steps you still cannot successfully stream, submit a support ticket using hello Azure portal.</span></span>
> 
> 

## <a name="media-services-learning-paths"></a><span data-ttu-id="de71d-139">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="de71d-139">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="de71d-140">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="de71d-140">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

