---
title: "Felsökning av automatisk registrering av Azure AD-domän domänanslutna datorer för Windows 10 och Windows Server 2016 | Microsoft Docs"
description: "Felsökning av automatisk registrering av Azure AD-domän domänanslutna datorer för Windows 10 och Windows Server 2016."
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
ms.openlocfilehash: 5b7f95f302f716d9221b5fae59aa2df5c956a524
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad--windows-10-and-windows-server-2016"></a><span data-ttu-id="a8a7e-103">Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD – Windows 10 och Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="a8a7e-103">Troubleshooting auto-registration of domain joined computers to Azure AD – Windows 10 and Windows Server 2016</span></span>

<span data-ttu-id="a8a7e-104">Det här avsnittet gäller följande klienter:</span><span class="sxs-lookup"><span data-stu-id="a8a7e-104">This topic is applicable to the following clients:</span></span>

-   <span data-ttu-id="a8a7e-105">Windows 10</span><span class="sxs-lookup"><span data-stu-id="a8a7e-105">Windows 10</span></span>
-   <span data-ttu-id="a8a7e-106">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="a8a7e-106">Windows Server 2016</span></span>

<span data-ttu-id="a8a7e-107">Andra windowsklienter finns i [felsökning automatisk registrering av domän domänanslutna datorer till Azure AD för Windows-klientversioner klienter](active-directory-device-registration-troubleshoot-windows-legacy.md).</span><span class="sxs-lookup"><span data-stu-id="a8a7e-107">For other Windows clients, see [Troubleshooting auto-registration of domain joined computers to Azure AD for Windows down-level clients](active-directory-device-registration-troubleshoot-windows-legacy.md).</span></span>

<span data-ttu-id="a8a7e-108">Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur du konfigurerar automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md) till stöd för följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="a8a7e-108">This topic assumes that you have configured auto-registration of domain-joined devices as outlined in described in [How to configure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-device-registration-get-started.md) to support the following scenarios:</span></span>

- [<span data-ttu-id="a8a7e-109">Enhetsbaserad villkorlig åtkomst</span><span class="sxs-lookup"><span data-stu-id="a8a7e-109">Device-based conditional access</span></span>](active-directory-conditional-access-automatic-device-registration-setup.md)

- [<span data-ttu-id="a8a7e-110">Enterprise centrala inställningar</span><span class="sxs-lookup"><span data-stu-id="a8a7e-110">Enterprise roaming of settings</span></span>](active-directory-windows-enterprise-state-roaming-overview.md)

- [<span data-ttu-id="a8a7e-111">Windows Hello för företag</span><span class="sxs-lookup"><span data-stu-id="a8a7e-111">Windows Hello for Business</span></span>](active-directory-azureadjoin-passport-deployment.md)


<span data-ttu-id="a8a7e-112">Det här dokumentet innehåller felsökningsinformation om hur du löser problem.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-112">This document provides troubleshooting guidance on how to resolve potential issues.</span></span> 

<span data-ttu-id="a8a7e-113">Registreringen stöds i Windows 10 November 2015 uppdatera och högre.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-113">The registration is supported in the Windows 10 November 2015 Update and above.</span></span>  
<span data-ttu-id="a8a7e-114">Vi rekommenderar att du använder årsdagar uppdateringen för att aktivera ovanstående scenarier.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-114">We recommend using the Anniversary Update for enabling the scenarios above.</span></span>

## <a name="step-1-retrieve-the-registration-status"></a><span data-ttu-id="a8a7e-115">Steg 1: Hämta registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="a8a7e-115">Step 1: Retrieve the registration status</span></span> 

<span data-ttu-id="a8a7e-116">**Att hämta registreringsstatus:**</span><span class="sxs-lookup"><span data-stu-id="a8a7e-116">**To retrieve the registration status:**</span></span>

1. <span data-ttu-id="a8a7e-117">Öppna Kommandotolken som administratör.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-117">Open the command prompt as an administrator.</span></span>

