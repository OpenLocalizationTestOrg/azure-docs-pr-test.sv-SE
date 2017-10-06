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
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-tooazure-ad--windows-10-and-windows-server-2016"></a>Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016

Det här avsnittet är tillämpliga toohello följande klienter:

-   Windows 10
-   Windows Server 2016

Andra windowsklienter finns i [felsökning automatisk registrering av domän domänanslutna datorer tooAzure AD för Windows äldre klienter](active-directory-device-registration-troubleshoot-windows-legacy.md).

Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md) toosupport hello följande scenarier:

- [Enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-automatic-device-registration-setup.md)

- [Enterprise centrala inställningar](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello för företag](active-directory-azureadjoin-passport-deployment.md)


Det här dokumentet innehåller felsökningsinformation för hur tooresolve potentiella problem. 

hello registrering stöds i Windows hello uppdatering 10 November 2015 och högre.  
Vi rekommenderar hello årsdagar Update för att aktivera hello scenarierna ovan.

## <a name="step-1-retrieve-hello-registration-status"></a>Steg 1: Hämta hello registreringsstatus 

**tooretrieve hello registreringsstatus:**

1. Öppna hello kommandotolk som administratör.

2. Typen **dsregcmd/status**



    +----------------------------------------------------------------------+
   | Enhetens tillstånd |+----------------------------------------------------------------------+
    
        AzureAdJoined : YES
     EnterpriseJoined: Inga DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 tumavtryck: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft-plattformen krypteringsprovider TpmProtected: Ja KeySignTest:: måste köras utökade tootest.
                  IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ja DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
   | Användartillstånd |+----------------------------------------------------------------------+
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    WamDefaultAuthority: organisationer WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Ja



## <a name="step-2-evaluate-hello-registration-status"></a>Steg 2: Utvärdera hello registreringsstatus 

Granska hello följande fält och se till att de har hello förväntade värden:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Ja  

Det här fältet visar om hello enheten är registrerad med Azure AD. Om hello värde visas som ”Nej”, har registrering inte slutförts. 

**Möjliga orsaker:**

- Autentisering för hello dator registreringen misslyckades.

- Det finns en HTTP-proxy i hello organisation som inte identifieras av hello-dator

- hello datorn går inte att nå Azure AD för autentisering eller Azure DRS för registrering

- hello datorn kanske inte på hello företags interna nätverk eller VPN-anslutning med fri tooan lokala AD-domänkontrollant.

- Om hello dator har TPM, kanske den i felaktigt tillstånd.

- Det kan finnas en felaktig konfiguration i tjänster antecknade i hello dokumentet tidigare att du måste tooverify igen. Vanliga exempel är:

    - Federationsservern har inte aktiverats för WS-Trust-slutpunkter

    - Federationsservern får inte tillåta inkommande autentisering från datorer i nätverket med hjälp av integrerad Windows-autentisering.

    - Det finns inget tjänstanslutningspunkten-objekt som pekar tooyour verifierat domännamn i Azure AD i hello AD-skog där hello datorn tillhör

---

### <a name="domainjoined--yes"></a>DomainJoined: Ja  

Det här fältet visar om hello enheten är domänansluten tooan lokala Active Directory eller inte. Om hello värde visas som **nr**, hello enheten kan inte automatiskt-register med Azure AD. Kontrollera först att hello enheten ansluter till toohello lokala Active Directory innan den kan registreras med Azure AD. Om du behöver för att ansluta till hello datorn tooAzure AD direkt gå tooLearn om funktioner i Azure Active Directory Join.

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: Nej  

Det här fältet visar hello enheten är registrerad med Azure AD, men som en personlig enhet (markerade som ”Arbetsplatsanslutna”). Om det här värdet ska visas som ”Nej” för en domänansluten dator registrerad i Azure AD, men om det visas på Ja innebär det att en arbets- eller skolkonto-kontot var tillagda tidigare toohello Slutför registreringen av datorn. I det här fallet ignoreras hello konto om du använder hello årsdagar uppdaterad version av Windows 10 (1607 när kör hello WinVer kommando i hello 'Körs' fönster eller Kommandotolken).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Ja och AzureADPrt: Ja
  
Dessa fält visar hello användaren har autentiserats tooAzure AD vid inloggning toohello enhet. Om de visar är 'Nej' hello följande möjliga orsaker:

- Felaktig lagring nyckeln (STK) i TPM som är associerade med hello enheten vid registreringen (kontrollera hello KeySignTest när du kör upphöjd).

- Alternativt inloggnings-ID

- Det gick inte att hitta HTTP-Proxy

## <a name="next-steps"></a>Nästa steg

Mer information finns i hello [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md) 
