---
title: "Självstudier: Azure Active Directory-integrering med RightAnswers | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och RightAnswers."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f09e25a-a716-41e1-8ca3-fd00e3d1b8cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 745e7ed5a13291afeed8f48a595e95b27d4b0e58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightanswers"></a><span data-ttu-id="37f05-103">Självstudier: Azure Active Directory-integrering med RightAnswers</span><span class="sxs-lookup"><span data-stu-id="37f05-103">Tutorial: Azure Active Directory integration with RightAnswers</span></span>

<span data-ttu-id="37f05-104">I kursen får du lära dig hur toointegrate RightAnswers med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="37f05-104">In this tutorial, you learn how toointegrate RightAnswers with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="37f05-105">Integrera RightAnswers med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="37f05-105">Integrating RightAnswers with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="37f05-106">Du kan styra i Azure AD som har åtkomst till tooRightAnswers</span><span class="sxs-lookup"><span data-stu-id="37f05-106">You can control in Azure AD who has access tooRightAnswers</span></span>
- <span data-ttu-id="37f05-107">Du kan aktivera din användare tooautomatically get inloggade tooRightAnswers (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="37f05-107">You can enable your users tooautomatically get signed-on tooRightAnswers (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="37f05-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="37f05-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="37f05-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="37f05-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37f05-110">Krav</span><span class="sxs-lookup"><span data-stu-id="37f05-110">Prerequisites</span></span>

<span data-ttu-id="37f05-111">tooconfigure Azure AD-integrering med RightAnswers, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="37f05-111">tooconfigure Azure AD integration with RightAnswers, you need hello following items:</span></span>

- <span data-ttu-id="37f05-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="37f05-112">An Azure AD subscription</span></span>
- <span data-ttu-id="37f05-113">En RightAnswers enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="37f05-113">A RightAnswers single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="37f05-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="37f05-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="37f05-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="37f05-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="37f05-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="37f05-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="37f05-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="37f05-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="37f05-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="37f05-118">Scenario description</span></span>
<span data-ttu-id="37f05-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="37f05-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="37f05-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="37f05-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="37f05-121">Att lägga till RightAnswers från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="37f05-121">Adding RightAnswers from hello gallery</span></span>
2. <span data-ttu-id="37f05-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37f05-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightanswers-from-hello-gallery"></a><span data-ttu-id="37f05-123">Att lägga till RightAnswers från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="37f05-123">Adding RightAnswers from hello gallery</span></span>
<span data-ttu-id="37f05-124">tooconfigure hello integrering av RightAnswers i Azure AD, behöver du tooadd RightAnswers hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="37f05-124">tooconfigure hello integration of RightAnswers into Azure AD, you need tooadd RightAnswers from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="37f05-125">**tooadd RightAnswers från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="37f05-125">**tooadd RightAnswers from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="37f05-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="37f05-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="37f05-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="37f05-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="37f05-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="37f05-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="37f05-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37f05-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="37f05-133">Skriv i sökrutan hello **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="37f05-133">In hello search box, type **RightAnswers**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_search.png)

5. <span data-ttu-id="37f05-135">Markera hello resultat på panelen **RightAnswers**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="37f05-135">In hello results panel, select **RightAnswers**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="37f05-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37f05-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="37f05-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RightAnswers baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="37f05-138">In this section, you configure and test Azure AD single sign-on with RightAnswers based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="37f05-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i RightAnswers är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="37f05-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RightAnswers is tooa user in Azure AD.</span></span> <span data-ttu-id="37f05-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i RightAnswers toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="37f05-140">In other words, a link relationship between an Azure AD user and hello related user in RightAnswers needs toobe established.</span></span>

<span data-ttu-id="37f05-141">I RightAnswers, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="37f05-141">In RightAnswers, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="37f05-142">tooconfigure och testa Azure AD enkel inloggning med RightAnswers, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="37f05-142">tooconfigure and test Azure AD single sign-on with RightAnswers, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="37f05-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="37f05-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="37f05-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37f05-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="37f05-145">**[Skapa en testanvändare RightAnswers](#creating-a-rightanswers-test-user)**  -toohave en motsvarighet för Britta Simon i RightAnswers som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="37f05-145">**[Creating a RightAnswers test user](#creating-a-rightanswers-test-user)** - toohave a counterpart of Britta Simon in RightAnswers that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="37f05-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="37f05-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="37f05-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="37f05-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="37f05-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37f05-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="37f05-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt RightAnswers program.</span><span class="sxs-lookup"><span data-stu-id="37f05-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RightAnswers application.</span></span>

<span data-ttu-id="37f05-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med RightAnswers:**</span><span class="sxs-lookup"><span data-stu-id="37f05-150">**tooconfigure Azure AD single sign-on with RightAnswers, perform hello following steps:**</span></span>

1. <span data-ttu-id="37f05-151">I hello Azure-portalen på hello **RightAnswers** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="37f05-151">In hello Azure portal, on hello **RightAnswers** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="37f05-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="37f05-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_samlbase.png)

3. <span data-ttu-id="37f05-155">På hello **RightAnswers domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="37f05-155">On hello **RightAnswers Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_url.png)

    <span data-ttu-id="37f05-157">a.</span><span class="sxs-lookup"><span data-stu-id="37f05-157">a.</span></span> <span data-ttu-id="37f05-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.rightanswers.com/portal/ss/`</span><span class="sxs-lookup"><span data-stu-id="37f05-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.rightanswers.com/portal/ss/`</span></span>

    <span data-ttu-id="37f05-159">b.</span><span class="sxs-lookup"><span data-stu-id="37f05-159">b.</span></span> <span data-ttu-id="37f05-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.rightanswers.com:<identifier>/portal`</span><span class="sxs-lookup"><span data-stu-id="37f05-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.rightanswers.com:<identifier>/portal`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="37f05-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="37f05-161">These values are not real.</span></span> <span data-ttu-id="37f05-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="37f05-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="37f05-163">Kontakta [RightAnswers klienten supportteamet](https://www.rightanswers.com/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="37f05-163">Contact [RightAnswers Client support team](https://www.rightanswers.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="37f05-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="37f05-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_certificate.png) 

5. <span data-ttu-id="37f05-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="37f05-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="37f05-168">tooconfigure enkel inloggning på **RightAnswers** sida, behöver du toosend hello hämtas **XML-Metadata för** för[RightAnswers supportteam](https://www.rightanswers.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="37f05-168">tooconfigure single sign-on on **RightAnswers** side, you need toosend hello downloaded **Metadata XML** too[RightAnswers support team](https://www.rightanswers.com/contact-us/).</span></span>

    >[!NOTE]
    ><span data-ttu-id="37f05-169">Supportteamet RightAnswers har toodo hello faktiska SSO konfiguration.</span><span class="sxs-lookup"><span data-stu-id="37f05-169">Your RightAnswers support team has toodo hello actual SSO configuration.</span></span>
    ><span data-ttu-id="37f05-170">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37f05-170">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="37f05-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="37f05-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="37f05-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="37f05-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="37f05-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="37f05-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="37f05-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="37f05-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="37f05-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37f05-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="37f05-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="37f05-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="37f05-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="37f05-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="37f05-180">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="37f05-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="37f05-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37f05-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="37f05-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="37f05-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightanswers-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="37f05-186">a.</span><span class="sxs-lookup"><span data-stu-id="37f05-186">a.</span></span> <span data-ttu-id="37f05-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="37f05-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="37f05-188">b.</span><span class="sxs-lookup"><span data-stu-id="37f05-188">b.</span></span> <span data-ttu-id="37f05-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="37f05-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="37f05-190">c.</span><span class="sxs-lookup"><span data-stu-id="37f05-190">c.</span></span> <span data-ttu-id="37f05-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="37f05-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="37f05-192">d.</span><span class="sxs-lookup"><span data-stu-id="37f05-192">d.</span></span> <span data-ttu-id="37f05-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="37f05-193">Click **Create**.</span></span>
 
### <a name="creating-a-rightanswers-test-user"></a><span data-ttu-id="37f05-194">Skapa en testanvändare RightAnswers</span><span class="sxs-lookup"><span data-stu-id="37f05-194">Creating a RightAnswers test user</span></span>

<span data-ttu-id="37f05-195">tooenable Azure AD-användare toolog i tooRightAnswers, måste de etableras i RightAnswers.</span><span class="sxs-lookup"><span data-stu-id="37f05-195">tooenable Azure AD users toolog in tooRightAnswers, they must be provisioned into RightAnswers.</span></span> <span data-ttu-id="37f05-196">När RightAnswers, etablering är en automatisk uppgift så att det finns ingen åtgärd-objekt.</span><span class="sxs-lookup"><span data-stu-id="37f05-196">When RightAnswers, provisioning is an automated task so there is no action item for you.</span></span>

<span data-ttu-id="37f05-197">Användare skapas automatiskt om det behövs under hello första enkel inloggning försöket.</span><span class="sxs-lookup"><span data-stu-id="37f05-197">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="37f05-198">Du kan använda något annat RightAnswers användarens konto skapas verktyg eller API: er som tillhandahålls av RightAnswers tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="37f05-198">You can use any other RightAnswers user account creation tools or APIs provided by RightAnswers tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="37f05-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="37f05-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="37f05-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRightAnswers.</span><span class="sxs-lookup"><span data-stu-id="37f05-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRightAnswers.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="37f05-202">**tooassign Britta Simon tooRightAnswers utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="37f05-202">**tooassign Britta Simon tooRightAnswers, perform hello following steps:**</span></span>

1. <span data-ttu-id="37f05-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="37f05-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="37f05-205">Välj i listan med program hello **RightAnswers**.</span><span class="sxs-lookup"><span data-stu-id="37f05-205">In hello applications list, select **RightAnswers**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightanswers-tutorial/tutorial_rightanswers_app.png) 

3. <span data-ttu-id="37f05-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="37f05-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="37f05-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="37f05-209">Click **Add** button.</span></span> <span data-ttu-id="37f05-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37f05-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="37f05-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="37f05-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="37f05-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37f05-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="37f05-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37f05-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="37f05-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37f05-215">Testing single sign-on</span></span>

<span data-ttu-id="37f05-216">Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="37f05-216">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="37f05-217">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="37f05-217">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>
## <a name="additional-resources"></a><span data-ttu-id="37f05-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="37f05-218">Additional resources</span></span>

* [<span data-ttu-id="37f05-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37f05-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="37f05-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="37f05-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightanswers-tutorial/tutorial_general_203.png

