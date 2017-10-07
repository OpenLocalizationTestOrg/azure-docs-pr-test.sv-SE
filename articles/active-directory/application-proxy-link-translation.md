---
title: "aaaTranslate länkar och URL: er Azure AD App Proxy | Microsoft Docs"
description: Beskriver hello grunderna om Azure AD Application Proxy-kopplingar.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7ec2b9fb01617067cf5d676037877bf72c19217b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="redirect-hardcoded-links-for-apps-published-with-azure-ad-application-proxy"></a>Omdirigera hårdkodad länkar till appar som publiceras med Azure AD Application Proxy

Azure AD Application Proxy gör dina lokala appar tillgängliga toousers som är fjärranslutna eller på sina egna enheter. Vissa appar har utvecklats med lokala länkar som är inbäddad i hello HTML. De här länkarna fungerar inte korrekt när hello app används via fjärranslutning. När du har flera lokala program punkt tooeach andra dina användare förväntar sig hello länkar tookeep fungerar när de inte är i hello-kontoret. 

hello bästa sätt toomake till att länkarna fungerar hello samma både inom och utanför företagets nätverk är tooconfigure hello externa URL: er för dina appar toobe hello samma som deras interna URL: er. Använd [anpassade domäner](active-directory-application-proxy-custom-domains.md) tooconfigure din externa URL: er toohave din företagets domännamn i stället för hello proxy för standarddomänen.

Om du inte kan använda anpassade domäner i din klientorganisation, behålls hello länken översättning funktion i Application Proxy länkarna fungerar oavsett var dina användarna finns. När du har appar som pekar direkt toointernal slutpunkter eller portar, kan du mappa publicerade dessa Intern URL: er toohello externa Application Proxy URL: er. När länköversättning är aktiverat och programproxy söker igenom HTML, CSS och välj JavaScript-taggar för publicerade interna länkar. Sedan översätter hello Application Proxy-tjänsten dem så att användarna får en oavbruten upplevelse.

>[!NOTE]
>hello länken översättningsfunktionen är för klienter som av något skäl inte kan använda anpassade domäner toohave hello samma interna och externa URL: er för sina appar. Innan du aktiverar den här funktionen finns om [anpassade domäner i Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) du kan använda.
>
>Eller, om hello-program som du behöver tooconfigure med länköversättning SharePoint, se [konfigurera alternativa åtkomstmappningar för SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx) för en annan metod toomapping länkar.

## <a name="how-link-translation-works"></a>Hur länkar översättning fungerar

Efter autentisering när hello-proxyserver passerar hello data toohello programanvändare, Application Proxy igenom hello program för hårdkodad länkar och ersätter dem med deras respektive publicerade externa URL: er.

Programproxy förutsätter att program är kodade i UTF-8. Om detta inte är fallet hello ange hello kodningstyp i en http-Svarsrubrik som `Content-Type:text/html;charset=utf-8`.

### <a name="which-links-are-affected"></a>Vilka länkar påverkas?

hello länken översättningsfunktionen söker bara efter länkar som finns i koden taggar i hello brödtexten i en app. Programproxy har en separat funktion för översättning av cookies eller URL: er i rubriker. 

Det finns två vanliga typer av interna länkar i lokala program:

- **Relativa interna länkar** den punkt tooa delad resurs i en lokal filstruktur som `/claims/claims.html`. De här länkarna fungerar automatiskt i appar som publiceras via Application Proxy och fortsätta toowork med eller utan länköversättning. 
- **Hårdkodad interna länkar** tooother lokala appar som `http://expenses` eller publicerade filer av `http://expenses/logo.jpg`. hello länken översättningsfunktionen fungerar på hårdkodad interna länkar och ändrar dem toopoint toohello externa URL: er som fjärranslutna användare behöver toogo via.

### <a name="how-do-apps-link-tooeach-other"></a>Hur länkar tooeach andra appar?

Länköversättning är aktiverat för varje program, så att du har kontroll över hello användarupplevelsen på hello per app-nivå. Aktivera länköversättning för en app när du vill hello länkar *från* som app-toobe översätts inte länkar *till* appen. 

Anta exempelvis att du har tre program som publicerats via Proxy för alla länkar tooeach andra: fördelar, utgifter och resa. Det finns en fjärde app Feedback som inte publicerats via Application Proxy.

När du aktiverar länköversättning för hello fördelar app hello länkar tooExpenses och resa är omdirigerade toohello externa URL: er för dessa appar, men hello länken tooFeedback omdirigeras inte eftersom det inte finns några externa URL: en. Länkar från utgifter och resa tillbaka tooBenefits inte fungerar, eftersom länköversättning inte har aktiverats för dessa två appar.

![Länkar från fördelar tooother appar när länköversättning är aktiverad](./media/application-proxy-link-translation/one_app.png)

### <a name="which-links-arent-translated"></a>Vilka länkar inte är översatt?

tooimprove prestanda och säkerhet, vissa länkar är inte översättas:

- Länkar inte inuti kodtaggar. 
- Länkar inte i HTML-, CSS- eller JavaScript. 
- Interna länkar öppnas från andra program. Att kommer inte översätta länkar skickas via e-post eller snabbmeddelande eller ingår i andra dokument. hello användarna måste tooknow toogo toohello externa URL: en.

Om du behöver toosupport något av dessa två scenarier, Använd hello samma interna och externa URL: er i stället för länköversättning.  

## <a name="enable-link-translation"></a>Aktivera länköversättning

Komma igång med länköversättning är lika enkelt som att klicka på en knapp:

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.
2. Gå för**Azure Active Directory** > **företagsprogram** > **alla program** > Välj hello-app som du vill toomanage > **Programproxy**.
3. Aktivera **översätta URL: er i programmet brödtext** för**Ja**.

   ![Välj Ja tootranslate URL: er i programmet brödtext](./media/application-proxy-link-translation/select_yes.png).
4. Välj **spara** tooapply ändringarna.

Nu när användarna har åtkomst till det här programmet genomsökas hello proxy för intern URL: er som har publicerats via Application Proxy på din klient.

## <a name="send-feedback"></a>Skicka feedback

Vi vill hjälp-toomake den här funktionen fungerar för alla dina appar. Vi söka över 30 taggar i HTML- och CSS och överväger vilka JavaScript fall toosupport. Om du har ett exempel på genererade länkar som inte är som översätts skickar du ett kodstycke för[Application Proxy Feedback](mailto:aadapfeedback@microsoft.com). 

## <a name="next-steps"></a>Nästa steg
[Använda anpassade domäner med Azure AD Application Proxy](active-directory-application-proxy-custom-domains.md) toohave hello samma interna och externa URL

[Konfigurera alternativa åtkomstmappningar för SharePoint 2013](https://technet.microsoft.com/library/cc263208.aspx)
