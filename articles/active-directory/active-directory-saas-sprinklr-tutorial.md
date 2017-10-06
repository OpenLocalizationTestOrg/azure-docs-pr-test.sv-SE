---
title: "Självstudier: Azure Active Directory-integrering med Sprinklr | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Sprinklr."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 14b467c72d4a453ed7ad248eadcdade710f105af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a><span data-ttu-id="acf32-103">Självstudier: Azure Active Directory-integrering med Sprinklr</span><span class="sxs-lookup"><span data-stu-id="acf32-103">Tutorial: Azure Active Directory integration with Sprinklr</span></span>

<span data-ttu-id="acf32-104">I kursen får du lära dig hur toointegrate Sprinklr med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="acf32-104">In this tutorial, you learn how toointegrate Sprinklr with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="acf32-105">Integrera Sprinklr med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="acf32-105">Integrating Sprinklr with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="acf32-106">Du kan styra i Azure AD som har åtkomst till tooSprinklr</span><span class="sxs-lookup"><span data-stu-id="acf32-106">You can control in Azure AD who has access tooSprinklr</span></span>
- <span data-ttu-id="acf32-107">Du kan aktivera din användare tooautomatically get inloggade tooSprinklr (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="acf32-107">You can enable your users tooautomatically get signed-on tooSprinklr (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="acf32-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="acf32-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="acf32-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="acf32-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="acf32-110">Krav</span><span class="sxs-lookup"><span data-stu-id="acf32-110">Prerequisites</span></span>

<span data-ttu-id="acf32-111">tooconfigure Azure AD-integrering med Sprinklr, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="acf32-111">tooconfigure Azure AD integration with Sprinklr, you need hello following items:</span></span>

- <span data-ttu-id="acf32-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="acf32-112">An Azure AD subscription</span></span>
- <span data-ttu-id="acf32-113">En Sprinklr enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="acf32-113">A Sprinklr single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="acf32-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="acf32-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="acf32-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="acf32-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="acf32-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="acf32-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="acf32-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="acf32-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="acf32-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="acf32-118">Scenario description</span></span>
<span data-ttu-id="acf32-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="acf32-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="acf32-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="acf32-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="acf32-121">Att lägga till Sprinklr från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="acf32-121">Adding Sprinklr from hello gallery</span></span>
2. <span data-ttu-id="acf32-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acf32-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sprinklr-from-hello-gallery"></a><span data-ttu-id="acf32-123">Att lägga till Sprinklr från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="acf32-123">Adding Sprinklr from hello gallery</span></span>
<span data-ttu-id="acf32-124">tooconfigure hello integrering av Sprinklr i Azure AD, behöver du tooadd Sprinklr hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="acf32-124">tooconfigure hello integration of Sprinklr into Azure AD, you need tooadd Sprinklr from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="acf32-125">**tooadd Sprinklr från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="acf32-125">**tooadd Sprinklr from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf32-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="acf32-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="acf32-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="acf32-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="acf32-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="acf32-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="acf32-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acf32-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="acf32-133">Skriv i sökrutan hello **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="acf32-133">In hello search box, type **Sprinklr**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. <span data-ttu-id="acf32-135">Markera hello resultat på panelen **Sprinklr**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="acf32-135">In hello results panel, select **Sprinklr**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="acf32-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acf32-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="acf32-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Sprinklr baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="acf32-138">In this section, you configure and test Azure AD single sign-on with Sprinklr based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="acf32-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Sprinklr är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acf32-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Sprinklr is tooa user in Azure AD.</span></span> <span data-ttu-id="acf32-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Sprinklr toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="acf32-140">In other words, a link relationship between an Azure AD user and hello related user in Sprinklr needs toobe established.</span></span>

<span data-ttu-id="acf32-141">I Sprinklr, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="acf32-141">In Sprinklr, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="acf32-142">tooconfigure och testa Azure AD enkel inloggning med Sprinklr, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="acf32-142">tooconfigure and test Azure AD single sign-on with Sprinklr, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="acf32-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="acf32-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="acf32-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="acf32-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="acf32-145">**[Skapa en testanvändare Sprinklr](#creating-a-sprinklr-test-user)**  -toohave en motsvarighet för Britta Simon i Sprinklr som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="acf32-145">**[Creating a Sprinklr test user](#creating-a-sprinklr-test-user)** - toohave a counterpart of Britta Simon in Sprinklr that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="acf32-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="acf32-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="acf32-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="acf32-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="acf32-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acf32-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="acf32-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Sprinklr program.</span><span class="sxs-lookup"><span data-stu-id="acf32-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Sprinklr application.</span></span>

<span data-ttu-id="acf32-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Sprinklr:**</span><span class="sxs-lookup"><span data-stu-id="acf32-150">**tooconfigure Azure AD single sign-on with Sprinklr, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf32-151">I hello Azure-portalen på hello **Sprinklr** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="acf32-151">In hello Azure portal, on hello **Sprinklr** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="acf32-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="acf32-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. <span data-ttu-id="acf32-155">På hello **Sprinklr domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acf32-155">On hello **Sprinklr Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    <span data-ttu-id="acf32-157">a.</span><span class="sxs-lookup"><span data-stu-id="acf32-157">a.</span></span> <span data-ttu-id="acf32-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="acf32-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    <span data-ttu-id="acf32-159">b.</span><span class="sxs-lookup"><span data-stu-id="acf32-159">b.</span></span> <span data-ttu-id="acf32-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.sprinklr.com`</span><span class="sxs-lookup"><span data-stu-id="acf32-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.sprinklr.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="acf32-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="acf32-161">These values are not real.</span></span> <span data-ttu-id="acf32-162">Uppdatera hello värdet med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="acf32-162">Update hello value with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="acf32-163">Kontakta [Sprinklr klienten supportteamet](https://www.sprinklr.com/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="acf32-163">Contact [Sprinklr Client support team](https://www.sprinklr.com/contact-us/) tooget these values.</span></span> 
 
4. <span data-ttu-id="acf32-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="acf32-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. <span data-ttu-id="acf32-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="acf32-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="acf32-168">På hello **Sprinklr Configuration** klickar du på **konfigurera Sprinklr** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="acf32-168">On hello **Sprinklr Configuration** section, click **Configure Sprinklr** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="acf32-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="acf32-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

7. <span data-ttu-id="acf32-170">Logga in tooyour Sprinklr företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="acf32-170">In a different web browser window, log in tooyour Sprinklr company site as an administrator.</span></span>

8. <span data-ttu-id="acf32-171">Gå för**Administration \> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="acf32-171">Go too**Administration \> Settings**.</span></span>
   
    <span data-ttu-id="acf32-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="acf32-172">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

9. <span data-ttu-id="acf32-173">Gå för**hantera Partner \> för enkel inloggning** på från hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="acf32-173">Go too**Manage Partner \> Single Sign** on from hello left pane.</span></span>
   
    <span data-ttu-id="acf32-174">![Hantera Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "hantera Partner")</span><span class="sxs-lookup"><span data-stu-id="acf32-174">![Manage Partner](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Manage Partner")</span></span>

10. <span data-ttu-id="acf32-175">Klicka på **+ Lägg till en enda inloggningar**.</span><span class="sxs-lookup"><span data-stu-id="acf32-175">Click **+Add Single Sign Ons**.</span></span>
   
    <span data-ttu-id="acf32-176">![Enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="acf32-176">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "Single Sign-Ons")</span></span>

11. <span data-ttu-id="acf32-177">På hello **för enkelinloggning** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acf32-177">On hello **Single Sign on** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="acf32-178">![Enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="acf32-178">![Single Sign-Ons](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "Single Sign-Ons")</span></span>

    <span data-ttu-id="acf32-179">a.</span><span class="sxs-lookup"><span data-stu-id="acf32-179">a.</span></span> <span data-ttu-id="acf32-180">I hello **namn** textruta, ange ett namn för konfigurationen (till exempel: *WAADSSOTest*).</span><span class="sxs-lookup"><span data-stu-id="acf32-180">In hello **Name** textbox, type a name for your configuration (for example: *WAADSSOTest*).</span></span>

    <span data-ttu-id="acf32-181">b.</span><span class="sxs-lookup"><span data-stu-id="acf32-181">b.</span></span> <span data-ttu-id="acf32-182">Välj **aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="acf32-182">Select **Enabled**.</span></span>

    <span data-ttu-id="acf32-183">c.</span><span class="sxs-lookup"><span data-stu-id="acf32-183">c.</span></span> <span data-ttu-id="acf32-184">Välj **nya SSO certifikatet**.</span><span class="sxs-lookup"><span data-stu-id="acf32-184">Select **Use new SSO Certificate**.</span></span>
             
    <span data-ttu-id="acf32-185">e.</span><span class="sxs-lookup"><span data-stu-id="acf32-185">e.</span></span> <span data-ttu-id="acf32-186">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="acf32-186">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate** textbox.</span></span>

    <span data-ttu-id="acf32-187">f.</span><span class="sxs-lookup"><span data-stu-id="acf32-187">f.</span></span> <span data-ttu-id="acf32-188">Klistra in hello **SAML enhets-ID** värde som du har kopierat från Azure-portalen i hello **enhets-Id** textruta.</span><span class="sxs-lookup"><span data-stu-id="acf32-188">Paste hello **SAML Entity ID** value which you have copied from Azure Portal into hello **Entity Id** textbox.</span></span>

    <span data-ttu-id="acf32-189">g.</span><span class="sxs-lookup"><span data-stu-id="acf32-189">g.</span></span> <span data-ttu-id="acf32-190">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värde som du har kopierat från Azure-portalen i hello **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="acf32-190">Paste hello **SAML Single Sign-On Service URL** value which you have copied from Azure Portal into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="acf32-191">h.</span><span class="sxs-lookup"><span data-stu-id="acf32-191">h.</span></span> <span data-ttu-id="acf32-192">Klistra in hello **Sign-Out URL** värde som du har kopierat från Azure-portalen i hello **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="acf32-192">Paste hello **Sign-Out URL** value which you have copied from Azure Portal into hello **Identity Provider Logout URL** textbox.</span></span>
     
    <span data-ttu-id="acf32-193">Jag.</span><span class="sxs-lookup"><span data-stu-id="acf32-193">i.</span></span> <span data-ttu-id="acf32-194">Som **SAML-ID typ**väljer **Assertion innehåller användare ”s sprinklr.com användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="acf32-194">As **SAML User ID Type**, select **Assertion contains User”s sprinklr.com username**.</span></span>

    <span data-ttu-id="acf32-195">j.</span><span class="sxs-lookup"><span data-stu-id="acf32-195">j.</span></span> <span data-ttu-id="acf32-196">Som **SAML Användarplats ID**väljer **användar-ID är i hello namnidentifierare elementet i hello ämne instruktionen**.</span><span class="sxs-lookup"><span data-stu-id="acf32-196">As **SAML User ID Location**, select **User ID is in hello Name Identifier element of hello Subject statement**.</span></span>

    <span data-ttu-id="acf32-197">k.</span><span class="sxs-lookup"><span data-stu-id="acf32-197">k.</span></span> <span data-ttu-id="acf32-198">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="acf32-198">Click **Save**.</span></span>
       
    <span data-ttu-id="acf32-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="acf32-199">![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")</span></span>

> [!TIP]
> <span data-ttu-id="acf32-200">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="acf32-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="acf32-201">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="acf32-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="acf32-202">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="acf32-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="acf32-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="acf32-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="acf32-204">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="acf32-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="acf32-206">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="acf32-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf32-207">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="acf32-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="acf32-209">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="acf32-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="acf32-211">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acf32-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="acf32-213">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acf32-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="acf32-215">a.</span><span class="sxs-lookup"><span data-stu-id="acf32-215">a.</span></span> <span data-ttu-id="acf32-216">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="acf32-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="acf32-217">b.</span><span class="sxs-lookup"><span data-stu-id="acf32-217">b.</span></span> <span data-ttu-id="acf32-218">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="acf32-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="acf32-219">c.</span><span class="sxs-lookup"><span data-stu-id="acf32-219">c.</span></span> <span data-ttu-id="acf32-220">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="acf32-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="acf32-221">d.</span><span class="sxs-lookup"><span data-stu-id="acf32-221">d.</span></span> <span data-ttu-id="acf32-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="acf32-222">Click **Create**.</span></span>
 
### <a name="creating-a-sprinklr-test-user"></a><span data-ttu-id="acf32-223">Skapa en testanvändare Sprinklr</span><span class="sxs-lookup"><span data-stu-id="acf32-223">Creating a Sprinklr test user</span></span>

1. <span data-ttu-id="acf32-224">Logga in tooyour Sprinklr företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="acf32-224">Log in tooyour Sprinklr company site as an administrator.</span></span>

2. <span data-ttu-id="acf32-225">Gå för**Administration \> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="acf32-225">Go too**Administration \> Settings**.</span></span>
   
    <span data-ttu-id="acf32-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="acf32-226">![Administration](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Administration")</span></span>

3. <span data-ttu-id="acf32-227">Gå för**hantera klienten \> användare** från hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="acf32-227">Go too**Manage Client \> Users** from hello left pane.</span></span>
   
    <span data-ttu-id="acf32-228">![Inställningar för](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="acf32-228">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "Settings")</span></span>

4. <span data-ttu-id="acf32-229">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="acf32-229">Click **Add User**.</span></span>
   
    <span data-ttu-id="acf32-230">![Inställningar för](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="acf32-230">![Settings](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "Settings")</span></span>

5. <span data-ttu-id="acf32-231">På hello **Redigera användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acf32-231">On hello **Edit user** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="acf32-232">![Redigera användare](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Redigera användare")</span><span class="sxs-lookup"><span data-stu-id="acf32-232">![Edit user](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Edit user")</span></span> 

    <span data-ttu-id="acf32-233">a.</span><span class="sxs-lookup"><span data-stu-id="acf32-233">a.</span></span> <span data-ttu-id="acf32-234">I hello **e-post**, **Förnamn** och **efternamn** textrutor hello typinformation för en Azure AD-användarkonto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="acf32-234">In hello **Email**, **First Name** and **Last Name** textboxes, type hello information of an Azure AD user account you want tooprovision.</span></span>

    <span data-ttu-id="acf32-235">b.</span><span class="sxs-lookup"><span data-stu-id="acf32-235">b.</span></span> <span data-ttu-id="acf32-236">Välj **lösenord inaktiveras**.</span><span class="sxs-lookup"><span data-stu-id="acf32-236">Select **Password Disabled**.</span></span>

    <span data-ttu-id="acf32-237">c.</span><span class="sxs-lookup"><span data-stu-id="acf32-237">c.</span></span> <span data-ttu-id="acf32-238">Välj **språk**.</span><span class="sxs-lookup"><span data-stu-id="acf32-238">Select **Language**.</span></span>

    <span data-ttu-id="acf32-239">d.</span><span class="sxs-lookup"><span data-stu-id="acf32-239">d.</span></span> <span data-ttu-id="acf32-240">Välj **användartyp**.</span><span class="sxs-lookup"><span data-stu-id="acf32-240">Select **User Type**.</span></span>

    <span data-ttu-id="acf32-241">e.</span><span class="sxs-lookup"><span data-stu-id="acf32-241">e.</span></span> <span data-ttu-id="acf32-242">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="acf32-242">Click **Update**.</span></span>
   
     >[!IMPORTANT]
     ><span data-ttu-id="acf32-243">**Lösenord inaktiveras** måste vara valda tooenable toolog för en användare i via en identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="acf32-243">**Password Disabled** must be selected tooenable a user toolog in via an Identity provider.</span></span> 
     
6. <span data-ttu-id="acf32-244">Gå för**rollen**, och utför sedan hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acf32-244">Go too**Role**, and then perform hello following steps:</span></span>
   
    <span data-ttu-id="acf32-245">![Samarbeta roller](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner roller")</span><span class="sxs-lookup"><span data-stu-id="acf32-245">![Partner Roles](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partner Roles")</span></span>

    <span data-ttu-id="acf32-246">a.</span><span class="sxs-lookup"><span data-stu-id="acf32-246">a.</span></span> <span data-ttu-id="acf32-247">Från hello **Global** väljer **alla\_behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="acf32-247">From hello **Global** list, select **ALL\_Permissions**.</span></span>  

    <span data-ttu-id="acf32-248">b.</span><span class="sxs-lookup"><span data-stu-id="acf32-248">b.</span></span> <span data-ttu-id="acf32-249">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="acf32-249">Click **Update**.</span></span>

>[!NOTE]
><span data-ttu-id="acf32-250">Du kan använda något annat Sprinklr användarens konto skapas verktyg eller API: er som tillhandahålls av Sprinklr tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acf32-250">You can use any other Sprinklr user account creation tools or APIs provided by Sprinklr tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="acf32-251">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="acf32-251">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="acf32-252">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSprinklr.</span><span class="sxs-lookup"><span data-stu-id="acf32-252">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSprinklr.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="acf32-254">**tooassign Britta Simon tooSprinklr utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="acf32-254">**tooassign Britta Simon tooSprinklr, perform hello following steps:**</span></span>

1. <span data-ttu-id="acf32-255">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="acf32-255">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="acf32-257">Välj i listan med program hello **Sprinklr**.</span><span class="sxs-lookup"><span data-stu-id="acf32-257">In hello applications list, select **Sprinklr**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. <span data-ttu-id="acf32-259">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="acf32-259">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="acf32-261">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="acf32-261">Click **Add** button.</span></span> <span data-ttu-id="acf32-262">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acf32-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="acf32-264">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="acf32-264">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="acf32-265">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acf32-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="acf32-266">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acf32-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="acf32-267">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acf32-267">Testing single sign-on</span></span>

<span data-ttu-id="acf32-268">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="acf32-268">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="acf32-269">När du klickar på hello Sprinklr panelen i hello åtkomstpanelen bör du hämta automatiskt inloggade tooyour Sprinklr programmet för mer information om hello åtkomstpanelen, se [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="acf32-269">When you click hello Sprinklr tile in hello Access Panel, you should get automatically signed-on tooyour Sprinklr application For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="acf32-270">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="acf32-270">Additional resources</span></span>

* [<span data-ttu-id="acf32-271">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="acf32-271">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="acf32-272">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="acf32-272">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png

