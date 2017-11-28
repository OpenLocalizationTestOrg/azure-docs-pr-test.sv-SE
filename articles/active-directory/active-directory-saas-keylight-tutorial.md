---
title: "Självstudier: Azure Active Directory-integrering med LockPath Keylight | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LockPath Keylight."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 5485aeb068ba6fbdb4ea9bfc89d401e00c5b1d29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lockpath-keylight"></a><span data-ttu-id="ca38e-103">Självstudier: Azure Active Directory-integrering med LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="ca38e-103">Tutorial: Azure Active Directory integration with LockPath Keylight</span></span>

<span data-ttu-id="ca38e-104">I kursen får du lära dig hur toointegrate LockPath Keylight med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ca38e-104">In this tutorial, you learn how toointegrate LockPath Keylight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ca38e-105">Integrera LockPath Keylight med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ca38e-105">Integrating LockPath Keylight with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ca38e-106">Du kan styra i Azure AD som har åtkomst tooLockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="ca38e-106">You can control in Azure AD who has access tooLockPath Keylight</span></span>
- <span data-ttu-id="ca38e-107">Du kan aktivera din användare tooautomatically get inloggade tooLockPath Keylight (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ca38e-107">You can enable your users tooautomatically get signed-on tooLockPath Keylight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ca38e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ca38e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ca38e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ca38e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca38e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ca38e-110">Prerequisites</span></span>

<span data-ttu-id="ca38e-111">tooconfigure Azure AD-integrering med LockPath Keylight, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ca38e-111">tooconfigure Azure AD integration with LockPath Keylight, you need hello following items:</span></span>

- <span data-ttu-id="ca38e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ca38e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ca38e-113">En LockPath Keylight enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ca38e-113">A LockPath Keylight single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ca38e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ca38e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ca38e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ca38e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ca38e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ca38e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ca38e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca38e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ca38e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ca38e-118">Scenario description</span></span>
<span data-ttu-id="ca38e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ca38e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ca38e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ca38e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ca38e-121">Att lägga till LockPath Keylight från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ca38e-121">Adding LockPath Keylight from hello gallery</span></span>
2. <span data-ttu-id="ca38e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca38e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lockpath-keylight-from-hello-gallery"></a><span data-ttu-id="ca38e-123">Att lägga till LockPath Keylight från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ca38e-123">Adding LockPath Keylight from hello gallery</span></span>
<span data-ttu-id="ca38e-124">tooconfigure hello integrering av LockPath Keylight i Azure AD, behöver du tooadd LockPath Keylight hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ca38e-124">tooconfigure hello integration of LockPath Keylight into Azure AD, you need tooadd LockPath Keylight from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ca38e-125">**tooadd LockPath Keylight från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ca38e-125">**tooadd LockPath Keylight from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca38e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ca38e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ca38e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ca38e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ca38e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca38e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ca38e-133">Skriv i sökrutan hello **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-133">In hello search box, type **LockPath Keylight**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_search.png)

5. <span data-ttu-id="ca38e-135">Markera hello resultat på panelen **LockPath Keylight**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ca38e-135">In hello results panel, select **LockPath Keylight**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ca38e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca38e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ca38e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LockPath Keylight baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ca38e-138">In this section, you configure and test Azure AD single sign-on with LockPath Keylight based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ca38e-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LockPath Keylight är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ca38e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LockPath Keylight is tooa user in Azure AD.</span></span> <span data-ttu-id="ca38e-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LockPath Keylight toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ca38e-140">In other words, a link relationship between an Azure AD user and hello related user in LockPath Keylight needs toobe established.</span></span>

<span data-ttu-id="ca38e-141">Tilldela hello värdet för hello i LockPath Keylight **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ca38e-141">In LockPath Keylight, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ca38e-142">tooconfigure och testa Azure AD enkel inloggning med LockPath Keylight, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ca38e-142">tooconfigure and test Azure AD single sign-on with LockPath Keylight, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ca38e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ca38e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ca38e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca38e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ca38e-145">**[Skapa en testanvändare LockPath Keylight](#creating-a-lockpath-keylight-test-user)**  -toohave en motsvarighet för Britta Simon i LockPath Keylight som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ca38e-145">**[Creating a LockPath Keylight test user](#creating-a-lockpath-keylight-test-user)** - toohave a counterpart of Britta Simon in LockPath Keylight that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ca38e-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca38e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ca38e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ca38e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ca38e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca38e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ca38e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="ca38e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LockPath Keylight application.</span></span>

<span data-ttu-id="ca38e-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LockPath Keylight:**</span><span class="sxs-lookup"><span data-stu-id="ca38e-150">**tooconfigure Azure AD single sign-on with LockPath Keylight, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca38e-151">I hello Azure-portalen på hello **LockPath Keylight** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-151">In hello Azure portal, on hello **LockPath Keylight** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ca38e-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca38e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_samlbase.png)

3. <span data-ttu-id="ca38e-155">På hello **LockPath Keylight domän och URL: er** avsnittet, utför följande steg hello::</span><span class="sxs-lookup"><span data-stu-id="ca38e-155">On hello **LockPath Keylight Domain and URLs** section, perform hello following steps::</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_url.png)

    <span data-ttu-id="ca38e-157">a.</span><span class="sxs-lookup"><span data-stu-id="ca38e-157">a.</span></span> <span data-ttu-id="ca38e-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.keylightgrc.com/`</span><span class="sxs-lookup"><span data-stu-id="ca38e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com/`</span></span>

    <span data-ttu-id="ca38e-159">b.</span><span class="sxs-lookup"><span data-stu-id="ca38e-159">b.</span></span> <span data-ttu-id="ca38e-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.keylightgrc.com`</span><span class="sxs-lookup"><span data-stu-id="ca38e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com`</span></span>

    <span data-ttu-id="ca38e-161">c.</span><span class="sxs-lookup"><span data-stu-id="ca38e-161">c.</span></span> <span data-ttu-id="ca38e-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.keylightgrc.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="ca38e-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.keylightgrc.com/Login.aspx`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="ca38e-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ca38e-163">These values are not real.</span></span> <span data-ttu-id="ca38e-164">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ca38e-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="ca38e-165">Kontakta [LockPath Keylight klienten supportteamet](https://www.lockpath.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ca38e-165">Contact [LockPath Keylight Client support team](https://www.lockpath.com/contact/) tooget these values.</span></span> 

4. <span data-ttu-id="ca38e-166">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ca38e-166">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_certificate.png) 

5. <span data-ttu-id="ca38e-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca38e-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="ca38e-170">På hello **LockPath Keylight Configuration** klickar du på **konfigurera LockPath Keylight** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ca38e-170">On hello **LockPath Keylight Configuration** section, click **Configure LockPath Keylight** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ca38e-171">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ca38e-171">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_configure.png) 

7. <span data-ttu-id="ca38e-173">tooenable SSO i LockPath Keylight utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca38e-173">tooenable SSO in LockPath Keylight, perform hello following steps:</span></span>
   
    <span data-ttu-id="ca38e-174">a.</span><span class="sxs-lookup"><span data-stu-id="ca38e-174">a.</span></span> <span data-ttu-id="ca38e-175">Inloggning tooyour LockPath Keylight kontot som administratör.</span><span class="sxs-lookup"><span data-stu-id="ca38e-175">Sign-on tooyour LockPath Keylight account as administrator.</span></span>
    
    <span data-ttu-id="ca38e-176">b.</span><span class="sxs-lookup"><span data-stu-id="ca38e-176">b.</span></span> <span data-ttu-id="ca38e-177">Hello-menyn överst hello **Person**, och välj **Keylight installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-177">In hello menu on hello top, click **Person**, and select **Keylight Setup**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/401.png) 

    <span data-ttu-id="ca38e-179">c.</span><span class="sxs-lookup"><span data-stu-id="ca38e-179">c.</span></span> <span data-ttu-id="ca38e-180">I hello treeview hello vänster, klickar du på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-180">In hello treeview on hello left, click **SAML**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/402.png) 

    <span data-ttu-id="ca38e-182">d.</span><span class="sxs-lookup"><span data-stu-id="ca38e-182">d.</span></span> <span data-ttu-id="ca38e-183">På hello **SAML inställningar** dialogrutan klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-183">On hello **SAML Settings** dialog, click **Edit**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/404.png) 

8. <span data-ttu-id="ca38e-185">På hello **redigera inställningar för SAML** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca38e-185">On hello **Edit SAML Settings** dialog page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    <span data-ttu-id="ca38e-187">a.</span><span class="sxs-lookup"><span data-stu-id="ca38e-187">a.</span></span> <span data-ttu-id="ca38e-188">Ange **SAML-autentisering** för**Active**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-188">Set **SAML authentication** too**Active**.</span></span>

    <span data-ttu-id="ca38e-189">b.</span><span class="sxs-lookup"><span data-stu-id="ca38e-189">b.</span></span> <span data-ttu-id="ca38e-190">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värde som du har kopierat från hello Azure-portalen i hello **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="ca38e-190">Paste hello **SAML Single Sign-On Service URL** value which you have copied from hello Azure portal into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="ca38e-191">c.</span><span class="sxs-lookup"><span data-stu-id="ca38e-191">c.</span></span> <span data-ttu-id="ca38e-192">Klistra in hello **tjänst-URL för enkel Sign-Out** värde som du har kopierat från hello Azure-portalen i hello **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="ca38e-192">Paste hello **Single Sign-Out Service URL** value which you have copied from hello Azure portal into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="ca38e-193">d.</span><span class="sxs-lookup"><span data-stu-id="ca38e-193">d.</span></span> <span data-ttu-id="ca38e-194">Klicka på **Välj fil** tooselect din hämtade LockPath Keylight certifikat, och klicka sedan på **öppna** tooupload hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="ca38e-194">Click **Choose File** tooselect your downloaded LockPath Keylight certificate, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="ca38e-195">e.</span><span class="sxs-lookup"><span data-stu-id="ca38e-195">e.</span></span> <span data-ttu-id="ca38e-196">Ange **användar-Id för SAML plats** för**NameIdentifier element av hello ämne instruktionen**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-196">Set **SAML User Id location** too**NameIdentifier element of hello subject statement**.</span></span>
    
    <span data-ttu-id="ca38e-197">f.</span><span class="sxs-lookup"><span data-stu-id="ca38e-197">f.</span></span> <span data-ttu-id="ca38e-198">Ange hello **Keylight Service Provider** med hello följer mönstret: **https://&lt;CompanyName&gt;. keylightgrc.com**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-198">Provide hello **Keylight Service Provider** using hello following pattern: **https://&lt;CompanyName&gt;.keylightgrc.com**.</span></span>
    
    <span data-ttu-id="ca38e-199">g.</span><span class="sxs-lookup"><span data-stu-id="ca38e-199">g.</span></span> <span data-ttu-id="ca38e-200">Ange **automatiskt etablera användare** för**Active**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-200">Set **Auto-provision users** too**Active**.</span></span>

    <span data-ttu-id="ca38e-201">h.</span><span class="sxs-lookup"><span data-stu-id="ca38e-201">h.</span></span> <span data-ttu-id="ca38e-202">Ange **automatiskt etablera kontotyp** för**fullständig användaren**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-202">Set **Auto-provision account type** too**Full User**.</span></span>

    <span data-ttu-id="ca38e-203">Jag.</span><span class="sxs-lookup"><span data-stu-id="ca38e-203">i.</span></span> <span data-ttu-id="ca38e-204">Ange **automatiskt etablera säkerhetsrollen**väljer **standardanvändare med SAML**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-204">Set **Auto-provision security role**, select **Standard User with SAML**.</span></span>
    
    <span data-ttu-id="ca38e-205">j.</span><span class="sxs-lookup"><span data-stu-id="ca38e-205">j.</span></span> <span data-ttu-id="ca38e-206">Ange **automatiskt etablera säkerhet config**väljer **Standard Användarkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-206">Set **Auto-provision security config**, select **Standard User Configuration**.</span></span>
     
    <span data-ttu-id="ca38e-207">k.</span><span class="sxs-lookup"><span data-stu-id="ca38e-207">k.</span></span> <span data-ttu-id="ca38e-208">I hello **e-attributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="ca38e-208">In hello **Email attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="ca38e-209">l.</span><span class="sxs-lookup"><span data-stu-id="ca38e-209">l.</span></span> <span data-ttu-id="ca38e-210">I hello **förnamn attributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="ca38e-210">In hello **First name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="ca38e-211">m.</span><span class="sxs-lookup"><span data-stu-id="ca38e-211">m.</span></span> <span data-ttu-id="ca38e-212">I hello **senaste namnattributet** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="ca38e-212">In hello **Last name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
    
    <span data-ttu-id="ca38e-213">n.</span><span class="sxs-lookup"><span data-stu-id="ca38e-213">n.</span></span> <span data-ttu-id="ca38e-214">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-214">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="ca38e-215">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ca38e-215">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ca38e-216">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ca38e-216">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ca38e-217">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ca38e-217">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ca38e-218">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca38e-218">Creating an Azure AD test user</span></span>
<span data-ttu-id="ca38e-219">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca38e-219">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ca38e-221">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ca38e-221">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca38e-222">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ca38e-222">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ca38e-224">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-224">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ca38e-226">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca38e-226">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ca38e-228">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca38e-228">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ca38e-230">a.</span><span class="sxs-lookup"><span data-stu-id="ca38e-230">a.</span></span> <span data-ttu-id="ca38e-231">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-231">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ca38e-232">b.</span><span class="sxs-lookup"><span data-stu-id="ca38e-232">b.</span></span> <span data-ttu-id="ca38e-233">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ca38e-233">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ca38e-234">c.</span><span class="sxs-lookup"><span data-stu-id="ca38e-234">c.</span></span> <span data-ttu-id="ca38e-235">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-235">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ca38e-236">d.</span><span class="sxs-lookup"><span data-stu-id="ca38e-236">d.</span></span> <span data-ttu-id="ca38e-237">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-237">Click **Create**.</span></span>
 
### <a name="creating-a-lockpath-keylight-test-user"></a><span data-ttu-id="ca38e-238">Skapa en testanvändare LockPath Keylight</span><span class="sxs-lookup"><span data-stu-id="ca38e-238">Creating a LockPath Keylight test user</span></span>

<span data-ttu-id="ca38e-239">I det här avsnittet skapar du en användare som kallas Britta Simon i LockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="ca38e-239">In this section, you create a user called Britta Simon in LockPath Keylight.</span></span> <span data-ttu-id="ca38e-240">LockPath Keylight stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="ca38e-240">LockPath Keylight supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="ca38e-241">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ca38e-241">There is no action item for you in this section.</span></span> <span data-ttu-id="ca38e-242">En ny användare skapas vid åtkomst till LockPath Keylight om hello användaren inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="ca38e-242">A new user is created when accessing LockPath Keylight if hello user doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="ca38e-243">Om du behöver toocreate en användare manuellt, måste toocontact hello [LockPath Keylight klienten supportteamet](https://www.lockpath.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="ca38e-243">If you need toocreate a user manually, you need toocontact hello [LockPath Keylight Client support team](https://www.lockpath.com/contact/).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="ca38e-244">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca38e-244">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="ca38e-245">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLockPath Keylight.</span><span class="sxs-lookup"><span data-stu-id="ca38e-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLockPath Keylight.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ca38e-247">**tooassign Britta Simon tooLockPath Keylight, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="ca38e-247">**tooassign Britta Simon tooLockPath Keylight, perform hello following steps:**</span></span>

1. <span data-ttu-id="ca38e-248">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ca38e-250">Välj i listan med program hello **LockPath Keylight**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-250">In hello applications list, select **LockPath Keylight**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_app.png) 

3. <span data-ttu-id="ca38e-252">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ca38e-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ca38e-254">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca38e-254">Click **Add** button.</span></span> <span data-ttu-id="ca38e-255">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca38e-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ca38e-257">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ca38e-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ca38e-258">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca38e-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ca38e-259">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca38e-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ca38e-260">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca38e-260">Testing single sign-on</span></span>

<span data-ttu-id="ca38e-261">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ca38e-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ca38e-262">Du bör få automatiskt inloggade tooyour LockPath Keylight programmet när du klickar på hello LockPath Keylight panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ca38e-262">When you click hello LockPath Keylight tile in hello Access Panel, you should get automatically signed-on tooyour LockPath Keylight application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ca38e-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ca38e-263">Additional resources</span></span>

* [<span data-ttu-id="ca38e-264">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ca38e-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ca38e-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ca38e-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png

