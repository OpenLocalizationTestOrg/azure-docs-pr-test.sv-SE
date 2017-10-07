---
title: "Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Symantec Web Security Service (WSS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: f4575e6f5eb63cd0e1d63edd35e30be3cacc3382
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Självstudier: Azure Active Directory-integrering med Symantec Web Security Service (WSS)

I kursen får du lära dig hur toointegrate Symantec Web Security Service (WSS) med Azure Active Directory (AD Azure).

Integrera Symantec Web Security Service (WSS) med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSymantec Web Security Service (WSS)
- Du kan aktivera din användare tooautomatically get inloggade tooSymantec Web Security Service (WSS) (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Symantec Web Security Service (WSS) behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Symantec Web Security Service (WSS) enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Symantec Web Security Service (WSS) från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-symantec-web-security-service-wss-from-hello-gallery"></a>Att lägga till Symantec Web Security Service (WSS) från hello-galleriet
tooconfigure hello integrering av Symantec Web Security Service (WSS) i Azure AD, behöver du tooadd Symantec Web Security Service (WSS) hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Symantec Web Security Service (WSS) från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Symantec Web Security Service (WSS)**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_search.png)

5. Markera hello resultat på panelen **Symantec Web Security Service (WSS)**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS) baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Symantec Web Security Service (WSS) är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Symantec Web Security Service (WSS) toobe upprättas.

I Symantec Web Security Service (WSS), tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Symantec Web Security Service (WSS), behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Symantec Web Security Service (WSS)](#creating-a-symantec-web-security-service-wss-test-user)**  -toohave en motsvarighet för Britta Simon i Symantec Web Security Service (WSS) som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Symantec Web Security Service (WSS)-program.

**tooconfigure Azure AD enkel inloggning med Symantec Web Security Service (WSS), utför följande steg hello:**

1. I hello Azure-portalen på hello **Symantec Web Security Service (WSS)** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. På hello **URL: er och Symantec Web Service (WSS) säkerhetsdomän** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. I hello **inloggnings-URL** textruta uppgift hello url som du vill tooblock enligt toohello blockerande princip hello Symantec Web Security Service (WSS).  
    
    b. I hello **identifierare** textruta typen hello URL:`https://saml.threatpulse.net:8443/saml/saml_realm`

    > [!NOTE] 
    > Dessa värden är inte verkliga. Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare. Kontakta [Symantec Web Security Service (WSS) klienten supportteamet](https://www.symantec.com/contact-us) tooget dessa värden. 

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_400.png)

6. tooconfigure enkel inloggning på **Symantec Web Security Service (WSS)** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us).

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-symantec-web-security-service-wss-test-user"></a>Skapa en testanvändare Symantec Web Security Service (WSS)

I det här avsnittet skapar du en användare som kallas Britta Simon i Symantec Web Security Service (WSS). Arbeta med [Symantec Web Security Service (WSS) supportteam](https://www.symantec.com/contact-us) att lägga till hello användare i hello Symantec Web Security Service (WSS)-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning. Också tillsammans med hello användaren information du behöver toosend hello offentliga IP-adress för hello datorn från vilken du försöker tooaccess hello program.

> [!NOTE]
> Kontrollera [Klicka här](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1) tooget din dator är offentlig IP-adress.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSymantec Web Security Service (WSS).

![Tilldela användare][200] 

**tooassign Britta Simon tooSymantec Web Security Service (WSS) utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Symantec Web Security Service (WSS)**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_symantecwebsecurityservicewss_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Symantec Web Security Service (WSS) panelen i hello åtkomstpanelen hämta omdirigerade toohello blockerar sida som hello blockerar principen har konfigurerats.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantecwebsecurityservice(wss)-tutorial/tutorial_general_203.png

