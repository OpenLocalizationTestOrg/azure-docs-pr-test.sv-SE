---
title: "Självstudier: Azure Active Directory-integrering med Learning på arbetet | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Learning på arbetet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d607174-bea1-4f40-8233-54cabe02c66a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: fa09d585d57932a95cadba9a66029765d7df3694
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a><span data-ttu-id="9f93a-103">Självstudier: Azure Active Directory-integrering med Learning på arbetet</span><span class="sxs-lookup"><span data-stu-id="9f93a-103">Tutorial: Azure Active Directory integration with Learning at Work</span></span>

<span data-ttu-id="9f93a-104">I kursen får du lära dig hur toointegrate Lär dig i arbetet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9f93a-104">In this tutorial, you learn how toointegrate Learning at Work with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9f93a-105">Integrera Learning på arbetet med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9f93a-105">Integrating Learning at Work with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9f93a-106">Du kan styra i Azure AD som har åtkomst till tooLearning på arbetet</span><span class="sxs-lookup"><span data-stu-id="9f93a-106">You can control in Azure AD who has access tooLearning at Work</span></span>
- <span data-ttu-id="9f93a-107">Du kan aktivera din användare tooautomatically get inloggade tooLearning på arbetet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9f93a-107">You can enable your users tooautomatically get signed-on tooLearning at Work (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9f93a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9f93a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9f93a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9f93a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f93a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9f93a-110">Prerequisites</span></span>

<span data-ttu-id="9f93a-111">tooconfigure Azure AD-integrering med Learning på arbetet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9f93a-111">tooconfigure Azure AD integration with Learning at Work, you need hello following items:</span></span>

- <span data-ttu-id="9f93a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9f93a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9f93a-113">En Learning vid arbete enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="9f93a-113">A Learning at Work single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9f93a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9f93a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9f93a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9f93a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9f93a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9f93a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9f93a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9f93a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9f93a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9f93a-118">Scenario description</span></span>
<span data-ttu-id="9f93a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9f93a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9f93a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9f93a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9f93a-121">Att lägga till Learning på arbetet från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9f93a-121">Adding Learning at Work from hello gallery</span></span>
2. <span data-ttu-id="9f93a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9f93a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-at-work-from-hello-gallery"></a><span data-ttu-id="9f93a-123">Att lägga till Learning på arbetet från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9f93a-123">Adding Learning at Work from hello gallery</span></span>
<span data-ttu-id="9f93a-124">tooconfigure hello integrering av Learning på arbetet till Azure AD, behöver du tooadd Learning på arbetet hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9f93a-124">tooconfigure hello integration of Learning at Work into Azure AD, you need tooadd Learning at Work from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9f93a-125">**tooadd Learning på arbetet från galleriet hello utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9f93a-125">**tooadd Learning at Work from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f93a-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9f93a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9f93a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9f93a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9f93a-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9f93a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9f93a-133">Skriv i sökrutan hello **Learning på arbetet**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-133">In hello search box, type **Learning at Work**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_search.png)

5. <span data-ttu-id="9f93a-135">Markera hello resultat på panelen **Learning på arbetet**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9f93a-135">In hello results panel, select **Learning at Work**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9f93a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9f93a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9f93a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Learning på arbetet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9f93a-138">In this section, you configure and test Azure AD single sign-on with Learning at Work based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9f93a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Learning på arbetet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9f93a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Learning at Work is tooa user in Azure AD.</span></span> <span data-ttu-id="9f93a-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i Learning på arbetet toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9f93a-140">In other words, a link relationship between an Azure AD user and hello related user in Learning at Work needs toobe established.</span></span>

<span data-ttu-id="9f93a-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Learning på arbetet.</span><span class="sxs-lookup"><span data-stu-id="9f93a-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Learning at Work.</span></span>

