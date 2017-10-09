---
title: "Självstudier: Azure Active Directory-integrering med AppBlade | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och AppBlade."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 06f3d8fcee97945c867bca6f3aebe15ecef04617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a><span data-ttu-id="e84ee-103">Självstudier: Azure Active Directory-integrering med AppBlade</span><span class="sxs-lookup"><span data-stu-id="e84ee-103">Tutorial: Azure Active Directory integration with AppBlade</span></span>

<span data-ttu-id="e84ee-104">I kursen får du lära dig hur toointegrate AppBlade med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e84ee-104">In this tutorial, you learn how toointegrate AppBlade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e84ee-105">Integrera AppBlade med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e84ee-105">Integrating AppBlade with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e84ee-106">Du kan styra i Azure AD som har åtkomst till tooAppBlade</span><span class="sxs-lookup"><span data-stu-id="e84ee-106">You can control in Azure AD who has access tooAppBlade</span></span>
- <span data-ttu-id="e84ee-107">Du kan aktivera din användare tooautomatically get inloggade tooAppBlade (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e84ee-107">You can enable your users tooautomatically get signed-on tooAppBlade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e84ee-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e84ee-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e84ee-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e84ee-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e84ee-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e84ee-110">Prerequisites</span></span>

<span data-ttu-id="e84ee-111">tooconfigure Azure AD-integrering med AppBlade, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e84ee-111">tooconfigure Azure AD integration with AppBlade, you need hello following items:</span></span>

- <span data-ttu-id="e84ee-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e84ee-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e84ee-113">En AppBlade enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="e84ee-113">An AppBlade single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e84ee-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e84ee-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e84ee-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e84ee-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e84ee-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e84ee-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e84ee-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e84ee-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e84ee-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e84ee-118">Scenario description</span></span>
<span data-ttu-id="e84ee-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e84ee-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e84ee-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e84ee-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e84ee-121">Att lägga till AppBlade från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e84ee-121">Adding AppBlade from hello gallery</span></span>
2. <span data-ttu-id="e84ee-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e84ee-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appblade-from-hello-gallery"></a><span data-ttu-id="e84ee-123">Att lägga till AppBlade från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e84ee-123">Adding AppBlade from hello gallery</span></span>
<span data-ttu-id="e84ee-124">tooconfigure hello integrering av AppBlade i Azure AD, behöver du tooadd AppBlade hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e84ee-124">tooconfigure hello integration of AppBlade into Azure AD, you need tooadd AppBlade from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e84ee-125">**tooadd AppBlade från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e84ee-125">**tooadd AppBlade from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e84ee-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e84ee-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e84ee-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e84ee-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e84ee-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e84ee-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e84ee-133">Skriv i sökrutan hello **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-133">In hello search box, type **AppBlade**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_search.png)

5. <span data-ttu-id="e84ee-135">Markera hello resultat på panelen **AppBlade**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e84ee-135">In hello results panel, select **AppBlade**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e84ee-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e84ee-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e84ee-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AppBlade baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e84ee-138">In this section, you configure and test Azure AD single sign-on with AppBlade based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e84ee-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i AppBlade är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e84ee-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AppBlade is tooa user in Azure AD.</span></span> <span data-ttu-id="e84ee-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i AppBlade toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e84ee-140">In other words, a link relationship between an Azure AD user and hello related user in AppBlade needs toobe established.</span></span>

<span data-ttu-id="e84ee-141">I AppBlade, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e84ee-141">In AppBlade, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e84ee-142">tooconfigure och testa Azure AD enkel inloggning med AppBlade, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e84ee-142">tooconfigure and test Azure AD single sign-on with AppBlade, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e84ee-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e84ee-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e84ee-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e84ee-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e84ee-145">**[Skapa en testanvändare AppBlade](#creating-an-appblade-test-user)**  -toohave en motsvarighet för Britta Simon i AppBlade som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e84ee-145">**[Creating an AppBlade test user](#creating-an-appblade-test-user)** - toohave a counterpart of Britta Simon in AppBlade that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e84ee-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e84ee-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e84ee-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e84ee-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e84ee-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e84ee-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e84ee-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt AppBlade program.</span><span class="sxs-lookup"><span data-stu-id="e84ee-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AppBlade application.</span></span>

<span data-ttu-id="e84ee-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med AppBlade:**</span><span class="sxs-lookup"><span data-stu-id="e84ee-150">**tooconfigure Azure AD single sign-on with AppBlade, perform hello following steps:**</span></span>

1. <span data-ttu-id="e84ee-151">I hello Azure-portalen på hello **AppBlade** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-151">In hello Azure portal, on hello **AppBlade** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e84ee-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e84ee-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_samlbase.png)

3. <span data-ttu-id="e84ee-155">På hello **AppBlade domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e84ee-155">On hello **AppBlade Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_url.png)

    <span data-ttu-id="e84ee-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.appblade.com/saml/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="e84ee-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.appblade.com/saml/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e84ee-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e84ee-158">This value is not real.</span></span> <span data-ttu-id="e84ee-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="e84ee-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="e84ee-160">Kontakta [AppBlade klienten supportteamet](mailto:support@appblade.com) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="e84ee-160">Contact [AppBlade Client support team](mailto:support@appblade.com) tooget hello value.</span></span> 
 
4. <span data-ttu-id="e84ee-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="e84ee-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_certificate.png) 

5. <span data-ttu-id="e84ee-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e84ee-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e84ee-165">tooconfigure enkel inloggning på **AppBlade** sida, behöver du toosend hello hämtas **XML-Metadata för** för[AppBlade supportteamet](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="e84ee-165">tooconfigure single sign-on on **AppBlade** side, you need toosend hello downloaded **Metadata XML** too[AppBlade support team](mailto:support@appblade.com).</span></span> <span data-ttu-id="e84ee-166">Dessutom be dem tooconfigure hello **utfärdar-URL för SSO** som `https://appblade.com/saml`.</span><span class="sxs-lookup"><span data-stu-id="e84ee-166">Also, please ask them tooconfigure hello **SSO Issuer URL** as `https://appblade.com/saml`.</span></span> <span data-ttu-id="e84ee-167">Den här inställningen krävs för toowork för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e84ee-167">This setting is required for single sign-on toowork.</span></span>


> [!TIP]
> <span data-ttu-id="e84ee-168">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="e84ee-168">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e84ee-169">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="e84ee-169">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e84ee-170">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e84ee-170">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e84ee-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e84ee-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="e84ee-172">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e84ee-172">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e84ee-174">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e84ee-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e84ee-175">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e84ee-175">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e84ee-177">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-177">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e84ee-179">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e84ee-179">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e84ee-181">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e84ee-181">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e84ee-183">a.</span><span class="sxs-lookup"><span data-stu-id="e84ee-183">a.</span></span> <span data-ttu-id="e84ee-184">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-184">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e84ee-185">b.</span><span class="sxs-lookup"><span data-stu-id="e84ee-185">b.</span></span> <span data-ttu-id="e84ee-186">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e84ee-186">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e84ee-187">c.</span><span class="sxs-lookup"><span data-stu-id="e84ee-187">c.</span></span> <span data-ttu-id="e84ee-188">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-188">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e84ee-189">d.</span><span class="sxs-lookup"><span data-stu-id="e84ee-189">d.</span></span> <span data-ttu-id="e84ee-190">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-190">Click **Create**.</span></span>
 
### <a name="creating-an-appblade-test-user"></a><span data-ttu-id="e84ee-191">Skapa en testanvändare AppBlade</span><span class="sxs-lookup"><span data-stu-id="e84ee-191">Creating an AppBlade test user</span></span>

<span data-ttu-id="e84ee-192">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i AppBlade.</span><span class="sxs-lookup"><span data-stu-id="e84ee-192">hello objective of this section is toocreate a user called Britta Simon in AppBlade.</span></span> <span data-ttu-id="e84ee-193">AppBlade stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="e84ee-193">AppBlade supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="e84ee-194">**Kontrollera att domännamnet är konfigurerad med AppBlade för användaretablering. Efter att endast hello-in-time användaretablering fungerar.**</span><span class="sxs-lookup"><span data-stu-id="e84ee-194">**Make sure that your domain name is configured with AppBlade for user provisioning. After that only hello just-in-time user provisioning works.**</span></span>

<span data-ttu-id="e84ee-195">Om hello användare har en e-postadress som slutar med hello-domän som konfigurerats av AppBlade för ditt konto och sedan hello användaren kommer automatiskt att ansluta hello kontot som en medlem med hello behörigheten som du anger, vilket är en ”Basic” (en grundläggande användare kan bara installera program), ”” (en användare kan ladda upp nya versioner av appen och hantera projekt) eller liknande ”administratör” (fullständig behörighet toohello administratörskonto).</span><span class="sxs-lookup"><span data-stu-id="e84ee-195">If hello user has an email address ending with hello domain configured by AppBlade for your account, then hello user will automatically join hello account as a member with hello permission level you specify, which is one of "Basic" (a basic user who can only install applications), "Team Member" (a user who can upload new app versions and manage projects), or "Administrator" (full admin privileges toohello account).</span></span> <span data-ttu-id="e84ee-196">Normalt en skulle välja Basic och främja användare manuellt via en inloggning för serveradministratör (AppBlade måste tooconfigure antingen en e-postbaserad administratörsinloggning i förväg eller befordra en användare för hello kunds räkning efter inloggningen).</span><span class="sxs-lookup"><span data-stu-id="e84ee-196">Normally one would choose Basic and then promote users manually via an Admin login (AppBlade needs tooconfigure either an email-based admin login in advance or promote a user on behalf of hello customer after login).</span></span>

<span data-ttu-id="e84ee-197">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e84ee-197">There is no action item for you in this section.</span></span> <span data-ttu-id="e84ee-198">En ny användare skapas under ett försök tooaccess AppBlade om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="e84ee-198">A new user is created during an attempt tooaccess AppBlade if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="e84ee-199">Om du behöver toocreate en användare manuellt, måste toocontact hello [AppBlade supportteamet](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="e84ee-199">If you need toocreate a user manually, you need toocontact hello [AppBlade support team](mailto:support@appblade.com).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e84ee-200">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e84ee-200">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e84ee-201">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAppBlade.</span><span class="sxs-lookup"><span data-stu-id="e84ee-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAppBlade.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e84ee-203">**tooassign Britta Simon tooAppBlade utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e84ee-203">**tooassign Britta Simon tooAppBlade, perform hello following steps:**</span></span>

1. <span data-ttu-id="e84ee-204">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e84ee-206">Välj i listan med program hello **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-206">In hello applications list, select **AppBlade**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_app.png) 

3. <span data-ttu-id="e84ee-208">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e84ee-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e84ee-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e84ee-210">Click **Add** button.</span></span> <span data-ttu-id="e84ee-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e84ee-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e84ee-213">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e84ee-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e84ee-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e84ee-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e84ee-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e84ee-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e84ee-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e84ee-216">Testing single sign-on</span></span>

<span data-ttu-id="e84ee-217">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e84ee-217">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="e84ee-218">Du bör få automatiskt inloggade tooyour AppBlade programmet när du klickar på hello AppBlade panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e84ee-218">When you click hello AppBlade tile in hello Access Panel, you should get automatically signed-on tooyour AppBlade application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e84ee-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e84ee-219">Additional resources</span></span>

* [<span data-ttu-id="e84ee-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e84ee-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e84ee-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e84ee-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_203.png

