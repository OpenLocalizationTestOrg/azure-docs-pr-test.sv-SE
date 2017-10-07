---
title: "Självstudier: Azure Active Directory-integrering med SciQuest tillbringar Director | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SciQuest tillbringar Director."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 47c46f1297054fd96b86c1d8c66e1a55ec151497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Självstudier: Azure Active Directory-integrering med SciQuest tillbringar Director
hello syftet med den här kursen är tooshow du hur toointegrate SciQuest tillbringar Director med Azure Active Directory (AD Azure).  
Integrera SciQuest tillbringar Director med Azure AD ger dig hello följande fördelar: 

* Du kan styra i Azure AD som har åtkomst tooSciQuest utgifter Director 
* Du kan aktivera din användare tooautomatically get inloggade tooSciQuest utgifter Director (Single Sign-On) med sina Azure AD-konton
* Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med SciQuest tillbringar Director, behöver du hello följande objekt:

* En Azure AD-prenumeration
* En SciQuest tillbringar Director enkel inloggning på aktiverade prenumeration

> [!NOTE]
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.
> 
> 

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Scenario-beskrivning
hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.  
hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SciQuest tillbringar Director från hello-galleriet 
2. Konfigurera och testa Azure AD enkel inloggning

## <a name="adding-sciquest-spend-director-from-hello-gallery"></a>Att lägga till SciQuest tillbringar Director från hello-galleriet
tooconfigure hello integrering av SciQuest tillbringar Director i Azure AD, behöver du tooadd SciQuest tillbringar Director hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SciQuest tillbringar Director från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**. 
   
    ![Active Directory][1]

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program][2]

4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Program][3]

5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Program][4]

6. Skriv i sökrutan hello **sciQuest tillbringar director**.
   
    ![Program][5]

7. I resultatfönstret hello väljer **SciQuest tillbringar Director**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![Program][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med SciQuest tillbringar Director baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SciQuest tillbringar Director tooan användare i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i SciQuest tillbringar Director toobe upprättas.  
Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SciQuest tillbringar Director.

tooconfigure och testa Azure AD enkel inloggning med SciQuest tillbringar Director, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enda enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare SciQuest tillbringar Director](#creating-a-halogen-software-test-user)**  -toohave en motsvarighet för Britta Simon i SciQuest tillbringar Director som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Konfigurera Azure AD-en enkel inloggning
hello syftet med det här avsnittet är tooenable Azure AD enkel inloggning i hello klassiska Azure-portalen och tooconfigure enkel inloggning i ditt SciQuest tillbringar Director-program.

**tooconfigure Azure AD enkel inloggning med SciQuest tillbringar Director utför hello följande steg:**

1. I hello klassiska Azure-portalen på hello **SciQuest tillbringar Director** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**dialogrutan.
   
    ![Konfigurera enkel inloggning][8]

2. På hello **hur skulle du som användare toosign på tooSciQuest utgifter Director** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Azure AD-Single Sign-On][9]

3. På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg: 
   
    ![Konfigurera Appinställningar för][10]
   
     a. I hello **logga URL** textruta, Skriv URL: en som används av dina användare toosign på tooyour SciQuest tillbringar Director program med hjälp av hello följer mönstret: *https://.* SciQuest.com/.**
   
     b. I hello **Reply URL** textruta, typen hello samma värde som du har skrivit in i hello **logga URL** textruta. 
   
     c. Klicka på **Nästa**.

4. På hello **Konfigurera enkel inloggning på SciQuest tillbringar Director** klickar du på **hämtar metadata**, och sedan spara hello metadatafil lokalt på datorn.
   
    ![Vad är Azure AD Connect?][11]

5. Kontakta SciQuest support tooenable den här autentiseringsmetoden använder hello ovan hämtas metadata.

6. Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan. 
   
    ![Vad är Azure AD Connect?][15]

7. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.  

### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Vad är Azure AD Connect?][100] 

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.
   
    ![Vad är Azure AD Connect?][101] 

4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**. 
   
    ![Vad är Azure AD Connect?][102] 

5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg:
   
    ![Vad är Azure AD Connect?][103] 
   
    a. Som **typ av användare**väljer **ny användare i din organisation**.
   
    b. I hello användarnamn **textruta**, typen **BrittaSimon**.
   
    c. Klicka på **Nästa**.

6. På hello **användarprofil** dialogrutan utför hello följande steg: 
   
    ![Vad är Azure AD Connect?][104] 
   
    a. I hello **Förnamn** textruta typen **Britta**.  
   
    b. I hello **efternamn** txtbox typ, **Simon**.
   
    c. I hello **visningsnamn** textruta typen **Britta Simon**.
   
    d. I hello **rollen** väljer **användaren**.
   
    e. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Vad är Azure AD Connect?][105]  

8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:
   
    ![Vad är Azure AD Connect?][106]   
   
    a. Skriv ned hello värdet för hello **nytt lösenord**.
   
    b. Klicka på **Complete** (Slutför).   

### <a name="creating-a-sciquest-spend-director-test-user"></a>Skapa en testanvändare SciQuest tillbringar Director
hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i SciQuest tillbringar Director.

Du behöver toocontact supportteamet SciQuest tillbringar Director och ge dem hello information om din test konto tooget som den har skapats.

Du kan också användas just-in-time etablering, en enkel inloggning funktion som stöds av SciQuest tillbringar Director.  
Om just-in-time-etablering aktiveras kan skapas användare automatiskt av SciQuest tillbringar Director under ett försök att enkel inloggning om de inte redan finns. Den här funktionen eliminerar behovet av hello toomanually skapa motsvarighet för enkel inloggning användare.

tooget just-in-time-etablering aktiverad måste du toocontact du är supportteamet SciQuest tillbringar Director.

### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access tooSciQuest utgifter Director.

![Vad är Azure AD Connect?][200]

**tooassign Britta Simon tooSciQuest utgifter Director utför hello följande steg:**

1. På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Vad är Azure AD Connect?][201]

2. Välj i listan med program hello **SciQuest tillbringar Director**.
   
    ![Vad är Azure AD Connect?][202]

3. Hello-menyn överst hello **användare**.
   
    ![Vad är Azure AD Connect?][203]

4. Markera i hello användare **Britta Simon**.
   
    ![Vad är Azure AD Connect?][204]

5. Klicka i hello verktygsfältet hello längst ned **tilldela**.
   
    ![Vad är Azure AD Connect?][205]

### <a name="testing-single-sign-on"></a>Testa enkel inloggning
hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  
Du bör få automatiskt inloggade tooyour SciQuest tillbringar Director programmet när du klickar på hello SciQuest tillbringar Director-panelen i hello åtkomstpanelen.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

