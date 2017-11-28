---
title: "Självstudier: Azure Active Directory-integrering med två Zscaler | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler två."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1fd8a940-7320-47e0-a176-2dd4eeca6db2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: dcd13d399f093f24a945f234401cd5b7e527ed34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a><span data-ttu-id="268fe-103">Självstudier: Azure Active Directory-integrering med två Zscaler</span><span class="sxs-lookup"><span data-stu-id="268fe-103">Tutorial: Azure Active Directory integration with Zscaler Two</span></span>

<span data-ttu-id="268fe-104">I kursen får du lära dig hur toointegrate Zscaler två med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="268fe-104">In this tutorial, you learn how toointegrate Zscaler Two with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="268fe-105">Integrera Zscaler två med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="268fe-105">Integrating Zscaler Two with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="268fe-106">Du kan styra i Azure AD som har åtkomst till tooZscaler två</span><span class="sxs-lookup"><span data-stu-id="268fe-106">You can control in Azure AD who has access tooZscaler Two</span></span>
- <span data-ttu-id="268fe-107">Du kan aktivera din användare tooautomatically get inloggade tooZscaler två (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="268fe-107">You can enable your users tooautomatically get signed-on tooZscaler Two (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="268fe-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="268fe-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="268fe-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="268fe-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="268fe-110">Krav</span><span class="sxs-lookup"><span data-stu-id="268fe-110">Prerequisites</span></span>

<span data-ttu-id="268fe-111">tooconfigure Azure AD-integrering med två Zscaler, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="268fe-111">tooconfigure Azure AD integration with Zscaler Two, you need hello following items:</span></span>

- <span data-ttu-id="268fe-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="268fe-112">An Azure AD subscription</span></span>
- <span data-ttu-id="268fe-113">En Zscaler två enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="268fe-113">A Zscaler Two single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="268fe-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="268fe-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="268fe-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="268fe-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="268fe-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="268fe-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="268fe-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="268fe-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="268fe-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="268fe-118">Scenario description</span></span>
<span data-ttu-id="268fe-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="268fe-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="268fe-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="268fe-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="268fe-121">Att lägga till två Zscaler från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="268fe-121">Adding Zscaler Two from hello gallery</span></span>
2. <span data-ttu-id="268fe-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="268fe-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-two-from-hello-gallery"></a><span data-ttu-id="268fe-123">Att lägga till två Zscaler från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="268fe-123">Adding Zscaler Two from hello gallery</span></span>
<span data-ttu-id="268fe-124">tooconfigure hello integrering av två Zscaler i Azure AD, behöver du tooadd Zscaler två hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="268fe-124">tooconfigure hello integration of Zscaler Two into Azure AD, you need tooadd Zscaler Two from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="268fe-125">**tooadd Zscaler två från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="268fe-125">**tooadd Zscaler Two from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="268fe-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="268fe-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="268fe-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="268fe-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="268fe-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="268fe-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="268fe-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="268fe-133">Skriv i sökrutan hello **Zscaler två**.</span><span class="sxs-lookup"><span data-stu-id="268fe-133">In hello search box, type **Zscaler Two**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_search.png)

5. <span data-ttu-id="268fe-135">Markera hello resultat på panelen **Zscaler två**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="268fe-135">In hello results panel, select **Zscaler Two**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="268fe-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="268fe-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="268fe-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler två baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="268fe-138">In this section, you configure and test Azure AD single sign-on with Zscaler Two based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="268fe-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i två Zscaler är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="268fe-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler Two is tooa user in Azure AD.</span></span> <span data-ttu-id="268fe-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zscaler två toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="268fe-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler Two needs toobe established.</span></span>

<span data-ttu-id="268fe-141">Tilldela hello värdet för hello i två Zscaler **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="268fe-141">In Zscaler Two, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="268fe-142">tooconfigure och testa Azure AD enkel inloggning med två Zscaler, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="268fe-142">tooconfigure and test Azure AD single sign-on with Zscaler Two, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="268fe-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="268fe-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="268fe-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  -tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="268fe-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="268fe-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="268fe-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="268fe-146">**[Skapa en Zscaler två testanvändare](#creating-a-zscaler-two-test-user)**  -toohave en motsvarighet för Britta Simon i Zscaler två som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="268fe-146">**[Creating a Zscaler Two test user](#creating-a-zscaler-two-test-user)** - toohave a counterpart of Britta Simon in Zscaler Two that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="268fe-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="268fe-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="268fe-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="268fe-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="268fe-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="268fe-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="268fe-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zscaler två program.</span><span class="sxs-lookup"><span data-stu-id="268fe-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler Two application.</span></span>

<span data-ttu-id="268fe-151">**Utför följande hello tooconfigure Azure AD enkel inloggning med två Zscaler:**</span><span class="sxs-lookup"><span data-stu-id="268fe-151">**tooconfigure Azure AD single sign-on with Zscaler Two, perform hello following steps:**</span></span>

1. <span data-ttu-id="268fe-152">I hello Azure-portalen på hello **Zscaler två** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="268fe-152">In hello Azure portal, on hello **Zscaler Two** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="268fe-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="268fe-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_samlbase.png)

3. <span data-ttu-id="268fe-156">På hello **Zscaler två domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="268fe-156">On hello **Zscaler Two Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_url.png)

   <span data-ttu-id="268fe-158">Ange hello-URL som används av dina användare toosign på tooyour ZScaler två program i hello inloggnings-URL-textrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-158">In hello Sign-on URL textbox, type hello URL used by your users toosign-on tooyour ZScaler Two application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="268fe-159">Du har tooupdate värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="268fe-159">You have tooupdate this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="268fe-160">Kontakta [Zscaler två klienten supportteamet](https://www.zscaler.com/company/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="268fe-160">Contact [Zscaler Two Client support team](https://www.zscaler.com/company/contact) tooget these values.</span></span>

4. <span data-ttu-id="268fe-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="268fe-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_certificate.png) 

