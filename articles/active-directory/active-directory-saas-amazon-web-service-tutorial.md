---
title: "Självstudier: Azure Active Directory-integrering med Amazon Web Services (AWS) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 1b79572ace63f6174ce4fa014c49bf44bd728228
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Självstudier: Azure Active Directory-integrering med Amazon Web Services (AWS)

I kursen får du lära dig hur toointegrate Amazon Web Services (AWS) med Azure Active Directory (AD Azure).

Integrera Amazon Web Services (AWS) med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooAmazon Web Services (AWS)
- Du kan aktivera din användare tooautomatically get inloggade tooAmazon Web Services (AWS) (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

tooenable single sign-on with Amazon Web Services (AWS), it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Amazon Web Services (AWS) behöver du hello följande objekt:

- En Azure AD-prenumeration
- Amazon Web Services (AWS) enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Amazon Web Services (AWS) från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-amazon-web-services-aws-from-hello-gallery"></a>Att lägga till Amazon Web Services (AWS) från hello-galleriet
tooconfigure hello integrering av Amazon Web Services (AWS) i Azure AD, behöver du tooadd Amazon Web Services (AWS) hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Amazon Web Services (AWS) från galleriet hello utför hello följande steg:**

1. I hello  **[Azure Portal](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **Amazon Web Services (AWS)**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. Markera hello resultat på panelen **Amazon Web Services (AWS)**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Amazon Web Services (AWS) baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Amazon Web Services (AWS) är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Amazon Web Services (AWS) toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Amazon Web Services (AWS).

tooconfigure och testa Azure AD enkel inloggning med Amazon Web Services (AWS), behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Amazon Web Services](#creating-an-amazon-web-services-test-user)**  -toohave en motsvarighet för Britta Simon i Amazon Web Services (AWS) och som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Amazon Web Services (AWS).

**tooconfigure Azure AD enkel inloggning med Amazon Web Services (AWS) utför hello följande steg:**

1. I hello Azure-portalen på hello **Amazon Web Services (AWS)** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. På hello **Amazon Web Services (AWS)-domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. hello Amazon Web Services (AWS) program förväntar hello SAML intyg i ett specifikt format. Konfigurera hello följande anspråk för det här programmet. Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:
    
    | Attributets namn  | Attributvärdet | namnområde |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://AWS.Amazon.com/SAML/Attributes |
    | Roll            | User.assignedroles |  https://AWS.Amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Du måste tooconfigure hello användaretablering i Azure AD toofetch alla hello roller från AWS-konsolen. Se hello etablering stegen nedan.

    a. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. I hello **namn** textruta hello attributnamn visas för den raden.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. Från hello **värdet** listan attributvärde för typ hello visas för den raden. Lägg till hello Namespace-värde som anges ovan.
    
    d. Klicka på **OK**.

7. Klicka på **spara** knappen toosave hello inställningar på Azure.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. I ett annat fönster i webbläsaren, inloggning tooyour Amazon Web Services (AWS) företagets webbplats som administratör.

9. Klicka på **konsolen Home**.
   
    ![Konfigurera enkel inloggning][11]

10. Klicka på **IAM** från **säkerhet, identitet och efterlevnad** service.
   
    ![Konfigurera enkel inloggning][12]

11. Klicka på **identitetsleverantörer**, och klicka sedan på **skapa providern**.
   
    ![Konfigurera enkel inloggning][13]

12. På hello **konfigurera Provider** dialogrutan utför hello följande steg:
   
    ![Konfigurera enkel inloggning][14]
 
    a. Som **providertyp**väljer **SAML**.

    b. I hello **providernamn** textruta skriver ett provider-namn (t.ex.: *trä*).

    c. tooupload metadata för hämtade filer, klickar du på **Välj fil**.

    d. Klicka på **nästa steg**.

13. På hello **Kontrollera Providerinformationen** dialogrutan sidan, klickar du på **skapa**. 
    
    ![Konfigurera enkel inloggning][15]

14. Klicka på **roller**, och klicka sedan på **Skapa ny roll**. 
    
    ![Konfigurera enkel inloggning][16]

15. På hello **ange rollnamn** dialogrutan utföra hello följande steg: 
    
    ![Konfigurera enkel inloggning][17] 

    a. I hello **rollnamn** textruta, ange ett rollnamn (t.ex.: *TestUser*). 

    b. Klicka på **nästa steg**.

16. På hello **Välj typ av roll** dialogrutan utföra hello följande steg: 
    
    ![Konfigurera enkel inloggning][18] 

    a. Välj **roll för identitets-providerns åtkomst**. 

    b. I hello **bevilja Web Single Sign-On (WebSSO) åtkomst tooSAML providers** klickar du på **Välj**.

17. På hello **upprätta förtroende** dialogrutan utföra hello följande steg:  
    
    ![Konfigurera enkel inloggning][19] 

    a. Som SAML-provider väljer hello SAML-provider som du skapade tidigare (t.ex.: *trä*)
  
    b. Klicka på **nästa steg**.

18. På hello **Kontrollera rollen förtroende** dialogrutan klickar du på **nästa steg**.
    
    ![Konfigurera enkel inloggning][32]

19. På hello **koppla principen** dialogrutan klickar du på **nästa steg**.
    
    ![Konfigurera enkel inloggning][33]

20. På hello **granska** dialogrutan utföra hello följande steg:
    
    ![Konfigurera enkel inloggning][34]
 
    a. Klicka på **skapa roll**.

    b. Skapa så många roller vid behov och mappa dem toohello identitetsleverantör.

21. Nu konfigurera hello användaretablering toofetch alla hello roller från AWS

    a. I hello AWS-konsolen logga in med ditt konto rot

    b. Klicka på ditt namn i hello övre högra hörnet och klicka sedan på hello **Mina säkerhetsreferenser** alternativet. Då öppnas en skärm som en varning visas. Klicka på knappen hello **säkerhetsreferenser** knappen toopass hello-skärmen.
        
       ![Konfigurera enkel inloggning][36]

       ![Konfigurera enkel inloggning][37]

    c. I hello snabbtangenter avsnittet klickar du på hello **skapa nya åtkomstnyckel** knappen. Detta genererar hello åtkomst nyckel-ID och ett token värde.
    
       ![Konfigurera enkel inloggning][38]

    d. Kopiera båda dessa värden och även hämta den, så att du inte förlorar den.

    e. Klicka på hello Azure-portalen på sidan för hello Amazon Web Services (AWS) programmet integration, **etablering**.
        
       ![Konfigurera enkel inloggning][35]

    f. Ange hello etablering läge för**automatisk**
        
       ![Konfigurera enkel inloggning][39]

    g. Nu i hello **clientsecret** och **hemlighet Token** klistra in hello motsvarande värden som du har kopierat från AWS-konsolen.
    
    h. Du kan klicka på hello **Testanslutningen** knappen tootest hello anslutningen. När det går bra kan du starta hello etablerar anslutningen.
       
       ![Konfigurera enkel inloggning][40]

    Jag. Aktivera nu hello Status för etablering för**på**. Detta startar hämtning hello roller från hello program.

       ![Konfigurera enkel inloggning][41]

    > [!NOTE]
    > Azure AD-etablering-tjänsten körs varje efter vissa tid toosync hello roller från AWS. Du bör se alla hello identitetsleverantör ansluten AWS roller till Azure AD och du kan använda dem när hello programmet toousers eller grupper.

<!--### Next steps

tooensure users can sign-in tooAmazon Web Services (AWS) after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooAmazon Web Services (AWS) in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. I hello **namn** textruta typen **BrittaSimon**.

    b. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.

    c. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.

    d. Klicka på **Skapa**.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Skapa en testanvändare Amazon Web Services

I ordning tooenable Azure AD-användare toolog i tooAmazon Web Services (AWS), måste de etablerats i Amazon Web Services (AWS). I hello fallet för Amazon Web Services (AWS) är etablering en manuell aktivitet.

**tooprovision ett användarkonto, utför följande steg hello:**

1. Logga in tooyour **Amazon Web Services (AWS)** företagets webbplats som administratör.

2. Klicka på hello **konsolen Home** ikon. 
   
    ![Konfigurera enkel inloggning][11]

3. Klicka på identitet och åtkomsthantering. 
   
    ![Konfigurera enkel inloggning][28]

4. I hello instrumentpanelen, klickar du på **användare**, och klicka sedan på **Create New Users**. 
   
    ![Konfigurera enkel inloggning][29]

5. Dialogrutan Skapa användare hello utför hello följande: 
   
    ![Konfigurera enkel inloggning][30]   
    
    a. I hello **ange användarnamn** textrutor, Skriv Brita Simon användarnamn (userprincipalname) i Azure AD.

    b. Klicka på **skapa.**
        
### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooAmazon Web Services (AWS).

![Tilldela användare][200] 

**tooassign Britta Simon tooAmazon Web Services (AWS), utför följande steg hello:**

1. I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Amazon Web Services (AWS)**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. På **Välj roll** fliken, väljer hello rätt roll för hello användare. Dessa roller visas med hello rollnamnet och identitetsnamn för providern. Det här sättet kan du enkelt identifiera hello roller från AWS.

8. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    
### <a name="testing-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.

När du klickar på hello Amazon Web Services (AWS) panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Amazon Web Services (AWS) program. 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
