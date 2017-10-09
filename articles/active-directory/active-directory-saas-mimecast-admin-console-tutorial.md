---
title: "Självstudier: Azure Active Directory-integrering med Mimecast administratörskonsolen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mimecast-administrationskonsolen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5a04a5abd9ff30d484bce0a5c97a1d3e48b69e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Självstudier: Azure Active Directory-integrering med Mimecast-administrationskonsolen

I kursen får du lära dig hur toointegrate Mimecast administrationskonsolen med Azure Active Directory (AD Azure).

Integrera Mimecast administratörskonsolen med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooMimecast administrationskonsolen.
- Du kan låta dina användare tooautomatically get inloggade tooMimecast administrationskonsolen (Single Sign-On) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats - hello Azure-portalen.

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Mimecast administratörskonsolen måste hello följande objekt:

- En Azure AD-prenumeration
- En Mimecast administratörskonsolen enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Mimecast administratörskonsolen från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-mimecast-admin-console-from-hello-gallery"></a>Att lägga till Mimecast administratörskonsolen från hello-galleriet
tooconfigure hello integrering av Mimecast administrationskonsolen i Azure AD, behöver du tooadd Mimecast administratörskonsolen hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Mimecast administratörskonsolen från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![hello Azure Active Directory-knappen][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **Mimecast administratörskonsolen**väljer **Mimecast administratörskonsolen** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.

    ![Mimecast administrationskonsolen i hello resultatlistan](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Mimecast administratörskonsolen baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i administratörskonsolen för Mimecast är tooa i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i administratörskonsolen för Mimecast toobe upprättas.

I administratörskonsolen för Mimecast tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Mimecast administratörskonsolen, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Mimecast administratörskonsolen](#create-a-mimecast-admin-console-test-user)**  -toohave en motsvarighet för Britta Simon i Mimecast Admin-konsolen som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Mimecast-administrationskonsolen.

**tooconfigure Azure AD enkel inloggning med Mimecast administrationskonsolen utför hello följande steg:**

1. I hello Azure-portalen på hello **Mimecast administratörskonsolen** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. På hello **Mimecast Admin Console domän och URL: er** avsnittet, utföra hello följande steg:

    ![URL: er och domänen för Mimecast Admin-konsolen med enkel inloggning information](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    I hello **inloggnings-URL** textruta typen hello URL:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > URL: en hello inloggning är regionspecifika.

4. På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. På hello **Mimecast konsolen Administratörskonfigurationen** klickar du på **konfigurera Mimecast administratörskonsolen** tooopen **konfigurera inloggning** fönster. Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**

    ![Mimecast Administratörskonfigurationen-konsolen](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. Logga in på ditt Mimecast administratörskonsolen som en administratör i en annan webbläsarfönster.

8. Gå för**Services \> programmet**.

    ![Tjänster](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "tjänster")

9. Klicka på **autentisering profiler**.

    ![Profiler för autentisering](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "profiler för autentisering")
    
10. Klicka på **nya autentiseringsprofil**.

    ![Nya autentisering profiler](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "nya profiler för autentisering")

11. I hello **autentiseringsprofil** avsnittet, utföra hello följande steg:

    ![Autentiseringsprofil](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "autentiseringsprofil")
    
    a. I hello **beskrivning** textruta, ange ett namn för din konfiguration.
    
    b. Välj **genomdriva SAML-autentisering för Mimecast administratörskonsolen**.
    
    c. Som **Provider**väljer **Azure Active Directory**.
    
    d. Klistra in **SAML enhets-ID**, som du har kopierat från hello Azure-portalen i hello **utfärdar-URL** textruta.
    
    e. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **inloggnings-URL** textruta.

    f. Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **logga ut URL** textruta.
    
    >[!NOTE]
    >hello är inloggnings-URL och hello logga ut URL-värdet för hello Mimecast administratörskonsolen hello samma.
    
    g. Öppna base-64-certifikat hämtas från Azure-portalen i anteckningar, ta bort hello (”*--*”) och hello sista raden (”*--*”), kopiera hello återstående innehållet i den i Urklipp, och sedan klistra in den toohello **identitetscertifikat Provider (Metadata)** textruta.
    
    h. Välj **tillåta enkel inloggning på**.
    
    Jag. Klicka på **Spara**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985) 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

   ![Skapa en testanvändare i Azure AD][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.

    ![hello webbinställningar](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan utför hello följande steg:

    ![hello användardialogrutan](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Skapa en testanvändare Mimecast administratörskonsolen

I ordning tooenable Azure AD-användare toolog i administratörskonsolen för Mimecast, måste de etableras i Mimecast-administrationskonsolen. Hello gäller Mimecast administratörskonsolen är etablering en manuell aktivitet.

* Du måste tooregister en domän innan du kan skapa användare.

**tooconfigure användaretablering, utför följande steg hello:**

1. Logga in tooyour **Mimecast administratörskonsolen** som administratör.
2. Gå för**kataloger \> internt**.
   
   ![Kataloger](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "kataloger")
3. Klicka på **registrera nya domän**.
   
   ![Registrera en ny domän](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "registrera ny domän")
4. När den nya domänen har skapats, klickar du på **ny adress**.
   
   ![Ny adress](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "ny adress")
5. I hello nya adress dialog, utföra hello följande steg:
   
   ![Spara](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "spara")
   
   a. Typen hello **e-postadress**, **globalt namn**, **lösenord**, och **Bekräfta lösenord** attribut för en giltig Azure AD-kontot du vill tooprovision i hello relaterade textrutor.

   b. Klicka på **Spara**.

>[!NOTE]
>Du kan använda andra Mimecast administratörskonsolen användare skapa verktyg eller API: er som tillhandahålls av Mimecast administratörskonsolen tooprovision Azure AD-användarkonton. 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMimecast administrationskonsolen.

![Tilldela hello användarroll][200] 

**tooassign Britta Simon tooMimecast administrationskonsolen utför hello följande steg:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Mimecast administratörskonsolen**.

    ![Hej Mimecast administratörskonsolen länken i listan med program hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. Hello-menyn hello vänster **användare och grupper**.

    ![Hej ”användare och grupper” länk][202]

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![hello Lägg uppdrag fönstret][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Mimecast administratörskonsolen programmet när du klickar på hello Mimecast administratörskonsolen panelen i hello åtkomstpanelen.
Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

