---
title: "aaaYou kan inte hämta det hädanefter hello Azure-portalen från en Windows-enhet | Microsoft Docs"
description: "Lär dig mer om det går inte att hämta det här kommer från och vad du kan kontrollera tooavoid som körs i den här dialogrutan."
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
ms.openlocfilehash: eda2aa10fbff5b3e515723219f61c1cb6f634e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="you-cant-get-there-from-here-on-a-windows-device"></a><span data-ttu-id="50e0f-104">Du kan inte ta dig dit härifrån på en Windows-enhet</span><span class="sxs-lookup"><span data-stu-id="50e0f-104">You can't get there from here on a Windows device</span></span>

<span data-ttu-id="50e0f-105">Vid ett försök att komma åt exempelvis organisationens SharePoint Online-intranät kan du stöta på en sida som anger att *du kan inte komma hit härifrån*.</span><span class="sxs-lookup"><span data-stu-id="50e0f-105">During an attempt to, for example, access your organization's SharePoint Online intranet you might run into a page that states that *you can't get there from here*.</span></span> <span data-ttu-id="50e0f-106">Du ser den här sidan eftersom administratören har konfigurerat en villkorlig åtkomstprincip som förhindrar åtkomst tooyour organisationens resurser under vissa förhållanden.</span><span class="sxs-lookup"><span data-stu-id="50e0f-106">You are seeing this page, because your administrator has configured a conditional access policy that prevents access tooyour organization's resources under certain conditions.</span></span> <span data-ttu-id="50e0f-107">Det kan vara nödvändigt toocontact support eller din administratör tooget lösa det här problemet, finns men det några saker som du kan prova själv först.</span><span class="sxs-lookup"><span data-stu-id="50e0f-107">While it might be necessary toocontact helpdesk or your administrator tooget this problem solved, there are a few things you can try out yourself, first.</span></span>

<span data-ttu-id="50e0f-108">Om du använder en **Windows** enhet, ska du kontrollera hello följande:</span><span class="sxs-lookup"><span data-stu-id="50e0f-108">If you are using a **Windows** device, you should check hello following:</span></span>

- <span data-ttu-id="50e0f-109">Använder du en webbläsare som stöds?</span><span class="sxs-lookup"><span data-stu-id="50e0f-109">Are you using a supported browser?</span></span>

- <span data-ttu-id="50e0f-110">Kör du en version av Windows som stöds på enheten?</span><span class="sxs-lookup"><span data-stu-id="50e0f-110">Are you running a supported version of Windows on your device?</span></span>

- <span data-ttu-id="50e0f-111">Är din enhet kompatibel?</span><span class="sxs-lookup"><span data-stu-id="50e0f-111">Is your device compliant?</span></span>






## <a name="supported-browser"></a><span data-ttu-id="50e0f-112">Webbläsare som stöds</span><span class="sxs-lookup"><span data-stu-id="50e0f-112">Supported browser</span></span>

<span data-ttu-id="50e0f-113">Om administratören har konfigurerat en princip för villkorlig åtkomst kan du bara få åtkomst till organisationens resurser genom att använda en webbläsare som stöds.</span><span class="sxs-lookup"><span data-stu-id="50e0f-113">If your administrator has configured a conditional access policy, you can only access your organization's resources by using a supported browser.</span></span> <span data-ttu-id="50e0f-114">På en Windows-enhet stöds bara **Internet Explorer** och **Edge**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-114">On a Windows device, only **Internet Explorer** and **Edge** are supported.</span></span>

<span data-ttu-id="50e0f-115">Du kan enkelt identifiera om du inte kommer åt en resurs på grund av tooan stöds inte webbläsaren genom att titta på hello informationsavsnittet till hello felsidan:</span><span class="sxs-lookup"><span data-stu-id="50e0f-115">You can easily identify whether you can't access a resource due tooan unsupported browser by looking at hello details section of hello error page:</span></span>

