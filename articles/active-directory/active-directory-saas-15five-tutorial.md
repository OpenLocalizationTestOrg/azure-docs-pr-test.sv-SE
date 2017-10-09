---
title: "Självstudier: Azure Active Directory-integrering med 15Five | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och 15Five."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 9e531615c16331ce000e285d13d9adce13735a04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a><span data-ttu-id="cebc9-103">Självstudier: Azure Active Directory-integrering med 15Five</span><span class="sxs-lookup"><span data-stu-id="cebc9-103">Tutorial: Azure Active Directory integration with 15Five</span></span>

<span data-ttu-id="cebc9-104">I kursen får du lära dig hur toointegrate 15Five med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cebc9-104">In this tutorial, you learn how toointegrate 15Five with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cebc9-105">Integrera 15Five med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cebc9-105">Integrating 15Five with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cebc9-106">Du kan styra i Azure AD som har åtkomst till too15Five</span><span class="sxs-lookup"><span data-stu-id="cebc9-106">You can control in Azure AD who has access too15Five</span></span>
- <span data-ttu-id="cebc9-107">Du kan aktivera din användare tooautomatically get inloggade too15Five (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cebc9-107">You can enable your users tooautomatically get signed-on too15Five (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cebc9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cebc9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="cebc9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cebc9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cebc9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cebc9-110">Prerequisites</span></span>

<span data-ttu-id="cebc9-111">tooconfigure Azure AD-integrering med 15Five, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="cebc9-111">tooconfigure Azure AD integration with 15Five, you need hello following items:</span></span>

- <span data-ttu-id="cebc9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cebc9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cebc9-113">En 15Five enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="cebc9-113">A 15Five single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cebc9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cebc9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cebc9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cebc9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cebc9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cebc9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cebc9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cebc9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cebc9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cebc9-118">Scenario description</span></span>
<span data-ttu-id="cebc9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cebc9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cebc9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cebc9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cebc9-121">Att lägga till 15Five från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cebc9-121">Adding 15Five from hello gallery</span></span>
2. <span data-ttu-id="cebc9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cebc9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-15five-from-hello-gallery"></a><span data-ttu-id="cebc9-123">Att lägga till 15Five från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="cebc9-123">Adding 15Five from hello gallery</span></span>
<span data-ttu-id="cebc9-124">tooconfigure hello integrering av 15Five i Azure AD, behöver du tooadd 15Five hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="cebc9-124">tooconfigure hello integration of 15Five into Azure AD, you need tooadd 15Five from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cebc9-125">**tooadd 15Five från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cebc9-125">**tooadd 15Five from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cebc9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cebc9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cebc9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cebc9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cebc9-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cebc9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cebc9-133">Skriv i sökrutan hello **15Five**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-133">In hello search box, type **15Five**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_search.png)

5. <span data-ttu-id="cebc9-135">Markera hello resultat på panelen **15Five**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="cebc9-135">In hello results panel, select **15Five**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cebc9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cebc9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cebc9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 15Five baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cebc9-138">In this section, you configure and test Azure AD single sign-on with 15Five based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cebc9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i 15Five är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cebc9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in 15Five is tooa user in Azure AD.</span></span> <span data-ttu-id="cebc9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i 15Five toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="cebc9-140">In other words, a link relationship between an Azure AD user and hello related user in 15Five needs toobe established.</span></span>

