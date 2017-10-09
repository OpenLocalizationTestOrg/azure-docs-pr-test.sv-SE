---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.openlocfilehash: fd19b3f178a2aee7dd2f204d6d3cf6df8fe6b444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook

I kursen får du lära dig hur toointegrate arbetsplats av Facebook med Azure Active Directory (AD Azure).

Integrera arbetsplats av Facebook med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooWorkplace av Facebook.
- Du kan aktivera användarna tooautomatically hämta inloggad tooWorkplace av Facebook (enkel inloggning) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats: hello Azure-portalen.

Mer information om programvara som en tjänst (SaaS) appintegrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En arbetsplats av Facebook enkel inloggning (SSO) aktiverat prenumeration

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD SSO i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till arbetsplatsen av Facebook från hello-galleriet.
2. Konfigurera och testa Azure AD enkel inloggning.

## <a name="add-workplace-by-facebook-from-hello-gallery"></a>Lägg till arbetsplatsen av Facebook från hello-galleriet
tooconfigure hello integrering av arbetsplatsen av Facebook i Azure AD, lägga till arbetsplatsen av Facebook från hello galleriet tooyour lista över hanterade SaaS-appar.

1. I hello [Azure-portalen](https://portal.azure.com), i hello vänstra rutan, Välj **Azure Active Directory**. 

    ![hello Azure Active Directory-knappen][1]

2. Bläddra för**företagsprogram** > **alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd hello nya program, Välj **nytt program** överst hello hello dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **arbetsplats av Facebook**, och välj **arbetsplats av Facebook** resultaten. Välj sedan **Lägg till**, tooadd hello program.

    ![Arbetsplats av Facebook i hello resultatlistan](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD SSO till arbetsplats av Facebook, baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren arbetsplatsen av Facebook är tooa i Azure AD. Med andra ord ska ett länkat förhållande mellan en Azure AD-användare och hello relaterade användare i arbetsplats av Facebook upprättas.

Etablera den här relationen genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** arbetsplatsen av Facebook.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

Du aktiverar Azure AD SSO i hello Azure-portalen i det här avsnittet och du konfigurerar enkel inloggning i din arbetsplats Facebook-programmet.

1. I hello Azure-portalen på hello **arbetsplats av Facebook** programmet integration anger **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. I hello **enkel inloggning** dialogrutan **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. I hello **arbetsplats Facebook-domänen och URL: er** avsnittet, hello följande:

    a. I hello **inloggnings-URL** text skriver en URL som använder hello följande mönster:`https://<company subdomain>.facebook.com`

    b. I hello **identifierare** text skriver en URL som använder hello följande mönster:`https://www.facebook.com/company/<scim company id>`

    > [!NOTE]
    > Dessa värden är bara ett exempel. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta hello [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/) tooget dessa värden. 

4. I hello **SAML-signeringscertifikat** väljer **certifikat (Base64)**, och sedan spara hello certifikatfilen på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Välj **Spara**.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. I hello **arbetsplats Facebook konfigurationen** väljer **konfigurera arbetsplats av Facebook** tooopen hello **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens** avsnitt.

    ![Arbetsplats av Facebook-konfiguration](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. Logga in tooyour arbetsplats av Facebook företagets plats som en administratör i ett annat webbläsarfönster.
  
   > [!NOTE] 
   > Som en del av hello SAML autentiseringsprocessen, kan arbetsplatsen använda frågesträngar för in too2.5 kilobyte i storlek i ordning toopass parametrar tooAzure AD.

8. I hello **företagets instrumentpanelen**, gå toohello **autentisering** fliken.

9. Under **SAML-autentisering**väljer **SSO endast** hello nedrullningsbara listan.

10. Ange värden för hello kopieras från hello **arbetsplats Facebook konfigurationen** avsnitt i hello Azure-portalen i hello motsvarande fält:

    *   I den **SAML URL** klistra in hello värdet för textrutan **inloggning tjänst-URL för enkel**, som du har kopierat från hello Azure-portalen.
    *   I den **utfärdar-URL för SAML** klistra in hello värdet för textrutan **SAML enhets-ID**, som du har kopierat från hello Azure-portalen.
    *   I **SAML logga ut omdirigera (valfritt)**, klistra in hello värdet för **Sign-Out URL**, som du har kopierat från hello Azure-portalen.
    *   Öppna din **Base64-kodat certifikat** i anteckningar som hämtats från hello Azure-portalen. Kopiera hello innehållet i den till Urklipp och klistra in den toothe **SAML certifikat** textruta.

11. Du kan behöva tooenter hello målgruppen URL mottagande URL och ACS (Assertion konsumenten Service) URL som visas under hello **SAML-konfiguration** avsnitt.

12. Rulla toohello längst ned i avsnittet hello och markera **Test SSO**. Ett popup-fönster visas med hello Azure AD-inloggningssida. tooauthenticate, ange dina autentiseringsuppgifter som vanligt. Kontrollera hello e-postadress returnerade tillbaka från Azure AD är hello samma som hello arbetsplatskonto som du är inloggad med.

13. Rulla toohello längst ned på sidan hello och välj om hello test har slutförts, **spara**.

14. Alla som använder arbetsplats kan nu se med Azure AD-inloggningssida för autentisering.

Du kan välja tooconfigure SAML utloggning URL, vilket kan vara används toopoint vid utloggning hello Azure AD-sidan. När den här inställningen har aktiverats och konfigurerats, hello användaren är inte längre utloggning sidan för dirigerad toohello arbetsyta. I stället är hello användaren omdirigerade toohello URL som har lagts till i hello SAML utloggning omdirigering inställningen.


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello app. När du lägger till den här appen från hello **Active Directory** > **företagsprogram** väljer du helt enkelt hello **enkel inloggning** fliken och åtkomst hello inbäddade dokumentationen via hello **Configuration** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen i hello [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>Konfigurera omautentisering frekvens

Du kan konfigurera arbetsplats tooprompt för SAML kontroll varje dag, tre dagar, en vecka, två veckor, en månad eller aldrig.

> [!NOTE] 
>hello minsta hello SAML-kontroll av mobila program värdet för tooone vecka.

Du kan också tvinga en SAML återställa för alla användare. toodo detta, Använd hello **kräver SAML-autentisering för alla användare nu** knappen.


### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

1. I hello **Azure-portalen**, i hello vänstra rutan, Välj **Azure Active Directory**.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper**, och välj **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan **Lägg till**.
 
    ![hello webbinställningar](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. I hello **användaren** dialogrutan rutan, hello följande:
 
    ![hello användardialogrutan](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** textruta, typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet**, och Skriv ned.

    d. Välj **Skapa**.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Skapa en arbetsplats av Facebook testanvändare

I det här avsnittet skapas en användare som kallas Britta Simon i arbetsplats med Facebook. Arbetsplats av Facebook stöder just-in-time-allokering som är aktiverad som standard.

Det finns inga åtgärder i det här avsnittet. Om en användare inte finns i arbetsplats av Facebook, skapas en ny när du försöker tooaccess arbetsplats med Facebook.

>[!Note]
>Om du behöver toocreate en användare manuellt kan kontakta hello [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/).

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure SSO genom att bevilja åtkomst tooWorkplace av Facebook.

![Tilldela användare][200] 

1. I hello Azure visas portal, öppna hello program. Gå toohello directory vyn finns för**företagsprogram**, och välj sedan **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **arbetsplats av Facebook**.

    ![hello arbetsplats av Facebook-länken i listan med program hello](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Välj hello menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Välj **Lägg till**. Sedan hello **Lägg uppdrag** väljer **användare och grupper**.

    ![hello Lägg uppdrag fönstret][203]

5. I hello **användare och grupper** dialogrutan **Britta Simon** i hello användarlistan.

6. I hello **användare och grupper** dialogrutan **Välj**.

7. I hello **Lägg uppdrag** dialogrutan **tilldela**.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.
Mer information finns i [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).


## <a name="next-steps"></a>Nästa steg

* Se hello [lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md).
* Läs [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).
* Läs mer om hur för[konfigurera användaretablering](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png

