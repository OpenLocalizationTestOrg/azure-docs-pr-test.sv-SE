---
title: "Självstudier: Azure Active Directory-integrering med SAML SSO för Jira resolution GmbH | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAML SSO för Jira resolution GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: a3436a9aa25640e931a61b5ba4a62611e6e07890
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a><span data-ttu-id="26476-103">Självstudier: Azure Active Directory-integrering med SAML SSO för Jira resolution GmbH</span><span class="sxs-lookup"><span data-stu-id="26476-103">Tutorial: Azure Active Directory integration with SAML SSO for Jira by resolution GmbH</span></span>

<span data-ttu-id="26476-104">I kursen får du lära dig hur toointegrate SAML SSO för Jira resolution GmbH med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="26476-104">In this tutorial, you learn how toointegrate SAML SSO for Jira by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="26476-105">Integrera SAML SSO för Jira resolution GmbH med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="26476-105">Integrating SAML SSO for Jira by resolution GmbH with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="26476-106">Du kan styra i Azure AD som har åtkomst tooSAML enkel inloggning för Jira resolution GmbH</span><span class="sxs-lookup"><span data-stu-id="26476-106">You can control in Azure AD who has access tooSAML SSO for Jira by resolution GmbH</span></span>
- <span data-ttu-id="26476-107">Du kan aktivera din användare tooautomatically get inloggade tooSAML enkel inloggning för Jira resolution GmbH (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="26476-107">You can enable your users tooautomatically get signed-on tooSAML SSO for Jira by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="26476-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="26476-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="26476-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="26476-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26476-110">Krav</span><span class="sxs-lookup"><span data-stu-id="26476-110">Prerequisites</span></span>

<span data-ttu-id="26476-111">tooconfigure Azure AD-integrering med SAML SSO för Jira resolution GmbH, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="26476-111">tooconfigure Azure AD integration with SAML SSO for Jira by resolution GmbH, you need hello following items:</span></span>

- <span data-ttu-id="26476-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="26476-112">An Azure AD subscription</span></span>
- <span data-ttu-id="26476-113">En SAML SSO för Jira av upplösning GmbH enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="26476-113">A SAML SSO for Jira by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="26476-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="26476-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="26476-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="26476-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="26476-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="26476-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="26476-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="26476-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="26476-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="26476-118">Scenario description</span></span>
<span data-ttu-id="26476-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="26476-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="26476-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="26476-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="26476-121">Att lägga till SAML SSO för Jira resolution GmbH från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="26476-121">Adding SAML SSO for Jira by resolution GmbH from hello gallery</span></span>
2. <span data-ttu-id="26476-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26476-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-hello-gallery"></a><span data-ttu-id="26476-123">Att lägga till SAML SSO för Jira resolution GmbH från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="26476-123">Adding SAML SSO for Jira by resolution GmbH from hello gallery</span></span>
<span data-ttu-id="26476-124">tooconfigure hello integrering av SAML SSO för Jira resolution GmbH i Azure AD, behöver du tooadd SAML SSO för Jira resolution GmbH hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="26476-124">tooconfigure hello integration of SAML SSO for Jira by resolution GmbH into Azure AD, you need tooadd SAML SSO for Jira by resolution GmbH from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="26476-125">**tooadd SAML SSO för Jira resolution GmbH från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="26476-125">**tooadd SAML SSO for Jira by resolution GmbH from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="26476-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="26476-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="26476-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="26476-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="26476-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="26476-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="26476-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26476-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="26476-133">Skriv i sökrutan hello **SAML SSO för Jira resolution GmbH**.</span><span class="sxs-lookup"><span data-stu-id="26476-133">In hello search box, type **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_search.png)

5. <span data-ttu-id="26476-135">Markera hello resultat på panelen **SAML SSO för Jira resolution GmbH**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="26476-135">In hello results panel, select **SAML SSO for Jira by resolution GmbH**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="26476-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26476-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="26476-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAML SSO för Jira resolution GmbH baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="26476-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="26476-139">För enkel inloggning toowork måste Azure AD tooknow användare vilka hello motsvarighet i SAML SSO för Jira resolution GmbH är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="26476-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAML SSO for Jira by resolution GmbH is tooa user in Azure AD.</span></span> <span data-ttu-id="26476-140">Med andra ord en länk mellan en Azure AD-användare och hello relaterade användare i SAML SSO för Jira resolution GmbH måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="26476-140">In other words, a link relationship between an Azure AD user and hello related user in SAML SSO for Jira by resolution GmbH needs toobe established.</span></span>

<span data-ttu-id="26476-141">SAML SSO för Jira resolution GmbH, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="26476-141">In SAML SSO for Jira by resolution GmbH, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="26476-142">tooconfigure och testa Azure AD enkel inloggning med SAML SSO för Jira resolution GmbH, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="26476-142">tooconfigure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="26476-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="26476-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="26476-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="26476-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="26476-145">**[Skapa en SAML SSO för Jira av upplösning GmbH testanvändare](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  -toohave en motsvarighet för Britta Simon SAML SSO för Jira resolution GmbH som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="26476-145">**[Creating a SAML SSO for Jira by resolution GmbH test user](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)** - toohave a counterpart of Britta Simon in SAML SSO for Jira by resolution GmbH that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="26476-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="26476-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="26476-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="26476-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="26476-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26476-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="26476-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din SAML SSO för Jira resolution GmbH program.</span><span class="sxs-lookup"><span data-stu-id="26476-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAML SSO for Jira by resolution GmbH application.</span></span>

<span data-ttu-id="26476-150">**tooconfigure Azure AD enkel inloggning med SAML SSO för Jira resolution GmbH, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="26476-150">**tooconfigure Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="26476-151">I hello Azure-portalen på hello **SAML SSO för Jira resolution GmbH** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="26476-151">In hello Azure portal, on hello **SAML SSO for Jira by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="26476-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="26476-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

3. <span data-ttu-id="26476-155">På hello **SAML SSO Jira resolution GmbH domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="26476-155">On hello **SAML SSO for Jira by resolution GmbH Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    <span data-ttu-id="26476-157">a.</span><span class="sxs-lookup"><span data-stu-id="26476-157">a.</span></span> <span data-ttu-id="26476-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="26476-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="26476-159">b.</span><span class="sxs-lookup"><span data-stu-id="26476-159">b.</span></span> <span data-ttu-id="26476-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="26476-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="26476-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="26476-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="26476-162">Om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="26476-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    <span data-ttu-id="26476-164">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="26476-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="26476-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="26476-165">These values are not real.</span></span> <span data-ttu-id="26476-166">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="26476-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="26476-167">Kontakta [SAML SSO för Jira resolution GmbH klienten supportteam](https://www.resolution.de/go/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="26476-167">Contact [SAML SSO for Jira by resolution GmbH Client support team](https://www.resolution.de/go/support) tooget these values.</span></span> 

5. <span data-ttu-id="26476-168">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="26476-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

6. <span data-ttu-id="26476-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="26476-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="26476-172">Logga in tooyour i ett annat webbläsarfönster **SAML SSO för Jira upplösning GmbH admin Portal** som administratör.</span><span class="sxs-lookup"><span data-stu-id="26476-172">In a different web browser window, log in tooyour **SAML SSO for Jira by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="26476-173">Hovra över kugge och klicka på hello **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="26476-173">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon1.png)

9. <span data-ttu-id="26476-175">Du kan omdirigerade tooAdministrator sidan.</span><span class="sxs-lookup"><span data-stu-id="26476-175">You are redirected tooAdministrator Access page.</span></span> <span data-ttu-id="26476-176">Ange hello **lösenord** och på **Bekräfta** knappen.</span><span class="sxs-lookup"><span data-stu-id="26476-176">Enter hello **Password** and click **Confirm** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon2.png)

10. <span data-ttu-id="26476-178">Klicka under tillägg fliken avsnitt **söka efter nya tillägg**.</span><span class="sxs-lookup"><span data-stu-id="26476-178">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="26476-179">Sök **SAML enkel inloggning (SSO) för JIRA** och på **installera** knappen tooinstall hello ny SAML-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="26476-179">Search **SAML Single Sign On (SSO) for JIRA** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon7.png)