<span data-ttu-id="cebc9-141">I 15Five, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cebc9-141">In 15Five, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="cebc9-142">tooconfigure och testa Azure AD enkel inloggning med 15Five, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cebc9-142">tooconfigure and test Azure AD single sign-on with 15Five, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cebc9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cebc9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cebc9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cebc9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cebc9-145">**[Skapa en testanvändare 15Five](#creating-a-15five-test-user)**  -toohave en motsvarighet för Britta Simon i 15Five som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cebc9-145">**[Creating a 15Five test user](#creating-a-15five-test-user)** - toohave a counterpart of Britta Simon in 15Five that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="cebc9-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cebc9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cebc9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cebc9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cebc9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cebc9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cebc9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt 15Five program.</span><span class="sxs-lookup"><span data-stu-id="cebc9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your 15Five application.</span></span>

<span data-ttu-id="cebc9-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med 15Five:**</span><span class="sxs-lookup"><span data-stu-id="cebc9-150">**tooconfigure Azure AD single sign-on with 15Five, perform hello following steps:**</span></span>

1. <span data-ttu-id="cebc9-151">I hello Azure-portalen på hello **15Five** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-151">In hello Azure portal, on hello **15Five** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cebc9-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cebc9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_samlbase.png)

3. <span data-ttu-id="cebc9-155">På hello **15Five domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cebc9-155">On hello **15Five Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_url.png)

    <span data-ttu-id="cebc9-157">a.</span><span class="sxs-lookup"><span data-stu-id="cebc9-157">a.</span></span> <span data-ttu-id="cebc9-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.15five.com`</span><span class="sxs-lookup"><span data-stu-id="cebc9-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.15five.com`</span></span>

    <span data-ttu-id="cebc9-159">b.</span><span class="sxs-lookup"><span data-stu-id="cebc9-159">b.</span></span> <span data-ttu-id="cebc9-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.15five.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="cebc9-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.15five.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cebc9-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cebc9-161">These values are not real.</span></span> <span data-ttu-id="cebc9-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="cebc9-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cebc9-163">Kontakta [15Five klienten supportteamet](https://www.15five.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cebc9-163">Contact [15Five Client support team](https://www.15five.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="cebc9-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="cebc9-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_certificate.png) 

5. <span data-ttu-id="cebc9-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cebc9-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cebc9-168">tooconfigure enkel inloggning på **15Five** sida, behöver du toosend hello hämtas **XML-Metadata för** för[15Five supportteamet](https://www.15five.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="cebc9-168">tooconfigure single sign-on on **15Five** side, you need toosend hello downloaded **Metadata XML** too[15Five support team](https://www.15five.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="cebc9-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="cebc9-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="cebc9-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="cebc9-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="cebc9-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cebc9-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cebc9-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cebc9-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="cebc9-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cebc9-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cebc9-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cebc9-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cebc9-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cebc9-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cebc9-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cebc9-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cebc9-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cebc9-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cebc9-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cebc9-184">a.</span><span class="sxs-lookup"><span data-stu-id="cebc9-184">a.</span></span> <span data-ttu-id="cebc9-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cebc9-186">b.</span><span class="sxs-lookup"><span data-stu-id="cebc9-186">b.</span></span> <span data-ttu-id="cebc9-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cebc9-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cebc9-188">c.</span><span class="sxs-lookup"><span data-stu-id="cebc9-188">c.</span></span> <span data-ttu-id="cebc9-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cebc9-190">d.</span><span class="sxs-lookup"><span data-stu-id="cebc9-190">d.</span></span> <span data-ttu-id="cebc9-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-191">Click **Create**.</span></span>
 
### <a name="creating-a-15five-test-user"></a><span data-ttu-id="cebc9-192">Skapa en testanvändare 15Five</span><span class="sxs-lookup"><span data-stu-id="cebc9-192">Creating a 15Five test user</span></span>

<span data-ttu-id="cebc9-193">tooenable Azure AD-användare toolog i too15Five, måste de etableras i 15Five.</span><span class="sxs-lookup"><span data-stu-id="cebc9-193">tooenable Azure AD users toolog in too15Five, they must be provisioned into 15Five.</span></span> <span data-ttu-id="cebc9-194">När 15Five, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="cebc9-194">When 15Five, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="cebc9-195">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="cebc9-195">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="cebc9-196">Logga in tooyour **15Five** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="cebc9-196">Log in tooyour **15Five** company site as administrator.</span></span>

2. <span data-ttu-id="cebc9-197">Gå för**hantera företagets**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-197">Go too**Manage Company**.</span></span>
   
    <span data-ttu-id="cebc9-198">![Hantera företagets](./media/active-directory-saas-15five-tutorial/IC784675.png "hantera företagets")</span><span class="sxs-lookup"><span data-stu-id="cebc9-198">![Manage Company](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")</span></span>

3. <span data-ttu-id="cebc9-199">Gå för**personer \> lägga till personer**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-199">Go too**People \> Add People**.</span></span>
   
    <span data-ttu-id="cebc9-200">![Personer](./media/active-directory-saas-15five-tutorial/IC784676.png "personer")</span><span class="sxs-lookup"><span data-stu-id="cebc9-200">![People](./media/active-directory-saas-15five-tutorial/IC784676.png "People")</span></span>

4. <span data-ttu-id="cebc9-201">I hello Lägg till ny Person avsnitt, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="cebc9-201">In hello Add New Person section, perform hello following steps:</span></span>
   
    <span data-ttu-id="cebc9-202">![Lägg till ny Person](./media/active-directory-saas-15five-tutorial/IC784677.png "Lägg till ny Person")</span><span class="sxs-lookup"><span data-stu-id="cebc9-202">![Add New Person](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")</span></span>
   
    <span data-ttu-id="cebc9-203">a.</span><span class="sxs-lookup"><span data-stu-id="cebc9-203">a.</span></span> <span data-ttu-id="cebc9-204">Typen hello **Förnamn**, **efternamn**, **rubrik**, **e-postadress** ett giltigt Azure Active Directory-konto som du vill tooprovision till hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="cebc9-204">Type hello **First Name**, **Last Name**, **Title**, **Email address** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="cebc9-205">b.</span><span class="sxs-lookup"><span data-stu-id="cebc9-205">b.</span></span> <span data-ttu-id="cebc9-206">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-206">Click **Done**.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="cebc9-207">hello Azure AD-kontot innehavaren får ett e-postmeddelande inklusive länken tooconfirm hello kontot innan det aktiveras.</span><span class="sxs-lookup"><span data-stu-id="cebc9-207">hello Azure AD account holder receives an email including a link tooconfirm hello account before it becomes active.</span></span>
   
### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cebc9-208">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="cebc9-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cebc9-209">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst too15Five.</span><span class="sxs-lookup"><span data-stu-id="cebc9-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access too15Five.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cebc9-211">**tooassign Britta Simon too15Five utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cebc9-211">**tooassign Britta Simon too15Five, perform hello following steps:**</span></span>

1. <span data-ttu-id="cebc9-212">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cebc9-214">Välj i listan med program hello **15Five**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-214">In hello applications list, select **15Five**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_app.png) 

3. <span data-ttu-id="cebc9-216">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cebc9-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cebc9-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cebc9-218">Click **Add** button.</span></span> <span data-ttu-id="cebc9-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cebc9-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cebc9-221">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="cebc9-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cebc9-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cebc9-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cebc9-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cebc9-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cebc9-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cebc9-224">Testing single sign-on</span></span>

<span data-ttu-id="cebc9-225">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cebc9-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="cebc9-226">När du klickar på hello 15Five panelen i hello åtkomstpanelen bör du hämta inloggningssidan för 15Five program.</span><span class="sxs-lookup"><span data-stu-id="cebc9-226">When you click hello 15Five tile in hello Access Panel, you should get login page of 15Five application.</span></span>
<span data-ttu-id="cebc9-227">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cebc9-227">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cebc9-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cebc9-228">Additional resources</span></span>

* [<span data-ttu-id="cebc9-229">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cebc9-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cebc9-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cebc9-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-15five-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-15five-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-15five-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-15five-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-15five-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-15five-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-15five-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-15five-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-15five-tutorial/tutorial_general_203.png

