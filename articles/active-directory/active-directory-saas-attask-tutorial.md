---
title: "Självstudier: Azure Active Directory-integrering med @Task| Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 0840763622086a02a27cfafff3b741bc66cec498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a>Självstudier: Azure Active Directory-integrering med@Task
hello syftet med den här kursen är tooshow du hur toointegrate @Task med Azure Active Directory (AD Azure).  
Integrera @Task med Azure AD tillhandahåller hello följande fördelar: 

* Du kan styra i Azure AD som har åtkomsttoo@Task
* Du kan aktivera användarna tooautomatically hämta inloggade too@Task (Single Sign-On) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med @Task, behöver du hello följande objekt:

* En Azure AD-prenumeration
* En @Task enkel inloggning aktiverad prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.
> 
> 

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Scenario-beskrivning
hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.  
hello-scenario som beskrivs i den här kursen består av tre huvudsakliga byggblock:

1. Lägga till @Task från hello-galleriet 
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-task-from-hello-gallery"></a>Lägga till @Task från hello-galleriet
tooconfigure hello integrering av @Task till Azure AD, behöver du tooadd @Task hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd @Task från galleriet hello utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**. 
   
    ![Active Directory][1] 
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program][2] 
4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Program][3] 
5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Program][4] 
6. Skriv i sökrutan hello  **@Task** .
   
    ![Program][5] 
7. I resultatfönstret hello väljer  **@Task** , och klicka sedan på **Slutför** tooadd hello program.
   
    ![Program][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
hello syftet med det här avsnittet är tooshow du hur tooconfigure och testa Azure AD med enkel inloggning med @Task baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork Azure AD måste tooknow vilka hello motsvarighet användare i @Task tooan användaren i Azure AD. Med andra ord en länk relation mellan en Azure AD-användare och hello relaterade användare i @Task måste toobe upprättas.   
Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i @Task.

tooconfigure och testa Azure AD enkel inloggning med @Task, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en @Tasktest användare](#creating-a-halogen-software-test-user)**  -toohave en motsvarighet för Britta Simon i @Taskthat är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD-Single Sign-On
hello syftet med det här avsnittet är tooenable Azure AD enkel inloggning i hello klassiska Azure-portalen och tooconfigure enkel inloggning i din @Task program.

**tooconfigure Azure AD enkel inloggning med @Task, utföra hello följande steg:**

1. I hello klassiska Azure-portalen på hello  **@Task**  integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.
   
    ![Konfigurera enkel inloggning][6] 
2. På hello **hur skulle du som användare toosign på too@Task**  väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Azure AD-Single Sign-On][7] 
3. På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:
   
    ![Konfigurera Appinställningar för][8] 
   
     a. I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour @Task program (t.ex.:*https://<Tenant name>.attask ondemand.com*).
   
     b. Klicka på **Nästa**.
4. På hello **Konfigurera enkel inloggning vid @Task**  klickar du på **hämtar metadata**, spara hello metadatafil lokalt på datorn och klicka sedan på **nästa**.
   
    ![Vad är Azure AD Connect?][9] 
5. Inloggning tooyour @Task företagets webbplats som administratör.
6. Gå för**enkel inloggning på konfigurationen**.
7. På hello **enkel inloggning** dialogrutan, utför följande steg hello
   
    ![Konfigurera enkel inloggning][23]
   
    a. Som **typen**väljer **SAML 2.0**.
   
    b. Välj **Service Provider-ID**.
   
    c. Kopiera hello på hello klassiska Azure-portalen, **Remote inloggnings-URL**, och klistra in den i hello **Portal Inloggningswebbadressen** textruta.
   
    d. Kopiera hello på hello klassiska Azure-portalen, **tjänst-URL för enkel Sign-Out**, och klistra in den i hello **Sign-Out URL** textruta.
   
    e. Kopiera hello på hello klassiska Azure-portalen, **ändra lösenord URL**, och klistra in den i hello **ändra lösenord URL** textruta.
   
    f. Klicka på **Spara**.
8. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **nästa**. 
   
    ![Vad är Azure AD Connect?][10]
9. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.  
   
    ![Vad är Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.  

![Skapa Azure AD-användare][20]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**. 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg: 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    a. Välj ny användare i din organisation som typ av användare.
   
    b. I hello användarnamn **textruta**, typen **BrittaSimon**.
   
    c. Klicka på **Nästa**.
6. På hello **användarprofil** dialogrutan utför hello följande steg: 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    a. I hello **Förnamn** textruta typen **Britta**.  
   
    b. I hello **efternamn** textruta typ, **Simon**.
   
    c. I hello **visningsnamn** textruta typen **Britta Simon**.
   
    d. I hello **rollen** väljer **användaren**.

    e. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    a. Skriv ned hello värdet för hello **nytt lösenord**.
   
    b. Klicka på **Complete** (Slutför).   

### <a name="creating-an-task-test-user"></a>Skapa en @Task testanvändare
hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i @Task.

**toocreate en användare som kallas Britta Simon i @Task, utföra hello följande steg:**

1. Logga in tooyour @Task företagets webbplats som administratör.
2. Hello-menyn överst hello **personer**.
3. Klicka på **ny Person**. 
4. Utför följande hello på hello ny Person dialogrutan:
   
    ![Skapa en @Task testanvändare][21] 
   
    a. I hello **Förnamn** textruta skriver ”Britta”.
   
    b. I hello **efternamn** textruta skriver ”Simon”.
   
    c. I hello **e-postadress** textruta Skriv Britta Simon e-postadress i Azure Active Directory.
   
    d. Klicka på **lägga till personen**.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access too@Task.

![Tilldela användare][200] 

**tooassign Britta Simon too@Task, utföra hello följande steg:**

1. På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Tilldela användare][201] 
2. Välj i listan med program hello  **@Task** .
   
    ![Tilldela användare][202] 
3. Hello-menyn överst hello **användare**.
   
    ![Tilldela användare][203] 
4. Markera i hello användare **Britta Simon**.
5. Klicka i hello verktygsfältet hello längst ned **tilldela**.
   
    ![Tilldela användare][205]

### <a name="testing-single-sign-on"></a>Testa enkel inloggning
hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  
När du klickar på hello @Task panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour @Task program.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