11. <span data-ttu-id="26476-181">hello plugin installationen startar.</span><span class="sxs-lookup"><span data-stu-id="26476-181">hello plugin installation will start.</span></span> <span data-ttu-id="26476-182">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="26476-182">Click **Close**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon8.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon9.png)

12. <span data-ttu-id="26476-185">Klicka på **Hantera**.</span><span class="sxs-lookup"><span data-stu-id="26476-185">Click **Manage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon10.png)
    
13. <span data-ttu-id="26476-187">Klicka på **konfigurera** tooconfigure hello nytt plugin-program.</span><span class="sxs-lookup"><span data-stu-id="26476-187">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon11.png)

14. <span data-ttu-id="26476-189">På **SAML SingleSignOn Plugin-konfiguration** klickar du på **lägga till ytterligare identitetsleverantör** knappen tooconfigure hello inställningar av identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="26476-189">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button tooconfigure hello settings of Identity Provider.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon4.png)

15. <span data-ttu-id="26476-191">Utför följande steg på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="26476-191">Perform following steps on this page:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon5.png)
 
    <span data-ttu-id="26476-193">a.</span><span class="sxs-lookup"><span data-stu-id="26476-193">a.</span></span> <span data-ttu-id="26476-194">Lägg till **namn** av hello identitetsprovider (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="26476-194">Add **Name** of hello Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="26476-195">b.</span><span class="sxs-lookup"><span data-stu-id="26476-195">b.</span></span> <span data-ttu-id="26476-196">Lägg till **beskrivning** av hello identitetsprovider (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="26476-196">Add **Description** of hello Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="26476-197">c.</span><span class="sxs-lookup"><span data-stu-id="26476-197">c.</span></span> <span data-ttu-id="26476-198">Klicka på **XML** och välj hello **Metadata** fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="26476-198">Click **XML** and select hello **Metadata** file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="26476-199">d.</span><span class="sxs-lookup"><span data-stu-id="26476-199">d.</span></span> <span data-ttu-id="26476-200">Klicka på **belastningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="26476-200">Click **Load** button.</span></span>

    <span data-ttu-id="26476-201">e.</span><span class="sxs-lookup"><span data-stu-id="26476-201">e.</span></span> <span data-ttu-id="26476-202">Den läser hello IdP metadata och fyller hello fält som är markerade i hello skärmbild.</span><span class="sxs-lookup"><span data-stu-id="26476-202">It reads hello IdP metadata and populates hello fields as highlighted in hello screenshot.</span></span>   

16. <span data-ttu-id="26476-203">Klicka på **Spara inställningar** knappen toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="26476-203">Click **Save settings** button toosave hello settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="26476-205">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="26476-205">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="26476-206">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="26476-206">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="26476-207">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="26476-207">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="26476-208">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="26476-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="26476-209">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="26476-209">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="26476-211">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="26476-211">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="26476-212">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="26476-212">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="26476-214">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="26476-214">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="26476-216">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26476-216">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="26476-218">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="26476-218">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="26476-220">a.</span><span class="sxs-lookup"><span data-stu-id="26476-220">a.</span></span> <span data-ttu-id="26476-221">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="26476-221">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="26476-222">b.</span><span class="sxs-lookup"><span data-stu-id="26476-222">b.</span></span> <span data-ttu-id="26476-223">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="26476-223">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="26476-224">c.</span><span class="sxs-lookup"><span data-stu-id="26476-224">c.</span></span> <span data-ttu-id="26476-225">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="26476-225">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="26476-226">d.</span><span class="sxs-lookup"><span data-stu-id="26476-226">d.</span></span> <span data-ttu-id="26476-227">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="26476-227">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a><span data-ttu-id="26476-228">Skapa en SAML SSO för Jira av upplösning GmbH testanvändare</span><span class="sxs-lookup"><span data-stu-id="26476-228">Creating a SAML SSO for Jira by resolution GmbH test user</span></span>

<span data-ttu-id="26476-229">tooenable Azure AD-användare toolog i tooSAML enkel inloggning för Jira resolution GmbH de måste etableras i SAML SSO för Jira resolution GmbH.</span><span class="sxs-lookup"><span data-stu-id="26476-229">tooenable Azure AD users toolog in tooSAML SSO for Jira by resolution GmbH, they must be provisioned into SAML SSO for Jira by resolution GmbH.</span></span>  
<span data-ttu-id="26476-230">SAML SSO för Jira resolution GmbH är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="26476-230">In SAML SSO for Jira by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="26476-231">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="26476-231">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="26476-232">Logga in tooyour SAML SSO för Jira av upplösning GmbH företagets plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="26476-232">Log in tooyour SAML SSO for Jira by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="26476-233">Hovra över kugge och klicka på hello **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="26476-233">Hover on cog and click hello **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user1.png) 

3. <span data-ttu-id="26476-235">Du är omdirigerade tooAdministrator åtkomst sidan tooenter **lösenord** och på **Bekräfta** knappen.</span><span class="sxs-lookup"><span data-stu-id="26476-235">You are redirected tooAdministrator Access page tooenter **Password** and click **Confirm** button.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user2.png) 

