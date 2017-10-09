---
title: "aaaAzure hantering och övervakning-översikt | Microsoft Docs"
description: " Azure tillhandahåller säkerhet mekanismer tooaid i hello hantering och övervakning av Azure-molntjänster och virtuella datorer.  Den här artikeln innehåller en översikt över dessa kärnfunktioner för säkerhet och tjänster. "
services: security
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: terrylan
ms.openlocfilehash: 0026fa97bab7e15c9f8de6856b5075abe2288f61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure-säkerhetshantering och övervakning av översikt
Azure tillhandahåller säkerhet mekanismer tooaid i hello hantering och övervakning av Azure-molntjänster och virtuella datorer. Den här artikeln innehåller en översikt över dessa kärnfunktioner för säkerhet och tjänster. Länkar finns tooarticles som ger information om varje så kan du läsa mer.

hello säkerheten för din Microsoft-molntjänster är en partnerskap och delat ansvar mellan dig och Microsoft. Delat ansvar innebär Microsoft ansvarar för hello Microsoft Azure och fysisk säkerhet för sina datacenter (med hjälp av säkerhetsskydd som låst Aktivitetsikon post dörrar, avgränsningstecken och skydd). Azure tillhandahåller dessutom starkt säkerhetsnivåer molnet på hello Programsteg som uppfyller hello säkerhet, sekretess och kompatibilitet dess krävande kundernas behov.

Du äger dina data och identiteter, hello ansvar för att skydda dem hello säkerheten för dina lokala resurser och hello säkerheten för moln-komponenter som du har kontroll. Microsoft ger dig med säkerhet kontroller och funktioner toohelp du skydda dina data och program. Din graden av ansvar för säkerhet baseras på hello typ av tjänst i molnet.

följande diagram hello sammanfattas hello saldo ansvar för både Microsoft och hello kund.

![Delat ansvar][1]

En mer grundlig genomgång i säkerhetshantering finns [säkerhetshantering i Azure](azure-security-management.md).

Här följer hello core funktioner toobe beskrivs i den här artikeln:

* Rollbaserad Access Control
* Programvara mot skadlig kod
* Multi-Factor Authentication
* ExpressRoute
* Virtuella nätverksgatewayer
* Privileged identity management
* Identitetsskydd
* Security Center

## <a name="role-based-access-control"></a>Rollbaserad Access Control
Rollbaserad åtkomstkontroll (RBAC) ger detaljerad åtkomsthantering för Azure-resurser. Med RBAC kan bevilja du personer endast hello mängd åtkomst som de behöver tooperform sitt arbete.  RBAC kan du också se till att de förlorar åtkomst tooresources i hello molnet när personer lämnar hello organisation.

Läs mer:

