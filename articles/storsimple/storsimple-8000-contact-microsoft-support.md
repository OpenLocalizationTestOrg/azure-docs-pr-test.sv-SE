---
title: "aaaCreate stöder biljett eller fall för StorSimple 8000-serien | Microsoft Docs"
description: "Lär dig hur toolog stöder begäran och starta en session med stöd på enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: alkohli;
ms.openlocfilehash: 832e3ac739dafa6dd8c24aaea38aef9c6f1b27b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="contact-microsoft-support"></a><span data-ttu-id="6182d-103">Kontakta Microsofts support</span><span class="sxs-lookup"><span data-stu-id="6182d-103">Contact Microsoft Support</span></span>

<span data-ttu-id="6182d-104">Hej StorSimple Enhetshanteraren ger hello funktion för**logga en ny supportförfrågan** inom hello service sammanfattning bladet.</span><span class="sxs-lookup"><span data-stu-id="6182d-104">hello StorSimple Device Manager provides hello capability too**log a new support request** within hello service summary blade.</span></span> <span data-ttu-id="6182d-105">Om det uppstår problem med din StorSimple-lösning kan du skapa en tjänstbegäran för teknisk support.</span><span class="sxs-lookup"><span data-stu-id="6182d-105">If you encounter any issues with your StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="6182d-106">Du kanske också måste toostart en supportsession på din StorSimple-enhet i en online session med supportpersonalen.</span><span class="sxs-lookup"><span data-stu-id="6182d-106">In an online session with your support engineer, you may also need toostart a support session on your StorSimple device.</span></span> <span data-ttu-id="6182d-107">Den här artikeln vägleder dig genom:</span><span class="sxs-lookup"><span data-stu-id="6182d-107">This article walks you through:</span></span>

