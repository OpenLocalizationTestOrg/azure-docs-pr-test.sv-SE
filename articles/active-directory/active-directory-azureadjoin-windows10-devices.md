---
title: "Med hjälp av Windows 10-enheter på din arbetsplats | Microsoft Docs"
description: "Ger en ögonblicksbild av funktioner för användare och IT-avdelningen, kontrast olika sätt en enhet kan etableras och användas i ett företag med Windows 10."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: 94ccc8fd-b17b-4fda-8d56-9d87aa37a9f9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 451842f764898af65dd7300e8b48442d256cea7b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-windows-10-devices-in-your-workplace"></a>Med hjälp av Windows 10-enheter på din arbetsplats
Gäller för: Windows 10-datorer

Windows 10 erbjuder tre modeller för organisationer som användarna ska kunna få åtkomst till arbetsresurser på ett säkert och enkelt sätt.

* **Azure Active Directory Join** (Azure AD Join), för åtkomst till resurser, till exempel Office 365 i första hand i molnet. Azure AD-anslutning är självbetjäning arbete som är nytt i Windows 10-etablering.
* **Domänanslutning**, för organisationer som har investerat i lokala appar och resurser. Anslut till domän ger en bättre upplevelse i Windows 10 när du är ansluten till Azure AD.
* **En ny förenklad BYOD upplevelse**för användare som vill lägga till ett arbetskonto- eller skolkonto i Windows och enkelt komma åt resurser på personliga enheter.

Följande tabell innehåller en ögonblicksbild av funktioner för användare och IT-administratörer, kontrast olika sätt en enhet kan etableras och användas i ett företag med Windows 10:

|  | Anslut till domän | Azure AD-anslutning | Personlig enhet |
| --- | --- | --- | --- |
| Windows-enhetsinloggning för arbets-eller skolkonton. |Ja |Ja |Nej |
| Användare enkel inloggning (SSO) till Office 365 och Azure AD-appar. SSO är möjligheten att logga in en gång att få åtkomst till företagsresurser. |Ja |Ja |Ja |
| Användaren SSO till Kerberos/NTLM-appar. |Ja |Begränsad |Ja, via VPN |
| Stark auktoriserings- och smidig inloggning för arbetet eller skolan konton med Microsoft Passport och Windows Hello. |Ja |Ja |Ja |
| Åtkomst till Windows Store för företag med ett arbets- eller skolkonto konto (inte ett Microsoft-konto). |Ja |Ja |Ja |
| Enterprise-kompatibel roaming av användarinställningar mellan enheter med arbets-eller skolkonton. |Ja |Ja |Ja |
| Möjligheten att begränsa åtkomsten till organisationens appar till enheter som är kompatibla med principer för organisationens enheter. |Ja |Ja |Ja |
| Självbetjäning etablering av enheter för ”arbeta överallt”. |Nej |Ja |Ja |
| Möjlighet att hantera enheter. |Ja, via GP/SCCM |Ja |Ja |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Använd arbete ägda enheter till Azure AD-anslutning och domän delta i Windows 10
Windows 10 ger arbete ägda enheter får åtkomst till arbetsresurser på två sätt:

* Azure AD-anslutning
* Anslut till domän
  
  Båda kan vara giltiga alternativ beroende på organisationens behov och krav. I vissa fall kan organisationer utnyttja att aktivera båda metoderna för distribution.

## <a name="when-to-use-azure-active-directory-join"></a>När du använder Azure Active Directory Join
Azure AD-anslutning är en ny självbetjäning arbete etablering upplevelse i Windows 10.  Det är riktat mot anställda som har åtkomst till arbetsresurser som Office 365 i första hand i molnet. Det är ett enkelt sätt att konfigurera datorer, surfplattor och telefoner för företaget. Enheter som hanteras via hantering av mobila enheter med hjälp av konsekvent kontroller över Windows-plattformar.

**Använda Azure AD Join för någon av följande skäl**:

* Du vill aktivera självbetjäning etablering av enheter för anställda på språng.
* Du ge användare arbete ägda mobila enheter som surfplattor och telefoner.
* Du vill hantera en uppsättning användare i Azure AD i stället för i Active Directory, till exempel när anställda, leverantörer eller studenter.
* Vill du ange funktioner för anslutning till arbetare i fjärranslutna avdelningskontor med begränsad lokal infrastruktur.
* Du har inte en lokal Active Directory.

