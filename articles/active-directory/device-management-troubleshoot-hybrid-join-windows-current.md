---
title: "aaaTroubleshooting hybrid Azure Active Directory-anslutna enheter för Windows 10 och Windows Server 2016 | Microsoft Docs"
description: "Felsökning av hybrid Azure Active Directory-anslutna enheter för Windows 10 och Windows Server 2016."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: cc252d1d0684d6632694afc8a367327794228c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-windows-10-and-windows-server-2016-devices"></a><span data-ttu-id="10612-103">Felsökning av hybrid Azure Active Directory-anslutna enheter för Windows 10 och Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="10612-103">Troubleshooting hybrid Azure Active Directory joined Windows 10 and Windows Server 2016 devices</span></span> 

<span data-ttu-id="10612-104">Det här avsnittet är tillämpliga toohello följande klienter:</span><span class="sxs-lookup"><span data-stu-id="10612-104">This topic is applicable toohello following clients:</span></span>

-   <span data-ttu-id="10612-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="10612-105">Windows 10</span></span>
-   <span data-ttu-id="10612-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="10612-106">Windows Server 2016</span></span>

<span data-ttu-id="10612-107">Andra windowsklienter finns i [felsökning hybrid Azure Active Directory-anslutna enheter för äldre](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="10612-107">For other Windows clients, see [Troubleshooting hybrid Azure Active Directory joined down-level devices](device-management-troubleshoot-hybrid-join-windows-legacy.md).</span></span>

<span data-ttu-id="10612-108">Det här avsnittet förutsätter att du har [konfigurerade hybrid Azure Active Directory-anslutna enheter](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="10612-108">This topic assumes that you have [configured hybrid Azure Active Directory joined devices](device-management-hybrid-azuread-joined-devices-setup.md) toosupport hello following scenarios:</span></span>

- <span data-ttu-id="10612-109">Enhetsbaserad villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="10612-109">Device-based conditional access</span></span>

- [<span data-ttu-id="10612-110">Enterprise centrala inställningar</span><span class="sxs-lookup"><span data-stu-id="10612-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="10612-111">Windows Hello för företag</span><span class="sxs-lookup"><span data-stu-id="10612-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="10612-112">Det här dokumentet innehåller felsökningsinformation för hur tooresolve potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="10612-112">This document provides troubleshooting guidance on how tooresolve potential issues.</span></span> 


<span data-ttu-id="10612-113">För Windows 10 och Windows Server 2016 hybrid Azure Active Directory join stöder hello Windows 10 November 2015 Update och högre.</span><span class="sxs-lookup"><span data-stu-id="10612-113">For Windows 10 and Windows Server 2016, hybrid Azure Active Directory join supports hello Windows 10 November 2015 Update and above.</span></span> <span data-ttu-id="10612-114">Vi rekommenderar att du använder hello årsdagar uppdateringen.</span><span class="sxs-lookup"><span data-stu-id="10612-114">We recommend using hello Anniversary update.</span></span>

## <a name="step-1-retrieve-hello-join-status"></a><span data-ttu-id="10612-115">Steg 1: Hämta hello koppling status</span><span class="sxs-lookup"><span data-stu-id="10612-115">Step 1: Retrieve hello join status</span></span> 

<span data-ttu-id="10612-116">**status för tooretrieve hello koppling:**</span><span class="sxs-lookup"><span data-stu-id="10612-116">**tooretrieve hello join status:**</span></span>

1. <span data-ttu-id="10612-117">Öppna hello Kommandotolken som administratör</span><span class="sxs-lookup"><span data-stu-id="10612-117">Open hello command prompt as an administrator</span></span>

2. <span data-ttu-id="10612-118">Typen **dsregcmd/status**</span><span class="sxs-lookup"><span data-stu-id="10612-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="10612-119">+----------------------------------------------------------------------+
   | Enhetens tillstånd |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="10612-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined: YES
     <span data-ttu-id="10612-120">EnterpriseJoined: Inga DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 tumavtryck: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft-plattformen krypteringsprovider TpmProtected: Ja KeySignTest:: måste köras utökade tootest.</span><span class="sxs-lookup"><span data-stu-id="10612-120">EnterpriseJoined: NO DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft Platform Crypto Provider TpmProtected: YES KeySignTest: : MUST Run elevated tootest.</span></span>
                  <span data-ttu-id="10612-121">IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/ msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https:// Portal.Manage-Beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https:// enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs: enterpriseregistration.Windows.NET DomainJoined: Ja DomainName: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="10612-121">Idp: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn:ms-drs:enterpriseregistration.windows.net DomainJoined: YES DomainName: CONTOSO</span></span>
    
    <span data-ttu-id="10612-122">+----------------------------------------------------------------------+
   | Användartillstånd |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="10612-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet: YES
               NgcKeyId: {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined: NO
          WamDefaultSet: YES
    <span data-ttu-id="10612-123">WamDefaultAuthority: organisationer WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Ja</span><span class="sxs-lookup"><span data-stu-id="10612-123">WamDefaultAuthority: organizations         WamDefaultId: https://login.microsoft.com       WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt: YES</span></span>



## <a name="step-2-evaluate-hello-join-status"></a><span data-ttu-id="10612-124">Steg 2: Utvärdera hello koppling status</span><span class="sxs-lookup"><span data-stu-id="10612-124">Step 2: Evaluate hello join status</span></span> 

<span data-ttu-id="10612-125">Granska hello följande fält och se till att de har hello förväntade värden:</span><span class="sxs-lookup"><span data-stu-id="10612-125">Review hello following fields and make sure that they have hello expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="10612-126">AzureAdJoined: Ja</span><span class="sxs-lookup"><span data-stu-id="10612-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="10612-127">Det här fältet anger om hello enhet är ansluten med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="10612-127">This field indicates whether hello device is joined with Azure AD.</span></span> <span data-ttu-id="10612-128">Om värdet för hello är **nr**, hello koppling tooAzure AD inte har slutförts ännu.</span><span class="sxs-lookup"><span data-stu-id="10612-128">If hello value is **NO**, hello join tooAzure AD has not completed yet.</span></span> 

<span data-ttu-id="10612-129">**Möjliga orsaker:**</span><span class="sxs-lookup"><span data-stu-id="10612-129">**Possible causes:**</span></span>

- <span data-ttu-id="10612-130">Autentisering för en koppling hello dator misslyckades.</span><span class="sxs-lookup"><span data-stu-id="10612-130">Authentication of hello computer for a join failed.</span></span>

- <span data-ttu-id="10612-131">Det finns en HTTP-proxy i hello organisation som inte identifieras av hello-dator</span><span class="sxs-lookup"><span data-stu-id="10612-131">There is an HTTP proxy in hello organization that cannot be discovered by hello computer</span></span>

- <span data-ttu-id="10612-132">hello datorn inte kan nå tooauthenticate Azure AD eller Azure DRS för registrering</span><span class="sxs-lookup"><span data-stu-id="10612-132">hello computer cannot reach Azure AD tooauthenticate or Azure DRS for registration</span></span>

- <span data-ttu-id="10612-133">hello datorn kanske inte på hello företags interna nätverk eller VPN-anslutning med fri tooan lokala AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="10612-133">hello computer may not be on hello organization’s internal network or on VPN with direct line of sight tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="10612-134">Om hello dator har TPM, kan det vara i ett felaktigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="10612-134">If hello computer has a TPM, it can be in a bad state.</span></span>

- <span data-ttu-id="10612-135">Det kan finnas en felaktig konfiguration i hello services antecknade i hello dokumentet tidigare att du måste tooverify igen.</span><span class="sxs-lookup"><span data-stu-id="10612-135">There might be a misconfiguration in hello services noted in hello document earlier that you will need tooverify again.</span></span> <span data-ttu-id="10612-136">Vanliga exempel är:</span><span class="sxs-lookup"><span data-stu-id="10612-136">Common examples are:</span></span>

    - <span data-ttu-id="10612-137">Federationsservern har inte aktiverats för WS-Trust-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="10612-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="10612-138">Federationsservern tillåter inte inkommande autentisering från datorer i nätverket med hjälp av integrerad Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="10612-138">Your federation server does not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="10612-139">Det finns inget tjänstanslutningspunkten-objekt som pekar tooyour verifierat domännamn i Azure AD i hello AD-skog där hello datorn tillhör</span><span class="sxs-lookup"><span data-stu-id="10612-139">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="10612-140">DomainJoined: Ja</span><span class="sxs-lookup"><span data-stu-id="10612-140">DomainJoined : YES</span></span>  

<span data-ttu-id="10612-141">Det här fältet anger om hello enhet är ansluten tooan lokala Active Directory eller inte.</span><span class="sxs-lookup"><span data-stu-id="10612-141">This field indicates whether hello device is joined tooan on-premises Active Directory or not.</span></span> <span data-ttu-id="10612-142">Om värdet för hello är **nr**, hello enheten kan inte utföra en hybrid Azure AD-koppling.</span><span class="sxs-lookup"><span data-stu-id="10612-142">If hello value is **NO**, hello device cannot perform a hybrid Azure AD join.</span></span>  

---

### <a name="workplacejoined--no"></a><span data-ttu-id="10612-143">WorkplaceJoined: Nej</span><span class="sxs-lookup"><span data-stu-id="10612-143">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="10612-144">Det här fältet anger om hello enheten är registrerad med Azure AD som en personlig enhet (markerad som *Arbetsplatsanslutna*).</span><span class="sxs-lookup"><span data-stu-id="10612-144">This field indicates whether hello device is registered with Azure AD as a personal device (marked as *Workplace Joined*).</span></span> <span data-ttu-id="10612-145">Det här värdet ska vara **nr** för en domänansluten dator som är också ansluten hybrid Azure AD.</span><span class="sxs-lookup"><span data-stu-id="10612-145">This value should be **NO** for a domain-joined computer that is also hybrid Azure AD joined.</span></span> <span data-ttu-id="10612-146">Om värdet för hello är **Ja**, ett arbets- eller skolkonto konto har lagts till tidigare toohello slutförande av hello Azure AD-hybridlösning koppling.</span><span class="sxs-lookup"><span data-stu-id="10612-146">If hello value is **YES**, a work or school account was added prior toohello completion of hello hybrid Azure AD join.</span></span> <span data-ttu-id="10612-147">I det här fallet ignoreras hello konto när du använder hello årsdagar uppdaterad version av Windows 10 (1607).</span><span class="sxs-lookup"><span data-stu-id="10612-147">In this case, hello account is ignored when using hello Anniversary Update version of Windows 10 (1607).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="10612-148">WamDefaultSet: Ja och AzureADPrt: Ja</span><span class="sxs-lookup"><span data-stu-id="10612-148">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="10612-149">De här fälten indikera om hello användaren har autentiserats tooAzure AD när du loggar in toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="10612-149">These fields indicate whether hello user has successfully authenticated tooAzure AD when signing in toohello device.</span></span> <span data-ttu-id="10612-150">Om hello värdena är **nr**, kan det bero på grund av:</span><span class="sxs-lookup"><span data-stu-id="10612-150">If hello values are **NO**, it could be due:</span></span>

- <span data-ttu-id="10612-151">Felaktig lagring nyckeln (STK) i TPM som är associerade med hello enheten vid registreringen (kontrollera hello KeySignTest när du kör upphöjd).</span><span class="sxs-lookup"><span data-stu-id="10612-151">Bad storage key (STK) in TPM associated with hello device upon registration (check hello KeySignTest while running elevated).</span></span>

- <span data-ttu-id="10612-152">Alternativt inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="10612-152">Alternate Login ID</span></span>

- <span data-ttu-id="10612-153">Det gick inte att hitta HTTP-Proxy</span><span class="sxs-lookup"><span data-stu-id="10612-153">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="10612-154">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10612-154">Next steps</span></span>

<span data-ttu-id="10612-155">Frågor, finns hello [enhetshantering vanliga frågor och svar](device-management-faq.md)</span><span class="sxs-lookup"><span data-stu-id="10612-155">For questions, see hello [device management FAQ](device-management-faq.md)</span></span> 
