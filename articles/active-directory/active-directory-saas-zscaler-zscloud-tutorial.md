---
title: "Självstudier: Azure Active Directory-integrering med Zscaler ZSCloud | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler ZSCloud."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: af6d5c1994e715cccf959cc9fd3ba998e5b9effa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a><span data-ttu-id="5baa9-103">Självstudier: Azure Active Directory-integrering med Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="5baa9-103">Tutorial: Azure Active Directory integration with Zscaler ZSCloud</span></span>

<span data-ttu-id="5baa9-104">I kursen får du lära dig hur toointegrate Zscaler ZSCloud med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5baa9-104">In this tutorial, you learn how toointegrate Zscaler ZSCloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5baa9-105">Integrera Zscaler ZSCloud med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5baa9-105">Integrating Zscaler ZSCloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5baa9-106">Du kan styra i Azure AD som har åtkomst tooZscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="5baa9-106">You can control in Azure AD who has access tooZscaler ZSCloud</span></span>
- <span data-ttu-id="5baa9-107">Du kan aktivera din användare tooautomatically get inloggade tooZscaler ZSCloud (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5baa9-107">You can enable your users tooautomatically get signed-on tooZscaler ZSCloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5baa9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5baa9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5baa9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5baa9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5baa9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5baa9-110">Prerequisites</span></span>

<span data-ttu-id="5baa9-111">tooconfigure Azure AD-integrering med Zscaler ZSCloud, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5baa9-111">tooconfigure Azure AD integration with Zscaler ZSCloud, you need hello following items:</span></span>

- <span data-ttu-id="5baa9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5baa9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5baa9-113">En Zscaler ZSCloud enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5baa9-113">A Zscaler ZSCloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5baa9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5baa9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5baa9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5baa9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5baa9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5baa9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5baa9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5baa9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5baa9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5baa9-118">Scenario description</span></span>
<span data-ttu-id="5baa9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5baa9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5baa9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5baa9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5baa9-121">Att lägga till Zscaler ZSCloud från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5baa9-121">Adding Zscaler ZSCloud from hello gallery</span></span>
2. <span data-ttu-id="5baa9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5baa9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-zscloud-from-hello-gallery"></a><span data-ttu-id="5baa9-123">Att lägga till Zscaler ZSCloud från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5baa9-123">Adding Zscaler ZSCloud from hello gallery</span></span>
<span data-ttu-id="5baa9-124">tooconfigure hello integrering av Zscaler ZSCloud i Azure AD, behöver du tooadd Zscaler ZSCloud hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5baa9-124">tooconfigure hello integration of Zscaler ZSCloud into Azure AD, you need tooadd Zscaler ZSCloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5baa9-125">**tooadd Zscaler ZSCloud från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5baa9-125">**tooadd Zscaler ZSCloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5baa9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5baa9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5baa9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5baa9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5baa9-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5baa9-133">Skriv i sökrutan hello **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-133">In hello search box, type **Zscaler ZSCloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. <span data-ttu-id="5baa9-135">Markera hello resultat på panelen **Zscaler ZSCloud**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5baa9-135">In hello results panel, select **Zscaler ZSCloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5baa9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5baa9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5baa9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler ZSCloud baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5baa9-138">In this section, you configure and test Azure AD single sign-on with Zscaler ZSCloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5baa9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zscaler ZSCloud är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5baa9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler ZSCloud is tooa user in Azure AD.</span></span> <span data-ttu-id="5baa9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zscaler ZSCloud toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5baa9-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler ZSCloud needs toobe established.</span></span>

<span data-ttu-id="5baa9-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="5baa9-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zscaler ZSCloud.</span></span>

<span data-ttu-id="5baa9-142">tooconfigure och testa Azure AD enkel inloggning med Zscaler ZSCloud, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5baa9-142">tooconfigure and test Azure AD single sign-on with Zscaler ZSCloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5baa9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5baa9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5baa9-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  -tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="5baa9-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
2. <span data-ttu-id="5baa9-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5baa9-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)**  - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5baa9-146">**[Skapa en testanvändare Zscaler ZSCloud](#creating-a-zscaler-zscloud-test-user)**  -toohave en motsvarighet för Britta Simon i Zscaler ZSCloud som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5baa9-146">**[Creating a Zscaler ZSCloud test user](#creating-a-zscaler-zscloud-test-user)** - toohave a counterpart of Britta Simon in Zscaler ZSCloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5baa9-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5baa9-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5baa9-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5baa9-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5baa9-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5baa9-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5baa9-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Zscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="5baa9-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler ZSCloud application.</span></span>

<span data-ttu-id="5baa9-151">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zscaler ZSCloud:**</span><span class="sxs-lookup"><span data-stu-id="5baa9-151">**tooconfigure Azure AD single sign-on with Zscaler ZSCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="5baa9-152">I hello Azure-portalen på hello **Zscaler ZSCloud** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-152">In hello Azure portal, on hello **Zscaler ZSCloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5baa9-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5baa9-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. <span data-ttu-id="5baa9-156">På hello **Zscaler ZSCloud domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5baa9-156">On hello **Zscaler ZSCloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     <span data-ttu-id="5baa9-158">I hello **inloggnings-URL** textruta typen hello URL som används av dina användare toosign på tooyour ZScaler ZSCloud program.</span><span class="sxs-lookup"><span data-stu-id="5baa9-158">In hello **Sign-on URL** textbox, type hello URL used by your users toosign-on tooyour ZScaler ZSCloud application.</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="5baa9-159">Du har tooupdate värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="5baa9-159">You have tooupdate this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="5baa9-160">Kontakta [Zscaler ZSCloud klienten supportteamet](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="5baa9-160">Contact [Zscaler ZSCloud Client support team](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint) tooget this value.</span></span> 
 
4. <span data-ttu-id="5baa9-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="5baa9-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. <span data-ttu-id="5baa9-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5baa9-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5baa9-165">På hello **Zscaler ZSCloud Configuration** klickar du på **konfigurera Zscaler ZSCloud** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5baa9-165">On hello **Zscaler ZSCloud Configuration** section, click **Configure Zscaler ZSCloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5baa9-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5baa9-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. <span data-ttu-id="5baa9-168">I en annan webbläsarfönster logga in tooyour ZScaler ZSCloud företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="5baa9-168">In a different web browser window, log in tooyour ZScaler ZSCloud company site as an administrator.</span></span>

8. <span data-ttu-id="5baa9-169">Hello-menyn överst hello **Administration**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-169">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="5baa9-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="5baa9-170">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="5baa9-171">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="5baa9-172">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="5baa9-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="5baa9-173">I hello **Välj autentiseringsalternativ för din organisation** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5baa9-173">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="5baa9-174">![Autentisering](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="5baa9-174">![Authentication](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="5baa9-175">a.</span><span class="sxs-lookup"><span data-stu-id="5baa9-175">a.</span></span> <span data-ttu-id="5baa9-176">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="5baa9-177">b.</span><span class="sxs-lookup"><span data-stu-id="5baa9-177">b.</span></span> <span data-ttu-id="5baa9-178">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="5baa9-179">På hello **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg hello och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="5baa9-179">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="5baa9-180">![Enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="5baa9-180">![Single Sign-On](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="5baa9-181">a.</span><span class="sxs-lookup"><span data-stu-id="5baa9-181">a.</span></span> <span data-ttu-id="5baa9-182">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värdet till hello **URL för hello SAML toowhich portalanvändare skickas för autentisering** textruta.</span><span class="sxs-lookup"><span data-stu-id="5baa9-182">Paste hello **SAML Single Sign-On Service URL** value into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="5baa9-183">b.</span><span class="sxs-lookup"><span data-stu-id="5baa9-183">b.</span></span> <span data-ttu-id="5baa9-184">I hello **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-184">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="5baa9-185">c.</span><span class="sxs-lookup"><span data-stu-id="5baa9-185">c.</span></span> <span data-ttu-id="5baa9-186">tooupload hämtade certifikatet, klicka på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-186">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="5baa9-187">d.</span><span class="sxs-lookup"><span data-stu-id="5baa9-187">d.</span></span> <span data-ttu-id="5baa9-188">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="5baa9-189">På hello **konfigurera användarautentisering** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5baa9-189">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="5baa9-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="5baa9-190">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="5baa9-191">a.</span><span class="sxs-lookup"><span data-stu-id="5baa9-191">a.</span></span> <span data-ttu-id="5baa9-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-192">Click **Save**.</span></span>

    <span data-ttu-id="5baa9-193">b.</span><span class="sxs-lookup"><span data-stu-id="5baa9-193">b.</span></span> <span data-ttu-id="5baa9-194">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="5baa9-195">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="5baa9-195">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="5baa9-196">tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="5baa9-196">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="5baa9-197">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="5baa9-198">Välj **Internetalternativ** från hello **verktyg** menyn för öppna hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-198">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="5baa9-199">![Internet-alternativ](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="5baa9-199">![Internet Options](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="5baa9-200">Klicka på hello **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="5baa9-200">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="5baa9-201">![Anslutningar](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="5baa9-201">![Connections](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="5baa9-202">Klicka på **LAN-inställningar** tooopen hello **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-202">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="5baa9-203">I hello avsnittet för Proxy-server, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5baa9-203">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="5baa9-204">![Proxyserver](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="5baa9-204">![Proxy server](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="5baa9-205">a.</span><span class="sxs-lookup"><span data-stu-id="5baa9-205">a.</span></span> <span data-ttu-id="5baa9-206">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="5baa9-207">b.</span><span class="sxs-lookup"><span data-stu-id="5baa9-207">b.</span></span> <span data-ttu-id="5baa9-208">Skriv i hello adress textruta **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-208">In hello Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="5baa9-209">c.</span><span class="sxs-lookup"><span data-stu-id="5baa9-209">c.</span></span> <span data-ttu-id="5baa9-210">Skriv i hello Port textruta **80**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-210">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="5baa9-211">d.</span><span class="sxs-lookup"><span data-stu-id="5baa9-211">d.</span></span> <span data-ttu-id="5baa9-212">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="5baa9-213">e.</span><span class="sxs-lookup"><span data-stu-id="5baa9-213">e.</span></span> <span data-ttu-id="5baa9-214">Klicka på **OK** tooclose hello **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-214">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="5baa9-215">Klicka på **OK** tooclose hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-215">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5baa9-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5baa9-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="5baa9-217">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5baa9-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5baa9-219">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5baa9-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5baa9-220">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5baa9-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5baa9-222">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-222">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5baa9-224">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-224">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5baa9-226">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5baa9-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5baa9-228">a.</span><span class="sxs-lookup"><span data-stu-id="5baa9-228">a.</span></span> <span data-ttu-id="5baa9-229">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5baa9-230">b.</span><span class="sxs-lookup"><span data-stu-id="5baa9-230">b.</span></span> <span data-ttu-id="5baa9-231">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5baa9-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5baa9-232">c.</span><span class="sxs-lookup"><span data-stu-id="5baa9-232">c.</span></span> <span data-ttu-id="5baa9-233">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5baa9-234">d.</span><span class="sxs-lookup"><span data-stu-id="5baa9-234">d.</span></span> <span data-ttu-id="5baa9-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-235">Click **Create**.</span></span>

### <a name="creating-a-zscaler-zscloud-test-user"></a><span data-ttu-id="5baa9-236">Skapa en testanvändare Zscaler ZSCloud</span><span class="sxs-lookup"><span data-stu-id="5baa9-236">Creating a Zscaler ZSCloud test user</span></span>

<span data-ttu-id="5baa9-237">tooenable Azure AD-användare toolog i tooZScaler ZSCloud, måste de vara etablerade tooZScaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="5baa9-237">tooenable Azure AD users toolog in tooZScaler ZSCloud, they must be provisioned tooZScaler ZSCloud.</span></span>  
<span data-ttu-id="5baa9-238">Hello gäller ZScaler ZSCloud är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5baa9-238">In hello case of ZScaler ZSCloud, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="5baa9-239">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="5baa9-239">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="5baa9-240">Logga in tooyour **Zscaler** klient.</span><span class="sxs-lookup"><span data-stu-id="5baa9-240">Log in tooyour **Zscaler** tenant.</span></span>

2. <span data-ttu-id="5baa9-241">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-241">Click **Administration**.</span></span>   
   
    <span data-ttu-id="5baa9-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="5baa9-242">![Administration](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="5baa9-243">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-243">Click **User Management**.</span></span>   
        
     <span data-ttu-id="5baa9-244">![Lägg till](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="5baa9-244">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

4. <span data-ttu-id="5baa9-245">I hello **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-245">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="5baa9-246">![Lägg till](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="5baa9-246">![Add](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="5baa9-247">I hello lägga till användare, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5baa9-247">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="5baa9-248">![Lägg till användare](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="5baa9-248">![Add User](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="5baa9-249">a.</span><span class="sxs-lookup"><span data-stu-id="5baa9-249">a.</span></span> <span data-ttu-id="5baa9-250">Typen hello **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och hello **avdelning** på en giltig AAD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="5baa9-250">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="5baa9-251">b.</span><span class="sxs-lookup"><span data-stu-id="5baa9-251">b.</span></span> <span data-ttu-id="5baa9-252">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-252">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="5baa9-253">Du kan använda något annat ZScaler ZSCloud användarens konto skapas verktyg eller API: er som tillhandahålls av ZScaler ZSCloud tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="5baa9-253">You can use any other ZScaler ZSCloud user account creation tools or APIs provided by ZScaler ZSCloud tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5baa9-254">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5baa9-254">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5baa9-255">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZscaler ZSCloud.</span><span class="sxs-lookup"><span data-stu-id="5baa9-255">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler ZSCloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5baa9-257">**tooassign Britta Simon tooZscaler ZSCloud, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="5baa9-257">**tooassign Britta Simon tooZscaler ZSCloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="5baa9-258">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-258">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5baa9-260">Välj i listan med program hello **Zscaler ZSCloud**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-260">In hello applications list, select **Zscaler ZSCloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. <span data-ttu-id="5baa9-262">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5baa9-262">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5baa9-264">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5baa9-264">Click **Add** button.</span></span> <span data-ttu-id="5baa9-265">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5baa9-267">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-267">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5baa9-268">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5baa9-269">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5baa9-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5baa9-270">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5baa9-270">Testing single sign-on</span></span>

<span data-ttu-id="5baa9-271">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5baa9-271">If you want tootest your single sign-on settings, open hello Access Panel.</span></span>

<span data-ttu-id="5baa9-272">Du bör få automatiskt inloggade tooyour Zscaler ZSCloud programmet när du klickar på hello Zscaler ZSCloud panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5baa9-272">When you click hello Zscaler ZSCloud tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler ZSCloud application.</span></span>

<span data-ttu-id="5baa9-273">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5baa9-273">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5baa9-274">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5baa9-274">Additional resources</span></span>

* [<span data-ttu-id="5baa9-275">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5baa9-275">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5baa9-276">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5baa9-276">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png

