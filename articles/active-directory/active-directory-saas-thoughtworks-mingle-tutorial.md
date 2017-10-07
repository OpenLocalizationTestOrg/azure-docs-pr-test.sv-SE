---
title: "Självstudier: Azure Active Directory-integrering med Thoughtworks Mingle | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Thoughtworks Mingle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 69d859d9-b7f7-4c42-bc8c-8036138be586
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c17f8e13d2db3de7d228d9b27128d134f98d6cdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Självstudier: Azure Active Directory-integrering med Thoughtworks Mingle

I kursen får du lära dig hur toointegrate Thoughtworks Mingle med Azure Active Directory (AD Azure).

Integrera Thoughtworks Mingle med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooThoughtworks Mingle
- Du kan aktivera din användare tooautomatically get inloggade tooThoughtworks Mingle (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Thoughtworks Mingle, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Thoughtworks Mingle enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Thoughtworks Mingle från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-thoughtworks-mingle-from-hello-gallery"></a>Att lägga till Thoughtworks Mingle från hello-galleriet
tooconfigure hello integrering av Thoughtworks Mingle i Azure AD, behöver du tooadd Thoughtworks Mingle hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Thoughtworks Mingle från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Thoughtworks Mingle**väljer **Thoughtworks Mingle** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Thoughtworks Mingle i hello resultatlistan](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Thoughtworks Mingle baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Thoughtworks Mingle är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Thoughtworks Mingle toobe upprättas.

Tilldela hello värdet för hello i Thoughtworks Mingle **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Thoughtworks Mingle, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Thoughtworks Mingle](#create-a-thoughtworks-mingle-test-user)**  -toohave en motsvarighet för Britta Simon i Thoughtworks Mingle som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Thoughtworks Mingle program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Thoughtworks Mingle:**

1. I hello Azure-portalen på hello **Thoughtworks Mingle** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_samlbase.png)

3. På hello **Thoughtworks Mingle domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och Thoughtworks Mingle domän med enkel inloggning information](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_url.png)

    I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.mingle.thoughtworks.com`

    > [!NOTE] 
    > hello-värdet är inte verkliga. Hello uppdateringsvärde med hello faktiska inloggnings-URL. Kontakta [Thoughtworks Mingle klienten supportteamet](https://support.thoughtworks.com/hc/categories/201743486-Mingle-Community-Support) tooget hello värde. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_400.png)

6. Logga in tooyour **Thoughtworks Mingle** företagets webbplats som administratör.

7. Klicka på hello **Admin** fliken och klicka sedan på **SSO Config**.
   
    ![Fliken Administration](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785157.png "SSO Config")

8. I hello **SSO Config** avsnittet, utföra hello följande steg:
   
    ![SSO Config](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785158.png "SSO Config")
    
    a. metadatafil för tooupload hello klickar du på **Välj fil**. 

    b. Klicka på **spara ändringar**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![hello webbinställningar](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello användardialogrutan](./media/active-directory-saas-thoughtworks-mingle-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="create-a-thoughtworks-mingle-test-user"></a>Skapa en Thoughtworks Mingle testanvändare

Azure AD-användare toobe kan toosign i, måste de vara etablerade toohello Thoughtworks Mingle program med hjälp av deras användarnamn för Azure Active Directory. Hello gäller Thoughtworks Mingle är etablering en manuell aktivitet.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour Thoughtworks Mingle företagets webbplats som administratör.

2. Klicka på **profil**.
   
    ![Ditt första projekt](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785160.png "ditt första projekt")

3. Klicka på hello **Admin** fliken och klicka sedan på **användare**.
   
    ![Användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785161.png "användare")

4. Klicka på **ny användare**.
   
    ![Ny användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785162.png "ny användare")

5. På hello **ny användare** dialogrutan utför hello följande steg:
   
    ![Dialogrutan Ny användare](./media/active-directory-saas-thoughtworks-mingle-tutorial/ic785163.png "ny användare")  
 
    a. Typen hello **inloggningsnamn**, **visningsnamn**, **Välj lösenord**, **Bekräfta lösenord** av ett giltigt Azure AD-kontot som du vill tooprovision relaterade textrutor till hello. 

    b. Som **användartyp**väljer **fullständig användaren**.

    c. Klicka på **skapa den här profilen**.

>[!NOTE]
>Du kan använda något annat Thoughtworks Mingle användarens konto skapas verktyg eller API: er som tillhandahålls av Thoughtworks Mingle tooprovision AAD-användarkonton.
> 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooThoughtworks Mingle.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooThoughtworks Mingle, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Thoughtworks Mingle**.

    ![Hej Thoughtworks Mingle länken i listan med program hello](./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_thoughtworksmingle_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Thoughtworks Mingle programmet när du klickar på hello Thoughtworks Mingle panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thoughtworks-mingle-tutorial/tutorial_general_203.png

