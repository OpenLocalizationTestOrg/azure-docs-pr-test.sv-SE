---
title: hantering av aaaIntroduction toodevice i Azure Active Directory | Microsoft Docs
description: "Lär dig hur enhetshantering kan du tooget kontroll över hello-enheter som har åtkomst till resurser i din miljö."
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
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: e2fc0a3e8d00dc69cf01db9074e34427e396cfcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toodevice-management-in-azure-active-directory"></a><span data-ttu-id="3e023-103">Introduktion toodevice management i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3e023-103">Introduction toodevice management in Azure Active Directory</span></span>

<span data-ttu-id="3e023-104">I mobile första, molnet först världen kan Azure Active Directory (Azure AD) toodevices för enkel inloggning, appar och tjänster från var som helst.</span><span class="sxs-lookup"><span data-stu-id="3e023-104">In a mobile-first, cloud-first world, Azure Active Directory (Azure AD) enables single sign-on toodevices, apps, and services from anywhere.</span></span> <span data-ttu-id="3e023-105">Med hello ökande mängden av enheter – inklusive Bring Your Own Device (BYOD), inför IT-proffs två andra mål:</span><span class="sxs-lookup"><span data-stu-id="3e023-105">With hello proliferation of devices - including Bring Your Own Device (BYOD), IT professionals are faced with two opposing goals:</span></span>

- <span data-ttu-id="3e023-106">Ge hello slutanvändare toobe produktiva var och när</span><span class="sxs-lookup"><span data-stu-id="3e023-106">Empower hello end users toobe productive wherever and whenever</span></span>
- <span data-ttu-id="3e023-107">Skydda hello företagets tillgångar när som helst</span><span class="sxs-lookup"><span data-stu-id="3e023-107">Protect hello corporate assets at any time</span></span>

<span data-ttu-id="3e023-108">Via enheter kan får användarna åtkomst tooyour företagets tillgångar.</span><span class="sxs-lookup"><span data-stu-id="3e023-108">Through devices, your users are getting access tooyour corporate assets.</span></span> <span data-ttu-id="3e023-109">tooprotect din företagets tillgångar som IT-administratör vill du toohave kontroll över enheterna.</span><span class="sxs-lookup"><span data-stu-id="3e023-109">tooprotect your corporate assets, as an IT administrator, you want toohave control over these devices.</span></span> <span data-ttu-id="3e023-110">Detta gör att du toomake till att dina användare har åtkomst till dina resurser från enheter som uppfyller dina krav för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="3e023-110">This enables you toomake sure that your users are accessing your resources from devices that meet your standards for security and compliance.</span></span> 

