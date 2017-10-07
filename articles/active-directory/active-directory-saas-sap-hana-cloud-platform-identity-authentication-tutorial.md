---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA Cloud Platform identitetsverifiering | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP HANA Cloud Platform identitetsverifiering."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 2142770779ddb745555b47fc85b5457b573f9506
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a>Självstudier: Azure Active Directory-integrering med SAP HANA Cloud Platform identitetsverifiering

I kursen får du lära dig hur toointegrate SAP HANA-plattformen identitetsautentisering i molnet med Azure Active Directory (AD Azure). Identitetsverifiering för SAP HANA Cloud Platform används som en proxy IdP tooaccess SAP-program med Azure AD som hello huvudsakliga IdP.

Integrera identitetsverifiering för SAP HANA moln plattform med Azure AD ger dig hello följande fördelar:

- Du kan styra i Azure AD som har åtkomst till tooSAP program
- Du kan aktivera för dina användare tooautomatically get inloggade tooSAP program enkel inloggning (SSO) med sina Azure AD-konton
- Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen

Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Krav

tooconfigure Azure AD-integrering med SAP HANA Cloud Platform identitetsverifiering måste hello följande objekt:

- En Azure AD-prenumeration
- En **identitetsverifiering för SAP HANA Cloud Platform** SSO aktiverad prenumeration


>[!NOTE] 
>tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.
>

tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:

- Du bör inte använda produktionsmiljön, om det inte är nödvändigt.
- Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Scenariobeskrivning
I kursen får testa du Azure AD enkel inloggning i en testmiljö.

hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:

1. Att lägga till SAP HANA Cloud Platform identitetsverifiering från hello-galleriet
2. Konfigurera och testa Azure AD-SSO

Innan du dyker hello tekniska detaljer, är det viktigt toounderstand hello begrepp som du kommer toolook på. hello identitetsverifiering för SAP HANA Cloud Platform och Azure Active Directory federation kan du tooimplement SSO över program eller tjänster som skyddas av AAD (som en IdP) med SAP-program och tjänster som skyddas av SAP HANA Cloud Platform identitet Autentisering.

För närvarande fungerar identitetsverifiering för SAP HANA Cloud Platform som Proxy identitetsleverantör tooSAP-program. Azure Active Directory fungerar i sin tur som hello inledande identitetsleverantör i den här installationen. 

hello följande diagram illustrerar detta:    

