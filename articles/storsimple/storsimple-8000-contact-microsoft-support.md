---
title: "Skapa supportärende eller fallet för StorSimple 8000-serien | Microsoft Docs"
description: "Lär dig hur du loggar begäran och starta en session med stöd på enheten StorSimple 8000-serien."
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
ms.openlocfilehash: 4b5a14237ce79100f980b2186b2c3c887abaa296
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="contact-microsoft-support"></a><span data-ttu-id="8dc94-103">Kontakta Microsofts support</span><span class="sxs-lookup"><span data-stu-id="8dc94-103">Contact Microsoft Support</span></span>

<span data-ttu-id="8dc94-104">Enhetshanteraren StorSimple ger möjlighet att **logga en ny supportförfrågan** inom tjänsten sammanfattning bladet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-104">The StorSimple Device Manager provides the capability to **log a new support request** within the service summary blade.</span></span> <span data-ttu-id="8dc94-105">Om det uppstår problem med din StorSimple-lösning kan du skapa en tjänstbegäran för teknisk support.</span><span class="sxs-lookup"><span data-stu-id="8dc94-105">If you encounter any issues with your StorSimple solution, you can create a service request for technical support.</span></span> <span data-ttu-id="8dc94-106">I en online-session med supportpersonalen, kan du också behöva starta en session med stöd på StorSimple-enheten.</span><span class="sxs-lookup"><span data-stu-id="8dc94-106">In an online session with your support engineer, you may also need to start a support session on your StorSimple device.</span></span> <span data-ttu-id="8dc94-107">Den här artikeln vägleder dig genom:</span><span class="sxs-lookup"><span data-stu-id="8dc94-107">This article walks you through:</span></span>

* <span data-ttu-id="8dc94-108">Så här skapar du en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="8dc94-108">How to create a support request.</span></span>
* <span data-ttu-id="8dc94-109">Hur du hanterar en begäran om livscykeln för support från i portalen.</span><span class="sxs-lookup"><span data-stu-id="8dc94-109">How to manage a support request lifecycle from within the portal.</span></span>
* <span data-ttu-id="8dc94-110">Hur du startar en supportsession i Windows PowerShell-gränssnittet för din StorSimple-enhet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-110">How to start a support session in the Windows PowerShell interface of your StorSimple device.</span></span>

<span data-ttu-id="8dc94-111">Granska de [StorSimple 8000-serien stöd SLA och information](https://msdn.microsoft.com/library/mt433077.aspx) innan du skapar en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="8dc94-111">Review the [StorSimple 8000 Series Support SLAs and information](https://msdn.microsoft.com/library/mt433077.aspx) before you create a Support request.</span></span>

## <a name="create-a-support-request"></a><span data-ttu-id="8dc94-112">Skapa en supportförfrågan</span><span class="sxs-lookup"><span data-stu-id="8dc94-112">Create a support request</span></span>

<span data-ttu-id="8dc94-113">Beroende på din [supportavtal](https://azure.microsoft.com/support/plans/), du kan skapa supportärenden på ett problem på din StorSimple-enhet direkt från bladet StorSimple Enhetshanteraren service sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="8dc94-113">Depending upon your [support plan](https://azure.microsoft.com/support/plans/), you can create support tickets for an issue on your StorSimple device directly from the StorSimple Device Manager service summary blade.</span></span> <span data-ttu-id="8dc94-114">Utför följande steg för att skapa en supportförfrågan:</span><span class="sxs-lookup"><span data-stu-id="8dc94-114">Perform the following steps to create a support request:</span></span>

#### <a name="to-create-a-support-request"></a><span data-ttu-id="8dc94-115">Så här skapar du en supportbegäran</span><span class="sxs-lookup"><span data-stu-id="8dc94-115">To create a support request</span></span>

1. <span data-ttu-id="8dc94-116">Gå till StorSimple Device Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8dc94-116">Go to your StorSimple Device Manager service.</span></span> <span data-ttu-id="8dc94-117">Inställningar för tjänsten sammanfattning bladet, gå till **stöd + felsökning** avsnittet och klicka sedan på **ny supportbegäran**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-117">In the service summary blade settings, go to **SUPPORT + TROUBLESHOOTING** section and then click **New support request**.</span></span>
     
    ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport1.png)
   
2. <span data-ttu-id="8dc94-119">I den **ny supportbegäran** bladet väljer **grunderna**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-119">In the **New support request** blade, select **Basics**.</span></span> <span data-ttu-id="8dc94-120">I den **grunderna** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8dc94-120">In the **Basics** blade, do the following steps:</span></span>
   1. <span data-ttu-id="8dc94-121">Från den **utfärda typ** listrutan, Välj **tekniska**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-121">From the **Issue type** drop-down list , select **Technical**.</span></span>
   2. <span data-ttu-id="8dc94-122">Aktuellt **prenumeration**, **Service** typ, och **resurs** (StorSimple Device Manager service) väljs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="8dc94-122">The current **Subscription**, **Service** type, and the **Resource** (StorSimple Device Manager service) are automatically chosen.</span></span> 
   3. <span data-ttu-id="8dc94-123">Välj en **supportavtal** från den nedrullningsbara listan om du har flera planer som är associerad med din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8dc94-123">Select a **Support plan** from the drop-down list if you have multiple plans associated with your subscription.</span></span> <span data-ttu-id="8dc94-124">Du behöver ett betalt supportavtal för att aktivera teknisk Support.</span><span class="sxs-lookup"><span data-stu-id="8dc94-124">You need a paid support plan to enable Technical Support.</span></span>
   4. <span data-ttu-id="8dc94-125">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-125">Click **Next**.</span></span>

       ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport2.png)

