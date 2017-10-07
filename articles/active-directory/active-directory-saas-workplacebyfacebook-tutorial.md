---
title: "Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och arbetsplats med Facebook."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f71a59527394730757d501a973251dc293fd3683
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Självstudier: Azure Active Directory-integrering med arbetsplats av Facebook

I kursen får du lära dig hur toointegrate arbetsplats av Facebook med Azure Active Directory (AD Azure).

Integrera arbetsplats av Facebook med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooWorkplace av Facebook
- Du kan aktivera din användare tooautomatically get inloggade tooWorkplace av Facebook (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med arbetsplats av Facebook, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En arbetsplats av Facebook enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till arbetsplatsen av Facebook från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-workplace-by-facebook-from-hello-gallery"></a>Att lägga till arbetsplatsen av Facebook från hello-galleriet
tooconfigure hello integrering av arbetsplatsen av Facebook i Azure AD, behöver du tooadd arbetsplats av Facebook hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd arbetsplats av Facebook från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **arbetsplats av Facebook**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. Markera hello resultat på panelen **arbetsplats av Facebook**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning till arbetsplats av Facebook baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren arbetsplatsen av Facebook är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i arbetsplats av Facebook toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** arbetsplatsen av Facebook.

tooconfigure och testa Azure AD enkel inloggning till arbetsplats av Facebook, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Konfigurera omautentisering frekvens](#configuring-reauthentication-frequency)**  -tooconfigure arbetsplats tooprompt för en SAML-kontroll.
3. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
4. **[Skapa en arbetsplats av Facebook testanvändare](#creating-a-workplace-by-facebook-test-user)**  -toohave en motsvarighet för Britta Simon arbetsplatsen av Facebook som är länkade toohello Azure AD-representation av användaren.
5. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
6. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet kan du aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning på din arbetsplats genom Facebook-programmet.

**tooconfigure Azure AD enkel inloggning till arbetsplats av Facebook, utför hello följande steg:**

1. I hello Azure-portalen på hello **arbetsplats av Facebook** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. På hello **arbetsplats Facebook-domänen och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<instancename>.facebook.com`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.facebook.com/company/<instancename>`

    > [!NOTE] 
    > Dessa värden är inte hello verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. På hello **arbetsplats Facebook konfigurationen** klickar du på **konfigurera arbetsplats av Facebook** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. I en annan webbläsarfönstret, inloggning tooyour arbetsplats av Facebook företagets plats som administratör.
  
   > [!NOTE] 
   > Som en del av hello SAML autentiseringsprocessen, kan arbetsplatsen använda frågesträngar för in too2.5 kilobyte i storlek i ordning toopass parametrar tooAzure AD.

8. I hello **företagets instrumentpanelen**, gå toohello **autentisering** fliken.

9. Under **SAML-autentisering**väljer **SSO endast** hello nedrullningsbara listan.

10. Inkommande hello värden kopieras från **arbetsplats Facebook konfigurationen** avsnitt i hello Azure-portalen i hello motsvarande fält:

    *   I **SAML URL** textruta klistra in hello värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.
    *   I **utfärdar-URL för SAML-textrutan**, klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.
    *   I **SAML logga ut omdirigera** (valfritt), klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.
    *   Öppna din **Base64-kodat certifikat** i anteckningar som hämtas från Azure-portalen, kopiera hello innehållet i den till Urklipp och klistra in den toothe **SAML certifikat** textruta.

11. Du kan behöva tooenter hello målgruppen URL mottagaren URL och ACS (Assertion konsumenten Service)-URL som visas under hello **SAML-konfiguration** avsnitt.

12. Rulla toohello längst ned på hello avsnittet och klicka på hello **Test SSO** knappen. Detta resulterar i ett popup-fönster som visas med Azure AD-inloggningssidan visas. Ange dina autentiseringsuppgifter i som normal tooauthenticate. 

    **Felsöka:** Kontrollera hello e-postadress som returneras från Azure AD är hello samma som hello arbetsplatskonto som du är inloggad med.

13. När hello test har slutförts, bläddra toohello längst ned på sidan för hello och klicka på hello **spara** knappen.

14. Alla användare som använder arbetsplats visas nu med Azure AD-inloggningssidan för autentisering.

15. **SAML logga ut omdirigera (valfritt)** - 

    Du kan välja toooptionally konfigurera en Url för SAML logga ut, vilket kan vara används toopoint på sidan för Azure AD logga ut. När den här inställningen har aktiverats och konfigurerats, kommer hello användaren inte längre dirigerad toohello arbetsplats logga ut sidan. Hello användaren kommer i stället att omdirigerade toohello url som har lagts till i hello SAML logga ut omdirigera inställningen.


> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="configuring-reauthentication-frequency"></a>Konfigurera omautentisering frekvens

Du kan konfigurera arbetsplats tooprompt för SAML-kontrollen varje dag, tre dagar i veckan, vecka, månad eller aldrig.

> [!NOTE] 
>hello minsta hello SAML-kontroll av mobila program värdet för tooone vecka.

Du kan också tvinga en SAML återställa för alla användare som använder hello knappen: kräver SAML-autentisering för alla användare nu.


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Skapa en arbetsplats av Facebook testanvändare

I det här avsnittet skapas en användare som kallas Britta Simon i arbetsplats med Facebook. Arbetsplats av Facebook stöder just-in-time-allokering som är aktiverad som standard.

Det finns inga åtgärder i det här avsnittet. Om en användare inte finns i arbetsplats av Facebook, skapas en ny när du försöker tooaccess arbetsplats med Facebook.

>[!Note]
>Om du behöver en användare manuellt, kontakta toocreate [arbetsplats av Facebook klienten supportteamet](https://workplace.fb.com/faq/)

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkplace av Facebook.

![Tilldela användare][200] 

**tooassign Britta Simon tooWorkplace av Facebook, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **arbetsplats av Facebook**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

