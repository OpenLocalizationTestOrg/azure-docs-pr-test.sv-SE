---
title: "Azure AD Domain Services: Jämföra Azure AD Domain Services tooDIY domänkontrollanter | Microsoft Docs"
description: "Jämföra Azure Active Directory Domain Services tooDIY-domänkontrollanter"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: maheshu
ms.openlocfilehash: 5e384f6a676e76e4f22ff62d4aeb578bc8481ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodecide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Hur toodecide om Azure AD Domain Services som passar dina användningsfall
Du kan distribuera dina arbetsbelastningar i Azure Infrastructure Services utan tooworry om hur du underhåller identitetsinfrastruktur i Azure med Azure AD Domain Services. Den här hanterade tjänsten skiljer sig från en typisk distribution för Windows Server Active Directory som du kan distribuera och administrera på egen hand. hello-tjänsten kan enkelt toodeploy och ger automatisk hälsoövervakning och reparation. Vi utvecklas ständigt hello tooadd stöd för vanliga distributionscenarier.

toodecide om toouse Azure AD Domain Services rekommenderar vi hello följande läsning material:

* Visa hello lista över [funktioner som erbjuds av Azure AD Domain Services](active-directory-ds-features.md).
* Granska gemensamma [distributionsscenarier för Azure AD Domain Services](active-directory-ds-scenarios.md).
* Slutligen [jämföra Azure AD Domain Services tooa själv AD alternativet](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).

## <a name="compare-azure-ad-domain-services-toodiy-ad-domain-in-azure"></a>Jämföra Azure AD Domain Services tooDIY AD-domän i Azure
hello hjälper nedan dig att välja mellan att använda Azure AD Domain Services och hantera din egen infrastruktur för AD i Azure.