3. <span data-ttu-id="8dc94-127">I den **ny supportbegäran** bladet väljer **steg 2 problemet**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-127">In the **New support request** blade, select **Step 2 Problem**.</span></span> <span data-ttu-id="8dc94-128">I den **problemet** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8dc94-128">In the **Problem** blade, do the following steps:</span></span>
    
    1. <span data-ttu-id="8dc94-129">Välj den **allvarlighetsgrad**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-129">Choose the **Severity**.</span></span>
    2. <span data-ttu-id="8dc94-130">Ange om problemet beror på enheten eller StorSimple enheten Manager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8dc94-130">Specify if the issue is related to the appliance or the StorSimple Device Manager service.</span></span>
    3. <span data-ttu-id="8dc94-131">Välj en **kategori** för den här utfärda och ge fler **information** om problemet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-131">Choose a **Category** for this issue and provide more **Details** about the issue.</span></span>
    4. <span data-ttu-id="8dc94-132">Ange startdatum och tidpunkt för problemet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-132">Provide the start date and time for the problem.</span></span>
    5. <span data-ttu-id="8dc94-133">I den **filuppladdning**, klicka på mappikonen och bläddra till support-paketet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-133">In the **File upload**, click the folder icon to browse to your support package.</span></span>
    6. <span data-ttu-id="8dc94-134">Kontrollera **dela diagnostikinformation**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-134">Check **Share diagnostic information**.</span></span>
    7. <span data-ttu-id="8dc94-135">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-135">Click **Next**.</span></span>

       ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport3.png) 

