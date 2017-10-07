---
title: "Självstudier: Azure Active Directory-integrering med Dropbox för företag | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Dropbox för företag."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3f33a43ca8fbd60486d7a400ae8246af768376ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Självstudier: Azure Active Directory-integrering med Dropbox för företag

I kursen får du lära dig hur toointegrate Dropbox för företag med Azure Active Directory (AD Azure).

Integrera Dropbox för företag med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooDropbox för företag
- Du kan aktivera din användare tooautomatically get inloggade tooDropbox för företag (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Dropbox för företag måste hello följande objekt:

- En Azure AD-prenumeration
- En Dropbox för Business enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Dropbox för företag från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-dropbox-for-business-from-hello-gallery"></a>Att lägga till Dropbox för företag från hello-galleriet
tooconfigure hello integrering av Dropbox för företag till Azure AD, behöver du tooadd Dropbox för Business hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Dropbox för företag från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **nytt program** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Dropbox för företag**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_search.png)

5. Markera hello resultat på panelen **Dropbox för företag**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Dropbox för företag som baseras på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Dropbox för företag är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Dropbox för företag toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Dropbox för företag.

tooconfigure och testa Azure AD enkel inloggning med Dropbox för företag, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en Dropbox för Business testanvändare](#creating-a-dropbox-for-business-test-user)**  -toohave en motsvarighet för Britta Simon i Dropbox för företag som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Dropbox för affärsprogram.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Dropbox för företag:**

1. I hello Azure-portalen på hello **Dropbox för företag** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. På hello **Dropbox Business domän och URL: er** avsnittet, utföra hello följande steg:

    a. Inloggning tooyour Dropbox för företag-klient. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769509.png "Konfigurera enkel inloggning")
   
    b. Hello navigeringsfönstret hello vänster, klicka på **administratörskonsolen**. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769510.png "Konfigurera enkel inloggning")
   
    c. På hello **administratörskonsolen**, klickar du på **autentisering** i hello vänstra navigeringsfönstret. 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769511.png "Konfigurera enkel inloggning")
   
    d. I hello **enkel inloggning** väljer **aktivera enkel inloggning**, och klicka sedan på **mer** tooexpand i det här avsnittet.  
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769512.png "Konfigurera enkel inloggning")
   
    e. Kopiera hello URL bredvid för**användarna kan logga in genom att ange sina e-postadress eller de kan gå direkt till**. 
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/ic769513.png)
    
    f. På hello Azure-portalen i hello **inloggnings-URL** textruta klistra in hello URL.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url.png)

     I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.dropbox.com/sso/<id>`

    > [!NOTE] 
    > Det här värdet är inte verkliga värde. Uppdatera hello värde med hello faktiska inloggnings-URL du får från deras avsnitt för enkel inloggning. Kontakta [Dropbox för Business Client supportteamet](https://www.dropbox.com/business/contact) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_400.png)

6. På hello **Dropbox för konfiguration av företag** klickar du på **konfigurera Dropbox för företag** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. tooconfigure enkel inloggning på **Dropbox för företag** sidan finns på din Dropbox för företag-klient i hello **enkel inloggning** avsnitt i hello **autentisering** sidan Utför följande steg hello: 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Konfigurera enkel inloggning")
   
    a. Klicka på **krävs**.
   
    b. I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in den i hello **inloggning URL** textruta.

    c. Klicka på **Välj certifikat**, och bläddra sedan tooyour **Base64-kodade certifikatfilen**.

    d. Klicka på **spara ändringar** toocomplete hello konfiguration på din DropBox för företag-klient.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_01.png) 

2.  toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_02.png) 

3. Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dropboxforbusiness-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-dropbox-for-business-test-user"></a>Skapa en Dropbox för Business testanvändare

I det här avsnittet skapas en användare som kallas Britta Simon i Dropbox för företag. Dropbox för företag stöder just-in-time-allokering som är aktiverad som standard.

Det finns ingen åtgärd objekt i det här avsnittet. Om en användare inte redan finns i Dropbox för företag, skapas en ny när du försöker tooaccess Dropbox för företag.

>[!Note]
>Om du behöver en användare manuellt, kontakta toocreate [Dropbox för Business Client supportteamet](https://www.dropbox.com/business/contact) 

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDropbox för företag.

![Tilldela användare][200] 

**tooassign Britta Simon tooDropbox för företag, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Dropbox för företag**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Dropbox för företag-panelen i hello åtkomstpanelen bör du hämta inloggningssidan i din Dropbox för affärsprogram.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurera Användaretablering](active-directory-saas-dropboxforbusiness-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dropboxforbusiness-tutorial/tutorial_general_203.png