* [Active Directory-teamets blogg på RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
* [Rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Programvara mot skadlig kod
Du kan använda program mot skadlig kod från leverantörer av större säkerhet, till exempel Microsoft, Symantec, Trend Micro, McAfee med Azure, och Kaspersky toohelp skydda dina virtuella datorer från skadliga filer, annonsprogram och andra hot.

Microsoft Antimalware erbjuder du hello möjlighet tooinstall en agent för program mot skadlig kod för virtuella datorer och PaaS-roller. Den här funktionen ger baserat på System Center Endpoint Protection, beprövade lokal säkerhet teknik toohello moln.

Vi erbjuder även djupgående integrering för Trenders [djup säkerhet](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ och [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ produkter i hello Azure-plattformen. DeepSecurity är en Antivirus och SecureCloud är en lösning för kryptering. DeepSecurity distribueras i virtuella datorer med hjälp av en modell för tillägget. Med hello portalens användargränssnitt och PowerShell kan välja du toouse DeepSecurity i nya virtuella datorer som skapas eller befintliga virtuella datorer som redan har distribuerats.

Skydd för Symantec-slutpunkt (SEP) stöds även på Azure. Kunder kan via portalen integration ange att de avser toouse SEP på en virtuell dator. SEP kan installeras på en helt ny virtuell dator via hello Azure-portalen eller kan installeras på en befintlig virtuell dator med hjälp av PowerShell.

Läs mer:

* [Distribuera lösningar för skydd mot skadlig kod i Azure Virtual Machines](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Microsoft program mot skadlig kod för Azure-molntjänster och virtuella datorer](azure-security-antimalware.md)
* [Hur tooinstall och konfigurera Trend Micro djup säkerhet som en tjänst på en Windows VM](../virtual-machines/windows/classic/install-trend.md)
* [Hur tooinstall och konfigurera Symantec Endpoint Protection på en Windows VM](../virtual-machines/windows/classic/install-symantec.md)
* [Nya alternativ för program mot skadlig kod för att skydda virtuella Azure-datorer – McAfee Endpoint Protection](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure Multi-Factor authentication (MFA) är en autentiseringsmetod som kräver hello använder mer än en verifieringsmetod och lägger till ett kritiskt andra säkerhetslager säkerhet toouser inloggningar och transaktioner. MFA hjälper dig att skydda åtkomst toodata och program och uppfyller efterfrågan från användarna för en process för enkel inloggning. Den ger stark autentisering via en mängd alternativ för verifiering – telefonsamtal, textmeddelande eller mobilapp meddelande eller verifiering kod och tredjeparts OATH-token.

Läs mer:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Vad är Azure Multi-Factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)
* [Hur Azure Multi-Factor Authentication fungerar](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute
Microsoft Azure ExpressRoute kan du utöka ditt lokala nätverk till hello Microsoft cloud via en dedikerad privata anslutning underlättas av en provider för anslutningen. Du kan upprätta anslutningar molntjänster tooMicrosoft, till exempel Microsoft Azure, Office 365 och CRM Online med ExpressRoute. Anslutningen kan vara från ett ”any-to-any”-nätverk (IP VPN), ett ”point-to-point”-nätverk med Ethernet eller en virtuell korsanslutning via en anslutningsleverantör på en samlokaliseringsanläggning. ExpressRoute-anslutningar inte överskrider hello offentliga Internet. Detta tillåter ExpressRoute anslutningar toooffer mer tillförlitlighet, högre hastighet, lägre latens och högre säkerhet än vanliga anslutningar via hello Internet.

Läs mer:

* [Teknisk översikt för ExpressRoute](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Virtuella nätverksgatewayer
VPN-gatewayer, även kallat Azure virtuella nätverks-gateway, används toosend nätverkstrafik mellan virtuella nätverk och lokala platser. De är också används toosend trafik mellan flera virtuella nätverk i Azure (VNet-till-VNet).  VPN-gatewayer ger säker anslutning mellan Azure och din infrastruktur.

Läs mer:

* [Om VPN-gatewayer](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Översikt över säkerheten i Azure-nätverk](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Ibland behöver användare toocarry Privilegierade åtgärder i Azure-resurser eller andra SaaS-program. Detta innebär ofta organisationer har toogive dem permanent privilegierad åtkomst i Azure Active Directory (AD Azure). Detta är en allt större säkerhetsrisk för molnbaserade resurser eftersom organisationer inte tillräckligt övervaka vad användarna gör med deras privilegierad åtkomst.
Om ett användarkonto med privilegierad åtkomst äventyras, kan dessutom som en överträdelse påverka din övergripande säkerheten för molnet. Azure AD Privileged Identity Management kan tooresolve den här risken genom att sänka hello tidsperioden privilegier och öka insyn i användning.  

Privileged Identity Management introducerar hello begreppet en temporär administratör för en roll eller just-in-time ”administratörsåtkomst, vilket är en användare behöver toocomplete aktiveringsprocessen för som tilldelats rollen. hello aktivering processen ändringar hello tilldelning av hello tooa användarroll i Azure AD från inaktiva tooactive för en angiven tidsperiod, till exempel åtta timmar.

Läs mer:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Kom igång med Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Identity Protection
Azure Active Directory (AD) Identity Protection ger en samlad vy över inloggning aktiviteter och potentiella säkerhetsproblem toohelp skydda din verksamhet. Identity Protection upptäcker misstänkta aktiviteter för användare och Privilegierade (admin) identiteter, baserat på signaler som brute force-attacker läcka ut autentiseringsuppgifter och inloggningar från okända platser och infekterade enheter.

Genom att tillhandahålla meddelanden och rekommenderade åtgärderna hjälper identitetsskydd toomitigate risker i realtid. Beräkning av användaren risk allvarlighetsgrad och du kan konfigurera programåtkomst för risk-baserade policys tooautomatically hjälp skyddar mot framtida hot.

Läs mer:

* [Azure Active Directory Identity Protection](../active-directory/active-directory-identityprotection.md)
* [Channel 9: Azure AD och Identity visa: Identity Protection Preview](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Security Center
Azure Security Center hjälper dig att förebygga, upptäcka och åtgärda toothreats och ger du ökad insyn i, och kontroll över, hello säkerheten för dina Azure-resurser. Härifrån kan du övervaka och hantera principer för alla Azureprenumerationer på en gång och upptäcka hot som annars kanske skulle förbli oupptäckta. Azure Security Center fungerar tillsammans med ett vittomfattande ekosystem med säkerhetslösningar.

Security Center hjälper dig att optimera och övervaka hello säkerheten för dina Azure-resurser genom att:

* Aktivera toodefine principer för dina Azure-prenumerationsresurser enligt tooyour företagets säkerhet måste och hello typ av program eller känslighet hello data i varje prenumeration.
* Övervakning av hello tillståndet för din virtuella Azure-datorer, nätverk och program.
* Ger en lista över prioriteras säkerhetsaviseringar, inklusive aviseringar från integrerade partner solutions tillsammans med hello information du behöver tooquickly undersöka och rekommendationer om hur tooremediate angreppet.

Läs mer:

* [Introduktion tooAzure Security Center](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