| **Funktion** | **Azure AD Domain Services** | **'Själv' AD i virtuella Azure-datorer** |
| --- |:---:|:---:|
| [**Hanterad tjänst**](active-directory-ds-comparison.md#managed-service) |**&#x2713;** |**& #x 2715;** |
| [**Säker distribution**](active-directory-ds-comparison.md#secure-deployments) |**&#x2713;** |Administratören måste toosecure hello distribution. |
| [**DNS-server**](active-directory-ds-comparison.md#dns-server) |**& #x 2713;**  (hanterad service) |**&#x2713;** |
| [**Administratörsbehörighet för domänen eller Enterprise**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges) |**& #x 2715;** |**&#x2713;** |
| [**Anslut till domän**](active-directory-ds-comparison.md#domain-join) |**&#x2713;** |**&#x2713;** |
| [**Med hjälp av NTLM och Kerberos-domänautentisering**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&#x2713;** |**&#x2713;** |
| [**Kerberos-begränsad delegering**](active-directory-ds-comparison.md#kerberos-constrained-delegation)|Resursbaserad|Resursbaserad & konto-baserade|
| [**Anpassade organisationsenhetsstruktur**](active-directory-ds-comparison.md#custom-ou-structure) |**&#x2713;** |**&#x2713;** |
| [**Schemautökningar**](active-directory-ds-comparison.md#schema-extensions) |**& #x 2715;** |**&#x2713;** |
| [**AD-domän/skogsförtroenden**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts) |**& #x 2715;** |**&#x2713;** |
| [**Läsa LDAP**](active-directory-ds-comparison.md#ldap-read) |**&#x2713;** |**&#x2713;** |
| [**Säkert LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap) |**&#x2713;** |**&#x2713;** |
| [**LDAP-skrivåtgärder**](active-directory-ds-comparison.md#ldap-write) |**& #x 2715;** |**&#x2713;** |
| [**Grupprincip**](active-directory-ds-comparison.md#group-policy) |**&#x2713;** |**&#x2713;** |
| [**Fördelade distributioner**](active-directory-ds-comparison.md#geo-dispersed-deployments) |**& #x 2715;** |**&#x2713;** |

#### <a name="managed-service"></a>Hanterad tjänst
Azure AD Domain Services-domäner som hanteras av Microsoft. Du har inte tooworry om korrigering, uppdateringar, övervakning, säkerhetskopiering, och säkerställa tillgängligheten för din domän. Dessa hanteringsaktiviteter erbjuds som en tjänst av Microsoft Azure för hanterade domäner.

#### <a name="secure-deployments"></a>Säker distribution
hello-hanterad domän har låsts enligt Microsofts säkerhetsrekommendationer för AD-distributioner på ett säkert sätt. De här rekommendationerna härrör från hello AD-produktteamets åren erfarenhet av tekniker och stöd för AD-distributioner. För distributioner av själv, behöver du tootake distribution steg toolock ned/säkra din distribution.

#### <a name="dns-server"></a>DNS-server
En Azure AD Domain Services-hanterad domän innehåller hanterade DNS-tjänster. Medlemmar i gruppen för hello AAD DC-administratörer kan hantera DNS på hello hanterade domän. Medlemmar i gruppen ges fullständig behörighet för DNS-Administration för hello hanterade domän. DNS-hantering kan utföras med hjälp av hello 'DNS-administrationskonsolen ”i paketet för hello Remote verktyg för fjärrserveradministration (RSAT).
[Mer information](active-directory-ds-admin-guide-administer-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domän eller en företagsadministratör privilegier
Dessa utökade privilegier erbjuds inte på en hanterad AAD-DS-domän. Program som kräver dessa förhöjd behörighet inte kan distribueras mot AAD DS hanterade domäner. En mindre deluppsättning av administratörsbehörighet är tillgängliga toomembers av hello delegerad administration grupp med namnet AAD DC-administratörer. Dessa behörigheter omfattar privilegier tooconfigure DNS, konfigurera en Grupprincip, får administratörsbehörighet på domänanslutna datorer osv.

#### <a name="domain-join"></a>Anslut till domän
Du kan ansluta till virtuella datorer toohello hanterade domän liknande toohow du ansluta till datorer tooan AD-domän.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Med hjälp av NTLM och Kerberos-domänautentisering
Du kan använda din företagsuppgifter tooauthenticate med hello hanterade domänen med Azure AD Domain Services. Autentiseringsuppgifter hålls synkroniserade med Azure AD-klienten. För synkroniserade klienter gör Azure AD Connect ändringar gjorts toocredentials lokala synkroniserade tooAzure AD. Du kan behöva tooset upp en AD-domän med en själv domän installationen förtroende med din lokala AD för tooauthenticate för användare med sina företagsuppgifter. Alternativt kan behöva du tooset in AD replikering tooensure att synkronisera användarlösenord tooyour Azure virtuella domänkontroller-datorer.

#### <a name="kerberos-constrained-delegation"></a>Kerberos-begränsad delegering
Du har inte ' domän ' administratörsbehörighet på en hanterad domän i Active Directory Domain Services. Därför kan du konfigurera kontot-baserade (traditionell) Kerberos-begränsad delegering. Du kan dock konfigurera hello säkrare Resursbaserad begränsad delegering.
[Mer information](active-directory-ds-enable-kcd.md)

#### <a name="custom-ou-structure"></a>Anpassade organisationsenhetsstruktur
Medlemmar i gruppen för hello AAD DC-administratörer kan skapa anpassade organisationsenheter inom hello hanterade domän. Användare som skapar anpassade organisationsenheter beviljas fullständig administratörsbehörighet över hello Organisationsenhet.
[Mer information](active-directory-ds-admin-guide-create-ou.md)

#### <a name="schema-extensions"></a>Schematillägg
Du kan inte utöka hello grundläggande schemat för en Azure AD Domain Services-hanterad domän. Därför kan program som förlitar sig på tillägg tooAD schema (t.ex, nya attribut under hello användarobjektet) kan inte hävas och flyttat tooAAD DS-domäner.

#### <a name="ad-domain-or-forest-trusts"></a>AD-domän eller skogsförtroenden
Hanterade domäner får inte vara konfigurerade tooset upp (inkommande/utgående) förtroenderelationer med andra domäner. Därför kan använda inte resursen skog distributionsscenarier Azure AD Domain Services. Distributioner där du föredrar att inte toosynchronize lösenord tooAzure AD använda inte på samma sätt kan Azure AD Domain Services.

#### <a name="ldap-read"></a>LDAP-läsning
hello hanterade domän stöder LDAP skrivskyddade arbetsbelastningar. Därför kan du distribuera program som utför LDAP läsåtgärder mot hello hanterade domän.

#### <a name="secure-ldap"></a>Säkert LDAP
Du kan konfigurera Azure AD Domain Services tooprovide säker LDAP åtkomst tooyour hanterade domänen, inklusive över hello internet.
[Mer information](active-directory-ds-admin-guide-configure-secure-ldap.md)

#### <a name="ldap-write"></a>LDAP-skrivåtgärder
hello-hanterad domän är skrivskyddad för användarobjekt. Därför fungerar inte program som utför LDAP-skrivåtgärder mot attribut för användarobjekt hello i en hanterad domän. Dessutom kan lösenord inte ändras från inom hello hanterade domän. Ett annat exempel är ändring av gruppmedlemskap eller Gruppattribut i hello hanterade domän, vilket inte är tillåtet. Dock ändringar toouser attribut eller lösenord som gjorts i Azure AD (via PowerShell/Azure portal) eller lokal AD synkroniseras toohello AAD-DS hanterade domän.

#### <a name="group-policy"></a>Grupprincip
Det finns en inbyggd GPO varje för hello ”AADDC Computers” och ”AADDC användare” behållare. Du kan anpassa dessa inbyggda grupprincipobjekt tooconfigure Grupprincip. Medlemmar i gruppen för hello AAD DC-administratörer kan också skapa anpassade grupprincipobjekt och länka dem tooexisting organisationsenheter (inklusive anpassade organisationsenheter).
[Mer information](active-directory-ds-admin-guide-administer-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>GEO-spridda distributioner
Azure AD Domain Services-hanterade domäner finns i ett enda virtuellt nätverk i Azure. För scenarier som kräver domän domänkontrollanter-toobe tillgängliga i Azure-regioner över hello world, kanske konfigurera domänkontrollanter i Azure IaaS-VM hello bättre alternativ.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>'Själv' (DIY) AD-distributionsalternativ
Du kan ha distribution användningsfall där du behöver några av hello funktionerna som erbjuds av en Windows Server AD-installation. Överväg att något av följande alternativ för själv (DIY) hello i dessa fall:

* **Fristående molnet domän:** kan du ställa in en fristående 'molnet domän' med hjälp av Azure virtuella datorer som har konfigurerats som domänkontrollanter. Den här infrastrukturen är inte integrerat med din lokala AD-miljö. Det här alternativet kräver en annan uppsättning 'molnet autentiseringsuppgifter' toologin/administrera VMs i hello molnet.
* **Distribution i skogar:** du kan konfigurera en domän i skogstopologi för hello resursen med hjälp av Azure virtuella datorer som konfigurerats som domänkontrollanter. Därefter kan du konfigurera ett AD-förtroende med din lokala AD-miljö. Du kan domänanslutning datorer (Azure VM) toothis resursskogen hello molnet. Användarautentisering sker via antingen en VPN-/ ExpressRoute-anslutning tooyour lokala katalog.
* **Utöka din lokal domän tooAzure:** kan du ansluta ett virtuellt Azure-nätverk tooyour lokalt nätverk med en VPN-/ ExpressRoute-anslutning. Denna inställning kan virtuella Azure-datorer toobe kopplade tooyour lokala AD. Ett annat alternativ är toopromote replika-domänkontroller i din lokala domän i Azure som en virtuell dator. Du kan sedan konfigurera den tooreplicate via en VPN-/ ExpressRoute-anslutning tooyour lokala katalog. Det här distributionsläget utökar dina lokala domän tooAzure effektivt.

> [!NOTE]
> Du kan bestämma att ett själv alternativ passar bättre för din distribution användningsfall. Överväg att [delning feedback](active-directory-ds-contact-us.md) toohelp oss att förstå vad funktioner hjälper du har valt Azure AD Domain Services i hello framtida. Denna feedback hjälper oss att utvecklas hello service toobetter färg distributionen måste och användningsfall.
>
>

Vi har publicerat [riktlinjer för att distribuera Windows Server Active Directory på Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx) toohelp lättare själv installationer.

## <a name="related-content"></a>Relaterat innehåll
* [Funktioner – Azure AD Domain Services](active-directory-ds-features.md)
* [Distributionsscenarier - Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Riktlinjer för att distribuera Windows Server Active Directory på Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx)