<span data-ttu-id="3e023-111">Hantering av enheter är också hello grunden för [enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="3e023-111">Device management is also hello foundation for [device-based conditional access](active-directory-conditional-access-policy-connected-applications.md).</span></span> <span data-ttu-id="3e023-112">Med enhetsbaserad villkorlig åtkomst kan du se till att åtkomst tooresources i din miljö är endast möjlig med betrodda enheter.</span><span class="sxs-lookup"><span data-stu-id="3e023-112">With device-based conditional access, you can ensure that access tooresources in your environment is only possible with trusted devices.</span></span>   

<span data-ttu-id="3e023-113">Det här avsnittet beskrivs hur enhetshantering i Azure Active Directory fungerar.</span><span class="sxs-lookup"><span data-stu-id="3e023-113">This topic explains how device management in Azure Active Directory works.</span></span>

## <a name="getting-devices-under-hello-control-of-azure-ad"></a><span data-ttu-id="3e023-114">Hantera enheter under hello kontroll av Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e023-114">Getting devices under hello control of Azure AD</span></span>

<span data-ttu-id="3e023-115">tooget en enhet under hello kontroll av Azure AD, har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="3e023-115">tooget a device under hello control of Azure AD, you have two options:</span></span>

- <span data-ttu-id="3e023-116">Registrera</span><span class="sxs-lookup"><span data-stu-id="3e023-116">Registering</span></span> 
- <span data-ttu-id="3e023-117">Koppla</span><span class="sxs-lookup"><span data-stu-id="3e023-117">Joining</span></span>

<span data-ttu-id="3e023-118">**Registrera** en enhet tooAzure AD kan du toomanage identitet för en enhet.</span><span class="sxs-lookup"><span data-stu-id="3e023-118">**Registering** a device tooAzure AD enables you toomanage a device’s identity.</span></span> <span data-ttu-id="3e023-119">När en enhet registreras, ger Azure AD-enhetsregistrering hello-enhet med en identitet som används tooauthenticate hello-enhet när en användare loggar in tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="3e023-119">When a device is registered, Azure AD device registration provides hello device with an identity that is used tooauthenticate hello device when a user signs-in tooAzure AD.</span></span> <span data-ttu-id="3e023-120">Du kan använda hello identitet tooenable eller inaktivera en enhet.</span><span class="sxs-lookup"><span data-stu-id="3e023-120">You can use hello identity tooenable or disable a device.</span></span>

<span data-ttu-id="3e023-121">När den kombineras med en enhetsattributen lösning för mobila enheter, till exempel Microsoft Intune, uppdateras hello attributen i Azure AD med ytterligare information om hello enhet.</span><span class="sxs-lookup"><span data-stu-id="3e023-121">When combined with a mobile device management(MDM) solution such as Microsoft Intune, hello device attributes in Azure AD are updated with additional information about hello device.</span></span> <span data-ttu-id="3e023-122">Detta ger dig toocreate regler för villkorlig åtkomst som säkerställer att åtkomsten från enheter toomeet dina krav för säkerhet och efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="3e023-122">This allows you toocreate conditional access rules that enforce access from devices toomeet your standards for security and compliance.</span></span> <span data-ttu-id="3e023-123">Mer information om registrerar enheter i Microsoft Intune finns i registrera enheter för hantering i Intune.</span><span class="sxs-lookup"><span data-stu-id="3e023-123">For more information on enrolling devices in Microsoft Intune, see Enroll devices for management in Intune .</span></span>

<span data-ttu-id="3e023-124">**Koppla** en enhet är ett tillägg tooregistering en enhet.</span><span class="sxs-lookup"><span data-stu-id="3e023-124">**Joining** a device is an extension tooregistering a device.</span></span> <span data-ttu-id="3e023-125">Det innebär den ger dig alla hello fördelar med att registrera en enhet och tillägg toothis, ändras även hello lokala tillståndet för en enhet.</span><span class="sxs-lookup"><span data-stu-id="3e023-125">This means, it provides you with all hello benefits of registering a device and in addition toothis, it also changes hello local state of a device.</span></span> <span data-ttu-id="3e023-126">Om du ändrar hello lokala tillstånd kan användare toosign i tooa enheten med ett organisations arbets- eller skolkonto i stället för ett personligt konto.</span><span class="sxs-lookup"><span data-stu-id="3e023-126">Changing hello local state enables your users toosign-in tooa device using an organizational work or school account instead of a personal account.</span></span>

## <a name="azure-ad-registered-devices"></a><span data-ttu-id="3e023-127">Azure AD som registrerade enheter</span><span class="sxs-lookup"><span data-stu-id="3e023-127">Azure AD registered devices</span></span>   

<span data-ttu-id="3e023-128">hello målet med Azure AD som registrerade enheter är du med stöd för hello tooprovide **Bring Your Own Device (BYOD)** scenario.</span><span class="sxs-lookup"><span data-stu-id="3e023-128">hello goal of Azure AD registered devices is tooprovide you with support for hello **Bring Your Own Device (BYOD)** scenario.</span></span> <span data-ttu-id="3e023-129">I det här scenariot kan en användare åtkomst till organisationens Azure Active Directory styrs resurser med hjälp av en personlig enhet.</span><span class="sxs-lookup"><span data-stu-id="3e023-129">In this scenario, a user can access your organization’s Azure Active Directory controlled resources using a personal device.</span></span>  

![Azure AD som registrerade enheter](./media/device-management-introduction/03.png)

<span data-ttu-id="3e023-131">hello åtkomst baserat på ett arbets- eller skolkonto konto som har angetts på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="3e023-131">hello access is based on a work or school account that has been entered on hello device.</span></span>  
<span data-ttu-id="3e023-132">Windows 10 kan till exempel användare tooadd ett arbets- eller Skol-konto tooa dator-, tablet- eller telefon.</span><span class="sxs-lookup"><span data-stu-id="3e023-132">For example, Windows 10 enables users tooadd a work or school account tooa personal computer, tablet, or phone.</span></span>  
<span data-ttu-id="3e023-133">När en användare har lagt till en arbets eller skolkonto, hello enhet är registrerad med Azure AD och du kan också registreras i hello mobila enheter (MDM) hanteringssystem som din organisation har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="3e023-133">When a user has added a work or school account, hello device is registered with Azure AD and optionally enrolled in hello mobile device management (MDM) system that your organization has configured.</span></span> <span data-ttu-id="3e023-134">Användare i organisationen kan lägga till ett arbetsplats eller skola konto tooa personlig enhet lätt:</span><span class="sxs-lookup"><span data-stu-id="3e023-134">Your organization’s users can add a work or school account tooa personal device conveniently:</span></span>

- <span data-ttu-id="3e023-135">Vid åtkomst till ett arbetsplats-program för hello första gången</span><span class="sxs-lookup"><span data-stu-id="3e023-135">When accessing a work application for hello first time</span></span>
- <span data-ttu-id="3e023-136">Manuellt via hello **inställningar** -menyn i hello skiftläget för Windows 10</span><span class="sxs-lookup"><span data-stu-id="3e023-136">Manually via hello **Settings** menu in hello case of Windows 10</span></span> 

<span data-ttu-id="3e023-137">Du kan konfigurera Azure AD som registrerade enheter för Windows 10, iOS, Android och macOS.</span><span class="sxs-lookup"><span data-stu-id="3e023-137">You can configure Azure AD registered devices for Windows 10, iOS, Android and macOS.</span></span>

## <a name="azure-ad-joined-devices"></a><span data-ttu-id="3e023-138">Azure AD-anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="3e023-138">Azure AD joined devices</span></span>

<span data-ttu-id="3e023-139">hello målet med Azure AD anslutna enheter är toosimplify:</span><span class="sxs-lookup"><span data-stu-id="3e023-139">hello goal of Azure AD joined devices is toosimplify:</span></span>

- <span data-ttu-id="3e023-140">Windows-distributioner av enheter som ägs av arbetet</span><span class="sxs-lookup"><span data-stu-id="3e023-140">Windows deployments of work-owned devices</span></span> 
- <span data-ttu-id="3e023-141">Åtkomst tooorganizational appar och resurser från valfri windowsenhet</span><span class="sxs-lookup"><span data-stu-id="3e023-141">Access tooorganizational apps and resources from any Windows device</span></span>

![Azure AD som registrerade enheter](./media/device-management-introduction/02.png)


<span data-ttu-id="3e023-143">Detta sker genom att erbjuda användarna en upplevelse som Självbetjäning för att hämta enheter som ägs av arbetet under hello kontroll av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e023-143">These goals are accomplished by providing your users with a self-service experience for getting work-owned devices under hello control of Azure AD.</span></span>  
<span data-ttu-id="3e023-144">**Azure AD-anslutning** är avsedd för organisationer som är moln första / endast molnbaserad.</span><span class="sxs-lookup"><span data-stu-id="3e023-144">**Azure AD Join** is intended for organizations that are cloud-first / cloud-only.</span></span> <span data-ttu-id="3e023-145">Detta är vanligtvis små och medelstora företag som inte har en lokal Windows Server Active Directory-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="3e023-145">These are typically small- and medium-sized businesses that do not have an on-premises Windows Server Active Directory infrastructure.</span></span> 

<span data-ttu-id="3e023-146">Implementera Azure AD anslutna enheter ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3e023-146">Implementing Azure AD joined devices provides you with hello following benefits:</span></span>

- <span data-ttu-id="3e023-147">**Enkel inloggning (SSO)** tooyour Azure hanteras SaaS-appar och tjänster.</span><span class="sxs-lookup"><span data-stu-id="3e023-147">**Single-Sign-On (SSO)** tooyour Azure managed SaaS apps and services.</span></span> <span data-ttu-id="3e023-148">Användarna ser anvisningarna för ytterligare autentisering vid åtkomst till arbetsresurser.</span><span class="sxs-lookup"><span data-stu-id="3e023-148">Your users don’t see additional authentication prompts when accessing work resources.</span></span> <span data-ttu-id="3e023-149">hello SSO funktioner är även när de inte är anslutna toohello domännätverk som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="3e023-149">hello SSO functionality is even when they are not connected toohello domain network available.</span></span>

- <span data-ttu-id="3e023-150">**Enterprise kompatibla centrala** av användarinställningar för domänanslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="3e023-150">**Enterprise compliant roaming** of user settings across joined devices.</span></span> <span data-ttu-id="3e023-151">Användarna behöver inte tooconnect ett Microsoft-konto (till exempel Hotmail) toosee inställningar över enheter.</span><span class="sxs-lookup"><span data-stu-id="3e023-151">Users don’t need tooconnect a Microsoft account (for example, Hotmail) toosee settings across devices.</span></span>

- <span data-ttu-id="3e023-152">**Åtkomst till tooWindows Store för företag** med hjälp av AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="3e023-152">**Access tooWindows Store for Business** using AD account.</span></span> <span data-ttu-id="3e023-153">Användarna kan välja från en inventering av program som är markerat av hello organisation.</span><span class="sxs-lookup"><span data-stu-id="3e023-153">Your users can choose from an inventory of applications pre-selected by hello organization.</span></span>

- <span data-ttu-id="3e023-154">**Windows Hello** stöd för säker och smidig åtkomst toowork resurser.</span><span class="sxs-lookup"><span data-stu-id="3e023-154">**Windows Hello** support for secure and convenient access toowork resources.</span></span>

- <span data-ttu-id="3e023-155">**Begränsning av åtkomst** tooapps från enheter som uppfyller policy för efterlevnad.</span><span class="sxs-lookup"><span data-stu-id="3e023-155">**Restriction of access** tooapps from only devices that meet compliance policy.</span></span>

<span data-ttu-id="3e023-156">Medan Azure AD-anslutning är främst avsedd för organisationer som inte har en lokal Windows Server Active Directory-infrastruktur, kan fungera utmärkt också använda den i scenarier där:</span><span class="sxs-lookup"><span data-stu-id="3e023-156">While Azure AD join is primarily intended for organizations that do not have an on-premises Windows Server Active Directory infrastructure, you can certainly also use it in scenarios where:</span></span>

- <span data-ttu-id="3e023-157">Du kan inte använda ett lokalt domänanslutning, till exempel om du behöver tooget mobila enheter som surfplattor och telefoner under kontroll.</span><span class="sxs-lookup"><span data-stu-id="3e023-157">You can’t use an on-premises domain join, for example, if you need tooget mobile devices such as tablets and phones under control.</span></span>

- <span data-ttu-id="3e023-158">Användarna behöver främst tooaccess Office 365 eller andra SaaS-appar som är integrerade med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3e023-158">Your users primarily need tooaccess Office 365 or other SaaS apps integrated with Azure AD.</span></span>

- <span data-ttu-id="3e023-159">Vill du toomanage en grupp användare i Azure AD i stället för i Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3e023-159">You want toomanage a group of users in Azure AD instead of in Active Directory.</span></span> <span data-ttu-id="3e023-160">Detta kan till exempel gälla tooseasonal anställda, leverantörer eller studenter.</span><span class="sxs-lookup"><span data-stu-id="3e023-160">This can apply, for example, tooseasonal workers, contractors, or students.</span></span>

- <span data-ttu-id="3e023-161">Vill du tooprovide anslutande funktioner tooworkers i fjärranslutna avdelningskontor med begränsad lokal infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="3e023-161">You want tooprovide joining capabilities tooworkers in remote branch offices with limited on-premises infrastructure.</span></span>

<span data-ttu-id="3e023-162">Du kan konfigurera Azure AD anslutna enheter för Windows 10-enheter.</span><span class="sxs-lookup"><span data-stu-id="3e023-162">You can configure Azure AD joined devices for Windows 10 devices.</span></span>


## <a name="hybrid-azure-ad-joined-devices"></a><span data-ttu-id="3e023-163">Hybrid Azure AD-anslutna enheter</span><span class="sxs-lookup"><span data-stu-id="3e023-163">Hybrid Azure AD joined devices</span></span>

<span data-ttu-id="3e023-164">För mer än en tio åren har har många organisationer använt hello domän koppling tootheir lokala Active Directory tooenable:</span><span class="sxs-lookup"><span data-stu-id="3e023-164">For more than a decade, many organizations have used hello domain join tootheir on-premises Active Directory tooenable:</span></span>

- <span data-ttu-id="3e023-165">IT-avdelningar toomanage arbete ägda enheter från en central plats.</span><span class="sxs-lookup"><span data-stu-id="3e023-165">IT departments toomanage work-owned devices from a central location.</span></span>

- <span data-ttu-id="3e023-166">Användare toosign i tootheir enheter med deras Active Directory arbets- eller skolkonton.</span><span class="sxs-lookup"><span data-stu-id="3e023-166">Users toosign in tootheir devices with their Active Directory work or school accounts.</span></span> 

<span data-ttu-id="3e023-167">Normalt organisationer med ett lokalt storleken är beroende av imaging metoder tooprovision enheter och de använder ofta **System Center Configuration Manager (SCCM)** eller **Grupprincip (GP)** toomanage dem.</span><span class="sxs-lookup"><span data-stu-id="3e023-167">Typically, organizations with an on-premises footprint rely on imaging methods tooprovision devices, and they often use **System Center Configuration Manager (SCCM)** or **group policy (GP)** toomanage them.</span></span>

<span data-ttu-id="3e023-168">Om din miljö har en lokal AD storleken och du även vill utnyttja hello funktionerna i Azure Active Directory, du kan implementera hybrid Azure AD anslutna enheter.</span><span class="sxs-lookup"><span data-stu-id="3e023-168">If your environment has an on-premises AD footprint and you also want benefit from hello capabilities provided by Azure Active Directory, you can implement hybrid Azure AD joined devices.</span></span> <span data-ttu-id="3e023-169">Dessa är enheter som är både, domänanslutna tooyour lokala Active Directory och Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3e023-169">These are devices that are both, joined tooyour on-premises Active Directory and your Azure Active Directory.</span></span>

![Azure AD som registrerade enheter](./media/device-management-introduction/01.png)


<span data-ttu-id="3e023-171">Du bör använda Azure AD hybrid anslutna enheter om:</span><span class="sxs-lookup"><span data-stu-id="3e023-171">You should use Azure AD hybrid joined devices if:</span></span>

- <span data-ttu-id="3e023-172">Win32-appar distribuerade toothese enheter som använder NTLM / Kerberos.</span><span class="sxs-lookup"><span data-stu-id="3e023-172">You have Win32 apps deployed toothese devices that use NTLM / Kerberos.</span></span>

- <span data-ttu-id="3e023-173">Du behöver GP eller SCCM / DCM toomanage enheter.</span><span class="sxs-lookup"><span data-stu-id="3e023-173">You require GP or SCCM / DCM toomanage devices.</span></span>

- <span data-ttu-id="3e023-174">Du vill toocontinue toouse avbildning lösningar tooconfigure enheter för dina anställda.</span><span class="sxs-lookup"><span data-stu-id="3e023-174">You want toocontinue toouse imaging solutions tooconfigure devices for your employees.</span></span>

<span data-ttu-id="3e023-175">Du kan konfigurera Hybrid Azure AD-anslutna enheter för Windows 10 och äldre enheter, till exempel Windows 8 och Windows 7.</span><span class="sxs-lookup"><span data-stu-id="3e023-175">You can configure Hybrid Azure AD joined devices for Windows 10 and down-level devices such as Windows 8 and Windows 7.</span></span>

## <a name="summary"></a><span data-ttu-id="3e023-176">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3e023-176">Summary</span></span>

<span data-ttu-id="3e023-177">Med hantering av enheter i Azure AD kan du:</span><span class="sxs-lookup"><span data-stu-id="3e023-177">With device management in Azure AD, you can:</span></span> 

- <span data-ttu-id="3e023-178">Förenkla hello processen för att göra enheter under hello kontroll av Azure AD</span><span class="sxs-lookup"><span data-stu-id="3e023-178">Simplify hello process of bringing devices under hello control of Azure AD</span></span>

- <span data-ttu-id="3e023-179">Ge användarna en enkel toouse åtkomst tooyour organisations molnbaserade resurser</span><span class="sxs-lookup"><span data-stu-id="3e023-179">Provide your users with an easy toouse access tooyour organization’s cloud-based resources</span></span>

<span data-ttu-id="3e023-180">Som regel i en USB bör du använda:</span><span class="sxs-lookup"><span data-stu-id="3e023-180">As a rule of a thumb, you should use:</span></span>

- <span data-ttu-id="3e023-181">Azure AD som registrerade enheter för personliga enheter</span><span class="sxs-lookup"><span data-stu-id="3e023-181">Azure AD registered devices for personal devices</span></span>

- <span data-ttu-id="3e023-182">Azure AD-anslutna enheter för enheter som inte är anslutna tooan lokala AD</span><span class="sxs-lookup"><span data-stu-id="3e023-182">Azure AD joined devices for devices that are not joined tooan on-premises AD</span></span> 

- <span data-ttu-id="3e023-183">Hybrid Azure AD-anslutna enheter för enheter som är anslutna tooan lokala AD</span><span class="sxs-lookup"><span data-stu-id="3e023-183">Hybrid Azure AD joined devices for devices that are joined tooan on-premises AD</span></span>     




## <a name="next-steps"></a><span data-ttu-id="3e023-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e023-184">Next steps</span></span>

- <span data-ttu-id="3e023-185">tooget en översikt över hur hello toomanage enhet i Azure portal, se [hantera enheter med hjälp av hello Azure-portalen](device-management-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="3e023-185">tooget an overview of how toomanage device in hello Azure portal, see [managing devices using hello Azure portal](device-management-azure-portal.md)</span></span>

- <span data-ttu-id="3e023-186">toolearn mer om enhetsbaserad villkorlig åtkomst finns [konfigurera Azure Active Directory enhetsbaserad villkorliga åtkomstprinciper](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="3e023-186">toolearn more about device-based conditional access, see [configure Azure Active Directory device-based conditional access policies](active-directory-conditional-access-policy-connected-applications.md).</span></span>

- <span data-ttu-id="3e023-187">toosetup hybrid Azure AD anslutna enheter, se [hur tooconfigure hybrid Azure Active Directory-anslutna enheter](device-management-hybrid-azuread-joined-devices-setup.md).</span><span class="sxs-lookup"><span data-stu-id="3e023-187">toosetup hybrid Azure AD joined devices, see [how tooconfigure hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md).</span></span>


