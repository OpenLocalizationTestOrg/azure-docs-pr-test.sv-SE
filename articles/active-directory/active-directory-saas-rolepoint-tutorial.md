---
title: "Självstudier: Azure Active Directory-integrering med RolePoint | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och RolePoint."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68d37f40-15da-45f5-a9e1-d53f78e786d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: jeedes
ms.openlocfilehash: 8629dd87c17d44ab89251ebbd19156c6d6cbedc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rolepoint"></a><span data-ttu-id="80bd7-103">Självstudier: Azure Active Directory-integrering med RolePoint</span><span class="sxs-lookup"><span data-stu-id="80bd7-103">Tutorial: Azure Active Directory integration with RolePoint</span></span>

<span data-ttu-id="80bd7-104">I kursen får du lära dig hur toointegrate RolePoint med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="80bd7-104">In this tutorial, you learn how toointegrate RolePoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="80bd7-105">Integrera RolePoint med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="80bd7-105">Integrating RolePoint with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="80bd7-106">Du kan styra i Azure AD som har åtkomst till tooRolePoint</span><span class="sxs-lookup"><span data-stu-id="80bd7-106">You can control in Azure AD who has access tooRolePoint</span></span>
- <span data-ttu-id="80bd7-107">Du kan aktivera din användare tooautomatically get inloggade tooRolePoint (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="80bd7-107">You can enable your users tooautomatically get signed-on tooRolePoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="80bd7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="80bd7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="80bd7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="80bd7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80bd7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="80bd7-110">Prerequisites</span></span>

<span data-ttu-id="80bd7-111">tooconfigure Azure AD-integrering med RolePoint, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="80bd7-111">tooconfigure Azure AD integration with RolePoint, you need hello following items:</span></span>

- <span data-ttu-id="80bd7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="80bd7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="80bd7-113">En RolePoint enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="80bd7-113">A RolePoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="80bd7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="80bd7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="80bd7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="80bd7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="80bd7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="80bd7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="80bd7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="80bd7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="80bd7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="80bd7-118">Scenario description</span></span>
<span data-ttu-id="80bd7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="80bd7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="80bd7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="80bd7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="80bd7-121">Att lägga till RolePoint från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="80bd7-121">Adding RolePoint from hello gallery</span></span>
2. <span data-ttu-id="80bd7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80bd7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rolepoint-from-hello-gallery"></a><span data-ttu-id="80bd7-123">Att lägga till RolePoint från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="80bd7-123">Adding RolePoint from hello gallery</span></span>
<span data-ttu-id="80bd7-124">tooconfigure hello integrering av RolePoint i Azure AD, behöver du tooadd RolePoint hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="80bd7-124">tooconfigure hello integration of RolePoint into Azure AD, you need tooadd RolePoint from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="80bd7-125">**tooadd RolePoint från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="80bd7-125">**tooadd RolePoint from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="80bd7-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="80bd7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="80bd7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="80bd7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80bd7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="80bd7-133">Skriv i sökrutan hello **RolePoint**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-133">In hello search box, type **RolePoint**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_search.png)

5. <span data-ttu-id="80bd7-135">Markera hello resultat på panelen **RolePoint**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="80bd7-135">In hello results panel, select **RolePoint**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="80bd7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80bd7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="80bd7-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RolePoint baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="80bd7-138">In this section, you configure and test Azure AD single sign-on with RolePoint based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="80bd7-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i RolePoint är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="80bd7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RolePoint is tooa user in Azure AD.</span></span> <span data-ttu-id="80bd7-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i RolePoint toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="80bd7-140">In other words, a link relationship between an Azure AD user and hello related user in RolePoint needs toobe established.</span></span>

<span data-ttu-id="80bd7-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i RolePoint.</span><span class="sxs-lookup"><span data-stu-id="80bd7-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in RolePoint.</span></span>

<span data-ttu-id="80bd7-142">tooconfigure och testa Azure AD enkel inloggning med RolePoint, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="80bd7-142">tooconfigure and test Azure AD single sign-on with RolePoint, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="80bd7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="80bd7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="80bd7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="80bd7-145">**[Skapa en testanvändare RolePoint](#creating-a-rolepoint-test-user)**  -toohave en motsvarighet för Britta Simon i RolePoint som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="80bd7-145">**[Creating a RolePoint test user](#creating-a-rolepoint-test-user)** - toohave a counterpart of Britta Simon in RolePoint that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="80bd7-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="80bd7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="80bd7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="80bd7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="80bd7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80bd7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="80bd7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt RolePoint program.</span><span class="sxs-lookup"><span data-stu-id="80bd7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RolePoint application.</span></span>

<span data-ttu-id="80bd7-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med RolePoint:**</span><span class="sxs-lookup"><span data-stu-id="80bd7-150">**tooconfigure Azure AD single sign-on with RolePoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="80bd7-151">I hello Azure-portalen på hello **RolePoint** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-151">In hello Azure portal, on hello **RolePoint** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="80bd7-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="80bd7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_samlbase.png)

