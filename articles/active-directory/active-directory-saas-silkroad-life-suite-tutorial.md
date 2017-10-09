---
title: "Självstudier: Azure Active Directory-integrering med SilkRoad livslängd Suite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SilkRoad livslängd Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: 07367282ab42b7332f166d64743b4b447aec4935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Självstudier: Azure Active Directory-integrering med SilkRoad livslängd Suite
hello syftet med den här kursen är tooshow du hur toointegrate SilkRoad livslängd Suite med Azure Active Directory (AD Azure). 

Integrera SilkRoad livslängd Suite med Azure AD ger dig hello följande fördelar: 

* Du kan styra i Azure AD som har åtkomst tooSilkRoad livslängd Suite 
* Du kan aktivera din användare tooautomatically get inloggade tooSilkRoad livslängd Suite enkel inloggning (SSO) med sina Azure AD-konton

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav
tooconfigure Azure AD-integrering med SilkRoad livslängd Suite måste hello följande objekt:

* En Azure AD-prenumeration
* En prenumeration SilkRoad livslängd Suite SSO aktiverad

>[!NOTE]
>tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö. 
> 

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

* Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
* Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Scenario-beskrivning
hello syftet med den här kursen är tooenable du tootest Azure AD SSO i en testmiljö.

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SilkRoad livslängd Suite från hello-galleriet 
2. Konfigurera och testa Azure AD-SSO

## <a name="add-silkroad-life-suite-from-hello-gallery"></a>Lägg till SilkRoad livslängd Suite från hello-galleriet
tooconfigure hello integrering av SilkRoad livslängd Suite i Azure AD, behöver du tooadd SilkRoad livslängd Suite hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd SilkRoad livslängd Suite från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**. 
   
    ![Active Directory][1]

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Program][2]

4. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Program][3]

5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
   
    ![Program][4]

6. Skriv i sökrutan hello **SilkRoad livslängd Suite**.
   
    ![Program][5]

