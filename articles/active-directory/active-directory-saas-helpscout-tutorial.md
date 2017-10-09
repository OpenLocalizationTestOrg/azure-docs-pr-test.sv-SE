---
title: "Självstudier: Azure Active Directory-integrering med hjälp Scout | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och hjälpa Scout."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 58edd140eb1eb5980796ca743b5f7acd891729a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Självstudier: Azure Active Directory-integrering med hjälp Scout

I kursen får du lära dig hur toointegrate hjälpa säljare med Azure Active Directory (AD Azure).

Du får hello följande fördelar från hjälp Scout integrera med Azure AD:

- Du kan styra vem som har åtkomst tooHelp Scout i Azure AD.
- Du kan logga in automatiskt dina användare tooHelp Scout med hjälp av enkel inloggning och användarens Azure AD-kontot.
- Du kan hantera dina konton i en enda central plats, hello Azure-portalen.

toolearn mer information om programvara som en tjänst (SaaS) app-integrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooset av Azure AD-integrering med hjälp Scout måste hello följande objekt:

- En Azure AD-prenumeration
- En prenumeration som hjälper Scout med enkel inloggning aktiverad 

> [!NOTE]
> Om du testar hello stegen i den här kursen rekommenderar vi att du inte testa dem i en produktionsmiljö.

