---
title: "Självstudier: Azure Active Directory-integrering med Fuze | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Fuze."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9780b4bf-1fd1-48c1-9ceb-f750225ae08a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: d0ea8c6456824e348301ed8bf1f5e00f4bfa8121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuze"></a><span data-ttu-id="9560d-103">Självstudier: Azure Active Directory-integrering med Fuze</span><span class="sxs-lookup"><span data-stu-id="9560d-103">Tutorial: Azure Active Directory integration with Fuze</span></span>

<span data-ttu-id="9560d-104">I kursen får du lära dig hur toointegrate Fuze med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9560d-104">In this tutorial, you learn how toointegrate Fuze with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9560d-105">Integrera Fuze med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9560d-105">Integrating Fuze with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9560d-106">Du kan styra i Azure AD som har åtkomst till tooFuze</span><span class="sxs-lookup"><span data-stu-id="9560d-106">You can control in Azure AD who has access tooFuze</span></span>
- <span data-ttu-id="9560d-107">Du kan aktivera din användare tooautomatically get inloggade tooFuze (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9560d-107">You can enable your users tooautomatically get signed-on tooFuze (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9560d-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="9560d-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="9560d-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9560d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9560d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9560d-110">Prerequisites</span></span>

<span data-ttu-id="9560d-111">tooconfigure Azure AD-integrering med Fuze, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9560d-111">tooconfigure Azure AD integration with Fuze, you need hello following items:</span></span>

- <span data-ttu-id="9560d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9560d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9560d-113">En Fuze enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="9560d-113">A Fuze single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="9560d-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9560d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="9560d-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9560d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9560d-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9560d-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="9560d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9560d-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="9560d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9560d-118">Scenario description</span></span>
<span data-ttu-id="9560d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9560d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9560d-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9560d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9560d-121">Att lägga till Fuze från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9560d-121">Adding Fuze from hello gallery</span></span>
2. <span data-ttu-id="9560d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9560d-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-fuze-from-hello-gallery"></a><span data-ttu-id="9560d-123">Att lägga till Fuze från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9560d-123">Adding Fuze from hello gallery</span></span>
<span data-ttu-id="9560d-124">tooconfigure hello integrering av Fuze i Azure AD, behöver du tooadd Fuze hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9560d-124">tooconfigure hello integration of Fuze into Azure AD, you need tooadd Fuze from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9560d-125">**tooadd Fuze från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9560d-125">**tooadd Fuze from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9560d-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9560d-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9560d-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9560d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9560d-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9560d-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9560d-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9560d-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9560d-133">Skriv i sökrutan hello **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="9560d-133">In hello search box, type **Fuze**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_000.png)

5. <span data-ttu-id="9560d-135">Markera hello resultat på panelen **Fuze**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9560d-135">In hello results panel, select **Fuze**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9560d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9560d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9560d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Fuze baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9560d-138">In this section, you configure and test Azure AD single sign-on with Fuze based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9560d-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Fuze är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9560d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Fuze is tooa user in Azure AD.</span></span> <span data-ttu-id="9560d-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Fuze toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9560d-140">In other words, a link relationship between an Azure AD user and hello related user in Fuze needs toobe established.</span></span>

<span data-ttu-id="9560d-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Fuze.</span><span class="sxs-lookup"><span data-stu-id="9560d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Fuze.</span></span>

