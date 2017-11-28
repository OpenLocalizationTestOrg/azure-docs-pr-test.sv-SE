---
title: "Självstudier: Azure Active Directory-integrering med GitHub | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och GitHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 688779de4e6627e49c0e3e8a7576f2f8c7a81ab1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a><span data-ttu-id="5e4eb-103">Självstudier: Azure Active Directory-integrering med GitHub</span><span class="sxs-lookup"><span data-stu-id="5e4eb-103">Tutorial: Azure Active Directory integration with GitHub</span></span>

<span data-ttu-id="5e4eb-104">I kursen får du lära dig hur toointegrate GitHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5e4eb-104">In this tutorial, you learn how toointegrate GitHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5e4eb-105">Integrera GitHub med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-105">Integrating GitHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5e4eb-106">Du kan styra i Azure AD som har åtkomst till tooGitHub</span><span class="sxs-lookup"><span data-stu-id="5e4eb-106">You can control in Azure AD who has access tooGitHub</span></span>
- <span data-ttu-id="5e4eb-107">Du kan aktivera din användare tooautomatically get inloggade tooGitHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5e4eb-107">You can enable your users tooautomatically get signed-on tooGitHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5e4eb-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="5e4eb-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="5e4eb-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5e4eb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e4eb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5e4eb-110">Prerequisites</span></span>

<span data-ttu-id="5e4eb-111">tooconfigure Azure AD-integrering med GitHub, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-111">tooconfigure Azure AD integration with GitHub, you need hello following items:</span></span>

- <span data-ttu-id="5e4eb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5e4eb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5e4eb-113">En GitHub enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5e4eb-113">A GitHub single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="5e4eb-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="5e4eb-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5e4eb-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="5e4eb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5e4eb-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="5e4eb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5e4eb-118">Scenario description</span></span>
<span data-ttu-id="5e4eb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5e4eb-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5e4eb-121">Att lägga till GitHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5e4eb-121">Adding GitHub from hello gallery</span></span>
2. <span data-ttu-id="5e4eb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e4eb-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-github-from-hello-gallery"></a><span data-ttu-id="5e4eb-123">Att lägga till GitHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5e4eb-123">Adding GitHub from hello gallery</span></span>
<span data-ttu-id="5e4eb-124">tooconfigure hello integrering av GitHub i Azure AD, behöver du tooadd GitHub hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-124">tooconfigure hello integration of GitHub into Azure AD, you need tooadd GitHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5e4eb-125">**tooadd GitHub från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5e4eb-125">**tooadd GitHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e4eb-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5e4eb-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5e4eb-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5e4eb-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5e4eb-133">Skriv i sökrutan hello **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-133">In hello search box, type **GitHub.com**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. <span data-ttu-id="5e4eb-135">Markera hello resultat på panelen **GitHub**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-135">In hello results panel, select **GitHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5e4eb-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e4eb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5e4eb-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med GitHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-138">In this section, you configure and test Azure AD single sign-on with GitHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5e4eb-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i GitHub är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in GitHub is tooa user in Azure AD.</span></span> <span data-ttu-id="5e4eb-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i GitHub toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-140">In other words, a link relationship between an Azure AD user and hello related user in GitHub needs toobe established.</span></span>

<span data-ttu-id="5e4eb-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i GitHub.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in GitHub.</span></span>

