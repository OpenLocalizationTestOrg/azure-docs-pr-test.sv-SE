---
title: "Självstudier: Azure Active Directory-integrering med IBM Kenexa undersökning Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och IBM Kenexa undersökning Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: cf7ed886b4418ac396ca7056827ee10fd7a19ef1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Självstudier: Azure Active Directory-integrering med IBM Kenexa undersökning Enterprise

I kursen får du lära dig hur toointegrate IBM Kenexa undersökning Enterprise med Azure Active Directory (AD Azure).

Integrera IBM Kenexa undersökning Enterprise med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst tooIBM Kenexa undersökning Enterprise.
- Du kan aktivera tooIBM Kenexa undersökning Enterprise användare tooautomatically inloggningen genom att använda enkel inloggning (SSO) med sina Azure AD-konton.
- Du kan hantera dina konton i en central plats: hello Azure-portalen.

Om du vill tooknow mer om programvara som en tjänst (SaaS) appintegrering med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med IBM Kenexa undersökning Enterprise, behöver du hello följande objekt:

- En Azure AD-prenumeration
- En prenumeration för IBM Kenexa undersökning Enterprise SSO aktiverad

> [!NOTE]
> När du testar hello stegen i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.

tootest hello stegen i den här självstudiekursen, Följ dessa rekommendationer:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD SSO i en testmiljö. hello-scenariot som skisseras i hello kursen består av två huvudsakliga byggblock:

* Att lägga till IBM Kenexa undersökning Enterprise från hello-galleriet
* Konfigurera och testa Azure AD-SSO

## <a name="add-ibm-kenexa-survey-enterprise-from-hello-gallery"></a>Lägg till IBM Kenexa undersökning Enterprise från hello-galleriet
tooconfigure hello integrering av IBM Kenexa undersökning Enterprise i Azure AD och lägga till IBM Kenexa undersökning Enterprise från hello galleriet tooyour lista över hanterade SaaS-appar.

tooadd IBM Kenexa undersökning Enterprise från galleriet hello hello följande:

