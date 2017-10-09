---
title: 'Azure Active Directory Domain Services: Funktioner | Microsoft Docs'
description: Funktioner i Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8d1c3eb3-1022-4add-a919-c98cc6584af1
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 1a9417bafa35959d280eabf3db6ad8530db4ea45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services"></a>Azure AD Domain Services
## <a name="features"></a>Funktioner
hello följande funktioner är tillgängliga i Azure AD Domain Services hanterade domäner.

* **Enkel distributionsupplevelse:** du kan aktivera Azure AD Domain Services för din Azure AD-klient med bara några klickningar. Oavsett om din Azure AD-klient är en moln-klient eller synkroniseras med din lokala katalog, kan din hanterade domän etableras snabbt.
* **Stöd för domänanslutning:** kan du enkelt domänanslutning datorer i hello din hanterade domän finns i virtuella Azure-nätverket. hello domänanslutning upplevelse på Windows-klient och Server-operativsystem fungerar sömlöst mot domäner som underhålls av Azure AD Domain Services. Du kan också använda automatisk domänanslutning tooling mot dessa domäner.
* **En domän instans per Azure AD-katalog:** du kan skapa en enda Active Directory-domän för varje Azure AD-katalog.
* **Skapa domäner med egna namn:** kan du skapa domäner med anpassade namn (till exempel ”contoso100.com”) med hjälp av Azure AD Domain Services. Du kan använda antingen verifierade och overifierade domännamn. Alternativt kan du också kan skapa en domän med hello inbyggda domänsuffix (det vill säga ' *. onmicrosoft.com') erbjuds av Azure AD-katalogen.
* **Integrerade med Azure AD:** du inte behöver tooconfigure eller hantera replikering tooAzure AD DS. Användarkonton, gruppmedlemskap och autentiseringsuppgifter (lösenord) från Azure AD-katalogen är automatiskt tillgängliga i Azure AD Domain Services. Nya användare, grupper eller ändringar tooattributes från Azure AD-klienten eller din lokala katalog är automatiskt synkroniserade tooAzure AD DS.
* **NTLM och Kerberos-autentisering:** med stöd för NTLM och Kerberos-autentisering kan du distribuera program som förlitar sig på Windows-integrerad autentisering.
* **Använd företagets autentiseringsuppgifter/lösenorden:** lösenord för användare i din Azure AD-klient fungerar med Azure AD Domain Services. Användare kan använda sina företagsuppgifter toodomain koppling datorer, logga in interaktivt eller via fjärrskrivbord och autentiseras mot hello-hanterad domän.
* **LDAP-bindning & LDAP läsa stöd:** du kan använda program som förlitar sig på LDAP-bindningar tooauthenticate användare i domäner som underhålls av Azure AD Domain Services. Dessutom kan läsåtgärder program som använder LDAP tooquery användaren eller datorn attribut från hello directory kan också fungera mot Azure AD Domain Services.
* **Säkert LDAP (LDAPS):** du kan aktivera åtkomst toohello directory över säker LDAP (LDAPS). Säker är LDAP tillgängliga i hello virtuella nätverk som standard. Du kan dock även aktivera säker LDAP-åtkomst via hello internet.
* **Grupprincip:** du kan använda ett inbyggt GPO varje för hello användare och datorer behållare tooenforce kompatibilitet med nödvändiga säkerhetsprinciper för användarkonton och domänanslutna datorer. Du kan också skapa egna anpassade grupprincipobjekt och tilldela dem toocustom organisationsenheter för[hanterar Grupprincip](active-directory-ds-admin-guide-administer-group-policy.md).
* **Hantera DNS:** medlemmar i gruppen för hello AAD DC-administratörer kan hantera DNS för din hanterade domän med hjälp av välbekanta DNS-administration tools som hello DNS-Administration MMC-snapin-modulen.
* **Skapa anpassade organisationsenheter (OU):** medlemmar i gruppen för hello AAD DC-administratörer kan skapa anpassade organisationsenheter i hello-hanterad domän. Dessa användare beviljas fullständig administratörsbehörighet över anpassade organisationsenheter, så att de kan lägga till eller ta bort tjänstkonton, datorer, grupper osv. i dessa anpassade organisationsenheter.
* **Tillgängliga i Azure-regioner:** Se hello [Azure-tjänster efter region](https://azure.microsoft.com/regions/#services/) sidan tooknow hello Azure-regioner som Azure AD Domain Services är tillgängligt.
* **Hög tillgänglighet:** Azure AD Domain Services ger hög tillgänglighet för din domän. Den här funktionen ger hello garanti på högre service drifttid och återhämtning toofailures. Inbyggda hälsoövervakning erbjudanden automatiska reparationer från fel av snurrande in nya instanser tooreplace misslyckades instanser och tooprovide fortsatt service för din domän.
* **Använda välbekanta verktyg:** du kan använda välbekanta verktyg för hantering av Windows Server Active Directory, till exempel hello Active Directory Administrationscenter eller Active Directory PowerShell tooadminister hanterade domäner.