Rekommendationer för att testa hello stegen i den här självstudiekursen:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [skaffa en kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. 

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till hjälp Scout från hello-galleriet.
2. Konfigurera och testa Azure AD enkel inloggning.

## <a name="add-help-scout-from-hello-gallery"></a>Lägg till hjälp Scout från hello-galleriet
tooset in hello integration av hjälpa Scout med Azure AD hello Gallery och lägga till hjälp Scout tooyour lista över hanterade SaaS-appar.

tooadd hjälper Scout från galleriet hello:

1. I hello [Azure-portalen](https://portal.azure.com), i hello vänstra menyn, Välj **Azure Active Directory**. 

    ![hello Azure Active Directory-knappen][1]

2. Välj **företagsprogram**, och välj sedan **alla program**.

    ![hello Enterprise program sida][2]
    
3. tooadd nya program, Välj **nytt program**.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **hjälp Scout**. I sökresultaten hello väljer **hjälp Scout**, och välj sedan **Lägg till**.

    ![Underlätta Scout hello resultatlistan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med hjälp Scout baserat på en användare med namnet *Britta Simon*.

För enkel inloggning toowork måste Azure AD tooknow hello Azure AD motsvarighet användare i Hjälp Scout. En länk relationen mellan en Azure AD-användare och hello relaterade användare i Hjälp Scout måste upprättas.

tooestablish hello länka relation i Hjälp Scout för **användarnamn**, tilldela hello värdet för hello **användarnamn** i Azure AD.

tooconfigure och testa Azure AD enkel inloggning med hjälp Scout fullständig hello följande uppgifter:

1. [Konfigurera Azure AD enkel inloggning](#set-up-azure-ad-single-sign-on). Ställer in en användare toouse den här funktionen.
2. [Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user). Testerna Azure AD enkel inloggning med hello användaren Britta Simon.
3. [Skapa en testanvändare hjälpa Scout](#create-a-help-scout-test-user). Skapar en motsvarighet för Britta Simon i Hjälp Scout som är länkade toohello Azure AD-representation av hello användare.
4. [Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user). Ställer in Britta Simon toouse Azure AD enkel inloggning.
5. [Testa enkel inloggning](#test-single-sign-on). Verifierar att hello-konfigurationen fungerar.

### <a name="set-up-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet kan du ställa in Azure AD enkel inloggning i hello Azure-portalen. Sedan kan konfigurera du enkel inloggning i tillämpningsprogrammet att Scout.

tooset in Azure AD enkel inloggning med hjälp Scout:

1. I hello Azure-portalen på hello **hjälp Scout** programmet integration anger **enkel inloggning**.
 
    ![Konfigurera enkel inloggning länk][4]

2. På hello **enkel inloggning** sidan för **läge**väljer **SAML-baserade inloggning**.
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. Under **hjälp Scout domän och URL: er**om du vill tooset in hello program i IDP-initierad läge fullständig hello följande steg:

    1. I hello **identifierare** ange en URL som har hello följande mönster:`urn:auth0:helpscout:<instancename>`

    2. I hello **Reply URL** ange en URL som har hello följande mönster:`https://helpscout.auth0.com/login/callback?connection=<instancename>`

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. Om du vill tooset in hello program i SP-initierad läge, väljer hello **visa avancerade inställningar för URL: en** kryssrutan och sedan hello följande:

    * I hello **inloggning URL** ange en URL som har hello följande format:`https://secure.helpscout.net/members/login/`

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > hello värden i dessa URL: er är bara exempel. Uppdatera hello värden med hello faktiska identifierare Webbadressen och svar. tooget dessa värden, kontakta [hjälp Scout supportteamet](mailto:help@helpscout.com). 

5. Under **SAML-signeringscertifikat**väljer **XML-Metadata för**, och sedan spara hello metadatafil på datorn.

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Välj **Spara**.

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. tooset in enkel inloggning på hello hjälpa Scout sida, skicka hello hämtas metadata XML-filen toohello [hjälp Scout supportteamet](mailto:help@helpscout.com). hello hjälpa Scout supportteamet gäller den här inställningen så att hello SAML enkel inloggning-anslutning är korrekt inställda på båda sidor.

> [!TIP]
> Du kan läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du installerar appen! När du har lagt till hello appen genom att välja **Active Directory** > **företagsprogram**väljer hello **enkel inloggning** fliken. Du kan komma åt inbäddade hello-dokumentationen i hello **Configuration** avsnitt på hello hello sidans nederkant. Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD

I det här avsnittet i hello Azure-portalen skapar du en användare med namnet Britta Simon.

![Skapa en testanvändare i Azure AD][100]

toocreate en testanvändare i Azure AD:

1. Välj i hello Azure-portalen hello vänstra menyn **Azure Active Directory**.

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. toodisplay hello lista över användare, Välj **användare och grupper**, och välj sedan **alla användare**.

    ![Välj användare och grupper och välj sedan alla användare](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. tooopen hello **användare** dialogrutan hello överst i hello **alla användare** väljer **Lägg till**.

    ![hello webbinställningar](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. I hello **användaren** dialogrutan, fullständig hello följande steg:

    1. I hello **namn** ange **BrittaSimon**.

    2. I hello **användarnamn** ange hello användarens Britta Simon e-postadress.

    3. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    4. Välj **Skapa**.

        ![hello användardialogrutan](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a>Skapa en hjälp Scout testanvändare

hello-objekt för det här avsnittet är toocreate en användare med namnet Britta Simon i Hjälp Scout. Hjälp Scout stöder just-in-time (JIT) etablering, som är aktiverad som standard.

Det finns ingen åtgärd eller uppgift toocomplete i det här avsnittet. Om en användare inte redan finns i Hjälp Scout, skapas en ny när du försöker tooaccess hjälpa Scout.

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Tillåt hello användaren Britta Simon toouse Azure AD enkel inloggning genom att tilldela hello användarens konto åtkomst tooHelp Scout.

![Tilldela hello användarroll][200] 

tooassign Britta Simon tooHelp Scout:

1. Öppna hello program i hello Azure-portalen, och sedan gå toohello directory vyn. Välj **företagsprogram**, och välj sedan **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **hjälp Scout**.

    ![hello hjälpa Scout länken i listan med program hello](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. Välj hello vänstra menyn **användare och grupper**.

    ![hello användare och grupper länk][202]

4. Välj **Lägg till**. Klicka sedan på hello **Lägg uppdrag** väljer **användare och grupper**.

    ![hello Lägg uppdrag fönstret][203]

5. På hello **användare och grupper** i hello lista över användare, Välj sidan **Britta Simon**.

6. På hello **användare och grupper** väljer **Välj**.

7. På hello **Lägg uppdrag** väljer **tilldela**.
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.

När du väljer hello hjälpa Scout panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour hjälpa Scout program.

Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

