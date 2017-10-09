---
title: "aaaAzure RemoteApp nätverkets bandbredd – allmänna riktlinjer | Microsoft Docs"
description: "Förstå några grundläggande bandbredd riktlinjer för din Azure RemoteApp-samlingar och appar."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 529bf318-ae37-4c14-a11c-43fa703d68a3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d3407eea71e2e8ac524787ee680cfd870c800864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a><span data-ttu-id="ab10a-103">Azure RemoteApp-nätverkets bandbredd – allmänna riktlinjer (om du kan testa din egen)</span><span class="sxs-lookup"><span data-stu-id="ab10a-103">Azure RemoteApp network bandwidth - general guidelines (if you can't test your own)</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ab10a-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="ab10a-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="ab10a-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="ab10a-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="ab10a-106">Om du inte har hello tid eller kapaciteten toorun hello [nätverks bandbredd testerna](remoteapp-bandwidthtests.md) för Azure RemoteApp här följer några ganska allmänna riktlinjer som hjälper dig att beräkna bandbredd per användare.</span><span class="sxs-lookup"><span data-stu-id="ab10a-106">If you do not have hello time or capability toorun hello [network bandwidth tests](remoteapp-bandwidthtests.md) for Azure RemoteApp, here are some fairly generic guidelines that can help you estimate network bandwidth per user.</span></span>

<span data-ttu-id="ab10a-107">Om du har en blandning av dessa scenarier kan vi rekommenderar inte något mindre än (eller lika med) 10 MB/s som hello minsta nätverkets bandbredd för moderna Internetanslutna appar i en fjärrmiljö.</span><span class="sxs-lookup"><span data-stu-id="ab10a-107">If you have a mix of these scenarios, we don't recommend anything less than (or equal to) 10 MB/s as hello MINIMUM network bandwidth for modern Internet-connected apps in a remote environment.</span></span> <span data-ttu-id="ab10a-108">(Även om som diskuterats, inte kommer det garanterar bättre än genomsnittlig användarupplevelsen.)</span><span class="sxs-lookup"><span data-stu-id="ab10a-108">(Although, as discussed, this will not guarantee a better than average user experience.)</span></span>

## <a name="complex-powerpoint-simple-powerpoint"></a><span data-ttu-id="ab10a-109">Komplexa PowerPoint enkla PowerPoint</span><span class="sxs-lookup"><span data-stu-id="ab10a-109">Complex PowerPoint, simple PowerPoint</span></span>
<span data-ttu-id="ab10a-110">Azure RemoteApp är bäst på 100 MB LAN.</span><span class="sxs-lookup"><span data-stu-id="ab10a-110">Azure RemoteApp does best on 100 MB LAN.</span></span> <span data-ttu-id="ab10a-111">Vid hello 10 MB/s nätverksprofil när jitter ovan 120 ms är mer än 5%, hello användaren ser en genomsnittlig upplevelse.</span><span class="sxs-lookup"><span data-stu-id="ab10a-111">At hello 10 MB/s network profile, when jitter above 120 ms is more than 5%, hello user will see an average experience.</span></span> <span data-ttu-id="ab10a-112">Olika glaring - exempelvis i ett bildspel på 1 MB/s hello, hello användaren kanske inte se animerade övergångar alls eftersom ramar hoppas över.</span><span class="sxs-lookup"><span data-stu-id="ab10a-112">At 1 MB/s hello different is glaring - for example, in a slide show, hello user might not see animated transitions at all because frames are skipped.</span></span>

## <a name="internet-explorer-mixed-pdf-pdf-text"></a><span data-ttu-id="ab10a-113">Internet Explorer, blandat PDF-filer, PDF, Text</span><span class="sxs-lookup"><span data-stu-id="ab10a-113">Internet Explorer, mixed PDF, PDF, Text</span></span>
<span data-ttu-id="ab10a-114">10 MB/s nätverksprofil är nära tooLAN i de flesta aspekter.</span><span class="sxs-lookup"><span data-stu-id="ab10a-114">10 MB/s network profile is close tooLAN in most aspects.</span></span> <span data-ttu-id="ab10a-115">1 MB/s ger en OK upplevelse, även om det kan finnas vissa jitter när en användare rullar när det finns avbildningar på hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="ab10a-115">1 MB/s will provide an OK experience, although there may be some jitter when a user scrolls while there are images on hello screen.</span></span>

## <a name="flash-video-youtube"></a><span data-ttu-id="ab10a-116">Videon (YouTube)</span><span class="sxs-lookup"><span data-stu-id="ab10a-116">Flash video (YouTube)</span></span>
<span data-ttu-id="ab10a-117">100 MB/s LAN ger hello bästa möjliga upplevelse, medan 10 MB/s är acceptabel (betydelse vi Håll dig uppdaterad med hello bildfrekvens men jitter ökar).</span><span class="sxs-lookup"><span data-stu-id="ab10a-117">100 MB/s LAN provides hello best experience, while 10 MB/s is acceptable (meaning we keep up with hello frame rate but jitter increases).</span></span> <span data-ttu-id="ab10a-118">Vid 1 MB/s är mycket hög och märkbar jitter.</span><span class="sxs-lookup"><span data-stu-id="ab10a-118">At 1 MB/s, jitter is very high and noticeable.</span></span>

## <a name="word-typing-word-remote-input"></a><span data-ttu-id="ab10a-119">Word skriva (Word remote indata)</span><span class="sxs-lookup"><span data-stu-id="ab10a-119">Word typing (Word remote input)</span></span>
<span data-ttu-id="ab10a-120">Det här är ett scenario med låg bandbredd användning.</span><span class="sxs-lookup"><span data-stu-id="ab10a-120">This is a low-bandwidth usage scenario.</span></span> <span data-ttu-id="ab10a-121">Vi ger lika bra i en upplevelse som LAN på 256 KB/sek.</span><span class="sxs-lookup"><span data-stu-id="ab10a-121">At 256 KB/s we provide as good of an experience as LAN.</span></span>

## <a name="learn-more"></a><span data-ttu-id="ab10a-122">Läs mer</span><span class="sxs-lookup"><span data-stu-id="ab10a-122">Learn more</span></span>
* [<span data-ttu-id="ab10a-123">Beräkna Azure RemoteApp användningen av nätverksbandbredd</span><span class="sxs-lookup"><span data-stu-id="ab10a-123">Estimate Azure RemoteApp network bandwidth usage</span></span>](remoteapp-bandwidth.md)
* [<span data-ttu-id="ab10a-124">Azure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans?</span><span class="sxs-lookup"><span data-stu-id="ab10a-124">Azure RemoteApp - how do network bandwidth and quality of experience work together?</span></span>](remoteapp-bandwidthexperience.md)
* [<span data-ttu-id="ab10a-125">Azure RemoteApp - tseting din användandet av nätverkets bandbredd med några vanliga scenarier</span><span class="sxs-lookup"><span data-stu-id="ab10a-125">Azure RemoteApp - tseting your network bandwidth usage with some common scenarios</span></span>](remoteapp-bandwidthtests.md)