![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Med den här installationen konfigureras din SAP HANA Cloud Platform identitetsverifiering klient som ett betrodda program i Azure Active Directory. 

Alla SAP-program och tjänster som du vill tooprotect via det här sättet konfigureras senare i hanteringskonsolen för hello identitetsverifiering för SAP HANA Cloud Platform!

Detta innebär att auktorisering för att bevilja åtkomst tooSAP program och tjänster måste tootake plats i identitetsverifiering för SAP HANA moln plattform för en sådan konfiguration (som skillnad från tooconfiguring auktorisering i Azure Active Directory).

Genom att konfigurera identitetsverifiering för SAP HANA moln plattform som ett program via hello Azure Active Directory Marketplace, behöver du inte tootake vård konfigurera nödvändiga enskilda anspråk / SAML intyg och omvandlingar behövs tooproduce en giltig Autentiseringstoken för SAP-program.

>[!NOTE] 
>För närvarande har Web SSO testats av båda parter endast. Flöden som krävs för App-API och API-API-kommunikation ska fungera men har inte testats, ännu. De kommer att testas som en del av efterföljande aktiviteter.
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-hello-gallery"></a>Lägg till SAP HANA Cloud Platform identitetsverifiering från hello-galleriet
tooconfigure hello integrering av identitetsverifiering för SAP HANA Cloud-plattformen i Azure AD, behöver du tooadd identitetsverifiering för SAP HANA Cloud Platform hello galleriet tooyour listan över hanterade SaaS-appar.

**tooadd identitetsverifiering för SAP HANA Cloud Platform från galleriet hello utför hello följande steg:**

1. I hello [ **Azure-hanteringsportalen**](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon. 

    ![Active Directory][1]

2. Navigera för**företagsprogram**. Gå sedan för**alla program**.

    ![Program][2]
    
3. Klicka på **Lägg till** hello längst upp i hello dialogrutan.

    ![Program][3]

4. Skriv i sökrutan hello **identitetsverifiering för SAP HANA Cloud Platform**.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. Markera hello resultat på panelen **identitetsverifiering för SAP HANA Cloud Platform**, och klicka sedan på **Lägg till** knappen tooadd hello program.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa Azure AD enkel inloggning
I det här avsnittet kan du konfigurera och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering baserat på en testanvändare som kallas ”Britta Simon”.

För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i identitetsverifiering för SAP HANA Cloud Platform är tooa i Azure AD. Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i identitetsverifiering för SAP HANA Cloud Platform toobe upprättas.

Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i identitetsverifiering för SAP HANA moln plattform.

tooconfigure och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering, behöver du toocomplete hello följande byggblock:

1. **[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.
2. **[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.
3. **[Skapa en testanvändare för SAP HANA Cloud Platform identitetsverifiering](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  -toohave en motsvarighet för Britta Simon i SAP HANA Cloud Platform identitetsverifiering som är länkade toohello Azure AD-representation av henne.
4. **[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.
5. **[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.

### <a name="configuring-azure-ad-sso"></a>Konfigurera Azure AD för enkel inloggning

I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i din app för identitetsverifiering för SAP HANA moln plattform.

Identitetsverifiering för SAP HANA Cloud Platform program förväntar hello SAML intyg i ett specifikt format. Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet. hello följande skärmbild visar ett exempel för det här.

![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

**tooconfigure Azure AD SSO med identitetsverifiering för SAP HANA moln plattform, utför hello följande steg:**

1. I hello Azure Management portal på hello **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **enkel inloggning**.

    ![Konfigurera enkel inloggning][4]

2. På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.
 
    ![Konfigurera enkel inloggning][5]

3. I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan om tillämpningsprogrammet SAP förväntar sig ett attribut till exempel ”förnamn”. Lägg till hello ”förnamn” attribut i hello SAML-token attribut dialogrutan.
 1. Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.
 
    ![Konfigurera enkel inloggning][6]

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. I hello **attributnamn** textruta typnamn hello attributet ”förnamn”.
 3. Från hello **attributvärdet** listan, Välj hello attributvärdet ”user.givenname”.
 4. Klicka på **OK**.

4. På hello **SAP HANA Cloud Platform identitet autentisering domän och URL: er** avsnittet, utföra hello följande steg:

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. I hello **logga URL** textruta skriver hello logga på URL: en för hello SAP-program.
 2. I hello **identifierare** textruta hello TYPVÄRDE följande mönster:`<entity-id>.accounts.ondemand.com` 
    * Om du inte vet det här värdet, följ hello identitetsverifiering för SAP HANA Cloud Platform dokumentation på [klientkonfiguration SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

5. På hello **konfiguration för SAP HANA Cloud Platform identitet verifiering** klickar du på **konfigurera SAP HANA Cloud Platform identitetsverifiering** tooopen **konfigurerainloggning** dialogrutan. Klicka på **SAML XML-Metadata** och spara hello-filen på datorn.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. tooget SSO konfigurerats för ditt program går tooSAP HANA Cloud Platform identitet autentisering-administrationskonsolen. hello URL har hello följande mönster:`https://<tenant-id>.accounts.ondemand.com/admin`
 * Följ sedan hello dokumentationen på identitetsverifiering för SAP HANA moln plattform för[konfigurera Microsoft Azure AD som företagets identitetsleverantör vid SAP HANA Cloud Platform identitetsverifiering](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

7. Klicka i hello Azure-hanteringsportalen **spara** knappen.
8. Fortsätt hello följande steg om du vill använda tooadd och aktivera enkel inloggning för ett annat SAP-program. Upprepa steg under hello avsnittet ”lägger till SAP HANA-plattformen identitetsautentisering i molnet från galleriet hello” tooadd en annan instans av identitetsverifiering för SAP HANA moln plattform.
9. I hello Azure Management portal på hello **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **inloggning länkade**.

    ![Konfigurera länkad inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. Spara hello konfiguration.

>[!NOTE] 
>hello nytt program använder hello SSO-konfiguration för hello tidigare SAP-programmet. Kontrollera att du Använd hello samma företagets identitetsleverantörer i hello SAP HANA Cloud Platform identitet autentisering-administrationskonsolen.
>

### <a name="create-an-azure-ad-test-user"></a>Skapa en testanvändare i Azure AD
hello syftet med det här avsnittet är toocreate en testanvändare i hello nya portalen kallas Britta Simon.

![Skapa Azure AD-användare][100]

**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**

1. I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. På hello **användaren** dialogrutan utför hello följande steg:
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. I hello **namn** textruta typen **BrittaSimon**.
  2. I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.
  3. Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.
  4. Klicka på **Skapa**. 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a>Skapa en SAP HANA Cloud Platform identitetsverifiering testanvändare

Du behöver inte toocreate en användare på identitetsverifiering för SAP HANA moln plattform. Användare i användararkivet hello Azure AD kan använda hello SSO-funktioner.

Identitetsverifiering för SAP HANA Cloud Platform stöder hello identitetsfederation alternativet. Det här alternativet kan hello programmet toocheck om hello användare som autentiseras av hello företagsidentitet providern finns i hello store för SAP HANA Cloud Platform identitet användarautentisering. 

I hello standardinställningen är hello identitetsfederation alternativet inaktiverat. Om identitetsfederation är aktiverad är endast hello-användare som importeras i identitetsverifiering för SAP HANA Cloud Platform kan tooaccess hello program. 

Mer information om hur tooenable eller inaktivera identitetsfederation med SAP HANA Cloud Platform ID-autentisering, se Aktivera identitetsfederation med identitetsverifiering för SAP HANA Cloud Platform i [konfigurera identitetsfederation med hello Store för SAP HANA Cloud Platform identitet användarautentisering. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-hello-azure-ad-test-user"></a>Tilldela användare hello Azure AD

I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooSAP HANA Cloud Platform identitetsverifiering.

![Tilldela användare][200] 

**tooassign Britta Simon tooSAP HANA Cloud Platform identitetsverifiering, utför hello följande steg:**

1. I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.

    ![Tilldela användare][201] 

2. Välj i listan med program hello **identitetsverifiering för SAP HANA Cloud Platform**.

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. Hello-menyn hello vänster **användare och grupper**.

    ![Tilldela användare][202] 

4. Klicka på **Lägg till** knappen. Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.

    ![Tilldela användare][203]

5. På **användare och grupper** markerar **Britta Simon** i hello användarlistan.

6. Klicka på **Välj** knappen på **användare och grupper** dialogrutan.

7. Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.
    

### <a name="test-single-sign-on"></a>Testa enkel inloggning

I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.

Du bör få automatiskt inloggade tooyour identitetsverifiering för SAP HANA Cloud Platform programmet när du klickar på hello identitetsverifiering för SAP HANA Cloud Platform panelen i hello åtkomstpanelen.


## <a name="additional-resources"></a>Ytterligare resurser

* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png