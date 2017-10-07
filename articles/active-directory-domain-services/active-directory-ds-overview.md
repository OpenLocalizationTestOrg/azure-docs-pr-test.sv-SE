---
title: aaaOverview av Azure Active Directory Domain Services | Microsoft Docs
description: "Översikt över Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 2b4884b3b8b639fcca438ddbab0bd3361aac53c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="overview"></a>Översikt
Azure Infrastructure Services aktivera toodeploy en mängd olika databehandling lösningar i ett flexibel sätt. Med Azure Virtual Machines kan du distribuera nästan omedelbart och du betalar bara för hello minut. Använda stöd för Windows, Linux, SQL Server, Oracle, IBM, SAP och BizTalk, kan du distribuera alla arbetsbelastningar, alla språk på nästan alla operativsystem. Dessa fördelar aktivera toomigrate äldre program som distribuerats lokalt tooAzure toosave på driftskostnader.

En viktig del av migreringen lokalt program tooAzure hanterar hello identitet måste dessa program. Directory-medvetna program kan förlitar sig på LDAP för Läs- eller skrivbehörighet toohello företagets katalog eller förlitar sig på Windows-integrerad autentisering (Kerberos eller NTLM-autentisering) tooauthenticate slutanvändare. Line-of-business (LOB)-program som körs på Windows Server distribueras vanligtvis på domänanslutna datorer så att de kan hanteras på ett säkert sätt med hjälp av en Grupprincip. too'lift och SKIFT ' lokala program toohello moln, måste dessa beroenden på hello företagsidentitet infrastruktur toobe löst.

Administratörer aktivera ofta tooone av hello följande lösningar toosatisfy hello identitet behoven i deras program som distribueras i Azure:

* Distribuera en plats-till-plats VPN-anslutning mellan arbetsbelastningar som körs i Azure Infrastructure Services och hello företagets katalog lokalt.
* Utöka hello företagets AD domänen/skogen infrastruktur genom att ställa in domänkontrollanter för repliken med hjälp av Azure virtuella datorer.
* Distribuera en fristående domän i Azure med hjälp av domänkontrollanter som distribueras som virtuella Azure-datorer.

Dessa metoder drabbas av hög kostnad och administrativa kostnader. Administratörer är obligatoriska toodeploy domänkontrollanter med hjälp av virtuella datorer i Azure. Dessutom kan de behöver toomanage, säker, korrigering, övervaka, säkerhetskopiering och felsöka dessa virtuella datorer. hello beroendet av VPN-anslutningar toohello lokalt directory gör arbetsbelastningar som distribuerats i Azure toobe sårbara tootransient nätverk problem eller avbrott. Dessa nätverksavbrott leda i sin tur till lägre drifttid och minskade tillförlitlighet för dessa program.

Vi har utformats Azure AD Domain Services tooprovide ett enklare alternativ.

## <a name="introducing-azure-ad-domain-services"></a>Introduktion till Azure AD Domain Services
Azure AD Domain Services tillhandahåller hanterad domäntjänster, till exempel domänanslutning, gruppen princip, LDAP, Kerberos/NTLM-autentisering som är helt kompatibel med Windows Server Active Directory. Du kan använda tjänsterna domän utan hello behöver du toodeploy, hantera och korrigering av domänkontrollanter i hello molnet. Azure AD Domain Services kan integreras med befintliga Azure AD-klienten, vilket gör det möjligt för användare toolog in med sina företagsuppgifter. Du kan dessutom använda befintliga grupper och användare konton toosecure åtkomst tooresources tillse en jämnare 'lift-och-SKIFT' för lokala resurser tooAzure infrastrukturtjänster.

Azure AD Domain Services-funktionen fungerar sömlöst oavsett om Azure AD-klienten är endast molnbaserad eller synkroniserade med din lokala Active Directory.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD Domain Services för endast molnbaserad organisationer
En molnbaserad Azure AD-klient (som ofta anges tooas ”hanterad klienter”) inte har någon lokal identitet storleken. Med andra ord finns användarkonton, lösenord och gruppmedlemskap alla interna toohello cloud - som är skapas och hanteras i Azure AD. Överväg ett ögonblick att Contoso är en molnbaserad Azure AD-klient. I följande illustration hello visas Contosos administratören har konfigurerat ett virtuellt nätverk i Azure Infrastructure Services. Program och serverarbetsbelastningar distribueras i det här virtuella nätverket i virtuella Azure-datorer. Eftersom Contoso är en molnbaserad klient, alla användaridentiteter, sina autentiseringsuppgifter och gruppmedlemskap skapas och hanteras i Azure AD.

![Azure AD Domain Services-översikt](./media/active-directory-domain-services-overview/aadds-overview.png)

Contosos IT-administratören kan aktivera Azure AD Domain Services för sina Azure AD-klient och välja toomake domäntjänster som är tillgängliga i det här virtuella nätverket. Därefter, Azure AD Domain Services tillhandahåller en hanterad domän och gör den tillgänglig i hello virtuella nätverk. Alla användarkonton, gruppmedlemskap och autentiseringsuppgifter som är tillgängliga i Contosos Azure AD-klient är också tillgängliga i den här nya domänen. Den här funktionen gör det möjligt för användare i hello organisation toosign i toohello domänen med sina företagsuppgifter - till exempel när du ansluter via fjärranslutning toodomain anslutna datorer via fjärrskrivbord. Administratörer kan etablera åtkomst tooresources i hello domänen med befintliga gruppmedlemskap. Program som distribueras i virtuella datorer på hello virtuellt nätverk kan använda funktioner som domänanslutning, LDAP-Läs-, LDAP-bindning, NTLM och Kerberos-autentisering och en Grupprincip.

