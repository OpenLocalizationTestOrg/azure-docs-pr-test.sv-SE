---
title: "Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Symantec Web Security Service (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9f02b3d4ce2073110c55af4b567b0e3b5a88404f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS)

I den här kursen får du lära dig hur toointegrate ditt Symantec Web Security Service (WSS) konto med ditt konto i Azure Active Directory (AD Azure) så att WSS kan autentisera en användare etablerad på hello Azure AD med hjälp av SAML-autentisering och tvinga användaren eller säkerhetsnivå för gruppregler.

Integrera Symantec Web Security Service (WSS) med Azure AD ger dig hello följande fördelar:

- Hantera alla hello användare och grupper som används av ditt konto för WSS från Azure AD-portalen. 

- Tillåt hello slutet användare tooauthenticate sig själva i WSS med sina Azure AD-autentiseringsuppgifter.

- Aktivera hello tvingande för användare och grupp säkerhetsnivå för regler som definierats i WSS-konto.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Symantec Web Security Service (WSS) behöver du hello följande objekt:

- En Azure AD-prenumeration
- Ett konto med Symantec Web Security Service (WSS)

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte användning av ett WSS-konto som används för närvarande för produktion ändamål.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte din WSS-konto som används för produktion syftet med det här testet om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I den här självstudiekursen kommer du konfigurera din Azure AD tooenable enkel inloggning tooWSS med hello slutanvändaren autentiseringsuppgifter som definierats i Azure AD-kontot.
hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till hello Symantec Web Security Service (WSS) app från galleriet hello
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a>Att lägga till Symantec Web Security Service (WSS) från hello-galleriet
tooconfigure hello integrering av Symantec Web Security Service (WSS) i Azure AD, behöver du tooadd Symantec Web Security Service (WSS) hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Symantec Web Security Service (WSS) från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Symantec Web Security Service (WSS)**väljer **Symantec Web Security Service (WSS)** resultatet-panelen klickar **Lägg till** knappen tooadd hello programmet.

    ![Symantec Web Security Service (WSS) i hello resultatlistan](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS) baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Symantec Web Security Service (WSS) är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Symantec Web Security Service (WSS) toobe upprättas.

I Symantec Web Security Service (WSS), tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS), behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Symantec Web Security Service (WSS)](#create-a-symantec-web-security-service-wss-test-user)**  -toohave en motsvarighet för Britta Simon i Symantec Web Security Service (WSS) som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Symantec Web Security Service (WSS)-program.

**tooconfigure Azure AD enkel inloggning med Symantec Web Security Service (WSS), utför följande steg hello:**

1. I hello Azure-portalen på hello **Symantec Web Security Service (WSS)** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. På hello **URL: er och Symantec Web Service (WSS) säkerhetsdomän** avsnittet, utföra hello följande steg:

    ![URL: er och Symantec Web Security Service (WSS)-domän med enkel inloggning information](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. I hello **identifierare** textruta typen hello URL:`https://saml.threatpulse.net:8443/saml/saml_realm`

    b. I hello **Reply URL** textruta typen hello URL:`https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Kontakta hello [Symantec Web Security Service (WSS) klienten supportteamet](https://www.symantec.com/contact-us) om hello värden för hello **identifierare** och **Reply URL** inte är av någon anledning.

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. Läs toohello WSS onlinedokumentation tooconfigure enkel inloggning på hello Symantec Web Security Service (WSS) sida. hello hämtas **XML-Metadata för** filen måste toobe importeras till hello WSS portal. Kontakta hello [Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us) om du behöver hjälp med hello konfiguration på hello WSS-portalen.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Skapa en testanvändare Symantec Web Security Service (WSS)

I det här avsnittet skapar du en användare som kallas Britta Simon i Symantec Web Security Service (WSS). hello motsvarande end användarnamn kan skapas manuellt i hello WSS-portalen eller vänta tills hello användare eller grupper som är etablerad på hello Azure AD-toobe synkroniseras toohello WSS portalen efter några minuter (~ 15 minuter). Användare måste skapas och aktiveras innan du använder enkel inloggning. hello offentliga IP-adress hello slutanvändarens dator som kommer att använda toobrowse webbplatser måste också toobe etableras i hello Symantec Web Security Service (WSS)-portalen.

> [!NOTE]
> Kontrollera [Klicka här](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget din dator är offentlig IP-adress.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSymantec Web Security Service (WSS).

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooSymantec Web Security Service (WSS) utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Symantec Web Security Service (WSS)**.

    ![hello Symantec Web Security Service (WSS) länken i listan med program hello](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet, du kan testa hello enkel inloggning nu när du har konfigurerat din WSS konto toouse din Azure AD för SAML-autentisering.

När du har konfigurerat dirigeras din webbläsare tooproxy webbtrafik tooWSS när du öppnar din webbläsare och försök toobrowse tooa plats och du kommer att toohello Azure inloggning sidan. Ange hello autentiseringsuppgifterna för slutanvändare för hello test som har etablerats i hello Azure AD (det vill säga BrittaSimon) och tillhörande lösenord. När autentiseringen är kommer du att kunna toobrowse toohello webbplats som du har valt. Du bör skapa en regel på hello WSS sida tooblock BrittaSimon Internet tooa viss plats bör sidan hello WSS block när du försöker toobrowse toothat webbplats som användare BrittaSimon.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png