5. <span data-ttu-id="268fe-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="268fe-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="268fe-165">På hello **Zscaler konfiguration med två** klickar du på **konfigurera Zscaler två** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="268fe-165">On hello **Zscaler Two Configuration** section, click **Configure Zscaler Two** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="268fe-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="268fe-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_configure.png) 

7. <span data-ttu-id="268fe-168">Logga in tooyour ZScaler två företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="268fe-168">In a different web browser window, log in tooyour ZScaler Two company site as an administrator.</span></span>

8. <span data-ttu-id="268fe-169">Hello-menyn överst hello **Administration**.</span><span class="sxs-lookup"><span data-stu-id="268fe-169">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="268fe-170">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="268fe-170">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="268fe-171">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="268fe-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="268fe-172">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-two-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="268fe-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-two-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="268fe-173">I hello **Välj autentiseringsalternativ för din organisation** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="268fe-173">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="268fe-174">![Autentisering](./media/active-directory-saas-zscaler-two-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="268fe-174">![Authentication](./media/active-directory-saas-zscaler-two-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="268fe-175">a.</span><span class="sxs-lookup"><span data-stu-id="268fe-175">a.</span></span> <span data-ttu-id="268fe-176">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="268fe-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="268fe-177">b.</span><span class="sxs-lookup"><span data-stu-id="268fe-177">b.</span></span> <span data-ttu-id="268fe-178">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="268fe-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="268fe-179">På hello **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg hello och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="268fe-179">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="268fe-180">![Enkel inloggning](./media/active-directory-saas-zscaler-two-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="268fe-180">![Single Sign-On](./media/active-directory-saas-zscaler-two-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="268fe-181">a.</span><span class="sxs-lookup"><span data-stu-id="268fe-181">a.</span></span> <span data-ttu-id="268fe-182">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **URL för hello SAML toowhich portalanvändare skickas för autentisering** textruta.</span><span class="sxs-lookup"><span data-stu-id="268fe-182">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="268fe-183">b.</span><span class="sxs-lookup"><span data-stu-id="268fe-183">b.</span></span> <span data-ttu-id="268fe-184">I hello **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="268fe-184">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="268fe-185">c.</span><span class="sxs-lookup"><span data-stu-id="268fe-185">c.</span></span> <span data-ttu-id="268fe-186">tooupload hämtade certifikatet, klicka på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="268fe-186">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="268fe-187">d.</span><span class="sxs-lookup"><span data-stu-id="268fe-187">d.</span></span> <span data-ttu-id="268fe-188">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="268fe-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="268fe-189">På hello **konfigurera användarautentisering** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="268fe-189">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="268fe-190">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="268fe-190">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="268fe-191">a.</span><span class="sxs-lookup"><span data-stu-id="268fe-191">a.</span></span> <span data-ttu-id="268fe-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="268fe-192">Click **Save**.</span></span>

    <span data-ttu-id="268fe-193">b.</span><span class="sxs-lookup"><span data-stu-id="268fe-193">b.</span></span> <span data-ttu-id="268fe-194">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="268fe-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="268fe-195">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="268fe-195">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="268fe-196">tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="268fe-196">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="268fe-197">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="268fe-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="268fe-198">Välj **Internetalternativ** från hello **verktyg** menyn för öppna hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-198">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="268fe-199">![Internet-alternativ](./media/active-directory-saas-zscaler-two-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="268fe-199">![Internet Options](./media/active-directory-saas-zscaler-two-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="268fe-200">Klicka på hello **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="268fe-200">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="268fe-201">![Anslutningar](./media/active-directory-saas-zscaler-two-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="268fe-201">![Connections](./media/active-directory-saas-zscaler-two-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="268fe-202">Klicka på **LAN-inställningar** tooopen hello **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-202">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="268fe-203">I hello avsnittet för Proxy-server, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="268fe-203">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="268fe-204">![Proxyserver](./media/active-directory-saas-zscaler-two-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="268fe-204">![Proxy server](./media/active-directory-saas-zscaler-two-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="268fe-205">a.</span><span class="sxs-lookup"><span data-stu-id="268fe-205">a.</span></span> <span data-ttu-id="268fe-206">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="268fe-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="268fe-207">b.</span><span class="sxs-lookup"><span data-stu-id="268fe-207">b.</span></span> <span data-ttu-id="268fe-208">Skriv i hello adress textruta **gateway.zscalertwo.net**.</span><span class="sxs-lookup"><span data-stu-id="268fe-208">In hello Address textbox, type **gateway.zscalertwo.net**.</span></span>

    <span data-ttu-id="268fe-209">c.</span><span class="sxs-lookup"><span data-stu-id="268fe-209">c.</span></span> <span data-ttu-id="268fe-210">Skriv i hello Port textruta **80**.</span><span class="sxs-lookup"><span data-stu-id="268fe-210">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="268fe-211">d.</span><span class="sxs-lookup"><span data-stu-id="268fe-211">d.</span></span> <span data-ttu-id="268fe-212">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="268fe-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="268fe-213">e.</span><span class="sxs-lookup"><span data-stu-id="268fe-213">e.</span></span> <span data-ttu-id="268fe-214">Klicka på **OK** tooclose hello **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-214">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="268fe-215">Klicka på **OK** tooclose hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-215">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="268fe-216">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="268fe-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="268fe-217">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="268fe-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="268fe-218">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="268fe-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="268fe-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="268fe-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="268fe-220">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="268fe-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="268fe-222">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="268fe-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="268fe-223">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="268fe-223">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="268fe-225">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="268fe-225">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="268fe-227">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-227">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="268fe-229">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="268fe-229">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-two-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="268fe-231">a.</span><span class="sxs-lookup"><span data-stu-id="268fe-231">a.</span></span> <span data-ttu-id="268fe-232">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="268fe-232">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="268fe-233">b.</span><span class="sxs-lookup"><span data-stu-id="268fe-233">b.</span></span> <span data-ttu-id="268fe-234">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="268fe-234">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="268fe-235">c.</span><span class="sxs-lookup"><span data-stu-id="268fe-235">c.</span></span> <span data-ttu-id="268fe-236">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="268fe-236">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="268fe-237">d.</span><span class="sxs-lookup"><span data-stu-id="268fe-237">d.</span></span> <span data-ttu-id="268fe-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="268fe-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-two-test-user"></a><span data-ttu-id="268fe-239">Skapa en Zscaler två testanvändare</span><span class="sxs-lookup"><span data-stu-id="268fe-239">Creating a Zscaler Two test user</span></span>

<span data-ttu-id="268fe-240">tooenable Azure AD-användare toolog i tooZScaler två, måste de vara etablerade tooZScaler två.</span><span class="sxs-lookup"><span data-stu-id="268fe-240">tooenable Azure AD users toolog in tooZScaler Two, they must be provisioned tooZScaler Two.</span></span> <span data-ttu-id="268fe-241">Hello gäller två ZScaler är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="268fe-241">In hello case of ZScaler Two, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="268fe-242">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="268fe-242">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="268fe-243">Logga in tooyour **Zscaler två** klient.</span><span class="sxs-lookup"><span data-stu-id="268fe-243">Log in tooyour **Zscaler Two** tenant.</span></span>

2. <span data-ttu-id="268fe-244">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="268fe-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="268fe-245">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="268fe-245">![Administration](./media/active-directory-saas-zscaler-two-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="268fe-246">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="268fe-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="268fe-247">![Lägg till](./media/active-directory-saas-zscaler-two-tutorial/ic781036.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="268fe-247">![Add](./media/active-directory-saas-zscaler-two-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="268fe-248">I hello **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="268fe-248">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="268fe-249">![Lägg till](./media/active-directory-saas-zscaler-two-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="268fe-249">![Add](./media/active-directory-saas-zscaler-two-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="268fe-250">I hello lägga till användare, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="268fe-250">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="268fe-251">![Lägg till användare](./media/active-directory-saas-zscaler-two-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="268fe-251">![Add User](./media/active-directory-saas-zscaler-two-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="268fe-252">a.</span><span class="sxs-lookup"><span data-stu-id="268fe-252">a.</span></span> <span data-ttu-id="268fe-253">Typen hello **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och hello **avdelning** av ett giltigt Azure AD-kontot som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="268fe-253">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid Azure AD account you want tooprovision.</span></span>

    <span data-ttu-id="268fe-254">b.</span><span class="sxs-lookup"><span data-stu-id="268fe-254">b.</span></span> <span data-ttu-id="268fe-255">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="268fe-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="268fe-256">Du kan använda något annat ZScaler två användare konto skapas verktyg eller API: er som tillhandahålls av ZScaler två tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="268fe-256">You can use any other ZScaler Two user account creation tools or APIs provided by ZScaler Two tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="268fe-257">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="268fe-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="268fe-258">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZscaler två.</span><span class="sxs-lookup"><span data-stu-id="268fe-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler Two.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="268fe-260">**tooassign Britta Simon tooZscaler två, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="268fe-260">**tooassign Britta Simon tooZscaler Two, perform hello following steps:**</span></span>

1. <span data-ttu-id="268fe-261">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="268fe-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="268fe-263">Välj i listan med program hello **Zscaler två**.</span><span class="sxs-lookup"><span data-stu-id="268fe-263">In hello applications list, select **Zscaler Two**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-two-tutorial/tutorial_zscalertwo_app.png) 

3. <span data-ttu-id="268fe-265">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="268fe-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="268fe-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="268fe-267">Click **Add** button.</span></span> <span data-ttu-id="268fe-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="268fe-270">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="268fe-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="268fe-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="268fe-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="268fe-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="268fe-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="268fe-273">Testing single sign-on</span></span>

<span data-ttu-id="268fe-274">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="268fe-274">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="268fe-275">Du bör få automatiskt inloggade tooyour Zscaler två program när du klickar på hello Zscaler två panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="268fe-275">When you click hello Zscaler Two tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler Two application.</span></span>
<span data-ttu-id="268fe-276">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="268fe-276">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="268fe-277">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="268fe-277">Additional resources</span></span>

* [<span data-ttu-id="268fe-278">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="268fe-278">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="268fe-279">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="268fe-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-two-tutorial/tutorial_general_203.png