3. <span data-ttu-id="80bd7-155">På hello **RolePoint domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="80bd7-155">On hello **RolePoint Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_url.png)

    <span data-ttu-id="80bd7-157">a.</span><span class="sxs-lookup"><span data-stu-id="80bd7-157">a.</span></span> <span data-ttu-id="80bd7-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.rolepoint.com/login`</span><span class="sxs-lookup"><span data-stu-id="80bd7-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.rolepoint.com/login`</span></span>
    
    <span data-ttu-id="80bd7-159">b.</span><span class="sxs-lookup"><span data-stu-id="80bd7-159">b.</span></span> <span data-ttu-id="80bd7-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.rolepoint.com/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="80bd7-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://app.rolepoint.com/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="80bd7-161">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="80bd7-161">These values are not hello real.</span></span> <span data-ttu-id="80bd7-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="80bd7-162">Update these values with hello actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="80bd7-163">Här föreslår vi att du toouse hello unikt värde i strängen i hello Identifier.Contact [RolePoint supportteamet](mailto:info@rolepoint.com) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="80bd7-163">Here we suggest you toouse hello unique value of string in hello Identifier.Contact [RolePoint support team](mailto:info@rolepoint.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="80bd7-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="80bd7-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_certificate.png) 

5. <span data-ttu-id="80bd7-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="80bd7-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rolepoint-tutorial/tutorial_general_400.png)


6. <span data-ttu-id="80bd7-168">tooconfigure enkel inloggning på **RolePoint** sida, behöver du toosend hello hämtas **XML-Metadata för** för[RolePoint supportteamet](mailto:info@rolepoint.com).</span><span class="sxs-lookup"><span data-stu-id="80bd7-168">tooconfigure single sign-on on **RolePoint** side, you need toosend hello downloaded **Metadata XML** too[RolePoint support team](mailto:info@rolepoint.com).</span></span>

> [!TIP]
> <span data-ttu-id="80bd7-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="80bd7-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="80bd7-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="80bd7-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="80bd7-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="80bd7-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="80bd7-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="80bd7-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="80bd7-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="80bd7-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="80bd7-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="80bd7-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="80bd7-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="80bd7-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80bd7-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="80bd7-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="80bd7-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rolepoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="80bd7-184">a.</span><span class="sxs-lookup"><span data-stu-id="80bd7-184">a.</span></span> <span data-ttu-id="80bd7-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="80bd7-186">b.</span><span class="sxs-lookup"><span data-stu-id="80bd7-186">b.</span></span> <span data-ttu-id="80bd7-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="80bd7-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="80bd7-188">c.</span><span class="sxs-lookup"><span data-stu-id="80bd7-188">c.</span></span> <span data-ttu-id="80bd7-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="80bd7-190">d.</span><span class="sxs-lookup"><span data-stu-id="80bd7-190">d.</span></span> <span data-ttu-id="80bd7-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-191">Click **Create**.</span></span>
 
### <a name="creating-a-rolepoint-test-user"></a><span data-ttu-id="80bd7-192">Skapa en testanvändare RolePoint</span><span class="sxs-lookup"><span data-stu-id="80bd7-192">Creating a RolePoint test user</span></span>

<span data-ttu-id="80bd7-193">I det här avsnittet skapar du en användare som kallas Britta Simon i RolePoint.</span><span class="sxs-lookup"><span data-stu-id="80bd7-193">In this section, you create a user called Britta Simon in RolePoint.</span></span> <span data-ttu-id="80bd7-194">Arbeta med [RolePoint supportteamet](mailto:info@rolepoint.com) tooadd hello användare i hello RolePoint plattform.</span><span class="sxs-lookup"><span data-stu-id="80bd7-194">Work with [RolePoint support team](mailto:info@rolepoint.com) tooadd hello users in hello RolePoint platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="80bd7-195">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="80bd7-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="80bd7-196">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRolePoint.</span><span class="sxs-lookup"><span data-stu-id="80bd7-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRolePoint.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="80bd7-198">**tooassign Britta Simon tooRolePoint utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="80bd7-198">**tooassign Britta Simon tooRolePoint, perform hello following steps:**</span></span>

1. <span data-ttu-id="80bd7-199">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="80bd7-201">Välj i listan med program hello **RolePoint**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-201">In hello applications list, select **RolePoint**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rolepoint-tutorial/tutorial_rolepoint_app.png) 

3. <span data-ttu-id="80bd7-203">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="80bd7-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="80bd7-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="80bd7-205">Click **Add** button.</span></span> <span data-ttu-id="80bd7-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80bd7-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="80bd7-208">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="80bd7-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="80bd7-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80bd7-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="80bd7-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="80bd7-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="80bd7-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="80bd7-211">Testing single sign-on</span></span>

<span data-ttu-id="80bd7-212">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="80bd7-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="80bd7-213">Du bör få automatiskt inloggade tooyour RolePoint programmet när du klickar på hello RolePoint panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="80bd7-213">When you click hello RolePoint tile in hello Access Panel, you should get automatically signed-on tooyour RolePoint application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="80bd7-214">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="80bd7-214">Additional resources</span></span>

* [<span data-ttu-id="80bd7-215">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="80bd7-215">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="80bd7-216">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="80bd7-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rolepoint-tutorial/tutorial_general_203.png