<span data-ttu-id="9560d-142">tooconfigure och testa Azure AD enkel inloggning med Fuze, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9560d-142">tooconfigure and test Azure AD single sign-on with Fuze, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9560d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9560d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9560d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9560d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9560d-145">**[Skapa en testanvändare Fuze](#creating-a-fuze-test-user)**  -toohave en motsvarighet för Britta Simon i Fuze som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="9560d-145">**[Creating a Fuze test user](#creating-a-fuze-test-user)** - toohave a counterpart of Britta Simon in Fuze that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="9560d-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9560d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9560d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9560d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9560d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9560d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9560d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Fuze program.</span><span class="sxs-lookup"><span data-stu-id="9560d-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Fuze application.</span></span>

<span data-ttu-id="9560d-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Fuze:**</span><span class="sxs-lookup"><span data-stu-id="9560d-150">**tooconfigure Azure AD single sign-on with Fuze, perform hello following steps:**</span></span>

1. <span data-ttu-id="9560d-151">I hello Azure Management portal på hello **Fuze** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9560d-151">In hello Azure Management portal, on hello **Fuze** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9560d-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9560d-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_01.png)

3. <span data-ttu-id="9560d-155">På hello **Fuze domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9560d-155">On hello **Fuze Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_020.png)
    
    <span data-ttu-id="9560d-157">I hello **inloggning URL** textruta typen hello inloggning URL som:`https://www.thinkingphones.com/jetspeed/portal/`</span><span class="sxs-lookup"><span data-stu-id="9560d-157">In hello **Sign on URL** textbox, type hello Sign-on URL as: `https://www.thinkingphones.com/jetspeed/portal/`</span></span>

4.  <span data-ttu-id="9560d-158">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9560d-158">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="9560d-160">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="9560d-160">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello xml file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_05.png) 

6. <span data-ttu-id="9560d-162">tooconfigure enkel inloggning på **Fuze** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Fuze supportteamet](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="9560d-162">tooconfigure single sign-on on **Fuze** side, you need toosend hello downloaded **Metadata XML** too[Fuze support team](https://www.fuze.com/support).</span></span> <span data-ttu-id="9560d-163">De ska ställa in detta i ordning toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="9560d-163">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9560d-164">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9560d-164">Creating an Azure AD test user</span></span>
<span data-ttu-id="9560d-165">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9560d-165">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9560d-167">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9560d-167">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9560d-168">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9560d-168">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9560d-170">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="9560d-170">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9560d-172">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9560d-172">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9560d-174">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9560d-174">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9560d-176">a.</span><span class="sxs-lookup"><span data-stu-id="9560d-176">a.</span></span> <span data-ttu-id="9560d-177">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9560d-177">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9560d-178">b.</span><span class="sxs-lookup"><span data-stu-id="9560d-178">b.</span></span> <span data-ttu-id="9560d-179">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9560d-179">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9560d-180">c.</span><span class="sxs-lookup"><span data-stu-id="9560d-180">c.</span></span> <span data-ttu-id="9560d-181">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9560d-181">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9560d-182">d.</span><span class="sxs-lookup"><span data-stu-id="9560d-182">d.</span></span> <span data-ttu-id="9560d-183">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9560d-183">Click **Create**.</span></span> 


### <a name="creating-a-fuze-test-user"></a><span data-ttu-id="9560d-184">Skapa en testanvändare Fuze</span><span class="sxs-lookup"><span data-stu-id="9560d-184">Creating a Fuze test user</span></span>

<span data-ttu-id="9560d-185">Fuze program stöder fullständig precis i tid användaren etablera så att användarna ska skapas automatiskt när de loggar in.</span><span class="sxs-lookup"><span data-stu-id="9560d-185">Fuze application supports full Just in time user provision, so users will get created automatically when they sign-in.</span></span> <span data-ttu-id="9560d-186">För andra information kontakta Fuze [stöder](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="9560d-186">For any other clarification, please contact Fuze [support](https://www.fuze.com/support).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9560d-187">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9560d-187">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9560d-188">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooFuze sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9560d-188">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooFuze.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9560d-190">**tooassign Britta Simon tooFuze utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9560d-190">**tooassign Britta Simon tooFuze, perform hello following steps:**</span></span>

1. <span data-ttu-id="9560d-191">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9560d-191">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9560d-193">Välj i listan med program hello **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="9560d-193">In hello applications list, select **Fuze**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_50.png) 

3. <span data-ttu-id="9560d-195">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9560d-195">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9560d-197">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9560d-197">Click **Add** button.</span></span> <span data-ttu-id="9560d-198">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9560d-198">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9560d-200">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9560d-200">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9560d-201">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9560d-201">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9560d-202">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9560d-202">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="testing-single-sign-on"></a><span data-ttu-id="9560d-203">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9560d-203">Testing single sign-on</span></span>

<span data-ttu-id="9560d-204">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9560d-204">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="9560d-205">Du bör få automatiskt inloggade tooyour Fuze programmet när du klickar på hello Fuze panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9560d-205">When you click hello Fuze tile in hello Access Panel, you should get automatically signed-on tooyour Fuze application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="9560d-206">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9560d-206">Additional resources</span></span>

* [<span data-ttu-id="9560d-207">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9560d-207">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9560d-208">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9560d-208">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_203.png