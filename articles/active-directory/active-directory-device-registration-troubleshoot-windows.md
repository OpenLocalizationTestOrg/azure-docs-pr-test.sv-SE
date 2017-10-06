---
title: "aaaTroubleshooting hello automatisk registrering av Azure AD-domän domänanslutna datorer för Windows 10 och Windows Server 2016 | Microsoft Docs"
description: "Felsöka hello automatisk registrering av Azure AD-domän domänanslutna datorer för Windows 10 och Windows Server 2016."
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
ms.date: 06/23/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3795323ce9392368b412b3e1208868431e59a74b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad--windows-10-and-windows-server-2016"></a><span data-ttu-id="19208-103">Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="19208-103">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>

<span data-ttu-id="19208-104">Det här avsnittet är tillämpliga toohello följande klienter:</span><span class="sxs-lookup"><span data-stu-id="19208-104">This topic is applicable toohello following clients:</span></span>

-   <span data-ttu-id="19208-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="19208-105">Windows 10</span></span>
-   <span data-ttu-id="19208-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="19208-106">Windows Server 2016</span></span>

<span data-ttu-id="19208-107">Andra windowsklienter finns i [felsökning automatisk registrering av domän domänanslutna datorer tooAzure AD för Windows äldre klienter](active-directory-device-registration-troubleshoot-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="19208-107">For other Windows clients, see [Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients](active-directory-device-registration-troubleshoot-windows-legacy.md).</span></span>

<span data-ttu-id="19208-108">Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md) toosupport hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="19208-108">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md) toosupport hello following scenarios:</span></span>

- [<span data-ttu-id="19208-109">Enhetsbaserad villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="19208-109">Device-based conditional access</span></span>](active-directory-conditional-access-automatic-device-registration-setup.md)

