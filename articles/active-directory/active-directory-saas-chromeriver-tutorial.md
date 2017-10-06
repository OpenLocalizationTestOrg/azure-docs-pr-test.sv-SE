---
title: "Självstudier: Azure Active Directory-integrering med Chromeriver | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Chromeriver."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 445c5600-e340-4724-a9cb-3cfaf5770b70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: 7cf0e36fb6407bf3915e26717a2a997ac13110e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-chromeriver"></a><span data-ttu-id="46633-103">Självstudier: Azure Active Directory-integrering med Chromeriver</span><span class="sxs-lookup"><span data-stu-id="46633-103">Tutorial: Azure Active Directory integration with Chromeriver</span></span>

<span data-ttu-id="46633-104">I kursen får du lära dig hur toointegrate Chromeriver med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="46633-104">In this tutorial, you learn how toointegrate Chromeriver with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="46633-105">Integrera Chromeriver med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="46633-105">Integrating Chromeriver with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="46633-106">Du kan styra i Azure AD som har åtkomst till tooChromeriver</span><span class="sxs-lookup"><span data-stu-id="46633-106">You can control in Azure AD who has access tooChromeriver</span></span>
- <span data-ttu-id="46633-107">Du kan aktivera din användare tooautomatically get inloggade tooChromeriver (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="46633-107">You can enable your users tooautomatically get signed-on tooChromeriver (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="46633-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="46633-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="46633-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="46633-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="46633-110">Krav</span><span class="sxs-lookup"><span data-stu-id="46633-110">Prerequisites</span></span>

<span data-ttu-id="46633-111">tooconfigure Azure AD-integrering med Chromeriver, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="46633-111">tooconfigure Azure AD integration with Chromeriver, you need hello following items:</span></span>

- <span data-ttu-id="46633-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="46633-112">An Azure AD subscription</span></span>
- <span data-ttu-id="46633-113">En Chromeriver enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="46633-113">A Chromeriver single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="46633-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="46633-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="46633-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="46633-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="46633-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="46633-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="46633-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="46633-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="46633-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="46633-118">Scenario description</span></span>
<span data-ttu-id="46633-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="46633-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="46633-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="46633-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="46633-121">Att lägga till Chromeriver från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="46633-121">Adding Chromeriver from hello gallery</span></span>
2. <span data-ttu-id="46633-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46633-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-chromeriver-from-hello-gallery"></a><span data-ttu-id="46633-123">Att lägga till Chromeriver från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="46633-123">Adding Chromeriver from hello gallery</span></span>
<span data-ttu-id="46633-124">tooconfigure hello integrering av Chromeriver i Azure AD, behöver du tooadd Chromeriver hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="46633-124">tooconfigure hello integration of Chromeriver into Azure AD, you need tooadd Chromeriver from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="46633-125">**tooadd Chromeriver från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46633-125">**tooadd Chromeriver from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="46633-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="46633-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="46633-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="46633-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="46633-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="46633-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="46633-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46633-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="46633-133">Skriv i sökrutan hello **Chromeriver**.</span><span class="sxs-lookup"><span data-stu-id="46633-133">In hello search box, type **Chromeriver**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_search.png)

5. <span data-ttu-id="46633-135">Markera hello resultat på panelen **Chromeriver**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="46633-135">In hello results panel, select **Chromeriver**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="46633-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46633-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="46633-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Chromeriver baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="46633-138">In this section, you configure and test Azure AD single sign-on with Chromeriver based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="46633-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Chromeriver är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="46633-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Chromeriver is tooa user in Azure AD.</span></span> <span data-ttu-id="46633-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Chromeriver toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="46633-140">In other words, a link relationship between an Azure AD user and hello related user in Chromeriver needs toobe established.</span></span>

<span data-ttu-id="46633-141">I Chromeriver, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="46633-141">In Chromeriver, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="46633-142">tooconfigure och testa Azure AD enkel inloggning med Chromeriver, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="46633-142">tooconfigure and test Azure AD single sign-on with Chromeriver, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="46633-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="46633-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="46633-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="46633-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="46633-145">**[Skapa en testanvändare Chromeriver](#creating-a-chromeriver-test-user)**  -toohave en motsvarighet för Britta Simon i Chromeriver som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="46633-145">**[Creating a Chromeriver test user](#creating-a-chromeriver-test-user)** - toohave a counterpart of Britta Simon in Chromeriver that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="46633-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="46633-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="46633-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="46633-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="46633-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46633-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="46633-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Chromeriver program.</span><span class="sxs-lookup"><span data-stu-id="46633-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Chromeriver application.</span></span>

<span data-ttu-id="46633-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Chromeriver:**</span><span class="sxs-lookup"><span data-stu-id="46633-150">**tooconfigure Azure AD single sign-on with Chromeriver, perform hello following steps:**</span></span>

1. <span data-ttu-id="46633-151">I hello Azure-portalen på hello **Chromeriver** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="46633-151">In hello Azure portal, on hello **Chromeriver** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="46633-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="46633-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_samlbase.png)

3. <span data-ttu-id="46633-155">På hello **Chromeriver domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="46633-155">On hello **Chromeriver Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_url.png)

    <span data-ttu-id="46633-157">a.</span><span class="sxs-lookup"><span data-stu-id="46633-157">a.</span></span> <span data-ttu-id="46633-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.chromeriver.com`</span><span class="sxs-lookup"><span data-stu-id="46633-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.chromeriver.com`</span></span>

    <span data-ttu-id="46633-159">b.</span><span class="sxs-lookup"><span data-stu-id="46633-159">b.</span></span> <span data-ttu-id="46633-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="46633-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="46633-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="46633-161">These values are not real.</span></span> <span data-ttu-id="46633-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="46633-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="46633-163">Kontakta [Chromeriver supportteamet](https://www.chromeriver.com/services/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="46633-163">Contact [Chromeriver support team](https://www.chromeriver.com/services/support) tooget these values.</span></span>
 


4. <span data-ttu-id="46633-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="46633-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_certificate.png) 

5. <span data-ttu-id="46633-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="46633-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="46633-168">tooconfigure enkel inloggning på **Chromeriver** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Chromeriver supportteamet](https://www.chromeriver.com/services/support).</span><span class="sxs-lookup"><span data-stu-id="46633-168">tooconfigure single sign-on on **Chromeriver** side, you need toosend hello downloaded **Metadata XML** too[Chromeriver support team](https://www.chromeriver.com/services/support).</span></span> <span data-ttu-id="46633-169">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="46633-169">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="46633-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="46633-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="46633-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="46633-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="46633-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="46633-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="46633-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="46633-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="46633-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="46633-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="46633-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46633-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="46633-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="46633-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="46633-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="46633-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="46633-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46633-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="46633-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="46633-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="46633-185">a.</span><span class="sxs-lookup"><span data-stu-id="46633-185">a.</span></span> <span data-ttu-id="46633-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="46633-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="46633-187">b.</span><span class="sxs-lookup"><span data-stu-id="46633-187">b.</span></span> <span data-ttu-id="46633-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="46633-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="46633-189">c.</span><span class="sxs-lookup"><span data-stu-id="46633-189">c.</span></span> <span data-ttu-id="46633-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="46633-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="46633-191">d.</span><span class="sxs-lookup"><span data-stu-id="46633-191">d.</span></span> <span data-ttu-id="46633-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="46633-192">Click **Create**.</span></span>
 
### <a name="creating-a-chromeriver-test-user"></a><span data-ttu-id="46633-193">Skapa en testanvändare Chromeriver</span><span class="sxs-lookup"><span data-stu-id="46633-193">Creating a Chromeriver test user</span></span>

<span data-ttu-id="46633-194">tooenable Azure AD-användare toolog i tooChromeriver, måste de etableras i Chromeriver.</span><span class="sxs-lookup"><span data-stu-id="46633-194">tooenable Azure AD users toolog in tooChromeriver, they must be provisioned into Chromeriver.</span></span>  

<span data-ttu-id="46633-195">Hello gäller Chromeriver, hello användarkonton måste toobe som skapats av din [Chromeriver supportteamet](https://www.chromeriver.com/services/support).</span><span class="sxs-lookup"><span data-stu-id="46633-195">In hello case of Chromeriver, hello user accounts need toobe created by your [Chromeriver support team](https://www.chromeriver.com/services/support).</span></span>

>[!NOTE]
><span data-ttu-id="46633-196">Du kan använda något annat Chromeriver användarens konto skapas verktyg eller API: er som tillhandahålls av Chromeriver tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="46633-196">You can use any other Chromeriver user account creation tools or APIs provided by Chromeriver tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="46633-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="46633-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="46633-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooChromeriver.</span><span class="sxs-lookup"><span data-stu-id="46633-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooChromeriver.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="46633-200">**tooassign Britta Simon tooChromeriver utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="46633-200">**tooassign Britta Simon tooChromeriver, perform hello following steps:**</span></span>

1. <span data-ttu-id="46633-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="46633-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="46633-203">Välj i listan med program hello **Chromeriver**.</span><span class="sxs-lookup"><span data-stu-id="46633-203">In hello applications list, select **Chromeriver**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_app.png) 

3. <span data-ttu-id="46633-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="46633-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="46633-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="46633-207">Click **Add** button.</span></span> <span data-ttu-id="46633-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46633-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="46633-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="46633-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="46633-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46633-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="46633-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="46633-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="46633-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="46633-213">Testing single sign-on</span></span>

<span data-ttu-id="46633-214">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="46633-214">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="46633-215">Du bör få automatiskt inloggade tooyour Chromeriver programmet när du klickar på hello Chromeriver panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="46633-215">When you click hello Chromeriver tile in hello Access Panel, you should get automatically signed-on tooyour Chromeriver application.</span></span> <span data-ttu-id="46633-216">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="46633-216">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="46633-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="46633-217">Additional resources</span></span>

* [<span data-ttu-id="46633-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="46633-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="46633-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="46633-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_203.png