7. I resultatfönstret hello väljer **SilkRoad livslängd Suite**, och klicka sedan på **Slutför** tooadd hello program.
   
    ![Program][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD SSO med SilkRoad livslängd Suite baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SilkRoad livslängd Suite tooan användare i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SilkRoad livslängd Suite toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SilkRoad livslängd Suite.

tooconfigure och testa Azure AD enkel inloggning med SilkRoad livslängd Suite, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare SilkRoad livslängd Suite](#creating-a-silkroad-life-suite-test-user)**  -toohave en motsvarighet för Britta Simon i SilkRoad livslängd Suite som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning
hello syftet med det här avsnittet är tooenable Azure AD SSO i hello klassiska Azure-portalen och tooconfigure SSO i SilkRoad livslängd Suite-program.

**tooconfigure Azure AD enkel inloggning med SilkRoad livslängd Suite utför hello följande steg:**

1. Inloggning tooyour SilkRoad företagets webbplats som administratör. 

  >[!NOTE] 
  > tooobtain åtkomst toohello SilkRoad livslängd Suite autentisering program för att konfigurera federation med Microsoft Azure AD kontakta SilkRoad Support eller genom ditt ombud SilkRoad tjänster.
  > 

2. Gå för**tjänstleverantör**, och klicka sedan på **Federation information**. 
   
    ![Azure AD-Single Sign-On][10] 

3. Klicka på **hämta Federationsmetadata**, och sedan spara hello metadatafil på datorn.
   
    ![Azure AD-Single Sign-On][11] 

4. I hello klassiska Azure-portalen på hello **SilkRoad livslängd Suite** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.
   
    ![Konfigurera enkel inloggning][6] 

5. På hello **hur skulle du som användare toosign på tooSilkRoad livslängd Suite** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.
   
    ![Azure AD-Single Sign-On][7] 

6. På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:
   
    ![Azure AD-Single Sign-On][8]   
 1. I hello **logga URL** textruta typen hello URL som används av din användare toosign på tooyour SilkRoad livslängd Suite plats (t.ex.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).  
 2. Öppna hello hämtas **Silkroad** metadatafil. 
 3. Leta upp hello **AssertionConsumerService** tagg och sedan kopiera hello **plats** attribut.         
   
    ![Azure AD-Single Sign-On][21] 
 4. Klistra in hello värdet i hello **Reply URL** textruta.  
 5. Klicka på **Nästa**.

6. På hello **Konfigurera enkel inloggning på SilkRoad livslängd Suite** utför hello följande steg:
   
    ![Azure AD-Single Sign-On][9]  
 1. Klicka på hämta certifikatet och spara hello-filen på datorn.  
 2. Klicka på **Nästa**.

7. I din **SilkRoad** programmet, klickar du på **autentisering källor**.
   
    ![Azure AD-Single Sign-On][12] 

8. Klicka på **Lägg till autentiseringskälla**. 
   
    ![Azure AD-Single Sign-On][13] 

9. I hello **Lägg till autentiseringskälla** avsnittet, utföra hello följande steg: 
   
    ![Azure AD-Single Sign-On][14]  
 1. Under **alternativ 2 - metadatafil**, klickar du på **Bläddra** tooupload hello hämtas metadatafil.  
 2. Klicka på **skapa identitetsleverantör med hjälp av fildata**.

10. I hello **autentisering källor** klickar du på **redigera**. 
    
     ![Azure AD-Single Sign-On][15] 

11. På hello **redigera autentiseringskälla** dialogrutan utföra hello följande steg: 
    
     ![Azure AD-Single Sign-On][16] 
 1. Som **aktiverad**väljer **Ja**.   
 2. I hello **IdP beskrivning** textruta skriver du en beskrivning för konfigurationen (t.ex.: *Azure AD SSO*).  
 3. I hello **IdP namnet** textruta, ange ett namn som är specifika tooyour konfiguration (t.ex.: *Azure SP*).  
 4. Klicka på **Spara**.

12. Inaktivera alla autentisering källor. 
    
     ![Azure AD-Single Sign-On][17]

13. I hello klassiska Azure-portalen på hello **enkel inloggning bekräftelse** klickar du på **nästa**.  
    
     ![Azure AD-Single Sign-On][18]

14. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.
    
     ![Azure AD-Single Sign-On][19]

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.

![Skapa Azure AD-användare][20]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**. 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg: 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. Välj ny användare i din organisation som typ av användare.  
 2. I hello användarnamn **textruta**, typen **BrittaSimon**. 
 3. Klicka på **Nästa**.

6. På hello **användarprofil** dialogrutan utför hello följande steg: 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. I hello **Förnamn** textruta typen **Britta**.    
 2. I hello **efternamn** textruta typ, **Simon**. 
 3. I hello **visningsnamn** textruta typen **Britta Simon**. 
 4. I hello **rollen** väljer **användaren**.
 5. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. Skriv ned hello värdet för hello **nytt lösenord**. 
 2. Klicka på **Complete** (Slutför).   

### <a name="create-a-silkroad-life-suite-test-user"></a>Skapa en testanvändare SilkRoad livslängd Suite
hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i SilkRoad livslängd Suite. Brittas måste ha ett ID för enkel inloggning (kallas ibland tooas en *AuthParam*) som matchar Britta's **e-postadress** i Azure AD.

**toocreate en användare som kallas Britta Simon i SilkRoad livslängd Suite utför hello följande steg:**

- Be din SilkRoad livslängd Suite support-teamet toocreate en användare som har som **SSO ID** attributet hello samma värde som hello **e-postadress** av Britta Simon i Azure AD.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD
hello syftet med det här avsnittet är tooenable Britta Simon toouse Azure SSO genom att ge sina access tooSilkRoad livslängd Suite.

![Tilldela användare][200] 

**tooassign Britta Simon tooSilkRoad livslängd Suite utför hello följande steg:**

1. På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.
   
    ![Tilldela användare][201] 

2. Välj i listan med program hello **SilkRoad livslängd Suite**.
   
    ![Tilldela användare][202] 

3. Hello-menyn överst hello **användare**.
   
    ![Tilldela användare][203] 

4. Markera i hello användare **Britta Simon**.

5. Klicka i hello verktygsfältet hello längst ned **tilldela**.
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a>Testa enkel inloggning
hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.  

När du klickar på hello SilkRoad livslängd Suite-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour SilkRoad livslängd Suite-program.

## <a name="additional-resources"></a>Ytterligare resurser
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





