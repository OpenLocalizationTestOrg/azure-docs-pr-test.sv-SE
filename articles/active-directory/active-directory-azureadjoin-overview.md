---
title: "Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join | Microsoft Docs"
description: "En detaljerad översikt över hur Windows 10-enheter kan använda Azure AD Join och hämta registreras i Azure Active Directory."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 0cd4942f-7d54-474e-bd12-8e6764b0d42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: d3a3d3efe1c43caff3b8d2956c14e8c90d05d22b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join
## <a name="what-is-azure-active-directory-join"></a>Vad är Azure Active Directory Join?
Azure Active Directory Join (Azure AD Join) är en funktion som registrerar en företagsägd enhet i Azure Active Directory för att aktivera centraliserad hantering av enheten. Gör det möjligt för användare, till exempel anställda och studenter för att ansluta till företaget molnet via Azure Active Directory. Detta möjliggör förenklad distribution av Windows och åtkomst till organisationens appar och resurser från valfri windowsenhet, både företagsägda och personligt ägda (BYOD).

Azure AD-anslutning är avsedd för företag som är moln-första/endast molnbaserad--vanligtvis små och medelstora företag som inte har en lokal Windows Server Active Directory-infrastruktur. Namn, Azure AD-anslutning kan och ska också används av stora organisationer på enheter som är oförmögna att göra en traditionell domänanslutning (mobila enheter, till exempel), eller om en användare i första hand behöver åtkomst till Office 365 eller andra SaaS appar som är integrerade med Azure AD.

Även om traditionella domänanslutning erbjuder fortfarande den bästa lokalt inloggningen på enheter som kan ansluta till domänen, är Azure AD-anslutning lämplig för enheter som inte kan ansluta till domänen. Azure AD-anslutning lämpar sig också för att hantera användare i molnet. Detta sker med hjälp av funktioner för hantering av mobila enheter i stället för med hjälp av traditionella domän hanteringsverktyg som en Grupprincip och System Center Configuration Manager (SCCM).

![Översikt över företagets enheter och personliga enheter med lokal Active Directory och Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)

## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Varför bör företag besluta om Azure AD Join?
* **Företag som är i stort sett i molnet**: Om du har flyttat eller flyttar till en modell där du minskar dina lokala storleken och vill använda mer i molnet, Azure AD Join kan ha nytta. Du har kanske skapat Azure AD-konton manuellt eller genom att synkronisera din lokala Active Directory. Oavsett hur du har ett konto i Azure AD och du kan använda för att logga in på Windows 10. Användarna kan ansluta sina datorer till Azure AD via antingen den out-of-box experience (OOBE) eller via menyn Inställningar. Efter anslutning, kommer användarna få enkel inloggning (SSO) åtkomst till molnresurser som Office 365 i webbläsaren eller i Office-program.
* **Utbildningssyfte**: ett av de scenarier som vi talas om ofta är att utbildningssyfte har två användartyper: lärare och övrig personal och studenter. Fakultetsmedlemmar anses långsiktig medlemmar i organisationen, så att skapa lokala konton för dem. är önskvärt. Men studenter shorter-term ingår i organisationen och därför kan hanteras i Azure AD. Det innebär att directory skala kan skickas till molnet i stället för att lagras lokalt. Det innebär också att studenter kan logga in till Windows med sina Azure AD-konton och få åtkomst till resurser i Office 365, antingen i webbläsaren eller i Office-program.
* **Retail-företag**: ett annat scenario som vi har fått om från kunder är deras webbprogrammet ska hantera när anställda enkelt.  Igen, skapas vanligtvis konton för långsiktig, ständig anställda som lokala konton på domänanslutna datorer. Men när anställda är shorter-term medlemmar i organisationen, så det är klokt att hantera dem där användarlicenser kan enkelt flytta runt. Skapa dessa användarkonton i molnet med Office 365 licenser kan användare få fördelarna med att logga in i Windows och Office-program med Azure AD-kontot. Under tiden kan behålla du mer flexibelt med licenser när de lämnar.
* **Andra företag**: trots att du underhåller användare i din lokala Active Directory du kan fortfarande dra nytta av att användare att Azure AD-ansluten. Det beror Azure AD ger en förenklad anslutande upplevelse, effektiv enhetshantering, automatisk hantering av mobilenhetsregistrering och kapacitet för enkel inloggning för Azure AD och lokala resurser.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Vilka funktioner finns Azure AD Join?
Med Azure AD Join får du följande:

