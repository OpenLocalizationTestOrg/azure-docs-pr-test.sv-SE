---
title: "Självstudier: Azure Active Directory-integrering med Zscaler | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zscaler."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 68c453f7-aff1-4614-92d3-9b86f3ad99dc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e2894534f5d6711fd6af618cd699fa5837b5bf26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler"></a><span data-ttu-id="a61be-103">Självstudier: Azure Active Directory-integrering med Zscaler</span><span class="sxs-lookup"><span data-stu-id="a61be-103">Tutorial: Azure Active Directory integration with Zscaler</span></span>

<span data-ttu-id="a61be-104">I kursen får du lära dig hur toointegrate Zscaler med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a61be-104">In this tutorial, you learn how toointegrate Zscaler with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a61be-105">Integrera Zscaler med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a61be-105">Integrating Zscaler with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a61be-106">Du kan styra i Azure AD som har åtkomst till tooZscaler</span><span class="sxs-lookup"><span data-stu-id="a61be-106">You can control in Azure AD who has access tooZscaler</span></span>
- <span data-ttu-id="a61be-107">Du kan aktivera din användare tooautomatically get inloggade tooZscaler (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a61be-107">You can enable your users tooautomatically get signed-on tooZscaler (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a61be-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a61be-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a61be-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a61be-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a61be-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a61be-110">Prerequisites</span></span>

<span data-ttu-id="a61be-111">tooconfigure Azure AD-integrering med Zscaler, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a61be-111">tooconfigure Azure AD integration with Zscaler, you need hello following items:</span></span>

- <span data-ttu-id="a61be-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a61be-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a61be-113">En Zscaler enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a61be-113">A Zscaler single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a61be-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a61be-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a61be-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a61be-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a61be-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a61be-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a61be-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a61be-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a61be-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a61be-118">Scenario description</span></span>
<span data-ttu-id="a61be-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a61be-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a61be-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a61be-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a61be-121">Att lägga till Zscaler från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a61be-121">Adding Zscaler from hello gallery</span></span>
2. <span data-ttu-id="a61be-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a61be-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-from-hello-gallery"></a><span data-ttu-id="a61be-123">Att lägga till Zscaler från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a61be-123">Adding Zscaler from hello gallery</span></span>
<span data-ttu-id="a61be-124">tooconfigure hello integrering av Zscaler i Azure AD, behöver du tooadd Zscaler hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a61be-124">tooconfigure hello integration of Zscaler into Azure AD, you need tooadd Zscaler from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a61be-125">**tooadd Zscaler från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a61be-125">**tooadd Zscaler from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a61be-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a61be-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a61be-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a61be-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a61be-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a61be-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a61be-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a61be-133">Skriv i sökrutan hello **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="a61be-133">In hello search box, type **Zscaler**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_search.png)

5. <span data-ttu-id="a61be-135">Markera hello resultat på panelen **Zscaler**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a61be-135">In hello results panel, select **Zscaler**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a61be-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a61be-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a61be-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a61be-138">In this section, you configure and test Azure AD single sign-on with Zscaler based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a61be-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zscaler är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a61be-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler is tooa user in Azure AD.</span></span> <span data-ttu-id="a61be-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zscaler toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a61be-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler needs toobe established.</span></span>