<span data-ttu-id="5e4eb-142">tooconfigure och testa Azure AD enkel inloggning med GitHub, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-142">tooconfigure and test Azure AD single sign-on with GitHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5e4eb-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5e4eb-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5e4eb-145">**[Skapa en testanvändare i GitHub](#creating-a-GitHub-test-user)**  -toohave en motsvarighet för Britta Simon i GitHub som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-145">**[Creating a GitHub test user](#creating-a-GitHub-test-user)** - toohave a counterpart of Britta Simon in GitHub that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="5e4eb-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5e4eb-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5e4eb-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e4eb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5e4eb-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt GitHub-program.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your GitHub application.</span></span>

<span data-ttu-id="5e4eb-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med GitHub:**</span><span class="sxs-lookup"><span data-stu-id="5e4eb-150">**tooconfigure Azure AD single sign-on with GitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e4eb-151">I hello Azure Management portal på hello **GitHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-151">In hello Azure Management portal, on hello **GitHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5e4eb-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. <span data-ttu-id="5e4eb-155">På hello **GitHub domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-155">On hello **GitHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    <span data-ttu-id="5e4eb-157">a.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-157">a.</span></span> <span data-ttu-id="5e4eb-158">I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://github.com/orgs/<entity-id>/sso`</span><span class="sxs-lookup"><span data-stu-id="5e4eb-158">In hello **Sign-on URL** textbox, type hello value as: `https://github.com/orgs/<entity-id>/sso`</span></span>

    <span data-ttu-id="5e4eb-159">b.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-159">b.</span></span> <span data-ttu-id="5e4eb-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://github.com/orgs/<entity-id>`</span><span class="sxs-lookup"><span data-stu-id="5e4eb-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://github.com/orgs/<entity-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5e4eb-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="5e4eb-162">Du har tooupdate dessa värden med hello faktiska rör-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-162">You have tooupdate these values with hello actual Sing-on URL and Identifier.</span></span> <span data-ttu-id="5e4eb-163">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="5e4eb-164">Gå tooGitHub Admin avsnittet tooretrieve dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-164">Go tooGitHub Admin section tooretrieve these values.</span></span> 

4. <span data-ttu-id="5e4eb-165">På hello **användarattribut** väljer **användar-ID** som user.mail.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-165">On hello **User Attributes** section, select **User Identifier** as user.mail.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. <span data-ttu-id="5e4eb-167">På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-167">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. <span data-ttu-id="5e4eb-169">På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-169">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="5e4eb-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-170">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. <span data-ttu-id="5e4eb-172">På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-172">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. <span data-ttu-id="5e4eb-174">På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-174">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="5e4eb-176">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-176">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. <span data-ttu-id="5e4eb-178">På hello **GitHub Configuration** klickar du på **konfigurera GitHub** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-178">On hello **GitHub Configuration** section, click **Configure GitHub** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. <span data-ttu-id="5e4eb-181">Logga in på webbplatsen GitHub organisation som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-181">In a different web browser window, log into your GitHub organization site as an administrator.</span></span>

12. <span data-ttu-id="5e4eb-182">Navigera för**inställningar** och på **säkerhet**</span><span class="sxs-lookup"><span data-stu-id="5e4eb-182">Navigate too**Settings** and click **Security**</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. <span data-ttu-id="5e4eb-184">Kontrollera hello **aktivera SAML-autentisering** rutan avslöja hello enkel inloggning konfigurationsfält.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-184">Check hello **Enable SAML authentication** box, revealing hello Single Sign-on configuration fields.</span></span> <span data-ttu-id="5e4eb-185">Använd sedan hello enkel inloggning URL värdet tooupdate hello enkel inloggnings-URL på Azure AD-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-185">Then, use hello single sign-on URL value tooupdate hello Single sign-on URL on Azure AD configuration.</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. <span data-ttu-id="5e4eb-187">Konfigurera hello följande fält:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-187">Configure hello following fields:</span></span>

    <span data-ttu-id="5e4eb-188">a.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-188">a.</span></span> <span data-ttu-id="5e4eb-189">**Logga in URL: en**: Ange **SAML enkel inloggning Tjänstwebbadress** från hello **konfigurera GitHub** avsnittet om Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e4eb-189">**Sign on URL**: Enter **SAML Single sign-on Service URL** from hello **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="5e4eb-190">b.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-190">b.</span></span> <span data-ttu-id="5e4eb-191">**Utfärdaren**: Ange **SAML enhets-ID** från hello **konfigurera GitHub** avsnittet om Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e4eb-191">**Issuer**: Enter **SAML Entity ID** from hello **Configure GitHub** section on Azure AD</span></span>

    <span data-ttu-id="5e4eb-192">c.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-192">c.</span></span> <span data-ttu-id="5e4eb-193">**Certifikatets offentliga**: öppna hello hämtat certifikat från Azure AD i en anteckningar och kopiera hello innehåll inklusive ”BEGIN CERTIFICATE” och ”END CERTIFICATE”</span><span class="sxs-lookup"><span data-stu-id="5e4eb-193">**Public Certificate**: Open hello downloaded certificate from Azure AD in a notepad and copy hello content including "BEGIN CERTIFICATE" and "END CERTIFICATE"</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. <span data-ttu-id="5e4eb-195">Klicka på **Test SAML-konfiguration** tooconfirm som inga verifieringsfel eller fel under enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-195">Click on **Test SAML configuration** tooconfirm that no validation failures or errors during SSO.</span></span>

    ![Inställningar](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. <span data-ttu-id="5e4eb-197">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="5e4eb-197">Click **Save**</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5e4eb-198">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e4eb-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="5e4eb-199">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-199">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5e4eb-201">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5e4eb-201">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e4eb-202">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-202">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5e4eb-204">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-204">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5e4eb-206">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-206">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5e4eb-208">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-208">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5e4eb-210">a.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-210">a.</span></span> <span data-ttu-id="5e4eb-211">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-211">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5e4eb-212">b.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-212">b.</span></span> <span data-ttu-id="5e4eb-213">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-213">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5e4eb-214">c.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-214">c.</span></span> <span data-ttu-id="5e4eb-215">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5e4eb-216">d.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-216">d.</span></span> <span data-ttu-id="5e4eb-217">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-217">Click **Create**.</span></span> 


### <a name="creating-a-github-test-user"></a><span data-ttu-id="5e4eb-218">Skapa en testanvändare i GitHub</span><span class="sxs-lookup"><span data-stu-id="5e4eb-218">Creating a GitHub test user</span></span>

<span data-ttu-id="5e4eb-219">I ordning tooenable Azure AD-användare toolog i GitHub, måste de etableras i GitHub.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-219">In order tooenable Azure AD users toolog into GitHub, they must be provisioned into GitHub.</span></span>  
<span data-ttu-id="5e4eb-220">Hello gäller GitHub är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-220">In hello case of GitHub, provisioning is a manual task.</span></span>

<span data-ttu-id="5e4eb-221">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5e4eb-221">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e4eb-222">Logga in tooyour GitHub företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-222">Log in tooyour GitHub company site as an administrator.</span></span>

2. <span data-ttu-id="5e4eb-223">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-223">Click **People**.</span></span>

    <span data-ttu-id="5e4eb-224">![Personer](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "personer")</span><span class="sxs-lookup"><span data-stu-id="5e4eb-224">![People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "People")</span></span>

3. <span data-ttu-id="5e4eb-225">Klicka på **inbjudan medlem**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-225">Click **Invite member**.</span></span>

    <span data-ttu-id="5e4eb-226">![Bjuda in användare](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="5e4eb-226">![Invite Users](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "Invite Users")</span></span>

4. <span data-ttu-id="5e4eb-227">På hello **inbjudan medlem** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5e4eb-227">On hello **Invite member** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="5e4eb-228">a.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-228">a.</span></span> <span data-ttu-id="5e4eb-229">I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-229">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="5e4eb-230">![Bjuda in](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="5e4eb-230">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "Invite People")</span></span>
    
    <span data-ttu-id="5e4eb-231">b.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-231">b.</span></span> <span data-ttu-id="5e4eb-232">Klicka på **skicka inbjudan**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-232">Click **Send Invitation**.</span></span>

    <span data-ttu-id="5e4eb-233">![Bjuda in](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="5e4eb-233">![Invite People](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "Invite People")</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e4eb-234">hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-234">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5e4eb-235">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5e4eb-235">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5e4eb-236">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooGitHub sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-236">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooGitHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5e4eb-238">**tooassign Britta Simon tooGitHub utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5e4eb-238">**tooassign Britta Simon tooGitHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="5e4eb-239">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-239">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5e4eb-241">Välj i listan med program hello **GitHub.com**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-241">In hello applications list, select **GitHub.com**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. <span data-ttu-id="5e4eb-243">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-243">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5e4eb-245">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-245">Click **Add** button.</span></span> <span data-ttu-id="5e4eb-246">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5e4eb-248">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-248">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5e4eb-249">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5e4eb-250">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="5e4eb-251">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5e4eb-251">Testing single sign-on</span></span>

<span data-ttu-id="5e4eb-252">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-252">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5e4eb-253">Du bör få inloggade tooyour GitHub programmet när du klickar på hello GitHub-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-253">When you click hello GitHub tile in hello Access Panel, you should get signed-on tooyour GitHub application.</span></span> <span data-ttu-id="5e4eb-254">Du måste logga in som en organisationskonto men måste toolog in med ditt eget konto.</span><span class="sxs-lookup"><span data-stu-id="5e4eb-254">You'll be logging in as an Organization account but then need toolog in with your personal account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="5e4eb-255">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5e4eb-255">Additional resources</span></span>

* [<span data-ttu-id="5e4eb-256">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5e4eb-256">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5e4eb-257">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5e4eb-257">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png