<span data-ttu-id="50e0f-116">![Meddelandet ”Du kan inte ta dig dit härifrån” för webbläsare som inte stöds](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="50e0f-116">!["You can't get there from here" message for unsupported browsers](./media/active-directory-conditional-access-device-remediation/02.png "Scenario")</span></span>

<span data-ttu-id="50e0f-117">hello endast reparationen har toouse en webbläsare som programmet hello stöder för din enhetsplattform.</span><span class="sxs-lookup"><span data-stu-id="50e0f-117">hello only remediation is toouse a browser that hello application supports for your device platform.</span></span> <span data-ttu-id="50e0f-118">En fullständig lista över webbläsare som stöds finns i [webbläsare som stöds](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span><span class="sxs-lookup"><span data-stu-id="50e0f-118">For a complete list of supported browsers, see [supported browsers](active-directory-conditional-access-supported-apps.md#supported-browsers-for-device-based-policies).</span></span>  


## <a name="supported-versions-of-windows"></a><span data-ttu-id="50e0f-119">Versioner av Windows som stöds</span><span class="sxs-lookup"><span data-stu-id="50e0f-119">Supported versions of Windows</span></span>

<span data-ttu-id="50e0f-120">följande hello måste vara sant om hello Windows-operativsystemet på din enhet:</span><span class="sxs-lookup"><span data-stu-id="50e0f-120">hello following must be true about hello Windows operating system on your device:</span></span> 

- <span data-ttu-id="50e0f-121">Om du kör ett Windows-operativsystemet på din enhet måste toobe Windows 7 eller senare.</span><span class="sxs-lookup"><span data-stu-id="50e0f-121">If you are running a Windows desktop operating system on your device, it needs toobe Windows 7 or later.</span></span>
- <span data-ttu-id="50e0f-122">Om du kör ett Windows server-operativsystem på enheten, måste det toobe Windows Server 2008 R2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="50e0f-122">If you are running a Windows server operating system on your device, it needs toobe Windows Server 2008 R2 or later.</span></span> 


## <a name="compliant-device"></a><span data-ttu-id="50e0f-123">Kompatibel enhet</span><span class="sxs-lookup"><span data-stu-id="50e0f-123">Compliant device</span></span>

<span data-ttu-id="50e0f-124">Administratören kan ha angett en princip för villkorlig åtkomst som tillåter åtkomst tooyour organisationens resurser från kompatibla enheter.</span><span class="sxs-lookup"><span data-stu-id="50e0f-124">Your administrator might have configured a conditional access policy that allows access tooyour organization's resources only from compliant devices.</span></span> <span data-ttu-id="50e0f-125">toobe kompatibla enheten måste vara antingen domänanslutna tooyour lokala Active Directory eller anslutna tooyour Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="50e0f-125">toobe compliant, your device must be either joined tooyour on-premises Active Directory or joined tooyour Azure Active Directory.</span></span>

<span data-ttu-id="50e0f-126">Du kan enkelt identifiera om du inte kommer åt en resurs på grund av tooa enhet som inte är kompatibel genom att titta på hello informationsavsnittet till hello felsidan:</span><span class="sxs-lookup"><span data-stu-id="50e0f-126">You can easily identify whether you can't access a resource due tooa device that is not compliant by looking at hello details section of hello error page:</span></span>
 
<span data-ttu-id="50e0f-127">![Meddelandet ”Du kan inte ta dig dit härifrån” för enheter som inte har registrerats](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span><span class="sxs-lookup"><span data-stu-id="50e0f-127">!["You can't get there from here" messages for unregistered devices](./media/active-directory-conditional-access-device-remediation/01.png "Scenario")</span></span>


### <a name="is-your-device-joined-tooan-on-premises-active-directory"></a><span data-ttu-id="50e0f-128">Är din enhet ansluten tooan lokala Active Directory?</span><span class="sxs-lookup"><span data-stu-id="50e0f-128">Is your device joined tooan on-premises Active Directory?</span></span>

<span data-ttu-id="50e0f-129">**Om enheten är ansluten tooan lokala Active Directory i din organisation:**</span><span class="sxs-lookup"><span data-stu-id="50e0f-129">**If your device is joined tooan on-premises Active Directory in your organization:**</span></span>

1. <span data-ttu-id="50e0f-130">Kontrollera att du loggar in tooWindows genom att använda ditt arbetskonto (din Active Directory-konto).</span><span class="sxs-lookup"><span data-stu-id="50e0f-130">Make sure that you sign in tooWindows by using your work account (your Active Directory account).</span></span>
2. <span data-ttu-id="50e0f-131">Ansluta tooyour företagets nätverk via ett virtuellt privat nätverk (VPN) eller DirectAccess.</span><span class="sxs-lookup"><span data-stu-id="50e0f-131">Connect tooyour corporate network via a virtual private network (VPN) or DirectAccess.</span></span>
3. <span data-ttu-id="50e0f-132">När du har anslutit tryck hello Windows-tangenten + hello L viktiga toolock Windows-sessionen.</span><span class="sxs-lookup"><span data-stu-id="50e0f-132">After you are connected, press hello Windows logo key + hello L key toolock your Windows session.</span></span>
4. <span data-ttu-id="50e0f-133">Lås upp Windows-sessionen genom att ange autentiseringsuppgifterna för ditt arbetskonto.</span><span class="sxs-lookup"><span data-stu-id="50e0f-133">Unlock your Windows session by entering your work account credentials.</span></span>
5. <span data-ttu-id="50e0f-134">Vänta en stund och försök sedan igen tooaccess hello programmet eller tjänsten.</span><span class="sxs-lookup"><span data-stu-id="50e0f-134">Wait for a minute, and then try again tooaccess hello application or service.</span></span>
6. <span data-ttu-id="50e0f-135">Om du ser hello samma klickar du på hello **mer** länka och kontakta sedan administratören med hello information.</span><span class="sxs-lookup"><span data-stu-id="50e0f-135">If you see hello same page, click hello **More details** link, and then contact your administrator with hello details.</span></span>


### <a name="is-your-device-not-joined-tooan-on-premises-active-directory"></a><span data-ttu-id="50e0f-136">Är din enhet inte ansluten tooan lokala Active Directory?</span><span class="sxs-lookup"><span data-stu-id="50e0f-136">Is your device not joined tooan on-premises Active Directory?</span></span>

<span data-ttu-id="50e0f-137">Om enheten inte är ansluten tooan lokala Active Directory och kör Windows 10, har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="50e0f-137">If your device is not joined tooan on-premises Active Directory and runs Windows 10, you have two options:</span></span>

* <span data-ttu-id="50e0f-138">Kör Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="50e0f-138">Run Azure AD Join</span></span>
* <span data-ttu-id="50e0f-139">Lägg till ditt arbete eller skola konto tooWindows</span><span class="sxs-lookup"><span data-stu-id="50e0f-139">Add your work or school account tooWindows</span></span>

<span data-ttu-id="50e0f-140">Information om hur de här alternativen skiljer sig åt finns i [Använda Windows 10-enheter i din arbetsplats](active-directory-azureadjoin-windows10-devices.md).</span><span class="sxs-lookup"><span data-stu-id="50e0f-140">For information about how these options are different, see [Using Windows 10 devices in your workplace](active-directory-azureadjoin-windows10-devices.md).</span></span>  
<span data-ttu-id="50e0f-141">Om enheten:</span><span class="sxs-lookup"><span data-stu-id="50e0f-141">If your device:</span></span>

- <span data-ttu-id="50e0f-142">Tillhör tooyour organisation bör du köra Azure AD Join.</span><span class="sxs-lookup"><span data-stu-id="50e0f-142">Belongs tooyour organization, you should run Azure AD Join.</span></span>
- <span data-ttu-id="50e0f-143">Är en personlig enhet eller en Windows phone ska lägga till ditt arbete eller skola konto tooWindows</span><span class="sxs-lookup"><span data-stu-id="50e0f-143">Is a personal device or a Windows phone, you should add your work or school account tooWindows</span></span> 



#### <a name="azure-ad-join-on-windows-10"></a><span data-ttu-id="50e0f-144">Azure AD Join i Windows 10</span><span class="sxs-lookup"><span data-stu-id="50e0f-144">Azure AD Join on Windows 10</span></span>

<span data-ttu-id="50e0f-145">hello knutna steg toojoin din enhet tooAzure AD är hello version av Windows 10 körs på den.</span><span class="sxs-lookup"><span data-stu-id="50e0f-145">hello steps toojoin your device tooAzure AD are tied hello version of Windows 10 you are running on it.</span></span> <span data-ttu-id="50e0f-146">toodetermine hello version av operativsystemet Windows 10, kör hello **winver** kommando:</span><span class="sxs-lookup"><span data-stu-id="50e0f-146">toodetermine hello version of your Windows 10 operating system, run hello **winver** command:</span></span> 

![Windows-version](./media/active-directory-conditional-access-device-remediation/03.png )


<span data-ttu-id="50e0f-148">**Windows 10 Anniversary Update (version 1607):**</span><span class="sxs-lookup"><span data-stu-id="50e0f-148">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="50e0f-149">Öppna hello **inställningar** app.</span><span class="sxs-lookup"><span data-stu-id="50e0f-149">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="50e0f-150">Klicka på **Konton** > **Access work or school** (Åtkomst till arbete eller skola).</span><span class="sxs-lookup"><span data-stu-id="50e0f-150">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="50e0f-151">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-151">Click **Connect**.</span></span>
4. <span data-ttu-id="50e0f-152">Klicka på **ansluta till den här enheten tooAzure AD**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-152">Click **Join this device tooAzure AD**.</span></span>
5. <span data-ttu-id="50e0f-153">Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.</span><span class="sxs-lookup"><span data-stu-id="50e0f-153">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
6. <span data-ttu-id="50e0f-154">Logga ut och logga sedan in igen med ditt arbetskonto.</span><span class="sxs-lookup"><span data-stu-id="50e0f-154">Sign out, and then sign in with your work account.</span></span>
7. <span data-ttu-id="50e0f-155">Försök igen tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="50e0f-155">Try again tooaccess hello application.</span></span>

<span data-ttu-id="50e0f-156">**Windows 10 November 2015 Update (version 1511):**</span><span class="sxs-lookup"><span data-stu-id="50e0f-156">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="50e0f-157">Öppna hello **inställningar** app.</span><span class="sxs-lookup"><span data-stu-id="50e0f-157">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="50e0f-158">Klicka på **System** > **Om**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-158">Click **System** > **About**.</span></span>
3. <span data-ttu-id="50e0f-159">Klicka på **Anslut till Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-159">Click **Join Azure AD**.</span></span>
4. <span data-ttu-id="50e0f-160">Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.</span><span class="sxs-lookup"><span data-stu-id="50e0f-160">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="50e0f-161">Logga ut och logga sedan in igen med ditt arbetskonto (ditt Azure AD-konto).</span><span class="sxs-lookup"><span data-stu-id="50e0f-161">Sign out, and then sign in with your work account (your Azure AD account).</span></span>
6. <span data-ttu-id="50e0f-162">Försök igen tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="50e0f-162">Try again tooaccess hello application.</span></span>


#### <a name="workplace-join-on-windows-81"></a><span data-ttu-id="50e0f-163">Anslut till arbetsplats i Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="50e0f-163">Workplace Join on Windows 8.1</span></span>

<span data-ttu-id="50e0f-164">Om enheten inte är ansluten till domänen och kör Windows 8.1, toodo en anslutning till arbetsplatsen och registreras i Microsoft Intune, hello gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="50e0f-164">If your device is not domain-joined and runs Windows 8.1, toodo a Workplace Join and enroll in Microsoft Intune, do hello following steps:</span></span>

1. <span data-ttu-id="50e0f-165">Öppna **Datorinställningar**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-165">Open **PC Settings**.</span></span>
2. <span data-ttu-id="50e0f-166">Klicka på **Nätverk** > **Arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-166">Click **Network** > **Workplace**.</span></span>
3. <span data-ttu-id="50e0f-167">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-167">Click **Join**.</span></span>
4. <span data-ttu-id="50e0f-168">Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.</span><span class="sxs-lookup"><span data-stu-id="50e0f-168">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="50e0f-169">Klicka på **Aktivera**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-169">Click **Turn on**.</span></span>
6. <span data-ttu-id="50e0f-170">Försök igen tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="50e0f-170">Try again tooaccess hello application.</span></span>



#### <a name="add-your-work-or-school-account-toowindows"></a><span data-ttu-id="50e0f-171">Lägg till ditt arbete eller skola konto tooWindows</span><span class="sxs-lookup"><span data-stu-id="50e0f-171">Add your work or school account tooWindows</span></span> 


<span data-ttu-id="50e0f-172">**Windows 10 Anniversary Update (version 1607):**</span><span class="sxs-lookup"><span data-stu-id="50e0f-172">**Windows 10 Anniversary Update (Version 1607):**</span></span>

1. <span data-ttu-id="50e0f-173">Öppna hello **inställningar** app.</span><span class="sxs-lookup"><span data-stu-id="50e0f-173">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="50e0f-174">Klicka på **Konton** > **Access work or school** (Åtkomst till arbete eller skola).</span><span class="sxs-lookup"><span data-stu-id="50e0f-174">Click **Accounts** > **Access work or school**.</span></span>
3. <span data-ttu-id="50e0f-175">Klicka på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-175">Click **Connect**.</span></span>
4. <span data-ttu-id="50e0f-176">Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.</span><span class="sxs-lookup"><span data-stu-id="50e0f-176">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="50e0f-177">Försök igen tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="50e0f-177">Try again tooaccess hello application.</span></span>


<span data-ttu-id="50e0f-178">**Windows 10 November 2015 Update (version 1511):**</span><span class="sxs-lookup"><span data-stu-id="50e0f-178">**Windows 10 November 2015 Update (Version 1511):**</span></span>

1. <span data-ttu-id="50e0f-179">Öppna hello **inställningar** app.</span><span class="sxs-lookup"><span data-stu-id="50e0f-179">Open hello **Settings** app.</span></span>
2. <span data-ttu-id="50e0f-180">Klicka på **Konton** > **Dina konton**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-180">Click **Accounts** > **Your accounts**.</span></span>
3. <span data-ttu-id="50e0f-181">Klicka på **Arbets- eller skolkonto**.</span><span class="sxs-lookup"><span data-stu-id="50e0f-181">Click **Add work or school account**.</span></span>
4. <span data-ttu-id="50e0f-182">Autentisera tooyour organisation, ange multifaktorautentisering om du uppmanas och följ hello steg som visas.</span><span class="sxs-lookup"><span data-stu-id="50e0f-182">Authenticate tooyour organization, provide multi-factor authentication if prompted, and then follow hello steps that are shown.</span></span>
5. <span data-ttu-id="50e0f-183">Försök igen tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="50e0f-183">Try again tooaccess hello application.</span></span>





## <a name="next-steps"></a><span data-ttu-id="50e0f-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50e0f-184">Next steps</span></span>
[<span data-ttu-id="50e0f-185">Villkorlig åtkomst i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="50e0f-185">Azure Active Directory conditional access</span></span>](active-directory-conditional-access.md)

