---
title: "Självstudier: Azure Active Directory-integrering med CS stjärnor | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och CS stjärnor."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5704d151-afb8-40a4-b286-8bacd4f279ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: d84533e8a7e9bca2f7bdf4be7f3050bca2f18496
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cs-stars"></a><span data-ttu-id="ac209-103">Självstudier: Azure Active Directory-integrering med CS stjärnor</span><span class="sxs-lookup"><span data-stu-id="ac209-103">Tutorial: Azure Active Directory integration with CS Stars</span></span>

<span data-ttu-id="ac209-104">I kursen får du lära dig hur toointegrate CS stjärnor med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ac209-104">In this tutorial, you learn how toointegrate CS Stars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ac209-105">Integrera CS stjärnor med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ac209-105">Integrating CS Stars with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ac209-106">Du kan styra i Azure AD som har åtkomst tooCS stjärnor</span><span class="sxs-lookup"><span data-stu-id="ac209-106">You can control in Azure AD who has access tooCS Stars</span></span>
- <span data-ttu-id="ac209-107">Du kan aktivera din användare tooautomatically hämta inloggade tooCS stjärnor (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ac209-107">You can enable your users tooautomatically get signed-on tooCS Stars (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ac209-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ac209-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ac209-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ac209-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac209-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ac209-110">Prerequisites</span></span>

<span data-ttu-id="ac209-111">tooconfigure Azure AD-integrering med CS stjärnor, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ac209-111">tooconfigure Azure AD integration with CS Stars, you need hello following items:</span></span>

- <span data-ttu-id="ac209-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ac209-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ac209-113">En CS stjärnor enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ac209-113">A CS Stars single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ac209-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ac209-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ac209-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ac209-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ac209-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ac209-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ac209-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ac209-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ac209-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ac209-118">Scenario description</span></span>
<span data-ttu-id="ac209-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ac209-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ac209-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ac209-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ac209-121">Att lägga till CS stjärnor från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ac209-121">Adding CS Stars from hello gallery</span></span>
2. <span data-ttu-id="ac209-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ac209-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cs-stars-from-hello-gallery"></a><span data-ttu-id="ac209-123">Att lägga till CS stjärnor från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ac209-123">Adding CS Stars from hello gallery</span></span>
<span data-ttu-id="ac209-124">tooconfigure hello integrering av CS stjärnor i Azure AD, behöver du tooadd CS stjärnor hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ac209-124">tooconfigure hello integration of CS Stars into Azure AD, you need tooadd CS Stars from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ac209-125">**tooadd CS stjärnor från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ac209-125">**tooadd CS Stars from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ac209-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ac209-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ac209-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ac209-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ac209-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ac209-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ac209-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ac209-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ac209-133">Skriv i sökrutan hello **CS stjärnor**.</span><span class="sxs-lookup"><span data-stu-id="ac209-133">In hello search box, type **CS Stars**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_search.png)

5. <span data-ttu-id="ac209-135">Markera hello resultat på panelen **CS stjärnor**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ac209-135">In hello results panel, select **CS Stars**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ac209-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ac209-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ac209-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CS stjärnor utifrån en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ac209-138">In this section, you configure and test Azure AD single sign-on with CS Stars based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ac209-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i CS stjärnor är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ac209-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CS Stars is tooa user in Azure AD.</span></span> <span data-ttu-id="ac209-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i CS stjärnor toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ac209-140">In other words, a link relationship between an Azure AD user and hello related user in CS Stars needs toobe established.</span></span>