* **Automatisk etablering av företagsägda enheter**: med Windows 10 användare kan konfigurera en helt ny form av krymp enhet i out of box experience, utan inblandning av IT.
* **Stöd för modern formfaktorer**: Azure AD-anslutning fungerar på enheter som inte har traditionella domänen ansluta funktioner.  
* **Stöd för befintliga organisationskonton**: användarna behöver inte längre skapa och upprätthålla ett personligt Microsoft-konto för att få bästa möjliga upplevelse på utfärdas av företagets enheter som de gjorde med Windows 8. De kan använda sina befintliga arbetskonton i Azure AD i stället. I många organisationer är innebär det i praktiken att användare kan ställa in och logga in på Windows med samma autentiseringsuppgifter som de använder för åtkomst till Office 365.
* **Automatisk hantering av mobilenhetsregistrering**: enheter kan registreras automatiskt i hantering av mobila enheter när du är ansluten till Azure AD. Den här processen fungerar med Microsoft Intune och partner lösningar för mobil enhetshantering. När enhetshantering görs med Intune, kan IT-administratörer övervaka/hantera Azure AD-anslutna enheter tillsammans med domänanslutna enheter i SCCM-hanteringskonsolen.
* **Enkel inloggning till företagsresurser**: användare få enkel inloggning från skrivbordet till appar och resurser i molnet, till exempel Office 365 och tusentals business-program som förlitar sig på Azure AD för autentisering via [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Företagsägda enheter som är anslutna till Azure AD få också SSO till lokala resurser när enheten är i ett företagsnätverk och varifrån som helst när resurserna är tillgängliga den [Azure AD Application Proxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).
* **OS tillstånd centrala**: hjälpmedelsinställningar, webbplatser, Wi-Fi-lösenord och andra inställningar synkroniseras mellan företagsägda enheter utan att kräva ett personligt microsoftkonto.
* **Enterprise-redo Windows Store**: Windows Store stöder app förvärv och licensiering med Azure AD-konton. Organisationer kan volymlicens appar och göra dem tillgängliga för användarna i organisationen.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Hur fungerar olika enheter med Azure AD Join?
| Företagets enheter (ansluten till lokala domän) | Företagets enheter (ansluten till molnet) | Personlig enhet |
| --- | --- | --- |
| Användare kan logga in till Windows med autentiseringsuppgifter för arbetet (som de gör i dag). |Användarna kan logga in till Windows med autentiseringsuppgifter för arbetet som hanteras i Azure AD. Detta är relevant för enheter i tre fall: <ol><li>Organisationen har inte Active Directory lokalt (till exempel ett litet företag).</li><li>Organisationen inte skapa alla användarkonton i Active Directory (till exempel konton för studenter, konsulter eller när anställda inte skapas i Active Directory).</li><li>Organisationen har enheter som inte kan anslutas till en domän i (lokalt), som Telefoner och surfplattor som kör en Mobile SKU (till exempel en sekundär enhet till en fabrik/retail våning).</li></ol> Azure AD-anslutning har stöd för sammanfogning av företagets enheter för både hanterade och externa organisationer. |Användare logga in till Windows med sina personliga Microsoft-kontouppgifter (ingen ändring). |
| Användare har åtkomst till roaminginställningarna och enterprise Windows Store. Dessa tjänster fungerar med arbetskonton och du behöver ett personligt microsoftkonto. Detta kräver att organisationer ska kunna ansluta sina lokala Active Directory till Azure AD. |Användarna kan göra självbetjäning installationen. De kan gå igenom förstahandsvalet (FRX) via ett arbetskonto som ett alternativ till att ha IT etablera enheter, även om båda metoderna stöds. |Användare kan enkelt lägga till ett arbetskonto som hanteras i Active Directory eller Azure AD. |
| Användare har enkel inloggning kan från skrivbordet fungera appar, webbplatser och resurser – inklusive både lokala resurser och molnappar som använder Azure AD för autentisering. |Enheter som registreras automatiskt i enterprise-katalogen (Azure AD) och registreras automatiskt i hantering av mobila enheter. (Azure AD Premium-funktion). |Användare har möjligheten för enkel inloggning mellan appar och webbplatser/resurser med det här arbetskontot. |
| Användare kan lägga till sina personliga Microsoft-konton för att komma åt sina egna bilder och filer utan att påverka företagsdata. (Inställningar för roaming fortsätta att arbeta med sitt arbetskonto.) Microsoft-konto kan SSO och inte längre enheter roaming för inställningar. |Användare kan göra en lösenordsåterställning via självbetjäning (SSPR) på winlogon, vilket innebär att de kan återställa ett nytt lösenord. (Azure AD Premium-funktion). |Användare har åtkomst till Windows Store för företag så att de kan hämta och använda av branschspecifika appar på sina personliga enheter. |

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för företaget: Sätt att använda enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