4. <span data-ttu-id="8dc94-137">I den **ny supportbegäran** bladet, klickar du på **steg 3 kontaktinformation**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-137">In the **New support request** blade, click **Step 3 Contact information**.</span></span> <span data-ttu-id="8dc94-138">I den **kontaktinformation** bladet gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8dc94-138">In the **Contact information** blade, do the following steps:</span></span>

    1. <span data-ttu-id="8dc94-139">I den **Kontaktalternativ**, ange önskad kontaktmetod (telefon eller e-post) och språk.</span><span class="sxs-lookup"><span data-stu-id="8dc94-139">In the **Contact options**, provide your preferred contact method (phone or email) and the language.</span></span> <span data-ttu-id="8dc94-140">Svarstiden väljs automatiskt baserat på din prenumeration planen.</span><span class="sxs-lookup"><span data-stu-id="8dc94-140">The response time is automatically selected based on your subscription plan.</span></span>
    2. <span data-ttu-id="8dc94-141">Kontaktinformation, anger du ditt namn, e-post, valfritt kontakt, land.</span><span class="sxs-lookup"><span data-stu-id="8dc94-141">In the Contact information, provide your name, email, optional contact, country.</span></span> <span data-ttu-id="8dc94-142">Välj den **spara kontaktändringar för framtida supportförfrågningar** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="8dc94-142">Select the **Save contact changes for future support requests** check box.</span></span>
    3. <span data-ttu-id="8dc94-143">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-143">Click **Create**.</span></span>
   
        ![Kontakta supporten för MS via nya portalen](./media/storsimple-8000-contact-microsoft-support/contactsupport5.png)   

    <span data-ttu-id="8dc94-145">Microsoft Support använder den här informationen för att nå ut till dig för ytterligare information, diagnostik och upplösning.</span><span class="sxs-lookup"><span data-stu-id="8dc94-145">Microsoft Support will use this information to reach out to you for further information, diagnosis, and resolution.</span></span>
<span data-ttu-id="8dc94-146">När du har skickat en begäran om en supporttekniker kommer att kontakta dig så snart som möjligt för att fortsätta med din begäran.</span><span class="sxs-lookup"><span data-stu-id="8dc94-146">After you have submitted your request, a Support engineer will contact you as soon as possible to proceed with your request.</span></span>

## <a name="manage-a-support-request"></a><span data-ttu-id="8dc94-147">Hantera en supportbegäran</span><span class="sxs-lookup"><span data-stu-id="8dc94-147">Manage a support request</span></span>

<span data-ttu-id="8dc94-148">När du har skapat ett supportärende kan du hantera ärendets livscykel på portalen.</span><span class="sxs-lookup"><span data-stu-id="8dc94-148">After creating a support ticket, you can manage the lifecycle of the ticket from within the portal.</span></span>

#### <a name="to-manage-your-support-requests"></a><span data-ttu-id="8dc94-149">Att hantera dina supportärenden</span><span class="sxs-lookup"><span data-stu-id="8dc94-149">To manage your support requests</span></span>

1. <span data-ttu-id="8dc94-150">Gå till sidan Hjälp och support, gå till **Bläddra > hjälp + support**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-150">To get to the help and support page, navigate to **Browse > Help + support**.</span></span>

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport1.png)

2. <span data-ttu-id="8dc94-152">En tabell lista över alla supportärenden visas i den **hjälp + support** bladet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-152">A tabular listing of All the support requests is displayed in the **Help + support** blade.</span></span>

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport2.png)

3. <span data-ttu-id="8dc94-154">Välj och klicka på en supportbegäran.</span><span class="sxs-lookup"><span data-stu-id="8dc94-154">Select and click a support request.</span></span> <span data-ttu-id="8dc94-155">Du kan visa status och detaljer för denna begäran.</span><span class="sxs-lookup"><span data-stu-id="8dc94-155">You can view the status and the details for this request.</span></span> <span data-ttu-id="8dc94-156">Klicka på **+ nytt meddelande** om du vill följa upp denna begäran.</span><span class="sxs-lookup"><span data-stu-id="8dc94-156">Click **+ New message** if you want to follow up on this request.</span></span>

    ![Hantera supportärenden](./media/storsimple-8000-contact-microsoft-support/managesupport3.png)

## <a name="start-a-support-session-in-windows-powershell-for-storsimple"></a><span data-ttu-id="8dc94-158">Starta en session med stöd i Windows PowerShell för StorSimple</span><span class="sxs-lookup"><span data-stu-id="8dc94-158">Start a support session in Windows PowerShell for StorSimple</span></span>

<span data-ttu-id="8dc94-159">Om du vill felsöka eventuella problem som kan uppstå med StorSimple-enhet behöver kommunicera med Microsoft Support-teamet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-159">To troubleshoot any issues that you might experience with the StorSimple device, you will need to engage with the Microsoft Support team.</span></span> <span data-ttu-id="8dc94-160">Microsoft Support kan behöva använda en session med stöd för att logga in på din enhet.</span><span class="sxs-lookup"><span data-stu-id="8dc94-160">Microsoft Support may need to use a support session to log on to your device.</span></span>

