---
title: "aaaLog supportärende för StorSimple 8000-serien | Microsoft Docs"
description: "Lär dig hur toocreate stöd för en begäran och starta en session stöd på din StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 2ebc20fe-f490-4749-8e43-c9fac86f1676
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/27/2017
ms.author: alkohli;anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e1a3aa3c56e036c782c4fb502c477dc0feaa0ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a><span data-ttu-id="ee8d4-103">Kontakta Microsofts Support för din StorSimple</span><span class="sxs-lookup"><span data-stu-id="ee8d4-103">Contact Microsoft Support for your StorSimple</span></span>
<span data-ttu-id="ee8d4-104">Om det uppstår problem med Microsoft Azure StorSimple-lösning kan du skapa en tjänstbegäran för teknisk support.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-104">If you encounter any issues with your Microsoft Azure StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="ee8d4-105">Du kanske också måste toostart en supportsession på din StorSimple-enhet i en online session med supportpersonalen.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-105">In an online session with your support engineer, you may also need toostart a support session on your StorSimple device.</span></span> <span data-ttu-id="ee8d4-106">Den här artikeln vägleder dig genom:</span><span class="sxs-lookup"><span data-stu-id="ee8d4-106">This article walks you through:</span></span>

* <span data-ttu-id="ee8d4-107">Hur toocreate stöd för en begäran.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-107">How toocreate a support request.</span></span>
* <span data-ttu-id="ee8d4-108">Hur hello toostart en session stöd i Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-108">How toostart a support session in hello Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="ee8d4-109">Granska hello [StorSimple 8000-serien stöd SLA och information](https://msdn.microsoft.com/library/mt433077.aspx) innan du skapar en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-109">Review hello [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="ee8d4-110">Skapa en supportförfrågan</span><span class="sxs-lookup"><span data-stu-id="ee8d4-110">Create a support request</span></span>
<span data-ttu-id="ee8d4-111">Utför följande steg toocreate en supportbegäran hello:</span><span class="sxs-lookup"><span data-stu-id="ee8d4-111">Perform hello following steps toocreate a support request:</span></span>

#### <a name="toocreate-a-support-request"></a><span data-ttu-id="ee8d4-112">toocreate en supportbegäran</span><span class="sxs-lookup"><span data-stu-id="ee8d4-112">toocreate a support request</span></span>
1. <span data-ttu-id="ee8d4-113">I hello [klassiska Azure-portalen](https://manage.windowsazure.com/)i hello övre högra hörnet, klickar du på namnet på ditt konto och klicka sedan på **kontakta Microsoft Support**.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-113">In hello [Azure classic portal](https://manage.windowsazure.com/), in hello upper right corner, click your account name and then click **Contact Microsoft Support**.</span></span>
   
    ![Kontakta MS supporten via ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. <span data-ttu-id="ee8d4-115">Du kommer att omdirigerade toohello nya Azure-portalen (portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ee8d4-115">You will be redirected toohello new Azure portal (portal.azure.com).</span></span> <span data-ttu-id="ee8d4-116">Klicka på hello **ny supportbegäran** panelen.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-116">Click hello **New support request** tile.</span></span>
   
    ![Kontakta supporten för MS via nya portalen](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    <span data-ttu-id="ee8d4-118">Hello höger på hello-skärmen, hello **ny supportbegäran** visas.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-118">On hello right side of hello screen, hello **New support request** pane appears.</span></span> 
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. <span data-ttu-id="ee8d4-120">I hello **grunderna** dialogrutan, fullständig hello följande:</span><span class="sxs-lookup"><span data-stu-id="ee8d4-120">In hello **Basics** dialog box, complete hello following:</span></span>                                
   
   1. <span data-ttu-id="ee8d4-121">Från hello **utfärda typ** listrutan, Välj **tekniska**.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-121">From hello **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="ee8d4-122">Välj en **prenumeration** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-122">Select a **Subscription** from hello drop-down list.</span></span>
   3. <span data-ttu-id="ee8d4-123">Från hello **Service** listrutan, Välj **StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-123">From hello **Service** drop-down list, select **StorSimple**.</span></span> 
   4. <span data-ttu-id="ee8d4-124">Välj en **supportavtal** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-124">Select a **Support plan** from hello drop-down list.</span></span> <span data-ttu-id="ee8d4-125">Du behöver en betald stöd plan tooenable teknisk Support.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-125">You need a paid support plan tooenable Technical Support.</span></span>
4. <span data-ttu-id="ee8d4-126">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-126">Click **Next**.</span></span> <span data-ttu-id="ee8d4-127">Hej **problemet** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-127">hello **Problem** dialog box appears.</span></span>
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. <span data-ttu-id="ee8d4-129">I hello **problemet** dialogrutan, fullständig hello följande:</span><span class="sxs-lookup"><span data-stu-id="ee8d4-129">In hello **Problem** dialog box, complete hello following:</span></span>
   
   1. <span data-ttu-id="ee8d4-130">Välj en **allvarlighetsgrad** nivå hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-130">Select a **Severity** level from hello drop-down list.</span></span>
   2. <span data-ttu-id="ee8d4-131">Välj en **problemtyp** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-131">Select a **Problem type** from hello drop-down list.</span></span>
   3. <span data-ttu-id="ee8d4-132">Välj en **kategori** hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-132">Select a **Category** from hello drop-down list.</span></span> 
   4. <span data-ttu-id="ee8d4-133">I hello **information** rutan, en kort beskrivning av problemet.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-133">In hello **Details** box, briefly describe your issue.</span></span>
   5. <span data-ttu-id="ee8d4-134">I hello **tidsram** rutan, ange hello datum, tid och tidszon som motsvarar toohello senaste förekomst av problemet.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-134">In hello **Time frame** box, indicate hello date, time, and time zone that corresponds toohello most recent occurrence of your issue.</span></span>
   6. <span data-ttu-id="ee8d4-135">Under **filuppladdning**, klickar du på hello mappen ikonen toobrowse tooyour stöd för paketet.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-135">Under **File upload**, click hello folder icon toobrowse tooyour support package.</span></span>
   7. <span data-ttu-id="ee8d4-136">Välj hello **dela diagnostikinformation** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-136">Select hello **Share diagnostic information** check box.</span></span>
6. <span data-ttu-id="ee8d4-137">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-137">Click **Next**.</span></span> <span data-ttu-id="ee8d4-138">Hej **kontaktinformation** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-138">hello **Contact information** dialog box appears.</span></span>
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. <span data-ttu-id="ee8d4-140">Ange din kontaktinformation och välj en kontaktmetod (telefon eller e-post).</span><span class="sxs-lookup"><span data-stu-id="ee8d4-140">Enter your contact information and select a contact method (phone or email).</span></span> 
8. <span data-ttu-id="ee8d4-141">Välj hello **spara kontaktändringar för framtida supportförfrågningar** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-141">Select hello **Save contact changes for future support requests** check box.</span></span>
9. <span data-ttu-id="ee8d4-142">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-142">Click **Create**.</span></span>

<span data-ttu-id="ee8d4-143">När du har skickat en begäran om en supporttekniker kommer att kontakta dig så snart som möjligt tooproceed med din begäran.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-143">After you have submitted your request, a Support engineer will contact you as soon as possible tooproceed with your request.</span></span>

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="ee8d4-144">Starta en session med stöd i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="ee8d4-144">Start a support session in Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="ee8d4-145">tootroubleshoot eventuella problem som du kan uppleva med hello StorSimple-enhet, måste tooengage med hello Microsoft Support-teamet.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-145">tootroubleshoot any issues that you might experience with hello StorSimple device, you will need tooengage with hello Microsoft Support team.</span></span> <span data-ttu-id="ee8d4-146">Microsoft Support kan behöva toouse en stöd session toolog på tooyour enhet.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-146">Microsoft Support may need toouse a support session toolog on tooyour device.</span></span> 

<span data-ttu-id="ee8d4-147">Utföra hello följande steg toostart en supportsession:</span><span class="sxs-lookup"><span data-stu-id="ee8d4-147">Perform hello following steps toostart a support session:</span></span>

#### <a name="toostart-a-support-session"></a><span data-ttu-id="ee8d4-148">toostart en supportsession</span><span class="sxs-lookup"><span data-stu-id="ee8d4-148">toostart a support session</span></span>
1. <span data-ttu-id="ee8d4-149">Hello enhet direkt via seriekonsolen hello eller via en telnet-session från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-149">Access hello device directly by using hello serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="ee8d4-150">toodo, följ hello här i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="ee8d4-150">toodo this, follow hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="ee8d4-151">Hello-sessionen som öppnas, trycker du på hello **RETUR** viktiga tooget en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-151">In hello session that opens, press hello **Enter** key tooget a command prompt.</span></span>
3. <span data-ttu-id="ee8d4-152">Välj alternativ 1, i menyn för seriekonsolen av hello **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-152">In hello serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="ee8d4-153">I hello kommandotolk, skriver du hello följande lösenord:</span><span class="sxs-lookup"><span data-stu-id="ee8d4-153">At hello prompt, type hello following password:</span></span> 
   
    `Password1`
5. <span data-ttu-id="ee8d4-154">Skriv hello följande kommando i Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="ee8d4-154">At hello prompt, type hello following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="ee8d4-155">En krypterad sträng visas tooyou.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-155">An encrypted string will be presented tooyou.</span></span> <span data-ttu-id="ee8d4-156">Kopiera den här strängen i en textredigerare, till exempel Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-156">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="ee8d4-157">Spara den här strängen och skicka den i ett e-postmeddelande tooMicrosoft Support.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-157">Save this string and send it in an email message tooMicrosoft Support.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="ee8d4-158">Du kan inaktivera stöd åtkomst genom att köra `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-158">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="ee8d4-159">Hej StorSimple-enheten försöker också toodisable stöd åtkomst 8 timmar efter hello-sessionen har startats.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-159">hello StorSimple device will also attempt toodisable support access 8 hours after hello session was initiated.</span></span> <span data-ttu-id="ee8d4-160">Det är en bästa praxis toochange StorSimple-enheten autentiseringsuppgifter när du initierar en supportsession.</span><span class="sxs-lookup"><span data-stu-id="ee8d4-160">It is a best practice toochange your StorSimple device credentials after initiating a support session.</span></span>
> 
> 