<span data-ttu-id="ac209-141">Tilldela hello värdet för hello i CS stjärnor, **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ac209-141">In CS Stars, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ac209-142">tooconfigure och testa Azure AD enkel inloggning med CS stjärnor, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ac209-142">tooconfigure and test Azure AD single sign-on with CS Stars, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ac209-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ac209-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ac209-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ac209-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ac209-145">**[Skapa en testanvändare CS stjärnor](#creating-a-cs-stars-test-user)**  -toohave en motsvarighet för Britta Simon i CS stjärnor som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ac209-145">**[Creating a CS Stars test user](#creating-a-cs-stars-test-user)** - toohave a counterpart of Britta Simon in CS Stars that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ac209-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ac209-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ac209-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ac209-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ac209-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ac209-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ac209-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet CS stjärnor.</span><span class="sxs-lookup"><span data-stu-id="ac209-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CS Stars application.</span></span>

<span data-ttu-id="ac209-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med CS stjärnor:**</span><span class="sxs-lookup"><span data-stu-id="ac209-150">**tooconfigure Azure AD single sign-on with CS Stars, perform hello following steps:**</span></span>

1. <span data-ttu-id="ac209-151">I hello Azure-portalen på hello **CS stjärnor** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ac209-151">In hello Azure portal, on hello **CS Stars** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ac209-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ac209-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_samlbase.png)

3. <span data-ttu-id="ac209-155">På hello **CS stjärnor domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ac209-155">On hello **CS Stars Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_url.png)

    <span data-ttu-id="ac209-157">a.</span><span class="sxs-lookup"><span data-stu-id="ac209-157">a.</span></span> <span data-ttu-id="ac209-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="ac209-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.csstars.com/enterprise/default.cmdx?ssoclient=<uniqueid>`</span></span>

    <span data-ttu-id="ac209-159">b.</span><span class="sxs-lookup"><span data-stu-id="ac209-159">b.</span></span> <span data-ttu-id="ac209-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.csstars.com/enterprise/`</span><span class="sxs-lookup"><span data-stu-id="ac209-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.csstars.com/enterprise/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ac209-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ac209-161">These values are not real.</span></span> <span data-ttu-id="ac209-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ac209-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ac209-163">Kontakta [CS stjärnor klienten supportteamet](http://www.marshclearsight.com/support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ac209-163">Contact [CS Stars Client support team](http://www.marshclearsight.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="ac209-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="ac209-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_certificate.png) 

5. <span data-ttu-id="ac209-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ac209-166">Click **Save** button.</span></span>

    <span data-ttu-id="ac209-167">![Konfigurera enkel inloggning](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="ac209-167">![Configure Single Sign-On](./media/active-directory-saas-cs-stars-tutorial/tutorial_general_400.png) 
<CS></span></span>
6. <span data-ttu-id="ac209-168">tooconfigure enkel inloggning på **CS stjärnor** sida, behöver du toosend hello hämtas **XML-Metadata för** för[CS stjärnor supportteam](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="ac209-168">tooconfigure single sign-on on **CS Stars** side, you need toosend hello downloaded **Metadata XML** too[CS Stars support team](http://www.marshclearsight.com/support/).</span></span> 
<CE>

> [!TIP]
> <span data-ttu-id="ac209-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ac209-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ac209-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ac209-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ac209-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ac209-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ac209-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac209-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="ac209-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ac209-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ac209-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ac209-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ac209-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ac209-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ac209-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ac209-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ac209-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ac209-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ac209-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ac209-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cs-stars-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ac209-184">a.</span><span class="sxs-lookup"><span data-stu-id="ac209-184">a.</span></span> <span data-ttu-id="ac209-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ac209-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ac209-186">b.</span><span class="sxs-lookup"><span data-stu-id="ac209-186">b.</span></span> <span data-ttu-id="ac209-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ac209-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ac209-188">c.</span><span class="sxs-lookup"><span data-stu-id="ac209-188">c.</span></span> <span data-ttu-id="ac209-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ac209-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ac209-190">d.</span><span class="sxs-lookup"><span data-stu-id="ac209-190">d.</span></span> <span data-ttu-id="ac209-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ac209-191">Click **Create**.</span></span>
 
### <a name="creating-a-cs-stars-test-user"></a><span data-ttu-id="ac209-192">Skapa en testanvändare CS stjärnor</span><span class="sxs-lookup"><span data-stu-id="ac209-192">Creating a CS Stars test user</span></span>

<span data-ttu-id="ac209-193">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i CS stjärnor.</span><span class="sxs-lookup"><span data-stu-id="ac209-193">hello objective of this section is toocreate a user called Britta Simon in CS Stars.</span></span>

<span data-ttu-id="ac209-194">tooget en användare som skapats i CS stjärnor, behöver du toocontact din [CS stjärnor supportteam](http://www.marshclearsight.com/support/).</span><span class="sxs-lookup"><span data-stu-id="ac209-194">tooget a user created in CS Stars, you need toocontact your [CS Stars support team](http://www.marshclearsight.com/support/).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ac209-195">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ac209-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ac209-196">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCS stjärnor.</span><span class="sxs-lookup"><span data-stu-id="ac209-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCS Stars.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ac209-198">**tooassign Britta Simon tooCS stjärnor, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ac209-198">**tooassign Britta Simon tooCS Stars, perform hello following steps:**</span></span>

1. <span data-ttu-id="ac209-199">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ac209-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ac209-201">Välj i listan med program hello **CS stjärnor**.</span><span class="sxs-lookup"><span data-stu-id="ac209-201">In hello applications list, select **CS Stars**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cs-stars-tutorial/tutorial_csstars_app.png) 

3. <span data-ttu-id="ac209-203">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ac209-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ac209-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ac209-205">Click **Add** button.</span></span> <span data-ttu-id="ac209-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ac209-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ac209-208">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ac209-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ac209-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ac209-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ac209-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ac209-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ac209-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ac209-211">Testing single sign-on</span></span>

<span data-ttu-id="ac209-212">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ac209-212">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="ac209-213">Du bör få automatiskt inloggade tooyour CS stjärnor programmet när du klickar på hello CS stjärnor panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ac209-213">When you click hello CS Stars tile in hello Access Panel, you should get automatically signed-on tooyour CS Stars application.</span></span>
 

## <a name="additional-resources"></a><span data-ttu-id="ac209-214">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ac209-214">Additional resources</span></span>

* [<span data-ttu-id="ac209-215">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ac209-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ac209-216">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ac209-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_203.png

