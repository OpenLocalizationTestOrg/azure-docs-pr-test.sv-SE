---
title: "Självstudier: Azure Active Directory-integrering med Intralinks | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Intralinks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 6fa49c932d0c48d4b48e04fe91af9fc86a0c1cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Självstudier: Azure Active Directory-integrering med Intralinks

I kursen får du lära dig hur toointegrate Intralinks med Azure Active Directory (AD Azure).

Integrera Intralinks med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooIntralinks
- Du kan aktivera din användare tooautomatically get inloggade tooIntralinks (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Intralinks, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Intralinks enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Intralinks från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-intralinks-from-hello-gallery"></a>Att lägga till Intralinks från hello-galleriet
tooconfigure hello integrering av Intralinks i Azure AD, behöver du tooadd Intralinks hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Intralinks från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Intralinks**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. Markera hello resultat på panelen **Intralinks**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Intralinks baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Intralinks är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Intralinks toobe upprättas.

I Intralinks, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Intralinks, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Intralinks](#creating-an-intralinks-test-user)**  -toohave en motsvarighet för Britta Simon i Intralinks som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Intralinks program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Intralinks:**

1. I hello Azure-portalen på hello **Intralinks** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. På hello **Intralinks domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Intralinks klienten supportteamet](https://www.intralinks.com/contact-1) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. tooconfigure enkel inloggning på **Intralinks** sida, behöver du toosend hello hämtas **XML-Metadata för** [Intralinks supportteam](https://www.intralinks.com/contact-1). De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-intralinks-test-user"></a>Skapa en testanvändare Intralinks

I det här avsnittet skapar du en användare som kallas Britta Simon i Intralinks. Se tillsammans med [Intralinks supportteam](https://www.intralinks.com/contact-1) tooadd hello användare i hello Intralinks plattform.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIntralinks.

![Tilldela användare][200] 

**tooassign Britta Simon tooIntralinks utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Intralinks**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.

### <a name="add-intralinks-via-or-elite-application"></a>Lägg till Intralinks VIA eller Elite program

Intralinks använder hello samma SSO-identitetsplattformen för alla andra Intralinks program utom affären Nexus program. Så om du planerar toouse något annat program för Intralinks sedan du först tooconfigure enkel inloggning för en primär Intralinks program med hjälp av hello proceduren ovan.

Efter det att du kan följa hello under proceduren tooadd ett annat Intralinks program i din klient som kan använda den här primära programmet för enkel inloggning. 

>[!NOTE]
>Den här funktionen är tillgänglig endast tooAzure AD Premium-SKU kunder och inte tillgängliga för kostnadsfri eller grundläggande SKU-kunder.

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]


2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Intralinks**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. På **Intralinks Lägg till app** utföra hello följande steg:

    ![Lägga till Intralinks VIA eller Elite program](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    a. I **namn** textruta, t.ex. Ange rätt namn av programmet hello **Intralinks Elite**.

    b. Klicka på **Lägg till** knappen.

6.  I hello Azure-portalen på hello **Intralinks** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

7. På hello **enkel inloggning** markerar **läge** som **inloggning länkade**.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. Hämta hello hello SP initierade SSO-URL från [Intralinks team](https://www.intralinks.com/contact-1) för hello andra Intralinks program och ange den i **konfigurera inloggnings-URL** enligt nedan. 
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     Skriv hello-URL som används av dina användare toosign på tooyour Intralinks program med hjälp av hello efter mönster i hello logga på URL: en textruta:
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. Tilldela hello programmet toouser eller grupper som visas i avsnittet hello  **[tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**.

### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Intralinks programmet när du klickar på hello Intralinks panelen i hello åtkomstpanelen.
Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

