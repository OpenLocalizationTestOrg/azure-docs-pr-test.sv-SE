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
# <a name="troubleshooting-auto-registration-of-domain-joined-computers-to-azure-ad--windows-10-and-windows-server-2016"></a>Felsökning av automatisk registrering av domän domänanslutna datorer till Azure AD – Windows 10 och Windows Server 2016

Det här avsnittet gäller följande klienter:

-   Windows 10
-   Windows Server 2016

Andra windowsklienter finns i [felsökning automatisk registrering av domän domänanslutna datorer till Azure AD för Windows-klientversioner klienter](active-directory-device-registration-troubleshoot-windows-legacy.md).

Det här avsnittet förutsätter att du har konfigurerat automatisk registrering för domänanslutna enheter som i beskrivs i [hur du konfigurerar automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-device-registration-get-started.md) till stöd för följande scenarier:

- [Enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-automatic-device-registration-setup.md)

- [Enterprise centrala inställningar](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello för företag](active-directory-azureadjoin-passport-deployment.md)


Det här dokumentet innehåller felsökningsinformation om hur du löser problem. 

Registreringen stöds i Windows 10 November 2015 uppdatera och högre.  
Vi rekommenderar att du använder årsdagar uppdateringen för att aktivera ovanstående scenarier.

## <a name="step-1-retrieve-the-registration-status"></a>Steg 1: Hämta registreringsstatus 

**Att hämta registreringsstatus:**

1. Öppna Kommandotolken som administratör.

2. Typen **dsregcmd/status**



    +----------------------------------------------------------------------+
   | Enhetens tillstånd |+----------------------------------------------------------------------+
    
        AzureAdJoined : YES
     EnterpriseJoined: Inga DeviceId: 5820fbe9-60c8-43b0-bb11-44aee233e4e7 tumavtryck: B753A6679CE720451921302CA873794D94C6204A KeyContainerId: bae6a60b-1d2f-4d2a-a298-33385f6d05e9 KeyProvider: Microsoft-plattformen krypto Provider TpmProtected: Ja KeySignTest:: måste köra utökade för att testa.
                  IDP: login.windows.net TenantId: 72b988bf-86f1-41af-91ab-2d7cd011db47 TenantName: Contoso AuthCodeUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/authorize AccessTokenUrl: https://login.microsoftonline.com/msitsupp.microsoft.com/oauth2/token MdmUrl: https://enrollment.manage-beta.microsoft.com/EnrollmentServer/Discovery.svc MdmTouUrl: https://portal.manage-beta.microsoft.com/TermsOfUse.aspx dmComplianceUrl: https://portal.manage-beta.microsoft.com/?portalAction=Compliance SettingsUrl: eyJVcmlzIjpbImh0dHBzOi8va2FpbGFuaS5vbmUubWljcm9zb2Z0LmNvbS8iLCJodHRwczovL2thaWxhbmkxLm9uZS5taWNyb3NvZnQuY29tLyJdfQ == JoinSrvVersion: 1.0 JoinSrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/device/ JoinSrvId: urn: ms-drs:enterpriseregistration.windows.net KeySrvVersion: 1.0 KeySrvUrl: https://enterpriseregistration.windows.net/EnrollmentServer/key/ KeySrvId: urn: ms-drs:enterpriseregistration.windows.net DomainJoined: Ja DomainName: CONTOSO
    
    +----------------------------------------------------------------------+
   | Användartillstånd |+----------------------------------------------------------------------+
    
                 NgcSet : YES
               NgcKeyId : {C7A9AEDC-780E-4FDA-B200-1AE15561A46B}
        WorkplaceJoined : NO
          WamDefaultSet : YES
    WamDefaultAuthority: organisationer WamDefaultId: https://login.microsoft.com WamDefaultGUID: {B16898C6-A148-4967-9171-64D755DA8520} (AzureAd) AzureAdPrt: Ja



## <a name="step-2-evaluate-the-registration-status"></a>Steg 2: Utvärdera registreringsstatus 

Granska följande fält och se till att de har de förväntade värdena:

### <a name="azureadjoined--yes"></a>AzureAdJoined: Ja  

Det här fältet visar om enheten är registrerad med Azure AD. Om värdet visas som ”Nej”, har registrering inte slutförts. 

**Möjliga orsaker:**

- Autentisering för datorn registreringen misslyckades.

- Det finns en HTTP-proxy i organisationen som inte identifieras av datorn

- Datorn kan inte nå Azure AD för autentisering eller Azure DRS för registrering

- Datorn kanske inte i organisationens interna nätverket eller VPN-anslutning med fri till en lokal AD-domänkontrollant.

- Om datorn har en TPM, kan det vara i ett felaktigt tillstånd.

- Det kan finnas en felaktig konfiguration i tjänster antecknade i dokumentet tidigare att du måste verifiera igen. Vanliga exempel är:

    - Federationsservern har inte aktiverats för WS-Trust-slutpunkter

    - Federationsservern får inte tillåta inkommande autentisering från datorer i nätverket med hjälp av integrerad Windows-autentisering.

    - Det finns inget tjänstanslutningspunkten-objekt som pekar på ett verifierat domännamn i Azure AD i AD-skog där datorn tillhör

---

### <a name="domainjoined--yes"></a>DomainJoined: Ja  

Det här fältet visar om enheten är ansluten till en lokal Active Directory eller inte. Om värdet som visas som **nr**, enheten kan inte automatiskt-register med Azure AD. Kontrollera först att enheten ansluter till lokala Active Directory innan den kan registreras med Azure AD. Om du behöver för att ansluta datorn till Azure AD direkt, öppnar du lär dig mer om funktioner i Azure Active Directory Join.

---

### <a name="workplacejoined--no"></a>WorkplaceJoined: Nej  

Det här fältet visar om enheten är registrerad med Azure AD, men som en personlig enhet (markerade som ”Arbetsplatsanslutna”). Om det här värdet ska visas som ”Nej” för en domänansluten dator registrerad i Azure AD, men om det visas som Ja innebär att ett arbets- eller skolkonto konto har lagts till innan datorn slutför registreringen. I det här fallet ignoreras kontot om du använder den årsdagar uppdaterade versionen av Windows 10 (1607 när körs WinVer kommandot i fönstret ”kör” eller Kommandotolken).

---

### <a name="wamdefaultset--yes-and-azureadprt--yes"></a>WamDefaultSet: Ja och AzureADPrt: Ja
  
Dessa fält visar att användaren har autentiserats till Azure AD vid inloggning till enheten. Om de visar 'Nej' följande är möjliga orsaker:

- Felaktig lagring nyckeln (STK) i TPM som är kopplade till enheten vid registreringen (kontrollera KeySignTest när du kör upphöjd).

- Alternativt inloggnings-ID

- Det gick inte att hitta HTTP-Proxy

## <a name="next-steps"></a>Nästa steg

Mer information finns i [automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md) 