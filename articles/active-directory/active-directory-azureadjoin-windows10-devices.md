---
title: aaaUsing Windows 10-enheter i din arbetsplats | Microsoft Docs
description: "Ger en ögonblicksbild av funktioner för användare och IT-avdelningen, kontrast hello olika sätt en enhet kan etableras och användas i ett företag med Windows 10."
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
ms.openlocfilehash: 8767e1649ced8737d20875f425c24198dcaa7f6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-10-devices-in-your-workplace"></a>Med hjälp av Windows 10-enheter på din arbetsplats
Gäller för: Windows 10-datorer

Windows 10 erbjuder tre modeller för organisationer som gör att användare tooaccess arbetsresurser på ett säkert och enkelt sätt.

* **Azure Active Directory Join** (Azure AD Join), för åtkomst till resurser, till exempel Office 365 i första hand i hello molnet. Azure AD-anslutning är självbetjäning arbete som är nytt i Windows 10-etablering.
* **Domänanslutning**, för organisationer som har investerat i lokala appar och resurser. Domänanslutning ger en bättre upplevelse i Windows 10 när ansluten tooAzure AD.
* **En ny förenklad BYOD upplevelse**för användare som vill tooadd ett arbets-kontot eller skola tooWindows och enkelt komma åt resurser på personliga enheter.

hello innehåller följande tabell en ögonblicksbild av funktioner för användare och IT-administratörer, kontrast hello olika sätt en enhet kan etableras och användas i ett företag med Windows 10:

|  | Anslut till domän | Azure AD-anslutning | Personlig enhet |
| --- | --- | --- | --- |
| Windows-enhetsinloggning för arbets-eller skolkonton. |Ja |Ja |Nej |
| Användare enkel inloggning (SSO) tooOffice 365 och Azure AD-appar. SSO är hello möjlighet toosign i en gång tooaccess organisations resurser. |Ja |Ja |Ja |
| Användaren SSO tooKerberos/NTLM-appar. |Ja |Begränsad |Ja, via VPN |
| Stark auktoriserings- och smidig inloggning för arbetet eller skolan konton med Microsoft Passport och Windows Hello. |Ja |Ja |Ja |
| Komma åt tooenterprise Windows Store med ett arbets- eller skolkonto konto (inte ett Microsoft-konto). |Ja |Ja |Ja |
| Enterprise-kompatibel roaming av användarinställningar mellan enheter med arbets-eller skolkonton. |Ja |Ja |Ja |
| hello möjlighet toorestrict åtkomst tooorganizational appar toodevices som är kompatibla med principer för organisationens enheter. |Ja |Ja |Ja |
| Självbetjäning etablering av enheter för ”arbeta överallt”. |Nej |Ja |Ja |
| Möjlighet toomanage enheter. |Ja, via GP/SCCM |Ja |Ja |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Använd arbete ägda enheter till Azure AD-anslutning och domän delta i Windows 10
Windows 10 innehåller enheter som ägs av arbetet tooaccess arbetsresurser på två sätt:

* Azure AD-anslutning
* Anslut till domän
  
  Båda kan vara giltiga alternativ beroende på organisationens behov och krav. I vissa fall kan organisationer utnyttja att aktivera båda metoderna för distribution.

## <a name="when-toouse-azure-active-directory-join"></a>När toouse Azure Active Directory Join
Azure AD-anslutning är en ny självbetjäning arbete etablering upplevelse i Windows 10.  Det är riktat mot anställda som har åtkomst till arbetsresurser som Office 365 i första hand i hello molnet. Det är ett enkelt sätt tooconfigure datorer, pekdatorer, och telefoner för hello enterprise. Enheter som hanteras via hantering av mobila enheter med hjälp av konsekvent kontroller över Windows-plattformar.

**Använda Azure AD Join för någon av följande skäl**:

* Vill du tooenable hello självbetjäning etablering av enheter för anställda på hello gå.
* Du ge användare arbete ägda mobila enheter som surfplattor och telefoner.
* Du toomanage en uppsättning användare i Azure AD i stället för i Active Directory, till exempel när anställda, leverantörer eller studenter.
* Vill du tooprovide anslutande funktioner tooworkers i fjärranslutna avdelningskontor med begränsad lokal infrastruktur.
* Du har inte en lokal Active Directory.

Vissa organisationer använder Azure AD Join som hello primära sätt toodeploy arbete ägda enheter, särskilt när de migreras de flesta eller alla sina resurser toohello moln. Hybrid organisationer med både Active Directory och Azure AD kan också välja toodeploy en metod eller ett annat beroende på hello användar- eller avdelning.

