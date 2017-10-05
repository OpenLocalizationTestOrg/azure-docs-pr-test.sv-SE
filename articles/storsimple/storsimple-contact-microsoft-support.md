---
title: "Logga supportärende för StorSimple 8000-serien | Microsoft Docs"
description: "Lär dig mer om att skapa en supportbegäran och starta en session stöd på din StorSimple-enhet."
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
ms.openlocfilehash: cecc2566b432e897b5eff0c12e66598f0518ed80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="contact-microsoft-support-for-your-storsimple"></a><span data-ttu-id="6dbf7-103">Kontakta Microsofts Support för din StorSimple</span><span class="sxs-lookup"><span data-stu-id="6dbf7-103">Contact Microsoft Support for your StorSimple</span></span>
<span data-ttu-id="6dbf7-104">Om det uppstår problem med Microsoft Azure StorSimple-lösning kan du skapa en tjänstbegäran för teknisk support.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-104">If you encounter any issues with your Microsoft Azure StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="6dbf7-105">I en online-session med supportpersonalen, kan du också behöva starta en session med stöd på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-105">In an online session with your support engineer, you may also need to start a support session on your StorSimple device.</span></span> <span data-ttu-id="6dbf7-106">Den här artikeln vägleder dig genom:</span><span class="sxs-lookup"><span data-stu-id="6dbf7-106">This article walks you through:</span></span>

