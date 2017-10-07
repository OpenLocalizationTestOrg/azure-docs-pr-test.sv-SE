---
title: "aaaConfigure Azure AD enkel inloggning för program | Microsoft Docs"
description: "Lär dig hur tooself tjänsten ansluta appar tooAzure Active Directory med hjälp av SAML och lösenordsbaserad SSO"
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 0d42eb0c-6d3f-4557-9030-e88e86709a19
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: asmalser
ms.reviewer: luleon
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 002a19a6c7ad25ea2f3b9c6a7c7874ed2be23cce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-single-sign-on-tooapplications-that-are-not-in-hello-azure-active-directory-application-gallery"></a>Konfigurera enkel inloggning tooapplications som inte ingår i hello Azure Active Directory-programgalleriet
Den här artikeln handlar om en funktion som gör att administratörer tooconfigure enkel inloggning tooapplications finns inte i hello Azure Active Directory app galleriet *utan att skriva kod*. Den här funktionen har frigjorts från technical preview 18 November 2015 och ingår i [Azure Active Directory Premium](active-directory-editions.md). Om du söker i stället för utvecklare vägledning om hur toointegrate anpassade appar med Azure AD via kod, se [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).

hello Azure Active Directory-programgalleriet innehåller en lista över program som är kända toosupport en form av enkel inloggning med Azure Active Directory, enligt beskrivningen i [i den här artikeln](active-directory-appssoaccess-whatis.md). När du (som en IT-specialist eller system integrator i din organisation) har hittat hello program du vill använda tooconnect, kan du sätta igång genom att följa hello stegvisa instruktioner i hello Azure management portal tooenable enkel inloggning.

Kunder med [Azure Active Directory Premium](active-directory-editions.md) licenser också få dessa ytterligare funktioner:

