---
title: "Självstudier: Azure Active Directory-integrering med SAP Business objektet molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP Business objektet moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: a3e9bd93897271531f91bcbc50cd361e8a20551e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Självstudier: Azure Active Directory-integrering med SAP Business objektet moln

I kursen får du lära dig hur toointegrate SAP Business objektet moln med Azure Active Directory (AD Azure).

Du får hello följande fördelar när du integrerar SAP Business objektet moln med Azure AD:

- Du kan styra vem som har åtkomst tooSAP Business objektet moln i Azure AD.
- Du kan logga in automatiskt dina användare tooSAP Business objektet moln med hjälp av enkel inloggning och användarens Azure AD-kontot.
- Du kan hantera dina konton i en enda central plats, hello Azure-portalen.

toolearn mer information om programvara som en tjänst (SaaS) app-integrering med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Krav

tooset av Azure AD-integrering med SAP Business objektet molnet måste hello följande objekt:

- En Azure AD-prenumeration
- En SAP Business objektet molnet, med enkel inloggning aktiverad

> [!NOTE]
> Om du testar hello stegen i den här kursen rekommenderar vi att du inte testa dem i en produktionsmiljö.

Rekommendationer för att testa hello stegen i den här självstudiekursen:

- Använd inte i produktionsmiljön, om det är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du [skaffa en kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö. 

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Lägg till SAP Business objektet moln från hello-galleriet.
2. Konfigurera och testa Azure AD enkel inloggning.

## <a name="add-sap-business-object-cloud-from-hello-gallery"></a>Lägg till SAP Business objektet moln från hello-galleriet
tooset in hello-integration för SAP Business objektet moln med Azure AD hello Gallery och lägga till SAP Business objektet molnet tooyour lista över hanterade SaaS-appar.

tooadd SAP Business objektet moln från galleriet hello:

1. I hello [Azure-portalen](https://portal.azure.com), i hello vänstra menyn, Välj **Azure Active Directory**. 

    ![hello Azure Active Directory-knappen][1]

2. Välj **företagsprogram**, och välj sedan **alla program**.

    ![hello Enterprise program sida][2]
    
3. tooadd nya program, Välj **nytt program**.

    ![hello-knappen för nytt program][3]

4. Skriv i sökrutan hello **SAP Business objektet molnet**.

    ![hello sökrutan](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. Markera hello resultat på panelen **SAP Business objektet molnet**, och välj sedan **Lägg till**.

    ![SAP Business objektet molnet i hello resultatlistan](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning

I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning till SAP Business objektet moln baserat på en användare med namnet *Britta Simon*.

För enkel inloggning toowork måste Azure AD tooknow hello Azure AD motsvarighet användaren i molnet för SAP Business-objektet. En länk relationen mellan en Azure AD-användare och hello relaterade användaren i SAP Business objektet molnet måste upprättas.

tooestablish hello länka relationen i SAP Business objektet molnet för **användarnamn**, tilldela hello värdet för hello **användarnamn** i Azure AD.

tooconfigure och testa Azure AD enkel inloggning med SAP Business objektet moln, fullständig hello följande uppgifter:

1. [Konfigurera Azure AD enkel inloggning](#set-up-azure-ad-single-sign-on). Ställer in en användare toouse den här funktionen.
2. [Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user). Testerna Azure AD enkel inloggning med hello användaren Britta Simon.
3. [Skapa en SAP Business objektet molnet testanvändare](#create-an-sap-business-object-cloud-test-user). Skapar en motsvarighet för Britta Simon i SAP Business objektet moln som är länkade toohello Azure AD-representation av hello användare.
4. [Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user). Ställer in Britta Simon toouse Azure AD enkel inloggning.
5. [Testa enkel inloggning](#test-single-sign-on). Verifierar att hello-konfigurationen fungerar.

### <a name="set-up-azure-ad-single-sign-on"></a>Konfigurera Azure AD enkel inloggning

I det här avsnittet Aktivera du Azure AD enkel inloggning i hello Azure-portalen. Sedan kan ställa du in enkel inloggning i ditt program för SAP Business objektet molnet.

tooset in Azure AD enkel inloggning med SAP Business objektet molnet:

1. I hello Azure-portalen på hello **SAP Business objektet molnet** programmet integration anger **enkel inloggning**.

    ![Välja enkel inloggning][4]

2. På hello **enkel inloggning** sidan för **läge**väljer **SAML-baserade inloggning**.
 
    ![Välj SAML-baserade inloggning](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Under **SAP Business objektet molnet domän och URL: er**fullständig hello följande steg:

    1. I hello **inloggnings-URL** ange en URL som har hello följande mönster: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. I hello **identifierare** ange en URL som har hello följande mönster:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![URL: er och SAP Business objektet molnet domän webbadresser](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > hello värden i dessa URL: er är bara exempel. Uppdatera hello värden med hello faktiska inloggning URL och identifierare URL. tooget hello inloggnings-URL, kontakta hello [SAP Business objektet Cloud klienten supportteamet](https://www.sap.com/product/analytics/cloud-analytics.support.html). Du kan hämta hello identifierare URL genom att hämta hello SAP Business objektet molnmetadata från hello-administrationskonsolen. Detta beskrivs senare i självstudiekursen hello. 

4. Under **SAML-signeringscertifikat**väljer **XML-Metadata för**. Spara hello metadatafil på datorn.

    ![Välj XML-Metadata](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Välj **Spara**.

    ![Klicka på Spara](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. Logga in tooyour SAP Business objektet molnet företagets platsen som en administratör i en annan webbläsarfönster.

7. Välj **menyn** > **System** > **Administration**.
    
    ![Välj menyn och sedan på System och Administration](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. På hello **säkerhet** fliken, väljer hello **redigera** (penna) ikon.
    
    ![På fliken Säkerhet hello Välj hello redigeringsikonen](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. För **autentiseringsmetod**väljer **SAML enkel inloggning (SSO)**.

    ![Välj SAML enkel inloggning för hello autentiseringsmetod](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. toodownload hello service provider metadata (steg 1), Välj **hämta**. Hello metadatafil hitta och kopiera hello **ID för entiteterna** värde. I hello Azure portal under **SAP Business objektet molnet domän och URL: er**, klistra in hello värde i hello **identifierare** rutan.

    ![Kopiera och klistra in hello ID för entiteterna värde](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. tooupload hello service provider metadata (steg 2) i hello-filen som du hämtade från hello Azure-portalen under **överför identitetsleverantör metadata**väljer **överför**.  

    ![Välj överför under överför identitetsleverantör metadata](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. I hello **användarattribut** listan, Välj hello användarattribut (steg3) som du vill toouse för din implementering. Den här användarattribut mappar toohello identitetsleverantör. tooenter ett anpassat attribut hello användare på sidan Använd hello **mappning av anpassade SAML** alternativet. Du kan också välja antingen **e-post** eller **ANVÄNDAR-ID** som hello användarattribut. Vi valde i vårt exempel **e-post** som vi har mappats hello identifierare användaranspråk med hello **userprincipalname** attribut i hello **användarattribut** avsnitt i hello Azure-portalen. Detta ger ett unikt e-post, som skickas toohello molnapp SAP Business objekt i varje lyckat SAML-svar.

    ![Välj användarattribut](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. tooverify hello konto med hello identitetsprovider (steg 4) i hello **inloggningen autentiseringsuppgifter (e-post)** ange hello användarens e-postadress. Markera **verifiera kontot**. hello läggs inloggningsuppgifter toohello användarkonto.

    ![Ange e-postadress och välj verifiera konto](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. Välj hello **spara** ikon.

    ![Spara ikon](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> Du kan läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du installerar appen! När du har lagt till hello appen genom att välja **Active Directory** > **företagsprogram**väljer hello **enkel inloggning** fliken. Du kan komma åt inbäddade hello-dokumentationen i hello **Configuration** avsnitt på hello hello sidans nederkant. Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
I det här avsnittet skapar du en användare med namnet Britta Simon i hello Azure-portalen.

toocreate en testanvändare i Azure AD:

1. Välj i hello Azure-portalen hello vänstra menyn **Azure Active Directory**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. toodisplay hello lista över användare, Välj **användare och grupper**, och välj sedan **alla användare**.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. tooopen hello **användare** dialogrutan **Lägg till**.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. I hello **användaren** dialogrutan, fullständig hello följande steg:
 
    1. I hello **namn** ange **BrittaSimon**.

    2. I hello **användarnamn** ange hello hello användarens Britta Simon e-postadress.

    3. Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.

    4. Välj **Skapa**.

        ![hello användardialogrutan](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Skapa Azure AD-användare][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Skapa en SAP Business objektet molnet testanvändare

Azure AD-användare måste etableras i SAP Business objektet moln innan de kan logga in tooSAP Business objektet molnet. I SAP Business objektet moln är etablering en manuell aktivitet.

tooprovision ett användarkonto:

1. Logga in tooyour SAP Business objektet molnet företagets platsen som en administratör.

2. Välj **menyn** > **säkerhet** > **användare**.

    ![Lägga till medarbetare](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. På hello **användare** tooadd ny användarinformation på sidan Välj  **+** . 

    ![Lägg till användare på sidan](./media/active-directory-saas-sapboc-tutorial/user4.png)

    Slutför hello följande steg:

    1. I hello **ANVÄNDAR-ID** ange hello användar-ID för hello användaren som **Britta**.

    2. I hello **Förnamn** ange hello förnamn hello användaren som **Britta**.

    3. I hello **efternamn** ange hello efternamn hello användaren som **Simon**.

    4. I hello **VISNINGSNAMN** ange hello hello användarens fullständiga namn som **Britta Simon**.

    5. I hello **e-post** ange hello hello användarens e-postadress som  **brittasimon@contoso.com** .

    6. På hello **Välj roller** väljer hello rätt roll för hello användare, och välj sedan **OK**.

      ![Välja en roll](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. Välj hello **spara** ikon.  


### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet Tillåt hello användaren Britta Simon toouse Azure AD enkel inloggning genom att tilldela hello användarens konto åtkomst tooSAP Business objektet molnet.

tooassign Britta Simon tooSAP Business objektet molnet:

1. Öppna hello program i hello Azure-portalen, och sedan gå toohello directory vyn. Välj **företagsprogram**, och välj sedan **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **SAP Business objektet molnet**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. Välj hello vänstra menyn **användare och grupper**.

    ![Välj användare och grupper][202] 

4. Välj **Lägg till**. Klicka sedan på hello **Lägg uppdrag** väljer **användare och grupper**.

    ![hello tilldelning av lägger du till sidan][203]

5. På hello **användare och grupper** i hello lista över användare, Välj sidan **Britta Simon**.

6. På hello **användare och grupper** väljer **Välj**.

7. På hello **Lägg uppdrag** väljer **tilldela**.

![Tilldela hello användarroll][200] 
    
### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av hello åtkomstpanelen.

När du väljer hello SAP Business objektet molnet panelen i hello åtkomstpanelen bör vara loggas du automatiskt tooyour molnapp SAP Business-objekt.

Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur toointegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