1. I hello [Azure-portalen](https://portal.azure.com), i hello vänster, klicka på hello **Azure Active Directory** knappen. 

    ![hello Azure Active Directory-knappen][1]

2. Välj **företagsprogram**, och välj sedan **alla program**.

    ![hello Enterprise program bladet][2]
    
3. tooadd ett program, klickar du på hello **nytt program** knappen.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **IBM Kenexa undersökning Enterprise**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. Markera i hello resultat **IBM Kenexa undersökning Enterprise**, och klicka sedan på hello **Lägg till** knappen tooadd hello program.

    ![IBM Kenexa undersökning Enterprise i hello resultatlistan](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD SSO med IBM Kenexa undersökning Enterprise baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooidentify hello IBM Kenexa undersökning Enterprise användaren motsvarighet i Azure AD. Med andra ord upprätta Azure AD en länk förhållandet mellan en Azure AD-användare och en relaterad användare i IBM Kenexa undersökning företag.

tooestablish hello länken relationen, tilldela hello värdet för hello **användarnamn** i IBM Kenexa undersökning företag som hello värde för hello **användarnamn** i Azure AD.

tooconfigure och testa Azure AD SSO med IBM Kenexa undersökning Enterprise fullständig hello byggblock i hello följande två avsnitt.

### <a name="configure-azure-ad-sso"></a>Konfigurera Azure AD för enkel inloggning

I det här avsnittet Aktivera Azure AD SSO i hello Azure-portalen och konfigurera enkel inloggning i IBM Kenexa undersökning Enterprise-programmet hello följande:

1. I hello Azure-portalen på hello **IBM Kenexa undersökning Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![IBM Kenexa undersökning Enterprise Konfigurera enkel inloggning länk][4]

2. I hello **enkel inloggning** i dialogrutan hello **läge** väljer **SAML-baserade inloggning** tooenable enkel inloggning.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. I hello **IBM Kenexa undersökning företagsdomänen och URL: er** avsnittet, utföra hello följande steg:

    ![IBM Kenexa undersökning företagsdomänen och URL: er med enkel inloggning information](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. I hello **identifierare** textruta typ en URL med hello följande mönster:`https://surveys.kenexa.com/<companycode>`

    b. I hello **Reply URL** textruta typ en URL med hello följande mönster:`https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > hello föregående värden är inte verkliga. Uppdatera dem med hello faktiska identifierare och reply URL. tooobtain hello faktiska värden, kontakta hello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw).

4. Under **SAML-signeringscertifikat**, klickar du på **certifikat (Base64)**, och sedan spara hello certifikat filen tooyour dator.

    ![länk för hämtning av hello-certifikat (Base64)](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    hello IBM Kenexa undersökning företagsprogram förväntar tooreceive hello Security intyg Markup Language (SAML) intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar toohello konfigurationen av din token SAML-attribut. hello måste värdet för hello användar-ID-anspråk i hello svar matcha hello SSO-ID som har konfigurerats i hello Kenexa system. toomap hello lämpliga användar-ID i din organisation som SSO Internet Datagram Protocol (IDP), arbeta med hello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw). 

    Som standard anger hello användar-ID som värde för hello användarens huvudnamn (UPN) i Azure AD. Du kan ändra värdet på hello **attributet** fliken som visas i följande skärmbild hello. hello integration fungerar endast när du har slutfört hello mappning av på rätt sätt.
    
    ![dialogrutan för hello användarattribut](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png) 

5. Klicka på **Spara**.

    ![hello Konfigurera enkel inloggning spara knappen](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. tooopen hello **konfigurera inloggning** fönstret under **företagskonfiguration för IBM Kenexa undersökning**, klickar du på **konfigurera IBM Kenexa undersökning Enterprise**. 
 
    ![hello konfigurera IBM Kenexa undersökning Enterprise länk](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. Kopiera hello **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** värden från hello **Snabbreferens** avsnitt.

8. I hello **konfigurera inloggning** fönstret under **Snabbreferens**, kopiera hello **Sign-Out URL**, **SAML enhets-ID**, och  **SAML inloggning tjänst-URL för enkel** värden.

9. tooconfigure SSO på hello **IBM Kenexa undersökning Enterprise** sida, skicka hello hämtas **certifikat (Base64)**, **Sign-Out URL**, **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** värden toohello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> Du kan se tooa kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com) när du ställer in hello app. När du har lagt till hello appen från hello **Active Directory** > **företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** fliken och sedan ansluta till hello inbäddade dokumentationen via hello **Configuration** avsnittet hello slutet. toolearn mer om hello inbäddade dokumentationen funktionen finns [Azure AD inbäddade dokumentationen](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
I det här avsnittet skapar du testanvändare Britta Simon i hello Azure-portalen hello följande:

![Skapa en testanvändare i Azure AD][100]

1. Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.
 
    ![hello webbinställningar](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. I hello **användaren** dialogrutan utför hello följande steg:
 
    ![hello användardialogrutan](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    a. I hello **namn** skriver **BrittaSimon**.

    b. I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.

    c. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    d. Klicka på **Skapa**.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Skapa en testanvändare IBM Kenexa undersökning Enterprise

I det här avsnittet kan du skapa en användare som kallas Britta Simon i IBM Kenexa undersökning Enterprise. 

toocreate användare i hello IBM Kenexa undersökning Enterprise system och mappa hello SSO-ID för dem, kan du arbeta med hello [IBM Kenexa undersökning Enterprise supportteamet](https://www.ibm.com/support/home/?lnk=fcw). Detta SSO-ID-värde ska också vara mappad toohello användaren identifierarvärde från Azure AD. Du kan ändra denna standardinställning på hello **attributet** fliken.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan låta du användare Britta Simon toouse Azure SSO genom att bevilja åtkomst tooIBM Kenexa undersökning Enterprise.

![Tilldela hello användarroll][200] 

tooassign användaren Britta Simon tooIBM Kenexa undersökning Enterprise hello följande:

1. Öppna hello i hello Azure-portalen, **program** visa, gå toohello **Directory** väljer **företagsprogram**, och klicka sedan på **alla program**.

    ![Hej ”företagsprogram” och ”alla program” länkar][201] 

2. I hello **program** väljer **IBM Kenexa undersökning Enterprise**.

    ![hello IBM Kenexa undersökning Enterprise länken i listan med program hello](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. Hello vänster klickar du på **användare och grupper**.

    ![Hej ”användare och grupper” länk][202] 

4. Klicka på hello **Lägg till** knappen och sedan, i hello **Lägg uppdrag** väljer **användare och grupper**.

    ![hello Lägg uppdrag fönstret][203]

5. I hello **användare och grupper** i dialogrutan hello **användare** väljer **Britta Simon**.

6. I hello **användare och grupper** dialogrutan klickar du på hello **Välj** knappen.

7. I hello **Lägg uppdrag** dialogrutan klickar du på hello **tilldela** knappen.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av hello åtkomstpanelen.

När du klickar på hello **IBM Kenexa undersökning Enterprise** panelen i Hej åtkomstpanelen, bör vara loggas du automatiskt tooyour IBM Kenexa undersökning företagsprogram.

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 
