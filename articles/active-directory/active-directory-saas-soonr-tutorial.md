---
title: "Självstudier: Azure Active Directory-integrering med Soonr arbetsplats | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Soonr arbetsplats."
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
ms.openlocfilehash: f950b45d0beceab2fa17b7690c9de81ec6603089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Självstudier: Azure Active Directory-integrering med Soonr arbetsplats

hello syftet med den här kursen är tooshow du hur toointegrate Soonr arbetsplats med Azure Active Directory (AD Azure).  
Integrera Soonr arbetsplats med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooSoonr arbetsplats
- Du kan aktivera din användare tooautomatically get inloggade tooSoonr arbetsplats (Single Sign-On) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med Soonr arbetsplats, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En Soonr arbetsplatsen enkel inloggning på aktiverade prenumeration


> [!NOTE] 
> tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.


tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Scenariobeskrivning
hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.  
hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till Soonr arbetsplats från hello-galleriet
2. Konfigurera och testa Azure AD enkel inloggning


## <a name="adding-soonr-workplace-from-hello-gallery"></a>Att lägga till Soonr arbetsplats från hello-galleriet
tooconfigure hello integrering av Soonr arbetsplats i Azure AD, behöver du tooadd Soonr arbetsplats hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd Soonr arbetsplats från galleriet hello utför hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**. 

    ![Active Directory][1]

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.

    ![Program][2]

4. Klicka på **Lägg till** på hello hello sidans nederkant.

    ![Program][3]

5. På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.
 
    ![Program][4]

6. Skriv i sökrutan hello **Soonr arbetsplats**.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. I resultatfönstret hello väljer **Soonr arbetsplats**, och klicka sedan på **Slutför** tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med Soonr arbetsplats baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Soonr arbetsplats tooan användare i Azure AD. Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Soonr arbetsplats toobe upprättas.  

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** Soonr arbetsplatsen.

tooconfigure och testa Azure AD enkel inloggning med Soonr arbetsplats, måste toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare Soonr arbetsplats](#creating-a-soonr-workplace-test-user)**  -toohave en motsvarighet för Britta Simon Soonr arbetsplatsen som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Soonr arbetsplats.


**Utför följande hello tooconfigure Azure AD enkel inloggning med Soonr arbetsplats:**

1. I hello klassiska Azure-portalen på hello **Soonr arbetsplats** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.

    ![Konfigurera enkel inloggning][6] 

2. På hello **hur skulle du som användare toosign på tooSoonr arbetsplats** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. På hello **konfigurera Appinställningar** dialogrutan utför följande steg hello:.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    a. I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.

    b. Klicka på **Nästa**.

    > [!NOTE] 
    > Observera att detta inte är hello verkliga värdet. Du har tooupdate värdet med hello faktiska inloggning URL. Kontakta Soonr arbetsplats support-teamet tooget det här värdet.

4. På hello **Konfigurera enkel inloggning på Soonr arbetsplats** klickar du på **hämtar metadata** och spara hello-filen på datorn:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. tooget SSO konfigurerats för ditt program Kontakta supportteamet Soonr arbetsplats och ge dem hello följande: 

    • hello hämtas **Metadata** fil

    • hello **utfärdar-URL**

    • hello **SAML SSO-URL**

    • hello **tjänst-URL för enkel Sign-Out**

    >[!NOTE]
    >Det här programmet har ersatts av <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask arbetsplats</a> och du kan se <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">detta</a> självstudiekursen för att konfigurera hello program med Azure AD.
   
6. Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska Azure-portalen, och klicka sedan på **nästa**.

    ![Azure AD-Single Sign-On][10]

7. På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.  
  
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.  

![Skapa Azure AD-användare][20]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.

3. toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. På hello **berätta om den här användaren** dialogrutan utför hello följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    a. Välj ny användare i din organisation som typ av användare.

    b. I hello användarnamn **textruta**, typen **BrittaSimon**.

    c. Klicka på **Nästa**.

6.  På hello **användarprofil** dialogrutan utför hello följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    a. I hello **Förnamn** textruta typen **Britta**.  

    b. I hello **efternamn** textruta typ, **Simon**.

    c. I hello **visningsnamn** textruta typen **Britta Simon**.

    d. I hello **rollen** väljer **användaren**.

    e. Klicka på **Nästa**.

7. På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    a. Skriv ned hello värdet för hello **nytt lösenord**.

    b. Klicka på **Complete** (Slutför).   



### <a name="creating-a-soonr-workplace-test-user"></a>Skapa en testanvändare Soonr arbetsplats

hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon Soonr arbetsplatsen. Se tillsammans med Soonr arbetsplats support-teamet toocreate en användare i hello-plattformen. Du kan höja hello-supportärende med Soonr från <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">här</a>.


### <a name="assigning-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access tooSoonr arbetsplats.

![Tilldela användare][200] 

**tooassign Britta Simon tooSoonr arbetsplats utför hello följande steg:**

1. På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **Soonr arbetsplats**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. Hello-menyn överst hello **användare**.

    ![Tilldela användare][203] 

1. Markera i hello användare **Britta Simon**.

2. Klicka i hello verktygsfältet hello längst ned **tilldela**.

    ![Tilldela användare][205]



### <a name="testing-single-sign-on"></a>Testa enkel inloggning

hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.  
Du bör få automatiskt inloggade tooyour Soonr arbetsplats programmet när du klickar på hello Soonr arbetsplats panelen i hello åtkomstpanelen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
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
