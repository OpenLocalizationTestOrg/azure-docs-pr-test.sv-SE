---
title: aaaExtending moln funktioner tooWindows 10 enheter via Azure Active Directory Join | Microsoft Docs
description: "En detaljerad översikt över hur Windows 10-enheter kan använda Azure AD Join tooget registrerad på Azure Active Directory."
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
ms.openlocfilehash: db9ae9caeb3951d1fdd1d2477827012fd10ace60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="extending-cloud-capabilities-toowindows-10-devices-through-azure-active-directory-join"></a>Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join
## <a name="what-is-azure-active-directory-join"></a>Vad är Azure Active Directory Join?
Azure Active Directory Join (Azure AD Join) är hello-funktion som registrerar en företagsägd enhet i Azure Active Directory tooenable centraliserad hantering av hello enhet. Gör det möjligt för användare, till exempel anställda och studenter tooconnect toohello enterprise moln via Azure Active Directory. Detta möjliggör förenklad distribution av Windows- och åtkomst tooorganizational appar och resurser från valfri windowsenhet, både företagsägda och personligt ägda (BYOD).

Azure AD-anslutning är avsedd för företag som är moln-första/endast molnbaserad--vanligtvis små och medelstora företag som inte har en lokal Windows Server Active Directory-infrastruktur. Att dessa, Azure AD-anslutning kan och kommer också att användas av stora organisationer på enheter som är oförmögna att göra en traditionell domänanslutning (mobila enheter, till exempel), eller för användare som behöver främst tooaccess Office 365 eller andra SaaS-appar som är integrerade med Azure AD.

Även om hello traditionella domänanslutning erbjuder fortfarande hello bästa lokala inloggningen på enheter som kan ansluta till domänen, Azure AD-anslutning är lämplig för enheter som inte kan ansluta till domänen. Azure AD-anslutning lämpar sig också för att hantera användare i hello molnet. Detta sker med hjälp av funktioner för hantering av mobila enheter i stället för med hjälp av traditionella domän hanteringsverktyg som en Grupprincip och System Center Configuration Manager (SCCM).

![Översikt över företagets enheter och personliga enheter med lokal Active Directory och Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)

## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Varför bör företag besluta om Azure AD Join?
* **Företag som är i stort sett i hello molnet**: Om du har flyttat eller flyttar tooa modell där du minskar dina lokala storleken och vill toooperate mer i molnet hello Azure AD Join kan ha nytta. Du har kanske skapat Azure AD-konton manuellt eller genom att synkronisera din lokala Active Directory. Oavsett hur du har ett konto i Azure AD och du kan använda den toosign i tooWindows 10. Användarna kan ansluta till sina datorer tooAzure AD via antingen hello out of box experience (OOBE) eller hello inställningsmenyn. Efter anslutning, kommer användarna få enkel inloggning (SSO) åtkomst toocloud resurser som Office 365 i webbläsaren eller i Office-program.
* **Utbildningssyfte**: ett av scenarierna för hello vi talas om ofta är att utbildningssyfte har två användartyper: lärare och övrig personal och studenter. Fakultetsmedlemmar anses vara långsiktig medlemmar av hello organisation, så att skapa lokala konton för dem. är önskvärt. Men studenter tillhör shorter-term hello organisation och därför kan hanteras i Azure AD. Det innebär att directory skala flyttas toohello moln i stället för att lagras lokalt. Det innebär också att studenter kan logga in tooWindows med sina Azure AD-konton och få åtkomst tooOffice 365 resurser, antingen i webbläsaren eller i Office-program.
* **Retail-företag**: ett annat scenario som vi har fått om från kunder är sina önskan toomanage när anställda enkelt.  Igen, skapas vanligtvis konton för långsiktig, ständig anställda som lokala konton på domänanslutna datorer. Men när anställda tillhör shorter-term hello org så att den är önskvärt toomanage dem där användarlicenser kan enkelt flytta runt. Skapa dessa användarkonton i hello moln med Office 365 licenser kan hello användare tooget hello fördelarna med att logga in tooWindows och Office-program med Azure AD-kontot. Under tiden kan behålla du mer flexibelt med licenser när de lämnar.
* **Andra företag**: trots att du underhåller användare i din lokala Active Directory du kan fortfarande dra nytta av att användare att Azure AD-ansluten. Det beror Azure AD ger en förenklad anslutande upplevelse, effektiv enhetshantering, automatisk hantering av mobilenhetsregistrering och kapacitet för enkel inloggning för Azure AD och lokala resurser.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Vilka funktioner finns Azure AD Join?
Med Azure AD Join får du hello följande:

