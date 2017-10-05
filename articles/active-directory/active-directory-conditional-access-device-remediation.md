---
title: "Du kan inte ta dig dit härifrån på Azure Portal från en Windows-enhet | Microsoft Docs"
description: "Läs om ursprunget till Du kan inte ta dig dit härifrån och vad du kan kontrollera för att inte stöta på den här dialogrutan."
services: active-directory
keywords: "enhetsbaserad villkorlig åtkomst, enhetsregistrering, aktivera enhetsregistrering, enhetsregistrering och MDM"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 16543c7bb7b6b236dcc24093c9963bc218ca1fa6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a><span data-ttu-id="7796b-104">Du kan inte ta dig dit härifrån på en Windows-enhet</span><span class="sxs-lookup"><span data-stu-id="7796b-104">You can't get there from here on a Windows device</span></span>

<span data-ttu-id="7796b-105">Vid ett försök att komma åt exempelvis organisationens SharePoint Online-intranät kan du stöta på en sida som anger att *du kan inte komma hit härifrån*.</span><span class="sxs-lookup"><span data-stu-id="7796b-105">During an attempt to, for example, access your organization's SharePoint Online intranet you might run into a page that states that *you can't get there from here*.</span></span> <span data-ttu-id="7796b-106">Du ser den här sidan eftersom administratören har konfigurerat en villkorlig åtkomstprincip som förhindrar åtkomst till organisationens resurser under vissa förhållanden.</span><span class="sxs-lookup"><span data-stu-id="7796b-106">You are seeing this page, because your administrator has configured a conditional access policy that prevents access to your organization's resources under certain conditions.</span></span> <span data-ttu-id="7796b-107">Det kan vara nödvändigt att kontakta supportavdelningen eller administratören för att lösa det här problemet, men det finns några saker som du kan prova själv först.</span><span class="sxs-lookup"><span data-stu-id="7796b-107">While it might be necessary to contact helpdesk or your administrator to get this problem solved, there are a few things you can try out yourself, first.</span></span>

<span data-ttu-id="7796b-108">Om du använder en **Windows**-enhet ska du kontrollera följande:</span><span class="sxs-lookup"><span data-stu-id="7796b-108">If you are using a **Windows** device, you should check the following:</span></span>

- <span data-ttu-id="7796b-109">Använder du en webbläsare som stöds?</span><span class="sxs-lookup"><span data-stu-id="7796b-109">Are you using a supported browser?</span></span>

- <span data-ttu-id="7796b-110">Kör du en version av Windows som stöds på enheten?</span><span class="sxs-lookup"><span data-stu-id="7796b-110">Are you running a supported version of Windows on your device?</span></span>

- <span data-ttu-id="7796b-111">Är din enhet kompatibel?</span><span class="sxs-lookup"><span data-stu-id="7796b-111">Is your device compliant?</span></span>






## <a name="supported-browser"></a><span data-ttu-id="7796b-112">Webbläsare som stöds</span><span class="sxs-lookup"><span data-stu-id="7796b-112">Supported browser</span></span>

<span data-ttu-id="7796b-113">Om administratören har konfigurerat en princip för villkorlig åtkomst kan du bara få åtkomst till organisationens resurser genom att använda en webbläsare som stöds.</span><span class="sxs-lookup"><span data-stu-id="7796b-113">If your administrator has configured a conditional access policy, you can only access your organization's resources by using a supported browser.</span></span> <span data-ttu-id="7796b-114">På en Windows-enhet stöds bara **Internet Explorer** och **Edge**.</span><span class="sxs-lookup"><span data-stu-id="7796b-114">On a Windows device, only **Internet Explorer** and **Edge** are supported.</span></span>

<span data-ttu-id="7796b-115">Du kan enkelt se om du inte åtkomst till en resurs på grund av en webbläsare som inte stöds genom att titta på informationsavsnittet på felsidan:</span><span class="sxs-lookup"><span data-stu-id="7796b-115">You can easily identify whether you can't access a resource due to an unsupported browser by looking at the details section of the error page:</span></span>