- [<span data-ttu-id="19208-110">Enterprise centrala inställningar</span><span class="sxs-lookup"><span data-stu-id="19208-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="19208-111">Windows Hello för företag</span><span class="sxs-lookup"><span data-stu-id="19208-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="19208-112">Det här dokumentet innehåller felsökningsinformation för hur tooresolve potentiella problem.</span><span class="sxs-lookup"><span data-stu-id="19208-112">This document provides troubleshooting guidance on how tooresolve potential issues.</span></span> 

<span data-ttu-id="19208-113">hello registrering stöds i Windows hello uppdatering 10 November 2015 och högre.</span><span class="sxs-lookup"><span data-stu-id="19208-113">hello registration is supported in hello Windows 10 November 2015 Update and above.</span></span>  
<span data-ttu-id="19208-114">Vi rekommenderar hello årsdagar Update för att aktivera hello scenarierna ovan.</span><span class="sxs-lookup"><span data-stu-id="19208-114">We recommend using hello Anniversary Update for enabling hello scenarios above.</span></span>

## <a name="step-1-retrieve-hello-registration-status"></a><span data-ttu-id="19208-115">Steg 1: Hämta hello registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="19208-115">Step 1: Retrieve hello registration status</span></span> 

<span data-ttu-id="19208-116">**tooretrieve hello registreringsstatus:**</span><span class="sxs-lookup"><span data-stu-id="19208-116">**tooretrieve hello registration status:**</span></span>

1. <span data-ttu-id="19208-117">Öppna hello kommandotolk som administratör.</span><span class="sxs-lookup"><span data-stu-id="19208-117">Open hello command prompt as an administrator.</span></span>

2. <span data-ttu-id="19208-118">Typen **dsregcmd/status**</span><span class="sxs-lookup"><span data-stu-id="19208-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="19208-119">+----------------------------------------------------------------------+
   | Enhetens tillstånd |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="19208-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined : YES
     <span data-ttu-id="19208-120">EnterpriseJoined: Inga DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 tumavtryck: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft-plattformen krypteringsprovider TpmProtected: Ja KeySignTest:: måste köras utökade tootest.</span><span class="sxs-lookup"><span data-stu-id="19208-120">EnterpriseJoined : NO DeviceId : 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint : B753A6679CE720451921302CA873794D94C6204A KeyContainerId : bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider : Microsoft Platform Crypto Provider TpmProtected : YES KeySignTest: : MUST Run elevated tootest.</span></span>
                  <span data-ttu-id="19208-121">IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ja DomainName: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="19208-121">Idp : login.windows.net TenantId : 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName : Contoso AuthCodeUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl : eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion : 1.0 JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion : 1.0 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId : urn:ms-drs:enterpriseregistration.windows.net DomainJoined : YES DomainName : CONTOSO</span></span>
    
    <span data-ttu-id="19208-122">+----------------------------------------------------------------------+
   | Användartillstånd |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="19208-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    <span data-ttu-id="19208-123">WamDefaultAuthority: organisationer WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Ja</span><span class="sxs-lookup"><span data-stu-id="19208-123">WamDefaultAuthority : organizations         WamDefaultId : https://login.microsoft.com       WamDefaultGUID : {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt : YES</span></span>



## <a name="step-2-evaluate-hello-registration-status"></a><span data-ttu-id="19208-124">Steg 2: Utvärdera hello registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="19208-124">Step 2: Evaluate hello registration status</span></span> 

<span data-ttu-id="19208-125">Granska hello följande fält och se till att de har hello förväntade värden:</span><span class="sxs-lookup"><span data-stu-id="19208-125">Review hello following fields and make sure that they have hello expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="19208-126">AzureAdJoined: Ja</span><span class="sxs-lookup"><span data-stu-id="19208-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="19208-127">Det här fältet visar om hello enheten är registrerad med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="19208-127">This field shows whether hello device is registered with Azure AD.</span></span> <span data-ttu-id="19208-128">Om hello värde visas som ”Nej”, har registrering inte slutförts.</span><span class="sxs-lookup"><span data-stu-id="19208-128">If hello value shows as ‘NO’, registration has not completed.</span></span> 

<span data-ttu-id="19208-129">**Möjliga orsaker:**</span><span class="sxs-lookup"><span data-stu-id="19208-129">**Possible causes:**</span></span>

- <span data-ttu-id="19208-130">Autentisering för hello dator registreringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="19208-130">Authentication of hello computer for registration failed.</span></span>

- <span data-ttu-id="19208-131">Det finns en HTTP-proxy i hello organisation som inte identifieras av hello-dator</span><span class="sxs-lookup"><span data-stu-id="19208-131">There is an HTTP proxy in hello organization that cannot be discovered by hello computer</span></span>

- <span data-ttu-id="19208-132">hello datorn går inte att nå Azure AD för autentisering eller Azure DRS för registrering</span><span class="sxs-lookup"><span data-stu-id="19208-132">hello computer cannot reach Azure AD for authentication or Azure DRS for registration</span></span>

- <span data-ttu-id="19208-133">hello datorn kanske inte på hello företags interna nätverk eller VPN-anslutning med fri tooan lokala AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="19208-133">hello computer may not be on hello organization’s internal network or on VPN with direct line of sight tooan on-premises AD domain controller.</span></span>

- <span data-ttu-id="19208-134">Om hello dator har TPM, kanske den i felaktigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="19208-134">If hello computer has a TPM, it may be in a bad state.</span></span>

- <span data-ttu-id="19208-135">Det kan finnas en felaktig konfiguration i tjänster antecknade i hello dokumentet tidigare att du måste tooverify igen.</span><span class="sxs-lookup"><span data-stu-id="19208-135">There may be a misconfiguration in services noted in hello document earlier that you will need tooverify again.</span></span> <span data-ttu-id="19208-136">Vanliga exempel är:</span><span class="sxs-lookup"><span data-stu-id="19208-136">Common examples are:</span></span>

    - <span data-ttu-id="19208-137">Federationsservern har inte aktiverats för WS-Trust-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="19208-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="19208-138">Federationsservern får inte tillåta inkommande autentisering från datorer i nätverket med hjälp av integrerad Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="19208-138">Your federation server may not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="19208-139">Det finns inget tjänstanslutningspunkten-objekt som pekar tooyour verifierat domännamn i Azure AD i hello AD-skog där hello datorn tillhör</span><span class="sxs-lookup"><span data-stu-id="19208-139">There is no Service Connection Point object that points tooyour verified domain name in Azure AD in hello AD forest where hello computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="19208-140">DomainJoined: Ja</span><span class="sxs-lookup"><span data-stu-id="19208-140">DomainJoined : YES</span></span>  

<span data-ttu-id="19208-141">Det här fältet visar om hello enheten är domänansluten tooan lokala Active Directory eller inte.</span><span class="sxs-lookup"><span data-stu-id="19208-141">This field shows whether hello device is joined tooan on-premises Active Directory or not.</span></span> <span data-ttu-id="19208-142">Om hello värde visas som **nr**, hello enheten kan inte automatiskt-register med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="19208-142">If hello value shows as **NO**, hello device cannot auto-register with Azure AD.</span></span> <span data-ttu-id="19208-143">Kontrollera först att hello enheten ansluter till toohello lokala Active Directory innan den kan registreras med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="19208-143">Check first that hello device joins toohello on-premises Active Directory before it can register with Azure AD.</span></span> <span data-ttu-id="19208-144">Om du behöver för att ansluta till hello datorn tooAzure AD direkt gå tooLearn om funktioner i Azure Active Directory Join.</span><span class="sxs-lookup"><span data-stu-id="19208-144">If you are looking for joining hello computer tooAzure AD directly, please go tooLearn about capabilities of Azure Active Directory Join.</span></span>

---

### <a name="workplacejoined--no"></a><span data-ttu-id="19208-145">WorkplaceJoined: Nej</span><span class="sxs-lookup"><span data-stu-id="19208-145">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="19208-146">Det här fältet visar hello enheten är registrerad med Azure AD, men som en personlig enhet (markerade som ”Arbetsplatsanslutna”).</span><span class="sxs-lookup"><span data-stu-id="19208-146">This field shows whether hello device is registered with Azure AD but as a personal device (marked as ‘Workplace Joined’).</span></span> <span data-ttu-id="19208-147">Om det här värdet ska visas som ”Nej” för en domänansluten dator registrerad i Azure AD, men om det visas på Ja innebär det att en arbets- eller skolkonto-kontot var tillagda tidigare toohello Slutför registreringen av datorn.</span><span class="sxs-lookup"><span data-stu-id="19208-147">If this value should show as ‘NO’ for a domain joined computer registered with Azure AD, however if it shows as YES it means that a work or school account was added prior toohello computer completing registration.</span></span> <span data-ttu-id="19208-148">I det här fallet ignoreras hello konto om du använder hello årsdagar uppdaterad version av Windows 10 (1607 när kör hello WinVer kommando i hello 'Körs' fönster eller Kommandotolken).</span><span class="sxs-lookup"><span data-stu-id="19208-148">In this case hello account will be ignored if using hello Anniversary Update version of Windows 10 (1607 when running hello WinVer command in hello ‘Run’ window or a command prompt window).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="19208-149">WamDefaultSet: Ja och AzureADPrt: Ja</span><span class="sxs-lookup"><span data-stu-id="19208-149">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="19208-150">Dessa fält visar hello användaren har autentiserats tooAzure AD vid inloggning toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="19208-150">These fields show that hello user has successfully authenticated tooAzure AD upon signing in toohello device.</span></span> <span data-ttu-id="19208-151">Om de visar är 'Nej' hello följande möjliga orsaker:</span><span class="sxs-lookup"><span data-stu-id="19208-151">If they show ‘NO’ hello following are possible causes:</span></span>

- <span data-ttu-id="19208-152">Felaktig lagring nyckeln (STK) i TPM som är associerade med hello enheten vid registreringen (kontrollera hello KeySignTest när du kör upphöjd).</span><span class="sxs-lookup"><span data-stu-id="19208-152">Bad storage key (STK) in TPM associated with hello device upon registration (check hello KeySignTest while running elevated).</span></span>

- <span data-ttu-id="19208-153">Alternativt inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="19208-153">Alternate Login ID</span></span>

- <span data-ttu-id="19208-154">Det gick inte att hitta HTTP-Proxy</span><span class="sxs-lookup"><span data-stu-id="19208-154">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="19208-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="19208-155">Next steps</span></span>

<span data-ttu-id="19208-156">Mer information finns i hello [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="19208-156">For more information, see hello [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 