4. <span data-ttu-id="26476-237">Under **Användarhantering** avsnittet klickar du på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="26476-237">Under **User management** tab section, click **create user**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user3.png) 

5. <span data-ttu-id="26476-239">På hello **”skapa nya användare”** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="26476-239">On hello **“Create new user”** dialog page, perform hello following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user4.png) 

    <span data-ttu-id="26476-241">a.</span><span class="sxs-lookup"><span data-stu-id="26476-241">a.</span></span> <span data-ttu-id="26476-242">I hello **e-postadress** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="26476-242">In hello **Email address** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="26476-243">b.</span><span class="sxs-lookup"><span data-stu-id="26476-243">b.</span></span> <span data-ttu-id="26476-244">I hello **fullständiga namn** textruta typen hello användarens som Britta Simon fullständiga namn.</span><span class="sxs-lookup"><span data-stu-id="26476-244">In hello **Full Name** textbox, type full name of hello user like Britta Simon.</span></span>

    <span data-ttu-id="26476-245">c.</span><span class="sxs-lookup"><span data-stu-id="26476-245">c.</span></span> <span data-ttu-id="26476-246">I hello **användarnamn** textruta hello e-post för användare som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="26476-246">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="26476-247">d.</span><span class="sxs-lookup"><span data-stu-id="26476-247">d.</span></span> <span data-ttu-id="26476-248">I hello **lösenord** textruta typen hello lösenord för användare.</span><span class="sxs-lookup"><span data-stu-id="26476-248">In hello **Password** textbox, type hello password of user.</span></span>

    <span data-ttu-id="26476-249">e.</span><span class="sxs-lookup"><span data-stu-id="26476-249">e.</span></span> <span data-ttu-id="26476-250">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="26476-250">Click **Create user**.</span></span>   

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="26476-251">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="26476-251">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="26476-252">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAML enkel inloggning för Jira resolution GmbH.</span><span class="sxs-lookup"><span data-stu-id="26476-252">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAML SSO for Jira by resolution GmbH.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="26476-254">**tooassign Britta Simon tooSAML SSO för Jira resolution GmbH, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="26476-254">**tooassign Britta Simon tooSAML SSO for Jira by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="26476-255">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="26476-255">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="26476-257">Välj i listan med program hello **SAML SSO för Jira resolution GmbH**.</span><span class="sxs-lookup"><span data-stu-id="26476-257">In hello applications list, select **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_app.png) 

3. <span data-ttu-id="26476-259">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="26476-259">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="26476-261">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="26476-261">Click **Add** button.</span></span> <span data-ttu-id="26476-262">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26476-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="26476-264">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="26476-264">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="26476-265">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26476-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="26476-266">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="26476-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="26476-267">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="26476-267">Testing single sign-on</span></span>

<span data-ttu-id="26476-268">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="26476-268">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="26476-269">När du klickar på hello SAML SSO för Jira av upplösning GmbH panelen i hello åtkomstpanelen bör du hämta automatiskt inloggade tooyour SAML SSO för Jira resolution GmbH program.</span><span class="sxs-lookup"><span data-stu-id="26476-269">When you click hello SAML SSO for Jira by resolution GmbH tile in hello Access Panel, you should get automatically signed-on tooyour SAML SSO for Jira by resolution GmbH application.</span></span>
<span data-ttu-id="26476-270">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="26476-270">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="26476-271">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="26476-271">Additional resources</span></span>

* [<span data-ttu-id="26476-272">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="26476-272">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="26476-273">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="26476-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_203.png

