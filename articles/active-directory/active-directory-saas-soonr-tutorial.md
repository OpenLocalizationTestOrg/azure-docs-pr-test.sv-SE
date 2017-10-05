---
title: "Självstudier: Azure Active Directory-integrering med Soonr arbetsplats | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Soonr arbetsplats."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 76946e4af624d70f2202601ee935523ca3db4314
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Självstudier: Azure Active Directory-integrering med Soonr arbetsplats

Syftet med den här kursen är att visa dig hur du integrerar Soonr arbetsplats med Azure Active Directory (AD Azure).  
Integrera Soonr arbetsplats med Azure AD ger dig följande fördelar:

- Du kan styra i Azure AD som har åtkomst till arbetsplats Soonr
- Du kan aktivera användarna att automatiskt hämta inloggade Soonr arbetsyta (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats – den klassiska Azure-portalen

Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

För att konfigurera Azure AD-integrering med Soonr arbetsplats, behöver du följande:

- En Azure AD-prenumeration
- En Soonr arbetsplatsen enkel inloggning på aktiverade prenumeration


> [!NOTE] 
> Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.


Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
Syftet med den här kursen är att du ska testa Azure AD enkel inloggning i en testmiljö.  
Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Soonr arbetsplats från galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-soonr-workplace-from-the-gallery"></a>Att lägga till Soonr arbetsplats från galleriet
Du måste lägga till Soonr arbetsplats från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Soonr arbetsplats i Azure AD.

**Utför följande steg för att lägga till Soonr arbetsplats från galleriet:**

1. I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**. 

    ![Active Directory][1]

2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.

3. Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.

    ![Program][2]

4. Klicka på **Lägg till** längst ned på sidan.

    ![Program][3]

5. På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.
 
    ![Program][4]

6. I sökrutan skriver **Soonr arbetsplats**.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. I resultatfönstret, Välj **Soonr arbetsplats**, och klicka sedan på **Slutför** lägga till programmet.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
Syftet med det här avsnittet är och hur du kan konfigurera och testa Azure AD enkel inloggning med Soonr arbetsplats utifrån en testanvändare som kallas ”Britta Simon”.

För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i Soonr arbetsplats till en användare i Azure AD är okänt. Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Soonr arbetsplatsen upprättas.  

Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** Soonr arbetsplatsen.

Om du vill konfigurera och testa Azure AD enkel inloggning med Soonr arbetsplats, måste du utföra följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Soonr arbetsplats](#creating-a-soonr-workplace-test-user)**  – du har en motsvarighet för Britta Simon Soonr arbetsplatsen som är kopplad till Azure AD-representation av henne.
4. **[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i den klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Soonr arbetsplats.


**Utför följande steg för att konfigurera Azure AD enkel inloggning med Soonr arbetsplats:**

1. I den klassiska Azure-portalen på den **Soonr arbetsplats** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.

    ![Konfigurera enkel inloggning][6] 

2. På den **hur vill du att användarna kan logga in på Soonr arbetsplats** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. På den **konfigurera Appinställningar** dialogrutan utför följande steg:.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    a. I den **logga URL** textruta Skriv en URL med följande mönster: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.

    b. Klicka på **Nästa**.

    > [!NOTE] 
    > Observera att detta inte är det verkliga värdet. Du måste uppdatera det här värdet med faktiska logga på URL: en. Kontakta supportteamet för Soonr arbetsplats för att hämta det här värdet.

4. På den **Konfigurera enkel inloggning på Soonr arbetsplats** klickar du på **hämtar metadata** och spara filen på datorn:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. För att få SSO konfigurerats för ditt program, Kontakta supportteamet Soonr arbetsplats och ange dem med följande: 

    • Den hämtade **Metadata** fil

    • Den **utfärdar-URL**

    • Den **SAML SSO-URL**

    • Den **enkel URL för utloggning**

    >[!NOTE]
    >Det här programmet har ersatts av <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask arbetsplats</a> och du kan se <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">detta</a> självstudiekursen för att konfigurera programmet med Azure AD.
   
6. Välj konfiguration för enkel inloggning bekräftelse i den klassiska Azure-portalen och klicka sedan på **nästa**.

    ![Azure AD-Single Sign-On][10]

7. På den **enkel inloggning bekräftelse** klickar du på **Slutför**.  
  
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
Syftet med det här avsnittet är att skapa en testanvändare i den klassiska Azure-portalen kallas Britta Simon.  

![Skapa Azure AD-användare][20]

**Utför följande steg för att skapa en testanvändare i Azure AD:**

1. I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.

3. Klicka för att visa en lista över användare, på menyn upp **användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. På den **berätta om den här användaren** dialogrutan utför följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    a. Välj ny användare i din organisation som typ av användare.

    b. I användarnamnet **textruta**, typen **BrittaSimon**.

    c. Klicka på **Nästa**.

6.  På den **användarprofil** dialogrutan utför följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    a. I den **Förnamn** textruta typen **Britta**.  

    b. I den **efternamn** textruta typ, **Simon**.

    c. I den **visningsnamn** textruta typen **Britta Simon**.

    d. I den **rollen** väljer **användaren**.

    e. Klicka på **Nästa**.

7. På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    a. Anteckna värdet för den **nytt lösenord**.

    b. Klicka på **Complete** (Slutför).   



### <a name="creating-a-soonr-workplace-test-user"></a>Skapa en testanvändare Soonr arbetsplats

Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon Soonr arbetsplatsen. Se tillsammans med Soonr arbetsplats supportteamet för att skapa en användare i plattformen. Du kan höja supportärende med Soonr från <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">här</a>.


### <a name="assigning-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändare

Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till arbetsplats Soonr.

![Tilldela användare][200] 

**Om du vill tilldela Soonr arbetsplats Britta Simon utför du följande steg:**

1. På den klassiska Azure-portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.

    ![Tilldela användare][201] 

2. Välj i listan med program **Soonr arbetsplats**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. Klicka på menyn högst upp **användare**.

    ![Tilldela användare][203] 

1. Välj i listan användare **Britta Simon**.

2. Klicka på i verktygsfältet längst ned i **tilldela**.

    ![Tilldela användare][205]



### <a name="testing-single-sign-on"></a>Testa enkel inloggning

Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.  
När du klickar på panelen Soonr arbetsplats på åtkomstpanelen du ska hämta automatiskt loggat in på ditt Soonr arbetsplats program.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
