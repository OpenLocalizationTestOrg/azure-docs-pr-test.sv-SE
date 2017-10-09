---
title: "Självstudier: Azure Active Directory-integrering med PagerDuty | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PagerDuty."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: c3cfbedac3bf075e2d8cd833d5de7ca0bc9468b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a><span data-ttu-id="404eb-103">Självstudier: Azure Active Directory-integrering med PagerDuty</span><span class="sxs-lookup"><span data-stu-id="404eb-103">Tutorial: Azure Active Directory integration with PagerDuty</span></span>

<span data-ttu-id="404eb-104">I kursen får du lära dig hur toointegrate PagerDuty med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="404eb-104">In this tutorial, you learn how toointegrate PagerDuty with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="404eb-105">Integrera PagerDuty med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="404eb-105">Integrating PagerDuty with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="404eb-106">Du kan styra i Azure AD som har åtkomst till tooPagerDuty</span><span class="sxs-lookup"><span data-stu-id="404eb-106">You can control in Azure AD who has access tooPagerDuty</span></span>
- <span data-ttu-id="404eb-107">Du kan aktivera din användare tooautomatically get inloggade tooPagerDuty (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="404eb-107">You can enable your users tooautomatically get signed-on tooPagerDuty (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="404eb-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="404eb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="404eb-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="404eb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="404eb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="404eb-110">Prerequisites</span></span>

<span data-ttu-id="404eb-111">tooconfigure Azure AD-integrering med PagerDuty, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="404eb-111">tooconfigure Azure AD integration with PagerDuty, you need hello following items:</span></span>

- <span data-ttu-id="404eb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="404eb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="404eb-113">En PagerDuty enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="404eb-113">A PagerDuty single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="404eb-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="404eb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="404eb-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="404eb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="404eb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="404eb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="404eb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="404eb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="404eb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="404eb-118">Scenario description</span></span>
<span data-ttu-id="404eb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="404eb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="404eb-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="404eb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="404eb-121">Att lägga till PagerDuty från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="404eb-121">Adding PagerDuty from hello gallery</span></span>
2. <span data-ttu-id="404eb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="404eb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pagerduty-from-hello-gallery"></a><span data-ttu-id="404eb-123">Att lägga till PagerDuty från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="404eb-123">Adding PagerDuty from hello gallery</span></span>
<span data-ttu-id="404eb-124">tooconfigure hello integrering av PagerDuty i Azure AD, behöver du tooadd PagerDuty hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="404eb-124">tooconfigure hello integration of PagerDuty into Azure AD, you need tooadd PagerDuty from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="404eb-125">**tooadd PagerDuty från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="404eb-125">**tooadd PagerDuty from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="404eb-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="404eb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="404eb-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="404eb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="404eb-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="404eb-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="404eb-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="404eb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="404eb-133">Skriv i sökrutan hello **PagerDuty**väljer **PagerDuty** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="404eb-133">In hello search box, type **PagerDuty**, select  **PagerDuty**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="404eb-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="404eb-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="404eb-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PagerDuty baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="404eb-136">In this section, you configure and test Azure AD single sign-on with PagerDuty based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="404eb-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PagerDuty är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="404eb-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PagerDuty is tooa user in Azure AD.</span></span> <span data-ttu-id="404eb-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PagerDuty toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="404eb-138">In other words, a link relationship between an Azure AD user and hello related user in PagerDuty needs toobe established.</span></span>

<span data-ttu-id="404eb-139">I PagerDuty, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="404eb-139">In PagerDuty, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="404eb-140">tooconfigure och testa Azure AD enkel inloggning med PagerDuty, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="404eb-140">tooconfigure and test Azure AD single sign-on with PagerDuty, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="404eb-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="404eb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="404eb-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="404eb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="404eb-143">**[Skapa en testanvändare PagerDuty](#create-a-pagerduty-test-user)**  -toohave en motsvarighet för Britta Simon i PagerDuty som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="404eb-143">**[Create a PagerDuty test user](#create-a-pagerduty-test-user)** - toohave a counterpart of Britta Simon in PagerDuty that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="404eb-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="404eb-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="404eb-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="404eb-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="404eb-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="404eb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="404eb-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt PagerDuty program.</span><span class="sxs-lookup"><span data-stu-id="404eb-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PagerDuty application.</span></span>

<span data-ttu-id="404eb-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PagerDuty:**</span><span class="sxs-lookup"><span data-stu-id="404eb-148">**tooconfigure Azure AD single sign-on with PagerDuty, perform hello following steps:**</span></span>

1. <span data-ttu-id="404eb-149">I hello Azure-portalen på hello **PagerDuty** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="404eb-149">In hello Azure portal, on hello **PagerDuty** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="404eb-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="404eb-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. <span data-ttu-id="404eb-153">På hello **PagerDuty domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="404eb-153">On hello **PagerDuty Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och PagerDuty domän med enkel inloggning information](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_url.png)

    <span data-ttu-id="404eb-155">a.</span><span class="sxs-lookup"><span data-stu-id="404eb-155">a.</span></span> <span data-ttu-id="404eb-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="404eb-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    <span data-ttu-id="404eb-157">b.</span><span class="sxs-lookup"><span data-stu-id="404eb-157">b.</span></span> <span data-ttu-id="404eb-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.pagerduty.com`</span><span class="sxs-lookup"><span data-stu-id="404eb-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.pagerduty.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="404eb-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="404eb-159">These values are not real.</span></span> <span data-ttu-id="404eb-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="404eb-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="404eb-161">Kontakta [PagerDuty klienten supportteamet](https://www.pagerduty.com/support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="404eb-161">Contact [PagerDuty Client support team](https://www.pagerduty.com/support/) tooget these values.</span></span> 

4. <span data-ttu-id="404eb-162">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="404eb-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. <span data-ttu-id="404eb-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="404eb-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-pagerduty-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="404eb-166">På hello **PagerDuty Configuration** klickar du på **konfigurera PagerDuty** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="404eb-166">On hello **PagerDuty Configuration** section, click **Configure PagerDuty** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="404eb-167">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="404eb-167">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![PagerDuty konfiguration](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. <span data-ttu-id="404eb-169">Logga in på webbplatsen Pagerduty företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="404eb-169">In a different web browser window, log into your Pagerduty company site as an administrator.</span></span>

8. <span data-ttu-id="404eb-170">Hello-menyn överst hello **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="404eb-170">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="404eb-171">![Kontoinställningar](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="404eb-171">![Account Settings](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "Account Settings")</span></span>

9. <span data-ttu-id="404eb-172">Klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="404eb-172">Click **Single Sign-on**.</span></span>
   
    <span data-ttu-id="404eb-173">![Enkel inloggning](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="404eb-173">![Single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "Single sign-on")</span></span>

10. <span data-ttu-id="404eb-174">På hello **aktivera enkel inloggning (SSO)** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="404eb-174">On hello **Enable Single Sign-on (SSO)** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="404eb-175">![Aktivera enkel inloggning](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "aktivera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="404eb-175">![Enable single sign-on](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "Enable single sign-on")</span></span>
   
    <span data-ttu-id="404eb-176">a.</span><span class="sxs-lookup"><span data-stu-id="404eb-176">a.</span></span> <span data-ttu-id="404eb-177">Öppna din Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="404eb-177">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>
  
    <span data-ttu-id="404eb-178">b.</span><span class="sxs-lookup"><span data-stu-id="404eb-178">b.</span></span> <span data-ttu-id="404eb-179">I hello **Inloggningswebbadressen** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="404eb-179">In hello **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="404eb-180">c.</span><span class="sxs-lookup"><span data-stu-id="404eb-180">c.</span></span> <span data-ttu-id="404eb-181">I hello **logga ut URL** textruta klistra in **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="404eb-181">In hello **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
 
    <span data-ttu-id="404eb-182">d.</span><span class="sxs-lookup"><span data-stu-id="404eb-182">d.</span></span> <span data-ttu-id="404eb-183">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="404eb-183">Select **Turn on Single Sign-on**.</span></span>
 
    <span data-ttu-id="404eb-184">e.</span><span class="sxs-lookup"><span data-stu-id="404eb-184">e.</span></span> <span data-ttu-id="404eb-185">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="404eb-185">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="404eb-186">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="404eb-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="404eb-187">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="404eb-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="404eb-188">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="404eb-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="404eb-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="404eb-189">Create an Azure AD test user</span></span>

<span data-ttu-id="404eb-190">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="404eb-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="404eb-192">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="404eb-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="404eb-193">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="404eb-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="404eb-195">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="404eb-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="404eb-197">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="404eb-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="404eb-199">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="404eb-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="404eb-201">a.</span><span class="sxs-lookup"><span data-stu-id="404eb-201">a.</span></span> <span data-ttu-id="404eb-202">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="404eb-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="404eb-203">b.</span><span class="sxs-lookup"><span data-stu-id="404eb-203">b.</span></span> <span data-ttu-id="404eb-204">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="404eb-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="404eb-205">c.</span><span class="sxs-lookup"><span data-stu-id="404eb-205">c.</span></span> <span data-ttu-id="404eb-206">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="404eb-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="404eb-207">d.</span><span class="sxs-lookup"><span data-stu-id="404eb-207">d.</span></span> <span data-ttu-id="404eb-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="404eb-208">Click **Create**.</span></span>
 
### <a name="create-a-pagerduty-test-user"></a><span data-ttu-id="404eb-209">Skapa en testanvändare PagerDuty</span><span class="sxs-lookup"><span data-stu-id="404eb-209">Create a PagerDuty test user</span></span>

<span data-ttu-id="404eb-210">tooenable Azure AD-användare toolog i tooPagerDuty, måste de etableras i PagerDuty.</span><span class="sxs-lookup"><span data-stu-id="404eb-210">tooenable Azure AD users toolog in tooPagerDuty, they must be provisioned into PagerDuty.</span></span>  
<span data-ttu-id="404eb-211">Hello gäller PagerDuty är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="404eb-211">In hello case of PagerDuty, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="404eb-212">Du kan använda något annat Pagerduty användarens konto skapas verktyg eller API: er som tillhandahålls av Pagerduty tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="404eb-212">You can use any other Pagerduty user account creation tools or APIs provided by Pagerduty tooprovision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="404eb-213">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="404eb-213">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="404eb-214">Logga in tooyour **Pagerduty** klient.</span><span class="sxs-lookup"><span data-stu-id="404eb-214">Log in tooyour **Pagerduty** tenant.</span></span>

2. <span data-ttu-id="404eb-215">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="404eb-215">In hello menu on hello top, click **Users**.</span></span>

3. <span data-ttu-id="404eb-216">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="404eb-216">Click **Add Users**.</span></span>
   
    <span data-ttu-id="404eb-217">![Lägg till användare](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="404eb-217">![Add Users](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "Add Users")</span></span>

4.  <span data-ttu-id="404eb-218">På hello **bjuda in ditt team** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="404eb-218">On hello **Invite your team** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="404eb-219">![Bjud in ditt team](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "bjuda in ditt team")</span><span class="sxs-lookup"><span data-stu-id="404eb-219">![Invite your team](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "Invite your team")</span></span>

    <span data-ttu-id="404eb-220">a.</span><span class="sxs-lookup"><span data-stu-id="404eb-220">a.</span></span> <span data-ttu-id="404eb-221">Typen hello **första och sista namnet** för användare som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="404eb-221">Type hello **First and Last Name** of user like **Britta Simon**.</span></span> 
   
    <span data-ttu-id="404eb-222">b.</span><span class="sxs-lookup"><span data-stu-id="404eb-222">b.</span></span> <span data-ttu-id="404eb-223">Ange **e-post** -adressen för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="404eb-223">Enter **Email** address of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="404eb-224">c.</span><span class="sxs-lookup"><span data-stu-id="404eb-224">c.</span></span> <span data-ttu-id="404eb-225">Klicka på **Lägg till**, och klicka sedan på **skicka inbjuder**.</span><span class="sxs-lookup"><span data-stu-id="404eb-225">Click **Add**, and then click **Send Invites**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="404eb-226">Alla tillagda användare får en inbjudan toocreate PagerDuty-konto.</span><span class="sxs-lookup"><span data-stu-id="404eb-226">All added users will receive an invite toocreate a PagerDuty account.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="404eb-227">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="404eb-227">Assign hello Azure AD test user</span></span>

<span data-ttu-id="404eb-228">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPagerDuty.</span><span class="sxs-lookup"><span data-stu-id="404eb-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPagerDuty.</span></span>

![Tilldela hello användarroll][200]

<span data-ttu-id="404eb-230">**tooassign Britta Simon tooPagerDuty utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="404eb-230">**tooassign Britta Simon tooPagerDuty, perform hello following steps:**</span></span>

1. <span data-ttu-id="404eb-231">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="404eb-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="404eb-233">Välj i listan med program hello **PagerDuty**.</span><span class="sxs-lookup"><span data-stu-id="404eb-233">In hello applications list, select **PagerDuty**.</span></span>

    ![Hej PagerDuty länken i listan med program hello](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. <span data-ttu-id="404eb-235">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="404eb-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="404eb-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="404eb-237">Click **Add** button.</span></span> <span data-ttu-id="404eb-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="404eb-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="404eb-240">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="404eb-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="404eb-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="404eb-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="404eb-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="404eb-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="404eb-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="404eb-243">Test single sign-on</span></span>

<span data-ttu-id="404eb-244">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="404eb-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="404eb-245">När du klickar på får hello PagerDuty panelen i hello åtkomst Panelyou automatiskt inloggade tooyour PagerDuty program.</span><span class="sxs-lookup"><span data-stu-id="404eb-245">When you click hello PagerDuty tile in hello Access Panelyou should get automatically signed-on tooyour PagerDuty application.</span></span>

<span data-ttu-id="404eb-246">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="404eb-246">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="404eb-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="404eb-247">Additional resources</span></span>

* [<span data-ttu-id="404eb-248">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="404eb-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="404eb-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="404eb-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_203.png