* Självbetjäning integrering av alla program som stöder SAML 2.0 identitetsleverantörer (SP-initierad eller IdP-initierad)
* Självbetjäning integrering av alla webbprogram som har en HTML-baserad inloggningssidan med [lösenordsbaserade SSO](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Självbetjäning anslutning av program som använder hello SCIM protokollet för användaretablering ([beskrivs här](active-directory-scim-provisioning.md))
* Möjlighet tooadd länkar tooany programmet hello [Office 365 app programstart](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) eller hello [Azure AD-åtkomstpanelen](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

Detta kan inkludera inte bara SaaS-program som du använder men ännu inte har publicerat toohello Azure AD application gallery utan från tredje part webbprogram som din organisation har distribuerats tooservers du kontroll över, antingen i hello molnet eller lokalt.

Dessa funktioner, även kallat *app integration mallar*anger standardbaserad anslutningspunkter för appar som stöder SAML, SCIM eller formulärbaserad autentisering och inkludera flexibla alternativ och inställningar för kompatibilitet med ett stort antal program. 

## <a name="adding-an-unlisted-application"></a>Lägga till ett program som inte finns i listan
tooconnect ett program med en app integration mall, logga in på hello Azure-hanteringsportalen med ditt konto i Azure Active Directory-administratör och bläddra toohello **Active Directory > [Directory] > program**väljer **Lägg till**, och sedan **lägga till ett program från galleriet hello**. 

![][1]

Hello app Gallery och du kan lägga till en ny app med hello **anpassad** kategori hello vänster eller genom att välja hello **lägga till ett olistade program** länk som visas i hello Sök resulterar om önskade appen kunde inte hittas. Du kan konfigurera hello enkel inloggning och beteende när du har angett ett namn för ditt program. 

**Tips**: som bästa praxis, använda hello Sök funktionen toocheck toosee om hello programmet finns redan i hello programgalleriet. Om hello app har hittats och dess beskrivning nämns ”enkel inloggning” och sedan hello program har redan stöd för federerad enkel inloggning. 

![][2]

Lägga till ett program som det här sättet ger en mycket lik upplevelse toohello finns tillgängliga för redan integrerade program. toostart, Välj **Konfigurera enkel inloggning**. hello nästa skärm anger hello följande tre alternativ för att konfigurera enkel inloggning, som beskrivs i följande avsnitt hello.

![][3]

## <a name="azure-ad-single-sign-on"></a>Azure AD-Single Sign-On
Välj det här alternativet tooconfigure för SAML-autentisering för hello program. Detta kräver att hello programstöd SAML 2.0 och du bör samla in information om hur toouse hello SAML-funktioner i programmet hello innan du fortsätter. När du har valt **nästa**, kommer du att ange tooenter tre olika URL: er motsvarande toohello SAML-slutpunkter för hello program. 

![][4]

Dessa är:

* **Logga URL (SP-initierad endast)** – där hello användaren går toothis toosign i programmet. Om programmet hello är konfigurerat tooperform service provider-initierad enkel inloggning, sedan när användarna navigerar toothis URL, hello-leverantör hello nödvändiga omdirigering tooAzure AD tooauthenticate, och logga in hello användare i. Om det här fältet fylls använder URL toolaunch hello programmet från Office 365 och hello Azure AD-åtkomstpanelen Azure AD. Om det här fältet är ommited och Azure AD i stället utför identitetsleverantör-initierat inloggning när hello appen startas från Office 365, hello Azure AD-åtkomstpanelen, eller från hello Azure AD enkel inloggning URL (copiable från fliken instrumentpanelen i hello).
* **Utfärdar-URL** -hello utfärdar-URL ska identifiera hello program för vilket enkel inloggning är konfigurerade. Detta är hello värde att Azure AD skickar tillbaka tooapplication som hello **målgruppen** hello SAML-token och programmet hello anges förväntade toovalidate den. Det här värdet visas också som hello **enhets-ID** i en SAML-metadata som tillhandahålls av programmet hello. Dokumentationen hello programmet SAML för information om vad det är enhets-ID eller målgruppen värdet. Nedan visas ett exempel på hur hello målgruppen URL visas i hello SAML-token returnerade toohello program:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Reply URL** -hello svars-URL: en är där programmet hello förväntar tooreceive hello SAML-token. Det är också hänvisade tooas hello **Assertion konsumenten Service (ACS) URL**. Dokumentationen hello programmet SAML information om dess SAML-token reply URL eller ACS-URL är.
  När dessa har angetts, klickar du på **nästa** tooproceed toohello nästa skärm. Den här skärmen innehåller information om vilka behov toobe har konfigurerat på hello program på klientsidan tooenable detta tooaccept en SAML-token från Azure AD. 

![][5]

Vilka värden som krävs varierar beroende på programmet hello, så kontrollera hello programmet SAML dokumentationen för mer information. Hej **inloggning** och **utloggning** URL för båda lösa toohello samma slutpunkt, vilket är hello SAML hantering av begäran slutpunkten för din instans av Azure AD. hello utfärdar-URL är hello-värde som visas som hello ”utfärdaren” inuti hello SAML-token utfärdade toohello program. 

När programmet har konfigurerats, klickar du på **nästa** knappen och sedan hello **Slutför** tooclose hello dialogrutan. 

## <a name="assigning-users-and-groups-tooyour-saml-application"></a>Tilldela användare och grupper tooyour SAML-program
När programmet har konfigurerade toouse Azure AD som en SAML-baserade identitetsleverantör, är nästan klara tootest. Azure AD utfärdar inte en token så att de toosign i hello program om de har beviljats åtkomst med hjälp av Azure AD som en säkerhetskontroll. Användare kan beviljas åtkomst direkt eller via en grupp som är medlem i. 

tooassign en användare eller grupp tooyour program klickar du på hello **tilldela användare** knappen. Välj hello användare eller grupp som du vill tooassign och välj sedan hello **tilldela** knappen. 

![][6]

Tilldela en användare kan Azure AD tooissue en token för hello användaren som orsakar en panel för det här programmet tooappear i hello användarens åtkomstpanelen. Ett program sida vid sida visas också i hello Office 365 startprogrammet om hello användaren använder Office 365. 

Du kan ladda upp en ikonlogotyp för hello-program med hello **överföra logotypen** hello-knappen **konfigurera** för hello program. 

### <a name="customizing-hello-claims-issued-in-hello-saml-token"></a>Anpassa hello anspråk som utfärdats i hello SAML-token
När en användare autentiseras toohello program, utfärdar Azure AD en SAML-token toohello app som innehåller information (eller anspråk) om hello-användaren som unikt identifierar dem. Som standard innehåller detta hello användarens användarnamn, e-postadress, Förnamn och efternamn. 

Du kan visa eller redigera hello anspråk skickas i hello SAML-token toohello programmet under hello **attribut** fliken. 

![][7]

Det finns två möjliga orsaker till varför du kanske behöver tooedit hello anspråk som utfärdats i SAML-token för hello: •hello program har skrivits toorequire en annan uppsättning anspråk URI: er eller anspråk värden •Your programmet har distribuerats på ett sätt som kräver hello NameIdentifier anspråk toobe något annat än hello användarnamn (AKA användarens huvudnamn) lagras i Azure Active Directory. 

Information om hur tooadd och redigera anspråk för dessa scenarier kolla detta [artikel om anspråk anpassning](active-directory-saml-claims-customization.md). 

### <a name="testing-hello-saml-application"></a>Testa hello SAML-program
När hello SAML URL: er och certifikat har konfigurerats i Azure AD och hello program, användare eller grupper som har tilldelats toohello program i Azure, och hello anspråk har granskat och redigera vid behov och sedan hello användaren är klar toosign till hello programmet. 

tootest, logga bara in hello Azure AD-åtkomstpanelen på https://myapps.microsoft.com med ett användarkonto som du tilldelade toohello program, och klicka sedan på hello panelen för hello programmet tookick hello enkel inloggning processen. Alternativt kan bläddra du direkt toohello inloggnings-URL för programmet hello och logga in därifrån. 

Felsöka tips finns i följande [artikel om hur toodebug SAML-baserade enkel inloggning tooapplications](active-directory-saml-debugging.md) 

## <a name="password-single-sign-on"></a>Lösenord för enkel inloggning
Välj det här alternativet tooconfigure [lösenordsbaserade enkel inloggning](active-directory-appssoaccess-whatis.md) för ett webbprogram som har en HTML-inloggningssida. Lösenordsbaserade SSO också hänvisade tooas lösenord vaulting, kan du toomanage användaren åtkomst och lösenord tooweb program som inte har stöd för identitetsfederation. Det är också användbart för scenarier där flera användare behöver tooshare ett enskilt konto, till exempel tooyour organisation sociala medier app konton. 

När du har valt **nästa**, kommer du att ange tooenter hello URL för hello programmet webbaserade inloggningssidan. Observera att detta måste vara hello sida som innehåller hello användarnamn och lösenord indatafälten. En gång angivna, Azure AD startar en process tooparse hello-inloggningssida för en inmatning användarnamn och ett lösenordsindata. Om hello processen inte lyckas, sedan hjälper den dig att en annan processen för att installera ett webbläsartillägg (kräver Internet Explorer, Chrome eller Firefox) som gör att du toomanually avbilda hello fält.

När hello-inloggningssida avbildas, kan tilldelas användare och grupper och autentiseringsuppgifter policys kan ställas in som vanlig [lösenord SSO appar](active-directory-appssoaccess-whatis.md).

Obs: Du kan ladda upp en ikonlogotyp för hello-program med hello **överföra logotypen** hello-knappen **konfigurera** för hello program. 

## <a name="existing-single-sign-on"></a>Befintliga enkel inloggning
Välj det här alternativet tooadd en länk tooan programmet tooyour organisations Azure AD-åtkomstpanelen eller Office 365-portalen. Du kan använda den här tooadd länkar toocustom webbprogram som använder Azure Active Directory Federation Services (eller andra federationstjänsten) i stället för Azure AD för autentisering. Eller, du kan lägga till djuplänkar toospecific SharePoint sidor eller andra webbsidor som du bara vill tooappear på dina användares åtkomst paneler. 

När du har valt **nästa**, kommer du att ange tooenter hello URL för hello programmet toolink till. När slutfört, användare och grupper kan tilldelas toohello program, vilket gör att hello programmet tooappear i hello [Office 365 app programstart](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) eller hello [Azure AD-åtkomstpanelen](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) för dessa användare.

Obs: Du kan ladda upp en ikonlogotyp för hello-program med hello **överföra logotypen** hello-knappen **konfigurera** för hello program.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Hur tooCustomize anspråk som utfärdats i hello SAML-Token för Pre-Integrated appar](active-directory-saml-claims-customization.md)
* [Felsökning av SAML-baserade enkel inloggning](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
