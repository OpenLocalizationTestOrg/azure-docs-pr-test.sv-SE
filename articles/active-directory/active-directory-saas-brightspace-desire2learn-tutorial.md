---
title: "Självstudier: Azure Active Directory-integrering med Brightspace av Desire2Learn | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Brightspace av Desire2Learn."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 99d03dc50defcb291a651a5415e30baab39e1e77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a><span data-ttu-id="9b78f-103">Självstudier: Azure Active Directory-integrering med Brightspace av Desire2Learn</span><span class="sxs-lookup"><span data-stu-id="9b78f-103">Tutorial: Azure Active Directory integration with Brightspace by Desire2Learn</span></span>

<span data-ttu-id="9b78f-104">I kursen får du lära dig hur toointegrate Brightspace av Desire2Learn med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9b78f-104">In this tutorial, you learn how toointegrate Brightspace by Desire2Learn with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9b78f-105">Integrera Brightspace av Desire2Learn med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9b78f-105">Integrating Brightspace by Desire2Learn with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9b78f-106">Du kan styra i Azure AD som har åtkomst till tooBrightspace av Desire2Learn</span><span class="sxs-lookup"><span data-stu-id="9b78f-106">You can control in Azure AD who has access tooBrightspace by Desire2Learn</span></span>
- <span data-ttu-id="9b78f-107">Du kan aktivera din användare tooautomatically get inloggade tooBrightspace av Desire2Learn (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9b78f-107">You can enable your users tooautomatically get signed-on tooBrightspace by Desire2Learn (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9b78f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9b78f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9b78f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9b78f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b78f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9b78f-110">Prerequisites</span></span>

<span data-ttu-id="9b78f-111">tooconfigure Azure AD-integrering med Brightspace av Desire2Learn måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9b78f-111">tooconfigure Azure AD integration with Brightspace by Desire2Learn, you need hello following items:</span></span>

- <span data-ttu-id="9b78f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9b78f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9b78f-113">En Brightspace av Desire2Learn enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="9b78f-113">A Brightspace by Desire2Learn single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9b78f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9b78f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9b78f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9b78f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9b78f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9b78f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9b78f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9b78f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9b78f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9b78f-118">Scenario description</span></span>
<span data-ttu-id="9b78f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9b78f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9b78f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9b78f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9b78f-121">Att lägga till Brightspace av Desire2Learn från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9b78f-121">Adding Brightspace by Desire2Learn from hello gallery</span></span>
2. <span data-ttu-id="9b78f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b78f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-brightspace-by-desire2learn-from-hello-gallery"></a><span data-ttu-id="9b78f-123">Att lägga till Brightspace av Desire2Learn från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9b78f-123">Adding Brightspace by Desire2Learn from hello gallery</span></span>
<span data-ttu-id="9b78f-124">tooconfigure hello integrering av Brightspace av Desire2Learn i Azure AD, behöver du tooadd Brightspace av Desire2Learn hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9b78f-124">tooconfigure hello integration of Brightspace by Desire2Learn into Azure AD, you need tooadd Brightspace by Desire2Learn from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9b78f-125">**tooadd Brightspace av Desire2Learn från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9b78f-125">**tooadd Brightspace by Desire2Learn from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9b78f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9b78f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9b78f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9b78f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9b78f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b78f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9b78f-133">Skriv i sökrutan hello **Brightspace av Desire2Learn**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-133">In hello search box, type **Brightspace by Desire2Learn**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

5. <span data-ttu-id="9b78f-135">Markera hello resultat på panelen **Brightspace av Desire2Learn**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9b78f-135">In hello results panel, select **Brightspace by Desire2Learn**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9b78f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b78f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9b78f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Brightspace av Desire2Learn baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9b78f-138">In this section, you configure and test Azure AD single sign-on with Brightspace by Desire2Learn based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9b78f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Brightspace av Desire2Learn är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9b78f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Brightspace by Desire2Learn is tooa user in Azure AD.</span></span> <span data-ttu-id="9b78f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Brightspace av Desire2Learn toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9b78f-140">In other words, a link relationship between an Azure AD user and hello related user in Brightspace by Desire2Learn needs toobe established.</span></span>

<span data-ttu-id="9b78f-141">Tilldela hello värdet för hello i Brightspace av Desire2Learn **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9b78f-141">In Brightspace by Desire2Learn, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9b78f-142">tooconfigure och testa Azure AD enkel inloggning med Brightspace av Desire2Learn, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9b78f-142">tooconfigure and test Azure AD single sign-on with Brightspace by Desire2Learn, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9b78f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9b78f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9b78f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9b78f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9b78f-145">**[Skapa en Brightspace av Desire2Learn testanvändare](#creating-a-brightspace-by-desire2learn-test-user)**  -toohave en motsvarighet för Britta Simon i Brightspace av Desire2Learn som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9b78f-145">**[Creating a Brightspace by Desire2Learn test user](#creating-a-brightspace-by-desire2learn-test-user)** - toohave a counterpart of Britta Simon in Brightspace by Desire2Learn that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9b78f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9b78f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9b78f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9b78f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9b78f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b78f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9b78f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Brightspace av Desire2Learn program.</span><span class="sxs-lookup"><span data-stu-id="9b78f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Brightspace by Desire2Learn application.</span></span>

<span data-ttu-id="9b78f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Brightspace av Desire2Learn:**</span><span class="sxs-lookup"><span data-stu-id="9b78f-150">**tooconfigure Azure AD single sign-on with Brightspace by Desire2Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="9b78f-151">I hello Azure-portalen på hello **Brightspace av Desire2Learn** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-151">In hello Azure portal, on hello **Brightspace by Desire2Learn** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9b78f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9b78f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

3. <span data-ttu-id="9b78f-155">På hello **Brightspace Desire2Learn domänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9b78f-155">On hello **Brightspace by Desire2Learn Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    <span data-ttu-id="9b78f-157">a.</span><span class="sxs-lookup"><span data-stu-id="9b78f-157">a.</span></span> <span data-ttu-id="9b78f-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="9b78f-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    <span data-ttu-id="9b78f-159">b.</span><span class="sxs-lookup"><span data-stu-id="9b78f-159">b.</span></span> <span data-ttu-id="9b78f-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span><span class="sxs-lookup"><span data-stu-id="9b78f-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9b78f-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9b78f-161">These values are not real.</span></span> <span data-ttu-id="9b78f-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="9b78f-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="9b78f-163">Kontakta [Brightspace av Desire2Learn supportteamet](https://www.d2l.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9b78f-163">Contact [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/) tooget these values.</span></span>
 


4. <span data-ttu-id="9b78f-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="9b78f-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

5. <span data-ttu-id="9b78f-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9b78f-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9b78f-168">tooconfigure enkel inloggning på **Brightspace av Desire2Learn** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Brightspace av Desire2Learn supportteamet](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="9b78f-168">tooconfigure single sign-on on **Brightspace by Desire2Learn** side, you need toosend hello downloaded **Metadata XML** too[Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="9b78f-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9b78f-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9b78f-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9b78f-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9b78f-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9b78f-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9b78f-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9b78f-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="9b78f-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9b78f-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9b78f-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9b78f-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9b78f-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9b78f-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9b78f-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9b78f-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b78f-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9b78f-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9b78f-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9b78f-184">a.</span><span class="sxs-lookup"><span data-stu-id="9b78f-184">a.</span></span> <span data-ttu-id="9b78f-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9b78f-186">b.</span><span class="sxs-lookup"><span data-stu-id="9b78f-186">b.</span></span> <span data-ttu-id="9b78f-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9b78f-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9b78f-188">c.</span><span class="sxs-lookup"><span data-stu-id="9b78f-188">c.</span></span> <span data-ttu-id="9b78f-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9b78f-190">d.</span><span class="sxs-lookup"><span data-stu-id="9b78f-190">d.</span></span> <span data-ttu-id="9b78f-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-191">Click **Create**.</span></span>
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a><span data-ttu-id="9b78f-192">Skapa en Brightspace av Desire2Learn testanvändare</span><span class="sxs-lookup"><span data-stu-id="9b78f-192">Creating a Brightspace by Desire2Learn test user</span></span>

<span data-ttu-id="9b78f-193">I ordning tooenable Azure AD-användare toolog i Brightspace av Desire2Learn, måste de etablerats i Brightspace av Desire2Learn.</span><span class="sxs-lookup"><span data-stu-id="9b78f-193">In order tooenable Azure AD users toolog into Brightspace by Desire2Learn, they must be provisioned into Brightspace by Desire2Learn.</span></span>  

<span data-ttu-id="9b78f-194">Hello gäller Brightspace av Desire2Learn, hello användarkonton måste toobe som skapats av din [Brightspace av Desire2Learn supportteamet](https://www.d2l.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="9b78f-194">In hello case of Brightspace by Desire2Learn, hello user accounts need toobe created by your [Brightspace by Desire2Learn support team](https://www.d2l.com/contact/).</span></span>

>[!NOTE]
><span data-ttu-id="9b78f-195">Du kan använda andra Brightspace genom Desire2Learn användare skapa verktyg eller API: er som tillhandahålls av Brightspace genom Desire2Learn tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="9b78f-195">You can use any other Brightspace by Desire2Learn user account creation tools or APIs provided by Brightspace by Desire2Learn tooprovision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9b78f-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9b78f-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9b78f-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBrightspace av Desire2Learn.</span><span class="sxs-lookup"><span data-stu-id="9b78f-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBrightspace by Desire2Learn.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9b78f-199">**tooassign Britta Simon tooBrightspace av Desire2Learn, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="9b78f-199">**tooassign Britta Simon tooBrightspace by Desire2Learn, perform hello following steps:**</span></span>

1. <span data-ttu-id="9b78f-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9b78f-202">Välj i listan med program hello **Brightspace av Desire2Learn**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-202">In hello applications list, select **Brightspace by Desire2Learn**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

3. <span data-ttu-id="9b78f-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9b78f-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9b78f-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9b78f-206">Click **Add** button.</span></span> <span data-ttu-id="9b78f-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b78f-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9b78f-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9b78f-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9b78f-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b78f-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9b78f-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b78f-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9b78f-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b78f-212">Testing single sign-on</span></span>

<span data-ttu-id="9b78f-213">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9b78f-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9b78f-214">När du klickar på hello Brightspace av Desire2Learn panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Brightspace av Desire2Learn program.</span><span class="sxs-lookup"><span data-stu-id="9b78f-214">When you click hello Brightspace by Desire2Learn tile in hello Access Panel, you should get automatically signed-on tooyour Brightspace by Desire2Learn application.</span></span>
<span data-ttu-id="9b78f-215">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9b78f-215">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b78f-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9b78f-216">Additional resources</span></span>

* [<span data-ttu-id="9b78f-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9b78f-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9b78f-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9b78f-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-brightspace-desire2learn-tutorial/tutorial_general_203.png

