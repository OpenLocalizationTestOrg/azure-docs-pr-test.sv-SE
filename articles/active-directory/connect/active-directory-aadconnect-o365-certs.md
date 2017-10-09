---
title: "aaaCertificate förnyelse för Office 365 och Azure AD-användare | Microsoft Docs"
description: "Den här artikeln förklarar tooOffice 365 användare hur tooresolve problem med e-postmeddelanden som meddelar dem om att förnya ett certifikat."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 543b7dc1-ccc9-407f-85a1-a9944c0ba1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: b9b309e06949dc5488cd628650be413f366ed347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Förnya federationscertifikat för Office 365 och Azure Active Directory
## <a name="overview"></a>Översikt
För lyckad federation mellan Azure Active Directory (Azure AD) och Active Directory Federation Services (AD FS) matchar hello-certifikat som används av AD FS toosign säkerhet token tooAzure AD det som har konfigurerats i Azure AD. Några matchningsfel kan leda toobroken förtroende. Azure AD ser till att den här informationen hålls synkroniserade när du distribuerar AD FS och Webbprogramproxy (för åtkomst till extranät).

Den här artikeln innehåller ytterligare information toomanage certifikat för tokensignering och hålla dem synkroniserade med Azure AD i hello följande fall:

* Du distribuerar inte hello Web Application Proxy och därför hello federationsmetadata är inte tillgänglig i hello extranät.
* Du använder inte hello standardkonfigurationen av AD FS för certifikat för tokensignering.
* Du använder en tredjeparts identitetsprovider.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Standardkonfigurationen för AD FS för certifikat för tokensignering
hello tokensignering token dekryptera certifikat är vanligtvis självsignerade certifikat och är bra för ett år. Som standard AD FS innehåller en automatisk förnyelse process som kallas **AutoCertificateRollover**. Om du använder AD FS 2.0 eller senare, Office 365 och Azure AD automatiskt uppdatera ditt certifikat innan den upphör.

