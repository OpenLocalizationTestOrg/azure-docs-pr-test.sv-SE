---
title: "aaaHow tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory | Microsoft Docs"
description: "Konfigurera din domänanslutna Windows-enheter tooregister automatiskt med Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 1d736eba734418231f12e23a8fc1a93405f129c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-device-registration"></a><span data-ttu-id="6d973-103">Komma igång med enhetsregistrering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d973-103">Get started with Azure Active Directory device registration</span></span>

<span data-ttu-id="6d973-104">Azure Active Directory device registration är hello grunden för scenarier med enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6d973-104">Azure Active Directory device registration is hello foundation for device-based conditional access scenarios.</span></span> <span data-ttu-id="6d973-105">När en enhet registreras, tillhandahåller Azure Active Directory-enhetsregistrering hello-enhet med en identitet som används tooauthenticate hello enhet när hello användaren loggar in.</span><span class="sxs-lookup"><span data-stu-id="6d973-105">When a device is registered, Azure Active Directory device registration provides hello device with an identity which is used tooauthenticate hello device when hello user signs in.</span></span> <span data-ttu-id="6d973-106">hello autentiserade enheten och hello attribut för hello enhet, kan du sedan vara används tooenforce villkorliga åtkomstprinciper för program som finns i hello molnet och lokalt.</span><span class="sxs-lookup"><span data-stu-id="6d973-106">hello authenticated device, and hello attributes of hello device, can then be used tooenforce conditional access policies for applications that are hosted in hello cloud and on-premises.</span></span>

<span data-ttu-id="6d973-107">När den kombineras med en enhetsattributen lösning för mobila enheter, till exempel Microsoft Intune, uppdateras hello attributen i Azure Active Directory med ytterligare information om hello enhet.</span><span class="sxs-lookup"><span data-stu-id="6d973-107">When combined with a mobile device management(MDM) solution such as Microsoft Intune, hello device attributes in Azure Active Directory are updated with additional information about hello device.</span></span> <span data-ttu-id="6d973-108">Detta ger dig toocreate regler för villkorlig åtkomst som säkerställer att åtkomsten från enheter toomeet dina krav för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="6d973-108">This allows you toocreate conditional access rules that enforce access from devices toomeet your standards for security and compliance.</span></span> <span data-ttu-id="6d973-109">Mer information om registrerar enheter i Microsoft Intune finns i registrera enheter för hantering i Intune.</span><span class="sxs-lookup"><span data-stu-id="6d973-109">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune.</span></span>
<span data-ttu-id="6d973-110">Scenarier som aktiveras med Azure Active Directory Device Registration Azure Active Directory Device Registration inkluderar stöd för iOS, Android och Windows enheter.</span><span class="sxs-lookup"><span data-stu-id="6d973-110">Scenarios enabled by Azure Active Directory Device Registration Azure Active Directory Device Registration includes support for iOS, Android, and Windows devices.</span></span> <span data-ttu-id="6d973-111">hello enskilda scenarierna som använder Azure AD Device Registration kan ha mer specifika krav och Plattformsstöd.</span><span class="sxs-lookup"><span data-stu-id="6d973-111">hello individual scenarios that utilize Azure AD Device Registration may have more specific requirements and platform support.</span></span> 

<span data-ttu-id="6d973-112">Dessa scenarier är följande:</span><span class="sxs-lookup"><span data-stu-id="6d973-112">These scenarios are as follows:</span></span>

- <span data-ttu-id="6d973-113">**Villkorlig åtkomst för Office 365-program med Microsoft Intune:** IT-administratörer kan etablera villkorlig åtkomst enheten principer toosecure företagsresurser, medan på hello samma tid så att informationsarbetare på kompatibla enheter tooaccess hello-tjänster.</span><span class="sxs-lookup"><span data-stu-id="6d973-113">**Conditional access for Office 365 applications with Microsoft Intune:** IT admins can provision conditional access device policies toosecure corporate resources, while at hello same time allowing information workers on compliant devices tooaccess hello services.</span></span> <span data-ttu-id="6d973-114">Mer information finns i Enhetsprinciper för villkorlig åtkomst för Office 365-tjänster.</span><span class="sxs-lookup"><span data-stu-id="6d973-114">For more information, see Conditional Access Device Policies for Office 365 services.</span></span>

- <span data-ttu-id="6d973-115">**Villkorlig åtkomst tooapplications som finns lokalt:** du kan använda registrerade enheter med åtkomstprinciper för program som är konfigurerade toouse AD FS med Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="6d973-115">**Conditional access tooapplications that are hosted on-premises:** You can use registered devices with access policies for applications that are configured toouse AD FS with Windows Server 2012 R2.</span></span> <span data-ttu-id="6d973-116">Mer information om hur du konfigurerar lokal villkorlig åtkomst finns i [Konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory Device Registration](active-directory-device-registration-on-premises-setup.md).</span><span class="sxs-lookup"><span data-stu-id="6d973-116">For more information about setting up conditional access for on-premises, see [Setting up On-premises Conditional Access using Azure Active Directory device registration](active-directory-device-registration-on-premises-setup.md).</span></span>

