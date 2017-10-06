---
title: "Självstudier: Azure Active Directory-integrering med Bynder | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: a2a8477580d28fe422f2836f483dff286bc71c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a>Självstudier: Azure Active Directory-integrering med Bynder
hello syftet med den här kursen är tooshow du hur toointegrate Bynder med Azure Active Directory (AD Azure).

Integrera Bynder med Azure AD ger dig hello följande fördelar:

* Du kan styra i Azure AD som har åtkomst till tooBynder
* Du kan aktivera för dina användare tooautomatically get inloggade tooBynder enkel inloggning (SSO) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med Bynder, behöver du hello följande objekt:

* En Azure AD-prenumeration
* En Bynder enkel inloggning (SSO) aktiverat prenumeration

>[!NOTE]
>tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö. 
> 

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
hello syftet med den här kursen är tooenable du tootest Microsoft Azure AD SSO i en testmiljö.

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Bynder från hello-galleriet
2. Konfigurera och testa Microsoft Azure AD-SSO

## <a name="add-bynder-from-hello-gallery"></a>Lägg till Bynder från hello-galleriet
tooconfigure hello integrering av Bynder i Azure AD, behöver du tooadd Bynder hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Bynder från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**. 
   
    ![Active Directory][1]
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program][2]
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Program][3]
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Program][4]
6. Skriv i sökrutan hello **Bynder**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. Markera hello resultat på panelen **Bynder**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![Att välja hello app i hello-galleriet](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a>Konfigurera och testa Microsoft Azure AD-SSO
hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Microsoft Azure AD SSO med Bynder baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bynder tooan användare i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bynder toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Bynder.

tooconfigure och testa Microsoft Azure AD SSO med Bynder, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Microsoft Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Microsoft Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Bynder](#creating-a-bynder-test-user)**  -toohave en motsvarighet för Britta Simon i Bynder som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Microsoft Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-microsoft-azure-ad-sso"></a>Konfigurera Microsoft Azure AD-SSO
I det här avsnittet att aktivera Microsoft Azure AD SSO i hello klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Bynder.

**tooconfigure Microsoft Azure AD SSO med Bynder, utför följande steg hello:**

1. I hello klassiska portalen på hello **Bynder** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.
   
    ![Konfigurera enkel inloggning][6] 
2. På hello **hur skulle du som användare toosign på tooBynder** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. På hello **konfigurera Appinställningar** dialogrutan sidan om du vill tooconfigure hello programmet i **IDP initierade läge**, utföra hello följande och klickar på **nästa**:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.getbynder.com/sso/SAML/authenticate/`
  2. Klicka på **Nästa**.
4. Om du inte vill tooconfigure hello programmet i **SP initierade läge** på hello **konfigurera Appinställningar** dialogrutan sidan, klickar på hello **”visa avancerade inställningar (valfritt)”**och ange sedan hello **logga URL** och på **nästa**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.getbynder.com/login/`
  2. Klicka på **Nästa**.
  
   >[!NOTE]
   >hello-värdet för hello logga URL i den här kursen är bara en placeholfer. tooget hello faktiska värdet för din miljö, kontakta Bynder.
   >

5. På hello **Konfigurera enkel inloggning på Bynder** , utföra hello följande och klickar på **nästa**:
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. Klicka på **hämtar metadata**, och spara sedan hello på datorn.
  2. Klicka på **Nästa**.
6. tooget SSO konfigurerats för programmet, kontaktar du supportteamet Bynder. Koppla hello hämtade metadatafil och dela den med Bynder team tooset in enkel inloggning på sidan.
7. Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska portalen och klicka sedan på **nästa**.
   
    ![Azure AD-Single Sign-On][10]
8. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.  
   
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska portalen kallas Britta Simon.

![Skapa Azure AD-användare][20]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. Välj ny användare i din organisation som typ av användare.
  2. I hello användarnamn **textruta**, typen **BrittaSimon**.
  3. Klicka på **Nästa**.
6. På hello **användarprofil** dialogrutan utför hello följande steg:
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. I hello **Förnamn** textruta typen **Britta**.  
  2. I hello **efternamn** textruta typ, **Simon**. 
  3. I hello **visningsnamn** textruta typen **Britta Simon**.
  4. I hello **rollen** väljer **användaren**.
  5. Klicka på **Nästa**.
7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. Skriv ned hello värdet för hello **nytt lösenord**.
   2. Klicka på **Complete** (Slutför).   

### <a name="create-a-bynder-test-user"></a>Skapa en testanvändare Bynder
hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Bynder. Bynder stöder just-in-time-etablering, vilket är aktiverat som standard.

Det finns ingen åtgärd objekt i det här avsnittet. En ny användare skapas under ett försök tooaccess Bynder om den inte finns.

>[!NOTE]
>Om du behöver toocreate en användare manuellt, måste toocontact hello Bynder supportteamet. 
> 

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure SSO genom att tilldela tooBynder sin åtkomst.

   ![Tilldela användare][200]

**tooassign Britta Simon tooBynder utför hello följande steg:**

1. På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Tilldela användare][201]
2. Välj i listan med program hello **Bynder**.
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. Hello-menyn överst hello **användare**.
   
    ![Tilldela användare][203]
4. Markera i hello användare **Britta Simon**.
5. Klicka i hello verktygsfältet hello längst ned **tilldela**.
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a>Testa enkel inloggning
hello syftet med det här avsnittet är tootest din Microsoft Azure AD SSO konfiguration av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour Bynder programmet när du klickar på hello Bynder panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
