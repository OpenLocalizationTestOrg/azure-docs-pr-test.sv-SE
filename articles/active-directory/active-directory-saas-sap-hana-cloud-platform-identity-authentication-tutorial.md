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
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a><span data-ttu-id="2c96e-103">Självstudier: Azure Active Directory-integrering med SAP HANA Cloud Platform identitetsverifiering</span><span class="sxs-lookup"><span data-stu-id="2c96e-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform Identity Authentication</span></span>

<span data-ttu-id="2c96e-104">I kursen får du lära dig hur toointegrate SAP HANA-plattformen identitetsautentisering i molnet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2c96e-104">In this tutorial, you learn how toointegrate SAP HANA Cloud Platform Identity Authentication with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2c96e-105">Identitetsverifiering för SAP HANA Cloud Platform används som en proxy IdP tooaccess SAP-program med Azure AD som hello huvudsakliga IdP.</span><span class="sxs-lookup"><span data-stu-id="2c96e-105">SAP HANA Cloud Platform Identity Authentication is used as a proxy IdP tooaccess SAP applications using Azure AD as hello main IdP.</span></span>

<span data-ttu-id="2c96e-106">Integrera identitetsverifiering för SAP HANA moln plattform med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2c96e-106">Integrating SAP HANA Cloud Platform Identity Authentication with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2c96e-107">Du kan styra i Azure AD som har åtkomst till tooSAP program</span><span class="sxs-lookup"><span data-stu-id="2c96e-107">You can control in Azure AD who has access tooSAP application</span></span>
- <span data-ttu-id="2c96e-108">Du kan aktivera för dina användare tooautomatically get inloggade tooSAP program enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2c96e-108">You can enable your users tooautomatically get signed-on tooSAP applications single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="2c96e-109">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2c96e-109">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="2c96e-110">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2c96e-110">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="2c96e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="2c96e-111">Prerequisites</span></span>

<span data-ttu-id="2c96e-112">tooconfigure Azure AD-integrering med SAP HANA Cloud Platform identitetsverifiering måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2c96e-112">tooconfigure Azure AD integration with SAP HANA Cloud Platform Identity Authentication, you need hello following items:</span></span>

- <span data-ttu-id="2c96e-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2c96e-113">An Azure AD subscription</span></span>
- <span data-ttu-id="2c96e-114">En **identitetsverifiering för SAP HANA Cloud Platform** SSO aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2c96e-114">A **SAP HANA Cloud Platform Identity Authentication** SSO enabled subscription</span></span>


>[!NOTE] 
><span data-ttu-id="2c96e-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2c96e-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="2c96e-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2c96e-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2c96e-117">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2c96e-117">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="2c96e-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2c96e-118">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2c96e-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2c96e-119">Scenario description</span></span>
<span data-ttu-id="2c96e-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2c96e-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="2c96e-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2c96e-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2c96e-122">Att lägga till SAP HANA Cloud Platform identitetsverifiering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2c96e-122">Adding SAP HANA Cloud Platform Identity Authentication from hello gallery</span></span>
2. <span data-ttu-id="2c96e-123">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="2c96e-123">Configuring and testing Azure AD SSO</span></span>

<span data-ttu-id="2c96e-124">Innan du dyker hello tekniska detaljer, är det viktigt toounderstand hello begrepp som du kommer toolook på.</span><span class="sxs-lookup"><span data-stu-id="2c96e-124">Before diving into hello technical details, it is vital toounderstand hello concepts you're going toolook at.</span></span> <span data-ttu-id="2c96e-125">hello identitetsverifiering för SAP HANA Cloud Platform och Azure Active Directory federation kan du tooimplement SSO över program eller tjänster som skyddas av AAD (som en IdP) med SAP-program och tjänster som skyddas av SAP HANA Cloud Platform identitet Autentisering.</span><span class="sxs-lookup"><span data-stu-id="2c96e-125">hello SAP HANA Cloud Platform Identity Authentication and Azure Active Directory federation enables you tooimplement SSO across applications or services protected by AAD (as an IdP) with SAP applications and services protected by SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="2c96e-126">För närvarande fungerar identitetsverifiering för SAP HANA Cloud Platform som Proxy identitetsleverantör tooSAP-program.</span><span class="sxs-lookup"><span data-stu-id="2c96e-126">Currently, SAP HANA Cloud Platform Identity Authentication acts as a Proxy Identity Provider tooSAP-applications.</span></span> <span data-ttu-id="2c96e-127">Azure Active Directory fungerar i sin tur som hello inledande identitetsleverantör i den här installationen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-127">Azure Active Directory in turn acts as hello leading Identity Provider in this setup.</span></span> 

