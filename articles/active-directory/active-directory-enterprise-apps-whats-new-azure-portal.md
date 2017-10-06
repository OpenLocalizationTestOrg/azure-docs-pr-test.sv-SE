---
title: "Vad är nytt i Enterprise programhantering i Azure Active Directory aaa | Microsoft Docs"
description: "Lär dig vad är nytt i Enterprise programhantering i Azure Active Directory."
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
editor: 
ms.assetid: 34ac4028-a5aa-40d9-a93b-0db4e0abd793
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: asteen
ms.reviewer: asteen
ms.openlocfilehash: 7f4b7b11b256f1e910e557f45f3709d762416f0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-enterprise-application-management-in-azure-active-directory"></a>Vad är nytt i Enterprise programhantering i Azure Active Directory 

Azure Active Directory (AD Azure) har förbättrats hanteringsverktyg för enterprise-programmet, med nya funktioner och möjligheter toomake hantera appar enklare och effektivare.

Här följer några av hello förbättringar för Azure AD i hello [Azure-portalen](https://portal.azure.com).

- En förbättrad programgalleriet användarupplevelse med en förenklad tillämpning skapa modell och stöd för alla hello programtyper som du ofta. 
- En helt ny Snabbstart-upplevelse som kan hjälpa dig att komma igång med en pilot för programmet. 
- Konfigurera självbetjäning principer med bara några klickningar. 
- Förbättringar tooapplication proxy, enkel inloggning konfiguration och sätta egna program-upplevelser, så att du tooget göra mer än tidigare.

## <a name="improvements-toohello-azure-active-directory-application-gallery"></a>Förbättringar av toohello Azure Active Directory-Programgalleriet

Lägg till program, oavsett om de är från hello [programgalleriet](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery), anpassade program du utvidgar toohello moln eller nya program som du utvecklar.  Du kan komma igång med den nya upplevelsen genom att klicka på **Lägg till** på hello **företagsprogram** översikt eller **alla program** blad.
 
  ![Lägga till ett program](./media/active-directory-enterprise-apps-whats-new-azure-portal/01.png)

En gång i hello gallery ser du våra aktuella program som stöder användaretablering visas främre och center.  Du kan bläddra alla typer av olika kategorier toodrill i hello-program som du är intresserad eller du kan använda hello upplevelse toorapidly hitta hello sökprogram du vill toointegrate.

  ![hello-programgalleriet](./media/active-directory-enterprise-apps-whats-new-azure-portal/02.png)

## <a name="add-custom-applications-from-one-place"></a>Lägga till anpassade program från en plats

Dessutom tooadding redan integrerade program från hello-galleriet och anpassade program konfiguration av alla hello-upplevelser som du har använt tooin hello klassiska hanteringsportalen är nu möjligt i hello nya portalen. Om du vill utvidga ett program från lokala använder hello programproxy, integrera ditt eget lösenord eller en extern SSO-program, eller skapar en helt ny program med hjälp av hello registret för programmet, du kan hämta tooit från den här ett enda Placera.

  ![Lägga till egna program](./media/active-directory-enterprise-apps-whats-new-azure-portal/03.png)

 
**tooget börjar lägga till ditt eget program**:

1. Klicka på hello **lägga till en egen länk** hello överst i hello programgalleriet. 
2. Du ser två alternativ framför du: **distribuera ett befintligt program** eller **utveckla ett nytt program**. Läs vidare toolearn hello skillnaden mellan hello två alternativ och hur toouse dem.

### <a name="deploying-existing-applications"></a>Distribuera befintliga program

1. Om du har ett program som redan kör och bara vill toointegrate till Azure AD för enkel inloggning på eller etablering, Välj hello **distribuera ett befintligt program** alternativet. Välj ett namn för programmet, klickar du på **Lägg till**.
2. Klart! I stället för att behöva tooknow alla hello information om programmet direkt, kan du nu ställa in hur programmet fungerar genom att navigera genom hello vänstra menyn och konfigurera hello programmet tooyour önskemål när som helst.

  ![Lägga till ett befintligt program med ett enda klick](./media/active-directory-enterprise-apps-whats-new-azure-portal/04.png)
 
### <a name="developing-new-applications"></a>Utveckla nya program

1. Om du utvecklar ett nytt program kan du enkelt för du tooget toohello programmet registret direkt från galleriet hello:
2. Klicka på hello **lägga till egna** alternativet från hello Application Gallery väljer hello **utveckla ett befintligt program** val och se en snabb länk rätt toohello program lägga till upplevelse.

  ![Lägga till en nyligen utvecklat program i ett par klick](./media/active-directory-enterprise-apps-whats-new-azure-portal/05.png)


>[!NOTE]
>När du lägger till ett program med hello registret för programmet, visas den visa upp i hello lista över företagsprogram där du kommer att kunna tooconfigure enkel inloggning och hantera principer för åtkomst för det nya programmet.

  ![Hantera åtkomst tooyour nytt program under företagsprogram](./media/active-directory-enterprise-apps-whats-new-azure-portal/06.png)


## <a name="quick-start-get-going-with-your-new-application-right-away"></a>Snabbstartsguide: komma igång med ditt nya program direkt 

När du har lagt till ett program, om det vara förintegrerade eller egna app har vi skapat en skräddarsydd upplevelse för Snabbkurs tooget du snabbt grundat i hello nya program upplevelsen. Om du följer varje alternativ systematiskt vi dig igenom hello UI och visar hur tooget igång med en pilot för det nya programmet så snabbt som möjligt. 
 
  ![hello nya program snabb start upplevelse](./media/active-directory-enterprise-apps-whats-new-azure-portal/07.png)

 Du kan besöka den nya Snabbstart upplevelsen när som helst och för alla program genom att klicka på **Snabbstart** från hello programmenyn vänstra navigeringsfönstret.


## <a name="updated-application-proxy-configuration"></a>Uppdaterade program proxykonfiguration
Nu ska vi Säg en hello nya program som du har lagt till körs i din lokala miljö och du vill toointegrate den med Azure AD.  Ett av hello kall Nyheter på hello ny konfiguration programupplevelse hello nya Azure AD-portalen är att dela upp hello programmet inloggning-läge från dess proxykonfiguration för programmet, du kan nu enkelt exponera lösenord SSO eller federerad program som körs i företagsnätverket högra toohello moln, utan att behöva toocreate flera instanser av programmet hello.

Dessutom toothis, du kan också konfigurera något hello nya program som du har lagt till för användning med hello Azure AD Application Proxy direkt från hello nya portalen, inklusive program som stöder intern Windows-autentisering inträffar.

  ![Konfigurera ett program toouse hello integrerad Windows-autentisering inloggning alternativet](./media/active-directory-enterprise-apps-whats-new-azure-portal/08.png)
 

tooget igång med att konfigurera en intern Windows-program för autentisering med hello Application Proxy:
1. Klicka på hello enkel inloggning navigeringsobjektet och välj **integrerad Windows-autentisering** från hello inloggning inställningsbladet och konfigurera hello inställningar tooyour önskemål.
2. Utöver att stödja dessa nya autentiseringslägen, kan du även överföra certifikat från anpassade domäner toosupport program som körs på säkra slutpunkter inom din organisation.  
 
   ![Överföra ett certifikat toobe används med hello Application Proxy](./media/active-directory-enterprise-apps-whats-new-azure-portal/09.png)

3. tooupload ett nytt certifikat för din favorit lokalt program klickar du på hello **programproxy** hello vänstra navigeringsfönstret-menyn, klickar på hello **certifikat** selector och ladda upp en certifikatfilen som vi kan använda tooencrypt begäranden från våra tooyour slutpunktsprogrammet moln.

## <a name="advanced-federated-single-sign-on-configuration"></a>Avancerad federerad enkel inloggning konfiguration

För de som du använder idag federerade program, finns det många nya funktioner i hello SAML-baserad inloggning konfiguration bladet. toostart med, nu du kan anpassa, lägga till, ta bort och mappa hello befintliga användare-attribut som anspråk i SAML-token.
 
  ![Anpassa hello SAML-token användarattribut skickades tooa federerade program](./media/active-directory-enterprise-apps-whats-new-azure-portal/10.png)


toocheck som ut hello nya federerad enkel inloggning konfiguration:
1. Öppna en federerade program **enkel inloggning** bladet från hello vänstra navigeringsfönstret-menyn och se till att hello '*SAML-baserade inloggning** läge är markerad. 
2. En gång, aktivera hello kryssrutan under hello **användarattribut** rubrik toomodify alla hello attribut ingår i hello SAML-token skickades toothat program.

Du kan också skapa, förnyelse, och hantera certifikat för federerad enkel inloggning, samt redigera som hämtar ett meddelande när ditt certifikat är om tooexpire. Du ser dessa nya alternativ under hello **certifikat** rubriken hello samma bladet för enkel inloggning.
 
  ![Skapa ett nytt certifikat, anpassa e-postmeddelanden upphör att gälla och alternativ för Certifikatsignering](./media/active-directory-enterprise-apps-whats-new-azure-portal/11.png)

### <a name="relay-state-paramenter"></a>Relay tillstånd paramenter
Slutligen har vi också utökade hello uppsättning URL för SAML-parametrar vi stöder tooinclude hello **Relay tillstånd parametern**, vilket är hello sidan användarna hamnar på inuti en externa program när hello inloggningen har slutförts. Detta är mycket användbart inställningen tooconfigure om du vill toosend ditt användare tooa specifika placeras inom hello programmet tooget går snabbt.

  ![Hello SAML Relay tillstånd för parametern](./media/active-directory-enterprise-apps-whats-new-azure-portal/12.png)
 
**Parametern för tooset hello relay state**:

1. Aktivera hello **visa avancerade inställningar för URL: en** kryssrutan under hello **domän och URL: er** rubriken på hello enkel inloggning på bladet för konfigurationen. 
2. När du gör det visas en uppsättning nya URL: en inkommande rutor visas som gör att du tooset denna och andra inställningar för SAML-URL.

## <a name="bring-your-own-password-sso-applications"></a>Ta med ditt eget lösenord SSO-program

Vi vet att inte alla program stöder federation höger out of hello box. Till exempel kanske har en hello nya program som du har lagt till en anpassad inloggningsskärm som användarna använda ett användarnamn och lösenord toosign i till. Du kan fortfarande integrera dessa typer av program med Azure AD med hjälp av vår **sätta egna program** funktionen som är nu tillgänglig för du tooconfigure i hello nya portalen.
 
  ![Integrera anpassade lösenord vaulting program med Azure AD](./media/active-directory-enterprise-apps-whats-new-azure-portal/13.png)

**toocheck ut hello ta med egna program funktionen**:

1. När du ställer in hello enkel inloggning läge för ett nytt anpassat program som du har lagt till för**lösenordsbaserade inloggning**, ange hello URL där programmet hello återgivningar dess inloggningssidan och klicka på **spara**.  
2. När du gör det, kommer vi automatiskt skrapa URL: en för ett användarnamn och lösenord Inmatningsruta och kan du överföra toouse Azure AD toosecurely lösenord toothat program med hjälp av hello åtkomst panelen webbläsartillägg.

## <a name="configure-self-service-application-access"></a>Konfigurera självbetjäning programåtkomst

När du har lagt till många nya program, kanske du vill tooallow toobrowse din användare och lägga till dessa program tootheir egna åtkomst paneler, utan att behöva toobother du som administratör. Med den senaste versionen kan du nu konfigurera och hantera självbetjäning programåtkomst direkt från hello nya portalen.

  ![Konfigurera självbetjäning programåtkomst lösenord SSO-program](./media/active-directory-enterprise-apps-whats-new-azure-portal/14.png)
 
**tooconfigure och hantera självbetjäning programåtkomst**:

1. tooget igång, kan du välja hello **självbetjäning** alternativet från hello program kvar navigeringsmenyn och ange hello **toorequest åtkomst toothis program för användarna?** för alternativet ' **Ja**'. 
2. Detta gör att du tooconfigure vem som får tooapprove åtkomst toothis program och vilken grupp Självbetjäningsanvändare läggs. Dessutom om hello programmet har konfigurerats för lösenord enkel inloggning på kan visas även ett annat alternativ som gör du om du vill att dessa godkännare toomanage hello lösenord tilldelat toohello program.

##<a name="feedback"></a>Feedback

Vi hoppas att du vill med hjälp av hello förbättrad upplevelse för Azure AD. Skriv ned hello feedback kommer! Publicera din feedback och förslag på förbättringar i hello **administrationsportalen** avsnitt i vår [Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory/category/162510-admin-portal).  Vi är glada om hur du skapar nya nya produkter varje dag, och använda vägledning-tooshape och definiera vad vi bygga härnäst.

## <a name="next-steps"></a>Nästa steg

Mer information finns i [hantera program med Azure Active Directory](active-directory-enable-sso-scenario.md).



