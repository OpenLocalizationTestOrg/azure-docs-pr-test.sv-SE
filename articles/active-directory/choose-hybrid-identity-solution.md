---
title: "aaaChoose ett Azure hybrididentitetslösning | Microsoft Docs"
description: "Hämta en grundläggande förståelse för hello tillgängliga hybrididentitetslösningar och rekommendationer för du toomake hello bästa identitet styrning beslut för din organisation."
keywords: 
author: jeffgilb
manager: femila
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/5/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 4ab8aff314f6308ab32f77e81cf0f07e1f5d7b41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-hybrid-identity-solutions"></a>Microsoft hybrididentitetslösningar
[Microsoft Azure Active Directory (AD Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) hybrididentitetslösningar aktivera toosynchronize lokala katalogobjekt med Azure AD när fortfarande hantera användare lokalt. hello beslut toomake först när du planerar toosynchronize din lokala Windows Server Active Directory med Azure AD är om du vill toouse synkroniseras identitet eller federerad identitet. Synkroniserade identiteter och eventuellt lösenordshashvärden, ge användarna toouse hello samma lösenord tooaccess både lokala och molnbaserade företagsresurser. För mer avancerade krav för scenarier, till exempel enkel inloggning (SSO) eller lokalt MFA måste toodeploy Active Directory Federation Services (AD FS) toofederate identiteter. 

Det finns flera alternativ för att konfigurera hybrididentitet. Den här artikeln innehåller information toohelp du väljer hello som passar din organisation baserat på enkel distribution bäst och måste din specifika identitets- och åtkomsthantering. När du planerar vilka identitetsmodellen som bäst passar organisationens behov, måste du också toothink om tid, befintlig infrastruktur, komplexiteten och kostnaden. Dessa faktorer är olika för varje organisation och kan ändras med tiden. Men om dina krav gör, har du också hello flexibilitet tooswitch tooa annan identitetsmodellen.

> [!TIP]
> Dessa lösningar levereras av [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="synchronized-identity"></a>Synkroniserade identiteter 
Synkroniserade identitet är hello enklaste sättet toosynchronize lokala katalogobjekt (användare och grupper) med Azure AD. 

![Synkroniserade hybrididentitet](./media/choose-hybrid-identity-solution/synchronized-identity.png)

Synkroniserade identitet är hello enklaste och snabbaste metoden, måste användarna fortfarande toomaintain separata lösenord för molnbaserade resurser. tooavoid, du kan också (valfritt) [synkronisera en hash av användarlösenord](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-synchronization#what-is-password-synchronization) tooyour Azure AD-katalog. Synkronisering av lösenord hashvärden låter användare toolog i toocloud-baserade organisationsresurser med hello samma användarnamn och lösenord som de använder lokalt. Azure AD Connect kontrollerar regelbundet din lokala katalog för ändringar och behåller synkroniserad Azure AD-katalogen. När en användarattribut eller lösenordet är ändrade lokala Active Directory, uppdateras den automatiskt i Azure AD. 

Hello standardalternativet lösenord synkronisering rekommenderas för de flesta organisationer som bara behöver tooenable sina användare toosign i tooOffice 365, SaaS-program och andra Azure AD-baserade resurser. Om det inte fungerar för dig, behöver du toodecide mellan direktautentisering och AD FS.

> [!TIP]
> Lösenord lagras i lokala Windows Server Active Directory i hello form av ett hash-värde som representerar hello faktiska användarens lösenord. Ett hash-värde är ett resultat av en enkelriktad matematisk funktion (hello hash-algoritm). Det finns ingen metod toorevert hello resultatet av en envägs funktion toohello oformaterad Textversion av ett lösenord. Du kan inte använda ett lösenord hash toosign tooyour lokalt nätverk. När du väljer toosynchronize lösenord Azure AD Connect extrakt lösenordshashvärden från hello lokala Active Directory och extra säkerhet som bearbetar toohello lösenords-hash innan den är synkroniserade tooAzure AD. Lösenordssynkronisering kan också användas tillsammans med lösenord återskrivning tooenable Självbetjäning för återställning av lösenord i Azure AD. Dessutom kan du aktivera enkel inloggning (SSO) för användare på domänanslutna datorer som är anslutna toohello företagsnätverket. Med enkel inloggning behöver aktiverade användare bara tooenter en användarnamn toosecurely åtkomst till molnresurser. 

## <a name="pass-through-authentication"></a>Direkt-autentisering
[Azure AD-direktautentisering](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication) är en lösning för verifiering av enkla lösenord för Azure AD-baserade tjänster som använder din lokala Active Directory. Om säkerhet och principer för efterlevnad för din organisation inte tillåter att skicka användarnas lösenord, även i ett hashformaterats formulär och behöver du bara toosupport desktop SSO för domänanslutna enheter, bör du utvärdera med hjälp av direktautentisering. Direkt-autentisering kräver inte en distribution i hello DMZ, vilket förenklar hello distribution av infrastruktur jämfört med AD FS. När användare loggar in med Azure AD, verifierar den här autentiseringsmetoden användarnas lösenord direkt mot din lokala Active Directory.

![Direkt-autentisering](./media/choose-hybrid-identity-solution/pass-through-authentication.png)

Det behövs ingen en komplex nätverksinfrastruktur med direktautentisering, och du behöver inte toostore lokala lösenord i molnet hello. I kombination med enkel inloggning, direktautentisering ger en helt integrerad upplevelse när du loggar in tooAzure AD eller andra molntjänster.

Direkt-autentisering har konfigurerats med Azure AD Connect, som använder en enkel lokal agent som lyssnar efter förfrågningar för verifiering av lösenord. hello-agenten kan enkelt distribuerade toomultiple datorer tooprovide hög tillgänglighet och belastningsutjämning. Eftersom all kommunikation är endast utgående, krävs inte för hello connector toobe installeras i en DMZ. hello krav på serverdatorn för hello connector är följande:

- Windows Server 2012 R2 eller senare
- Ansluten tooa domän i hello skog som användare verifieras

> [!NOTE]
> Azure AD-direktautentisering är för närvarande under förhandsgranskning och stöds för web webbläsarbaserad och Office-klienter som har stöd för modern autentisering. För klienter som inte stöds, till exempel äldre Office-klienter och Exchange ActiveSync (inklusive interna e-klienter på mobila enheter), är det rekommenderade toouse hello modern autentisering motsvarande. Modern autentisering inte bara gör direktautentisering, men kan även användas för villkorlig åtkomst principer toobe tillämpas, till exempel multifaktorautentisering. 

Direkt-autentisering stöds inte för närvarande när med hjälp av Windows 10-enheter anslutna tooAzure AD. Men du kan använda synkronisering av lösenords-hash som automatisk återställning toosupport Windows 10 och tidigare nämnts hello äldre klienter. Hello förhandsversionen är lösenord hash-synkronisering aktiverad som standard när direktautentisering alternativet är markerat som hello inloggning i Azure AD Connect.


## <a name="federated-identity-ad-fs"></a>Federerade identiteter (AD FS)
För mer kontroll över hur användare kommer åt Office 365 och andra molntjänster som du kan ställa in katalogsynkronisering med enkel inloggning (SSO) [Active Directory Federation Services (AD FS)](https://docs.microsoft.com/windows-server/identity/ad-fs/overview/whats-new-active-directory-federation-services-windows-server-2016). Federering användarens inloggningar med AD FS delegater autentisering tooan lokal server som validerar användarens autentiseringsuppgifter. I den här modellen skickas lokala Active Directory-autentiseringsuppgifter aldrig tooAzure AD.

![Federerade identiteter](./media/choose-hybrid-identity-solution/federated-identity.png)

Kallas även identitetsfederation, den här inloggningsmetod säkerställer att alla användarautentisering är kontrollerade lokalt och gör att administratörer tooimplement rigorösa nivåer för åtkomstkontroll. Identitetsfederation med AD FS är alternativet hello mest komplicerade och kräver distribution av ytterligare servrar i din lokala miljö. Identitetsfederation genomför också tooproviding 24 x 7 stöd för Active Directory och AD FS-infrastrukturen. Den här hög nivå av stöd är nödvändigt eftersom om din lokala Internet-, domänkontrollant, eller AD FS-servrar är tillgängliga, användare kan inte logga in toocloud tjänster.

> [!TIP]
> Om du väljer toouse Federation med Active Directory Federation Services (AD FS), kan du om du vill ställa in synkronisering av lösenord som en säkerhetskopia om det inte går att AD FS-infrastrukturen.


## <a name="common-scenarios-and-recommendations"></a>Vanliga scenarier och rekommendationer
Här följer några vanliga hybrididentitet och scenarier med åtkomst med rekommendationer som toowhich hybridalternativet identitet (eller alternativ) kan vara lämpligt för varje.

|Jag behöver:|PWS och enkel inloggning<sup>1</sup>| Tereftalsyra och enkel inloggning<sup>2</sup> | AD FS<sup>3</sup>|
|-----|-----|-----|-----|
|Synkronisera nya, kontakt, grupp- och användarkonton skapas automatiskt i min lokala Active Directory toohello molnet.|![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)| ![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png) |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|
|Konfigurera min klient för hybridscenarion för Office 365|![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)| ![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png) |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|
|Aktivera Mina användare toosign i och åtkomst till molntjänster med hjälp av sina lokala lösenord|![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)| ![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png) |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|
|Implementera enkel inloggning med företagets autentiseringsuppgifter|![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)| ![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png) |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|
|Se till att inga lösenordshashvärden lagras i hello moln| |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|
|Aktivera lokal multifaktorautentisering lösningar| | |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|
|Stöd för autentisering med smartkort för Mina användare<sup>4</sup>| | |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|
|Meddelandevisning lösenord upphör att gälla i hello Office-portalen och på hello Windows 10 desktop| | |![Rekommenderas](./media/choose-hybrid-identity-solution/ic195031.png)|

> <sup>1</sup> Lösenordssynkronisering med enkel inloggning. 

> <sup>2</sup> direkt-autentisering och enkel inloggning. 

> <sup>3</sup> federerad enkel inloggning med AD FS.

> <sup>4</sup> AD FS kan integreras med din enterprise PKI tooallow logga in med certifikat. Dessa certifikat kan vara soft-certifikat som har distribuerats via betrodda etablering kanaler, till exempel MDM eller grupprincipobjekt eller smartkort-certifikat (inklusive PIV/CAC kort) eller Hello för företag (cert-förtroende). Mer information om stöd för smartkort-autentisering finns [bloggen](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/).


## <a name="next-steps"></a>Nästa steg
[Läs mer i en Azure Proof of Concept-miljö](https://aka.ms/aad-poc)

[Installera Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)

[Övervaka hybrid identitetssynkronisering](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health)

