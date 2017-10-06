---
title: aaaFAQs - Azure Active Directory Domain Services | Microsoft Docs
description: "Vanliga frågor och svar om Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 48731820-9e8c-4ec2-95e8-83dba1e58775
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: maheshu
ms.openlocfilehash: 42a6d659f6408bf694cb2aa6ec24bff7a76b0565
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-domain-services-frequently-asked-questions-faqs"></a>Azure Active Directory Domain Services: Vanliga frågor (FAQ)
Den här sidan svar på vanliga frågor om hello Azure Active Directory Domain Services. Hålla kontroll för uppdateringar.

### <a name="troubleshooting-guide"></a>Felsökningsguide
Referera tooour [felsökningsguide](active-directory-ds-troubleshooting.md) för lösningar toocommon problem uppstod när du konfigurerar eller administrera Azure AD Domain Services.

### <a name="configuration"></a>Konfiguration
#### <a name="can-i-create-multiple-domains-for-a-single-azure-ad-directory"></a>Kan jag skapa flera domäner för en enda Azure AD-katalog?
Nej. Du kan bara skapa en enda domän som underhålls av Azure AD Domain Services för en enda Azure AD-katalog.  

#### <a name="can-i-enable-azure-ad-domain-services-in-an-azure-resource-manager-virtual-network"></a>Kan jag aktivera Azure AD Domain Services i ett virtuellt nätverk med Azure Resource Manager?
Nej. Azure AD Domain Services kan bara aktiveras i den klassiska Azure-nätverk. Du kan ansluta hello klassiskt virtuellt nätverk tooa Resource Manager virtuellt nätverk med virtuella nätverk peering toouse din hanterade domän i ett virtuellt nätverk för hanteraren för filserverresurser.

#### <a name="can-i-enable-azure-ad-domain-services-in-a-federated-azure-ad-directory-i-use-adfs-tooauthenticate-users-for-access-toooffice-365-can-i-enable-azure-ad-domain-services-for-this-directory"></a>Kan jag aktivera Azure AD Domain Services i en federerad Azure AD-katalog? Jag använder AD FS tooauthenticate användare för åtkomst tooOffice 365. Kan jag aktivera Azure AD Domain Services för den här katalogen?
Nej. Azure AD Domain Services måste komma åt toohello lösenordshashvärden av användarkonton, tooauthenticate användare via NTLM eller Kerberos. I en federerad katalog lagras lösenordshashvärden inte i hello Azure AD-katalog. Därför fungerar inte Azure AD Domain Services med sådana Azure AD-kataloger.

