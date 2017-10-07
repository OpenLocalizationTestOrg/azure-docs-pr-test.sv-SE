---
title: "Självstudier: Azure Active Directory-integrering med ersättning Gateway | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ersättning Gateway."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34336386-998a-4d47-ab55-721d97708e5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 30cdd538471b373468c3d990e4568ddb08081a76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-reward-gateway"></a><span data-ttu-id="46ff7-103">Självstudier: Azure Active Directory-integrering med ersättning Gateway</span><span class="sxs-lookup"><span data-stu-id="46ff7-103">Tutorial: Azure Active Directory integration with Reward Gateway</span></span>

<span data-ttu-id="46ff7-104">I kursen får du lära dig hur toointegrate ersättning Gateway med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="46ff7-104">In this tutorial, you learn how toointegrate Reward Gateway with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="46ff7-105">Integrera ersättning Gateway med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="46ff7-105">Integrating Reward Gateway with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="46ff7-106">Du kan styra i Azure AD som har åtkomst tooReward Gateway</span><span class="sxs-lookup"><span data-stu-id="46ff7-106">You can control in Azure AD who has access tooReward Gateway</span></span>
- <span data-ttu-id="46ff7-107">Du kan aktivera din användare tooautomatically get inloggade tooReward Gateway (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="46ff7-107">You can enable your users tooautomatically get signed-on tooReward Gateway (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="46ff7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="46ff7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="46ff7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="46ff7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46ff7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="46ff7-110">Prerequisites</span></span>

<span data-ttu-id="46ff7-111">tooconfigure Azure AD-integrering med ersättning Gateway, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="46ff7-111">tooconfigure Azure AD integration with Reward Gateway, you need hello following items:</span></span>

- <span data-ttu-id="46ff7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="46ff7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="46ff7-113">En Gateway för ersättning enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="46ff7-113">A Reward Gateway single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="46ff7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="46ff7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="46ff7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="46ff7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="46ff7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="46ff7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="46ff7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="46ff7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="46ff7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="46ff7-118">Scenario description</span></span>
<span data-ttu-id="46ff7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="46ff7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="46ff7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="46ff7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="46ff7-121">Lägger till ersättning Gateway från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="46ff7-121">Adding Reward Gateway from hello gallery</span></span>
2. <span data-ttu-id="46ff7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46ff7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-reward-gateway-from-hello-gallery"></a><span data-ttu-id="46ff7-123">Lägger till ersättning Gateway från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="46ff7-123">Adding Reward Gateway from hello gallery</span></span>
<span data-ttu-id="46ff7-124">tooconfigure hello integrering av ersättning Gateway i Azure AD, behöver du tooadd ersättning Gateway från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="46ff7-124">tooconfigure hello integration of Reward Gateway into Azure AD, you need tooadd Reward Gateway from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="46ff7-125">**tooadd ersättning Gateway från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46ff7-125">**tooadd Reward Gateway from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="46ff7-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="46ff7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="46ff7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="46ff7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="46ff7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46ff7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="46ff7-133">Skriv i sökrutan hello **ersättning Gateway**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-133">In hello search box, type **Reward Gateway**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_search.png)

5. <span data-ttu-id="46ff7-135">Markera hello resultat på panelen **ersättning Gateway**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="46ff7-135">In hello results panel, select **Reward Gateway**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="46ff7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46ff7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="46ff7-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med ersättning Gateway baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="46ff7-138">In this section, you configure and test Azure AD single sign-on with Reward Gateway based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="46ff7-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ersättning Gateway är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="46ff7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Reward Gateway is tooa user in Azure AD.</span></span> <span data-ttu-id="46ff7-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ersättning Gateway toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="46ff7-140">In other words, a link relationship between an Azure AD user and hello related user in Reward Gateway needs toobe established.</span></span>

<span data-ttu-id="46ff7-141">Tilldela hello värdet hello i ersättning Gateway **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="46ff7-141">In Reward Gateway, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="46ff7-142">tooconfigure och testa Azure AD enkel inloggning med ersättning Gateway, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="46ff7-142">tooconfigure and test Azure AD single sign-on with Reward Gateway, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="46ff7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="46ff7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="46ff7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="46ff7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="46ff7-145">**[Skapa en testanvändare ersättning Gateway](#creating-a-reward-gateway-test-user)**  -toohave en motsvarighet för Britta Simon i ersättning Gateway som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="46ff7-145">**[Creating a Reward Gateway test user](#creating-a-reward-gateway-test-user)** - toohave a counterpart of Britta Simon in Reward Gateway that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="46ff7-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="46ff7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="46ff7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="46ff7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="46ff7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46ff7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="46ff7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för ersättning Gateway.</span><span class="sxs-lookup"><span data-stu-id="46ff7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Reward Gateway application.</span></span>

<span data-ttu-id="46ff7-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med ersättning Gateway:**</span><span class="sxs-lookup"><span data-stu-id="46ff7-150">**tooconfigure Azure AD single sign-on with Reward Gateway, perform hello following steps:**</span></span>

1. <span data-ttu-id="46ff7-151">I hello Azure-portalen på hello **ersättning Gateway** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-151">In hello Azure portal, on hello **Reward Gateway** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="46ff7-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="46ff7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_samlbase.png)

3. <span data-ttu-id="46ff7-155">På hello **ersättning Gateway-domänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="46ff7-155">On hello **Reward Gateway Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_url.png)

    <span data-ttu-id="46ff7-157">a.</span><span class="sxs-lookup"><span data-stu-id="46ff7-157">a.</span></span> <span data-ttu-id="46ff7-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="46ff7-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.rewardgateway.com` |
    | `https://<companyname>.rewardgateway.co.uk/` |
    | `https://<companyname>.rewardgateway.co.nz/` |
    | `https://<companyname>.rewardgateway.com.au/` |

    <span data-ttu-id="46ff7-159">b.</span><span class="sxs-lookup"><span data-stu-id="46ff7-159">b.</span></span> <span data-ttu-id="46ff7-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="46ff7-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    |  `https://<companyname>.rewardgateway.com/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.co.uk/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.co.nz/Authentication/EndLogin?idp=<Unique Id>` |
    | `https://<companyname>.rewardgateway.com.au/Authentication/EndLogin?idp=<Unique Id>` |

    > [!NOTE] 
    > <span data-ttu-id="46ff7-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="46ff7-161">These values are not real.</span></span> <span data-ttu-id="46ff7-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="46ff7-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="46ff7-163">Kontakta [ersättning Gateway supportteamet](mailto:clientsupport@rewardgateway.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="46ff7-163">Contact [Reward Gateway support team](mailto:clientsupport@rewardgateway.com) tooget these values.</span></span>
 
4. <span data-ttu-id="46ff7-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="46ff7-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_certificate.png) 

5. <span data-ttu-id="46ff7-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="46ff7-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="46ff7-168">tooconfigure enkel inloggning på **ersättning Gateway** sida, behöver du toosend hello hämtas **XML-Metadata för** för[ersättning Gateway supportteamet](mailto:clientsupport@rewardgateway.com).</span><span class="sxs-lookup"><span data-stu-id="46ff7-168">tooconfigure single sign-on on **Reward Gateway** side, you need toosend hello downloaded **Metadata XML** too[Reward Gateway support team](mailto:clientsupport@rewardgateway.com).</span></span> <span data-ttu-id="46ff7-169">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="46ff7-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="46ff7-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="46ff7-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="46ff7-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="46ff7-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="46ff7-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="46ff7-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="46ff7-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="46ff7-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="46ff7-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="46ff7-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="46ff7-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46ff7-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="46ff7-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="46ff7-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="46ff7-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="46ff7-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46ff7-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="46ff7-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="46ff7-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="46ff7-185">a.</span><span class="sxs-lookup"><span data-stu-id="46ff7-185">a.</span></span> <span data-ttu-id="46ff7-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="46ff7-187">b.</span><span class="sxs-lookup"><span data-stu-id="46ff7-187">b.</span></span> <span data-ttu-id="46ff7-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="46ff7-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="46ff7-189">c.</span><span class="sxs-lookup"><span data-stu-id="46ff7-189">c.</span></span> <span data-ttu-id="46ff7-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="46ff7-191">d.</span><span class="sxs-lookup"><span data-stu-id="46ff7-191">d.</span></span> <span data-ttu-id="46ff7-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-192">Click **Create**.</span></span>
 
### <a name="creating-a-reward-gateway-test-user"></a><span data-ttu-id="46ff7-193">Skapa en testanvändare ersättning Gateway</span><span class="sxs-lookup"><span data-stu-id="46ff7-193">Creating a Reward Gateway test user</span></span>

<span data-ttu-id="46ff7-194">I det här avsnittet kan du skapa en användare som kallas Britta Simon i ersättning Gateway.</span><span class="sxs-lookup"><span data-stu-id="46ff7-194">In this section, you create a user called Britta Simon in Reward Gateway.</span></span> <span data-ttu-id="46ff7-195">Arbeta med ersättning Gateway [supportteam](mailto:clientsupport@rewardgateway.com) tooadd hello användare i hello ersättning Gateway-plattformen.</span><span class="sxs-lookup"><span data-stu-id="46ff7-195">Work with Reward Gateway [support team](mailto:clientsupport@rewardgateway.com) tooadd hello users in hello Reward Gateway platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="46ff7-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="46ff7-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="46ff7-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooReward Gateway.</span><span class="sxs-lookup"><span data-stu-id="46ff7-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooReward Gateway.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="46ff7-199">**tooassign Britta Simon tooReward Gateway, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="46ff7-199">**tooassign Britta Simon tooReward Gateway, perform hello following steps:**</span></span>

1. <span data-ttu-id="46ff7-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="46ff7-202">Välj i listan med program hello **ersättning Gateway**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-202">In hello applications list, select **Reward Gateway**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_app.png) 

3. <span data-ttu-id="46ff7-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="46ff7-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="46ff7-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="46ff7-206">Click **Add** button.</span></span> <span data-ttu-id="46ff7-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46ff7-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="46ff7-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="46ff7-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="46ff7-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46ff7-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="46ff7-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46ff7-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="46ff7-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46ff7-212">Testing single sign-on</span></span>

<span data-ttu-id="46ff7-213">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="46ff7-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="46ff7-214">Du bör få automatiskt inloggade tooyour ersättning Gateway programmet när du klickar på hello ersättning Gateway-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="46ff7-214">When you click hello Reward Gateway tile in hello Access Panel, you should get automatically signed-on tooyour Reward Gateway application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46ff7-215">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="46ff7-215">Additional resources</span></span>

* [<span data-ttu-id="46ff7-216">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="46ff7-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="46ff7-217">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="46ff7-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_203.png