## <a name="setting-up-azure-active-directory-device-registration"></a><span data-ttu-id="6d973-117">Konfigurera Azure Active Directory Device Registration</span><span class="sxs-lookup"><span data-stu-id="6d973-117">Setting up Azure Active Directory Device Registration</span></span>

<span data-ttu-id="6d973-118">toosetup enhetsregistrering, har du flera alternativ:</span><span class="sxs-lookup"><span data-stu-id="6d973-118">toosetup device registration, you have multiple options:</span></span>

- <span data-ttu-id="6d973-119">Enheter kan registreras när kopplade tooAzure AD-domän.</span><span class="sxs-lookup"><span data-stu-id="6d973-119">Devices can register when joined tooAzure AD domain.</span></span> <span data-ttu-id="6d973-120">Mer information om det här avsnittet kan du läsa mer om Azure AD Join och hello-inställningar som krävs för användare toojoin Azure AD-domän.</span><span class="sxs-lookup"><span data-stu-id="6d973-120">For more on this topic, you can Learn more about Azure AD Join and hello settings required for users toojoin Azure AD domain.</span></span>

- <span data-ttu-id="6d973-121">Enheter kan registreras när användarna lägger till arbetet eller skolan konton tooWindows på en personlig enhet eller när mobila enheter ansluter tooa arbetsresurser kräver registrering.</span><span class="sxs-lookup"><span data-stu-id="6d973-121">Devices can be registered when users add work or school accounts tooWindows on a personal device or when mobile devices connect tooa work resources requiring registration.</span></span> <span data-ttu-id="6d973-122">tooensure, måste du aktivera Azure Active Directory Device Registration hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6d973-122">tooensure this, you must enable Azure AD Device Registration in hello Azure Portal.</span></span> 

- <span data-ttu-id="6d973-123">Enheter kan registreras med hjälp av automatisk enhetsregistrering för traditionella domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="6d973-123">Devices can be registered using automatic device registration for traditional domain-joined machines.</span></span> <span data-ttu-id="6d973-124">tooensure, måste du första installationen Azure AD Connect innan du fortsätter med automatisk enhetsregistrering.</span><span class="sxs-lookup"><span data-stu-id="6d973-124">tooensure this, you must first Setup Azure AD Connect before you continue with automatic device registration.</span></span>

<span data-ttu-id="6d973-125">Senaste instruktioner finns i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="6d973-125">For latest instructions, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>  
<span data-ttu-id="6d973-126">Du kan också granska och aktivera eller inaktivera registrerade enheter med hjälp av hello-Administratörsportalen i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="6d973-126">You can also review and enable or disable registered devices using hello Administrator Portal in Azure Active Directory.</span></span>

## <a name="enable-hello-azure-active-directory-device-registration-service"></a><span data-ttu-id="6d973-127">Aktivera hello enhetsregistreringstjänsten för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d973-127">Enable hello Azure Active Directory device registration service</span></span>

<span data-ttu-id="6d973-128">**tooenable hello enhetsregistreringstjänsten för Azure Active Directory:**</span><span class="sxs-lookup"><span data-stu-id="6d973-128">**tooenable hello Azure Active Directory device registration service:**</span></span>

1.  <span data-ttu-id="6d973-129">Logga in toohello Microsoft Azure-portalen som administratör.</span><span class="sxs-lookup"><span data-stu-id="6d973-129">Sign in toohello Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="6d973-130">Hello vänster, Välj **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6d973-130">On hello left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="6d973-131">På fliken för hello katalog väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="6d973-131">On hello Directory tab, select your directory.</span></span>

4.  <span data-ttu-id="6d973-132">Klicka på **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="6d973-132">Click **Configure**.</span></span>

5.  <span data-ttu-id="6d973-133">Rulla för**enheter**.</span><span class="sxs-lookup"><span data-stu-id="6d973-133">Scroll too**Devices**.</span></span>

6.  <span data-ttu-id="6d973-134">Markera alla användare kan registrera SINA enheter med AZURE AD.</span><span class="sxs-lookup"><span data-stu-id="6d973-134">Select ALL for USERS MAY REGISTER THEIR DEVICES WITH AZURE AD.</span></span>

7.  <span data-ttu-id="6d973-135">Välj hello maximalt antal enheter som du vill tooauthorize per användare.</span><span class="sxs-lookup"><span data-stu-id="6d973-135">Select hello maximum number of devices you want tooauthorize per user.</span></span>

<span data-ttu-id="6d973-136">hello registrering med Microsoft Intune eller Mobile Device Management för Office 365 kräver registrering av enheten.</span><span class="sxs-lookup"><span data-stu-id="6d973-136">hello enrollment with Microsoft Intune or Mobile Device Management for Office 365 requires device registration.</span></span> <span data-ttu-id="6d973-137">Om du har konfigurerat någon av dessa tjänster **alla** är markerad och **NONE** är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="6d973-137">If you have configured either of these services, **ALL** is selected and **NONE** is disabled.</span></span> <span data-ttu-id="6d973-138">Kontrollera att de är korrekt konfigurerade och har rätt hello-licensiering.</span><span class="sxs-lookup"><span data-stu-id="6d973-138">Please ensure that they are configured correctly and have hello appropriate licensing.</span></span>