<span data-ttu-id="9f93a-142">tooconfigure och testa Azure AD enkel inloggning med Learning på arbetet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9f93a-142">tooconfigure and test Azure AD single sign-on with Learning at Work, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9f93a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9f93a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9f93a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f93a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9f93a-145">**[Skapa en Learning vid arbete testanvändare](#creating-a-learning-at-work-test-user)**  -toohave en motsvarighet för Britta Simon i Learning vid arbete som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9f93a-145">**[Creating a Learning at Work test user](#creating-a-learning-at-work-test-user)** - toohave a counterpart of Britta Simon in Learning at Work that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9f93a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9f93a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9f93a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9f93a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9f93a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9f93a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9f93a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din utbildning vid arbete program.</span><span class="sxs-lookup"><span data-stu-id="9f93a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Learning at Work application.</span></span>

<span data-ttu-id="9f93a-150">**tooconfigure Azure AD enkel inloggning med Learning på arbetet, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9f93a-150">**tooconfigure Azure AD single sign-on with Learning at Work, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f93a-151">I hello Azure-portalen på hello **Learning på arbetet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-151">In hello Azure portal, on hello **Learning at Work** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9f93a-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9f93a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_samlbase.png)

3. <span data-ttu-id="9f93a-155">På hello **Learning vid arbete domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9f93a-155">On hello **Learning at Work Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_url.png)

    <span data-ttu-id="9f93a-157">a.</span><span class="sxs-lookup"><span data-stu-id="9f93a-157">a.</span></span> <span data-ttu-id="9f93a-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span><span class="sxs-lookup"><span data-stu-id="9f93a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span></span>

    <span data-ttu-id="9f93a-159">b.</span><span class="sxs-lookup"><span data-stu-id="9f93a-159">b.</span></span> <span data-ttu-id="9f93a-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span><span class="sxs-lookup"><span data-stu-id="9f93a-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9f93a-161">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="9f93a-161">These values are not hello real.</span></span> <span data-ttu-id="9f93a-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="9f93a-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9f93a-163">Kontakta [Learning vid arbete klienten supportteamet](https://www.learninga-z.com/site/contact/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9f93a-163">Contact [Learning at Work Client support team](https://www.learninga-z.com/site/contact/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="9f93a-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="9f93a-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_certificate.png) 

5. <span data-ttu-id="9f93a-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9f93a-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9f93a-168">På hello **Learning vid arbete konfigurationen** klickar du på **konfigurera Learning på arbetet** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9f93a-168">On hello **Learning at Work Configuration** section, click **Configure Learning at Work** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9f93a-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="9f93a-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_configure.png) 

7. <span data-ttu-id="9f93a-171">tooconfigure enkel inloggning på **Learning på arbetet** sida, behöver du toosend hello hämtas **XML-Metadata för**, **SAML enhets-ID**, **SAML enkel inloggning Tjänst-URL**, och **Sign-Out URL** för[Learning på arbetet support](https://www.learninga-z.com/site/contact/support).</span><span class="sxs-lookup"><span data-stu-id="9f93a-171">tooconfigure single sign-on on **Learning at Work** side, you need toosend hello downloaded **Metadata XML**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL** too[Learning at Work support](https://www.learninga-z.com/site/contact/support).</span></span>

> [!TIP]
> <span data-ttu-id="9f93a-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9f93a-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9f93a-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9f93a-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9f93a-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9f93a-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9f93a-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f93a-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="9f93a-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9f93a-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9f93a-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9f93a-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f93a-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9f93a-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9f93a-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9f93a-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9f93a-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9f93a-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9f93a-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9f93a-187">a.</span><span class="sxs-lookup"><span data-stu-id="9f93a-187">a.</span></span> <span data-ttu-id="9f93a-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9f93a-189">b.</span><span class="sxs-lookup"><span data-stu-id="9f93a-189">b.</span></span> <span data-ttu-id="9f93a-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9f93a-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9f93a-191">c.</span><span class="sxs-lookup"><span data-stu-id="9f93a-191">c.</span></span> <span data-ttu-id="9f93a-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9f93a-193">d.</span><span class="sxs-lookup"><span data-stu-id="9f93a-193">d.</span></span> <span data-ttu-id="9f93a-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-at-work-test-user"></a><span data-ttu-id="9f93a-195">Skapa en Learning vid arbete testanvändare</span><span class="sxs-lookup"><span data-stu-id="9f93a-195">Creating a Learning at Work test user</span></span>

<span data-ttu-id="9f93a-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Learning på arbetet.</span><span class="sxs-lookup"><span data-stu-id="9f93a-196">In this section, you create a user called Britta Simon in Learning at Work.</span></span> <span data-ttu-id="9f93a-197">Arbeta med [Learning på arbetet support](https://www.learninga-z.com/site/contact/support) tooadd hello användare i hello Learning vid arbete plattform.</span><span class="sxs-lookup"><span data-stu-id="9f93a-197">Work with [Learning at Work support](https://www.learninga-z.com/site/contact/support) tooadd hello users in hello Learning at Work platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9f93a-198">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9f93a-198">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9f93a-199">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLearning på arbetet.</span><span class="sxs-lookup"><span data-stu-id="9f93a-199">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearning at Work.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9f93a-201">**tooassign Britta Simon tooLearning på arbetet, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9f93a-201">**tooassign Britta Simon tooLearning at Work, perform hello following steps:**</span></span>

1. <span data-ttu-id="9f93a-202">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-202">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9f93a-204">Välj i listan med program hello **Learning på arbetet**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-204">In hello applications list, select **Learning at Work**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_app.png) 

3. <span data-ttu-id="9f93a-206">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9f93a-206">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9f93a-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9f93a-208">Click **Add** button.</span></span> <span data-ttu-id="9f93a-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9f93a-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9f93a-211">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9f93a-211">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9f93a-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9f93a-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9f93a-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9f93a-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9f93a-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9f93a-214">Testing single sign-on</span></span>

<span data-ttu-id="9f93a-215">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9f93a-215">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9f93a-216">När du klickar på hello Learning vid arbete panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour Learning vid arbete program.</span><span class="sxs-lookup"><span data-stu-id="9f93a-216">When you click hello Learning at Work tile in hello Access Panel, you should get automatically signed-on tooyour Learning at Work application.</span></span>
<span data-ttu-id="9f93a-217">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9f93a-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f93a-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9f93a-218">Additional resources</span></span>

* [<span data-ttu-id="9f93a-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9f93a-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9f93a-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9f93a-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png