Vissa organisationer använder Azure AD Join som det vanligaste sättet att distribuera enheter som ägs av arbetet, särskilt när de migrera de flesta eller alla sina resurser till molnet. Hybrid organisationer med både Active Directory och Azure AD kan också välja att distribuera en metod eller ett annat beroende på användaren eller avdelning.

Regioner och universitet, till exempel kan hantera anställda i Active Directory och studenter i Azure AD. Vissa företag kanske vill hantera fjärranslutna kontor eller försäljning avdelningar i Azure AD. Både Azure AD-anslutning och domän join-metoder kan användas i en hybrid-organisation. Azure AD-anslutning kan vara en utmärkt på Domänanslutning för att distribuera enheter i en arbetsmiljö.

**Om de mest vanliga åtkomsten till företagets resurser via molnet organisationen kan få ytterligare fördelar om**:

* Du kan ta bort beroenden för lokala identitetsinfrastruktur.
* Du kan förenkla dina enheter distributionsmodell komma från lösningar för avbildning genom att tillåta Self service configuration.
* Du kan använda hantering av mobila enheter för att hantera alla dina enheter på olika plattformar.

Läs mer om Azure AD Join [utöka molnfunktioner till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>När du ska använda domänen koppling (eller fortsätta använda den)
Många organisationer har använt domänanslutning arbete enheterna ska ansluta under de senaste 15 år. Det gör det möjligt för användare att logga in på sina enheter med sitt arbete med Active Directory- eller skolkonton. Anslut till domän kan också IT att helt och centralt hantera dessa enheter. Organisationer bygger vanligtvis på avbildning metoder för att etablera enheter och ofta använda System Center Configuration Manager (SCCM) eller gruppen princip (GP) för att hantera dem.

**Ditt företag ska använda domänen koppling (eller fortsätta använda den) för någon av följande skäl**:

* Du har Win32-appar som distribueras till dessa enheter som använder NTLM/Kerberos.
* Du behöver GP eller SCCM/DCM att hantera enheter.
* Vill du fortsätta att använda avbildningsverktyg lösningar för att konfigurera enheter för dina anställda.

**Anslut till domän i Windows 10 ger dig också följande fördelar vid anslutning till Azure AD**:

* Stark autentisering av enheten-bundna och praktiskt inloggning för arbetet eller skolan konton med Microsoft Passport och Windows Hello.
* Åtkomst till enterprise Windows Store för enheter som arbets- eller skolkonton (ingen Microsoft-konto krävs).
* Enterprise-kompatibel roaming av användarinställningar över enheter som använder arbets- eller skolkonto konton (ingen Microsoft-konto krävs).
* Möjligheten att begränsa åtkomsten till organisationens appar till enheter som är kompatibla med principer för organisationens enheter.

Läs mer om Azure AD Join [utöka molnfunktioner till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Aktivera anslutning av personligt ägda enheter på arbetet eller skolan
För att stödja BYOD i företaget, kan Windows 10 användaren att lägga till ett arbets- eller skolkonto konto till användarens dator, surfplatta eller telefon. När användaren lägger till ett konto för arbetet eller skolan, är enheten registrerad med Azure AD och (valfritt) har registrerats i det mobila enhetshanteringssystemet som organisationen har konfigurerats. Katalogen visas som ”registrerad' vs dessa enheter. Azure AD ansluten. IT-administratörer kan tillämpa olika principer baserat på den här informationen att tillhandahålla en ljusare touch på personligt ägda enheter än på enheter som ägs av arbetet om så önskas.

Användare kan lägga till en arbets eller skolkonto för att deras personliga enhet mycket enkelt. De kan göra detta vid åtkomst till ett arbetsplats-program för första gången eller de kan göra det manuellt via menyn Inställningar. Det här kontot ger enkel inloggning till organisationens resurser.

Läs mer om Azure AD Join [ansluta domänanslutna enheter till Azure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Aktivera domän eller Azure AD-koppling
Alla metoder som beskrivs ovan (Anslut till domän, Azure AD-anslutning och Lägg till arbetsplats eller skola konto) har startpunkter i användargränssnittet för Windows 10. Alla kräver dock en IT-administratör att aktivera funktionerna i infrastrukturen för upplevelsen ska fungera.

## <a name="requirements-for-deploying-azure-ad-join"></a>Krav för distribution av Azure AD Join
Om du vill distribuera Azure AD Join för alla användare behöver du följande:

* En Azure AD-prenumeration.
* En Azure AD Premium-prenumeration, till exempel management auto-registrering av mobila enheter, om du behöver flera funktioner.
* Hantering av mobila enheter – till exempel en prenumeration på Microsoft Intune, hantering av mobila enheter för Office 365, eller någon av partner mobilenheter management leverantörer som integreras med Azure AD. (Se den [frågor och svar](#frequently-asked-questions) mot slutet av den här artikeln för mer information).

Om din verksamhet hybrid, rekommenderar vi att du distribuerar Azure AD Connect för att utöka den lokala katalogen till Azure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Krav för att använda domänanslutning med Azure AD
Domänanslutning fortsätter att fungera som det har alltid. Men om du vill hämta Azure AD-fördelar behöver du följande:

* En Azure AD-prenumeration.
* En distribution av Azure AD Connect att utöka lokala katalog till Azure AD.
* En princip som gör att domänanslutna enheter har villkorlig åtkomst till Azure AD.
* En princip som ger åtkomst till ”domänanslutna” enheter om du vill kunna begränsa åtkomsten för vissa enheter.
* System Center Configuration Manager version 1509 för Technical Preview att aktivera regler för att kräva kompatibla enheter. (Se TechNet-dokumentation och blogginlägget).

Mer information om domänanslutning i Windows 10 finns [ansluta domänanslutna enheter till Azure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Krav för att använda BYOD och ”Lägg till ett arbets- eller skolkonto konto”
Så här aktiverar du ”bring your own device” (BYOD) med arbets- eller skolkonto konton, behöver du följande:

* En Azure AD-prenumeration.
* En Azure AD Premium-prenumeration, till exempel management auto-registrering av mobila enheter, om du behöver flera funktioner.

## <a name="requirements-for-using-microsoft-passport"></a>Krav för att använda Microsoft Passport
Du behöver följande för att aktivera Microsoft Passport:

* En public key infrastructure (PKI) för stöd för certifikatbaserad autentisering som använder Microsoft Passport.
* En Intune-prenumeration för stöd för certifikatbaserad autentisering som använder Microsoft Passport för Azure AD Join och arbets-eller skolkonton.
* System Center Configuration Manager version 1509 för Technical Preview (se TechNet-dokumentation och blogginlägget) för stöd för certifikatbaserad autentisering som använder Microsoft Passport för domänanslutning.
* En princip för att aktivera Microsoft Passport i organisationen.

Som ett alternativ till att använda en PKI kan aktivera du nyckelbaserad Microsoft Passport genom att göra följande:

* Distribuera Windows Server 2016 ”förhandsversion 1 i produktion” domänkontrollanter (inget behov av domänens eller skogens funktionsnivåer, flera domänkontrollanter för redundans fungerar varje Active Directory-plats räcker).
* Ange princip för att aktivera Microsoft Passport i organisationen.

Mer information om Microsoft Passport och Windows Hello i Windows 10 finns i < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Vilka partner mobilenheter management produkter integrera med Azure AD?
Följande produkter leverantör integrera med Azure AD för enhetlig registrering och villkorlig åtkomst i Windows 10:

* AirWatch med VMware
* Citrix Xenmobile
* Lightspeed Mobile Manager
* SOTI lokal hantering av mobila enheter
* MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Nyheter om arbetsplatsen delta i Windows 10?
Anslut till arbetsplatsen användes i Windows 8.1 för att aktivera BYOD. I Windows 10 aktiveras BYOD via ”Lägg till ett arbets- och skolkonton som beskrivs tidigare i det här dokumentet. För organisationer som inte integrera sina hantering av mobila enheter med Azure AD, kan användare registrera enheten för hantering via **inställningar** > **konton** > **åtkomst till arbetsplats**.

### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Kan användarna ansluta sitt Microsoft-konto till domänkontot i Windows 10?
Inte i Windows 10. I Windows 8.1 kunde användare av domänanslutna enheter ”ansluta” sitt Microsoft-konto (till exempel Hotmail, Live, Outlook, Xbox, etc.) till ett domänkonto för att aktivera vissa upplevelser som SSO till Live-tjänster, användning av Windows Store och roaming av användarinställningar över enheter. Microsoft-konto ”ansluta” funktioner i Windows 10 har tagits bort. Användaren kan lägga till en eller flera Microsoft-konton som ytterligare konton för att aktivera enkel inloggning till tjänster, till exempel Windows Store. Detta görs **inställningar** > **konton** > **ditt konto**.

Användare som uppgraderar från Windows 8.1-domänanslutna enheter och vem som hade sitt Microsoft-konto ansluten och har automatiskt sitt anslutna Microsoft-konto som lagts till i listan över ytterligare konton som de använder.

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för företaget: Sätt att använda enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