<span data-ttu-id="a61be-141">I Zscaler, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a61be-141">In Zscaler, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a61be-142">tooconfigure och testa Azure AD enkel inloggning med Zscaler, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a61be-142">tooconfigure and test Azure AD single sign-on with Zscaler, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a61be-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a61be-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a61be-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  -tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="a61be-144">**[Configuring proxy settings](#configuring-proxy-settings)** - tooconfigure hello proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="a61be-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a61be-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="a61be-146">**[Skapa en testanvändare Zscaler](#creating-a-zscaler-test-user)**  -toohave en motsvarighet för Britta Simon i Zscaler som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a61be-146">**[Creating a Zscaler test user](#creating-a-zscaler-test-user)** - toohave a counterpart of Britta Simon in Zscaler that is linked toohello Azure AD representation of user.</span></span>
5. <span data-ttu-id="a61be-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a61be-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="a61be-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a61be-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a61be-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a61be-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a61be-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zscaler program.</span><span class="sxs-lookup"><span data-stu-id="a61be-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zscaler application.</span></span>

<span data-ttu-id="a61be-151">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zscaler:**</span><span class="sxs-lookup"><span data-stu-id="a61be-151">**tooconfigure Azure AD single sign-on with Zscaler, perform hello following steps:**</span></span>

1. <span data-ttu-id="a61be-152">I hello Azure-portalen på hello **Zscaler** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a61be-152">In hello Azure portal, on hello **Zscaler** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a61be-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a61be-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_samlbase.png)

3. <span data-ttu-id="a61be-156">På hello **Zscaler domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a61be-156">On hello **Zscaler Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_url.png)

    <span data-ttu-id="a61be-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.zsclaer.net`</span><span class="sxs-lookup"><span data-stu-id="a61be-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.zsclaer.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a61be-159">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a61be-159">This value is not real.</span></span> <span data-ttu-id="a61be-160">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a61be-160">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a61be-161">Kontakta [Zscaler klienten supportteamet](https://www.zscaler.com/company/contact) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="a61be-161">Contact [Zscaler Client support team](https://www.zscaler.com/company/contact) tooget this value.</span></span> 

4. <span data-ttu-id="a61be-162">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a61be-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_certificate.png) 

5. <span data-ttu-id="a61be-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a61be-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a61be-166">På hello **Zscaler Configuration** klickar du på **konfigurera Zscaler** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a61be-166">On hello **Zscaler Configuration** section, click **Configure Zscaler** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a61be-167">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a61be-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_configure.png) 

7. <span data-ttu-id="a61be-169">Logga in tooyour ZScaler företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a61be-169">In a different web browser window, log in tooyour ZScaler company site as an administrator.</span></span>