#### <a name="can-i-make-azure-ad-domain-services-available-in-multiple-virtual-networks-within-my-subscription"></a>Kan jag göra Azure AD Domain Services finns i flera virtuella nätverk i min prenumeration?
själva hello tjänsten stöder inte det här scenariot. Azure AD Domain Services finns i endast ett virtuellt nätverk i taget. Du kan dock konfigurera anslutningen mellan flera virtuella nätverk tooexpose Azure AD Domain Services tooother virtuella nätverk. Den här artikeln beskriver hur du kan [ansluta virtuella nätverk i Azure](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

#### <a name="can-i-enable-azure-ad-domain-services-using-powershell"></a>Kan jag aktivera Azure AD Domain Services med hjälp av PowerShell?
PowerShell/automatisk distribution av Azure AD Domain Services är inte tillgänglig för närvarande.

#### <a name="is-azure-ad-domain-services-available-in-hello-new-azure-portal"></a>Är Azure AD Domain Services i hello nya Azure-portalen?
Nej. Azure AD Domain Services kan konfigureras i hello [klassiska Azure-portalen](https://manage.windowsazure.com). Vi räknar tooextend stöd för hello [Azure-portalen](https://portal.azure.com) i hello framtida.

#### <a name="can-i-add-domain-controllers-tooan-azure-ad-domain-services-managed-domain"></a>Kan jag lägga till domänen domänkontrollanter tooan Azure AD Domain Services-hanterad domän?
Nej. hello-domän som tillhandahålls av Azure AD Domain Services är en hanterad domän. Du behöver inte tooprovision, konfigurera eller på annat sätt hantera domänkontrollanter för den här domänen - dessa management aktiviteter tillhandahålls som en tjänst av Microsoft. Därför kan du lägga till ytterligare domänkontrollanter (skrivskyddad eller skrivskyddad) för hello hanterade domän.

### <a name="administration-and-operations"></a>Administration och åtgärder
#### <a name="can-i-connect-toohello-domain-controller-for-my-managed-domain-using-remote-desktop"></a>Kan jag ansluta toohello domänkontrollant för min hanterade domänen med hjälp av fjärrskrivbord?
Nej. Du har inte behörighet tooconnect toodomain styrenheter för hello hanterade domänen via fjärrskrivbord. Medlemmar i gruppen för hello AAD DC-administratörer kan administrera hello hanterade domänen med hjälp av Administrationsverktyg för AD, till exempel hello Active Directory Administration Center (ADAC) eller AD PowerShell. Dessa installeras med hjälp av hello 'Remote Server Administration Tools-funktionen på en Windows server anslöts toohello hanterade domänen.

#### <a name="ive-enabled-azure-ad-domain-services-what-user-account-do-i-use-toodomain-join-machines-toothis-domain"></a>Jag har aktiverat Azure AD Domain Services. Vilket konto ska jag använda toodomain ansluta till datorer toothis domänen?
Medlemmar i hello administrativ grupp AAD DC-administratörer kan domänanslutning datorer. Dessutom kan beviljas medlemmar i gruppen fjärråtkomst till skrivbordet toomachines som har anslutits toohello domän.

#### <a name="do-i-have-domain-administrator-privileges-for-hello-managed-domain-provided-by-azure-ad-domain-services"></a>Måste jag domänadministratörsbehörighet för hello hanterad domän som tillhandahålls av Azure AD Domain Services?
Nej. Du har inte beviljats administratörsbehörighet på hello-hanterad domän. Både domän-administratör och företagsadministratörsbehörighet krävs behörighet är inte tillgängliga för toouse inom hello domän. Befintliga domänadministratör eller enterprise administratörsgrupper i Azure AD-katalogen också beviljas inte administratörsbehörighet för domänen/enterprise hello domän.

#### <a name="can-i-modify-group-memberships-using-ldap-or-other-ad-administrative-tools-on-managed-domains"></a>Kan jag ändra gruppmedlemskap via LDAP eller andra administrativa verktyg för AD på hanterade domäner?
Nej. Gruppmedlemskap kan inte ändras i domäner som underhålls av Azure AD Domain Services. Detsamma gäller för användarattribut hello. Du kan dock ändra gruppmedlemskap eller användarattribut i Azure AD eller på den lokala domänen. Dessa ändringar är automatiskt synkroniserade tooAzure AD DS.

#### <a name="how-long-does-it-take-for-changes-i-make-toomy-azure-ad-directory-toobe-visible-in-my-managed-domain"></a>Hur lång tid tar det innan ändringarna jag göra toomy Azure AD directory toobe synliga i min hanterade domänen?
Ändringar i Azure AD-katalogen med hello Azure AD-Gränssnittet eller PowerShell är synkroniserade tooyour hanterad domän. Den här synkroniseringsprocessen körs i hello bakgrund. När hello en inledande synkronisering av din katalog är klar kan det vanligtvis tar ungefär 20 minuter för ändringar i Azure AD toobe visas i din hanterade domän.

#### <a name="can-i-extend-hello-schema-of-hello-managed-domain-provided-by-azure-ad-domain-services"></a>Kan jag utöka hello schema hello hanterad domän som tillhandahålls av Azure AD Domain Services?
Nej. hello schemat hanteras av Microsoft för hello hanterade domän. Schematillägg stöds inte av Azure AD Domain Services.

#### <a name="can-i-modify-or-add-dns-records-in-my-managed-domain"></a>Kan jag ändra eller lägga till DNS-poster i min hanterade domänen?
Ja. Medlemmar i hello ' AAD DC-administratörsgruppen har beviljats behörighet för DNS-administratören toomodify DNS registrerar i hello-hanterad domän. De kan använda hello DNS-hanteringskonsolen på en dator kör Windows Server kopplade toohello hanterad domän, toomanage DNS. toouse hello DNS-hanteringskonsolen, installera 'DNS-serververktyg', som ingår i hello 'Remote Server Administration Tools' valfri funktion på hello-servern. Mer information om [verktyg för att administrera, övervaka och felsöka DNS](https://technet.microsoft.com/library/cc753579.aspx) finns på TechNet.

### <a name="billing-and-availability"></a>Fakturering och tillgänglighet
#### <a name="is-azure-ad-domain-services-a-paid-service"></a>Är Azure AD Domain Services en betald tjänst?
Ja. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/active-directory-ds/).

#### <a name="is-there-a-free-trial-for-hello-service"></a>Finns det en kostnadsfri utvärderingsversion för hello tjänsten?
Den här tjänsten ingår i hello kostnadsfri utvärderingsversion för Azure. Du kan registrera dig för en [kostnadsfri utvärderingsversion för en månad Azure](https://azure.microsoft.com/pricing/free-trial/).

#### <a name="can-i-get-azure-ad-domain-services-as-part-of-enterprise-mobility-suite-ems-do-i-need-azure-ad-premium-toouse-azure-ad-domain-services"></a>Kan jag Azure AD Domain Services som en del av Enterprise Mobility Suite (EMS)? Behöver jag Azure AD Premium toouse Azure AD Domain Services?
Nej. Azure AD Domain Services är inte en del av EMS är en betala per användning i Azure-tjänsten. Azure AD Domain Services kan användas med alla versioner av Azure AD (ledigt, grundläggande och, Premium). Du debiteras timme, beroende på användning.

#### <a name="what-azure-regions-is-hello-service-available-in"></a>Vilka Azure-regioner är hello tjänsten?
Se toohello [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/) sidan toosee en lista över hello Azure-regioner där Azure AD Domain Services är tillgängligt.