* <span data-ttu-id="6dbf7-107">Så här skapar du en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-107">How to create a support request.</span></span>
* <span data-ttu-id="6dbf7-108">Hur du startar en supportsession i Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-108">How to start a support session in the Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="6dbf7-109">Granska de [StorSimple 8000-serien stöd SLA och information](https://msdn.microsoft.com/library/mt433077.aspx) innan du skapar en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-109">Review the [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="6dbf7-110">Skapa en supportförfrågan</span><span class="sxs-lookup"><span data-stu-id="6dbf7-110">Create a support request</span></span>
<span data-ttu-id="6dbf7-111">Utför följande steg för att skapa en supportförfrågan:</span><span class="sxs-lookup"><span data-stu-id="6dbf7-111">Perform the following steps to create a support request:</span></span>

#### <a name="to-create-a-support-request"></a><span data-ttu-id="6dbf7-112">Så här skapar du en supportbegäran</span><span class="sxs-lookup"><span data-stu-id="6dbf7-112">To create a support request</span></span>
1. <span data-ttu-id="6dbf7-113">I den [klassiska Azure-portalen](https://manage.windowsazure.com/)i det övre högra hörnet, klickar du på namnet på ditt konto och klicka sedan på **kontakta Microsoft Support**.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-113">In the [Azure classic portal](https://manage.windowsazure.com/), in the upper right corner, click your account name and then click **Contact Microsoft Support**.</span></span>
   
    ![Kontakta MS supporten via ManagementPortal](./media/storsimple-contact-microsoft-support/Ibiza1.png)
2. <span data-ttu-id="6dbf7-115">Du omdirigeras till den nya Azure-portalen (portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6dbf7-115">You will be redirected to the new Azure portal (portal.azure.com).</span></span> <span data-ttu-id="6dbf7-116">Klicka på den **ny supportbegäran** panelen.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-116">Click the **New support request** tile.</span></span>
   
    ![Kontakta supporten för MS via nya portalen](./media/storsimple-contact-microsoft-support/Ibiza2.png)
   
    <span data-ttu-id="6dbf7-118">På höger sida av skärmen, den **ny supportbegäran** visas.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-118">On the right side of the screen, the **New support request** pane appears.</span></span> 
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza3a.png)
3. <span data-ttu-id="6dbf7-120">I den **grunderna** dialogrutan Fyll följande:</span><span class="sxs-lookup"><span data-stu-id="6dbf7-120">In the **Basics** dialog box, complete the following:</span></span>                                
   
   1. <span data-ttu-id="6dbf7-121">Från den **utfärda typ** listrutan, Välj **tekniska**.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-121">From the **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="6dbf7-122">Välj en **prenumeration** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-122">Select a **Subscription** from the drop-down list.</span></span>
   3. <span data-ttu-id="6dbf7-123">Från den **Service** listrutan, Välj **StorSimple**.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-123">From the **Service** drop-down list, select **StorSimple**.</span></span> 
   4. <span data-ttu-id="6dbf7-124">Välj en **supportavtal** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-124">Select a **Support plan** from the drop-down list.</span></span> <span data-ttu-id="6dbf7-125">Du behöver ett betalt supportavtal för att aktivera teknisk Support.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-125">You need a paid support plan to enable Technical Support.</span></span>
4. <span data-ttu-id="6dbf7-126">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-126">Click **Next**.</span></span> <span data-ttu-id="6dbf7-127">Den **problemet** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-127">The **Problem** dialog box appears.</span></span>
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza5a.png) 
5. <span data-ttu-id="6dbf7-129">I den **problemet** dialogrutan Fyll följande:</span><span class="sxs-lookup"><span data-stu-id="6dbf7-129">In the **Problem** dialog box, complete the following:</span></span>
   
   1. <span data-ttu-id="6dbf7-130">Välj en **allvarlighetsgrad** nivån från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-130">Select a **Severity** level from the drop-down list.</span></span>
   2. <span data-ttu-id="6dbf7-131">Välj en **problemtyp** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-131">Select a **Problem type** from the drop-down list.</span></span>
   3. <span data-ttu-id="6dbf7-132">Välj en **kategori** från den nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-132">Select a **Category** from the drop-down list.</span></span> 
   4. <span data-ttu-id="6dbf7-133">I den **information** rutan, en kort beskrivning av problemet.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-133">In the **Details** box, briefly describe your issue.</span></span>
   5. <span data-ttu-id="6dbf7-134">I den **tidsram** rutan, ange datum, tid och tidszon som motsvarar den senaste förekomsten av problemet.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-134">In the **Time frame** box, indicate the date, time, and time zone that corresponds to the most recent occurrence of your issue.</span></span>
   6. <span data-ttu-id="6dbf7-135">Under **filuppladdning**, klicka på mappikonen och bläddra till support-paketet.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-135">Under **File upload**, click the folder icon to browse to your support package.</span></span>
   7. <span data-ttu-id="6dbf7-136">Välj den **dela diagnostikinformation** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-136">Select the **Share diagnostic information** check box.</span></span>
6. <span data-ttu-id="6dbf7-137">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-137">Click **Next**.</span></span> <span data-ttu-id="6dbf7-138">Den **kontaktinformation** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-138">The **Contact information** dialog box appears.</span></span>
   
    ![Nytt stöd för begäran i](./media/storsimple-contact-microsoft-support/Ibiza6a.png) 
7. <span data-ttu-id="6dbf7-140">Ange din kontaktinformation och välj en kontaktmetod (telefon eller e-post).</span><span class="sxs-lookup"><span data-stu-id="6dbf7-140">Enter your contact information and select a contact method (phone or email).</span></span> 
8. <span data-ttu-id="6dbf7-141">Välj den **spara kontaktändringar för framtida supportförfrågningar** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-141">Select the **Save contact changes for future support requests** check box.</span></span>
9. <span data-ttu-id="6dbf7-142">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-142">Click **Create**.</span></span>

<span data-ttu-id="6dbf7-143">När du har skickat en begäran om en supporttekniker kommer att kontakta dig så snart som möjligt för att fortsätta med din begäran.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-143">After you have submitted your request, a Support engineer will contact you as soon as possible to proceed with your request.</span></span>

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="6dbf7-144">Starta en session med stöd i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="6dbf7-144">Start a support session in Windows PowerShell for StorSimple</span></span>
<span data-ttu-id="6dbf7-145">Om du vill felsöka eventuella problem som kan uppstå med StorSimple-enhet behöver kommunicera med Microsoft Support-teamet.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-145">To troubleshoot any issues that you might experience with the StorSimple device, you will need to engage with the Microsoft Support team.</span></span> <span data-ttu-id="6dbf7-146">Microsoft Support kan behöva använda en session med stöd för att logga in på din enhet.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-146">Microsoft Support may need to use a support session to log on to your device.</span></span> 

<span data-ttu-id="6dbf7-147">Utför följande steg för att starta en supportsession:</span><span class="sxs-lookup"><span data-stu-id="6dbf7-147">Perform the following steps to start a support session:</span></span>

#### <a name="to-start-a-support-session"></a><span data-ttu-id="6dbf7-148">Starta en supportsession</span><span class="sxs-lookup"><span data-stu-id="6dbf7-148">To start a support session</span></span>
1. <span data-ttu-id="6dbf7-149">Komma åt enheten direkt via seriekonsolen eller via en telnet-session från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-149">Access the device directly by using the serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="6dbf7-150">Om du vill göra detta, följer du anvisningarna i [Använd PuTTY för att ansluta till enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="6dbf7-150">To do this, follow the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="6dbf7-151">I sessionen som öppnas, trycker du på den **RETUR** för att få fram en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-151">In the session that opens, press the **Enter** key to get a command prompt.</span></span>
3. <span data-ttu-id="6dbf7-152">Välj alternativ 1, i menyn för seriekonsolen **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-152">In the serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="6dbf7-153">I Kommandotolken skriver du följande lösenord:</span><span class="sxs-lookup"><span data-stu-id="6dbf7-153">At the prompt, type the following password:</span></span> 
   
    `Password1`
5. <span data-ttu-id="6dbf7-154">Skriv följande kommando i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="6dbf7-154">At the prompt, type the following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="6dbf7-155">En krypterad sträng visas för dig.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-155">An encrypted string will be presented to you.</span></span> <span data-ttu-id="6dbf7-156">Kopiera den här strängen i en textredigerare, till exempel Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-156">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="6dbf7-157">Spara den här strängen och skicka den i ett e-postmeddelande till Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-157">Save this string and send it in an email message to Microsoft Support.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6dbf7-158">Du kan inaktivera stöd åtkomst genom att köra `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-158">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="6dbf7-159">StorSimple-enheten försöker inaktivera stöd för åtkomst till 8 timmar när sessionen har startats.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-159">The StorSimple device will also attempt to disable support access 8 hours after the session was initiated.</span></span> <span data-ttu-id="6dbf7-160">Det är en bra idé att ändra autentiseringsuppgifterna StorSimple-enhet när du initierar en supportsession.</span><span class="sxs-lookup"><span data-stu-id="6dbf7-160">It is a best practice to change your StorSimple device credentials after initiating a support session.</span></span>
> 
> 