### <a name="renewal-notification-from-hello-office-365-portal-or-an-email"></a>Förnyelse meddelande från hello Office 365-portalen eller ett e-postmeddelande
> [!NOTE]
> Om du har fått ett e-postmeddelande eller en portal meddelande som ber dig toorenew ditt certifikat för Office, se [hantera ändras tootoken signeringscertifikat](#managecerts) toocheck om du behöver tootake något. Microsoft är medveten om ett eventuellt problem som kan leda till toonotifications för förnyelse av certifikat som skickas, även om ingen åtgärd krävs.
>
>

Azure AD försöker toomonitor hello federationsmetadata och uppdatera hello certifikaten för tokensignering som anges av dessa metadata. 30 dagar före hello förfallodatum för certifikat för tokensignering hello kontrollerar Azure AD om nya certifikat är tillgängliga genom att avsöka hello federationsmetadata.

* Om den kan har avsöka hello federationsmetadata och hämta hello nya certifikat, utfärdas utan e-postmeddelande eller en varning i hello Office 365-portalen toohello användare.
* Om det går inte att hämta hello nya certifikaten för tokensignering, antingen eftersom hello federationsmetadata kan inte nås eller automatisk förnyelse inte är aktiverat Azure AD utfärdar ett e-postmeddelande och en varning i hello Office 365-portalen.

![Office 365-portalen meddelande](./media/active-directory-aadconnect-o365-certs/notification.png)

> [!IMPORTANT]
> Om du använder AD FS, tooensure affärskontinuitet, kontrollerar du att servrarna har hello efter uppdateringar så att autentiseringsfel för kända problem, inte sker. Detta minskar kända AD FS-proxy serverproblem för förnyelse och framtida förnyelse punkter:
>
> Server 2012 R2 - [Windows Server kan 2014-uppdateringen](http://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 och 2012 - [autentisering via proxy misslyckas i Windows Server 2012 eller Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)
>
>

## Kontrollera om hello certifikat måste toobe uppdateras<a name="managecerts"></a>
### <a name="step-1-check-hello-autocertificaterollover-state"></a>Steg 1: Kontrollera hello AutoCertificateRollover tillstånd
Öppna PowerShell på AD FS-servern. Kontrollera att hello AutoCertificateRollover har värdet tooTrue.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

>[!NOTE] 
>Om du använder AD FS 2.0 kan du först köra Add-Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Steg 2: Kontrollera att AD FS och Azure AD är synkroniserade
Öppna hello Azure AD PowerShell-Kommandotolken på AD FS-servern och Anslut tooAzure AD.

> [!NOTE]
> Du kan ladda ned Azure AD PowerShell [här](https://technet.microsoft.com/library/jj151815.aspx).
>
>

    Connect-MsolService

Kontrollera hello certifikat i AD FS och Azure AD-förtroende egenskaper för hello angiven domän.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Om hello tumavtryck i både hello utdata matchning certifikaten är synkroniserade med Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-tooexpire"></a>Steg 3: Kontrollera om ditt certifikat är om tooexpire
Hello utdata av Get-MsolFederationProperty eller Get-AdfsCertificate och kontrollera hello datum under ”inte efter”. Om hello datum är mindre än 30 dagar direkt, bör du vidta åtgärder.

| AutoCertificateRollover | Certifikat som är synkroniserade med Azure AD | Federationsmetadata är offentligt tillgänglig | Giltighetsperiod | Åtgärd |
|:---:|:---:|:---:|:---:|:---:|
| Ja |Ja |Ja |- |Ingen åtgärd krävs. Se [förnya certifikat för tokensignering certifikat automatiskt](#autorenew). |
| Ja |Nej |- |Mindre än 15 dagar |Förnya omedelbart. Se [förnya certifikat för tokensignering certifikat manuellt](#manualrenew). |
| Nej |- |- |Mindre än 30 dagar |Förnya omedelbart. Se [förnya certifikat för tokensignering certifikat manuellt](#manualrenew). |

\[-] Spelar ingen roll

## Förnya hello certifikatet för tokensignering automatiskt (rekommenderas)<a name="autorenew"></a>
Du behöver inte tooperform alla manuella steg om båda hello följande är sant:

* Du har distribuerat Web Application Proxy, vilket kan ge åtkomst toohello federationsmetadata från hello extranät.
* Du använder standardkonfigurationen för hello AD FS (AutoCertificateRollover är aktiverat).

Kontrollera hello följande tooconfirm som hello certifikat kan uppdateras automatiskt.

**1. hello AD FS-egenskapen AutoCertificateRollover måste anges tooTrue.** Detta anger att AD FS kommer automatiskt att generera nya certifikat för tokensignering och tokendekryptering certifikat innan hello gamla de som går ut.

**2. hello AD FS-federationsmetadata är allmänt tillgänglig.** Kontrollera att din federationsmetadata är offentligt tillgänglig genom att gå toohello följande URL: en från en dator hello offentliga internet (från hello företagsnätverket):

https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

där `(your_FS_name) `ersätts med hello federation service värdnamn används i din organisation, till exempel fs.contoso.com.  Om du kan tooverify båda av de här inställningarna, behöver du inte toodo någonting annat.  

Exempel: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Förnya hello certifikatet för tokensignering manuellt<a name="manualrenew"></a>
Du kan välja toorenew hello certifikat för tokensignering manuellt. Till exempel kan hello följande scenarier fungera bättre för manuell förnyelse:

* Token signera certifikat är inte självsignerade certifikat. hello vanligaste anledningen är att din organisation hanterar AD FS-certifikat som registrerats från en organisations certifikatutfärdare.
* Nätverkssäkerhet tillåter inte hello federation metadata toobe offentligt tillgängliga.

I dessa scenarier varje gång du uppdaterar hello-certifikat för tokensignering måste du uppdatera din Office 365-domän med hjälp av hello PowerShell-kommando uppdatering MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Steg 1: Kontrollera att AD FS har nya certifikat för tokensignering
**Icke-standardkonfigurationen**

Om du använder en icke-förvalt konfiguration av AD FS (där **AutoCertificateRollover** har angetts för**FALSKT**), använder du troligtvis anpassade certifikat (inte självsignerade). Mer information om hur toorenew hello AD FS-tokensignering certifikat finns [vägledning för kunder som inte använder AD FS självsignerade certifikat](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federationsmetadata är inte tillgänglig**

Hej å andra sidan, om **AutoCertificateRollover** har angetts för**SANT**, men din federationsmetadata är inte offentligt tillgänglig, kontrollera först att nya certifikat för tokensignering har genererats av AD FS. Bekräfta att du har nya certifikaten för tokensignering av tar hello följande steg:

1. Kontrollera att du är inloggad på toohello primär AD FS-servern.
2. Kontrollera hello aktuella signeringscertifikat i AD FS genom att öppna ett PowerShell-kommandofönster och kör följande kommando hello:

    PS C:\>Get-ADFSCertificate – CertificateType certifikat för tokensignering

   > [!NOTE]
   > Om du använder AD FS 2.0, bör du köra Add-Pssnapin Microsoft.Adfs.Powershell först.
   >
   >
3. Titta på hello kommandoutdata på alla certifikat i listan. Om AD FS har genererat ett nytt certifikat, bör du se två certifikat i hello utdata: en för vilka hello **IsPrimary** värdet är **SANT** och hello **NotAfter** datum är inom fem dagar och ett som **IsPrimary** är **FALSKT** och **NotAfter** handlar om ett år i hello framtida.
4. Om du bara finns ett certifikat och hello **NotAfter** datumet ligger inom fem dagar, måste du toogenerate ett nytt certifikat.
5. toogenerate ett nytt certifikat, kör följande kommando i Kommandotolken PowerShell hello: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Kontrollera hello uppdateringen genom att köra följande kommando igen hello: PS C:\>Get-ADFSCertificate – CertificateType certifikat för tokensignering

Två certifikat ska visas nu, ett som har en **NotAfter** datum ungefär ett år i hello framtida och för vilka hello **IsPrimary** värdet är **FALSKT**.

### <a name="step-2-update-hello-new-token-signing-certificates-for-hello-office-365-trust"></a>Steg 2: Uppdatera hello nya certifikaten för tokensignering för hello Office 365-förtroende
Uppdatera Office 365 med hello nya token signering certifikat toobe används för hello förtroende på följande sätt.

1. Öppna hello Microsoft Azure Active Directory-modulen för Windows PowerShell.
2. Kör $cred = Get-Credential. När denna cmdlet efterfrågar autentiseringsuppgifter, skriver du cloud service administratörskontots autentiseringsuppgifter.
3. Kör Anslut MsolService – autentiseringsuppgifter $cred. Denna cmdlet ansluter toohello Molntjänsten. Skapa en kontext som ansluter du toohello Molntjänsten krävs innan du kör hello ytterligare cmdlets som installerats av hello-verktyget.
4. Om du kör kommandona på en dator som inte är primär hello AD FS-federationsserver, kör Set-MSOLAdfscontext-datorn <AD FS primary server>, där <AD FS primary server> är hello interna FQDN-namnet för hello primär AD FS-servern. Denna cmdlet skapar en kontext som ansluter du tooAD FS.
5. Kör Update MSOLFederatedDomain – DomainName <domain>. Denna cmdlet hello inställningar från AD FS i hello Molntjänsten för uppdateringar och konfigurerar hello förtroenderelation mellan hello två.

> [!NOTE]
> Om du behöver toosupport flera toppnivådomäner, till exempel contoso.com och fabrikam.com, måste du använda hello **SupportMultipleDomain** växel med alla cmdlets. Mer information finns i [stöd för flera domäner på översta nivån](active-directory-aadconnect-multiple-domains.md).
>
>

## Reparera Azure AD-förtroende med hjälp av Azure AD Connect<a name="connectrenew"></a>
Om du har konfigurerat dina AD FS-servergrupp och Azure AD-förtroende med hjälp av Azure AD Connect kan du använda Azure AD Connect toodetect om du behöver tootake någon åtgärd för certifikat för tokensignering. Om du behöver toorenew hello certifikat kan använda du Azure AD Connect toodo så.

Mer information finns i [reparera hello förtroende](active-directory-aadconnect-federation-management.md).
