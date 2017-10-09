---
title: "Självstudier: Azure Active Directory-integrering med SAML SSO för antal samverkande resolution GmbH | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAML SSO för antal samverkande resolution GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6b47d483-d3a3-442d-b123-171e3f0f7486
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: fe50636709857ec49023e24bdc8c6cd8c58e3c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-confluence-by-resolution-gmbh"></a><span data-ttu-id="91067-103">Självstudier: Azure Active Directory-integrering med SAML SSO för antal samverkande resolution GmbH</span><span class="sxs-lookup"><span data-stu-id="91067-103">Tutorial: Azure Active Directory integration with SAML SSO for Confluence by resolution GmbH</span></span>

<span data-ttu-id="91067-104">I kursen får du lära dig hur toointegrate SAML SSO för antal samverkande resolution GmbH med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="91067-104">In this tutorial, you learn how toointegrate SAML SSO for Confluence by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="91067-105">Integrera SAML SSO för antal samverkande resolution GmbH med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="91067-105">Integrating SAML SSO for Confluence by resolution GmbH with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="91067-106">Du kan styra i Azure AD som har åtkomst tooSAML SSO för antal samverkande resolution GmbH</span><span class="sxs-lookup"><span data-stu-id="91067-106">You can control in Azure AD who has access tooSAML SSO for Confluence by resolution GmbH</span></span>
- <span data-ttu-id="91067-107">Du kan aktivera din användare tooautomatically get inloggade tooSAML SSO för antal samverkande resolution GmbH (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="91067-107">You can enable your users tooautomatically get signed-on tooSAML SSO for Confluence by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="91067-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91067-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="91067-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="91067-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91067-110">Krav</span><span class="sxs-lookup"><span data-stu-id="91067-110">Prerequisites</span></span>

<span data-ttu-id="91067-111">tooconfigure Azure AD-integrering med SAML SSO för antal samverkande resolution GmbH, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="91067-111">tooconfigure Azure AD integration with SAML SSO for Confluence by resolution GmbH, you need hello following items:</span></span>

- <span data-ttu-id="91067-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="91067-112">An Azure AD subscription</span></span>
- <span data-ttu-id="91067-113">En SAML SSO för växer samman av upplösning GmbH enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="91067-113">A SAML SSO for Confluence by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="91067-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="91067-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="91067-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="91067-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="91067-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="91067-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="91067-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="91067-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="91067-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="91067-118">Scenario description</span></span>
<span data-ttu-id="91067-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="91067-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="91067-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="91067-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="91067-121">Att lägga till SAML SSO för antal samverkande resolution GmbH från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91067-121">Adding SAML SSO for Confluence by resolution GmbH from hello gallery</span></span>
2. <span data-ttu-id="91067-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91067-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-confluence-by-resolution-gmbh-from-hello-gallery"></a><span data-ttu-id="91067-123">Att lägga till SAML SSO för antal samverkande resolution GmbH från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91067-123">Adding SAML SSO for Confluence by resolution GmbH from hello gallery</span></span>

<span data-ttu-id="91067-124">tooconfigure hello integrering av SAML SSO för antal samverkande resolution GmbH i Azure AD, behöver du tooadd SAML SSO för antal samverkande resolution GmbH hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="91067-124">tooconfigure hello integration of SAML SSO for Confluence by resolution GmbH into Azure AD, you need tooadd SAML SSO for Confluence by resolution GmbH from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="91067-125">**tooadd SAML SSO för antal samverkande resolution GmbH från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91067-125">**tooadd SAML SSO for Confluence by resolution GmbH from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="91067-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91067-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="91067-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="91067-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="91067-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="91067-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="91067-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91067-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="91067-133">Skriv i sökrutan hello **SAML SSO för antal samverkande resolution GmbH**.</span><span class="sxs-lookup"><span data-stu-id="91067-133">In hello search box, type **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_search.png)

5. <span data-ttu-id="91067-135">Markera hello resultat på panelen **SAML SSO för antal samverkande resolution GmbH**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="91067-135">In hello results panel, select **SAML SSO for Confluence by resolution GmbH**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="91067-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91067-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="91067-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAML SSO för antal samverkande resolution GmbH baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="91067-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="91067-139">För enkel inloggning toowork måste Azure AD tooknow användare vilka hello motsvarighet i SAML SSO för antal samverkande resolution GmbH är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="91067-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAML SSO for Confluence by resolution GmbH is tooa user in Azure AD.</span></span> <span data-ttu-id="91067-140">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i SAML SSO för antal samverkande resolution GmbH måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="91067-140">In other words, a link relationship between an Azure AD user and hello related user in SAML SSO for Confluence by resolution GmbH needs toobe established.</span></span>

<span data-ttu-id="91067-141">SAML SSO för antal samverkande resolution GmbH, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="91067-141">In SAML SSO for Confluence by resolution GmbH, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="91067-142">tooconfigure och testa Azure AD enkel inloggning med SAML SSO för antal samverkande resolution GmbH, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="91067-142">tooconfigure and test Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="91067-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="91067-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="91067-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91067-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="91067-145">**[Skapa en SAML SSO för växer samman av upplösning GmbH testanvändare](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)**  -toohave en motsvarighet för Britta Simon SAML SSO för antal samverkande resolution GmbH som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="91067-145">**[Creating a SAML SSO for Confluence by resolution GmbH test user](#creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user)** - toohave a counterpart of Britta Simon in SAML SSO for Confluence by resolution GmbH that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="91067-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91067-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="91067-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="91067-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="91067-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91067-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="91067-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din SAML SSO för antal samverkande resolution GmbH program.</span><span class="sxs-lookup"><span data-stu-id="91067-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAML SSO for Confluence by resolution GmbH application.</span></span>

<span data-ttu-id="91067-150">**tooconfigure Azure AD enkel inloggning med SAML SSO för antal samverkande resolution GmbH, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="91067-150">**tooconfigure Azure AD single sign-on with SAML SSO for Confluence by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="91067-151">I hello Azure-portalen på hello **SAML SSO för antal samverkande resolution GmbH** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="91067-151">In hello Azure portal, on hello **SAML SSO for Confluence by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="91067-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91067-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_samlbase.png)

3. <span data-ttu-id="91067-155">På hello **SAML SSO växer samman resolution GmbH domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="91067-155">On hello **SAML SSO for Confluence by resolution GmbH Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_1.png)

    <span data-ttu-id="91067-157">a.</span><span class="sxs-lookup"><span data-stu-id="91067-157">a.</span></span> <span data-ttu-id="91067-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="91067-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="91067-159">b.</span><span class="sxs-lookup"><span data-stu-id="91067-159">b.</span></span> <span data-ttu-id="91067-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="91067-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="91067-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="91067-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="91067-162">Om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="91067-162">If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_url_2.png)

    <span data-ttu-id="91067-164">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="91067-164">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="91067-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="91067-165">These values are not real.</span></span> <span data-ttu-id="91067-166">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="91067-166">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="91067-167">Kontakta [SAML SSO för antal samverkande resolution GmbH klienten supportteam](https://www.resolution.de/go/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="91067-167">Contact [SAML SSO for Confluence by resolution GmbH Client support team](https://www.resolution.de/go/support) tooget these values.</span></span> 

