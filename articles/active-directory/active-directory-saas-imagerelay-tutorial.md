---
title: "Självstudier: Azure Active Directory-integrering med bild Relay | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och avbildningen Relay."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: baf39e4437b85c2de5b524984ad5ca39badbab63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>Självstudier: Azure Active Directory-integrering med bild Relay

I kursen får du lära dig hur toointegrate avbildningen Relay med Azure Active Directory (AD Azure).

Integrera avbildningen Relay med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooImage Relay
- Du kan aktivera din användare tooautomatically get inloggade tooImage Relay (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med bild Relay måste hello följande objekt:

- En Azure AD-prenumeration
- En avbildning Relay enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till bilden Relay från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-image-relay-from-hello-gallery"></a>Att lägga till bilden Relay från hello-galleriet
tooconfigure hello integrering av avbildningen Relay i Azure AD, behöver du tooadd avbildningen Relay hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd bild Relay från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **avbildningen Relay**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. Markera hello resultat på panelen **avbildningen Relay**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med bild Relay baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i avbildningen Relay är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i avbildningen Relay toobe upprätta.

I bild Relay tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med bild Relay måste toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en avbildning Relay testanvändare](#creating-an-image-relay-test-user)**  -toohave en motsvarighet för Britta Simon i avbildningen Relay som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i avbildningen Relay-program.

**Utför följande hello tooconfigure Azure AD enkel inloggning med bild Relay:**

1. I hello Azure-portalen på hello **avbildningen Relay** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. På hello **avbildningen Relay domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    a. I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.imagerelay.com/`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [avbildningen Relay klienten supportteamet](http://support.imagerelay.com/) tooget dessa värden. 
 


4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. På hello **konfiguration av avbildningen Relay** klickar du på **konfigurera avbildningen Relay** tooopen **konfigurera inloggning** fönster. Kopiera hello **Sign-Out URL: en och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. Logga in tooyour avbildningen Relay företagets webbplats som en administratör i ett nytt webbläsarfönster.

8. Klicka på hello i hello verktygsfältet överst hello **användare och behörigheter** arbetsbelastning.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. Klicka på **nya behörighet att skapa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. I hello **enkel inloggning på inställningar** arbetsbelastning, Välj hello **den här gruppen kan bara logga in via enkel inloggning** kryssrutan och klicka sedan på **spara**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. Gå för**kontoinställningar**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. Gå toohello **enkel inloggning på inställningar** arbetsbelastning.
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. På hello **SAML inställningar** dialogrutan utföra hello följande steg:
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    a. I **inloggnings-URL** textruta klistra in hello värdet för **inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.

    b. I **logga ut URL** textruta klistra in hello värdet för **tjänst-URL för enkel Sign-Out** som du har kopierat från Azure-portalen.

    c. Som **Format för namn-Id**väljer **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.

    d. Som **bindning alternativ för begäranden från hello Service Provider (bild relä)**väljer **POST bindning**.

    e. Under **x.509-certifikat**, klickar du på **uppdatering certifikat**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Öppna hello hämtat certifikat i anteckningar, kopiera hello innehållet och klistra in den i textrutan för hello x.509-certifikat.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. I **in Användaretablering** avsnitt, Välj hello **Aktivera in Användaretablering**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Välj hello behörighetsgruppen (till exempel **SSO grundläggande**) som tillåts toosign i endast via enkel inloggning.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    Jag. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-image-relay-test-user"></a>Skapa en avbildning Relay testanvändare

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i avbildningen Relay.

**toocreate en användare som kallas Britta Simon i avbildningen Relay utför hello följande steg:**

1. Inloggning tooyour avbildningen Relay företagets webbplats som administratör.

2. Gå för**användare och behörigheter** och välj **skapa SSO användare**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. Ange hello **e-post**, **Förnamn**, **efternamn**, och **företagets** hello användare som du vill tooprovision och välj hello behörighetsgruppen (till exempel SSO grundläggande) vilket är hello-grupp som kan logga in endast via enkel inloggning.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. Klicka på **Skapa**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooImage Relay.

![Tilldela användare][200] 

**tooassign Britta Simon tooImage Relay, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **avbildningen Relay**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.    

Du bör få automatiskt inloggade tooyour avbildningen Relay programmet när du klickar på hello avbildningen Relay-panelen i hello åtkomstpanelen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

