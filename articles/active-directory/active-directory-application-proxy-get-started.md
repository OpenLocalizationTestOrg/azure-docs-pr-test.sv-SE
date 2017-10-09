---
title: "aaaHow tooprovide säker fjärråtkomst tooon lokala appar"
description: "Beskriver hur toouse Azure AD Application Proxy tooprovide säker fjärråtkomst tooyour lokala appar."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 289e970ed0596fcd06ccf6b2ad92203366fbb494
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooprovide-secure-remote-access-tooon-premises-applications"></a>Hur säkra tooprovide fjärråtkomst tooon lokala program

Anställda vill idag toobe produktiva var som helst, när som helst och från valfri enhet. De vill toowork på sina egna enheter, oavsett om de är bärbara datorer, surfplattor eller telefoner. Och de förväntar sig toobe kan tooaccess sina program både SaaS-appar i hello molnet och företagets appar lokalt. Ge åtkomst tooon lokala program har traditionellt berörda virtuella privata nätverk (VPN) eller demilitariserad zoner (DMZs). Inte bara är dessa lösningar komplexa och svåra toomake säkra, men de är kostsamma tooset in och hantera.

Det finns ett bättre sätt!

En modern personal i hello mobile-första molnet första world behöver en lösning för moderna fjärråtkomst. Azure AD Application Proxy är en funktion i Azure Active Directory som erbjuder fjärråtkomst som en tjänst. Det innebär att det är enkelt toodeploy, använda och hantera.

[!INCLUDE [identity](../../includes/azure-ad-licenses.md)]

## <a name="what-is-azure-active-directory-application-proxy"></a>Vad är Azure Active Directory Application Proxy?
Azure AD Application Proxy ger enkel inloggning (SSO) och säker fjärråtkomst för webbprogram finns lokalt. Vissa appar som du vill ha toopublish omfattar SharePoint-webbplatser, Outlook Web Access eller några andra LOB webbprogram som du har. Dessa lokala web program som är integrerade med Azure AD hello samma identitet och styr plattform som används av O365. Slutanvändarna kan komma åt dina lokala program hello samma sätt som de har åtkomst till O365 och andra SaaS-appar som är integrerade med Azure AD. Du inte behöver toochange hello nätverksinfrastruktur eller kräver tooprovide för VPN-lösningen för dina användare.

## <a name="why-is-application-proxy-a-better-solution"></a>Varför är Application Proxy en bättre lösning?
Azure AD Application Proxy ger en enkel, säker och kostnadseffektiv fjärråtkomst lösning tooall lokala program.

Azure AD Application Proxy är:

* **Enkel**
   * Du inte behöver toochange eller uppdatera ditt program toowork med Application Proxy. 
   * Användarna får en konsekvent autentiseringsupplevelse. De kan använda hello MyApps portal tooget enkel inloggning tooboth SaaS-appar i hello moln och dina appar lokalt. 
* **Skydda**
   * När du publicerar dina appar med hjälp av Azure AD Application Proxy kan du dra nytta av hello omfattande auktorisering kontroller och säkerhet analyser i Azure. Du får skalbar molnlagring säkerhet och säkerheten i Azure-funktioner som villkorlig åtkomst och tvåstegsverifiering verifiering.
   * Du har inte tooopen alla inkommande anslutningar via brandväggen-toogive dina användare fjärråtkomst. 
* **Kostnadseffektiv**
   * Programproxy fungerar i hello molnet så att du kan spara tid och pengar. Lokala lösningar normalt kräver tooset in och underhålla DMZs, edge servrar eller andra komplexa infrastrukturer.  

## <a name="what-kind-of-applications-work-with-application-proxy"></a>Vilka typer av program som fungerar med Application Proxy?
Du kan komma åt olika typer av interna program med Azure AD Application Proxy:

* Webbprogram som använder [integrerad Windows-autentisering](active-directory-application-proxy-sso-using-kcd.md) för autentisering  
* Webbprogram som använder formulärbaserad eller [huvud-baserade](application-proxy-ping-access.md) åtkomst  
* Web API: er som du vill tooexpose toorich program på olika enheter  
* Program som finns bakom en [servern för fjärrskrivbordsgateway](application-proxy-publish-remote-desktop.md)  
* Rich-klientprogram som är integrerade med hello Active Directory Authentication Library (ADAL)

