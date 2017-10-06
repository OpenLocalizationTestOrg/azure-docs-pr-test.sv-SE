---
title: "Självstudier: Azure Active Directory-integrering med RFPIO | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och RFPIO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 87187076-7b50-4247-814f-f217b052703f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: e0c692276276edd8f859e73d81cf54d75a65957a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rfpio"></a>Självstudier: Azure Active Directory-integrering med RFPIO

I kursen får du lära dig hur toointegrate RFPIO med Azure Active Directory (AD Azure).

Integrera RFPIO med Azure AD ger dig hello följande fördelar:

- Du kan styra vem i Azure AD som har åtkomst till tooRFPIO.
- Du kan låta dina användare tooautomatically get inloggade tooRFPIO (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats--hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med RFPIO, behöver du hello följande objekt:

- En Azure AD-prenumeration.
- En RFPIO enkel inloggning på aktiverat prenumeration.

> [!NOTE]
> Vi rekommenderar inte att du använder en tootest hello produktionsmiljön i den här kursen.

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Använd inte din produktionsmiljö om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägger till RFPIO från hello-galleriet.
2. Konfigurera och testa Azure AD enkel inloggning.

## <a name="add-rfpio-from-hello-gallery"></a>Lägg till RFPIO från hello-galleriet
tooconfigure hello integrering av RFPIO i Azure AD, behöver du tooadd RFPIO hello galleriet tooyour listan över hanterade SaaS-appar.

### <a name="tooadd-rfpio-from-hello-gallery"></a>tooadd RFPIO från hello-galleriet

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, Välj hello **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Välj **företagsprogram**, och välj sedan **alla program**.

    ![Program][2]
    
3. tooadd ett nytt program väljer hello **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **RFPIO**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_search.png)

5. Markera hello resultat på panelen **RFPIO**, och välj sedan hello **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RFPIO baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello relationen är mellan motsvarighet användare i RFPIO och en användare i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i RFPIO toobe upprättas.

Tilldela hello värdet i RFPIO, **användarnamn** i Azure AD som hello värde för **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med RFPIO, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**--tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**--tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare RFPIO](#creating-a-rfpio-test-user)**  --toohave en motsvarighet för Britta Simon i RFPIO som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**--tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  --tooverify om hello konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt RFPIO program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med RFPIO:**

1. I hello Azure-portalen på hello **RFPIO** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_samlbase.png)

3. På hello **RFPIO domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url.png)

    a. I hello **identifierare** textruta typen hello URL:`https://www.rfpio.com`

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url1.png)

    b. Kontrollera **visa avancerade inställningar för URL: en**.

    c. I hello **Relay tillstånd** textrutan anger du ett strängvärde. Kontakta [RFPIO supportteam](https://www.rfpio.com/contact/) tooget det här värdet. 

4. Kontrollera **visa avancerade inställningar för URL: en**. Om du inte vill tooconfigure hello programmet i **SP** initierade läge:   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_url2.png)

    I hello **inloggning URL** textruta typen hello URL:`https://www.app.rfpio.com`

5. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_certificate.png) 

6. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_general_400.png)

7. I en annan webbläsarfönster inloggningen toohello **RFPIO** webbplats som administratör.

8. Klicka på listrutan för hello nedre vänstra hörnet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app1.png)

9. Klicka på hello **organisationsinställningar**. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app2.png)

10. Klicka på hello **funktioner & INTEGRATION**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app4.png)

11. I hello **SAML SSO Configuration** klickar du på **redigera**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app3.png)

12. Utför följande åtgärder i det här avsnittet:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app5.png)
    
    a. Kopiera hello innehållet i hello **hämtas XML-Metadata för** och klistra in den i hello **identitet configuration** fältet.

    > [!NOTE]
    >toocopy hello innehållet i hämtas **XML-Metadata för** Använd **anteckningar ++** eller rätt **XML-redigerare**. 

    b. Klicka på **Validera**.

    c. När du klickar på **verifiera**, vänd **SAML(Enabled)** tooon.

    d. Klicka på **skicka**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rfpio-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-rfpio-test-user"></a>Skapa en testanvändare RFPIO

tooenable Azure AD-användare toolog i tooRFPIO, måste de etableras i RFPIO.  
Hello gäller RFPIO är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour RFPIO företagets webbplats som administratör.

2. Klicka på listrutan för hello nedre vänstra hörnet.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app1.png)

3. Klicka på hello **organisationsinställningar**. 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app2.png)

4. Klicka på **GRUPPMEDLEMMAR**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app6.png)

5. Klicka på **Lägg till MEDLEMMAR**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app7.png)

6. I hello **lägga till nya medlemmar** avsnitt. Utför följande åtgärder:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/app8.png)

    a. Ange **e-postadress** i hello **ange en e-postadress per rad** fältet.

    b. Välj Plese **rollen** enligt dina krav.

    c. Klicka på **Lägg till MEDLEMMAR**.
        
    > [!NOTE]
    > hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRFPIO.

![Tilldela användare][200] 

**tooassign Britta Simon tooRFPIO utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **RFPIO**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rfpio-tutorial/tutorial_rfpio_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour RFPIO programmet när du klickar på hello RFPIO panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rfpio-tutorial/tutorial_general_203.png

