---
title: "Självstudier: Azure Active Directory-integrering med Kantega SSO för antal samverkande | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Kantega SSO för växer samman."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d0d99c14-a6ca-45f2-bb84-633126095e7a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: b35eb8757e41e86228a3a9ee10869086cc801c9b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-confluence"></a><span data-ttu-id="941db-103">Självstudier: Azure Active Directory-integrering med Kantega SSO för växer samman</span><span class="sxs-lookup"><span data-stu-id="941db-103">Tutorial: Azure Active Directory integration with Kantega SSO for Confluence</span></span>

<span data-ttu-id="941db-104">I kursen får du lära dig hur toointegrate Kantega SSO för växer samman med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="941db-104">In this tutorial, you learn how toointegrate Kantega SSO for Confluence with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="941db-105">Integrera Kantega SSO för växer samman med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="941db-105">Integrating Kantega SSO for Confluence with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="941db-106">Du kan styra i Azure AD som har åtkomst tooKantega SSO för växer samman</span><span class="sxs-lookup"><span data-stu-id="941db-106">You can control in Azure AD who has access tooKantega SSO for Confluence</span></span>
- <span data-ttu-id="941db-107">Du kan aktivera din användare tooautomatically get inloggade tooKantega SSO för antal samverkande (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="941db-107">You can enable your users tooautomatically get signed-on tooKantega SSO for Confluence (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="941db-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="941db-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="941db-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="941db-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="941db-110">Krav</span><span class="sxs-lookup"><span data-stu-id="941db-110">Prerequisites</span></span>

<span data-ttu-id="941db-111">tooconfigure Azure AD-integrering med Kantega SSO för antal samverkande måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="941db-111">tooconfigure Azure AD integration with Kantega SSO for Confluence, you need hello following items:</span></span>

- <span data-ttu-id="941db-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="941db-112">An Azure AD subscription</span></span>
- <span data-ttu-id="941db-113">En Kantega SSO för antal samverkande enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="941db-113">A Kantega SSO for Confluence single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="941db-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="941db-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="941db-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="941db-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="941db-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="941db-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="941db-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="941db-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="941db-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="941db-118">Scenario description</span></span>
<span data-ttu-id="941db-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="941db-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="941db-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="941db-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="941db-121">Att lägga till Kantega SSO för växer samman från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="941db-121">Adding Kantega SSO for Confluence from hello gallery</span></span>
2. <span data-ttu-id="941db-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="941db-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kantega-sso-for-confluence-from-hello-gallery"></a><span data-ttu-id="941db-123">Att lägga till Kantega SSO för växer samman från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="941db-123">Adding Kantega SSO for Confluence from hello gallery</span></span>
<span data-ttu-id="941db-124">tooconfigure hello integrering av Kantega SSO för antal samverkande till Azure AD, behöver du tooadd Kantega SSO för antal samverkande hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="941db-124">tooconfigure hello integration of Kantega SSO for Confluence into Azure AD, you need tooadd Kantega SSO for Confluence from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="941db-125">**tooadd Kantega SSO för växer samman från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="941db-125">**tooadd Kantega SSO for Confluence from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="941db-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="941db-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="941db-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="941db-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="941db-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="941db-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="941db-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="941db-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="941db-133">Skriv i sökrutan hello **Kantega SSO för antal samverkande**.</span><span class="sxs-lookup"><span data-stu-id="941db-133">In hello search box, type **Kantega SSO for Confluence**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_search.png)

5. <span data-ttu-id="941db-135">Markera hello resultat på panelen **Kantega SSO för antal samverkande**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="941db-135">In hello results panel, select **Kantega SSO for Confluence**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="941db-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="941db-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="941db-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kantega SSO för antal samverkande baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="941db-138">In this section, you configure and test Azure AD single sign-on with Kantega SSO for Confluence based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="941db-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Kantega SSO för antal samverkande är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="941db-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Kantega SSO for Confluence is tooa user in Azure AD.</span></span> <span data-ttu-id="941db-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren Kantega SSO för antal samverkande toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="941db-140">In other words, a link relationship between an Azure AD user and hello related user in Kantega SSO for Confluence needs toobe established.</span></span>

<span data-ttu-id="941db-141">Kantega SSO för antal samverkande, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="941db-141">In Kantega SSO for Confluence, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="941db-142">tooconfigure och testa Azure AD enkel inloggning med Kantega SSO för antal samverkande, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="941db-142">tooconfigure and test Azure AD single sign-on with Kantega SSO for Confluence, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="941db-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="941db-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="941db-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="941db-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="941db-145">**[Skapa en Kantega SSO för antal samverkande testanvändare](#creating-a-kantega-sso-for-confluence-test-user)**  -toohave en motsvarighet för Britta Simon Kantega SSO för antal samverkande som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="941db-145">**[Creating a Kantega SSO for Confluence test user](#creating-a-kantega-sso-for-confluence-test-user)** - toohave a counterpart of Britta Simon in Kantega SSO for Confluence that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="941db-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="941db-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="941db-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="941db-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="941db-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="941db-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="941db-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Kantega SSO för antal samverkande program.</span><span class="sxs-lookup"><span data-stu-id="941db-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Kantega SSO for Confluence application.</span></span>

<span data-ttu-id="941db-150">**tooconfigure Azure AD enkel inloggning med Kantega SSO för antal samverkande, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="941db-150">**tooconfigure Azure AD single sign-on with Kantega SSO for Confluence, perform hello following steps:**</span></span>

1. <span data-ttu-id="941db-151">I hello Azure-portalen på hello **Kantega SSO för antal samverkande** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="941db-151">In hello Azure portal, on hello **Kantega SSO for Confluence** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="941db-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="941db-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_samlbase.png)

3. <span data-ttu-id="941db-155">I **IDP** initierade läge på hello **Kantega SSO växer samman domänen och URL: er** avsnittet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="941db-155">In **IDP** initiated mode, on hello **Kantega SSO for Confluence Domain and URLs** section perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url1.png)

    <span data-ttu-id="941db-157">a.</span><span class="sxs-lookup"><span data-stu-id="941db-157">a.</span></span> <span data-ttu-id="941db-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="941db-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    <span data-ttu-id="941db-159">b.</span><span class="sxs-lookup"><span data-stu-id="941db-159">b.</span></span> <span data-ttu-id="941db-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="941db-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