## <a name="how-does-application-proxy-work"></a>Hur fungerar Application Proxy?
Det finns två komponenter som du behöver tooconfigure toomake Application Proxy arbete: en koppling och den externa slutpunkten. 

hello connector är en förenklad agent som placeras på en Windows-Server i nätverket. hello anslutningen underlättar hello trafikflöde från hello Application Proxy-tjänsten i hello molnet tooyour programmet lokalt. Den används endast utgående anslutningar så att du inte ha några inkommande portar för tooopen eller placera något i hello DMZ. hello kopplingar är tillståndslösa och hämtar information från hello molnet vid behov. Mer information om kopplingar, t.ex. hur de belastningsutjämning och autentisera, se [förstå Azure AD Application Proxy kopplingar](application-proxy-understand-connectors.md). 

hello externa slutpunkten är hur användarna kan nå dina program medan utanför nätverket. De kan antingen vara direkt tooan externa URL: en som du bestämmer eller de kan komma åt programmet hello hello MyApps-portalen. När användarna gå tooone dessa slutpunkter, autentisera i Azure AD och sedan dirigeras via hello anslutningsprogram toohello lokalt.

 ![Diagram över AzureAD Application Proxy](./media/active-directory-application-proxy-get-started/azureappproxxy.png)

1. hello användare har åtkomst till programmet hello via hello Application Proxy-tjänsten och dirigerad toohello Azure AD-inloggningssida tooauthenticate.
2. Efter en lyckad inloggning, en token skapas och skickas toohello klientenheten.
3. hello klienten skickar hello token toohello Application Proxy-tjänsten, som hämtar hello användarens huvudnamn (UPN) och säkerhet huvudnamn (SPN) från hello token dirigerar sedan hello begäran toohello Application Proxy connector.
4. Om du har konfigurerat enkel inloggning utför hello connector ytterligare autentisering krävs för hello användares räkning.
5. hello-kopplingen skickar hello begäran toohello lokalt program.  
6. hello svar skickas via Application Proxy-tjänsten och connector toohello användare.

### <a name="single-sign-on"></a>Enkel inloggning
Azure AD Application Proxy ger enkel inloggning (SSO) tooapplications som använder integrerad Windows-autentisering (IWA) eller anspråksmedvetna program. Om programmet använder IWA, personifierar Application Proxy hello användare med hjälp av Kerberos-begränsad delegering tooprovide enkel inloggning. Om du har ett anspråksmedvetet program som har förtroende för Azure Active Directory fungerar SSO eftersom hello användaren redan har autentiserats av Azure AD.

Mer information om Kerberos finns [alla önskade tooknow om Kerberos-begränsad delegering (KCD)](https://blogs.technet.microsoft.com/applicationproxyblog/2015/09/21/all-you-want-to-know-about-kerberos-constrained-delegation-kcd).

### <a name="managing-apps"></a>Hantera appar
En appen publiceras med Application Proxy, du kan hantera den precis som andra företagsapp i hello Azure-portalen. Du kan använder Azure Active Directory-säkerhetsfunktioner som villkorlig åtkomst och tvåstegsverifiering verifiering, kontrollera behörigheter och anpassa hello företagsanpassning för din app. 

## <a name="get-started"></a>Kom igång

Innan du konfigurerar Application Proxy, kontrollera att du har en stöds [Azure Active Directory edition](https://azure.microsoft.com/pricing/details/active-directory/) och Azure AD-katalog som du är en global administratör.

Kom igång med Application Proxy i två steg:

1. [Aktivera Application Proxy och konfigurera hello connector](active-directory-application-proxy-enable.md).    
2. [Publicera program](active-directory-application-proxy-publish.md) -Använd hello snabbt och enkelt guiden tooget dina lokala appar publicerade och är åtkomlig via fjärranslutning.

## <a name="whats-next"></a>Nästa steg
När du har publicerat din första app finns mycket mer du kan göra med Application Proxy:

* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
* [Publicera program med ditt domännamn](active-directory-application-proxy-custom-domains.md)
* [Lär dig mer om Azure AD Application Proxy-kopplingar](application-proxy-understand-connectors.md)
* [Arbeta med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md) 
* [Ange en anpassad startsida](application-proxy-office365-app-launcher.md)

Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)

