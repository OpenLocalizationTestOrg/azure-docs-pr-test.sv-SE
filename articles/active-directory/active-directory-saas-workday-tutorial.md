---
title: "Självstudier: Azure Active Directory-integrering med Workday | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Workday."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: aaa41e372e45f464c4540a70fc79c98dbc06d6b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a><span data-ttu-id="4ac77-103">Självstudier: Azure Active Directory-integrering med Workday</span><span class="sxs-lookup"><span data-stu-id="4ac77-103">Tutorial: Azure Active Directory integration with Workday</span></span>

<span data-ttu-id="4ac77-104">I kursen får du lära dig hur toointegrate Workday med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4ac77-104">In this tutorial, you learn how toointegrate Workday with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4ac77-105">Integrera Workday med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4ac77-105">Integrating Workday with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4ac77-106">Du kan styra i Azure AD som har åtkomst till tooWorkday</span><span class="sxs-lookup"><span data-stu-id="4ac77-106">You can control in Azure AD who has access tooWorkday</span></span>
- <span data-ttu-id="4ac77-107">Du kan aktivera din användare tooautomatically get inloggade tooWorkday (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4ac77-107">You can enable your users tooautomatically get signed-on tooWorkday (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4ac77-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4ac77-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4ac77-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4ac77-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ac77-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4ac77-110">Prerequisites</span></span>

<span data-ttu-id="4ac77-111">tooconfigure Azure AD-integrering med arbetsdagen, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4ac77-111">tooconfigure Azure AD integration with Workday, you need hello following items:</span></span>

- <span data-ttu-id="4ac77-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4ac77-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4ac77-113">En Workday enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="4ac77-113">A Workday single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4ac77-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4ac77-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4ac77-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4ac77-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4ac77-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4ac77-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4ac77-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4ac77-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4ac77-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4ac77-118">Scenario description</span></span>
<span data-ttu-id="4ac77-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4ac77-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4ac77-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4ac77-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4ac77-121">Att lägga till Workday från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4ac77-121">Adding Workday from hello gallery</span></span>
2. <span data-ttu-id="4ac77-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ac77-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workday-from-hello-gallery"></a><span data-ttu-id="4ac77-123">Att lägga till Workday från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4ac77-123">Adding Workday from hello gallery</span></span>
<span data-ttu-id="4ac77-124">tooconfigure hello integrering av arbetsdagen i Azure AD, behöver du tooadd Workday hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4ac77-124">tooconfigure hello integration of Workday into Azure AD, you need tooadd Workday from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4ac77-125">**tooadd Workday från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4ac77-125">**tooadd Workday from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4ac77-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4ac77-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4ac77-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4ac77-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4ac77-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ac77-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4ac77-133">Skriv i sökrutan hello **Workday**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-133">In hello search box, type **Workday**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. <span data-ttu-id="4ac77-135">Markera hello resultat på panelen **Workday**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4ac77-135">In hello results panel, select **Workday**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4ac77-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ac77-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4ac77-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workday baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4ac77-138">In this section, you configure and test Azure AD single sign-on with Workday based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4ac77-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Workday är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4ac77-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workday is tooa user in Azure AD.</span></span> <span data-ttu-id="4ac77-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Workday toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4ac77-140">In other words, a link relationship between an Azure AD user and hello related user in Workday needs toobe established.</span></span>

<span data-ttu-id="4ac77-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Workday.</span><span class="sxs-lookup"><span data-stu-id="4ac77-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Workday.</span></span>

<span data-ttu-id="4ac77-142">tooconfigure och testa Azure AD enkel inloggning med arbetsdagen, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4ac77-142">tooconfigure and test Azure AD single sign-on with Workday, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4ac77-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4ac77-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4ac77-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4ac77-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4ac77-145">**[Skapa en testanvändare Workday](#creating-a-workday-test-user)**  -toohave en motsvarighet för Britta Simon i Workday som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4ac77-145">**[Creating a Workday test user](#creating-a-workday-test-user)** - toohave a counterpart of Britta Simon in Workday that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4ac77-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4ac77-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4ac77-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4ac77-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4ac77-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ac77-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4ac77-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Workday-program.</span><span class="sxs-lookup"><span data-stu-id="4ac77-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workday application.</span></span>

<span data-ttu-id="4ac77-150">**tooconfigure Azure AD enkel inloggning med arbetsdagen, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4ac77-150">**tooconfigure Azure AD single sign-on with Workday, perform hello following steps:**</span></span>

1. <span data-ttu-id="4ac77-151">I hello Azure-portalen på hello **Workday** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-151">In hello Azure portal, on hello **Workday** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4ac77-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4ac77-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. <span data-ttu-id="4ac77-155">På hello **Workday domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ac77-155">On hello **Workday Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    <span data-ttu-id="4ac77-157">a.</span><span class="sxs-lookup"><span data-stu-id="4ac77-157">a.</span></span> <span data-ttu-id="4ac77-158">I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://impl.workday.com/<tenant>/login-saml2.htmld`</span><span class="sxs-lookup"><span data-stu-id="4ac77-158">In hello **Sign-on URL** textbox, type hello value as: `https://impl.workday.com/<tenant>/login-saml2.htmld`</span></span>

    <span data-ttu-id="4ac77-159">b.</span><span class="sxs-lookup"><span data-stu-id="4ac77-159">b.</span></span> <span data-ttu-id="4ac77-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://impl.workday.com/<tenant>/login-saml.htmld`</span><span class="sxs-lookup"><span data-stu-id="4ac77-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://impl.workday.com/<tenant>/login-saml.htmld`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4ac77-161">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="4ac77-161">These values are not hello real.</span></span> <span data-ttu-id="4ac77-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="4ac77-162">Update these values with hello actual Sign-on URL and Reply URL.</span></span> <span data-ttu-id="4ac77-163">Svars-URL: en måste ha en underdomän till exempel: www wd2, wd3, wd3 impl, wd5, wd5 impl).</span><span class="sxs-lookup"><span data-stu-id="4ac77-163">Your reply URL must have a subdomain for example: www, wd2, wd3, wd3-impl, wd5, wd5-impl).</span></span> <span data-ttu-id="4ac77-164">Med hjälp av något som liknar ”*http://www.myworkday.com*” fungerar men ”*http://myworkday.com*” finns inte.</span><span class="sxs-lookup"><span data-stu-id="4ac77-164">Using something like "*http://www.myworkday.com*" works but "*http://myworkday.com*" does not.</span></span> <span data-ttu-id="4ac77-165">Kontakta [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4ac77-165">Contact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) tooget these values.</span></span> 
 

4. <span data-ttu-id="4ac77-166">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="4ac77-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. <span data-ttu-id="4ac77-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4ac77-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4ac77-170">På hello **Workday Configuration** klickar du på **konfigurera Workday** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4ac77-170">On hello **Workday Configuration** section, click **Configure Workday** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4ac77-171">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4ac77-171">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="4ac77-172">![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="4ac77-172">![Configure Single Sign-On](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS></span></span>
7. <span data-ttu-id="4ac77-173">Logga in tooyour Workday företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="4ac77-173">In a different web browser window, log in tooyour Workday company site as an administrator.</span></span>

8. <span data-ttu-id="4ac77-174">Gå för**menyn \> arbetsstationen**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-174">Go too**Menu \> Workbench**.</span></span>
   
    <span data-ttu-id="4ac77-175">![Arbetsstationen](./media/active-directory-saas-workday-tutorial/IC782923.png "arbetsstationen")</span><span class="sxs-lookup"><span data-stu-id="4ac77-175">![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")</span></span>

9. <span data-ttu-id="4ac77-176">Gå för**administrationen**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-176">Go too**Account Administration**.</span></span>
   
    <span data-ttu-id="4ac77-177">![Kontot Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "konto Administration")</span><span class="sxs-lookup"><span data-stu-id="4ac77-177">![Account Administration](./media/active-directory-saas-workday-tutorial/IC782924.png "Account Administration")</span></span>

10. <span data-ttu-id="4ac77-178">Gå för**redigera klient inställningar – säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-178">Go too**Edit Tenant Setup – Security**.</span></span>
   
    <span data-ttu-id="4ac77-179">![Redigera klient säkerhet](./media/active-directory-saas-workday-tutorial/IC782925.png "redigera klient säkerhet")</span><span class="sxs-lookup"><span data-stu-id="4ac77-179">![Edit Tenant Security](./media/active-directory-saas-workday-tutorial/IC782925.png "Edit Tenant Security")</span></span>

11. <span data-ttu-id="4ac77-180">I hello **omdirigering av URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ac77-180">In hello **Redirection URLs** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="4ac77-181">![Omdirigering av URL: er](./media/active-directory-saas-workday-tutorial/IC7829581.png "omdirigerings-URL: er")</span><span class="sxs-lookup"><span data-stu-id="4ac77-181">![Redirection URLs](./media/active-directory-saas-workday-tutorial/IC7829581.png "Redirection URLs")</span></span>
   
    <span data-ttu-id="4ac77-182">a.</span><span class="sxs-lookup"><span data-stu-id="4ac77-182">a.</span></span> <span data-ttu-id="4ac77-183">Klicka på **lägga till raden**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-183">Click **Add Row**.</span></span>
   
    <span data-ttu-id="4ac77-184">b.</span><span class="sxs-lookup"><span data-stu-id="4ac77-184">b.</span></span> <span data-ttu-id="4ac77-185">I hello **inloggningen omdirigerings-URL** textruta och hello **Mobile omdirigerings-URL** textruta typen hello **inloggnings-URL** du har angett på hello **Workday domän och URL: er** avsnitt i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4ac77-185">In hello **Login Redirect URL** textbox and hello **Mobile Redirect URL** textbox, type hello **Sign-on URL** you have entered on hello **Workday Domain and URLs** section of hello Azure portal.</span></span>
   
    <span data-ttu-id="4ac77-186">c.</span><span class="sxs-lookup"><span data-stu-id="4ac77-186">c.</span></span> <span data-ttu-id="4ac77-187">I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **Sign-Out URL**, och klistra in den i hello **logga ut omdirigerings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="4ac77-187">In hello Azure portal, on hello **Configure sign-on** window, copy hello **Sign-Out URL**, and then paste it into hello **Logout Redirect URL** textbox.</span></span>
   
    <span data-ttu-id="4ac77-188">d.</span><span class="sxs-lookup"><span data-stu-id="4ac77-188">d.</span></span>  <span data-ttu-id="4ac77-189">I **miljö** textruta typnamn hello miljö.</span><span class="sxs-lookup"><span data-stu-id="4ac77-189">In **Environment** textbox, type hello environment name.</span></span>  

    >[!NOTE]
    > <span data-ttu-id="4ac77-190">hello värdet för attributet för hello-miljö är knutna toohello värdet för hello klient-URL:</span><span class="sxs-lookup"><span data-stu-id="4ac77-190">hello value of hello Environment attribute is tied toohello value of hello tenant URL:</span></span>  
    ><span data-ttu-id="4ac77-191">-Om hello domännamnet för hello Workday-klient-URL börjar med impl till exempel: *https://impl.workday.com/\<klient\>/login-saml2.htmld*), hello **miljö** attributet måste anges tooImplementation.</span><span class="sxs-lookup"><span data-stu-id="4ac77-191">-If hello domain name of hello Workday tenant URL starts with impl for example: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), hello **Environment** attribute must be set tooImplementation.</span></span>  
    ><span data-ttu-id="4ac77-192">-Om hello domännamn startas med ett annat, måste toocontact [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello matchar **miljö** värde.</span><span class="sxs-lookup"><span data-stu-id="4ac77-192">-If hello domain name starts with something else, you need toocontact [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) tooget hello matching **Environment** value.</span></span>

12. <span data-ttu-id="4ac77-193">I hello **SAML installationsprogrammet** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ac77-193">In hello **SAML Setup** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="4ac77-194">![SAML-installationsprogrammet](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-installationen")</span><span class="sxs-lookup"><span data-stu-id="4ac77-194">![SAML Setup](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML Setup")</span></span>
   
    <span data-ttu-id="4ac77-195">a.</span><span class="sxs-lookup"><span data-stu-id="4ac77-195">a.</span></span>  <span data-ttu-id="4ac77-196">Välj **aktivera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-196">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="4ac77-197">b.</span><span class="sxs-lookup"><span data-stu-id="4ac77-197">b.</span></span>  <span data-ttu-id="4ac77-198">Klicka på **lägga till raden**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-198">Click **Add Row**.</span></span>

13. <span data-ttu-id="4ac77-199">I hello identitetsleverantörer för SAML-avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ac77-199">In hello SAML Identity Providers section, perform hello following steps:</span></span>
   
    <span data-ttu-id="4ac77-200">![SAML identitetsleverantörer](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML identitetsleverantörer")</span><span class="sxs-lookup"><span data-stu-id="4ac77-200">![SAML Identity Providers](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML Identity Providers")</span></span>
   
    <span data-ttu-id="4ac77-201">a.</span><span class="sxs-lookup"><span data-stu-id="4ac77-201">a.</span></span> <span data-ttu-id="4ac77-202">Skriv ett providernamn i hello identitet providernamn textruta (till exempel: *SPInitiatedSSO*).</span><span class="sxs-lookup"><span data-stu-id="4ac77-202">In hello Identity Provider Name textbox, type a provider name (for example: *SPInitiatedSSO*).</span></span>
   
    <span data-ttu-id="4ac77-203">b.</span><span class="sxs-lookup"><span data-stu-id="4ac77-203">b.</span></span> <span data-ttu-id="4ac77-204">I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **SAML enhets-ID** värdet och klistrar in den i hello **utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="4ac77-204">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Entity ID** value, and then paste it into hello **Issuer** textbox.</span></span>
   
    <span data-ttu-id="4ac77-205">c.</span><span class="sxs-lookup"><span data-stu-id="4ac77-205">c.</span></span> <span data-ttu-id="4ac77-206">Välj **aktivera arbetsdagar initierade logga ut**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-206">Select **Enable Workday Initiated Logout**.</span></span>
   
    <span data-ttu-id="4ac77-207">d.</span><span class="sxs-lookup"><span data-stu-id="4ac77-207">d.</span></span> <span data-ttu-id="4ac77-208">I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **Sign-Out URL** värdet och klistrar in den i hello **logga ut begäran URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="4ac77-208">In hello Azure portal, on hello **Configure sign-on** window, copy hello **Sign-Out URL** value, and then paste it into hello **Logout Request URL** textbox.</span></span>

    <span data-ttu-id="4ac77-209">e.</span><span class="sxs-lookup"><span data-stu-id="4ac77-209">e.</span></span> <span data-ttu-id="4ac77-210">Klicka på **providern offentliga nyckel identitetscertifikat**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-210">Click **Identity Provider Public Key Certificate**, and then click **Create**.</span></span> 

    <span data-ttu-id="4ac77-211">![Skapa](./media/active-directory-saas-workday-tutorial/IC782928.png "skapa")</span><span class="sxs-lookup"><span data-stu-id="4ac77-211">![Create](./media/active-directory-saas-workday-tutorial/IC782928.png "Create")</span></span>

    <span data-ttu-id="4ac77-212">f.</span><span class="sxs-lookup"><span data-stu-id="4ac77-212">f.</span></span> <span data-ttu-id="4ac77-213">Klicka på **skapa x509 offentliga nyckel**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-213">Click **Create x509 Public Key**.</span></span> 

    <span data-ttu-id="4ac77-214">![Skapa](./media/active-directory-saas-workday-tutorial/IC782929.png "skapa")</span><span class="sxs-lookup"><span data-stu-id="4ac77-214">![Create](./media/active-directory-saas-workday-tutorial/IC782929.png "Create")</span></span>


14. <span data-ttu-id="4ac77-215">I hello **visa x509 offentliga nyckel** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ac77-215">In hello **View x509 Public Key** section, perform hello following steps:</span></span> 
   
    <span data-ttu-id="4ac77-216">![Visa x509 offentliga nyckel](./media/active-directory-saas-workday-tutorial/IC782930.png "visa x509 offentliga nyckel")</span><span class="sxs-lookup"><span data-stu-id="4ac77-216">![View x509 Public Key](./media/active-directory-saas-workday-tutorial/IC782930.png "View x509 Public Key")</span></span> 
   
    <span data-ttu-id="4ac77-217">a.</span><span class="sxs-lookup"><span data-stu-id="4ac77-217">a.</span></span> <span data-ttu-id="4ac77-218">I hello **namn** textruta, ange ett namn för certifikatet (till exempel: *utrustningen\_SP*).</span><span class="sxs-lookup"><span data-stu-id="4ac77-218">In hello **Name** textbox, type a name for your certificate (for example: *PPE\_SP*).</span></span>
   
    <span data-ttu-id="4ac77-219">b.</span><span class="sxs-lookup"><span data-stu-id="4ac77-219">b.</span></span> <span data-ttu-id="4ac77-220">I hello **giltigt från** textruta typen hello giltig från attributvärde för ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="4ac77-220">In hello **Valid From** textbox, type hello valid from attribute value of your certificate.</span></span>
   
    <span data-ttu-id="4ac77-221">c.</span><span class="sxs-lookup"><span data-stu-id="4ac77-221">c.</span></span>  <span data-ttu-id="4ac77-222">I hello **till** textruta TYPVÄRDE hello giltig tooattribute av certifikatet.</span><span class="sxs-lookup"><span data-stu-id="4ac77-222">In hello **Valid To** textbox, type hello valid tooattribute value of your certificate.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="4ac77-223">Du kan hämta hello giltigt från datum och hello giltig toodate från hello hämtat certifikat genom att dubbelklicka på den.</span><span class="sxs-lookup"><span data-stu-id="4ac77-223">You can get hello valid from date and hello valid toodate from hello downloaded certificate by double-clicking it.</span></span>  <span data-ttu-id="4ac77-224">hello datum visas under hello **information** fliken.</span><span class="sxs-lookup"><span data-stu-id="4ac77-224">hello dates are listed under hello **Details** tab.</span></span>
    > 
    >
   
    <span data-ttu-id="4ac77-225">d.</span><span class="sxs-lookup"><span data-stu-id="4ac77-225">d.</span></span>  <span data-ttu-id="4ac77-226">Öppna din Base64-kodade certifikatet i anteckningar och kopiera hello innehållet i den.</span><span class="sxs-lookup"><span data-stu-id="4ac77-226">Open your base-64 encoded certificate in notepad, and then copy hello content of it.</span></span>
   
    <span data-ttu-id="4ac77-227">e.</span><span class="sxs-lookup"><span data-stu-id="4ac77-227">e.</span></span>  <span data-ttu-id="4ac77-228">I hello **certifikat** textruta klistra in hello innehållet i Urklipp.</span><span class="sxs-lookup"><span data-stu-id="4ac77-228">In hello **Certificate** textbox, paste hello content of your clipboard.</span></span>
   
    <span data-ttu-id="4ac77-229">f.</span><span class="sxs-lookup"><span data-stu-id="4ac77-229">f.</span></span>  <span data-ttu-id="4ac77-230">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-230">Click **OK**.</span></span>

15. <span data-ttu-id="4ac77-231">Utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="4ac77-231">Perform hello following steps:</span></span> 
   
    <span data-ttu-id="4ac77-232">![Konfiguration av SSO](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="4ac77-232">![SSO configuration](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO configuration")</span></span>
   
    <span data-ttu-id="4ac77-233">a.</span><span class="sxs-lookup"><span data-stu-id="4ac77-233">a.</span></span>  <span data-ttu-id="4ac77-234">Aktivera hello **x509 privata nyckel**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-234">Enable hello **x509 Private Key Pair**.</span></span>
   
    <span data-ttu-id="4ac77-235">b.</span><span class="sxs-lookup"><span data-stu-id="4ac77-235">b.</span></span>  <span data-ttu-id="4ac77-236">I hello **Service Provider-ID** textruta typen **http://www.workday.com**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-236">In hello **Service Provider ID** textbox, type **http://www.workday.com**.</span></span>
   
    <span data-ttu-id="4ac77-237">c.</span><span class="sxs-lookup"><span data-stu-id="4ac77-237">c.</span></span>  <span data-ttu-id="4ac77-238">Välj **aktivera SP initierade SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-238">Select **Enable SP Initiated SAML Authentication**.</span></span>
   
    <span data-ttu-id="4ac77-239">d.</span><span class="sxs-lookup"><span data-stu-id="4ac77-239">d.</span></span>  <span data-ttu-id="4ac77-240">I hello Azure-portalen på hello **konfigurera inloggning** fönster, kopiera hello **SAML enkel inloggning Tjänstwebbadress** värdet och klistrar in den i hello **IdP SSO-tjänstens URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="4ac77-240">In hello Azure portal, on hello **Configure sign-on** window, copy hello **SAML Single Sign-On Service URL** value, and then paste it into hello **IdP SSO Service URL** textbox.</span></span>
   
    <span data-ttu-id="4ac77-241">e.</span><span class="sxs-lookup"><span data-stu-id="4ac77-241">e.</span></span> <span data-ttu-id="4ac77-242">Välj **inte Deflate SP-initierad autentiseringsbegäran**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-242">Select **Do Not Deflate SP-initiated Authentication Request**.</span></span>
   
    <span data-ttu-id="4ac77-243">f.</span><span class="sxs-lookup"><span data-stu-id="4ac77-243">f.</span></span> <span data-ttu-id="4ac77-244">Som **begära signatur autentiseringsmetod**väljer **SHA256**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-244">As **Authentication Request Signature Method**, select **SHA256**.</span></span> 
   
    <span data-ttu-id="4ac77-245">![Autentiseringsmetod begäran signatur](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "autentiseringsmetod begäran signatur")</span><span class="sxs-lookup"><span data-stu-id="4ac77-245">![Authentication Request Signature Method](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "Authentication Request Signature Method")</span></span> 
   
    <span data-ttu-id="4ac77-246">g.</span><span class="sxs-lookup"><span data-stu-id="4ac77-246">g.</span></span> <span data-ttu-id="4ac77-247">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-247">Click **OK**.</span></span> 
   
    <span data-ttu-id="4ac77-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span><span class="sxs-lookup"><span data-stu-id="4ac77-248">![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE></span></span>

> [!TIP]
> <span data-ttu-id="4ac77-249">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4ac77-249">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4ac77-250">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4ac77-250">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4ac77-251">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4ac77-251">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4ac77-252">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4ac77-252">Creating an Azure AD test user</span></span>
<span data-ttu-id="4ac77-253">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4ac77-253">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4ac77-255">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4ac77-255">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4ac77-256">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4ac77-256">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4ac77-258">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-258">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4ac77-260">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ac77-260">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4ac77-262">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ac77-262">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4ac77-264">a.</span><span class="sxs-lookup"><span data-stu-id="4ac77-264">a.</span></span> <span data-ttu-id="4ac77-265">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-265">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4ac77-266">b.</span><span class="sxs-lookup"><span data-stu-id="4ac77-266">b.</span></span> <span data-ttu-id="4ac77-267">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4ac77-267">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4ac77-268">c.</span><span class="sxs-lookup"><span data-stu-id="4ac77-268">c.</span></span> <span data-ttu-id="4ac77-269">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-269">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4ac77-270">d.</span><span class="sxs-lookup"><span data-stu-id="4ac77-270">d.</span></span> <span data-ttu-id="4ac77-271">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-271">Click **Create**.</span></span>
 
### <a name="creating-a-workday-test-user"></a><span data-ttu-id="4ac77-272">Skapa en arbetsdag testanvändare</span><span class="sxs-lookup"><span data-stu-id="4ac77-272">Creating a Workday test user</span></span>

<span data-ttu-id="4ac77-273">tooget en testanvändare etableras i Workday måste toocontact hello [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html).</span><span class="sxs-lookup"><span data-stu-id="4ac77-273">tooget a test user provisioned into Workday, you need toocontact hello [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html).</span></span>
<span data-ttu-id="4ac77-274">hello [Workday klienten supportteamet](https://www.workday.com/en-us/partners-services/services/support.html) skapar hello användare åt dig.</span><span class="sxs-lookup"><span data-stu-id="4ac77-274">hello [Workday Client support team](https://www.workday.com/en-us/partners-services/services/support.html) creates hello user for you.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4ac77-275">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4ac77-275">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4ac77-276">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkday.</span><span class="sxs-lookup"><span data-stu-id="4ac77-276">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkday.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4ac77-278">**tooassign Britta Simon tooWorkday utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4ac77-278">**tooassign Britta Simon tooWorkday, perform hello following steps:**</span></span>

1. <span data-ttu-id="4ac77-279">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-279">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4ac77-281">Välj i listan med program hello **Workday**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-281">In hello applications list, select **Workday**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. <span data-ttu-id="4ac77-283">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4ac77-283">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4ac77-285">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4ac77-285">Click **Add** button.</span></span> <span data-ttu-id="4ac77-286">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ac77-286">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4ac77-288">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4ac77-288">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4ac77-289">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ac77-289">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4ac77-290">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ac77-290">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4ac77-291">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ac77-291">Testing single sign-on</span></span>

<span data-ttu-id="4ac77-292">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4ac77-292">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="4ac77-293">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4ac77-293">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ac77-294">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4ac77-294">Additional resources</span></span>

* [<span data-ttu-id="4ac77-295">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4ac77-295">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4ac77-296">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4ac77-296">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="4ac77-297">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="4ac77-297">Configure User Provisioning</span></span>](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png