Några av de viktigaste egenskaperna i för hello-hanterad domän som etableras av Azure AD DS är följande:

* Contosos IT-administratören behöver inte toomanage, korrigera eller övervaka den här domänen eller domänkontrollanter för den här hanterade domänen.
* Det finns ingen måste toomanage AD-replikering för den här domänen. Användarkonton, gruppmedlemskap och autentiseringsuppgifter från Contosos Azure AD-klient är automatiskt tillgängliga i den här hanterade domänen.
* Eftersom hello domän hanteras av Azure AD Domain Services, Contoso IT-administratören har inte behörighet för domänadministratör eller Företagsadministratörer på den här domänen.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD Domain Services för hybrid organisationer
Organisationer med en hybrid-IT-infrastruktur använda en blandning av molnresurser och lokala resurser. Dessa organisationer synkronisera identitetsinformation från sina lokala directory tootheir Azure AD-klient. Som hybrid organisationer se ut mer toomigrate i sina lokala program toohello moln, särskilt äldre katalog-medvetna program, kan Azure AD Domain Services vara användbara toothem.

Litware Corporation har distribuerat [Azure AD Connect](../active-directory/active-directory-aadconnect.md), toosynchronize identitetsinformation från sina lokala directory tootheir Azure AD-klient. hello ID-information som synkroniseras innehåller användarkonton, deras hashvärdena för autentiseringsuppgifterna för autentisering (synkronisering av lösenord) och gruppmedlemskap.

> [!NOTE]
> **Lösenordssynkronisering är obligatoriskt för hybrid organisationer toouse Azure AD Domain Services**. Det här kravet är eftersom användarnas autentiseringsuppgifter behövs i hello hanterade domän som tillhandahålls av Azure AD Domain Services, tooauthenticate dessa användare via metoder för NTLM eller Kerberos-autentisering.
>
>

![Azure AD Domain Services för Litware Corporation](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

hello visas föregående bild hur organisationer med en hybrid-IT-infrastruktur, till exempel Litware Corporation kan använda Azure AD Domain Services. Litwares program och serverarbetsbelastningar som kräver domäntjänster distribueras i ett virtuellt nätverk i Azure Infrastructure Services. Litware's IT-administratören kan aktivera Azure AD Domain Services för sina Azure AD-klient och välja toomake en hanterad domän som är tillgängliga i det här virtuella nätverket. Eftersom Litware är en organisation med en hybrid-IT-infrastruktur, är användarkonton, grupper och autentiseringsuppgifter synkroniserade tootheir Azure AD-klient från sina lokala katalog. Den här funktionen kan användare toosign i toohello domänen med sina företagsuppgifter, till exempel vid fjärranslutning toomachines anslöts toohello domänen via fjärrskrivbord. Administratörer kan etablera åtkomst tooresources i hello domänen med befintliga gruppmedlemskap. Program som distribueras i virtuella datorer på hello virtuellt nätverk kan använda funktioner som domänanslutning, LDAP-Läs-, LDAP-bindning, NTLM och Kerberos-autentisering och en Grupprincip.

Några av de viktigaste egenskaperna i för hello-hanterad domän som etableras av Azure AD DS är följande:

* hello-hanterad domän är en fristående domän. Det är inte en förlängning av Litwares lokal domän.
* Litware's IT-administratören behöver inte toomanage, korrigeringsfil eller övervaka domänkontrollanter för den här hanterade domänen.
* Det finns ingen måste toomanage AD replikering toothis domän. Användarkonton, gruppmedlemskap och autentiseringsuppgifter från Litwares lokal katalog är synkroniserad tooAzure AD via Azure AD Connect. Dessa användarkonton, gruppmedlemskap och autentiseringsuppgifter är automatiskt tillgängliga i hello hanterade domänen.
* Eftersom hello domän hanteras av Azure AD Domain Services, Litware's IT-administratören har inte behörighet för domänadministratör eller Företagsadministratörer på den här domänen.

## <a name="benefits"></a>Fördelar
Med Azure AD Domain Services kan du utnyttja hello följande fördelar:

* **Enkel** – du kan uppfylla hello identitet behov av virtuella datorer distribueras tooAzure infrastrukturtjänster med ett par enkla klick. Du inte behöver toodeploy och hantera identitetsinfrastruktur i Azure eller installationen anslutning tillbaka tooyour på lokala identitetsinfrastrukturen.
* **Integrerad** – Azure AD Domain Services är djupt integrerad med Azure AD-klienten. Du kan nu använda Azure AD som en integrerad, molnbaserad enterprise-katalog som caters toohello måste både moderna program och traditionella directory-medvetna program.
* **Kompatibel** – Azure AD DS bygger på hello beprövade klass företagsinfrastruktur av Windows Server Active Directory. Dina program kan därför inte förlita sig på en högre grad av kompatibilitet med Windows Server Active Directory-funktioner. Det finns för närvarande inte alla funktioner som är tillgängliga i Windows Server AD i Azure AD Domain Services. Dock är tillgängliga funktioner kompatibla med hello motsvarande Windows Server AD-funktioner du förlita dig på i din lokala infrastruktur. hello LDAP, utgör Kerberos, NTLM, Grupprincip och domän koppling funktioner en mogen erbjudande som har testats och förfinad över olika versioner av Windows Server.
* **Kostnadseffektiv** – du kan undvika hello infrastruktur och hanteringsserver belastningen som är associerad med att hantera identitet infrastruktur toosupport traditionella directory-medvetna program med Azure AD Domain Services. Du kan flytta dessa program tooAzure infrastrukturtjänster och dra nytta av större besparingar på driftskostnader.
