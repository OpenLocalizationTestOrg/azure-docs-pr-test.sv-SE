---
title: "Självstudier: Azure Active Directory-integrering med Predictix olika planering | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Predictix olika planering."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 1294b712caf12fdafaf65d70a02ee9fbdc3e84a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a><span data-ttu-id="ca010-103">Självstudier: Azure Active Directory-integrering med Predictix olika planering</span><span class="sxs-lookup"><span data-stu-id="ca010-103">Tutorial: Azure Active Directory integration with Predictix Assortment Planning</span></span>

<span data-ttu-id="ca010-104">I kursen får du lära dig hur toointegrate Predictix olika planering med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ca010-104">In this tutorial, you learn how toointegrate Predictix Assortment Planning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ca010-105">Integrera Predictix olika planering med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ca010-105">Integrating Predictix Assortment Planning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ca010-106">Du kan styra i Azure AD som har åtkomst till tooPredictix olika planering.</span><span class="sxs-lookup"><span data-stu-id="ca010-106">You can control in Azure AD who has access tooPredictix Assortment Planning.</span></span>
- <span data-ttu-id="ca010-107">Du kan låta dina användare tooautomatically get inloggade tooPredictix olika planering (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="ca010-107">You can enable your users tooautomatically get signed-on tooPredictix Assortment Planning (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="ca010-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ca010-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="ca010-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ca010-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca010-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ca010-110">Prerequisites</span></span>

<span data-ttu-id="ca010-111">tooconfigure Azure AD-integrering med Predictix olika planering måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ca010-111">tooconfigure Azure AD integration with Predictix Assortment Planning, you need hello following items:</span></span>

- <span data-ttu-id="ca010-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ca010-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ca010-113">En Predictix olika planering enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ca010-113">A Predictix Assortment Planning single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ca010-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ca010-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ca010-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ca010-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ca010-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ca010-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ca010-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca010-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ca010-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ca010-118">Scenario description</span></span>
<span data-ttu-id="ca010-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ca010-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ca010-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ca010-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ca010-121">Att lägga till Predictix olika planering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ca010-121">Adding Predictix Assortment Planning from hello gallery</span></span>
2. <span data-ttu-id="ca010-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca010-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-assortment-planning-from-hello-gallery"></a><span data-ttu-id="ca010-123">Att lägga till Predictix olika planering från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ca010-123">Adding Predictix Assortment Planning from hello gallery</span></span>
<span data-ttu-id="ca010-124">tooconfigure hello integrering av Predictix olika planering i Azure AD, behöver du tooadd Predictix olika planering hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ca010-124">tooconfigure hello integration of Predictix Assortment Planning into Azure AD, you need tooadd Predictix Assortment Planning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ca010-125">**tooadd Predictix olika planering från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ca010-125">**tooadd Predictix Assortment Planning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca010-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ca010-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="ca010-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ca010-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ca010-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ca010-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="ca010-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca010-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="ca010-133">Skriv i sökrutan hello **Predictix olika planering**väljer **Predictix olika planering** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ca010-133">In hello search box, type **Predictix Assortment Planning**, select **Predictix Assortment Planning** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Predictix olika planering i hello resultatlistan](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ca010-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca010-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ca010-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Predictix olika planering baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ca010-136">In this section, you configure and test Azure AD single sign-on with Predictix Assortment Planning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ca010-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Predictix olika planering är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ca010-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Predictix Assortment Planning is tooa user in Azure AD.</span></span> <span data-ttu-id="ca010-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Predictix olika planering toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="ca010-138">In other words, a link relationship between an Azure AD user and hello related user in Predictix Assortment Planning needs toobe established.</span></span>

<span data-ttu-id="ca010-139">I Predictix olika planering, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ca010-139">In Predictix Assortment Planning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ca010-140">tooconfigure och testa Azure AD enkel inloggning med Predictix olika planering måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ca010-140">tooconfigure and test Azure AD single sign-on with Predictix Assortment Planning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ca010-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ca010-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ca010-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca010-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ca010-143">**[Skapa en testanvändare Predictix olika planering](#create-a-predictix-assortment-planning-test-user)**  -toohave en motsvarighet för Britta Simon Predictix olika planera som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ca010-143">**[Create a Predictix Assortment Planning test user](#create-a-predictix-assortment-planning-test-user)** - toohave a counterpart of Britta Simon in Predictix Assortment Planning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ca010-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca010-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ca010-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ca010-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ca010-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca010-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ca010-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Predictix olika planering.</span><span class="sxs-lookup"><span data-stu-id="ca010-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Predictix Assortment Planning application.</span></span>

<span data-ttu-id="ca010-148">**tooconfigure Azure AD enkel inloggning med Predictix olika planering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="ca010-148">**tooconfigure Azure AD single sign-on with Predictix Assortment Planning, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca010-149">I hello Azure-portalen på hello **Predictix olika planering** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ca010-149">In hello Azure portal, on hello **Predictix Assortment Planning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="ca010-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca010-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_samlbase.png)

3. <span data-ttu-id="ca010-153">På hello **Predictix olika Planera domän- och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca010-153">On hello **Predictix Assortment Planning Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Predictix olika Planera domän med enkel inloggning information](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_url.png)

    <span data-ttu-id="ca010-155">a.</span><span class="sxs-lookup"><span data-stu-id="ca010-155">a.</span></span> <span data-ttu-id="ca010-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="ca010-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com/sso/request`|
    | `https://<sub-domain>.dev.ap.predictix.com/`|

    <span data-ttu-id="ca010-157">b.</span><span class="sxs-lookup"><span data-stu-id="ca010-157">b.</span></span> <span data-ttu-id="ca010-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="ca010-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com`|
    | `https://<sub-domain>.dev.ap.predictix.com`|
    
    > [!NOTE] 
    > <span data-ttu-id="ca010-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ca010-159">These values are not real.</span></span> <span data-ttu-id="ca010-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ca010-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ca010-161">Kontakta [Predictix olika planering klienten supportteamet](http://www.infor.com/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ca010-161">Contact [Predictix Assortment Planning Client support team](http://www.infor.com/support) tooget these values.</span></span> 
 


4. <span data-ttu-id="ca010-162">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ca010-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_certificate.png) 

5. <span data-ttu-id="ca010-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca010-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ca010-166">På hello **Predictix olika planera Configuration** klickar du på **konfigurera Predictix olika planering** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ca010-166">On hello **Predictix Assortment Planning Configuration** section, click **Configure Predictix Assortment Planning** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ca010-167">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ca010-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Planera konfiguration för Predictix blandning](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_configure.png) 

7. <span data-ttu-id="ca010-169">tooconfigure enkel inloggning på **Predictix olika planering** sida, behöver du toosend hello hämtas **Certificate(Base64)**, **SAML enhets-ID**,  **SAML enkel inloggning Tjänstwebbadress**, och **Sign-Out URL** för[Predictix olika planering supportteamet](http://www.infor.com/support).</span><span class="sxs-lookup"><span data-stu-id="ca010-169">tooconfigure single sign-on on **Predictix Assortment Planning** side, you need toosend hello downloaded **Certificate(Base64)**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL**  too[Predictix Assortment Planning support team](http://www.infor.com/support).</span></span> <span data-ttu-id="ca010-170">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="ca010-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="ca010-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ca010-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ca010-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ca010-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ca010-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ca010-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ca010-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca010-174">Create an Azure AD test user</span></span>

<span data-ttu-id="ca010-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca010-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="ca010-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ca010-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca010-178">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca010-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="ca010-180">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ca010-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="ca010-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca010-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="ca010-184">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca010-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_04.png)

    <span data-ttu-id="ca010-186">a.</span><span class="sxs-lookup"><span data-stu-id="ca010-186">a.</span></span> <span data-ttu-id="ca010-187">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ca010-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ca010-188">b.</span><span class="sxs-lookup"><span data-stu-id="ca010-188">b.</span></span> <span data-ttu-id="ca010-189">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca010-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="ca010-190">c.</span><span class="sxs-lookup"><span data-stu-id="ca010-190">c.</span></span> <span data-ttu-id="ca010-191">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="ca010-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="ca010-192">d.</span><span class="sxs-lookup"><span data-stu-id="ca010-192">d.</span></span> <span data-ttu-id="ca010-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ca010-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-assortment-planning-test-user"></a><span data-ttu-id="ca010-194">Skapa en testanvändare Predictix olika planering</span><span class="sxs-lookup"><span data-stu-id="ca010-194">Create a Predictix Assortment Planning test user</span></span>

<span data-ttu-id="ca010-195">I det här avsnittet kan du skapa en användare som kallas Britta Simon Predictix olika planera.</span><span class="sxs-lookup"><span data-stu-id="ca010-195">In this section, you create a user called Britta Simon in Predictix Assortment Planning.</span></span> <span data-ttu-id="ca010-196">Se tillsammans med [Predictix olika planering supportteamet](http://www.infor.com/contact/) tooadd hello användare i hello Predictix olika planering plattform.</span><span class="sxs-lookup"><span data-stu-id="ca010-196">Please work with [Predictix Assortment Planning support team](http://www.infor.com/contact/) tooadd hello users in hello Predictix Assortment Planning platform.</span></span>
 > [!NOTE]
 > <span data-ttu-id="ca010-197">hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="ca010-197">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="ca010-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca010-198">Assign hello Azure AD test user</span></span>

<span data-ttu-id="ca010-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPredictix olika planering.</span><span class="sxs-lookup"><span data-stu-id="ca010-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPredictix Assortment Planning.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="ca010-201">**tooassign Britta Simon tooPredictix olika planering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="ca010-201">**tooassign Britta Simon tooPredictix Assortment Planning, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca010-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ca010-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ca010-204">Välj i listan med program hello **Predictix olika planering**.</span><span class="sxs-lookup"><span data-stu-id="ca010-204">In hello applications list, select **Predictix Assortment Planning**.</span></span>

    ![Hej Predictix olika planering länken i listan med program hello](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_app.png)  

3. <span data-ttu-id="ca010-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ca010-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="ca010-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca010-208">Click **Add** button.</span></span> <span data-ttu-id="ca010-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca010-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="ca010-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ca010-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ca010-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca010-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ca010-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca010-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ca010-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca010-214">Test single sign-on</span></span>

<span data-ttu-id="ca010-215">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ca010-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ca010-216">Du bör få automatiskt inloggade tooyour Predictix olika planering programmet när du klickar på hello Predictix olika planering panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ca010-216">When you click hello Predictix Assortment Planning tile in hello Access Panel, you should get automatically signed-on tooyour Predictix Assortment Planning application.</span></span>
<span data-ttu-id="ca010-217">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ca010-217">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ca010-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ca010-218">Additional resources</span></span>

* [<span data-ttu-id="ca010-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ca010-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ca010-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ca010-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_203.png

