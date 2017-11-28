---
title: "Självstudier: Azure Active Directory-integrering med önster Karlsson Factiva | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och önster Karlsson Factiva."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b36e97e8-37a6-4096-a894-530427ee1331
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: 7c42b5d64433c7bdcb512771a3e68115cc5f6874
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dow-jones-factiva"></a><span data-ttu-id="e0e79-103">Självstudier: Azure Active Directory-integrering med önster Karlsson Factiva</span><span class="sxs-lookup"><span data-stu-id="e0e79-103">Tutorial: Azure Active Directory integration with Dow Jones Factiva</span></span>

<span data-ttu-id="e0e79-104">I kursen får du lära dig hur toointegrate önster Karlsson Factiva med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e0e79-104">In this tutorial, you learn how toointegrate Dow Jones Factiva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e0e79-105">Integrera önster Karlsson Factiva med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e0e79-105">Integrating Dow Jones Factiva with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e0e79-106">Du kan styra i Azure AD som har åtkomst tooDow Karlsson Factiva</span><span class="sxs-lookup"><span data-stu-id="e0e79-106">You can control in Azure AD who has access tooDow Jones Factiva</span></span>
- <span data-ttu-id="e0e79-107">Du kan aktivera din användare tooautomatically get inloggade tooDow Karlsson Factiva (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e0e79-107">You can enable your users tooautomatically get signed-on tooDow Jones Factiva (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e0e79-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e0e79-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e0e79-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e0e79-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0e79-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e0e79-110">Prerequisites</span></span>

<span data-ttu-id="e0e79-111">tooconfigure Azure AD-integrering med önster Karlsson Factiva, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e0e79-111">tooconfigure Azure AD integration with Dow Jones Factiva, you need hello following items:</span></span>

- <span data-ttu-id="e0e79-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0e79-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e0e79-113">En önster Karlsson Factiva enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e0e79-113">A Dow Jones Factiva single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e0e79-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0e79-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e0e79-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e0e79-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e0e79-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e0e79-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e0e79-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0e79-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e0e79-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e0e79-118">Scenario description</span></span>
<span data-ttu-id="e0e79-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e0e79-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e0e79-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0e79-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e0e79-121">Att lägga till önster Karlsson Factiva från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e0e79-121">Adding Dow Jones Factiva from hello gallery</span></span>
2. <span data-ttu-id="e0e79-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0e79-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dow-jones-factiva-from-hello-gallery"></a><span data-ttu-id="e0e79-123">Att lägga till önster Karlsson Factiva från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e0e79-123">Adding Dow Jones Factiva from hello gallery</span></span>
<span data-ttu-id="e0e79-124">tooconfigure hello integrering av önster Karlsson Factiva i Azure AD, behöver du tooadd önster Karlsson Factiva hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e0e79-124">tooconfigure hello integration of Dow Jones Factiva into Azure AD, you need tooadd Dow Jones Factiva from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e0e79-125">**tooadd önster Karlsson Factiva från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0e79-125">**tooadd Dow Jones Factiva from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0e79-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e0e79-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e0e79-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e0e79-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e0e79-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0e79-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e0e79-133">Skriv i sökrutan hello **önster Karlsson Factiva**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-133">In hello search box, type **Dow Jones Factiva**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_search.png)

5. <span data-ttu-id="e0e79-135">Markera hello resultat på panelen **önster Karlsson Factiva**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e0e79-135">In hello results panel, select **Dow Jones Factiva**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e0e79-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0e79-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e0e79-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med önster Karlsson Factiva baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e0e79-138">In this section, you configure and test Azure AD single sign-on with Dow Jones Factiva based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e0e79-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i önster Karlsson Factiva är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e0e79-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Dow Jones Factiva is tooa user in Azure AD.</span></span> <span data-ttu-id="e0e79-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i önster Karlsson Factiva toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e0e79-140">In other words, a link relationship between an Azure AD user and hello related user in Dow Jones Factiva needs toobe established.</span></span>

<span data-ttu-id="e0e79-141">Tilldela hello värdet för hello i önster Karlsson Factiva **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e0e79-141">In Dow Jones Factiva, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e0e79-142">tooconfigure och testa Azure AD enkel inloggning med önster Karlsson Factiva, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e0e79-142">tooconfigure and test Azure AD single sign-on with Dow Jones Factiva, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e0e79-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e0e79-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e0e79-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0e79-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e0e79-145">**[Skapa en testanvändare önster Karlsson Factiva](#creating-a-dow-jones-factiva-test-user)**  -toohave en motsvarighet för Britta Simon i önster Karlsson Factiva som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e0e79-145">**[Creating a Dow Jones Factiva test user](#creating-a-dow-jones-factiva-test-user)** - toohave a counterpart of Britta Simon in Dow Jones Factiva that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e0e79-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0e79-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e0e79-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e0e79-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e0e79-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0e79-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e0e79-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet önster Karlsson Factiva.</span><span class="sxs-lookup"><span data-stu-id="e0e79-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Dow Jones Factiva application.</span></span>

<span data-ttu-id="e0e79-150">**tooconfigure Azure AD enkel inloggning med önster Karlsson Factiva utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0e79-150">**tooconfigure Azure AD single sign-on with Dow Jones Factiva, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0e79-151">I hello Azure-portalen på hello **önster Karlsson Factiva** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-151">In hello Azure portal, on hello **Dow Jones Factiva** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e0e79-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e0e79-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_samlbase.png)

3. <span data-ttu-id="e0e79-155">På hello **önster Karlsson Factiva domän och URL: er** avsnittet, hello användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="e0e79-155">On hello **Dow Jones Factiva Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_url.png)

4. <span data-ttu-id="e0e79-157">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="e0e79-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_certificate.png) 

5. <span data-ttu-id="e0e79-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0e79-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e0e79-161">tooconfigure enkel inloggning på **önster Karlsson Factiva** sida, behöver du toosend hello hämtas **XML-Metadata för** för[önster Karlsson Factiva supportteamet](https://www.dowjones.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="e0e79-161">tooconfigure single sign-on on **Dow Jones Factiva** side, you need toosend hello downloaded **Metadata XML** too[Dow Jones Factiva support team](https://www.dowjones.com/contact/).</span></span> <span data-ttu-id="e0e79-162">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="e0e79-162">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e0e79-163">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="e0e79-163">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e0e79-164">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="e0e79-164">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e0e79-165">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e0e79-165">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e0e79-166">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0e79-166">Creating an Azure AD test user</span></span>
<span data-ttu-id="e0e79-167">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e0e79-167">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e0e79-169">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0e79-169">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0e79-170">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e0e79-170">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e0e79-172">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-172">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e0e79-174">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0e79-174">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e0e79-176">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e0e79-176">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e0e79-178">a.</span><span class="sxs-lookup"><span data-stu-id="e0e79-178">a.</span></span> <span data-ttu-id="e0e79-179">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-179">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e0e79-180">b.</span><span class="sxs-lookup"><span data-stu-id="e0e79-180">b.</span></span> <span data-ttu-id="e0e79-181">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e0e79-181">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e0e79-182">c.</span><span class="sxs-lookup"><span data-stu-id="e0e79-182">c.</span></span> <span data-ttu-id="e0e79-183">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-183">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e0e79-184">d.</span><span class="sxs-lookup"><span data-stu-id="e0e79-184">d.</span></span> <span data-ttu-id="e0e79-185">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-185">Click **Create**.</span></span>
 
### <a name="creating-a-dow-jones-factiva-test-user"></a><span data-ttu-id="e0e79-186">Skapa en testanvändare önster Karlsson Factiva</span><span class="sxs-lookup"><span data-stu-id="e0e79-186">Creating a Dow Jones Factiva test user</span></span>

<span data-ttu-id="e0e79-187">I det här avsnittet skapar du en användare som kallas Britta Simon i önster Karlsson Factiva.</span><span class="sxs-lookup"><span data-stu-id="e0e79-187">In this section, you create a user called Britta Simon in Dow Jones Factiva.</span></span> <span data-ttu-id="e0e79-188">Se tillsammans med önster [Karlsson Factiva supportteamet](https://www.dowjones.com/contact/) tooadd hello användare i hello önster Karlsson Factiva plattform.</span><span class="sxs-lookup"><span data-stu-id="e0e79-188">Please work with Dow [Jones Factiva support team](https://www.dowjones.com/contact/) tooadd hello users in hello Dow Jones Factiva platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e0e79-189">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e0e79-189">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e0e79-190">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDow Karlsson Factiva.</span><span class="sxs-lookup"><span data-stu-id="e0e79-190">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDow Jones Factiva.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e0e79-192">**tooassign Britta Simon tooDow Karlsson Factiva utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e0e79-192">**tooassign Britta Simon tooDow Jones Factiva, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0e79-193">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-193">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e0e79-195">Välj i listan med program hello **önster Karlsson Factiva**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-195">In hello applications list, select **Dow Jones Factiva**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_app.png) 

3. <span data-ttu-id="e0e79-197">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e0e79-197">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e0e79-199">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0e79-199">Click **Add** button.</span></span> <span data-ttu-id="e0e79-200">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0e79-200">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e0e79-202">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e0e79-202">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e0e79-203">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0e79-203">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e0e79-204">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e0e79-204">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e0e79-205">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e0e79-205">Testing single sign-on</span></span>

<span data-ttu-id="e0e79-206">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e0e79-206">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e0e79-207">Du bör få automatiskt inloggade tooyour önster Karlsson Factiva programmet när du klickar på hello önster Karlsson Factiva panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e0e79-207">When you click hello Dow Jones Factiva tile in hello Access Panel, you should get automatically signed-on tooyour Dow Jones Factiva application.</span></span>
<span data-ttu-id="e0e79-208">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e0e79-208">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e0e79-209">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e0e79-209">Additional resources</span></span>

* [<span data-ttu-id="e0e79-210">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e0e79-210">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e0e79-211">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e0e79-211">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_203.png