<span data-ttu-id="2c96e-128">hello följande diagram illustrerar detta:</span><span class="sxs-lookup"><span data-stu-id="2c96e-128">hello following diagram illustrates this:</span></span>    

![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

<span data-ttu-id="2c96e-130">Med den här installationen konfigureras din SAP HANA Cloud Platform identitetsverifiering klient som ett betrodda program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2c96e-130">With this setup, your SAP HANA Cloud Platform Identity Authentication tenant will be configured as a trusted application in Azure Active Directory.</span></span> 

<span data-ttu-id="2c96e-131">Alla SAP-program och tjänster som du vill tooprotect via det här sättet konfigureras senare i hanteringskonsolen för hello identitetsverifiering för SAP HANA Cloud Platform!</span><span class="sxs-lookup"><span data-stu-id="2c96e-131">All SAP applications and services you want tooprotect through this way are subsequently configured in hello SAP HANA Cloud Platform Identity Authentication management console!</span></span>

<span data-ttu-id="2c96e-132">Detta innebär att auktorisering för att bevilja åtkomst tooSAP program och tjänster måste tootake plats i identitetsverifiering för SAP HANA moln plattform för en sådan konfiguration (som skillnad från tooconfiguring auktorisering i Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="2c96e-132">This means that authorization for granting access tooSAP applications and services needs tootake place in SAP HANA Cloud Platform Identity Authentication for such a setup (as opposed tooconfiguring authorization in Azure Active Directory).</span></span>

<span data-ttu-id="2c96e-133">Genom att konfigurera identitetsverifiering för SAP HANA moln plattform som ett program via hello Azure Active Directory Marketplace, behöver du inte tootake vård konfigurera nödvändiga enskilda anspråk / SAML intyg och omvandlingar behövs tooproduce en giltig Autentiseringstoken för SAP-program.</span><span class="sxs-lookup"><span data-stu-id="2c96e-133">By configuring SAP HANA Cloud Platform Identity Authentication as an application through hello Azure Active Directory Marketplace, you don't need tootake care of configuring needed individual claims / SAML assertions and transformations needed tooproduce a valid authentication token for SAP applications.</span></span>

>[!NOTE] 
><span data-ttu-id="2c96e-134">För närvarande har Web SSO testats av båda parter endast.</span><span class="sxs-lookup"><span data-stu-id="2c96e-134">Currently Web SSO has been tested by both parties, only.</span></span> <span data-ttu-id="2c96e-135">Flöden som krävs för App-API och API-API-kommunikation ska fungera men har inte testats, ännu.</span><span class="sxs-lookup"><span data-stu-id="2c96e-135">Flows needed for App-to-API or API-to-API communication should work but have not been tested, yet.</span></span> <span data-ttu-id="2c96e-136">De kommer att testas som en del av efterföljande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="2c96e-136">They will be tested as part of subsequent activities.</span></span>
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-hello-gallery"></a><span data-ttu-id="2c96e-137">Lägg till SAP HANA Cloud Platform identitetsverifiering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2c96e-137">Add SAP HANA Cloud Platform Identity Authentication from hello gallery</span></span>
<span data-ttu-id="2c96e-138">tooconfigure hello integrering av identitetsverifiering för SAP HANA Cloud-plattformen i Azure AD, behöver du tooadd identitetsverifiering för SAP HANA Cloud Platform hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2c96e-138">tooconfigure hello integration of SAP HANA Cloud Platform Identity Authentication into Azure AD, you need tooadd SAP HANA Cloud Platform Identity Authentication from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2c96e-139">**tooadd identitetsverifiering för SAP HANA Cloud Platform från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2c96e-139">**tooadd SAP HANA Cloud Platform Identity Authentication from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c96e-140">I hello [ **Azure-hanteringsportalen**](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2c96e-140">In hello [**Azure Management portal**](https://portal.azure.com), on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2c96e-142">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-142">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2c96e-143">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-143">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2c96e-145">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-145">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2c96e-147">Skriv i sökrutan hello **identitetsverifiering för SAP HANA Cloud Platform**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-147">In hello search box, type **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. <span data-ttu-id="2c96e-149">Markera hello resultat på panelen **identitetsverifiering för SAP HANA Cloud Platform**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2c96e-149">In hello results panel, select **SAP HANA Cloud Platform Identity Authentication**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="2c96e-151">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2c96e-151">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="2c96e-152">I det här avsnittet kan du konfigurera och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2c96e-152">In this section, you configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2c96e-153">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i identitetsverifiering för SAP HANA Cloud Platform är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2c96e-153">For SSO toowork, Azure AD needs tooknow what hello counterpart user in SAP HANA Cloud Platform Identity Authentication is tooa user in Azure AD.</span></span> <span data-ttu-id="2c96e-154">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i identitetsverifiering för SAP HANA Cloud Platform toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2c96e-154">In other words, a link relationship between an Azure AD user and hello related user in SAP HANA Cloud Platform Identity Authentication needs toobe established.</span></span>

<span data-ttu-id="2c96e-155">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="2c96e-155">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="2c96e-156">tooconfigure och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2c96e-156">tooconfigure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2c96e-157">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-157">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2c96e-158">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c96e-158">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2c96e-159">**[Skapa en testanvändare för SAP HANA Cloud Platform identitetsverifiering](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  -toohave en motsvarighet för Britta Simon i SAP HANA Cloud Platform identitetsverifiering som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="2c96e-159">**[Creating a SAP HANA Cloud Platform Identity Authentication test user](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)** - toohave a counterpart of Britta Simon in SAP HANA Cloud Platform Identity Authentication that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="2c96e-160">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2c96e-160">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2c96e-161">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2c96e-161">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="2c96e-162">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2c96e-162">Configuring Azure AD SSO</span></span>

<span data-ttu-id="2c96e-163">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i din app för identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="2c96e-163">In this section, you enable Azure AD SSO in hello Azure Management portal and configure single sign-on in your SAP HANA Cloud Platform Identity Authentication application.</span></span>

<span data-ttu-id="2c96e-164">Identitetsverifiering för SAP HANA Cloud Platform program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="2c96e-164">SAP HANA Cloud Platform Identity Authentication application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2c96e-165">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="2c96e-165">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="2c96e-166">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="2c96e-166">hello following screenshot shows an example for this.</span></span>

![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

<span data-ttu-id="2c96e-168">**tooconfigure Azure AD SSO med identitetsverifiering för SAP HANA moln plattform, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2c96e-168">**tooconfigure Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c96e-169">I hello Azure Management portal på hello **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-169">In hello Azure Management portal, on hello **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2c96e-171">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2c96e-171">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning][5]

3. <span data-ttu-id="2c96e-173">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan om tillämpningsprogrammet SAP förväntar sig ett attribut till exempel ”förnamn”.</span><span class="sxs-lookup"><span data-stu-id="2c96e-173">In hello **User Attributes** section on hello **Single sign-on** dialog, if your SAP application expects an attribute for example "firstName".</span></span> <span data-ttu-id="2c96e-174">Lägg till hello ”förnamn” attribut i hello SAML-token attribut dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-174">On hello SAML token attributes dialog, add hello "firstName" attribute.</span></span>
 1. <span data-ttu-id="2c96e-175">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
 
    ![Konfigurera enkel inloggning][6]

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. <span data-ttu-id="2c96e-178">I hello **attributnamn** textruta typnamn hello attributet ”förnamn”.</span><span class="sxs-lookup"><span data-stu-id="2c96e-178">In hello **Attribute Name** textbox, type hello attribute name "firstName".</span></span>
 3. <span data-ttu-id="2c96e-179">Från hello **attributvärdet** listan, Välj hello attributvärdet ”user.givenname”.</span><span class="sxs-lookup"><span data-stu-id="2c96e-179">From hello **Attribute Value** list, select hello attribute value "user.givenname".</span></span>
 4. <span data-ttu-id="2c96e-180">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-180">Click **Ok**.</span></span>

4. <span data-ttu-id="2c96e-181">På hello **SAP HANA Cloud Platform identitet autentisering domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2c96e-181">On hello **SAP HANA Cloud Platform Identity Authentication Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. <span data-ttu-id="2c96e-183">I hello **logga URL** textruta skriver hello logga på URL: en för hello SAP-program.</span><span class="sxs-lookup"><span data-stu-id="2c96e-183">In hello **Sign On URL** textbox, type hello sign on URL for hello SAP application.</span></span>
 2. <span data-ttu-id="2c96e-184">I hello **identifierare** textruta hello TYPVÄRDE följande mönster:`<entity-id>.accounts.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="2c96e-184">In hello **Identifier** textbox, type hello value following pattern: `<entity-id>.accounts.ondemand.com`</span></span> 
    * <span data-ttu-id="2c96e-185">Om du inte vet det här värdet, följ hello identitetsverifiering för SAP HANA Cloud Platform dokumentation på [klientkonfiguration SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span><span class="sxs-lookup"><span data-stu-id="2c96e-185">If you don't know this value, please follow hello SAP HANA Cloud Platform Identity Authentication documentation on [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span></span>

5. <span data-ttu-id="2c96e-186">På hello **konfiguration för SAP HANA Cloud Platform identitet verifiering** klickar du på **konfigurera SAP HANA Cloud Platform identitetsverifiering** tooopen **konfigurerainloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-186">On hello **SAP HANA Cloud Platform Identity Authentication Configuration** section, click **Configure SAP HANA Cloud Platform Identity Authentication** tooopen **Configure sign-on** dialog.</span></span> <span data-ttu-id="2c96e-187">Klicka på **SAML XML-Metadata** och spara hello-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2c96e-187">Then, click on **SAML XML Metadata** and save hello file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. <span data-ttu-id="2c96e-190">tooget SSO konfigurerats för ditt program går tooSAP HANA Cloud Platform identitet autentisering-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-190">tooget SSO configured for your application, go tooSAP HANA Cloud Platform Identity Authentication Administration Console.</span></span> <span data-ttu-id="2c96e-191">hello URL har hello följande mönster:`https://<tenant-id>.accounts.ondemand.com/admin`</span><span class="sxs-lookup"><span data-stu-id="2c96e-191">hello URL has hello following pattern: `https://<tenant-id>.accounts.ondemand.com/admin`</span></span>
 * <span data-ttu-id="2c96e-192">Följ sedan hello dokumentationen på identitetsverifiering för SAP HANA moln plattform för[konfigurera Microsoft Azure AD som företagets identitetsleverantör vid SAP HANA Cloud Platform identitetsverifiering](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span><span class="sxs-lookup"><span data-stu-id="2c96e-192">Then, follow hello documentation on SAP HANA Cloud Platform Identity Authentication too[Configure Microsoft Azure AD as Corporate Identity Provider at SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span></span> 

7. <span data-ttu-id="2c96e-193">Klicka i hello Azure-hanteringsportalen **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-193">In hello Azure Management portal, click **Save** button.</span></span>
8. <span data-ttu-id="2c96e-194">Fortsätt hello följande steg om du vill använda tooadd och aktivera enkel inloggning för ett annat SAP-program.</span><span class="sxs-lookup"><span data-stu-id="2c96e-194">Continue hello following steps only if you want tooadd and enable SSO for another SAP application.</span></span> <span data-ttu-id="2c96e-195">Upprepa steg under hello avsnittet ”lägger till SAP HANA-plattformen identitetsautentisering i molnet från galleriet hello” tooadd en annan instans av identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="2c96e-195">Repeat steps under hello section “Adding SAP HANA Cloud Platform Identity Authentication from hello gallery” tooadd another instance of SAP HANA Cloud Platform Identity Authentication.</span></span>
9. <span data-ttu-id="2c96e-196">I hello Azure Management portal på hello **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **inloggning länkade**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-196">In hello Azure Management portal, on hello **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Linked Sign-on**.</span></span>

    ![Konfigurera länkad inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. <span data-ttu-id="2c96e-198">Spara hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2c96e-198">Then, save hello configuration.</span></span>

>[!NOTE] 
><span data-ttu-id="2c96e-199">hello nytt program använder hello SSO-konfiguration för hello tidigare SAP-programmet.</span><span class="sxs-lookup"><span data-stu-id="2c96e-199">hello new application will leverage hello SSO configuration for hello previous SAP application.</span></span> <span data-ttu-id="2c96e-200">Kontrollera att du Använd hello samma företagets identitetsleverantörer i hello SAP HANA Cloud Platform identitet autentisering-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-200">Please make sure you use hello same Corporate Identity Providers in hello SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span>
>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="2c96e-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c96e-201">Create an Azure AD test user</span></span>
<span data-ttu-id="2c96e-202">hello syftet med det här avsnittet är toocreate en testanvändare i hello nya portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2c96e-202">hello objective of this section is toocreate a test user in hello new portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2c96e-204">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2c96e-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c96e-205">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2c96e-205">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2c96e-207">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="2c96e-207">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2c96e-209">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-209">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2c96e-211">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2c96e-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. <span data-ttu-id="2c96e-213">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>
  2. <span data-ttu-id="2c96e-214">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2c96e-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>
  3. <span data-ttu-id="2c96e-215">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>
  4. <span data-ttu-id="2c96e-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-216">Click **Create**.</span></span> 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a><span data-ttu-id="2c96e-217">Skapa en SAP HANA Cloud Platform identitetsverifiering testanvändare</span><span class="sxs-lookup"><span data-stu-id="2c96e-217">Create a SAP HANA Cloud Platform Identity Authentication test user</span></span>

<span data-ttu-id="2c96e-218">Du behöver inte toocreate en användare på identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="2c96e-218">You don't need toocreate an user on SAP HANA Cloud Platform Identity Authentication.</span></span> <span data-ttu-id="2c96e-219">Användare i användararkivet hello Azure AD kan använda hello SSO-funktioner.</span><span class="sxs-lookup"><span data-stu-id="2c96e-219">Users who are in hello Azure AD user store can use hello SSO functionality.</span></span>

<span data-ttu-id="2c96e-220">Identitetsverifiering för SAP HANA Cloud Platform stöder hello identitetsfederation alternativet.</span><span class="sxs-lookup"><span data-stu-id="2c96e-220">SAP HANA Cloud Platform Identity Authentication supports hello Identity Federation option.</span></span> <span data-ttu-id="2c96e-221">Det här alternativet kan hello programmet toocheck om hello användare som autentiseras av hello företagsidentitet providern finns i hello store för SAP HANA Cloud Platform identitet användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="2c96e-221">This option allows hello application toocheck if hello users authenticated by hello corporate identity provider exist in hello user store of SAP HANA Cloud Platform Identity Authentication.</span></span> 

<span data-ttu-id="2c96e-222">I hello standardinställningen är hello identitetsfederation alternativet inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="2c96e-222">In hello default setting, hello Identity Federation option is disabled.</span></span> <span data-ttu-id="2c96e-223">Om identitetsfederation är aktiverad är endast hello-användare som importeras i identitetsverifiering för SAP HANA Cloud Platform kan tooaccess hello program.</span><span class="sxs-lookup"><span data-stu-id="2c96e-223">If Identity Federation is enabled, only hello users that are imported in SAP HANA Cloud Platform Identity Authentication are able tooaccess hello application.</span></span> 

<span data-ttu-id="2c96e-224">Mer information om hur tooenable eller inaktivera identitetsfederation med SAP HANA Cloud Platform ID-autentisering, se Aktivera identitetsfederation med identitetsverifiering för SAP HANA Cloud Platform i [konfigurera identitetsfederation med hello Store för SAP HANA Cloud Platform identitet användarautentisering. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span><span class="sxs-lookup"><span data-stu-id="2c96e-224">For more information about how tooenable or disable Identity Federation with SAP HANA Cloud Platform Identity Authentication, see Enable Identity Federation with SAP HANA Cloud Platform Identity Authentication in [Configure Identity Federation with hello User Store of SAP HANA Cloud Platform Identity Authentication.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="2c96e-225">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2c96e-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="2c96e-226">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooSAP HANA Cloud Platform identitetsverifiering.</span><span class="sxs-lookup"><span data-stu-id="2c96e-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooSAP HANA Cloud Platform Identity Authentication.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2c96e-228">**tooassign Britta Simon tooSAP HANA Cloud Platform identitetsverifiering, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2c96e-228">**tooassign Britta Simon tooSAP HANA Cloud Platform Identity Authentication, perform hello following steps:**</span></span>

1. <span data-ttu-id="2c96e-229">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-229">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2c96e-231">Välj i listan med program hello **identitetsverifiering för SAP HANA Cloud Platform**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-231">In hello applications list, select **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. <span data-ttu-id="2c96e-233">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2c96e-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2c96e-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-235">Click **Add** button.</span></span> <span data-ttu-id="2c96e-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2c96e-238">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2c96e-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2c96e-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2c96e-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="test-single-sign-on"></a><span data-ttu-id="2c96e-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2c96e-241">Test single sign-on</span></span>

<span data-ttu-id="2c96e-242">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-242">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="2c96e-243">Du bör få automatiskt inloggade tooyour identitetsverifiering för SAP HANA Cloud Platform programmet när du klickar på hello identitetsverifiering för SAP HANA Cloud Platform panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2c96e-243">When you click hello SAP HANA Cloud Platform Identity Authentication tile in hello Access Panel, you should get automatically signed-on tooyour SAP HANA Cloud Platform Identity Authentication application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="2c96e-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2c96e-244">Additional resources</span></span>

* [<span data-ttu-id="2c96e-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2c96e-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2c96e-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2c96e-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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