4. <span data-ttu-id="941db-161">I **SP** initierade läge, kontrollera **visa avancerade inställningar för URL: en** och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="941db-161">In **SP** initiated mode, check **Show advanced URL settings** and perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_url2.png)

    <span data-ttu-id="941db-163">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span><span class="sxs-lookup"><span data-stu-id="941db-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="941db-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="941db-164">These values are not real.</span></span> <span data-ttu-id="941db-165">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="941db-165">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="941db-166">Dessa värden tas emot under hello konfigurationen av antal samverkande plugin som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="941db-166">These values are received during hello configuration of Confluence plugin, which is explained later in hello tutorial.</span></span>

5. <span data-ttu-id="941db-167">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="941db-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_certificate.png) 

6. <span data-ttu-id="941db-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="941db-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="941db-171">Logga in tooyour i ett annat webbläsarfönster **växer samman administrationsportalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="941db-171">In a different web browser window, log in tooyour **Confluence admin portal** as an administrator.</span></span>

8. <span data-ttu-id="941db-172">Hovra över kugge och klicka på hello **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="941db-172">Hover on cog and click hello **Add-ons**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon1.png)

9. <span data-ttu-id="941db-174">Under **ATLASSIAN MARKETPLACE** klickar du på **söka efter nya tillägg**.</span><span class="sxs-lookup"><span data-stu-id="941db-174">Under **ATLASSIAN MARKETPLACE** tab, click **Find new add-ons**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon.png)

10. <span data-ttu-id="941db-176">Sök **Kantega SSO för antal samverkande SAML Kerberos** och på **installera** knappen tooinstall hello ny SAML-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="941db-176">Search **Kantega SSO for Confluence SAML Kerberos** and click **Install** button tooinstall hello new SAML plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon2.png)

11. <span data-ttu-id="941db-178">hello plugin-installationen startar.</span><span class="sxs-lookup"><span data-stu-id="941db-178">hello plugin installation starts.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon3.png)