* **Automatisk etablering av företagsägda enheter**: med Windows 10 användare kan konfigurera en helt ny form av krymp enhet i hello out of box experience utan inblandning av IT.
* **Stöd för modern formfaktorer**: Azure AD-anslutning fungerar på enheter som inte har hello traditionella domän ansluta funktioner.  
* **Stöd för befintliga organisationskonton**: användare inte längre behöver toocreate och upprätthålla en en personlig Microsoft-konto tooget hello bästa möjliga upplevelse på utfärdas av företagets enheter som de gjorde med Windows 8. De kan använda sina befintliga arbetskonton i Azure AD i stället. I många organisationer detta innebär att användare kan ställa in och logga in tooWindows med hello samma autentiseringsuppgifter de använder tooaccess Office 365.
* **Automatisk hantering av mobilenhetsregistrering**: enheter kan registreras automatiskt i hantering av mobila enheter när du är ansluten tooAzure AD. Den här processen fungerar med Microsoft Intune och partner lösningar för mobil enhetshantering. När enhetshantering görs med Intune, kan IT-administratörer övervaka/hantera Azure AD-anslutna enheter tillsammans med domänanslutna enheter hello SCCM-hanteringskonsolen.
* **Enkel inloggning toocompany resurser**: användare få enkel inloggning från hello Windows skrivbord tooapps och resurser i hello molnet, till exempel Office 365 och tusentals business-program som förlitar sig på Azure AD för autentisering via [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Företagsägda enheter som är kopplade tooAzure AD också få SSO tooon lokala resurser när hello enheten är i ett företagsnätverk och varifrån som helst när resurserna är tillgängliga via hello [Azure AD Application Proxy](https://msdn.microsoft.com/library/azure/Dn768219.aspx).
* **OS tillstånd centrala**: hjälpmedelsinställningar, webbplatser, Wi-Fi-lösenord och andra inställningar synkroniseras mellan företagsägda enheter utan att kräva ett personligt microsoftkonto.
* **Enterprise-redo Windows Store**: hello Windows Store stöder app förvärv och licensiering med Azure AD-konton. Organisationer kan volymlicens appar och göra dem tillgängliga toohello användare i organisationen.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Hur fungerar olika enheter med Azure AD Join?
| Företagets enheter (kopplade tooon lokal domän) | Företagets enheter (kopplade toohello moln) | Personlig enhet |
| --- | --- | --- |
| Användare kan logga in till Windows med autentiseringsuppgifter för arbetet (som de gör i dag). |Användarna kan logga in tooWindows med autentiseringsuppgifter för arbetet som hanteras i Azure AD. Detta är relevant för enheter i tre fall: <ol><li>hello organisation har inte Active Directory lokalt (till exempel ett litet företag).</li><li>hello organisation inte skapa alla användarkonton i Active Directory (till exempel konton för studenter, konsulter eller när anställda inte skapas i Active Directory).</li><li>hello organisation har enheter som inte kan vara domänansluten tooan (lokalt)-domän som Telefoner och surfplattor som kör en Mobile SKU (till exempel en sekundär enhet tas tooa factory/retail våning).</li></ol> Azure AD-anslutning har stöd för sammanfogning av företagets enheter för både hanterade och externa organisationer. |Användare loggar in tooWindows med sina personliga Microsoft-kontouppgifter (ingen ändring). |
| Användare har åtkomst tooroaming inställningar och hello enterprise Windows Store. Dessa tjänster fungerar med arbetskonton och du behöver ett personligt microsoftkonto. Detta kräver organisationer tooconnect sina lokala Active Directory tooAzure AD. |Användarna kan göra självbetjäning installationen. De kan gå igenom hello förstahandsvalet (FRX) via ett arbetskonto som ett alternativt toohaving IT etablera hello enheter, även om båda metoderna stöds. |Användare kan enkelt lägga till ett arbetskonto som hanteras i Active Directory eller Azure AD. |
| Användare har SSO möjlighet från hello skrivbord toowork appar, webbplatser och resurser – inklusive både lokala resurser och molnappar som använder Azure AD för autentisering. |Enheter registreras automatiskt i hello enterprise directory (Azure AD) och registreras automatiskt i hantering av mobila enheter. (Azure AD Premium-funktion). |Användare har möjligheten för enkel inloggning mellan appar och toowebsites/resurser med det här arbetskontot. |
| Användare kan du lägga till sina personliga Microsoft-konton tooaccess sina egna bilder och filer utan att påverka företagsdata. (Inställningar för roaming fortsätter toowork med sitt arbetskonto.) hello Microsoft-konto aktiverar enkel inloggning och inte längre enheter hello roaming för inställningar. |Användare kan göra en lösenordsåterställning via självbetjäning (SSPR) på winlogon, vilket innebär att de kan återställa ett nytt lösenord. (Azure AD Premium-funktion). |Användare har åtkomst till toohello enterprise Windows Store, så att de kan hämta och använda av branschspecifika appar på sina personliga enheter. |

## <a name="additional-information"></a>Ytterligare information
* [Windows 10 för hello företaget: sätt toouse enheter för arbete](active-directory-azureadjoin-windows10-devices-overview.md)
* [Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join](active-directory-azureadjoin-user-upgrade.md)
* [Autentisera identiteter utan lösenord via Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Läs mer om användningsscenarier för Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurera Azure AD Join](active-directory-azureadjoin-setup.md)

