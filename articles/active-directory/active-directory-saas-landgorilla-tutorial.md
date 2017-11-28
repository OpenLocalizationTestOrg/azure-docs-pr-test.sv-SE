---
title: "Självstudier: Azure Active Directory-integrering med mark Gorilla klienten | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och mark Gorilla."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: jeedes
ms.openlocfilehash: e95a30551e636108fe22a7ab6d1827bc12d7f9a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-land-gorilla-client"></a><span data-ttu-id="75b11-103">Självstudier: Azure Active Directory-integrering med mark Gorilla klienten</span><span class="sxs-lookup"><span data-stu-id="75b11-103">Tutorial: Azure Active Directory integration with Land Gorilla Client</span></span>

<span data-ttu-id="75b11-104">I kursen får du lära dig hur toointegrate mark Gorilla klienten med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="75b11-104">In this tutorial, you learn how toointegrate Land Gorilla Client with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75b11-105">Integrera mark Gorilla klienten med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="75b11-105">Integrating Land Gorilla Client with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="75b11-106">Du kan styra i Azure AD som har åtkomst tooLand Gorilla klienten</span><span class="sxs-lookup"><span data-stu-id="75b11-106">You can control in Azure AD who has access tooLand Gorilla Client</span></span>
- <span data-ttu-id="75b11-107">Du kan aktivera din användare tooautomatically get inloggade tooLand Gorilla klienten (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="75b11-107">You can enable your users tooautomatically get signed-on tooLand Gorilla Client (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="75b11-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="75b11-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="75b11-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75b11-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="75b11-110">Krav</span><span class="sxs-lookup"><span data-stu-id="75b11-110">Prerequisites</span></span>

<span data-ttu-id="75b11-111">tooconfigure Azure AD-integrering med mark Gorilla klienten behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="75b11-111">tooconfigure Azure AD integration with Land Gorilla Client, you need hello following items:</span></span>

- <span data-ttu-id="75b11-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="75b11-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75b11-113">En mark Gorilla klienten enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="75b11-113">A Land Gorilla Client single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="75b11-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="75b11-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="75b11-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="75b11-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75b11-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="75b11-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="75b11-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75b11-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="75b11-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="75b11-118">Scenario description</span></span>
<span data-ttu-id="75b11-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="75b11-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75b11-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="75b11-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75b11-121">Lägger till mark Gorilla klient från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="75b11-121">Adding Land Gorilla Client from hello gallery</span></span>
2. <span data-ttu-id="75b11-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75b11-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-land-gorilla-client-from-hello-gallery"></a><span data-ttu-id="75b11-123">Lägger till mark Gorilla klient från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="75b11-123">Adding Land Gorilla Client from hello gallery</span></span>
<span data-ttu-id="75b11-124">tooconfigure hello integrering av mark Gorilla klienten i Azure AD, behöver du tooadd mark Gorilla klientens hello-galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="75b11-124">tooconfigure hello integration of Land Gorilla Client into Azure AD, you need tooadd Land Gorilla Client from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="75b11-125">**tooadd mark Gorilla klienten från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75b11-125">**tooadd Land Gorilla Client from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="75b11-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75b11-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="75b11-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="75b11-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="75b11-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="75b11-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="75b11-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75b11-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="75b11-133">Skriv i sökrutan hello **mark Gorilla klienten**.</span><span class="sxs-lookup"><span data-stu-id="75b11-133">In hello search box, type **Land Gorilla Client**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_search.png)

5. <span data-ttu-id="75b11-135">Markera hello resultat på panelen **mark Gorilla klienten**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="75b11-135">In hello results panel, select **Land Gorilla Client**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="75b11-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75b11-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="75b11-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med mark Gorilla klient med en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="75b11-138">In this section, you configure and test Azure AD single sign-on with Land Gorilla Client based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="75b11-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i mark Gorilla klienten är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75b11-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Land Gorilla Client is tooa user in Azure AD.</span></span> <span data-ttu-id="75b11-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i mark Gorilla klienten toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="75b11-140">In other words, a link relationship between an Azure AD user and hello related user in Land Gorilla Client needs toobe established.</span></span>

<span data-ttu-id="75b11-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** mark Gorilla-klienten.</span><span class="sxs-lookup"><span data-stu-id="75b11-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Land Gorilla Client.</span></span>

<span data-ttu-id="75b11-142">tooconfigure och testa Azure AD enkel inloggning med mark Gorilla klienten behöver toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="75b11-142">tooconfigure and test Azure AD single sign-on with Land Gorilla Client, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="75b11-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="75b11-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="75b11-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med en begränsad grupp.</span><span class="sxs-lookup"><span data-stu-id="75b11-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with limited group.</span></span>
3. <span data-ttu-id="75b11-145">**[Skapa en testanvändare mark Gorilla](#creating-a-land-gorilla-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75b11-145">**[Creating a Land Gorilla test user](#creating-a-land-gorilla-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="75b11-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75b11-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75b11-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="75b11-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="75b11-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75b11-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="75b11-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Land Gorilla klientprogram.</span><span class="sxs-lookup"><span data-stu-id="75b11-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Land Gorilla Client application.</span></span>

<span data-ttu-id="75b11-150">**tooconfigure Azure AD enkel inloggning med mark Gorilla klienten utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75b11-150">**tooconfigure Azure AD single sign-on with Land Gorilla Client, perform hello following steps:**</span></span>

1. <span data-ttu-id="75b11-151">I hello Azure Management portal på hello **mark Gorilla klienten** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="75b11-151">In hello Azure Management portal, on hello **Land Gorilla Client** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="75b11-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75b11-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_samlbase.png)

3. <span data-ttu-id="75b11-155">På hello **mark Gorilla klienten domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75b11-155">On hello **Land Gorilla Client Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_url_02.png)

    <span data-ttu-id="75b11-157">a.</span><span class="sxs-lookup"><span data-stu-id="75b11-157">a.</span></span> <span data-ttu-id="75b11-158">I hello **identifierare** textruta hello TYPVÄRDE med någon av följande mönster hello:</span><span class="sxs-lookup"><span data-stu-id="75b11-158">In hello **Identifier** textbox, type hello value using one of hello following pattern:</span></span> 
    
    `https://<customer domain>.landgorilla.com/` 
    
    `https://www.<customer domain>.landgorilla.com`

    <span data-ttu-id="75b11-159">b.</span><span class="sxs-lookup"><span data-stu-id="75b11-159">b.</span></span> <span data-ttu-id="75b11-160">I hello **Reply URL** textruta, ange ett URL-Adressen med något av följande mönster hello:</span><span class="sxs-lookup"><span data-stu-id="75b11-160">In hello **Reply URL** textbox, type a URL using one of hello following pattern:</span></span>

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`
    
    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`

    > [!NOTE] 
    > <span data-ttu-id="75b11-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="75b11-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="75b11-162">Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="75b11-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="75b11-163">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="75b11-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="75b11-164">Kontakta [mark Gorilla klienten team](https://www.landgorilla.com/support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="75b11-164">Contact [Land Gorilla Client team](https://www.landgorilla.com/support/) tooget these values.</span></span> 

4. <span data-ttu-id="75b11-165">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="75b11-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_certificate.png) 

5. <span data-ttu-id="75b11-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="75b11-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="75b11-169">tooget SSO konfigurationen har slutförts för programmet i slutet av mark Gorilla Kontakta [mark Gorilla klienten supportteamet](https://www.landgorilla.com/support/) och ge dem hello hämtas **”XML-Metadata för** fil.</span><span class="sxs-lookup"><span data-stu-id="75b11-169">tooget SSO configuration complete for your application at Land Gorilla end, Contact [Land Gorilla Client support team](https://www.landgorilla.com/support/) and provide them with hello downloaded **“Metadata XML** file.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="75b11-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="75b11-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="75b11-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75b11-171">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="75b11-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75b11-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="75b11-174">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75b11-174">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="75b11-176">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="75b11-176">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="75b11-178">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75b11-178">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="75b11-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="75b11-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="75b11-182">a.</span><span class="sxs-lookup"><span data-stu-id="75b11-182">a.</span></span> <span data-ttu-id="75b11-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75b11-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75b11-184">b.</span><span class="sxs-lookup"><span data-stu-id="75b11-184">b.</span></span> <span data-ttu-id="75b11-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="75b11-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="75b11-186">c.</span><span class="sxs-lookup"><span data-stu-id="75b11-186">c.</span></span> <span data-ttu-id="75b11-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="75b11-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="75b11-188">d.</span><span class="sxs-lookup"><span data-stu-id="75b11-188">d.</span></span> <span data-ttu-id="75b11-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="75b11-189">Click **Create**.</span></span> 

### <a name="creating-a-land-gorilla-test-user"></a><span data-ttu-id="75b11-190">Skapa en testanvändare mark Gorilla</span><span class="sxs-lookup"><span data-stu-id="75b11-190">Creating a Land Gorilla test user</span></span>

<span data-ttu-id="75b11-191">Se tillsammans med [mark Gorilla supportteamet](https://www.landgorilla.com/support/) tooadd hello användare i hello mark Gorilla plattform.</span><span class="sxs-lookup"><span data-stu-id="75b11-191">Please work with [Land Gorilla support team](https://www.landgorilla.com/support/) tooadd hello users in hello Land Gorilla platform.</span></span>
    
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="75b11-192">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="75b11-192">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="75b11-193">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access tooLand Gorilla klienten.</span><span class="sxs-lookup"><span data-stu-id="75b11-193">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooLand Gorilla Client.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="75b11-195">**tooassign Britta Simon tooLand Gorilla klienten utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75b11-195">**tooassign Britta Simon tooLand Gorilla Client, perform hello following steps:**</span></span>

1. <span data-ttu-id="75b11-196">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="75b11-196">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="75b11-198">Välj i listan med program hello **mark Gorilla klienten**.</span><span class="sxs-lookup"><span data-stu-id="75b11-198">In hello applications list, select **Land Gorilla Client**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_app.png) 

3. <span data-ttu-id="75b11-200">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="75b11-200">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="75b11-202">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="75b11-202">Click **Add** button.</span></span> <span data-ttu-id="75b11-203">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75b11-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="75b11-205">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="75b11-205">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="75b11-206">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75b11-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75b11-207">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75b11-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="75b11-208">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75b11-208">Testing single sign-on</span></span>

<span data-ttu-id="75b11-209">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="75b11-209">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="75b11-210">När du klickar på hello mark Gorilla klienten panelen i hello åtkomstpanelen får automatiskt inloggade tooyour mark Gorilla klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="75b11-210">When you click hello Land Gorilla Client tile in hello Access Panel, you should get automatically signed-on tooyour Land Gorilla Client application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="75b11-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="75b11-211">Additional resources</span></span>

* [<span data-ttu-id="75b11-212">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75b11-212">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75b11-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="75b11-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_203.png
