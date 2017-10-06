---
title: "aaaConditional åtkomst tooon lokal appar – Azure AD | Microsoft Docs"
description: "Beskriver hur tooset konfigurerar villkorlig åtkomst för program som du publicerar toobe som nås med hjälp av Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2e97722b-eb4e-4078-b607-9fed210d8a0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 7bed25dd4ba17941e77d8c4b2b9ba4edcf0cf597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-conditional-access-in-azure-ad-application-proxy"></a>Arbeta med villkorlig åtkomst i Azure AD Application Proxy

>[!NOTE]
>Den här artikeln gäller toohello klassiska Azure-portalen som dras. Vi rekommenderar att du använder hello [Azure-portalen](https://portal.azure.com). I hello Azure-portalen hello Application Proxy appar har samma funktioner för villkorlig åtkomst som de andra SaaS-app. toolearn mer information om hur villkorlig åtkomst finns [Kom igång med villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Du kan konfigurera åtkomst regler toogrant villkorlig åtkomst tooapplications publiceras med hjälp av Application Proxy. Detta innebär att du kan:

* Kräv Multi-Factor authentication per program
* Kräv Multi-Factor authentication endast när användare inte är på arbetet
* Blockera användare från att komma åt programmet hello när de inte är på arbetet

De här reglerna kan vara tillämpade tooall användare och grupper eller endast toospecific användare och grupper. Som standard gäller hello regeln tooall användare som har åtkomst till toohello program. Hello regeln kan dock också vara begränsade toousers som är medlemmar i de angivna säkerhetsgrupperna.  

Åtkomstregler utvärderas när en användare ansluter till en federerad program som använder OAuth 2.0, OpenID Connect, SAML och WS-Federation. Dessutom utvärderas åtkomstregler med OAuth 2.0 och OpenID Connect när en uppdateringstoken används tooacquire en åtkomst-token.

## <a name="conditional-access-prerequisites"></a>Krav för villkorlig åtkomst
* Prenumerationen tooAzure Active Directory Premium
* En federerade eller hanterade Azure Active Directory-klient
* Federerade klienter kräver Multi-Factor authentication (MFA)  
    ![Konfigurera regler för åtkomst - kräver Multi-Factor authentication](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Konfigurera Multi-Factor authentication per program
1. Logga in som administratör i hello klassiska Azure-portalen.
2. Gå tooActive Directory och välj hello katalog där du vill tooenable Application Proxy.
3. Klicka på **program** och rulla ned toohello **åtkomstregler** avsnitt. hello åtkomst regler avsnitt visas endast för program som publiceras med hjälp av Webbprogramproxy som använder federerad autentisering.
4. Aktivera hello regeln genom att välja **aktivera åtkomstregler** för**på**.
5. Ange hello användare och grupper toowhom hello regler gäller. Använd hello **Lägg till grupp** knappen tooselect en eller flera grupper toowhich hello regeln gäller. Den här dialogrutan kan också vara används tooremove valda grupper.  När hello regler är valda tooapply toogroups, hello åtkomstregler tillämpas endast för användare som tillhör tooone av hello angivna säkerhetsgrupper.  

   * Kontrollera tooexplicitly utelämna säkerhetsgrupper från hello regel **utom** och ange en eller flera grupper. Användare som är medlemmar i en grupp i hello utom lista är inte obligatoriska tooperform multifaktorautentisering.  
   * Om en användare har konfigurerats med hello per användare multifaktorautentisering funktionen, åsidosätter den här inställningen hello programmet multifaktorautentisering regler. En användare som har konfigurerats för multifaktorautentisering för per användare är obligatoriska tooperform multifaktorautentisering även om de har undantagits från hello programmets multifaktorautentisering regler. Lär dig mer om [Multi-Factor autentisering och användarspecifika inställningar](../multi-factor-authentication/multi-factor-authentication.md).
6. Välj hello åtkomstregel som du vill tooset:

   * **Kräver Multi-Factor authentication**: användare toowhom åtkomstregler gälla är obligatoriska toocomplete multifaktorautentisering innan åtkomst hello programmet toowhich hello regeln gäller.
   * **Kräver Multi-Factor authentication när de inte är på arbetet**: användarna i tooaccess hello programmet från en betrodd IP-adress är inte obligatoriska tooperform multifaktorautentisering. hello betrodda IP-adressintervall kan konfigureras på inställningssidan för hello multifaktorautentisering.
   * **Blockera åtkomst när de inte är på arbetet**: användarna försöker tooaccess hello programmet från utanför företagets nätverk inte kan tooaccess hello program.

## <a name="configuring-mfa-for-federation-services"></a>Konfigurera MFA för federation services
För federerade klienter multifaktorautentisering (MFA) kan utföras genom Azure Active Directory eller hello lokala AD FS-servern. Som standard sker MFA på valfri sida hos Azure Active Directory. tooconfigure MFA lokalt, kör Windows PowerShell och Använd hello – SupportsMFA egenskapen tooset hello Azure AD-modulen.

hello följande exempel visas hur tooenable lokalt MFA med hjälp av hello [cmdlet Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) hello contoso.com innehavaren:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

I tillägg toosetting denna flagga, hello federerade klient AD FS-instansen måste vara konfigurerad tooperform multifaktorautentisering. Följ anvisningarna för hello för [distribuerar Microsoft Azure Multi-Factor authentication lokalt](../multi-factor-authentication/multi-factor-authentication-get-started-server.md).

## <a name="see-also"></a>Se även
* [Arbeta med anspråksmedvetna program](active-directory-application-proxy-claims-aware-apps.md)
* [Publicera program med Application Proxy](active-directory-application-proxy-publish.md)
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
* [Publicera program med ditt domännamn](active-directory-application-proxy-custom-domains.md)

Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)
