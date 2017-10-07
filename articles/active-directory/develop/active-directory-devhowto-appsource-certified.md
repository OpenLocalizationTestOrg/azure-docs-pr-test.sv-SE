---
title: "aaaHow tooget AppSource certifierad för Azure Active Directory | Microsoft Docs"
description: "Information om hur tooget programmet AppSource certifierad för Azure Active Directory."
services: active-directory
documentationcenter: 
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: e9f07e9220afcba1120b987122fe770fe5225eed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-appsource-certified-for-azure-active-directory"></a>Hur tooget AppSource certifierad för Azure Active Directory
[Microsoft AppSource](https://appsource.microsoft.com/) är ett mål för business användare toodiscover försök och hantera line-of-business SaaS-program (fristående SaaS och tillägg tooexisting Microsoft SaaS-produkter).

toolist fristående SaaS-program på AppSource ditt program måste acceptera enkel inloggning från arbetskonton från alla företag eller organisation som har Azure Active Directory. hello inloggningsprocessen måste använda hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) eller [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokoll. SAML-integration accepteras inte för AppSource certifiering.

## <a name="guides-and-code-samples"></a>Guider och kodexempel
Om du vill toolearn om hur toointegrate ditt program med Azure Active Directory med öppna ID connect, Följ våra guider och kodexempel i hello [Utvecklarhandbok för Azure Active Directory](./active-directory-developers-guide.md#get-started "komma igång med Azure AD för utvecklare").

## <a name="multi-tenant-applications"></a>Program med flera klienter

Ett program som accepterar inloggningar från användare från företaget eller organisationen som har Azure Active Directory utan att kräva en separat instans, konfiguration och distribution kallas en *flera innehavare programmet*. AppSource rekommenderar att program implementera multitenans tooenable hello *enkelklickning* kostnadsfria utvärderingsversionen.

I ordning tooenable multitenans för ditt program:
- Ange `Multi-Tenanted` egenskapen för`Yes` för registrering av applikation på hello [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (som standard konfigureras program som har skapats i hello Azure-portalen som *stöd för en innehavare*)
- Uppdatera din kod toosend begäranden toohello '`common`-slutpunkt (uppdatera hello slutpunkten från *https://login.microsoftonline.com/ {yourtenant}* för*https://login.microsoftonline.com/common*)
- För vissa plattformar som ASP.NET, behöver du också tooupdate din kod tooaccept flera certifikatutfärdare

Läs mer om multitenans: [hur hello toosign i alla Azure Active Directory (AD) användare med flera innehavare programmönster](./active-directory-devhowto-multi-tenant-overview.md).

### <a name="single-tenant-applications"></a>Stöd för en innehavare program
Program som accepterar endast inloggningar från användare av en definierad Azure Active Directory-instans kallas *stöd för en innehavare programmet*. Externa användare (inklusive arbetet eller skolan konton från andra organisationer eller personligt konto) kan logga in tooa stöd för en innehavare programmet när du lägger till varje användare som *gästkontot* toohello Azure Active Directory-instans som programmet hello är registrerat. Du kan lägga till användare som gäst konton tooan Azure Active Directory via hello [ *Azure AD B2B-samarbete* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - och det kan göras [genom att programmera](../active-directory-b2b-code-samples.md). När du lägger till en användare som gäst konto tooan Azure Active Directory, skickas ett e-postinbjudan toohello användare, som har tooaccept hello inbjudan genom att klicka på hello länken i e-postinbjudan hello. Inbjudningar som skickas tooan ytterligare användare i organisationen bjuda in som även är medlem i hello partnerorganisation är inte obligatoriska tooaccept en inbjudan toosign i.

Stöd för en innehavare program kan du aktivera hello *kontakta mig* upplevelse, men om du vill tooenable hello enkelklickning / kostnadsfria utvärderingsversionen som AppSource rekommenderar, aktiverar du multitenans för ditt program i stället.


## <a name="appsource-trial-experiences"></a>AppSource utvärderingsversion upplevelser

### <a name="free-trial-customer-led-trial-experience"></a>Kostnadsfri utvärderingsversion (kund-ledde utvärderingsversionen) 
Hej *kunden ledde utvärderingsversion* är hello-upplevelse som AppSource rekommenderar som den erbjuder ett enda musklick åtkomst tooyour program. Under en illustration av hur det här upplevelsen ser ut som:<br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li>Användaren hittar programmet i AppSource webbplats</li><li>'Kostnadsfri utvärderingsversion' alternativet väljs</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li>AppSource dirigerar användaren tooa URL på din webbplats</li><li>Webbplatsen startar hello <i>single-sign-on</i> automatiskt (på sidinläsning)</li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li>Användaren är omdirigerade tooMicrosoft inloggning sida</li><li>Användaren anger autentiseringsuppgifter toosign i</li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li>Användaren lämnar samtycke för ditt program</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Inloggningen har slutförts och användaren är omdirigerade tillbaka tooyour webbplats</li><li>Användaren startar hello kostnadsfri utvärderingsversion</li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a>Kontakta mig (Partner ledde utvärderingsversionen)
Hej *partner utvärderingsversionen* kan användas när en manuell eller en långsiktig åtgärd behöver toohappen tooprovision hello användare / företaget: exempelvis programmet måste tooprovision virtuella datorer, databasinstanser, eller åtgärder som tar mycket tid toocomplete. I det här fallet när användaren väljer hello *begära utvärderingsversion* knappen och fyller i ett formulär, AppSource skickar du hello användares kontaktinformation. Vid mottagning av den här informationen kan du sedan etablera hello miljö och skicka hello instruktioner toohello användare på hur tooaccess hello utvärderingsversionen:<br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li>Användaren hittar programmet i AppSource-webbplats</li><li>Kontakta mig alternativet automatiskt</li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li>Fyller i ett formulär med kontaktinformation</li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td>Du får användarinformation</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td>Konfigurationsmiljö</td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td>Kontakta användaren med testversionen information</td>
        </tr>
        </table><br/><br/>
        <ul><li>Du får användaren information och utvärderingsversion instans av installationsprogrammet</li><li>Du kan skicka hello hyperlink tooaccess dina programanvändare toohello</li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li>Användare har åtkomst till programmet och fullständig hello single-sign-on-processen</li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li>Användaren lämnar samtycke för ditt program</li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li>Inloggningen har slutförts och användaren är omdirigerade tillbaka tooyour webbplats</li><li>Användaren startar hello kostnadsfri utvärderingsversion</li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a>Mer information
Läs mer om hello AppSource utvärderingsversionen [den här videon](https://aka.ms/trialexperienceforwebapps). 
 
## <a name="next-steps"></a>Nästa steg

- Mer information om hur du skapar program som stöder Azure Active Directory inloggningar finns [Autentiseringsscenarier för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios) 

- Mer information om hur toolist SaaS-program i AppSource, gå finns [AppSource partnerinformation](https://appsource.microsoft.com/partners)


## <a name="get-support"></a>Support
Vi använder för Azure Active Directory-integrering [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) med tooprovide hello community-support. 

Vi rekommenderar starkt att du be dina frågor på Stack Overflow först och bläddra befintliga problem toosee om någon har frågat din fråga innan. Kontrollera att dina frågor eller kommentarer är märkta med `[azure-active-directory]`.

Använd hello följande kommentarer avsnittet tooprovide feedback och hjälp oss att förfina och utforma innehållet.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->