5. <span data-ttu-id="91067-168">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="91067-168">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_certificate.png) 

6. <span data-ttu-id="91067-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="91067-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_400.png)  
    
7. <span data-ttu-id="91067-172">Logga in tooyour i ett annat webbläsarfönster **SAML SSO för antal samverkande upplösning GmbH admin Portal** som administratör.</span><span class="sxs-lookup"><span data-stu-id="91067-172">In a different web browser window, log in tooyour **SAML SSO for Confluence by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="91067-173">Hovra över kugge och klicka på hello **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="91067-173">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon1.png)

9. <span data-ttu-id="91067-175">Du kan omdirigerade tooAdministrator sidan.</span><span class="sxs-lookup"><span data-stu-id="91067-175">You are redirected tooAdministrator Access page.</span></span> <span data-ttu-id="91067-176">Ange hello lösenord och klicka **Bekräfta** knappen.</span><span class="sxs-lookup"><span data-stu-id="91067-176">Enter hello password and click **Confirm** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon2.png)

10. <span data-ttu-id="91067-178">Under **ATLASSIAN MARKETPLACE** klickar du på **söka efter nya tillägg**.</span><span class="sxs-lookup"><span data-stu-id="91067-178">Under **ATLASSIAN MARKETPLACE** tab, click **Find new add-ons**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon.png)

11. <span data-ttu-id="91067-180">Sök **SAML enkel inloggning (SSO) för antal samverkande** och på **installera** knappen tooinstall hello ny SAML-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="91067-180">Search **SAML Single Sign On (SSO) for Confluence** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon7.png)

12. <span data-ttu-id="91067-182">hello plugin installationen startar.</span><span class="sxs-lookup"><span data-stu-id="91067-182">hello plugin installation will start.</span></span> <span data-ttu-id="91067-183">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="91067-183">Click **Close**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon8.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon9.png)