2. <span data-ttu-id="a8a7e-118">Typen **dsregcmd/status**</span><span class="sxs-lookup"><span data-stu-id="a8a7e-118">Type **dsregcmd /status**</span></span>



    <span data-ttu-id="a8a7e-119">+----------------------------------------------------------------------+
   | Enhetens tillstånd |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="a8a7e-119">+----------------------------------------------------------------------+
    | Device State                                                         |  +----------------------------------------------------------------------+</span></span>
    
        AzureAdJoined : YES
     <span data-ttu-id="a8a7e-120">EnterpriseJoined: Inga DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 tumavtryck: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft-plattformen krypto Provider TpmProtected: Ja KeySignTest:: måste köra utökade för att testa.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-120">EnterpriseJoined : NO DeviceId : 5820fbe9-60c8-43b0-bb11-44aee233e4e7 Thumbprint : B753A6679CE720451921302CA873794D94C6204A KeyContainerId : bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider : Microsoft Platform Crypto Provider TpmProtected : YES KeySignTest: : MUST Run elevated to test.</span></span>
                  <span data-ttu-id="a8a7e-121">IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ja DomainName: CONTOSO</span><span class="sxs-lookup"><span data-stu-id="a8a7e-121">Idp : login.windows.net TenantId : 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName : Contoso AuthCodeUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl : https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl : https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl : https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl : https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl : eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ== JoinSrvVersion : 1.0 JoinSrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId : urn:ms-drs:enterpriseregistration.windows.net KeySrvVersion : 1.0 KeySrvUrl : https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId : urn:ms-drs:enterpriseregistration.windows.net DomainJoined : YES DomainName : CONTOSO</span></span>
    
    <span data-ttu-id="a8a7e-122">+----------------------------------------------------------------------+
   | Användartillstånd |+----------------------------------------------------------------------+</span><span class="sxs-lookup"><span data-stu-id="a8a7e-122">+----------------------------------------------------------------------+
    | User State                                                           |  +----------------------------------------------------------------------+</span></span>
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    <span data-ttu-id="a8a7e-123">WamDefaultAuthority: organisationer WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Ja</span><span class="sxs-lookup"><span data-stu-id="a8a7e-123">WamDefaultAuthority : organizations         WamDefaultId : https://login.microsoft.com       WamDefaultGUID : {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd)           AzureAdPrt : YES</span></span>



## <a name="step-2-evaluate-the-registration-status"></a><span data-ttu-id="a8a7e-124">Steg 2: Utvärdera registreringsstatus</span><span class="sxs-lookup"><span data-stu-id="a8a7e-124">Step 2: Evaluate the registration status</span></span> 

<span data-ttu-id="a8a7e-125">Granska följande fält och se till att de har de förväntade värdena:</span><span class="sxs-lookup"><span data-stu-id="a8a7e-125">Review the following fields and make sure that they have the expected values:</span></span>

### <a name="azureadjoined--yes"></a><span data-ttu-id="a8a7e-126">AzureAdJoined: Ja</span><span class="sxs-lookup"><span data-stu-id="a8a7e-126">AzureAdJoined : YES</span></span>  

<span data-ttu-id="a8a7e-127">Det här fältet visar om enheten är registrerad med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-127">This field shows whether the device is registered with Azure AD.</span></span> <span data-ttu-id="a8a7e-128">Om värdet visas som ”Nej”, har registrering inte slutförts.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-128">If the value shows as ‘NO’, registration has not completed.</span></span> 

<span data-ttu-id="a8a7e-129">**Möjliga orsaker:**</span><span class="sxs-lookup"><span data-stu-id="a8a7e-129">**Possible causes:**</span></span>

- <span data-ttu-id="a8a7e-130">Autentisering för datorn registreringen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-130">Authentication of the computer for registration failed.</span></span>

- <span data-ttu-id="a8a7e-131">Det finns en HTTP-proxy i organisationen som inte identifieras av datorn</span><span class="sxs-lookup"><span data-stu-id="a8a7e-131">There is an HTTP proxy in the organization that cannot be discovered by the computer</span></span>

- <span data-ttu-id="a8a7e-132">Datorn kan inte nå Azure AD för autentisering eller Azure DRS för registrering</span><span class="sxs-lookup"><span data-stu-id="a8a7e-132">The computer cannot reach Azure AD for authentication or Azure DRS for registration</span></span>

- <span data-ttu-id="a8a7e-133">Datorn kanske inte i organisationens interna nätverket eller VPN-anslutning med fri till en lokal AD-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-133">The computer may not be on the organization’s internal network or on VPN with direct line of sight to an on-premises AD domain controller.</span></span>

- <span data-ttu-id="a8a7e-134">Om datorn har en TPM, kan det vara i ett felaktigt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-134">If the computer has a TPM, it may be in a bad state.</span></span>

- <span data-ttu-id="a8a7e-135">Det kan finnas en felaktig konfiguration i tjänster antecknade i dokumentet tidigare att du måste verifiera igen.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-135">There may be a misconfiguration in services noted in the document earlier that you will need to verify again.</span></span> <span data-ttu-id="a8a7e-136">Vanliga exempel är:</span><span class="sxs-lookup"><span data-stu-id="a8a7e-136">Common examples are:</span></span>

    - <span data-ttu-id="a8a7e-137">Federationsservern har inte aktiverats för WS-Trust-slutpunkter</span><span class="sxs-lookup"><span data-stu-id="a8a7e-137">Your federation server does not have WS-Trust endpoints enabled</span></span>

    - <span data-ttu-id="a8a7e-138">Federationsservern får inte tillåta inkommande autentisering från datorer i nätverket med hjälp av integrerad Windows-autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-138">Your federation server may not allow inbound authentication from computers in your network using Integrated Windows Authentication.</span></span>

    - <span data-ttu-id="a8a7e-139">Det finns inget tjänstanslutningspunkten-objekt som pekar på ett verifierat domännamn i Azure AD i AD-skog där datorn tillhör</span><span class="sxs-lookup"><span data-stu-id="a8a7e-139">There is no Service Connection Point object that points to your verified domain name in Azure AD in the AD forest where the computer belongs to</span></span>

