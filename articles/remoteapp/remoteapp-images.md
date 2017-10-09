---
title: aaaWhat finns i hello Azure RemoteApp-mallavbildningarna? | Microsoft Docs
description: "Läs mer om hello mallavbildningarna som ingår i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7f8442b2-81da-421e-a453-aa53ba2066b7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: ea012cec8dc581a8bd4a5a138ce302de19d5c6af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-in-hello-azure-remoteapp-template-images"></a><span data-ttu-id="22a9d-104">Vad är hello Azure RemoteApp-mallavbildningarna?</span><span class="sxs-lookup"><span data-stu-id="22a9d-104">What is in hello Azure RemoteApp template images?</span></span>
> [!IMPORTANT]
> <span data-ttu-id="22a9d-105">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="22a9d-105">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="22a9d-106">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="22a9d-106">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="22a9d-107">Din Azure RemoteApp-prenumeration innehåller tre mallavbildningar:</span><span class="sxs-lookup"><span data-stu-id="22a9d-107">Your Azure RemoteApp subscription includes three template images:</span></span>

* <span data-ttu-id="22a9d-108">Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="22a9d-108">Windows Server 2012</span></span>
* <span data-ttu-id="22a9d-109">Microsoft Office 365 ProPlus (Office 365-prenumeration krävs)</span><span class="sxs-lookup"><span data-stu-id="22a9d-109">Microsoft Office 365 ProPlus (Office 365 subscription required)</span></span>
* <span data-ttu-id="22a9d-110">Microsoft Office 2013 Professional Plus (endast utvärderingsversion)</span><span class="sxs-lookup"><span data-stu-id="22a9d-110">Microsoft Office 2013 Professional Plus (trial only)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22a9d-111">Azure RemoteApp-prenumeration ger dig åtkomst toohello programvara i hello bilder med hello undantag av Office 365 ProPlus, som kräver en separat prenumeration och Office 2013, som inte kan användas i produktion.</span><span class="sxs-lookup"><span data-stu-id="22a9d-111">Your Azure RemoteApp subscription grants you access toohello software in hello images, with hello exception of Office 365 ProPlus, which requires a separate subscription, and Office 2013, which cannot be used in production.</span></span> <span data-ttu-id="22a9d-112">Det innebär att du kan dela hello programmen eller apparna i hello mallavbildningarna med dina användare.</span><span class="sxs-lookup"><span data-stu-id="22a9d-112">This means that you can share hello programs or apps on hello template images with your users.</span></span> <span data-ttu-id="22a9d-113">Om du skapar en samling som använder hello Windows Server 2012 R2-avbildningen kan publicera du System Center Endpoint Protection för användare tooaccess via RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="22a9d-113">For example, if you create a collection that uses hello Windows Server 2012 R2 image, you can publish System Center Endpoint Protection for users tooaccess through RemoteApp.</span></span>
> 
> <span data-ttu-id="22a9d-114">Kolla in hello [RemoteApp-licensieringsuppgifterna](remoteapp-licensing.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="22a9d-114">Check out hello [RemoteApp licensing details](remoteapp-licensing.md) for more information.</span></span> <span data-ttu-id="22a9d-115">Och [använda Office med Azure RemoteApp](remoteapp-o365.md) för hello Office-licensiering information.</span><span class="sxs-lookup"><span data-stu-id="22a9d-115">And [Using Office with Azure RemoteApp](remoteapp-o365.md) for hello Office licensing info.</span></span>
> 
> 

<span data-ttu-id="22a9d-116">I nedanstående avsnitt finns information om vad varje avbildning innehåller.</span><span class="sxs-lookup"><span data-stu-id="22a9d-116">Read on for details on what each image contains.</span></span>

## <a name="windows-server-2012-r2--hello-vanilla-image"></a><span data-ttu-id="22a9d-117">Windows Server 2012 R2 (”Hej vanliga avbildningen)</span><span class="sxs-lookup"><span data-stu-id="22a9d-117">Windows Server 2012 R2  ("hello vanilla image")</span></span>
<span data-ttu-id="22a9d-118">Den här avbildningen baseras på operativsystemet Microsoft Windows Server 2012 R2 Datacenter och hello följande roller och funktioner installerade toomeet hello krav för Azure RemoteApp-mallavbildningar:</span><span class="sxs-lookup"><span data-stu-id="22a9d-118">This image is based on Microsoft Windows Server 2012 R2 Datacenter operating system and has hello following roles and features installed toomeet hello requirements for Azure RemoteApp template images:</span></span>

* <span data-ttu-id="22a9d-119">.NET Framework 4.5, 3.5.1, 3.5</span><span class="sxs-lookup"><span data-stu-id="22a9d-119">.NET Framework 4.5, 3.5.1, 3.5</span></span>
* <span data-ttu-id="22a9d-120">Skrivbordsmiljö</span><span class="sxs-lookup"><span data-stu-id="22a9d-120">Desktop Experience</span></span>
* <span data-ttu-id="22a9d-121">Handskriftstjänster</span><span class="sxs-lookup"><span data-stu-id="22a9d-121">Ink and Handwriting Services</span></span>
* <span data-ttu-id="22a9d-122">Media Foundation</span><span class="sxs-lookup"><span data-stu-id="22a9d-122">Media Foundation</span></span>
* <span data-ttu-id="22a9d-123">Värd för fjärrskrivbordssession</span><span class="sxs-lookup"><span data-stu-id="22a9d-123">Remote Desktop Session Host</span></span>
* <span data-ttu-id="22a9d-124">Windows PowerShell 4.0</span><span class="sxs-lookup"><span data-stu-id="22a9d-124">Windows PowerShell 4.0</span></span>
* <span data-ttu-id="22a9d-125">Windows PowerShell ISE</span><span class="sxs-lookup"><span data-stu-id="22a9d-125">Windows PowerShell ISE</span></span>
* <span data-ttu-id="22a9d-126">WoW64-stöd</span><span class="sxs-lookup"><span data-stu-id="22a9d-126">WoW64 Support</span></span>

<span data-ttu-id="22a9d-127">Den här avbildningen innehåller även hello följande program:</span><span class="sxs-lookup"><span data-stu-id="22a9d-127">This image also has hello following applications installed:</span></span>

* <span data-ttu-id="22a9d-128">Adobe Flash Player</span><span class="sxs-lookup"><span data-stu-id="22a9d-128">Adobe Flash Player</span></span>
* <span data-ttu-id="22a9d-129">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="22a9d-129">Microsoft Silverlight</span></span>
* <span data-ttu-id="22a9d-130">Microsoft System Center 2012 Endpoint Protection</span><span class="sxs-lookup"><span data-stu-id="22a9d-130">Microsoft System Center 2012 Endpoint Protection</span></span>
* <span data-ttu-id="22a9d-131">Microsoft Windows Media Player</span><span class="sxs-lookup"><span data-stu-id="22a9d-131">Microsoft Windows Media Player</span></span>

## <a name="microsoft-office-365-proplus-subscription-required"></a><span data-ttu-id="22a9d-132">Microsoft Office 365 ProPlus (prenumeration krävs)</span><span class="sxs-lookup"><span data-stu-id="22a9d-132">Microsoft Office 365 ProPlus (subscription required)</span></span>
<span data-ttu-id="22a9d-133">Office 365 är hello mest efterfrågade programmet så vi har skapat en ”anpassad” avbildning du toowork med.</span><span class="sxs-lookup"><span data-stu-id="22a9d-133">Office 365 is hello most requested application, so we created a "custom" image for you toowork with.</span></span>

<span data-ttu-id="22a9d-134">Den här avbildningen är en utökning av hello vanliga avbildningen och har hello följande komponenter från Microsoft Office 365 ProPlus installerat dessutom toohello komponenter som beskrivs i hello Windows Server 2012 R2-avbildningen:</span><span class="sxs-lookup"><span data-stu-id="22a9d-134">This image is an extension of hello vanilla image and has hello following components of Microsoft Office 365 ProPlus installed in addition toohello components described in hello Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="22a9d-135">Åtkomst</span><span class="sxs-lookup"><span data-stu-id="22a9d-135">Access</span></span>
* <span data-ttu-id="22a9d-136">Excel</span><span class="sxs-lookup"><span data-stu-id="22a9d-136">Excel</span></span>
* <span data-ttu-id="22a9d-137">Lync</span><span class="sxs-lookup"><span data-stu-id="22a9d-137">Lync</span></span>
* <span data-ttu-id="22a9d-138">OneNote</span><span class="sxs-lookup"><span data-stu-id="22a9d-138">OneNote</span></span>
* <span data-ttu-id="22a9d-139">OneDrive för företag (Observera att hello sync-agenten inte stöds för användning med Azure RemoteApp)</span><span class="sxs-lookup"><span data-stu-id="22a9d-139">OneDrive for Business (note that hello sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="22a9d-140">Outlook</span><span class="sxs-lookup"><span data-stu-id="22a9d-140">Outlook</span></span>
* <span data-ttu-id="22a9d-141">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="22a9d-141">PowerPoint</span></span>
* <span data-ttu-id="22a9d-142">Word</span><span class="sxs-lookup"><span data-stu-id="22a9d-142">Word</span></span>
* <span data-ttu-id="22a9d-143">Språkverktyg för Microsoft Office</span><span class="sxs-lookup"><span data-stu-id="22a9d-143">Microsoft Office Proofing Tools</span></span>

<span data-ttu-id="22a9d-144">hello avbildningen ingår även Visio Pro och Project Pro.</span><span class="sxs-lookup"><span data-stu-id="22a9d-144">hello image also includes Visio Pro and Project Pro.</span></span>

<span data-ttu-id="22a9d-145">Och hello följande program:</span><span class="sxs-lookup"><span data-stu-id="22a9d-145">And hello following applications, as well:</span></span>

* <span data-ttu-id="22a9d-146">SQL Native Client</span><span class="sxs-lookup"><span data-stu-id="22a9d-146">SQL Native client</span></span>
* <span data-ttu-id="22a9d-147">ODBC-drivrutin</span><span class="sxs-lookup"><span data-stu-id="22a9d-147">ODBC Driver</span></span>
* <span data-ttu-id="22a9d-148">SQL Server Data Mining-klient</span><span class="sxs-lookup"><span data-stu-id="22a9d-148">SQL Server Data Mining client</span></span>
* <span data-ttu-id="22a9d-149">MasterDataServices-klient</span><span class="sxs-lookup"><span data-stu-id="22a9d-149">MasterDataServices client</span></span>
* <span data-ttu-id="22a9d-150">Microsoft Publisher</span><span class="sxs-lookup"><span data-stu-id="22a9d-150">Microsoft Publisher</span></span>
* <span data-ttu-id="22a9d-151">PowerQuery</span><span class="sxs-lookup"><span data-stu-id="22a9d-151">PowerQuery</span></span>
* <span data-ttu-id="22a9d-152">PowerMap</span><span class="sxs-lookup"><span data-stu-id="22a9d-152">PowerMap</span></span>

<span data-ttu-id="22a9d-153">Alla funktioner i Office 365 ProPlus-apparna är bara tillgängliga för användare som har en Office 365 ProPlus-plan.</span><span class="sxs-lookup"><span data-stu-id="22a9d-153">Full functionality of Office 365 ProPlus apps is available only for users who have an Office 365 ProPlus plan.</span></span> <span data-ttu-id="22a9d-154">Mer information om hello Office 365-prenumeration planer finns [Office 365-tjänstplaner](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span><span class="sxs-lookup"><span data-stu-id="22a9d-154">For more details on hello Office 365 subscription plans see [Office 365 service plans](http://technet.microsoft.com/library/office-365-plan-options.aspx).</span></span> <span data-ttu-id="22a9d-155">Har du fler frågor?</span><span class="sxs-lookup"><span data-stu-id="22a9d-155">Still have questions?</span></span> <span data-ttu-id="22a9d-156">Kolla in hello [Office 365 + RemoteApp](remoteapp-o365.md) information.</span><span class="sxs-lookup"><span data-stu-id="22a9d-156">Check out hello [Office 365 + RemoteApp](remoteapp-o365.md) information.</span></span> <span data-ttu-id="22a9d-157">Se även hello ny artikel, [hur toouse din Office 365-prenumeration med Azure RemoteApp](remoteapp-officesubscription.md).</span><span class="sxs-lookup"><span data-stu-id="22a9d-157">Also check out hello new article, [How toouse your Office 365 subscription with Azure RemoteApp](remoteapp-officesubscription.md).</span></span>

<span data-ttu-id="22a9d-158">Observera att du toolicense Office 365 ProPlus, Visio Pro och Project Pro separat – alla har en egen licens.</span><span class="sxs-lookup"><span data-stu-id="22a9d-158">Note that you need toolicense Office 365 ProPlus, Visio Pro, and Project Pro separately - they each have their own license.</span></span>

## <a name="microsoft-office-2013-professional-plus-trial-only"></a><span data-ttu-id="22a9d-159">Microsoft Office 2013 Professional Plus (endast utvärderingsversion)</span><span class="sxs-lookup"><span data-stu-id="22a9d-159">Microsoft Office 2013 Professional Plus (trial only)</span></span>
<span data-ttu-id="22a9d-160">Du kan testa hello service med hello Office 2013-avbildningen under hello utvärderingsperioden.</span><span class="sxs-lookup"><span data-stu-id="22a9d-160">During hello free trial period, you can test hello service with hello Office 2013 image.</span></span>

<span data-ttu-id="22a9d-161">Den här avbildningen är en utökning av hello vanliga avbildningen och har hello följande komponenter från Microsoft Office 2013 Professional Plus installerat dessutom toohello komponenter som beskrivs i hello Windows Server 2012 R2-avbildningen:</span><span class="sxs-lookup"><span data-stu-id="22a9d-161">This image is an extension of hello vanilla image and has hello following components of Microsoft Office 2013 Professional Plus installed in addition toohello components described in hello Windows Server 2012 R2 image:</span></span>

* <span data-ttu-id="22a9d-162">Åtkomst</span><span class="sxs-lookup"><span data-stu-id="22a9d-162">Access</span></span>
* <span data-ttu-id="22a9d-163">Excel</span><span class="sxs-lookup"><span data-stu-id="22a9d-163">Excel</span></span>
* <span data-ttu-id="22a9d-164">Lync</span><span class="sxs-lookup"><span data-stu-id="22a9d-164">Lync</span></span>
* <span data-ttu-id="22a9d-165">OneNote</span><span class="sxs-lookup"><span data-stu-id="22a9d-165">OneNote</span></span>
* <span data-ttu-id="22a9d-166">OneDrive för företag (Observera att hello sync-agenten inte stöds för användning med Azure RemoteApp)</span><span class="sxs-lookup"><span data-stu-id="22a9d-166">OneDrive for Business (note that hello sync agent is not supported for use with Azure RemoteApp)</span></span>
* <span data-ttu-id="22a9d-167">Outlook</span><span class="sxs-lookup"><span data-stu-id="22a9d-167">Outlook</span></span>
* <span data-ttu-id="22a9d-168">PowerPoint</span><span class="sxs-lookup"><span data-stu-id="22a9d-168">PowerPoint</span></span>
* <span data-ttu-id="22a9d-169">Project</span><span class="sxs-lookup"><span data-stu-id="22a9d-169">Project</span></span>
* <span data-ttu-id="22a9d-170">Visio</span><span class="sxs-lookup"><span data-stu-id="22a9d-170">Visio</span></span>
* <span data-ttu-id="22a9d-171">Word</span><span class="sxs-lookup"><span data-stu-id="22a9d-171">Word</span></span>
* <span data-ttu-id="22a9d-172">Språkverktyg för Microsoft Office</span><span class="sxs-lookup"><span data-stu-id="22a9d-172">Microsoft Office Proofing Tools</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22a9d-173">**Juridisk information:** Den här avbildningen innehåller inte en licens för Microsoft Office och *kan inte användas för produktion*.</span><span class="sxs-lookup"><span data-stu-id="22a9d-173">**Legal information:** This image does not include a Microsoft Office license and *cannot be used for production*.</span></span> <span data-ttu-id="22a9d-174">hello Office 2013 Professional Plus avbildningen är avsedd för Utvärderingsanvändning endast.</span><span class="sxs-lookup"><span data-stu-id="22a9d-174">hello Office 2013 Professional Plus image is intended for trial use only.</span></span> <span data-ttu-id="22a9d-175">Om du vill toouse Office-appar i Azure RemoteApp för produktion, måste toouse hello Office 365 ProPlus-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="22a9d-175">If you want toouse Office apps in Azure RemoteApp for production, you need toouse hello Office 365 ProPlus image.</span></span> <span data-ttu-id="22a9d-176">Mer info om Office-licensiering finns i artikeln om att [använda Office 365 med Azure RemoteApp](remoteapp-o365.md).</span><span class="sxs-lookup"><span data-stu-id="22a9d-176">For more details on licensing Office, see [Using Office 365 with Azure RemoteApp](remoteapp-o365.md)</span></span>
> 
> 