12. <span data-ttu-id="941db-180">När hello installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="941db-180">Once hello installation is complete.</span></span> <span data-ttu-id="941db-181">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="941db-181">Click **Close**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon33.png)

13. <span data-ttu-id="941db-183">Klicka på **Hantera**.</span><span class="sxs-lookup"><span data-stu-id="941db-183">Click **Manage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon34.png)
    
14. <span data-ttu-id="941db-185">Klicka på **konfigurera** tooconfigure hello nytt plugin-program.</span><span class="sxs-lookup"><span data-stu-id="941db-185">Click **Configure** tooconfigure hello new plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon35.png)

15. <span data-ttu-id="941db-187">Den här nya plugin-program även finns under **användare och säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="941db-187">This new plugin can also be found under **USERS & SECURITY** tab.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon36.png)
    
16. <span data-ttu-id="941db-189">I hello **SAML** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="941db-189">In hello **SAML** section.</span></span> <span data-ttu-id="941db-190">Välj **Azure Active Directory (AD Azure)** från hello **Lägg till identitetsleverantör** listrutan.</span><span class="sxs-lookup"><span data-stu-id="941db-190">Select **Azure Active Directory (Azure AD)** from hello **Add identity provider** dropdown.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon4.png)

17. <span data-ttu-id="941db-192">Välj prenumerationsnivån som **grundläggande**.</span><span class="sxs-lookup"><span data-stu-id="941db-192">Select subscription level as **Basic**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon5.png)       

18. <span data-ttu-id="941db-194">På hello **appegenskaper** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="941db-194">On hello **App properties** section, perform following steps:</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon6.png)

    <span data-ttu-id="941db-196">a.</span><span class="sxs-lookup"><span data-stu-id="941db-196">a.</span></span> <span data-ttu-id="941db-197">Kopiera hello **App-ID URI** värde och använda det som **identifierare, Reply URL och inloggnings-URL** på hello **Kantega SSO växer samman domänen och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="941db-197">Copy hello **App ID URI** value and use it as **Identifier, Reply URL, and Sign-On URL** on hello **Kantega SSO for Confluence Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="941db-198">b.</span><span class="sxs-lookup"><span data-stu-id="941db-198">b.</span></span> <span data-ttu-id="941db-199">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="941db-199">Click **Next**.</span></span>

19. <span data-ttu-id="941db-200">På hello **Metadata importera** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="941db-200">On hello **Metadata import** section, perform following steps:</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon7.png)

    <span data-ttu-id="941db-202">a.</span><span class="sxs-lookup"><span data-stu-id="941db-202">a.</span></span> <span data-ttu-id="941db-203">Välj **metadatafil på datorn**, och överföra metadata-fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="941db-203">Select **Metadata file on my computer**, and upload metadata file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="941db-204">b.</span><span class="sxs-lookup"><span data-stu-id="941db-204">b.</span></span> <span data-ttu-id="941db-205">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="941db-205">Click **Next**.</span></span>

20. <span data-ttu-id="941db-206">På hello **och enkel inloggning** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="941db-206">On hello **Name and SSO location** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon8.png)
    
    <span data-ttu-id="941db-208">a.</span><span class="sxs-lookup"><span data-stu-id="941db-208">a.</span></span> <span data-ttu-id="941db-209">Lägg till namnet på hello identitetsleverantör i **identitet providernamn** textruta (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="941db-209">Add Name of hello Identity Provider in **Identity provider name** textbox (e.g Azure AD).</span></span>

    <span data-ttu-id="941db-210">b.</span><span class="sxs-lookup"><span data-stu-id="941db-210">b.</span></span> <span data-ttu-id="941db-211">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="941db-211">Click **Next**.</span></span>

21. <span data-ttu-id="941db-212">Kontrollera hello signeringscertifikat och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="941db-212">Verify hello Signing certificate and click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon9.png)

