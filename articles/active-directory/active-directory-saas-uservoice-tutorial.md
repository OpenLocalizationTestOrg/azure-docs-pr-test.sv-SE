---
title: "Självstudier: Azure Active Directory-integrering med UserVoice | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och UserVoice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 9eade8435ae6c6a3821bbbec9ab7c27ed7ad91ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a><span data-ttu-id="21483-103">Självstudier: Azure Active Directory-integrering med UserVoice</span><span class="sxs-lookup"><span data-stu-id="21483-103">Tutorial: Azure Active Directory integration with UserVoice</span></span>

<span data-ttu-id="21483-104">I kursen får du lära dig hur toointegrate UserVoice med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="21483-104">In this tutorial, you learn how toointegrate UserVoice with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="21483-105">Integrera UserVoice med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="21483-105">Integrating UserVoice with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="21483-106">Du kan styra i Azure AD som har åtkomst till tooUserVoice.</span><span class="sxs-lookup"><span data-stu-id="21483-106">You can control in Azure AD who has access tooUserVoice.</span></span>
- <span data-ttu-id="21483-107">Du kan låta dina användare tooautomatically get inloggade tooUserVoice (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="21483-107">You can enable your users tooautomatically get signed-on tooUserVoice (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="21483-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="21483-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="21483-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="21483-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21483-110">Krav</span><span class="sxs-lookup"><span data-stu-id="21483-110">Prerequisites</span></span>

<span data-ttu-id="21483-111">tooconfigure Azure AD-integrering med UserVoice, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="21483-111">tooconfigure Azure AD integration with UserVoice, you need hello following items:</span></span>

- <span data-ttu-id="21483-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="21483-112">An Azure AD subscription</span></span>
- <span data-ttu-id="21483-113">En UserVoice enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="21483-113">A UserVoice single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="21483-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="21483-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="21483-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="21483-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="21483-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="21483-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="21483-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="21483-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="21483-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="21483-118">Scenario description</span></span>
<span data-ttu-id="21483-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="21483-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="21483-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="21483-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="21483-121">Att lägga till UserVoice från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="21483-121">Adding UserVoice from hello gallery</span></span>
2. <span data-ttu-id="21483-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21483-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-uservoice-from-hello-gallery"></a><span data-ttu-id="21483-123">Att lägga till UserVoice från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="21483-123">Adding UserVoice from hello gallery</span></span>
<span data-ttu-id="21483-124">tooconfigure hello integrering av UserVoice i Azure AD, behöver du tooadd UserVoice hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="21483-124">tooconfigure hello integration of UserVoice into Azure AD, you need tooadd UserVoice from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="21483-125">**tooadd UserVoice från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="21483-125">**tooadd UserVoice from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="21483-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="21483-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="21483-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="21483-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="21483-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="21483-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="21483-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21483-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="21483-133">Skriv i sökrutan hello **UserVoice**väljer **UserVoice** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="21483-133">In hello search box, type **UserVoice**, select **UserVoice** from result panel then click **Add** button tooadd hello application.</span></span>

    ![UserVoice i hello resultatlistan](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="21483-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21483-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="21483-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UserVoice baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="21483-136">In this section, you configure and test Azure AD single sign-on with UserVoice based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="21483-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i UserVoice är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="21483-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UserVoice is tooa user in Azure AD.</span></span> <span data-ttu-id="21483-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i UserVoice toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="21483-138">In other words, a link relationship between an Azure AD user and hello related user in UserVoice needs toobe established.</span></span>

<span data-ttu-id="21483-139">I UserVoice, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="21483-139">In UserVoice, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="21483-140">tooconfigure och testa Azure AD enkel inloggning med UserVoice, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="21483-140">tooconfigure and test Azure AD single sign-on with UserVoice, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="21483-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="21483-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="21483-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="21483-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="21483-143">**[Skapa en testanvändare UserVoice](#create-a-uservoice-test-user)**  -toohave en motsvarighet för Britta Simon i UserVoice som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="21483-143">**[Create a UserVoice test user](#create-a-uservoice-test-user)** - toohave a counterpart of Britta Simon in UserVoice that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="21483-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="21483-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="21483-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="21483-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="21483-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21483-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="21483-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt UserVoice-program.</span><span class="sxs-lookup"><span data-stu-id="21483-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UserVoice application.</span></span>

<span data-ttu-id="21483-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med UserVoice:**</span><span class="sxs-lookup"><span data-stu-id="21483-148">**tooconfigure Azure AD single sign-on with UserVoice, perform hello following steps:**</span></span>

1. <span data-ttu-id="21483-149">I hello Azure-portalen på hello **UserVoice** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="21483-149">In hello Azure portal, on hello **UserVoice** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="21483-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="21483-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_samlbase.png)

3. <span data-ttu-id="21483-153">På hello **UserVoice domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21483-153">On hello **UserVoice Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och UserVoice domän med enkel inloggning information](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_url.png)

    <span data-ttu-id="21483-155">a.</span><span class="sxs-lookup"><span data-stu-id="21483-155">a.</span></span> <span data-ttu-id="21483-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="21483-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    <span data-ttu-id="21483-157">b.</span><span class="sxs-lookup"><span data-stu-id="21483-157">b.</span></span> <span data-ttu-id="21483-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.UserVoice.com`</span><span class="sxs-lookup"><span data-stu-id="21483-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenantname>.UserVoice.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="21483-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="21483-159">These values are not real.</span></span> <span data-ttu-id="21483-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="21483-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="21483-161">Kontakta [UserVoice klienten supportteamet](https://www.uservoice.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="21483-161">Contact [UserVoice Client support team](https://www.uservoice.com/) tooget these values.</span></span>

4. <span data-ttu-id="21483-162">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="21483-162">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_certificate.png) 

5. <span data-ttu-id="21483-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="21483-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-uservoice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="21483-166">På hello **UserVoice Configuration** klickar du på **konfigurera UserVoice** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="21483-166">On hello **UserVoice Configuration** section, click **Configure UserVoice** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="21483-167">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="21483-167">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![UserVoice-konfiguration](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_configure.png) 

7. <span data-ttu-id="21483-169">Logga in tooyour UserVoice-webbplatsen för företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="21483-169">In a different web browser window, log in tooyour UserVoice company site as an administrator.</span></span>

8. <span data-ttu-id="21483-170">Klicka i hello verktygsfältet hello längst upp **inställningar**, och välj sedan **webbportalen** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="21483-170">In hello toolbar on hello top, click **Settings**, and then select **Web portal** from hello menu.</span></span>
   
    <span data-ttu-id="21483-171">![Inställningar på sidan App](./media/active-directory-saas-uservoice-tutorial/ic777519.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="21483-171">![Settings Section On App Side](./media/active-directory-saas-uservoice-tutorial/ic777519.png "Settings")</span></span>

9. <span data-ttu-id="21483-172">På hello **webbportalen** på fliken hello **användarautentisering** klickar du på **redigera** tooopen hello **redigera användarautentisering** dialogrutan sidan.</span><span class="sxs-lookup"><span data-stu-id="21483-172">On hello **Web portal** tab, in hello **User authentication** section, click **Edit** tooopen hello **Edit User Authentication** dialog page.</span></span>
   
    <span data-ttu-id="21483-173">![Webbportalen fliken](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web-portalen")</span><span class="sxs-lookup"><span data-stu-id="21483-173">![Web portal Tab](./media/active-directory-saas-uservoice-tutorial/ic777520.png "Web portal")</span></span>

10. <span data-ttu-id="21483-174">På hello **redigera användarautentisering** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21483-174">On hello **Edit User Authentication** dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="21483-175">![Redigera användarautentisering](./media/active-directory-saas-uservoice-tutorial/ic777521.png "redigera användarautentisering")</span><span class="sxs-lookup"><span data-stu-id="21483-175">![Edit user authentication](./media/active-directory-saas-uservoice-tutorial/ic777521.png "Edit user authentication")</span></span>
   
    <span data-ttu-id="21483-176">a.</span><span class="sxs-lookup"><span data-stu-id="21483-176">a.</span></span> <span data-ttu-id="21483-177">Klicka på **enkel inloggning (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="21483-177">Click **Single Sign-On (SSO)**.</span></span>
 
    <span data-ttu-id="21483-178">b.</span><span class="sxs-lookup"><span data-stu-id="21483-178">b.</span></span> <span data-ttu-id="21483-179">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **SSO Remote inloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="21483-179">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **SSO Remote Sign-In** textbox.</span></span>

    <span data-ttu-id="21483-180">c.</span><span class="sxs-lookup"><span data-stu-id="21483-180">c.</span></span> <span data-ttu-id="21483-181">Klistra in hello **Sign-Out URL** -värde som du har kopierat från hello Azure-portalen i hello **SSO Remote Sign-Out textruta**.</span><span class="sxs-lookup"><span data-stu-id="21483-181">Paste hello **Sign-Out URL** value, which you have copied from hello Azure portal into hello **SSO Remote Sign-Out textbox**.</span></span>
 
    <span data-ttu-id="21483-182">d.</span><span class="sxs-lookup"><span data-stu-id="21483-182">d.</span></span> <span data-ttu-id="21483-183">Klistra in hello **tumavtrycket** -värde som du har kopierat från Azure-portalen i den **aktuella certifikat SHA1 fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="21483-183">Paste hello **Thumbprint** value , which you have copied from Azure portal  into the **Current certificate SHA1 fingerprint** textbox.</span></span>
    
    <span data-ttu-id="21483-184">e.</span><span class="sxs-lookup"><span data-stu-id="21483-184">e.</span></span> <span data-ttu-id="21483-185">Klicka på **spara autentiseringsinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="21483-185">Click **Save authentication settings**.</span></span>

> [!TIP]
> <span data-ttu-id="21483-186">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="21483-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="21483-187">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="21483-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="21483-188">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="21483-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="21483-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="21483-189">Create an Azure AD test user</span></span>

<span data-ttu-id="21483-190">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="21483-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="21483-192">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="21483-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="21483-193">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="21483-193">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-uservoice-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="21483-195">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="21483-195">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-uservoice-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="21483-197">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21483-197">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-uservoice-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="21483-199">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21483-199">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-uservoice-tutorial/create_aaduser_04.png)

    <span data-ttu-id="21483-201">a.</span><span class="sxs-lookup"><span data-stu-id="21483-201">a.</span></span> <span data-ttu-id="21483-202">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="21483-202">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="21483-203">b.</span><span class="sxs-lookup"><span data-stu-id="21483-203">b.</span></span> <span data-ttu-id="21483-204">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="21483-204">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="21483-205">c.</span><span class="sxs-lookup"><span data-stu-id="21483-205">c.</span></span> <span data-ttu-id="21483-206">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="21483-206">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="21483-207">d.</span><span class="sxs-lookup"><span data-stu-id="21483-207">d.</span></span> <span data-ttu-id="21483-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="21483-208">Click **Create**.</span></span>
 
### <a name="create-a-uservoice-test-user"></a><span data-ttu-id="21483-209">Skapa en testanvändare UserVoice</span><span class="sxs-lookup"><span data-stu-id="21483-209">Create a UserVoice test user</span></span>

<span data-ttu-id="21483-210">tooenable Azure AD-användare toolog i tooUserVoice, måste de etableras till UserVoice.</span><span class="sxs-lookup"><span data-stu-id="21483-210">tooenable Azure AD users toolog in tooUserVoice, they must be provisioned into UserVoice.</span></span> <span data-ttu-id="21483-211">Hello gäller UserVoice är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="21483-211">In hello case of UserVoice, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="21483-212">tooprovision ett användarkonto, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="21483-212">tooprovision a user account, perform hello following steps:</span></span>
1. <span data-ttu-id="21483-213">Logga in tooyour **UserVoice** klient.</span><span class="sxs-lookup"><span data-stu-id="21483-213">Log in tooyour **UserVoice** tenant.</span></span>

2. <span data-ttu-id="21483-214">Gå för**inställningar**.</span><span class="sxs-lookup"><span data-stu-id="21483-214">Go too**Settings**.</span></span>
   
    <span data-ttu-id="21483-215">![Inställningar för](./media/active-directory-saas-uservoice-tutorial/ic777811.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="21483-215">![Settings](./media/active-directory-saas-uservoice-tutorial/ic777811.png "Settings")</span></span>

3. <span data-ttu-id="21483-216">Klicka på **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="21483-216">Click **General**.</span></span>

4. <span data-ttu-id="21483-217">Klicka på **agenter och behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="21483-217">Click **Agents and permissions**.</span></span>
   
    <span data-ttu-id="21483-218">![Agenter och behörigheter](./media/active-directory-saas-uservoice-tutorial/ic777812.png "agenter och behörigheter")</span><span class="sxs-lookup"><span data-stu-id="21483-218">![Agents and permissions](./media/active-directory-saas-uservoice-tutorial/ic777812.png "Agents and permissions")</span></span>

5. <span data-ttu-id="21483-219">Klicka på **lägger till administratörer**.</span><span class="sxs-lookup"><span data-stu-id="21483-219">Click **Add admins**.</span></span>
   
    <span data-ttu-id="21483-220">![Lägg till administratörer](./media/active-directory-saas-uservoice-tutorial/ic777813.png "lägger till administratörer")</span><span class="sxs-lookup"><span data-stu-id="21483-220">![Add admins](./media/active-directory-saas-uservoice-tutorial/ic777813.png "Add admins")</span></span>

6. <span data-ttu-id="21483-221">På hello **bjuda in administratörer** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21483-221">On hello **Invite admins** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="21483-222">![Bjud in administratörer](./media/active-directory-saas-uservoice-tutorial/ic777814.png "bjuda in administratörer")</span><span class="sxs-lookup"><span data-stu-id="21483-222">![Invite admins](./media/active-directory-saas-uservoice-tutorial/ic777814.png "Invite admins")</span></span>
   
    <span data-ttu-id="21483-223">a.</span><span class="sxs-lookup"><span data-stu-id="21483-223">a.</span></span> <span data-ttu-id="21483-224">Ange hello hello konto du vill tooprovision och klicka sedan på e-postadress i hello e-postmeddelanden textruta **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="21483-224">In hello Emails textbox, type hello email address of hello account you want tooprovision, and then click **Add**.</span></span>
   
    <span data-ttu-id="21483-225">b.</span><span class="sxs-lookup"><span data-stu-id="21483-225">b.</span></span> <span data-ttu-id="21483-226">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="21483-226">Click **Invite**.</span></span>

> [!NOTE]
> <span data-ttu-id="21483-227">Du kan använda något annat UserVoice användarens konto skapas verktyg eller API: er som tillhandahålls av UserVoice tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="21483-227">You can use any other UserVoice user account creation tools or APIs provided by UserVoice tooprovision AAD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="21483-228">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="21483-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="21483-229">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooUserVoice.</span><span class="sxs-lookup"><span data-stu-id="21483-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUserVoice.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="21483-231">**tooassign Britta Simon tooUserVoice utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="21483-231">**tooassign Britta Simon tooUserVoice, perform hello following steps:**</span></span>

1. <span data-ttu-id="21483-232">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="21483-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="21483-234">Välj i listan med program hello **UserVoice**.</span><span class="sxs-lookup"><span data-stu-id="21483-234">In hello applications list, select **UserVoice**.</span></span>

    ![Hej UserVoice länken i listan med program hello](./media/active-directory-saas-uservoice-tutorial/tutorial_uservoice_app.png)  

3. <span data-ttu-id="21483-236">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="21483-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="21483-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="21483-238">Click **Add** button.</span></span> <span data-ttu-id="21483-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21483-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="21483-241">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="21483-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="21483-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21483-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="21483-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21483-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="21483-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21483-244">Test single sign-on</span></span>

<span data-ttu-id="21483-245">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="21483-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="21483-246">Du bör få automatiskt inloggade tooyour UserVoice programmet när du klickar på hello UserVoice-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="21483-246">When you click hello UserVoice tile in hello Access Panel, you should get automatically signed-on tooyour UserVoice application.</span></span>
<span data-ttu-id="21483-247">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="21483-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="21483-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="21483-248">Additional resources</span></span>

* [<span data-ttu-id="21483-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="21483-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="21483-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="21483-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-uservoice-tutorial/tutorial_general_203.png

