---
title: "Självstudier: Azure Active Directory-integrering med Jobscience | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Jobscience."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 4a4c78aad6d324795a15a9569542afc23b4716d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a><span data-ttu-id="bb0a1-103">Självstudier: Azure Active Directory-integrering med Jobscience</span><span class="sxs-lookup"><span data-stu-id="bb0a1-103">Tutorial: Azure Active Directory integration with Jobscience</span></span>

<span data-ttu-id="bb0a1-104">I kursen får du lära dig hur toointegrate Jobscience med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bb0a1-104">In this tutorial, you learn how toointegrate Jobscience with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bb0a1-105">Integrera Jobscience med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-105">Integrating Jobscience with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bb0a1-106">Du kan styra i Azure AD som har åtkomst till tooJobscience</span><span class="sxs-lookup"><span data-stu-id="bb0a1-106">You can control in Azure AD who has access tooJobscience</span></span>
- <span data-ttu-id="bb0a1-107">Du kan aktivera din användare tooautomatically get inloggade tooJobscience (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bb0a1-107">You can enable your users tooautomatically get signed-on tooJobscience (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bb0a1-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bb0a1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bb0a1-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bb0a1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb0a1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bb0a1-110">Prerequisites</span></span>

<span data-ttu-id="bb0a1-111">tooconfigure Azure AD-integrering med Jobscience, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-111">tooconfigure Azure AD integration with Jobscience, you need hello following items:</span></span>

- <span data-ttu-id="bb0a1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bb0a1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bb0a1-113">En Jobscience enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="bb0a1-113">A Jobscience single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bb0a1-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bb0a1-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bb0a1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bb0a1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bb0a1-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bb0a1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bb0a1-118">Scenario description</span></span>
<span data-ttu-id="bb0a1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bb0a1-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bb0a1-121">Att lägga till Jobscience från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bb0a1-121">Adding Jobscience from hello gallery</span></span>
2. <span data-ttu-id="bb0a1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb0a1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobscience-from-hello-gallery"></a><span data-ttu-id="bb0a1-123">Att lägga till Jobscience från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bb0a1-123">Adding Jobscience from hello gallery</span></span>
<span data-ttu-id="bb0a1-124">tooconfigure hello integrering av Jobscience i Azure AD, behöver du tooadd Jobscience hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-124">tooconfigure hello integration of Jobscience into Azure AD, you need tooadd Jobscience from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bb0a1-125">**tooadd Jobscience från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bb0a1-125">**tooadd Jobscience from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb0a1-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bb0a1-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bb0a1-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bb0a1-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bb0a1-133">Skriv i sökrutan hello **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-133">In hello search box, type **Jobscience**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_search.png)

5. <span data-ttu-id="bb0a1-135">Markera hello resultat på panelen **Jobscience**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-135">In hello results panel, select **Jobscience**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bb0a1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb0a1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bb0a1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Jobscience baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-138">In this section, you configure and test Azure AD single sign-on with Jobscience based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bb0a1-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Jobscience är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Jobscience is tooa user in Azure AD.</span></span> <span data-ttu-id="bb0a1-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Jobscience toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-140">In other words, a link relationship between an Azure AD user and hello related user in Jobscience needs toobe established.</span></span>

<span data-ttu-id="bb0a1-141">I Jobscience, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-141">In Jobscience, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bb0a1-142">tooconfigure och testa Azure AD enkel inloggning med Jobscience, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-142">tooconfigure and test Azure AD single sign-on with Jobscience, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bb0a1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bb0a1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bb0a1-145">**[Skapa en testanvändare Jobscience](#creating-a-jobscience-test-user)**  -toohave en motsvarighet för Britta Simon i Jobscience som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-145">**[Creating a Jobscience test user](#creating-a-jobscience-test-user)** - toohave a counterpart of Britta Simon in Jobscience that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bb0a1-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bb0a1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bb0a1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb0a1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bb0a1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Jobscience program.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Jobscience application.</span></span>

<span data-ttu-id="bb0a1-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Jobscience:**</span><span class="sxs-lookup"><span data-stu-id="bb0a1-150">**tooconfigure Azure AD single sign-on with Jobscience, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb0a1-151">I hello Azure-portalen på hello **Jobscience** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-151">In hello Azure portal, on hello **Jobscience** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bb0a1-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_samlbase.png)

3. <span data-ttu-id="bb0a1-155">På hello **Jobscience domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-155">On hello **Jobscience Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_url.png)

    <span data-ttu-id="bb0a1-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`http://<company name>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="bb0a1-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:  `http://<company name>.my.salesforce.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="bb0a1-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-158">This value is not real.</span></span> <span data-ttu-id="bb0a1-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="bb0a1-160">Hämta det här värdet genom att [Jobscience klienten supportteamet](https://www.jobscience.com/support) eller från hello SSO profil skapas som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-160">Get this value by [Jobscience Client support team](https://www.jobscience.com/support) or from hello SSO profile you will create which is explained later in hello tutorial.</span></span> 
 
4. <span data-ttu-id="bb0a1-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_certificate.png) 

5. <span data-ttu-id="bb0a1-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bb0a1-165">På hello **Jobscience Configuration** klickar du på **konfigurera Jobscience** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-165">On hello **Jobscience Configuration** section, click **Configure Jobscience** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="bb0a1-166">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="bb0a1-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_configure.png) 

7. <span data-ttu-id="bb0a1-168">Logga in tooyour Jobscience företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-168">Log in tooyour Jobscience company site as an administrator.</span></span>

8. <span data-ttu-id="bb0a1-169">Gå för**installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-169">Go too**Setup**.</span></span>
   
   <span data-ttu-id="bb0a1-170">![Installationsprogrammet](./media/active-directory-saas-jobscience-tutorial/IC784358.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-170">![Setup](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")</span></span>

9. <span data-ttu-id="bb0a1-171">Hello vänstra navigeringsfönstret i hello **administrera** klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello  **Min domän** sidan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-171">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
   
   <span data-ttu-id="bb0a1-172">![Min domän](./media/active-directory-saas-jobscience-tutorial/ic767825.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-172">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="bb0a1-173">tooverify som din domän har angetts korrekt, kontrollera att den är i ”**steg 4 distribueras tooUsers**” och granska din ”**Mina Domäninställningar**”.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-173">tooverify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed tooUsers**” and review your “**My Domain Settings**”.</span></span>

    <span data-ttu-id="bb0a1-174">![Domän distribuerat tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "domän distribueras tooUser")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-174">![Domain Deployed tooUser](./media/active-directory-saas-jobscience-tutorial/ic784377.png "Domain Deployed tooUser")</span></span>

11. <span data-ttu-id="bb0a1-175">Klicka på på hello Jobscience företagets **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-175">On hello Jobscience company site, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="bb0a1-176">![Säkerhetsåtgärder](./media/active-directory-saas-jobscience-tutorial/ic784364.png "säkerhetsåtgärder")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-176">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784364.png "Security Controls")</span></span>

12. <span data-ttu-id="bb0a1-177">I hello **inställningar för enkel inloggning** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-177">In hello **Single Sign-On Settings** section, perform hello following steps:</span></span>
    
    <span data-ttu-id="bb0a1-178">![Enkel inloggning inställningar](./media/active-directory-saas-jobscience-tutorial/ic781026.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-178">![Single Sign-On Settings](./media/active-directory-saas-jobscience-tutorial/ic781026.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="bb0a1-179">a.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-179">a.</span></span> <span data-ttu-id="bb0a1-180">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-180">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="bb0a1-181">b.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-181">b.</span></span> <span data-ttu-id="bb0a1-182">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-182">Click **New**.</span></span>

13. <span data-ttu-id="bb0a1-183">På hello **SAML enkel inloggning inställningen redigera** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-183">On hello **SAML Single Sign-On Setting Edit** dialog, perform hello following steps:</span></span>
    
    <span data-ttu-id="bb0a1-184">![SAML enkel inloggning inställningen](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML enkel inloggning inställningen")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-184">![SAML Single Sign-On Setting](./media/active-directory-saas-jobscience-tutorial/ic784365.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="bb0a1-185">a.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-185">a.</span></span> <span data-ttu-id="bb0a1-186">I hello **namn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-186">In hello **Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="bb0a1-187">b.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-187">b.</span></span> <span data-ttu-id="bb0a1-188">I **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-188">In **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bb0a1-189">c.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-189">c.</span></span> <span data-ttu-id="bb0a1-190">I hello **enhets-Id** textruta typ`https://salesforce-jobscience.com`</span><span class="sxs-lookup"><span data-stu-id="bb0a1-190">In hello **Entity Id** textbox, type `https://salesforce-jobscience.com`</span></span>

    <span data-ttu-id="bb0a1-191">d.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-191">d.</span></span> <span data-ttu-id="bb0a1-192">Klicka på **Bläddra** tooupload Azure AD-certifikat.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-192">Click **Browse** tooupload your Azure AD certificate.</span></span>

    <span data-ttu-id="bb0a1-193">e.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-193">e.</span></span> <span data-ttu-id="bb0a1-194">Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-194">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>

    <span data-ttu-id="bb0a1-195">f.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-195">f.</span></span> <span data-ttu-id="bb0a1-196">Som **SAML identitet plats**väljer **identitet är i hello NameIdentfier elementet i hello ämne instruktionen**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-196">As **SAML Identity Location**, select **Identity is in hello NameIdentfier element of hello Subject statement**.</span></span>

    <span data-ttu-id="bb0a1-197">g.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-197">g.</span></span> <span data-ttu-id="bb0a1-198">I **identitet providern inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-198">In **Identity Provider Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bb0a1-199">h.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-199">h.</span></span> <span data-ttu-id="bb0a1-200">I **identitet providern logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-200">In **Identity Provider Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="bb0a1-201">Jag.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-201">i.</span></span> <span data-ttu-id="bb0a1-202">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-202">Click **Save**.</span></span>

14. <span data-ttu-id="bb0a1-203">Hello vänstra navigeringsfönstret i hello **administrera** klickar du på **domänhantering** tooexpand hello Närliggande avsnitt och klickar sedan på **min domän** tooopen hello  **Min domän** sidan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-203">On hello left navigation pane, in hello **Administer** section, click **Domain Management** tooexpand hello related section, and then click **My Domain** tooopen hello **My Domain** page.</span></span> 
    
    <span data-ttu-id="bb0a1-204">![Min domän](./media/active-directory-saas-jobscience-tutorial/ic767825.png "min domän")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-204">![My Domain](./media/active-directory-saas-jobscience-tutorial/ic767825.png "My Domain")</span></span>

15. <span data-ttu-id="bb0a1-205">På hello **min domän** i hello sidan **inloggningen sidan anpassning** klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-205">On hello **My Domain** page, in hello **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="bb0a1-206">![Inloggningssidan anpassning](./media/active-directory-saas-jobscience-tutorial/ic767826.png "inloggningssidan anpassning")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-206">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic767826.png "Login Page Branding")</span></span>

16. <span data-ttu-id="bb0a1-207">På hello **inloggningen sidan anpassning** i hello sidan **Autentiseringstjänsten** avsnitt, hello namnet på din **SAML SSO inställningar** visas.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-207">On hello **Login Page Branding** page, in hello **Authentication Service** section, hello name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="bb0a1-208">Markera den och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-208">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="bb0a1-209">![Inloggningssidan anpassning](./media/active-directory-saas-jobscience-tutorial/ic784366.png "inloggningssidan anpassning")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-209">![Login Page Branding](./media/active-directory-saas-jobscience-tutorial/ic784366.png "Login Page Branding")</span></span>

17. <span data-ttu-id="bb0a1-210">tooget hello SP initieras för enkel inloggning på inloggnings-URL-Klicka på hello **inställningar för enkel inloggning** i hello **säkerhetsåtgärder** menyn avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-210">tooget hello SP initiated Single Sign on Login URL click on hello **Single Sign On settings** in hello **Security Controls** menu section.</span></span>

    <span data-ttu-id="bb0a1-211">![Säkerhetsåtgärder](./media/active-directory-saas-jobscience-tutorial/ic784368.png "säkerhetsåtgärder")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-211">![Security Controls](./media/active-directory-saas-jobscience-tutorial/ic784368.png "Security Controls")</span></span>
    
    <span data-ttu-id="bb0a1-212">Klicka på hello SSO-profil som du har skapat i hello ovanstående steg.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-212">Click hello SSO profile you have created in hello step above.</span></span> <span data-ttu-id="bb0a1-213">Den här sidan visar hello enkel inloggning på URL: en för ditt företag (till exempel [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span><span class="sxs-lookup"><span data-stu-id="bb0a1-213">This page shows hello Single Sign on URL for your company (for example, [https://companyname.my.salesforce.com?so=companyid](https://companyname.my.salesforce.com?so=companyid).</span></span>    

> [!TIP]
> <span data-ttu-id="bb0a1-214">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="bb0a1-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bb0a1-215">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bb0a1-216">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bb0a1-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bb0a1-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb0a1-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="bb0a1-218">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bb0a1-220">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bb0a1-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb0a1-221">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bb0a1-223">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bb0a1-225">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bb0a1-227">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscience-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bb0a1-229">a.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-229">a.</span></span> <span data-ttu-id="bb0a1-230">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bb0a1-231">b.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-231">b.</span></span> <span data-ttu-id="bb0a1-232">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bb0a1-233">c.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-233">c.</span></span> <span data-ttu-id="bb0a1-234">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bb0a1-235">d.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-235">d.</span></span> <span data-ttu-id="bb0a1-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-236">Click **Create**.</span></span>
 
### <a name="creating-a-jobscience-test-user"></a><span data-ttu-id="bb0a1-237">Skapa en testanvändare Jobscience</span><span class="sxs-lookup"><span data-stu-id="bb0a1-237">Creating a Jobscience test user</span></span>

<span data-ttu-id="bb0a1-238">I ordning tooenable Azure AD-användare toolog i tooJobscience, måste de etableras i Jobscience.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-238">In order tooenable Azure AD users toolog in tooJobscience, they must be provisioned into Jobscience.</span></span> <span data-ttu-id="bb0a1-239">Hello gäller Jobscience är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-239">In hello case of Jobscience, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="bb0a1-240">Du kan använda något annat Jobscience användarens konto skapas verktyg eller API: er som tillhandahålls av Jobscience tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-240">You can use any other Jobscience user account creation tools or APIs provided by Jobscience tooprovision Azure Active Directory user accounts.</span></span>
>  
        
<span data-ttu-id="bb0a1-241">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="bb0a1-241">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb0a1-242">Logga in tooyour **Jobscience** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-242">Log in tooyour **Jobscience** company site as administrator.</span></span>

2. <span data-ttu-id="bb0a1-243">Gå tooSetup.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-243">Go tooSetup.</span></span>
   
   <span data-ttu-id="bb0a1-244">![Installationsprogrammet](./media/active-directory-saas-jobscience-tutorial/ic784358.png "installationen")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-244">![Setup](./media/active-directory-saas-jobscience-tutorial/ic784358.png "Setup")</span></span>
3. <span data-ttu-id="bb0a1-245">Gå för**hantera användare \> användare**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-245">Go too**Manage Users \> Users**.</span></span>
   
   <span data-ttu-id="bb0a1-246">![Användare](./media/active-directory-saas-jobscience-tutorial/ic784369.png "användare")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-246">![Users](./media/active-directory-saas-jobscience-tutorial/ic784369.png "Users")</span></span>
4. <span data-ttu-id="bb0a1-247">Klicka på **ny användare**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-247">Click **New User**.</span></span>
   
   <span data-ttu-id="bb0a1-248">![Alla användare](./media/active-directory-saas-jobscience-tutorial/ic784370.png "alla användare")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-248">![All Users](./media/active-directory-saas-jobscience-tutorial/ic784370.png "All Users")</span></span>
5. <span data-ttu-id="bb0a1-249">På hello **Redigera användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb0a1-249">On hello **Edit User** dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="bb0a1-250">![Redigera användare](./media/active-directory-saas-jobscience-tutorial/ic784371.png "Redigera användare")</span><span class="sxs-lookup"><span data-stu-id="bb0a1-250">![User Edit](./media/active-directory-saas-jobscience-tutorial/ic784371.png "User Edit")</span></span>
   
   <span data-ttu-id="bb0a1-251">a.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-251">a.</span></span> <span data-ttu-id="bb0a1-252">I hello **Förnamn** textruta, ange ett förnamn för hello användare som Britta.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-252">In hello **First Name** textbox, type a first name of hello user like Britta.</span></span>
   
   <span data-ttu-id="bb0a1-253">b.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-253">b.</span></span> <span data-ttu-id="bb0a1-254">I hello **efternamn** textruta anger efternamn för användaren hello som Simon.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-254">In hello **Last Name** textbox, type a last name of hello user like Simon.</span></span>
   
   <span data-ttu-id="bb0a1-255">c.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-255">c.</span></span> <span data-ttu-id="bb0a1-256">I hello **Alias** textruta typnamn hello användaren som brittas alias.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-256">In hello **Alias** textbox, type an alias name of hello user like brittas.</span></span>

   <span data-ttu-id="bb0a1-257">d.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-257">d.</span></span> <span data-ttu-id="bb0a1-258">I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-258">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="bb0a1-259">e.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-259">e.</span></span> <span data-ttu-id="bb0a1-260">I hello **användarnamn** textruta, Skriv ett användarnamn för användaren som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-260">In hello **User Name** textbox, type a user name of user like Brittasimon@contoso.com.</span></span>

   <span data-ttu-id="bb0a1-261">f.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-261">f.</span></span> <span data-ttu-id="bb0a1-262">I hello **smeknamn** textruta typnamn nick för användaren som Simon.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-262">In hello **Nick Name** textbox, type a nick name of user like Simon.</span></span>

   <span data-ttu-id="bb0a1-263">g.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-263">g.</span></span> <span data-ttu-id="bb0a1-264">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-264">Click **Save**.</span></span>

    
> [!NOTE]
> <span data-ttu-id="bb0a1-265">hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-265">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bb0a1-266">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb0a1-266">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bb0a1-267">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooJobscience.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-267">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooJobscience.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bb0a1-269">**tooassign Britta Simon tooJobscience utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bb0a1-269">**tooassign Britta Simon tooJobscience, perform hello following steps:**</span></span>

1. <span data-ttu-id="bb0a1-270">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-270">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bb0a1-272">Välj i listan med program hello **Jobscience**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-272">In hello applications list, select **Jobscience**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscience-tutorial/tutorial_jobscience_app.png) 

3. <span data-ttu-id="bb0a1-274">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-274">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bb0a1-276">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-276">Click **Add** button.</span></span> <span data-ttu-id="bb0a1-277">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-277">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bb0a1-279">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-279">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bb0a1-280">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-280">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bb0a1-281">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-281">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bb0a1-282">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bb0a1-282">Testing single sign-on</span></span>

<span data-ttu-id="bb0a1-283">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-283">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="bb0a1-284">Du bör få automatiskt inloggade tooyour Jobscience programmet när du klickar på hello Jobscience panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bb0a1-284">When you click hello Jobscience tile in hello Access Panel, you should get automatically signed-on tooyour Jobscience application.</span></span>
<span data-ttu-id="bb0a1-285">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bb0a1-285">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb0a1-286">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bb0a1-286">Additional resources</span></span>

* [<span data-ttu-id="bb0a1-287">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bb0a1-287">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bb0a1-288">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bb0a1-288">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscience-tutorial/tutorial_general_203.png

