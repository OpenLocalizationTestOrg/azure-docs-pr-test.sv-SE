---
title: "Azure Active Directory Domain Services: Scenarier för distribution av | Microsoft Docs"
description: "Distributionsscenarier för Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: maheshu
ms.openlocfilehash: 8a64bd8f7c6eba8f6490e10fa62bef195b6b3d5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-scenarios-and-use-cases"></a>Distributionsscenarier och användningsfall
I det här avsnittet ska titta vi på några scenarier och användningsområden som är undantagna från Azure Active Directory (AD) Domain Services.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Säker, enkel administration av virtuella Azure-datorer
Du kan använda Azure Active Directory Domain Services toomanage Azure virtuella datorer på ett effektivt sätt. Virtuella Azure-datorer kan vara domänansluten toohello hanterade domän, vilket gör att du toouse företagets Annonsen autentiseringsuppgifter toolog i. Den här metoden hjälper till att undvika autentiseringsuppgifter management problem, till exempel underhålla lokala administratörskonton på var och en av dina virtuella Azure-datorer.

Servern virtuella datorer som är kopplade toohello hanterad domän kan också hanteras och skyddas med en Grupprincip. Du kan tillämpa obligatoriska säkerhet baslinjer tooyour Azure virtuella datorer och låsa dem i enlighet med företagets riktlinjer. Exempelvis kan du använda gruppen policy management funktioner toorestrict hello typer av program som kan startas på dessa virtuella datorer.