22. <span data-ttu-id="941db-214">På hello **växer samman användarkonton** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="941db-214">On hello **Confluence user accounts** section, perform following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon10.png)

    <span data-ttu-id="941db-216">a.</span><span class="sxs-lookup"><span data-stu-id="941db-216">a.</span></span> <span data-ttu-id="941db-217">Välj **skapa användare i antal Samverkandes interna katalogen om det behövs** och ange hello rätt namn i hello gruppen för användare (kan vara flera Nej.</span><span class="sxs-lookup"><span data-stu-id="941db-217">Select **Create users in Confluence's internal Directory if needed** and enter hello appropriate name of hello group for users (can be multiple no.</span></span> <span data-ttu-id="941db-218">av (grupper avgränsade med kommatecken).</span><span class="sxs-lookup"><span data-stu-id="941db-218">of groups separated by comma).</span></span>

    <span data-ttu-id="941db-219">b.</span><span class="sxs-lookup"><span data-stu-id="941db-219">b.</span></span> <span data-ttu-id="941db-220">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="941db-220">Click **Next**.</span></span>

23. <span data-ttu-id="941db-221">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="941db-221">Click **Finish**.</span></span>   

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon11.png)

24. <span data-ttu-id="941db-223">På hello **kända domäner för Azure AD** avsnittet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="941db-223">On hello **Known domains for Azure AD** section, perform following steps:</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/addon12.png)

    <span data-ttu-id="941db-225">a.</span><span class="sxs-lookup"><span data-stu-id="941db-225">a.</span></span> <span data-ttu-id="941db-226">Välj **kända domäner** hello vänstra panelen av hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="941db-226">Select **Known domains** from hello left panel of hello page.</span></span>

    <span data-ttu-id="941db-227">b.</span><span class="sxs-lookup"><span data-stu-id="941db-227">b.</span></span> <span data-ttu-id="941db-228">Ange domännamnet i hello **kända domäner** textruta.</span><span class="sxs-lookup"><span data-stu-id="941db-228">Enter domain name in hello **Known domains** textbox.</span></span>

    <span data-ttu-id="941db-229">c.</span><span class="sxs-lookup"><span data-stu-id="941db-229">c.</span></span> <span data-ttu-id="941db-230">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="941db-230">Click **Save**.</span></span> 

> [!TIP]
> <span data-ttu-id="941db-231">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="941db-231">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="941db-232">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="941db-232">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="941db-233">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="941db-233">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="941db-234">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="941db-234">Creating an Azure AD test user</span></span>
<span data-ttu-id="941db-235">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="941db-235">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="941db-237">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="941db-237">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="941db-238">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="941db-238">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="941db-240">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="941db-240">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="941db-242">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="941db-242">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="941db-244">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="941db-244">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kantegassoforconfluence-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="941db-246">a.</span><span class="sxs-lookup"><span data-stu-id="941db-246">a.</span></span> <span data-ttu-id="941db-247">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="941db-247">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="941db-248">b.</span><span class="sxs-lookup"><span data-stu-id="941db-248">b.</span></span> <span data-ttu-id="941db-249">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="941db-249">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="941db-250">c.</span><span class="sxs-lookup"><span data-stu-id="941db-250">c.</span></span> <span data-ttu-id="941db-251">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="941db-251">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="941db-252">d.</span><span class="sxs-lookup"><span data-stu-id="941db-252">d.</span></span> <span data-ttu-id="941db-253">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="941db-253">Click **Create**.</span></span>
 
### <a name="creating-a-kantega-sso-for-confluence-test-user"></a><span data-ttu-id="941db-254">Skapa en Kantega SSO för antal samverkande testanvändare</span><span class="sxs-lookup"><span data-stu-id="941db-254">Creating a Kantega SSO for Confluence test user</span></span>

<span data-ttu-id="941db-255">tooenable Azure AD-användare toolog i tooConfluence, måste de etableras i växer samman.</span><span class="sxs-lookup"><span data-stu-id="941db-255">tooenable Azure AD users toolog in tooConfluence, they must be provisioned into Confluence.</span></span> <span data-ttu-id="941db-256">Hello gäller Kantega SSO för antal samverkande är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="941db-256">In hello case of Kantega SSO for Confluence, provisioning is a manual task.</span></span>