Regioner och universitet, till exempel kan hantera anställda i Active Directory och studenter i Azure AD. Vissa företag kanske vill toomanage fjärranslutna kontor eller försäljning avdelningar i Azure AD. Både Azure AD-anslutning och domän join-metoder kan användas i en hybrid-organisation. Azure AD-anslutning kan vara en utmärkt toodomain koppling för att distribuera enheter i en arbetsmiljö.

**Om hello mest vanliga åtkomst tooenterprise resurser via hello molnet organisationen kan få ytterligare fördelar om**:

* Du kan ta bort beroenden för lokala identitetsinfrastruktur.
* Du kan förenkla dina enheter distributionsmodell komma från lösningar för avbildning genom att tillåta Self service configuration.
* Du kan använda toomanage för hantering av mobila enheter alla enheter på olika plattformar.

Läs mer om Azure AD Join [utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="when-toouse-domain-join-or-keep-using-it"></a>När toouse domänanslutning (eller fortsätta att använda den)
För hello har senaste 15 år många organisationer använt domän ansluta tooconnect arbete enheter. Det gör det möjligt för användare toosign i tootheir enheter med deras Active Directory arbets- eller skolkonton. Anslut till domän kan också IT toocentrally och hantera dessa enheter. Organisationer bygger vanligtvis på imaging metoder tooprovision enheter och ofta använda System Center Configuration Manager (SCCM) eller gruppen princip (GP) toomanage dem.

**Ditt företag ska använda domänen koppling (eller fortsätta använda den) för någon av följande skäl**:

* Du har Win32-appar distribuerade toothese enheter som använder NTLM/Kerberos.
* Du behöver GP eller SCCM/DCM toomanage enheter.
* Du vill toocontinue toouse avbildning lösningar tooconfigure enheter för dina anställda.

**Anslut till domän i Windows 10 ger dig även hello följande fördelar när ansluten tooAzure AD**:

* Stark autentisering av enheten-bundna och praktiskt inloggning för arbetet eller skolan konton med Microsoft Passport och Windows Hello.
* Komma åt toohello enterprise Windows Store för enheter som använder arbete eller skola konton (ingen Microsoft-konto krävs).
* Enterprise-kompatibel roaming av användarinställningar över enheter som använder arbets- eller skolkonto konton (ingen Microsoft-konto krävs).
* hello möjlighet toorestrict åtkomst tooorganizational appar toodevices som är kompatibla med principer för organisationens enheter.

Läs mer om Azure AD Join [utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Aktivera anslutning av personligt ägda enheter på arbetet eller skolan
toosupport BYOD i hello enterprise, Windows 10 ger hello användaren hello möjlighet tooadd en arbets- eller Skol-konto tootheir dator-, tablet- eller telefon. Efter hello användare lägger till ett arbets eller skolkonto hello enheten registrerad med Azure AD och du kan också registreras i hanteringssystem för hello mobila enheter som har konfigurerats hello i organisationen. hello directory visas som ”registrerad' jämfört dessa enheter. Azure AD ansluten. IT-administratörer kan tillämpa olika principer baserat på den här informationen att tillhandahålla en ljusare touch på personligt ägda enheter än på enheter som ägs av arbetet om så önskas.

Användare kan lägga till ett arbetsplats eller skola konto tootheir personlig enhet mycket enkelt. De kan göra detta vid åtkomst till ett arbetsplats-program för hello första gången eller de kan göra det manuellt via hello inställningsmenyn. Det här kontot kommer att tillhandahålla enkel inloggning tooorganizational resurser.

Läs mer om Azure AD Join [ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Aktivera domän eller Azure AD-koppling
Alla metoder som beskrivs ovan (Anslut till domän, Azure AD-anslutning och Lägg till arbetsplats eller skola konto) har startpunkter hello Windows 10 användarupplevelsen. Alla kräver dock en IT-administratören tooenable hello funktioner i hello infrastruktur hello upplevelse ska fungera.

## <a name="requirements-for-deploying-azure-ad-join"></a>Krav för distribution av Azure AD Join
toodeploy Azure AD Join för alla användare måste hello följande:

* En Azure AD-prenumeration.
* En Azure AD Premium-prenumeration, till exempel management auto-registrering av mobila enheter, om du behöver flera funktioner.
* Hantering av mobila enheter – till exempel en prenumeration på Microsoft Intune, hantering av mobila enheter för Office 365, eller någon av hello partner mobilenheter management leverantörer som integreras med Azure AD. (Se hello [frågor och svar](#frequently-asked-questions) nära hello slutet av den här artikeln för mer information).

Om din verksamhet hybrid, rekommenderar vi att du distribuerar Azure AD Connect tooextend hello lokalt directory tooAzure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Krav för att använda domänanslutning med Azure AD
Domänanslutning fortsätter toowork eftersom den har alltid. Dock tooget hello Azure AD fördelar måste hello följande:

* En Azure AD-prenumeration.
* En distribution av Azure AD Connect tooextend hello lokalt directory tooAzure AD.
* En princip som gör att domänanslutna enheter toohave villkorlig åtkomst tooAzure AD.
* En princip som tillåter åtkomst för ”domänanslutna” enheter om du vill toobe kan toorestrict åtkomst för vissa enheter.
* System Center Configuration Manager version 1509 för Technical Preview tooenable regler för att kräva kompatibla enheter. (Se hello TechNet-dokumentation och blogginlägget).

Mer information om domänanslutning i Windows 10 finns [ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Krav för att använda BYOD och ”Lägg till ett arbets- eller skolkonto konto”
tooenable ”bring your own device” (BYOD) med arbets- eller skolkonto konton, behöver du hello följande:

* En Azure AD-prenumeration.
* En Azure AD Premium-prenumeration, till exempel management auto-registrering av mobila enheter, om du behöver flera funktioner.

## <a name="requirements-for-using-microsoft-passport"></a>Krav för att använda Microsoft Passport
tooenable Microsoft Passport behöver hello följande:

* En public key infrastructure (PKI) för stöd för certifikatbaserad autentisering som använder Microsoft Passport.
* En Intune-prenumeration för stöd för certifikatbaserad autentisering som använder Microsoft Passport för Azure AD Join och arbets-eller skolkonton.
* System Center Configuration Manager version 1509 för Technical Preview (se hello TechNet-dokumentation och blogginlägget) för stöd för certifikatbaserad autentisering som använder Microsoft Passport för domänanslutning.
* En princip för att aktivera Microsoft Passport i hello organisation.

Som ett alternativ toousing en PKI, kan du aktivera nyckelbaserad Microsoft Passport hello följande:

* Distribuera Windows Server 2016 ”förhandsversion 1 i produktion” domänkontrollanter (inget behov av domänens eller skogens funktionsnivåer, flera domänkontrollanter för redundans fungerar varje Active Directory-plats räcker).
* Ange princip tooenable Microsoft Passport i hello organisation.

Mer information om Microsoft Passport och Windows Hello i Windows 10 finns i < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Vilka partner mobilenheter management produkter integrera med Azure AD?
hello integrera följande leverantör produkter med Azure AD för enhetlig registrering och villkorlig åtkomst i Windows 10:

* AirWatch med VMware
* Citrix Xenmobile
* Lightspeed Mobile Manager
* SOTI lokal hantering av mobila enheter
* MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Nyheter om arbetsplatsen delta i Windows 10?
Anslut till arbetsplatsen användes i Windows 8.1 tooenable BYOD. I Windows 10 aktiveras BYOD via ”Lägg till ett arbets- och skolkonton som beskrivs tidigare i det här dokumentet. För organisationer som inte integrera sina hantering av mobila enheter med Azure AD kan användarna kan registrera hello enheter för hantering via **inställningar** > **konton**  >  **Åtkomst till arbetsplats**.

### <a name="can-users-connect-their-microsoft-account-tootheir-domain-account-in-windows-10"></a>Användare kan ansluta sitt Microsoft-konto tootheir domän-konto i Windows 10?
Inte i Windows 10. I Windows 8.1 kan användarna för domänanslutna enheter ”ansluta” sitt Microsoft-konto (till exempel Hotmail, Live, Outlook, Xbox, etc.) tootheir domän konto tooenable vissa upplevelser som SSO tooLive tjänster, användning av hello Windows Store och roaming för Användarinställningar över enheter. Hello Microsoft-konto ”ansluta” funktioner i Windows 10 har tagits bort. hello-användare kan lägga till en eller flera Microsoft-konton som ytterligare konton tooenable SSO tooconsumer tjänster, till exempel hello Windows Store. Detta görs **inställningar** > **konton** > **ditt konto**.

Användare som uppgraderar från Windows 8.1-domänanslutna enheter och som har sitt Microsoft-konto ansluta, kommer automatiskt har sitt anslutna Microsoft-konto läggas till toohello lista över ytterligare konton som de använder.

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