13. <span data-ttu-id="91067-186">Klicka på **Hantera**.</span><span class="sxs-lookup"><span data-stu-id="91067-186">Click **Manage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon10.png)
    
14. <span data-ttu-id="91067-188">Klicka på **konfigurera** tooconfigure hello nytt plugin-program.</span><span class="sxs-lookup"><span data-stu-id="91067-188">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon11.png)

15. <span data-ttu-id="91067-190">Den här nya plugin-program även finns under **användare och säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="91067-190">This new plugin can also be found under **USERS & SECURITY** tab.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon3.png)
    
16. <span data-ttu-id="91067-192">På **SAML SingleSignOn Plugin-konfiguration** klickar du på **lägga till ytterligare identitetsleverantör** knappen tooconfigure hello inställningar av identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="91067-192">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button tooconfigure hello settings of Identity Provider.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon4.png)

17. <span data-ttu-id="91067-194">Utför följande steg på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="91067-194">Perform following steps on this page:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon5.png)
 
    <span data-ttu-id="91067-196">a.</span><span class="sxs-lookup"><span data-stu-id="91067-196">a.</span></span> <span data-ttu-id="91067-197">Lägg till **namn** av hello identitetsprovider (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="91067-197">Add **Name** of hello Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="91067-198">b.</span><span class="sxs-lookup"><span data-stu-id="91067-198">b.</span></span> <span data-ttu-id="91067-199">Lägg till **beskrivning** av hello identitetsprovider (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="91067-199">Add **Description** of hello Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="91067-200">c.</span><span class="sxs-lookup"><span data-stu-id="91067-200">c.</span></span> <span data-ttu-id="91067-201">Klicka på **XML** och välj hello **Metadata** -fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="91067-201">Click **XML** and select hello **Metadata** file that you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="91067-202">d.</span><span class="sxs-lookup"><span data-stu-id="91067-202">d.</span></span> <span data-ttu-id="91067-203">Klicka på **belastningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="91067-203">Click **Load** button.</span></span>

    <span data-ttu-id="91067-204">e.</span><span class="sxs-lookup"><span data-stu-id="91067-204">e.</span></span> <span data-ttu-id="91067-205">Den läser hello IdP metadata och fyller hello fält som är markerade i hello skärmbild.</span><span class="sxs-lookup"><span data-stu-id="91067-205">It reads hello IdP metadata and populates hello fields as highlighted in hello screenshot.</span></span>   
18. <span data-ttu-id="91067-206">Klicka på **Spara inställningar** knappen toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="91067-206">Click **Save settings** button toosave hello settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="91067-208">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="91067-208">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="91067-209">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="91067-209">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="91067-210">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="91067-210">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="91067-211">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="91067-211">Creating an Azure AD test user</span></span>
<span data-ttu-id="91067-212">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91067-212">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="91067-214">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91067-214">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="91067-215">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91067-215">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="91067-217">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="91067-217">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="91067-219">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91067-219">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="91067-221">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91067-221">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssoconfluence-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="91067-223">a.</span><span class="sxs-lookup"><span data-stu-id="91067-223">a.</span></span> <span data-ttu-id="91067-224">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="91067-224">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="91067-225">b.</span><span class="sxs-lookup"><span data-stu-id="91067-225">b.</span></span> <span data-ttu-id="91067-226">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="91067-226">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="91067-227">c.</span><span class="sxs-lookup"><span data-stu-id="91067-227">c.</span></span> <span data-ttu-id="91067-228">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="91067-228">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="91067-229">d.</span><span class="sxs-lookup"><span data-stu-id="91067-229">d.</span></span> <span data-ttu-id="91067-230">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="91067-230">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-confluence-by-resolution-gmbh-test-user"></a><span data-ttu-id="91067-231">Skapa en SAML SSO för växer samman av upplösning GmbH testanvändare</span><span class="sxs-lookup"><span data-stu-id="91067-231">Creating a SAML SSO for Confluence by resolution GmbH test user</span></span>

<span data-ttu-id="91067-232">tooenable Azure AD-användare toolog i tooSAML enkel inloggning för antal samverkande resolution GmbH de måste etableras i SAML SSO för antal samverkande resolution GmbH.</span><span class="sxs-lookup"><span data-stu-id="91067-232">tooenable Azure AD users toolog in tooSAML SSO for Confluence by resolution GmbH, they must be provisioned into SAML SSO for Confluence by resolution GmbH.</span></span>  
<span data-ttu-id="91067-233">SAML SSO för antal samverkande resolution GmbH är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="91067-233">In SAML SSO for Confluence by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="91067-234">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="91067-234">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="91067-235">Logga in tooyour SAML SSO för växer samman av upplösning GmbH företagets plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="91067-235">Log in tooyour SAML SSO for Confluence by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="91067-236">Hovra över kugge och klicka på hello **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="91067-236">Hover on cog and click hello **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssoconfluence-tutorial/user1.png) 

3. <span data-ttu-id="91067-238">Under avsnittet för användare, klickar du på **lägga till användare** fliken. På hello **”Lägg till en användare”** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91067-238">Under Users section, click **Add users** tab. On hello **“Add a User”** dialog page, perform hello following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssoconfluence-tutorial/user2.png) 

    <span data-ttu-id="91067-240">a.</span><span class="sxs-lookup"><span data-stu-id="91067-240">a.</span></span> <span data-ttu-id="91067-241">I hello **användarnamn** textruta hello e-post för användare som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91067-241">In hello **Username** textbox, type hello email of user like Britta Simon.</span></span>

    <span data-ttu-id="91067-242">b.</span><span class="sxs-lookup"><span data-stu-id="91067-242">b.</span></span> <span data-ttu-id="91067-243">I hello **fullständiga namn** textruta typen hello användarens fullständiga namn som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91067-243">In hello **Full Name** textbox, type hello full name of user like Britta Simon.</span></span>

    <span data-ttu-id="91067-244">c.</span><span class="sxs-lookup"><span data-stu-id="91067-244">c.</span></span> <span data-ttu-id="91067-245">I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="91067-245">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="91067-246">d.</span><span class="sxs-lookup"><span data-stu-id="91067-246">d.</span></span> <span data-ttu-id="91067-247">I hello **lösenord** textruta hello lösenordstyp för Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91067-247">In hello **Password** textbox, type hello password for Britta Simon.</span></span>

    <span data-ttu-id="91067-248">e.</span><span class="sxs-lookup"><span data-stu-id="91067-248">e.</span></span> <span data-ttu-id="91067-249">Klicka på **Bekräfta lösenord** hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="91067-249">Click **Confirm Password** reenter hello password.</span></span>
    
    <span data-ttu-id="91067-250">f.</span><span class="sxs-lookup"><span data-stu-id="91067-250">f.</span></span> <span data-ttu-id="91067-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="91067-251">Click **Add** button.</span></span>    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="91067-252">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="91067-252">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="91067-253">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAML SSO för antal samverkande resolution GmbH.</span><span class="sxs-lookup"><span data-stu-id="91067-253">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAML SSO for Confluence by resolution GmbH.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="91067-255">**tooassign Britta Simon tooSAML SSO för antal samverkande resolution GmbH, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="91067-255">**tooassign Britta Simon tooSAML SSO for Confluence by resolution GmbH, perform hello following steps:**</span></span>

1. <span data-ttu-id="91067-256">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="91067-256">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="91067-258">Välj i listan med program hello **SAML SSO för antal samverkande resolution GmbH**.</span><span class="sxs-lookup"><span data-stu-id="91067-258">In hello applications list, select **SAML SSO for Confluence by resolution GmbH**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_samlssoconfluence_app.png) 

3. <span data-ttu-id="91067-260">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="91067-260">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="91067-262">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="91067-262">Click **Add** button.</span></span> <span data-ttu-id="91067-263">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91067-263">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="91067-265">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="91067-265">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="91067-266">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91067-266">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="91067-267">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91067-267">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="91067-268">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91067-268">Testing single sign-on</span></span>

<span data-ttu-id="91067-269">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="91067-269">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="91067-270">När du klickar på hello SAML SSO för växer samman av upplösning GmbH panelen i hello åtkomstpanelen bör du hämta automatiskt inloggade tooyour SAML SSO för antal samverkande resolution GmbH program.</span><span class="sxs-lookup"><span data-stu-id="91067-270">When you click hello SAML SSO for Confluence by resolution GmbH tile in hello Access Panel, you should get automatically signed-on tooyour SAML SSO for Confluence by resolution GmbH application.</span></span>
<span data-ttu-id="91067-271">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="91067-271">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="91067-272">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="91067-272">Additional resources</span></span>

* [<span data-ttu-id="91067-273">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="91067-273">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="91067-274">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="91067-274">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssoconfluence-tutorial/tutorial_general_203.png

