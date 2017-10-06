---
title: "Självstudier: Azure Active Directory-integrering med Cisco Spark | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Cisco Spark."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 386c4fd816095e1c61de01dd1dee1bbd00311a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Självstudier: Azure Active Directory-integrering med Cisco Spark

I kursen får du lära dig hur toointegrate Cisco Väck med Azure Active Directory (AD Azure).

Integrera Cisco Spark med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooCisco Spark
- Du kan aktivera din användare tooautomatically get inloggade tooCisco Spark (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Cisco Spark, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Cisco-Spark enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Cisco Spark från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-cisco-spark-from-hello-gallery"></a>Att lägga till Cisco Spark från hello-galleriet
tooconfigure hello integrering av Cisco Spark i Azure AD, behöver du tooadd Cisco Spark hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Cisco Spark från galleriet hello utför hello följande steg:**

1. I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Cisco Spark**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_search.png)

5. Markera hello resultat på panelen **Cisco Spark**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Cisco Spark baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Cisco Spark är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Cisco Spark toobe upprättas.

Tilldela hello värdet för hello i Cisco Spark **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.

tooconfigure och testa Azure AD enkel inloggning med Cisco Spark, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Cisco Spark](#creating-a-cisco-spark-test-user)**  -toohave en motsvarighet för Britta Simon i Cisco Spark som är länkade toohello Azure AD-representation av användaren.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Cisco Spark-program.

**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Cisco Spark:**

1. I hello Azure-portalen på hello **Cisco Spark** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_samlbase.png)

3. På hello **Cisco Spark domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_url.png)

    a. I hello **inloggnings-URL** textruta Skriv en URL som:`https://web.ciscospark.com/#/signin`

    b. I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://idbroker.webex.com/<companyname>`

    > [!NOTE] 
    > Det här värdet är inte verkliga. Uppdatera det här värdet med hello faktiska identifierare. Kontakta [Cisco Spark klient supportteamet](https://support.ciscospark.com/) tooget det här värdet. 
 
4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_certificate.png) 

5. Cisco Spark program förväntar hello SAML intyg toocontain specifika attribut. Konfigurera hello följande attribut för det här programmet. Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_07.png) 

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:
    
    | Attributets namn  | Attributvärdet |
    | --------------- | -------------------- |    
    |   UID    | User.userPrincipalName |   

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_attribute_05.png)
    
    b. I hello **namn** textruta hello attributnamn visas för den raden.
    
    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden.
    
    d. Klicka på **OK**.

7. Klicka på **spara** knappen.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_400.png)

8. Logga in för[Cisco Molnhantering samarbete](https://admin.ciscospark.com/) med din fullständiga administratörsautentiseringsuppgifter.

9. Välj **inställningar** och under hello **autentisering** klickar du på **ändra**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)
    
10. Välj **integrera en identitetsleverantör med 3 part. (Avancerat)**  och gå toohello nästa skärm.

11. På hello **importera Idp Metadata** sidan antingen dra och släppa hello Azure AD-metadatafil på hello sida eller använda hello filen webbläsare alternativet toolocate och överför hello Azure AD-metadatafil. Markera **kräver certifikat som signerats av en certifikatutfärdare i Metadata (säkrare)** och på **nästa**. 
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

12. Välj **SSO Testanslutningen**, och när en ny webbläsarflik öppnas autentisera med Azure AD genom att logga in.

13. Returnera toohello **Cisco samarbete Molnhantering** flik i webbläsaren. Om hello testet lyckades, väljer **det här testet lyckades. Aktivera enkel inloggning alternativet** och på **nästa**.

> [!TIP]
> Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!  När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello. Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-a-cisco-spark-test-user"></a>Skapa en testanvändare Cisco Spark

I det här avsnittet skapar du en användare som kallas Britta Simon i Cisco Spark. I det här avsnittet skapar du en användare som kallas Britta Simon i Cisco Spark.

1. Gå toohello [Cisco Molnhantering samarbete](https://admin.ciscospark.com/) med din fullständiga administratörsautentiseringsuppgifter.

2. Klicka på **användare** och sedan **hantera användare**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. I hello **hantera användare** väljer **manuellt lägga till eller ändra användare** och på **nästa**.

4. Välj **namn och e-postadress**. Sedan fylla hello textrutan på följande sätt:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 
    
    a. I hello **Förnamn** textruta typen **Britta**. 
    
    b. I hello **efternamn** textruta typen **Simon**.
    
    c. I hello **e-postadress** textruta typen  **britta.simon@contoso.com** .

5. Klicka på hello plus logga tooadd Britta Simon. Klicka sedan på **Nästa**.

6. I hello **Lägg till tjänster för användare** -fönstret klickar du på **spara** och sedan **Slutför**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCisco Spark.

![Tilldela användare][200] 

**tooassign Britta Simon tooCisco Spark, utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Cisco Spark**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cisco-spark-tutorial/tutorial_ciscospark_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.

När du klickar på hello Cisco Spark-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Cisco Spark-program.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[100]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png