<span data-ttu-id="941db-257">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="941db-257">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="941db-258">Logga in tooyour Kantega SSO för antal samverkande företagets platsen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="941db-258">Log in tooyour Kantega SSO for Confluence company site as an administrator.</span></span>

2. <span data-ttu-id="941db-259">Hovra över kugge och klicka på hello **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="941db-259">Hover on cog and click hello **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforconfluence-tutorial/user1.png) 

3. <span data-ttu-id="941db-261">Under avsnittet för användare, klickar du på **Lägg till användare** fliken. På hello **”Lägg till en användare”** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="941db-261">Under Users section, click **Add Users** tab. On hello **“Add a User”** dialog page, perform hello following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-kantegassoforconfluence-tutorial/user2.png) 

    <span data-ttu-id="941db-263">a.</span><span class="sxs-lookup"><span data-stu-id="941db-263">a.</span></span> <span data-ttu-id="941db-264">I hello **användarnamn** textruta hello e-post för användare som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="941db-264">In hello **Username** textbox, type hello email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="941db-265">b.</span><span class="sxs-lookup"><span data-stu-id="941db-265">b.</span></span> <span data-ttu-id="941db-266">I hello **fullständiga namn** textruta typen hello användarens fullständiga namn som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="941db-266">In hello **Full Name** textbox, type hello full name of user like Britta Simon.</span></span>

    <span data-ttu-id="941db-267">c.</span><span class="sxs-lookup"><span data-stu-id="941db-267">c.</span></span> <span data-ttu-id="941db-268">I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="941db-268">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="941db-269">d.</span><span class="sxs-lookup"><span data-stu-id="941db-269">d.</span></span> <span data-ttu-id="941db-270">I hello **lösenord** textruta typen hello lösenord för användaren.</span><span class="sxs-lookup"><span data-stu-id="941db-270">In hello **Password** textbox, type hello password for user.</span></span>

    <span data-ttu-id="941db-271">e.</span><span class="sxs-lookup"><span data-stu-id="941db-271">e.</span></span> <span data-ttu-id="941db-272">Klicka på **Bekräfta lösenord** hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="941db-272">Click **Confirm Password** reenter hello password.</span></span>
    
    <span data-ttu-id="941db-273">f.</span><span class="sxs-lookup"><span data-stu-id="941db-273">f.</span></span> <span data-ttu-id="941db-274">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="941db-274">Click **Add** button.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="941db-275">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="941db-275">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="941db-276">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooKantega SSO för växer samman.</span><span class="sxs-lookup"><span data-stu-id="941db-276">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooKantega SSO for Confluence.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="941db-278">**tooassign Britta Simon tooKantega SSO för antal samverkande, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="941db-278">**tooassign Britta Simon tooKantega SSO for Confluence, perform hello following steps:**</span></span>

1. <span data-ttu-id="941db-279">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="941db-279">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="941db-281">Välj i listan med program hello **Kantega SSO för antal samverkande**.</span><span class="sxs-lookup"><span data-stu-id="941db-281">In hello applications list, select **Kantega SSO for Confluence**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_kantegassoforconfluence_app.png) 

3. <span data-ttu-id="941db-283">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="941db-283">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="941db-285">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="941db-285">Click **Add** button.</span></span> <span data-ttu-id="941db-286">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="941db-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="941db-288">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="941db-288">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="941db-289">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="941db-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="941db-290">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="941db-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="941db-291">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="941db-291">Testing single sign-on</span></span>

<span data-ttu-id="941db-292">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="941db-292">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="941db-293">När du klickar på hello Kantega SSO för antal samverkande panelen i hello åtkomstpanelen bör du hämta automatiskt inloggade tooyour Kantega SSO för antal samverkande program.</span><span class="sxs-lookup"><span data-stu-id="941db-293">When you click hello Kantega SSO for Confluence tile in hello Access Panel, you should get automatically signed-on tooyour Kantega SSO for Confluence application.</span></span>
<span data-ttu-id="941db-294">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="941db-294">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="941db-295">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="941db-295">Additional resources</span></span>

* [<span data-ttu-id="941db-296">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="941db-296">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="941db-297">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="941db-297">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kantegassoforconfluence-tutorial/tutorial_general_203.png