![Effektiv administration av virtuella Azure-datorer](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Som når slutet av livslängd för servrar och annan infrastruktur, flyttar Contoso många program som finns på lokalt toohello i molnet. Deras aktuella IT-standarden kräver att servrarna som är värd för företagets program måste vara domänanslutna och hanterad med hjälp av en Grupprincip. Contosos föredrar toodomain ansluta till virtuella datorer i Azure, toomake administration lättare för IT-administratören. Detta innebär att administratörer och användare kan logga in med sina företagsuppgifter. Vid hello samtidigt datorer kan vara konfigurerade toocomply med baslinjer för krävs säkerhet med hjälp av en Grupprincip. Contoso föredrar inte toohave toodeploy, övervaka och hantera domänkontrollanter i Azure toosecure virtuella Azure-datorer. Därför Azure AD Domain Services är ett bra val för den här användningsfall.

**Distributionsanteckningar**

Överväg följande viktiga punkter för det här distributionsscenariot hello:

* Hanterade domäner som tillhandahålls av Azure AD Domain Services ger en enda flat OU (organisationsenhet)-struktur som standard. Alla domänanslutna datorer placeras i en enda flat Organisationsenhet. Du kan dock välja toocreate anpassade organisationsenheter.
* Azure AD Domain Services har stöd för enkla Grupprincip i hello form av ett inbyggda GPO varje för hello-användare och datorer behållare. Du kan skapa anpassade grupprincipobjekt och rikta dem toocustom organisationsenheter.
* Azure AD Domain Services stöder hello grundläggande AD datorn objektet schemat. Du kan inte utöka schemat hello datorobjekt.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-tooazure-infrastructure-services"></a>Lift och SKIFT ett lokalt program som använder LDAP bind autentisering tooAzure infrastrukturtjänster
![LDAP-bindning](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso har ett lokalt program som köpts från en ISV många år sedan. hello programmet är i underhållsläge hello ISV och begärda ändringar toohello programmet är orimligt hög för Contoso. Det här programmet har en klientdel som samlar in autentiseringsuppgifter med hjälp av ett webbformulär och autentiserar användare genom att utföra en LDAP-bindning toohello webbaserade företagets Active Directory. Contoso vill toomigrate det här programmet tooAzure infrastrukturtjänster. Det är önskvärt att hello programmet fungerar som är utan ändringar. Dessutom kan ska användare vara kan tooauthenticate med sina befintliga företagsuppgifter och utan att ha tooretrain användare toodo saker på olika sätt. Med andra ord slutanvändare bör vara oblivious av där programmet hello körs och hello migreringen ska vara transparent toothem.

**Distributionsanteckningar**

Överväg följande viktiga punkter för det här distributionsscenariot hello:

* Se till att programmet hello inte behöver toomodify/skrivning toohello directory. LDAP-skrivbehörighet toomanaged domäner som tillhandahålls av Azure AD Domain Services stöds inte.
* Det går inte att ändra lösenord direkt mot hello hanterade domän. Användare kan ändra sina lösenord antingen med hjälp av Azure AD självbetjäning lösenord ändra mekanism eller mot hello lokala katalog. Dessa ändringar är automatiskt synkroniserats och finns tillgängliga i hello hanterade domän.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-tooaccess-hello-directory-tooazure-infrastructure-services"></a>Lift och SKIFT ett lokalt program som använder LDAP läsa tooaccess hello directory tooAzure infrastrukturtjänster
Contoso har ett lokalt branschspecifika (LOB)-program som har utvecklats nästan tio år sedan. Det här programmet är medveten om katalog och var utformad toowork med Windows Server AD. hello programmet använder LDAP (Lightweight Directory Access Protocol) tooread information/attribut om användare från Active Directory. hello-programmet inte ändra attribut och annat skriva toohello directory. Contoso vill toomigrate det här programmet tooAzure infrastrukturtjänster och pensionera hello åldern på lokal maskinvara som för närvarande är värd för det här programmet. hello-programmet kan inte vara omskrivet toouse moderna directory API: er, till exempel hello REST-baserad Azure AD Graph API. Därför är ett alternativ för lift och SKIFT önskade där programmet hello kan vara migrerade toorun i hello moln, utan att ändra koden eller skriva om programmet hello.

**Distributionsanteckningar**

Överväg följande viktiga punkter för det här distributionsscenariot hello:

* Se till att programmet hello inte behöver toomodify/skrivning toohello directory. LDAP-skrivbehörighet toomanaged domäner som tillhandahålls av Azure AD Domain Services stöds inte.
* Se till att programmet hello inte behöver en anpassad utökat Active Directory-schemat. Schematillägg stöds inte i Azure AD Domain Services.

## <a name="migrate-an-on-premises-service-or-daemon-application-tooazure-infrastructure-services"></a>Migrera en lokal tjänst eller daemon programmet tooAzure infrastrukturtjänster
Vissa program består av flera nivåer där en av hello nivåer måste tooperform autentiserade anrop tooa backend-nivå, till exempel en databasnivå. Active Directory-tjänstkonton används ofta för dessa användningsområden. Du kan lift och SKIFT sådana program tooAzure infrastrukturtjänster och Använd Azure AD Domain Services för hello identitet måste dessa program. Du kan välja toouse hello samma konto som ska synkroniseras från din lokala katalog tooAzure AD. Alternativt kan du först skapa en anpassad Organisationsenhet och sedan skapa ett separat tjänstkonto i denna Organisationsenhet toodeploy sådana program.

![Tjänstkonto som använder WIA](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso har ett specialbyggt valvet program som innehåller en frontwebb, en SQLServer och backend FTP-servern. Windows-integrerad autentisering för tjänstkonton är används tooauthenticate hello toohello FTP frontwebbserver. hello frontwebb ställs in toorun som ett tjänstkonto. hello backend-servern är konfigurerad tooauthorize åtkomst från hello-tjänstkontot för hello frontwebb. Contoso föredrar inte toohave toodeploy en domain controller virtuell dator i hello molnet toomove det här programmet tooAzure infrastrukturtjänster. Contosos IT-administratör kan distribuera hello-servrar som hyser hello frontwebb, SQLServer och hello FTP server tooAzure virtuella datorer. De här datorerna är anslutna tooan Azure AD Domain Services hanterade domän. De kan sedan använda hello samma tjänstkonto i sina lokala kataloger för hello app autentisering. Tjänstkontot är synkroniserade toohello Azure AD Domain Services-hanterad domän och är tillgängliga för användning.

**Distributionsanteckningar**

Överväg följande viktiga punkter för det här distributionsscenariot hello:

* Kontrollera att programmet hello använder användarnamn/lösenord för autentisering. Certifikat/smartkort-baserad autentisering stöds inte av Azure AD Domain Services.
* Det går inte att ändra lösenord direkt mot hello hanterade domän. Användare kan ändra sina lösenord antingen med hjälp av Azure AD självbetjäning lösenord ändra mekanism eller mot hello lokala katalog. Dessa ändringar är automatiskt synkroniserats och finns tillgängliga i hello hanterade domän.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Windows Server Remote desktop services-distributioner i Azure
Du kan använda Azure AD Domain Services tooprovide hanteras AD DS tooyour fjärrskrivbordsservrar distribuerade i Azure.

Mer information om det här distributionsscenariot se hur för[integrera Azure AD Domain Services med RDS-distribution](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).
