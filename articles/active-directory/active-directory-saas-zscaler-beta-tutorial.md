---
title: "Självstudier: Azure Active Directory-integrering med Zscaler Beta | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler Beta."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1471c2b51ca5684a11acd40f4e450521605bb786
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a><span data-ttu-id="21985-103">Självstudier: Azure Active Directory-integrering med Zscaler Beta</span><span class="sxs-lookup"><span data-stu-id="21985-103">Tutorial: Azure Active Directory integration with Zscaler Beta</span></span>

<span data-ttu-id="21985-104">I kursen får du lära dig hur toointegrate Zscaler Beta med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="21985-104">In this tutorial, you learn how toointegrate Zscaler Beta with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="21985-105">Integrera Zscaler Beta med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="21985-105">Integrating Zscaler Beta with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="21985-106">Du kan styra i Azure AD som har åtkomst tooZscaler Beta</span><span class="sxs-lookup"><span data-stu-id="21985-106">You can control in Azure AD who has access tooZscaler Beta</span></span>
- <span data-ttu-id="21985-107">Du kan aktivera din användare tooautomatically get inloggade tooZscaler Beta (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="21985-107">You can enable your users tooautomatically get signed-on tooZscaler Beta (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="21985-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="21985-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="21985-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="21985-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21985-110">Krav</span><span class="sxs-lookup"><span data-stu-id="21985-110">Prerequisites</span></span>

<span data-ttu-id="21985-111">tooconfigure Azure AD-integrering med Zscaler Beta, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="21985-111">tooconfigure Azure AD integration with Zscaler Beta, you need hello following items:</span></span>

- <span data-ttu-id="21985-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="21985-112">An Azure AD subscription</span></span>
- <span data-ttu-id="21985-113">En Zscaler Beta enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="21985-113">A Zscaler Beta single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="21985-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="21985-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="21985-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="21985-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="21985-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="21985-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="21985-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="21985-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="21985-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="21985-118">Scenario description</span></span>
<span data-ttu-id="21985-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="21985-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="21985-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="21985-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="21985-121">Att lägga till Zscaler Beta från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="21985-121">Adding Zscaler Beta from hello gallery</span></span>
2. <span data-ttu-id="21985-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21985-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-beta-from-hello-gallery"></a><span data-ttu-id="21985-123">Att lägga till Zscaler Beta från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="21985-123">Adding Zscaler Beta from hello gallery</span></span>
<span data-ttu-id="21985-124">tooconfigure hello integrering av Zscaler Beta i Azure AD, behöver du tooadd Zscaler Beta hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="21985-124">tooconfigure hello integration of Zscaler Beta into Azure AD, you need tooadd Zscaler Beta from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="21985-125">**tooadd Zscaler Beta från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="21985-125">**tooadd Zscaler Beta from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="21985-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="21985-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="21985-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="21985-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="21985-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="21985-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="21985-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="21985-133">Skriv i sökrutan hello **Zscaler Beta**.</span><span class="sxs-lookup"><span data-stu-id="21985-133">In hello search box, type **Zscaler Beta**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

5. <span data-ttu-id="21985-135">Markera hello resultat på panelen **Zscaler Beta**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="21985-135">In hello results panel, select **Zscaler Beta**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="21985-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21985-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="21985-138">Du konfigurera och testa Azure AD enkel inloggning med Zscaler Beta baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="21985-138">In this section, you configure and test Azure AD single sign-on with Zscaler Beta based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="21985-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i betaversionen av Zscaler är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="21985-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler Beta is tooa user in Azure AD.</span></span> <span data-ttu-id="21985-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i betaversionen av Zscaler toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="21985-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler Beta needs toobe established.</span></span>

<span data-ttu-id="21985-141">Tilldela hello värdet hello i betaversionen av Zscaler **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="21985-141">In Zscaler Beta, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="21985-142">tooconfigure och testa Azure AD enkel inloggning med Zscaler Beta, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="21985-142">tooconfigure and test Azure AD single sign-on with Zscaler Beta, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="21985-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="21985-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="21985-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  -tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="21985-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="21985-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="21985-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="21985-146">**[Skapa en testanvändare Zscaler Beta](#creating-a-zscaler-beta-test-user)**  -toohave en motsvarighet för Britta Simon i betaversionen av Zscaler som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="21985-146">**[Creating a Zscaler Beta test user](#creating-a-zscaler-beta-test-user)** - toohave a counterpart of Britta Simon in Zscaler Beta that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="21985-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="21985-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="21985-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="21985-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="21985-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21985-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="21985-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zscaler Beta-program.</span><span class="sxs-lookup"><span data-stu-id="21985-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler Beta application.</span></span>

<span data-ttu-id="21985-151">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zscaler Beta:**</span><span class="sxs-lookup"><span data-stu-id="21985-151">**tooconfigure Azure AD single sign-on with Zscaler Beta, perform hello following steps:**</span></span>

1. <span data-ttu-id="21985-152">I hello Azure-portalen på hello **Zscaler Beta** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="21985-152">In hello Azure portal, on hello **Zscaler Beta** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="21985-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="21985-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

3. <span data-ttu-id="21985-156">På hello **Zscaler Beta domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21985-156">On hello **Zscaler Beta Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    <span data-ttu-id="21985-158">Ange hello-URL som används av dina användare toosign på tooyour Zscaler Beta-programmet i hello inloggning URL: en textruta.</span><span class="sxs-lookup"><span data-stu-id="21985-158">In hello Sign-on URL textbox, type hello URL used by your users toosign-on tooyour Zscaler Beta application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="21985-159">Du har tooupdate värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="21985-159">You have tooupdate this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="21985-160">Kontakta [Zscaler Betaklienten supportteamet](https://www.zscaler.com/company/contact) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="21985-160">Contact [Zscaler Beta Client support team](https://www.zscaler.com/company/contact) tooget this value.</span></span> 

4. <span data-ttu-id="21985-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="21985-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

5. <span data-ttu-id="21985-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="21985-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="21985-165">På hello **Zscaler Beta Configuration** klickar du på **konfigurera Zscaler Beta** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="21985-165">On hello **Zscaler Beta Configuration** section, click **Configure Zscaler Beta** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="21985-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="21985-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

7. <span data-ttu-id="21985-168">I en annan webbläsarfönster logga in tooyour Zscaler Beta företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="21985-168">In a different web browser window, log in tooyour Zscaler Beta company site as an administrator.</span></span>

8. <span data-ttu-id="21985-169">Hello-menyn överst hello **Administration**.</span><span class="sxs-lookup"><span data-stu-id="21985-169">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="21985-170">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="21985-170">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="21985-171">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="21985-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="21985-172">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="21985-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="21985-173">I hello **Välj autentiseringsalternativ för din organisation** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21985-173">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="21985-174">![Autentisering](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="21985-174">![Authentication](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="21985-175">a.</span><span class="sxs-lookup"><span data-stu-id="21985-175">a.</span></span> <span data-ttu-id="21985-176">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="21985-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="21985-177">b.</span><span class="sxs-lookup"><span data-stu-id="21985-177">b.</span></span> <span data-ttu-id="21985-178">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="21985-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="21985-179">På hello **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg hello och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="21985-179">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="21985-180">![Enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="21985-180">![Single Sign-On](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="21985-181">a.</span><span class="sxs-lookup"><span data-stu-id="21985-181">a.</span></span> <span data-ttu-id="21985-182">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **URL för hello SAML toowhich portalanvändare skickas för autentisering** textruta.</span><span class="sxs-lookup"><span data-stu-id="21985-182">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="21985-183">b.</span><span class="sxs-lookup"><span data-stu-id="21985-183">b.</span></span> <span data-ttu-id="21985-184">I hello **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="21985-184">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="21985-185">c.</span><span class="sxs-lookup"><span data-stu-id="21985-185">c.</span></span> <span data-ttu-id="21985-186">tooupload hämtade certifikatet, klicka på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="21985-186">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="21985-187">d.</span><span class="sxs-lookup"><span data-stu-id="21985-187">d.</span></span> <span data-ttu-id="21985-188">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="21985-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="21985-189">På hello **konfigurera användarautentisering** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21985-189">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="21985-190">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="21985-190">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="21985-191">a.</span><span class="sxs-lookup"><span data-stu-id="21985-191">a.</span></span> <span data-ttu-id="21985-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="21985-192">Click **Save**.</span></span>

    <span data-ttu-id="21985-193">b.</span><span class="sxs-lookup"><span data-stu-id="21985-193">b.</span></span> <span data-ttu-id="21985-194">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="21985-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="21985-195">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="21985-195">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="21985-196">tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="21985-196">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="21985-197">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="21985-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="21985-198">Välj **Internetalternativ** från hello **verktyg** menyn för öppna hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-198">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="21985-199">![Internet-alternativ](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="21985-199">![Internet Options](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="21985-200">Klicka på hello **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="21985-200">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="21985-201">![Anslutningar](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="21985-201">![Connections](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="21985-202">Klicka på **LAN-inställningar** tooopen hello **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-202">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="21985-203">I hello avsnittet för Proxy-server, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21985-203">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="21985-204">![Proxyserver](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="21985-204">![Proxy server](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="21985-205">a.</span><span class="sxs-lookup"><span data-stu-id="21985-205">a.</span></span> <span data-ttu-id="21985-206">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="21985-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="21985-207">b.</span><span class="sxs-lookup"><span data-stu-id="21985-207">b.</span></span> <span data-ttu-id="21985-208">Skriv i hello adress textruta **gateway.zscalerbeta.net**.</span><span class="sxs-lookup"><span data-stu-id="21985-208">In hello Address textbox, type **gateway.zscalerbeta.net**.</span></span>

    <span data-ttu-id="21985-209">c.</span><span class="sxs-lookup"><span data-stu-id="21985-209">c.</span></span> <span data-ttu-id="21985-210">Skriv i hello Port textruta **80**.</span><span class="sxs-lookup"><span data-stu-id="21985-210">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="21985-211">d.</span><span class="sxs-lookup"><span data-stu-id="21985-211">d.</span></span> <span data-ttu-id="21985-212">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="21985-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="21985-213">e.</span><span class="sxs-lookup"><span data-stu-id="21985-213">e.</span></span> <span data-ttu-id="21985-214">Klicka på **OK** tooclose hello **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-214">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="21985-215">Klicka på **OK** tooclose hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-215">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="21985-216">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="21985-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="21985-217">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="21985-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="21985-218">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="21985-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="21985-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="21985-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="21985-220">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="21985-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="21985-222">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="21985-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="21985-223">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="21985-223">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="21985-225">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="21985-225">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="21985-227">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-227">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="21985-229">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21985-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="21985-231">a.</span><span class="sxs-lookup"><span data-stu-id="21985-231">a.</span></span> <span data-ttu-id="21985-232">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="21985-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="21985-233">b.</span><span class="sxs-lookup"><span data-stu-id="21985-233">b.</span></span> <span data-ttu-id="21985-234">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="21985-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="21985-235">c.</span><span class="sxs-lookup"><span data-stu-id="21985-235">c.</span></span> <span data-ttu-id="21985-236">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="21985-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="21985-237">d.</span><span class="sxs-lookup"><span data-stu-id="21985-237">d.</span></span> <span data-ttu-id="21985-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="21985-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-beta-test-user"></a><span data-ttu-id="21985-239">Skapa en testanvändare Zscaler Beta</span><span class="sxs-lookup"><span data-stu-id="21985-239">Creating a Zscaler Beta test user</span></span>

<span data-ttu-id="21985-240">tooenable Azure AD-användare toolog i tooZscaler Beta, måste de vara etablerade tooZscaler Beta.</span><span class="sxs-lookup"><span data-stu-id="21985-240">tooenable Azure AD users toolog in tooZscaler Beta, they must be provisioned tooZscaler Beta.</span></span> <span data-ttu-id="21985-241">Hello gäller Zscaler Beta är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="21985-241">In hello case of Zscaler Beta, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="21985-242">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="21985-242">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="21985-243">Logga in tooyour **Zscaler Beta** klient.</span><span class="sxs-lookup"><span data-stu-id="21985-243">Log in tooyour **Zscaler Beta** tenant.</span></span>

2. <span data-ttu-id="21985-244">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="21985-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="21985-245">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="21985-245">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="21985-246">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="21985-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="21985-247">![Lägg till](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="21985-247">![Add](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="21985-248">I hello **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="21985-248">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="21985-249">![Lägg till](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="21985-249">![Add](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="21985-250">I hello lägga till användare, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="21985-250">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="21985-251">![Lägg till användare](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="21985-251">![Add User](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="21985-252">a.</span><span class="sxs-lookup"><span data-stu-id="21985-252">a.</span></span> <span data-ttu-id="21985-253">Typen hello **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och hello **avdelning** av ett giltigt Azure AD-kontot som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="21985-253">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid Azure AD account you want tooprovision.</span></span>

    <span data-ttu-id="21985-254">b.</span><span class="sxs-lookup"><span data-stu-id="21985-254">b.</span></span> <span data-ttu-id="21985-255">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="21985-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="21985-256">Du kan använda något annat Zscaler Beta användarens konto skapas verktyg eller API: er som tillhandahålls av Zscaler Beta tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="21985-256">You can use any other Zscaler Beta user account creation tools or APIs provided by Zscaler Beta tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="21985-257">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="21985-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="21985-258">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZscaler Beta.</span><span class="sxs-lookup"><span data-stu-id="21985-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler Beta.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="21985-260">**tooassign Britta Simon tooZscaler Beta, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="21985-260">**tooassign Britta Simon tooZscaler Beta, perform hello following steps:**</span></span>

1. <span data-ttu-id="21985-261">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="21985-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="21985-263">Välj i listan med program hello **Zscaler Beta**.</span><span class="sxs-lookup"><span data-stu-id="21985-263">In hello applications list, select **Zscaler Beta**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

3. <span data-ttu-id="21985-265">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="21985-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="21985-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="21985-267">Click **Add** button.</span></span> <span data-ttu-id="21985-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="21985-270">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="21985-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="21985-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="21985-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="21985-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="21985-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="21985-273">Testing single sign-on</span></span>

<span data-ttu-id="21985-274">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="21985-274">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="21985-275">När du klickar på hello Zscaler Beta-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Zscaler Beta-programmet.</span><span class="sxs-lookup"><span data-stu-id="21985-275">When you click hello Zscaler Beta tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler Beta application.</span></span>
<span data-ttu-id="21985-276">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="21985-276">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="21985-277">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="21985-277">Additional resources</span></span>

* [<span data-ttu-id="21985-278">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="21985-278">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="21985-279">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="21985-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_203.png