<span data-ttu-id="6d973-139">Som standard aktiveras inte tvåfaktorsautentisering för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6d973-139">By default, two-factor authentication is not enabled for hello service.</span></span> <span data-ttu-id="6d973-140">Tvåfaktorsautentisering rekommenderas dock när du registrerar en enhet.</span><span class="sxs-lookup"><span data-stu-id="6d973-140">However, two-factor authentication is recommended when registering a device.</span></span>

- <span data-ttu-id="6d973-141">Innan du kräver tvåfaktorsautentisering för den här tjänsten, måste du konfigurera en provider för tvåfaktorsautentisering i Azure Active Directory och konfigurera dina användarkonton för multi-Factor Authentication går att lägga till Multi-Factor Authentication tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d973-141">Before requiring two-factor authentication for this service, you must configure a two-factor authentication provider in Azure Active Directory and configure your user accounts for Multi-Factor Authentication, see Adding Multi-Factor Authentication tooAzure Active Directory</span></span>

- <span data-ttu-id="6d973-142">Om du använder AD FS med Windows Server 2012 R2 måste du konfigurera en modul för tvåfaktorsautentisering i AD FS, finns i använda Multifaktorautentisering med Active Directory Federation Services.</span><span class="sxs-lookup"><span data-stu-id="6d973-142">If you are using AD FS with Windows Server 2012 R2, you must configure a two-factor authentication module in AD FS, see Using Multi-Factor Authentication with Active Directory Federation Services.</span></span>

## <a name="view-and-manage-device-objects-in-azure-active-directory"></a><span data-ttu-id="6d973-143">Visa och hantera enhetsobjekt i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6d973-143">View and manage device objects in Azure Active Directory</span></span>

<span data-ttu-id="6d973-144">Du kan visa, blockera och avblockera enheter från hello Azure-administratörsportalen.</span><span class="sxs-lookup"><span data-stu-id="6d973-144">From hello Azure Administrator portal, you can view, block, and unblock devices.</span></span> <span data-ttu-id="6d973-145">En enhet som är blockerad har inte längre åtkomst tooapplications som är konfigurerade tooallow endast registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="6d973-145">A device that is blocked will no longer have access tooapplications that are configured tooallow only registered devices.</span></span>

<span data-ttu-id="6d973-146">**tooview och hantera enhetsobjekt i Azure Active Directory:**</span><span class="sxs-lookup"><span data-stu-id="6d973-146">**tooview and manage device objects in Azure Active Directory:**</span></span>
 
1.  <span data-ttu-id="6d973-147">Logga in toohello Microsoft Azure-portalen som administratör.</span><span class="sxs-lookup"><span data-stu-id="6d973-147">Log on toohello Microsoft Azure portal as administrator.</span></span>

2.  <span data-ttu-id="6d973-148">Hello vänster, Välj **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="6d973-148">On hello left pane, select **Active Directory**.</span></span>

3.  <span data-ttu-id="6d973-149">Välj din katalog.</span><span class="sxs-lookup"><span data-stu-id="6d973-149">Select your directory.</span></span>

4.  <span data-ttu-id="6d973-150">Välj **användare**.</span><span class="sxs-lookup"><span data-stu-id="6d973-150">Select **Users**.</span></span> 

5.  <span data-ttu-id="6d973-151">Klicka på hello användare som du vill toosee hello enheter.</span><span class="sxs-lookup"><span data-stu-id="6d973-151">Click hello user for which you want toosee hello devices.</span></span>

6.  <span data-ttu-id="6d973-152">Välj **enheter**.</span><span class="sxs-lookup"><span data-stu-id="6d973-152">Select **Devices**.</span></span>

7.  <span data-ttu-id="6d973-153">Välj **registrerade enheter**.</span><span class="sxs-lookup"><span data-stu-id="6d973-153">Select **Registered Devices**.</span></span>

<span data-ttu-id="6d973-154">Nu kan du granska, blockera eller avblockera hello användarens registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="6d973-154">Now, you can review, block, or unblock hello user's registered devices.</span></span>
<span data-ttu-id="6d973-155">Windows 10-enheter som är lokalt ansluten till domänen och registreras automatiskt visas inte under fliken för hello-användare. Använd hello Get-MsolDevice PowerShell-kommandot toofind alla företagets enheter.</span><span class="sxs-lookup"><span data-stu-id="6d973-155">Windows 10 devices that are on-premises domain-joined and automatically registered do not appear under hello Users tab. Please use hello Get-MsolDevice PowerShell command toofind all your enterprise devices.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="6d973-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6d973-156">Next steps</span></span>

<span data-ttu-id="6d973-157">toosetup automatisk registrering av enheten, se [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="6d973-157">toosetup automated device registration, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>