<span data-ttu-id="8dc94-161">Utför följande steg för att starta en supportsession:</span><span class="sxs-lookup"><span data-stu-id="8dc94-161">Perform the following steps to start a support session:</span></span>

#### <a name="to-start-a-support-session"></a><span data-ttu-id="8dc94-162">Starta en supportsession</span><span class="sxs-lookup"><span data-stu-id="8dc94-162">To start a support session</span></span>

1. <span data-ttu-id="8dc94-163">Komma åt enheten direkt via seriekonsolen eller via en telnet-session från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="8dc94-163">Access the device directly by using the serial console or through a telnet session from a remote computer.</span></span> <span data-ttu-id="8dc94-164">Om du vill göra detta, följer du anvisningarna i [Använd PuTTY för att ansluta till enhetens seriekonsol](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span><span class="sxs-lookup"><span data-stu-id="8dc94-164">To do this, follow the steps in [Use PuTTY to connect to the device serial console](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).</span></span>
2. <span data-ttu-id="8dc94-165">I sessionen som öppnas, trycker du på den **RETUR** för att få fram en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="8dc94-165">In the session that opens, press the **Enter** key to get a command prompt.</span></span>
3. <span data-ttu-id="8dc94-166">Välj alternativ 1, i menyn för seriekonsolen **logga in med fullständig åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="8dc94-166">In the serial console menu, select option 1, **Log in with full access**.</span></span>
4. <span data-ttu-id="8dc94-167">I Kommandotolken skriver du följande lösenord:</span><span class="sxs-lookup"><span data-stu-id="8dc94-167">At the prompt, type the following password:</span></span>
   
    `Password1`
5. <span data-ttu-id="8dc94-168">Skriv följande kommando i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="8dc94-168">At the prompt, type the following command:</span></span>
   
    `Enable-HcsSupportAccess`
6. <span data-ttu-id="8dc94-169">En krypterad sträng visas för dig.</span><span class="sxs-lookup"><span data-stu-id="8dc94-169">An encrypted string will be presented to you.</span></span> <span data-ttu-id="8dc94-170">Kopiera den här strängen i en textredigerare, till exempel Anteckningar.</span><span class="sxs-lookup"><span data-stu-id="8dc94-170">Copy this string into a text editor such as Notepad.</span></span>
7. <span data-ttu-id="8dc94-171">Spara den här strängen och skicka den i ett e-postmeddelande till Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="8dc94-171">Save this string and send it in an email message to Microsoft Support.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8dc94-172">Du kan inaktivera stöd åtkomst genom att köra `Disable-HcsSupportAccess`.</span><span class="sxs-lookup"><span data-stu-id="8dc94-172">You can disable support access by running `Disable-HcsSupportAccess`.</span></span> <span data-ttu-id="8dc94-173">StorSimple-enheten försöker inaktivera stöd för åtkomst till 8 timmar när sessionen har startats.</span><span class="sxs-lookup"><span data-stu-id="8dc94-173">The StorSimple device will also attempt to disable support access 8 hours after the session was initiated.</span></span> <span data-ttu-id="8dc94-174">Det är en bra idé att ändra autentiseringsuppgifterna StorSimple-enhet när du initierar en supportsession.</span><span class="sxs-lookup"><span data-stu-id="8dc94-174">It is a best practice to change your StorSimple device credentials after initiating a support session.</span></span>


## <a name="next-steps"></a><span data-ttu-id="8dc94-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8dc94-175">Next steps</span></span>

<span data-ttu-id="8dc94-176">Lär dig hur du [diagnostisera och lösa problem som rör enheten StorSimple 8000-serien](storsimple-troubleshoot-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="8dc94-176">Learn how to [diagnose and solve problems related to your StorSimple 8000 series device](storsimple-troubleshoot-deployment.md)</span></span>