8. <span data-ttu-id="a61be-170">Hello-menyn överst hello **Administration**.</span><span class="sxs-lookup"><span data-stu-id="a61be-170">In hello menu on hello top, click **Administration**.</span></span>
   
    <span data-ttu-id="a61be-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="a61be-171">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="a61be-172">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="a61be-172">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="a61be-173">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="a61be-173">![Manage Users & Authentication](./media/active-directory-saas-zscaler-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="a61be-174">I hello **Välj autentiseringsalternativ för din organisation** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a61be-174">In hello **Choose Authentication Options for your Organization** section, perform hello following steps:</span></span>   
                
    <span data-ttu-id="a61be-175">![Autentisering](./media/active-directory-saas-zscaler-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="a61be-175">![Authentication](./media/active-directory-saas-zscaler-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="a61be-176">a.</span><span class="sxs-lookup"><span data-stu-id="a61be-176">a.</span></span> <span data-ttu-id="a61be-177">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a61be-177">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="a61be-178">b.</span><span class="sxs-lookup"><span data-stu-id="a61be-178">b.</span></span> <span data-ttu-id="a61be-179">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a61be-179">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="a61be-180">På hello **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg hello och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="a61be-180">On hello **Configure SAML Single Sign-On Parameters** dialog page, perform hello following steps, and then click **Done**</span></span>

    <span data-ttu-id="a61be-181">![Enkel inloggning](./media/active-directory-saas-zscaler-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a61be-181">![Single Sign-On](./media/active-directory-saas-zscaler-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="a61be-182">a.</span><span class="sxs-lookup"><span data-stu-id="a61be-182">a.</span></span> <span data-ttu-id="a61be-183">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **URL för hello SAML toowhich portalanvändare skickas för autentisering** textruta.</span><span class="sxs-lookup"><span data-stu-id="a61be-183">Paste hello **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **URL of hello SAML Portal toowhich users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="a61be-184">b.</span><span class="sxs-lookup"><span data-stu-id="a61be-184">b.</span></span> <span data-ttu-id="a61be-185">I hello **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="a61be-185">In hello **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="a61be-186">c.</span><span class="sxs-lookup"><span data-stu-id="a61be-186">c.</span></span> <span data-ttu-id="a61be-187">tooupload hämtade certifikatet, klicka på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="a61be-187">tooupload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="a61be-188">d.</span><span class="sxs-lookup"><span data-stu-id="a61be-188">d.</span></span> <span data-ttu-id="a61be-189">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="a61be-189">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="a61be-190">På hello **konfigurera användarautentisering** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a61be-190">On hello **Configure User Authentication** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="a61be-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="a61be-191">![Administration](./media/active-directory-saas-zscaler-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="a61be-192">a.</span><span class="sxs-lookup"><span data-stu-id="a61be-192">a.</span></span> <span data-ttu-id="a61be-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a61be-193">Click **Save**.</span></span>

    <span data-ttu-id="a61be-194">b.</span><span class="sxs-lookup"><span data-stu-id="a61be-194">b.</span></span> <span data-ttu-id="a61be-195">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="a61be-195">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="a61be-196">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="a61be-196">Configuring proxy settings</span></span>
### <a name="tooconfigure-hello-proxy-settings-in-internet-explorer"></a><span data-ttu-id="a61be-197">tooconfigure hello proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="a61be-197">tooconfigure hello proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="a61be-198">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a61be-198">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="a61be-199">Välj **Internetalternativ** från hello **verktyg** menyn för öppna hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-199">Select **Internet options** from hello **Tools** menu for open hello **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="a61be-200">![Internet-alternativ](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="a61be-200">![Internet Options](./media/active-directory-saas-zscaler-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="a61be-201">Klicka på hello **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="a61be-201">Click hello **Connections** tab.</span></span>   
  
     <span data-ttu-id="a61be-202">![Anslutningar](./media/active-directory-saas-zscaler-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="a61be-202">![Connections](./media/active-directory-saas-zscaler-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="a61be-203">Klicka på **LAN-inställningar** tooopen hello **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-203">Click **LAN settings** tooopen hello **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="a61be-204">I hello avsnittet för Proxy-server, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a61be-204">In hello Proxy server section, perform hello following steps:</span></span>   
   
    <span data-ttu-id="a61be-205">![Proxyserver](./media/active-directory-saas-zscaler-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="a61be-205">![Proxy server](./media/active-directory-saas-zscaler-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="a61be-206">a.</span><span class="sxs-lookup"><span data-stu-id="a61be-206">a.</span></span> <span data-ttu-id="a61be-207">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="a61be-207">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="a61be-208">b.</span><span class="sxs-lookup"><span data-stu-id="a61be-208">b.</span></span> <span data-ttu-id="a61be-209">Skriv i hello adress textruta **gateway.zscaler.net**.</span><span class="sxs-lookup"><span data-stu-id="a61be-209">In hello Address textbox, type **gateway.zscaler.net**.</span></span>

    <span data-ttu-id="a61be-210">c.</span><span class="sxs-lookup"><span data-stu-id="a61be-210">c.</span></span> <span data-ttu-id="a61be-211">Skriv i hello Port textruta **80**.</span><span class="sxs-lookup"><span data-stu-id="a61be-211">In hello Port textbox, type **80**.</span></span>

    <span data-ttu-id="a61be-212">d.</span><span class="sxs-lookup"><span data-stu-id="a61be-212">d.</span></span> <span data-ttu-id="a61be-213">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="a61be-213">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="a61be-214">e.</span><span class="sxs-lookup"><span data-stu-id="a61be-214">e.</span></span> <span data-ttu-id="a61be-215">Klicka på **OK** tooclose hello **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-215">Click **OK** tooclose hello **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="a61be-216">Klicka på **OK** tooclose hello **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-216">Click **OK** tooclose hello **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="a61be-217">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a61be-217">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a61be-218">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a61be-218">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a61be-219">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a61be-219">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a61be-220">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a61be-220">Creating an Azure AD test user</span></span>
<span data-ttu-id="a61be-221">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a61be-221">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a61be-223">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a61be-223">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a61be-224">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a61be-224">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a61be-226">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a61be-226">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a61be-228">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-228">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a61be-230">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a61be-230">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a61be-232">a.</span><span class="sxs-lookup"><span data-stu-id="a61be-232">a.</span></span> <span data-ttu-id="a61be-233">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a61be-233">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a61be-234">b.</span><span class="sxs-lookup"><span data-stu-id="a61be-234">b.</span></span> <span data-ttu-id="a61be-235">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a61be-235">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a61be-236">c.</span><span class="sxs-lookup"><span data-stu-id="a61be-236">c.</span></span> <span data-ttu-id="a61be-237">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a61be-237">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a61be-238">d.</span><span class="sxs-lookup"><span data-stu-id="a61be-238">d.</span></span> <span data-ttu-id="a61be-239">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a61be-239">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-test-user"></a><span data-ttu-id="a61be-240">Skapa en testanvändare Zscaler</span><span class="sxs-lookup"><span data-stu-id="a61be-240">Creating a Zscaler test user</span></span>

<span data-ttu-id="a61be-241">tooenable Azure AD-användare toolog i tooZScaler, måste de vara etablerade tooZScaler.</span><span class="sxs-lookup"><span data-stu-id="a61be-241">tooenable Azure AD users toolog in tooZScaler, they must be provisioned tooZScaler.</span></span>  
<span data-ttu-id="a61be-242">Hello gäller ZScaler är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a61be-242">In hello case of ZScaler, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="a61be-243">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="a61be-243">tooconfigure user provisioning, perform hello following steps:</span></span>

1. <span data-ttu-id="a61be-244">Logga in tooyour **Zscaler** klient.</span><span class="sxs-lookup"><span data-stu-id="a61be-244">Log in tooyour **Zscaler** tenant.</span></span>

2. <span data-ttu-id="a61be-245">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="a61be-245">Click **Administration**.</span></span>   
   
    <span data-ttu-id="a61be-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="a61be-246">![Administration](./media/active-directory-saas-zscaler-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="a61be-247">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="a61be-247">Click **User Management**.</span></span>   
        
     <span data-ttu-id="a61be-248">![Lägg till](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="a61be-248">![Add](./media/active-directory-saas-zscaler-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="a61be-249">I hello **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a61be-249">In hello **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="a61be-250">![Lägg till](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="a61be-250">![Add](./media/active-directory-saas-zscaler-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="a61be-251">I hello lägga till användare, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a61be-251">In hello Add User section, perform hello following steps:</span></span>
        
    <span data-ttu-id="a61be-252">![Lägg till användare](./media/active-directory-saas-zscaler-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="a61be-252">![Add User](./media/active-directory-saas-zscaler-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="a61be-253">a.</span><span class="sxs-lookup"><span data-stu-id="a61be-253">a.</span></span> <span data-ttu-id="a61be-254">Typen hello **UserID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper**och hello **avdelning** på en giltig AAD-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="a61be-254">Type hello **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and hello **Department** of a valid AAD account you want tooprovision.</span></span>

    <span data-ttu-id="a61be-255">b.</span><span class="sxs-lookup"><span data-stu-id="a61be-255">b.</span></span> <span data-ttu-id="a61be-256">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a61be-256">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="a61be-257">Du kan använda något annat ZScaler användarens konto skapas verktyg eller API: er som tillhandahålls av ZScaler tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a61be-257">You can use any other ZScaler user account creation tools or APIs provided by ZScaler tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a61be-258">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a61be-258">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a61be-259">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZscaler.</span><span class="sxs-lookup"><span data-stu-id="a61be-259">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZscaler.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a61be-261">**tooassign Britta Simon tooZscaler utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a61be-261">**tooassign Britta Simon tooZscaler, perform hello following steps:**</span></span>

1. <span data-ttu-id="a61be-262">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a61be-262">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a61be-264">Välj i listan med program hello **Zscaler**.</span><span class="sxs-lookup"><span data-stu-id="a61be-264">In hello applications list, select **Zscaler**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-tutorial/tutorial_zscaler_app.png) 

3. <span data-ttu-id="a61be-266">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a61be-266">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a61be-268">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a61be-268">Click **Add** button.</span></span> <span data-ttu-id="a61be-269">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-269">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a61be-271">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a61be-271">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a61be-272">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-272">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a61be-273">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a61be-273">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a61be-274">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a61be-274">Testing single sign-on</span></span>

<span data-ttu-id="a61be-275">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a61be-275">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a61be-276">Du bör få automatiskt inloggade tooyour Zscaler programmet när du klickar på hello Zscaler panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a61be-276">When you click hello Zscaler tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler application.</span></span>
<span data-ttu-id="a61be-277">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a61be-277">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a61be-278">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a61be-278">Additional resources</span></span>

* [<span data-ttu-id="a61be-279">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a61be-279">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a61be-280">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a61be-280">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-tutorial/tutorial_general_203.png