---

### <a name="domainjoined--yes"></a><span data-ttu-id="a8a7e-140">DomainJoined: Ja</span><span class="sxs-lookup"><span data-stu-id="a8a7e-140">DomainJoined : YES</span></span>  

<span data-ttu-id="a8a7e-141">Det här fältet visar om enheten är ansluten till en lokal Active Directory eller inte.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-141">This field shows whether the device is joined to an on-premises Active Directory or not.</span></span> <span data-ttu-id="a8a7e-142">Om värdet som visas som **nr**, enheten kan inte automatiskt-register med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-142">If the value shows as **NO**, the device cannot auto-register with Azure AD.</span></span> <span data-ttu-id="a8a7e-143">Kontrollera först att enheten ansluter till lokala Active Directory innan den kan registreras med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-143">Check first that the device joins to the on-premises Active Directory before it can register with Azure AD.</span></span> <span data-ttu-id="a8a7e-144">Om du behöver för att ansluta datorn till Azure AD direkt, öppnar du lär dig mer om funktioner i Azure Active Directory Join.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-144">If you are looking for joining the computer to Azure AD directly, please go to Learn about capabilities of Azure Active Directory Join.</span></span>

---

### <a name="workplacejoined--no"></a><span data-ttu-id="a8a7e-145">WorkplaceJoined: Nej</span><span class="sxs-lookup"><span data-stu-id="a8a7e-145">WorkplaceJoined : NO</span></span>  

<span data-ttu-id="a8a7e-146">Det här fältet visar om enheten är registrerad med Azure AD, men som en personlig enhet (markerade som ”Arbetsplatsanslutna”).</span><span class="sxs-lookup"><span data-stu-id="a8a7e-146">This field shows whether the device is registered with Azure AD but as a personal device (marked as ‘Workplace Joined’).</span></span> <span data-ttu-id="a8a7e-147">Om det här värdet ska visas som ”Nej” för en domänansluten dator registrerad i Azure AD, men om det visas som Ja innebär att ett arbets- eller skolkonto konto har lagts till innan datorn slutför registreringen.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-147">If this value should show as ‘NO’ for a domain joined computer registered with Azure AD, however if it shows as YES it means that a work or school account was added prior to the computer completing registration.</span></span> <span data-ttu-id="a8a7e-148">I det här fallet ignoreras kontot om du använder den årsdagar uppdaterade versionen av Windows 10 (1607 när körs WinVer kommandot i fönstret ”kör” eller Kommandotolken).</span><span class="sxs-lookup"><span data-stu-id="a8a7e-148">In this case the account will be ignored if using the Anniversary Update version of Windows 10 (1607 when running the WinVer command in the ‘Run’ window or a command prompt window).</span></span>

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a><span data-ttu-id="a8a7e-149">WamDefaultSet: Ja och AzureADPrt: Ja</span><span class="sxs-lookup"><span data-stu-id="a8a7e-149">WamDefaultSet : YES and AzureADPrt : YES</span></span>
  
<span data-ttu-id="a8a7e-150">Dessa fält visar att användaren har autentiserats till Azure AD vid inloggning till enheten.</span><span class="sxs-lookup"><span data-stu-id="a8a7e-150">These fields show that the user has successfully authenticated to Azure AD upon signing in to the device.</span></span> <span data-ttu-id="a8a7e-151">Om de visar 'Nej' följande är möjliga orsaker:</span><span class="sxs-lookup"><span data-stu-id="a8a7e-151">If they show ‘NO’ the following are possible causes:</span></span>

- <span data-ttu-id="a8a7e-152">Felaktig lagring nyckeln (STK) i TPM som är kopplade till enheten vid registreringen (kontrollera KeySignTest när du kör upphöjd).</span><span class="sxs-lookup"><span data-stu-id="a8a7e-152">Bad storage key (STK) in TPM associated with the device upon registration (check the KeySignTest while running elevated).</span></span>

- <span data-ttu-id="a8a7e-153">Alternativt inloggnings-ID</span><span class="sxs-lookup"><span data-stu-id="a8a7e-153">Alternate Login ID</span></span>

- <span data-ttu-id="a8a7e-154">Det gick inte att hitta HTTP-Proxy</span><span class="sxs-lookup"><span data-stu-id="a8a7e-154">HTTP Proxy not found</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8a7e-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8a7e-155">Next steps</span></span>

<span data-ttu-id="a8a7e-156">Mer information finns i [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md)</span><span class="sxs-lookup"><span data-stu-id="a8a7e-156">For more information, see the [Automatic device registration FAQ](active-directory-device-registration-faq.md)</span></span> 