* <span data-ttu-id="6182d-108">Hur toocreate stöd för en begäran.</span><span class="sxs-lookup"><span data-stu-id="6182d-108">How toocreate a support request.</span></span>
* <span data-ttu-id="6182d-109">Hur begäran toomanage en livscykel från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="6182d-109">How toomanage a support request lifecycle from within hello portal.</span></span>
* <span data-ttu-id="6182d-110">Hur hello toostart en session stöd i Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="6182d-110">How toostart a support session in hello Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="6182d-111">Granska hello [StorSimple 8000-serien stöd SLA och information](https://msdn.microsoft.com/library/mt433077.aspx) innan du skapar en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="6182d-111">Review hello [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="6182d-112">Skapa en supportförfrågan</span><span class="sxs-lookup"><span data-stu-id="6182d-112">Create a support request</span></span>

<span data-ttu-id="6182d-113">Beroende på din [supportavtal](https://azure.microsoft.com/support/plans/), du kan skapa supportärenden på ett problem på din StorSimple-enhet direkt från hello StorSimple Enhetshanteraren service sammanfattning bladet.</span><span class="sxs-lookup"><span data-stu-id="6182d-113">Depending upon your [support plan](https://azure.microsoft.com/support/plans/), you can create support tickets for an issue on your StorSimple device directly from hello StorSimple Device Manager service summary blade.</span></span> <span data-ttu-id="6182d-114">Utför följande steg toocreate en supportbegäran hello:</span><span class="sxs-lookup"><span data-stu-id="6182d-114">Perform hello following steps toocreate a support request:</span></span>

#### <a name="toocreate-a-support-request"></a><span data-ttu-id="6182d-115">toocreate en supportbegäran</span><span class="sxs-lookup"><span data-stu-id="6182d-115">toocreate a support request</span></span>

1. <span data-ttu-id="6182d-116">Gå tooyour StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6182d-116">Go tooyour StorSimple Device Manager service.</span></span> <span data-ttu-id="6182d-117">Hello sammanfattning bladet inställningar för tjänsten, gå för**stöd + felsökning** avsnittet och klicka sedan på **ny supportbegäran**.</span><span class="sxs-lookup"><span data-stu-id="6182d-117">In hello service summary blade settings, go too**SUPPORT + TROUBLESHOOTING** section and then click **New support request**.</span></span>
     
    ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. <span data-ttu-id="6182d-119">I hello **ny supportbegäran** bladet väljer **grunderna**.</span><span class="sxs-lookup"><span data-stu-id="6182d-119">In hello **New support request** blade, select **Basics**.</span></span> <span data-ttu-id="6182d-120">I hello **grunderna** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6182d-120">In hello **Basics** blade, do hello following steps:</span></span>
   1. <span data-ttu-id="6182d-121">Från hello **utfärda typ** listrutan, Välj **tekniska**.</span><span class="sxs-lookup"><span data-stu-id="6182d-121">From hello **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="6182d-122">hello aktuella **prenumeration**, **Service** typ och hello **resurs** (StorSimple Device Manager service) väljs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6182d-122">hello current **Subscription**, **Service** type, and hello **Resource** (StorSimple Device Manager service) are automatically chosen.</span></span> 
   3. <span data-ttu-id="6182d-123">Välj en **supportavtal** hello nedrullningsbara listan om du har flera planer som är associerad med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6182d-123">Select a **Support plan** from hello drop-down list if you have multiple plans associated with your subscription.</span></span> <span data-ttu-id="6182d-124">Du behöver en betald stöd plan tooenable teknisk Support.</span><span class="sxs-lookup"><span data-stu-id="6182d-124">You need a paid support plan tooenable Technical Support.</span></span>
   4. <span data-ttu-id="6182d-125">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="6182d-125">Click **Next**.</span></span>

       ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. <span data-ttu-id="6182d-127">I hello **ny supportbegäran** bladet väljer **steg 2 problemet**.</span><span class="sxs-lookup"><span data-stu-id="6182d-127">In hello **New support request** blade, select **Step 2 Problem**.</span></span> <span data-ttu-id="6182d-128">I hello **problemet** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6182d-128">In hello **Problem** blade, do hello following steps:</span></span>
    
    1. <span data-ttu-id="6182d-129">Välj hello **allvarlighetsgrad**.</span><span class="sxs-lookup"><span data-stu-id="6182d-129">Choose hello **Severity**.</span></span>
    2. <span data-ttu-id="6182d-130">Ange om hello problemet är relaterade toohello enhet eller hello StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6182d-130">Specify if hello issue is related toohello appliance or hello StorSimple Device Manager service.</span></span>
    3. <span data-ttu-id="6182d-131">Välj en **kategori** för den här utfärda och ge fler **information** om hello problemet.</span><span class="sxs-lookup"><span data-stu-id="6182d-131">Choose a **Category** for this issue and provide more **Details** about hello issue.</span></span>
    4. <span data-ttu-id="6182d-132">Ange hello startdatum och tidpunkt för hello problem.</span><span class="sxs-lookup"><span data-stu-id="6182d-132">Provide hello start date and time for hello problem.</span></span>
    5. <span data-ttu-id="6182d-133">I hello **filuppladdning**, klickar du på hello mappen ikonen toobrowse tooyour stöd för paketet.</span><span class="sxs-lookup"><span data-stu-id="6182d-133">In hello **File upload**, click hello folder icon toobrowse tooyour support package.</span></span>
    6. <span data-ttu-id="6182d-134">Kontrollera **dela diagnostikinformation**.</span><span class="sxs-lookup"><span data-stu-id="6182d-134">Check **Share diagnostic information**.</span></span>
    7. <span data-ttu-id="6182d-135">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="6182d-135">Click **Next**.</span></span>

       ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. <span data-ttu-id="6182d-137">I hello **ny supportbegäran** bladet, klickar du på **steg 3 kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="6182d-137">In hello **New support request** blade, click **Step 3 Contact information**.</span></span> <span data-ttu-id="6182d-138">I hello **kontaktinformation** bladet hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6182d-138">In hello **Contact information** blade, do hello following steps:</span></span>

    1. <span data-ttu-id="6182d-139">I hello **Kontaktalternativ**anger önskad kontaktmetod (telefon eller e-post) och hello språk.</span><span class="sxs-lookup"><span data-stu-id="6182d-139">In hello **Contact options**, provide your preferred contact method (phone or email) and hello language.</span></span> <span data-ttu-id="6182d-140">svarstid för hello väljs automatiskt baserat på din prenumeration planen.</span><span class="sxs-lookup"><span data-stu-id="6182d-140">hello response time is automatically selected based on your subscription plan.</span></span>
    2. <span data-ttu-id="6182d-141">Ange ditt namn, e-post, valfritt kontakt, land i hello kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="6182d-141">In hello Contact information, provide your name, email, optional contact, country.</span></span> <span data-ttu-id="6182d-142">Välj hello **spara kontaktändringar för framtida supportförfrågningar** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="6182d-142">Select hello **Save contact changes for future support requests** check box.</span></span>
    3. <span data-ttu-id="6182d-143">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6182d-143">Click **Create**.</span></span>
   
        ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    <span data-ttu-id="6182d-145">Microsoft Support använder denna information tooreach ut tooyou för ytterligare information, diagnostik och upplösning.</span><span class="sxs-lookup"><span data-stu-id="6182d-145">Microsoft Support will use this information tooreach out tooyou for further information, diagnosis, and resolution.</span></span>
<span data-ttu-id="6182d-146">När du har skickat en begäran om en supporttekniker kommer att kontakta dig så snart som möjligt tooproceed med din begäran.</span><span class="sxs-lookup"><span data-stu-id="6182d-146">After you have submitted your request, a Support engineer will contact you as soon as possible tooproceed with your request.</span></span>

## <a name="manage-a-support-request"></a><span data-ttu-id="6182d-147">Hantera en supportbegäran</span><span class="sxs-lookup"><span data-stu-id="6182d-147">Manage a support request</span></span>

<span data-ttu-id="6182d-148">När du har skapat ett supportärende kan du hantera hello livscykeln för hello-biljett från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="6182d-148">After creating a support ticket, you can manage hello lifecycle of hello ticket from within hello portal.</span></span>

#### <a name="toomanage-your-support-requests"></a><span data-ttu-id="6182d-149">toomanage din support begär</span><span class="sxs-lookup"><span data-stu-id="6182d-149">toomanage your support requests</span></span>

1. <span data-ttu-id="6182d-150">tooget toohello hjälp och supportsida, navigera för**Bläddra > hjälp + support**.</span><span class="sxs-lookup"><span data-stu-id="6182d-150">tooget toohello help and support page, navigate too**Browse > Help + support**.</span></span>

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. <span data-ttu-id="6182d-152">En tabell lista över alla hello support begär visas i hello **hjälp + support** bladet.</span><span class="sxs-lookup"><span data-stu-id="6182d-152">A tabular listing of All hello support requests is displayed in hello **Help + support** blade.</span></span>

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. <span data-ttu-id="6182d-154">Välj och klicka på en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="6182d-154">Select and click a support request.</span></span> <span data-ttu-id="6182d-155">Du kan visa hello status och hello information för denna begäran.</span><span class="sxs-lookup"><span data-stu-id="6182d-155">You can view hello status and hello details for this request.</span></span> <span data-ttu-id="6182d-156">Klicka på **+ nytt meddelande** om du vill att toofollow på denna begäran.</span><span class="sxs-lookup"><span data-stu-id="6182d-156">Click **+ New message** if you want toofollow up on this request.</span></span>

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="6182d-158">Starta en session med stöd i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="6182d-158">Start a support session in Windows PowerShell for StorSimple</span></span>

<span data-ttu-id="6182d-159">tootroubleshoot eventuella problem som du kan uppleva med hello StorSimple-enhet, måste tooengage med hello Microsoft Support-teamet.</span><span class="sxs-lookup"><span data-stu-id="6182d-159">tootroubleshoot any issues that you might experience with hello StorSimple device, you will need tooengage with hello Microsoft Support team.</span></span> <span data-ttu-id="6182d-160">Microsoft Support kan behöva toouse en stöd session toolog på tooyour enhet.</span><span class="sxs-lookup"><span data-stu-id="6182d-160">Microsoft Support may need toouse a support session toolog on tooyour device.</span></span>

<span data-ttu-id="6182d-161">Utföra hello följande steg toostart en supportsession:</span><span class="sxs-lookup"><span data-stu-id="6182d-161">Perform hello following steps toostart a support session:</span></span>

#### <a name="toostart-a-support-session"></a><span data-ttu-id="6182d-162">toostart en supportsession</span><span class="sxs-lookup"><span data-stu-id="6182d-162">toostart a support session</span></span>

1. <span data-ttu-id="6182d-163">Hello enhet direkt via seriekonsolen hello eller via en telnet-session från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="6182d-163">Access hello device directly by using hello serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="6182d-164">toodo, följ hello här i [använda PuTTY tooconnect toohello enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="6182d-164">toodo this, follow hello steps in [Use PuTTY tooconnect toohello device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="6182d-165">Hello-sessionen som öppnas, trycker du på hello **RETUR** viktiga tooget en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="6182d-165">In hello session that opens, press hello **Enter** key tooget a command prompt.</span></span>
3. <span data-ttu-id="6182d-166">Välj alternativ 1, i menyn för seriekonsolen av hello **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="6182d-166">In hello serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="6182d-167">I hello kommandotolk, skriver du hello följande lösenord:</span><span class="sxs-lookup"><span data-stu-id="6182d-167">At hello prompt, type hello following password:</span></span>
   
    `Password1`
5. <span data-ttu-id="6182d-168">Skriv hello följande kommando i Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="6182d-168">At hello prompt, type hello following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="6182d-169">En krypterad sträng visas tooyou.</span><span class="sxs-lookup"><span data-stu-id="6182d-169">An encrypted string will be presented tooyou.</span></span> <span data-ttu-id="6182d-170">Kopiera den här strängen i en textredigerare, till exempel Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="6182d-170">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="6182d-171">Spara den här strängen och skicka den i ett e-postmeddelande tooMicrosoft Support.</span><span class="sxs-lookup"><span data-stu-id="6182d-171">Save this string and send it in an email message tooMicrosoft Support.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6182d-172">Du kan inaktivera stöd åtkomst genom att köra `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="6182d-172">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="6182d-173">Hej StorSimple-enheten försöker också toodisable stöd åtkomst 8 timmar efter hello-sessionen har startats.</span><span class="sxs-lookup"><span data-stu-id="6182d-173">hello StorSimple device will also attempt toodisable support access 8 hours after hello session was initiated.</span></span> <span data-ttu-id="6182d-174">Det är en bästa praxis toochange StorSimple-enheten autentiseringsuppgifter när du initierar en supportsession.</span><span class="sxs-lookup"><span data-stu-id="6182d-174">It is a best practice toochange your StorSimple device credentials after initiating a support session.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6182d-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6182d-175">Next steps</span></span>

<span data-ttu-id="6182d-176">Lär dig hur för[diagnostisera och lösa problem relaterade tooyour StorSimple 8000-serieenhet](storsimple-troubleshoot-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="6182d-176">Learn how too[diagnose and solve problems related tooyour StorSimple 8000 series device](storsimple-troubleshoot-deployment.md)</span></span>