<span data-ttu-id="7796b-116">![Meddelandet ”Du kan inte ta dig dit härifrån” för webbläsare som inte stöds](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="7796b-116">!["You can't get there from here" message for unsupported browsers](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span></span>

<span data-ttu-id="7796b-117">Det enda du kan göra är att använda en webbläsare som stöds av programmet för din enhetsplattform.</span><span class="sxs-lookup"><span data-stu-id="7796b-117">The only remediation is to use a browser that the application supports for your device platform.</span></span> <span data-ttu-id="7796b-118">En fullständig lista över webbläsare som stöds finns i [webbläsare som stöds](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span><span class="sxs-lookup"><span data-stu-id="7796b-118">For a complete list of supported browsers, see [supported browsers](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span></span>  


## <a name="supported-versions-of-windows"></a><span data-ttu-id="7796b-119">Versioner av Windows som stöds</span><span class="sxs-lookup"><span data-stu-id="7796b-119">Supported versions of Windows</span></span>

<span data-ttu-id="7796b-120">Följande villkor måste vara uppfyllda för Windows-operativsystemet på din enhet:</span><span class="sxs-lookup"><span data-stu-id="7796b-120">The following must be true about the Windows operating system on your device:</span></span> 

- <span data-ttu-id="7796b-121">Om du kör ett Windows-operativsystem på din enhet måste det vara Windows 7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7796b-121">If you are running a Windows desktop operating system on your device, it needs to be Windows 7 or later.</span></span>
- <span data-ttu-id="7796b-122">Om du kör ett Windows server-operativsystem på din enhet måste det måste vara Windows Server 2008 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7796b-122">If you are running a Windows server operating system on your device, it needs to be Windows Server 2008 R2 or later.</span></span> 


## <a name="compliant-device"></a><span data-ttu-id="7796b-123">Kompatibel enhet</span><span class="sxs-lookup"><span data-stu-id="7796b-123">Compliant device</span></span>

<span data-ttu-id="7796b-124">Administratören kan ha konfigurerat en villkorlig åtkomstprincip som tillåter åtkomst till organisationens resurser enbart från kompatibla enheter.</span><span class="sxs-lookup"><span data-stu-id="7796b-124">Your administrator might have configured a conditional access policy that allows access to your organization's resources only from compliant devices.</span></span> <span data-ttu-id="7796b-125">För att vara kompatibel måste enheten antingen vara ansluten till din lokala Active Directory eller till din Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7796b-125">To be compliant, your device must be either joined to your on-premises Active Directory or joined to your Azure Active Directory.</span></span>

<span data-ttu-id="7796b-126">Du kan enkelt se om du inte åtkomst till en resurs på grund av en enhet som inte är kompatibel genom att titta på informationsavsnittet på felsidan:</span><span class="sxs-lookup"><span data-stu-id="7796b-126">You can easily identify whether you can't access a resource due to a device that is not compliant by looking at the details section of the error page:</span></span>
 
<span data-ttu-id="7796b-127">![Meddelandet ”Du kan inte ta dig dit härifrån” för enheter som inte har registrerats](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="7796b-127">!["You can't get there from here" messages for unregistered devices](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span></span>


### <a name="is-your-device-joined-to-an-on-premises-active-directory"></a><span data-ttu-id="7796b-128">Är enheten ansluten till en lokal Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7796b-128">Is your device joined to an on-premises Active Directory?</span></span>

<span data-ttu-id="7796b-129">**Om enheten är ansluten till en lokal Active Directory i din organisation:**</span><span class="sxs-lookup"><span data-stu-id="7796b-129">**If your device is joined to an on-premises Active Directory in your organization:**</span></span>

1. <span data-ttu-id="7796b-130">Kontrollera att du har loggat in i Windows med ditt arbetskonto (ditt Active Directory-konto).</span><span class="sxs-lookup"><span data-stu-id="7796b-130">Make sure that you sign in to Windows by using your work account (your Active Directory account).</span></span>
2. <span data-ttu-id="7796b-131">Anslut till företagsnätverket via ett virtuellt privat nätverk (VPN) eller DirectAccess.</span><span class="sxs-lookup"><span data-stu-id="7796b-131">Connect to your corporate network via a virtual private network (VPN) or DirectAccess.</span></span>
3. <span data-ttu-id="7796b-132">När du är ansluten, trycker du på Windows-tangenten + L för att låsa Windows-sessionen.</span><span class="sxs-lookup"><span data-stu-id="7796b-132">After you are connected, press the Windows logo key + the L key to lock your Windows session.</span></span>
4. <span data-ttu-id="7796b-133">Lås upp Windows-sessionen genom att ange autentiseringsuppgifterna för ditt arbetskonto.</span><span class="sxs-lookup"><span data-stu-id="7796b-133">Unlock your Windows session by entering your work account credentials.</span></span>
5. <span data-ttu-id="7796b-134">Vänta en stund och prova sedan att ansluta till programmet eller tjänsten igen.</span><span class="sxs-lookup"><span data-stu-id="7796b-134">Wait for a minute, and then try again to access the application or service.</span></span>
6. <span data-ttu-id="7796b-135">Om samma sida visas, klickar du på länken **Mer information** och kontaktar din administratör och anger informationen.</span><span class="sxs-lookup"><span data-stu-id="7796b-135">If you see the same page, click the **More details** link, and then contact your administrator with the details.</span></span>


### <a name="is-your-device-not-joined-to-an-on-premises-active-directory"></a><span data-ttu-id="7796b-136">Är enheten inte ansluten till en lokal Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7796b-136">Is your device not joined to an on-premises Active Directory?</span></span>

<span data-ttu-id="7796b-137">Om enheten inte är ansluten till en lokal Active Directory och kör Windows 10 har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="7796b-137">If your device is not joined to an on-premises Active Directory and runs Windows 10, you have two options:</span></span>

* <span data-ttu-id="7796b-138">Kör Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="7796b-138">Run Azure AD Join</span></span>
* <span data-ttu-id="7796b-139">Lägg till ditt arbets- eller skolkonto till Windows</span><span class="sxs-lookup"><span data-stu-id="7796b-139">Add your work or school account to Windows</span></span>

<span data-ttu-id="7796b-140">Information om hur de här alternativen skiljer sig åt finns i [Använda Windows 10-enheter i din arbetsplats](active-directory-azureadjoin-windows10-devices.md).</span><span class="sxs-lookup"><span data-stu-id="7796b-140">For information about how these options are different, see [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md).</span></span>  
<span data-ttu-id="7796b-141">Om enheten:</span><span class="sxs-lookup"><span data-stu-id="7796b-141">If your device:</span></span>

- <span data-ttu-id="7796b-142">Tillhör din organisation bör du köra Azure AD Join.</span><span class="sxs-lookup"><span data-stu-id="7796b-142">Belongs to your organization, you should run Azure AD Join.</span></span>
- <span data-ttu-id="7796b-143">Är det en personlig enhet eller en Windows Phone bör du lägga till ditt arbets- eller skolkonto i Windows</span><span class="sxs-lookup"><span data-stu-id="7796b-143">Is a personal device or a Windows phone, you should add your work or school account to Windows</span></span> 



#### <a name="azure-ad-join-on-windows-10"></a><span data-ttu-id="7796b-144">Azure AD Join i Windows 10</span><span class="sxs-lookup"><span data-stu-id="7796b-144">Azure AD Join on Windows 10</span></span>

<span data-ttu-id="7796b-145">Stegen för att ansluta din enhet till Azure AD beror på vilken version av Windows 10 som körs på den.</span><span class="sxs-lookup"><span data-stu-id="7796b-145">The steps to join your device to Azure AD are tied the version of Windows 10 you are running on it.</span></span> <span data-ttu-id="7796b-146">Om du vill kontrollera vilken version av operativsystemet Windows 10 som körs, kör du kommandot **winver**:</span><span class="sxs-lookup"><span data-stu-id="7796b-146">To determine the version of your Windows 10 operating system, run the **winver** command:</span></span> 

![Windows-version](./media/active-directory-conditional-access-device-remediation/03.png )


<span data-ttu-id="7796b-148">**Windows 10 Anniversary Update (version 1607):**</span><span class="sxs-lookup"><span data-stu-id="7796b-148">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="7796b-149">Öppna appen **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7796b-149">Open the **Settings** app.</span></span>
2. <span data-ttu-id="7796b-150">Klicka på **Konton** > **Access work or school** (Åtkomst till arbete eller skola).</span><span class="sxs-lookup"><span data-stu-id="7796b-150">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="7796b-151">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="7796b-151">Click **Connect**.</span></span>
4. <span data-ttu-id="7796b-152">Klicka på **Anslut denna enhet till Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="7796b-152">Click **Join this device to Azure AD**.</span></span>
5. <span data-ttu-id="7796b-153">Autentisera dig i din organisation, ange multifaktorautentisering vid uppmaning och följ sedan stegen som visas.</span><span class="sxs-lookup"><span data-stu-id="7796b-153">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
6. <span data-ttu-id="7796b-154">Logga ut och logga sedan in igen med ditt arbetskonto.</span><span class="sxs-lookup"><span data-stu-id="7796b-154">Sign out, and then sign in with your work account.</span></span>
7. <span data-ttu-id="7796b-155">Prova att öppna programmet igen.</span><span class="sxs-lookup"><span data-stu-id="7796b-155">Try again to access the application.</span></span>

<span data-ttu-id="7796b-156">**Windows 10 November 2015 Update (version 1511):**</span><span class="sxs-lookup"><span data-stu-id="7796b-156">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="7796b-157">Öppna appen **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7796b-157">Open the **Settings** app.</span></span>
2. <span data-ttu-id="7796b-158">Klicka på **System** > **Om**.</span><span class="sxs-lookup"><span data-stu-id="7796b-158">Click **System** > **About**.</span></span>
3. <span data-ttu-id="7796b-159">Klicka på **Anslut till Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="7796b-159">Click **Join Azure AD**.</span></span>
4. <span data-ttu-id="7796b-160">Autentisera dig i din organisation, ange multifaktorautentisering vid uppmaning och följ sedan stegen som visas.</span><span class="sxs-lookup"><span data-stu-id="7796b-160">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="7796b-161">Logga ut och logga sedan in igen med ditt arbetskonto (ditt Azure AD-konto).</span><span class="sxs-lookup"><span data-stu-id="7796b-161">Sign out, and then sign in with your work account (your Azure AD account).</span></span>
6. <span data-ttu-id="7796b-162">Prova att öppna programmet igen.</span><span class="sxs-lookup"><span data-stu-id="7796b-162">Try again to access the application.</span></span>


#### <a name="workplace-join-on-windows-81"></a><span data-ttu-id="7796b-163">Anslut till arbetsplats i Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="7796b-163">Workplace Join on Windows 8.1</span></span>

<span data-ttu-id="7796b-164">Om enheten inte är domänansluten och kör Windows 8.1 kan du, för att göra en Workplace Join och registrera enheten i Microsoft Intune göra följande steg:</span><span class="sxs-lookup"><span data-stu-id="7796b-164">If your device is not domain-joined and runs Windows 8.1, to do a Workplace Join and enroll in Microsoft Intune, do the following steps:</span></span>

1. <span data-ttu-id="7796b-165">Öppna **Datorinställningar**.</span><span class="sxs-lookup"><span data-stu-id="7796b-165">Open **PC Settings**.</span></span>
2. <span data-ttu-id="7796b-166">Klicka på **Nätverk** > **Arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="7796b-166">Click **Network** > **Workplace**.</span></span>
3. <span data-ttu-id="7796b-167">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="7796b-167">Click **Join**.</span></span>
4. <span data-ttu-id="7796b-168">Autentisera dig i din organisation, ange multifaktorautentisering vid uppmaning och följ sedan stegen som visas.</span><span class="sxs-lookup"><span data-stu-id="7796b-168">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="7796b-169">Klicka på **Aktivera**.</span><span class="sxs-lookup"><span data-stu-id="7796b-169">Click **Turn on**.</span></span>
6. <span data-ttu-id="7796b-170">Prova att öppna programmet igen.</span><span class="sxs-lookup"><span data-stu-id="7796b-170">Try again to access the application.</span></span>



#### <a name="add-your-work-or-school-account-to-windows"></a><span data-ttu-id="7796b-171">Lägg till ditt arbets- eller skolkonto till Windows</span><span class="sxs-lookup"><span data-stu-id="7796b-171">Add your work or school account to Windows</span></span> 


<span data-ttu-id="7796b-172">**Windows 10 Anniversary Update (version 1607):**</span><span class="sxs-lookup"><span data-stu-id="7796b-172">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="7796b-173">Öppna appen **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7796b-173">Open the **Settings** app.</span></span>
2. <span data-ttu-id="7796b-174">Klicka på **Konton** > **Access work or school** (Åtkomst till arbete eller skola).</span><span class="sxs-lookup"><span data-stu-id="7796b-174">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="7796b-175">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="7796b-175">Click **Connect**.</span></span>
4. <span data-ttu-id="7796b-176">Autentisera dig i din organisation, ange multifaktorautentisering vid uppmaning och följ sedan stegen som visas.</span><span class="sxs-lookup"><span data-stu-id="7796b-176">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="7796b-177">Prova att öppna programmet igen.</span><span class="sxs-lookup"><span data-stu-id="7796b-177">Try again to access the application.</span></span>


<span data-ttu-id="7796b-178">**Windows 10 November 2015 Update (version 1511):**</span><span class="sxs-lookup"><span data-stu-id="7796b-178">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="7796b-179">Öppna appen **Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7796b-179">Open the **Settings** app.</span></span>
2. <span data-ttu-id="7796b-180">Klicka på **Konton** > **Dina konton**.</span><span class="sxs-lookup"><span data-stu-id="7796b-180">Click **Accounts** > **Your accounts**.</span></span>
3. <span data-ttu-id="7796b-181">Klicka på **Arbets- eller skolkonto**.</span><span class="sxs-lookup"><span data-stu-id="7796b-181">Click **Add work or school account**.</span></span>
4. <span data-ttu-id="7796b-182">Autentisera dig i din organisation, ange multifaktorautentisering vid uppmaning och följ sedan stegen som visas.</span><span class="sxs-lookup"><span data-stu-id="7796b-182">Authenticate to your organization, provide multi-factor authentication if prompted, and then follow the steps that are shown.</span></span>
5. <span data-ttu-id="7796b-183">Prova att öppna programmet igen.</span><span class="sxs-lookup"><span data-stu-id="7796b-183">Try again to access the application.</span></span>





## <a name="next-steps"></a><span data-ttu-id="7796b-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7796b-184">Next steps</span></span>
[<span data-ttu-id="7796b-185">Villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7796b-185">Azure Active Directory conditional access</span></span>](active-directory-conditional-access.md)

