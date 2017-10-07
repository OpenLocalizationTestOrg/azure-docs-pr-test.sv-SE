---
title: "Självstudier: Azure Active Directory-integrering med arbetsytan Lms | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och arbetsytan LMS."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: 8f4a09266a108e2c92326b0909dd0650b1c84d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a><span data-ttu-id="a5c3a-103">Självstudier: Azure Active Directory-integrering med arbetsytan LMS</span><span class="sxs-lookup"><span data-stu-id="a5c3a-103">Tutorial: Azure Active Directory integration with Canvas LMS</span></span>

<span data-ttu-id="a5c3a-104">I kursen får du lära dig hur toointegrate arbetsytan med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a5c3a-104">In this tutorial, you learn how toointegrate Canvas with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a5c3a-105">Integrera arbetsytan med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-105">Integrating Canvas with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a5c3a-106">Du kan styra i Azure AD som har åtkomst till tooCanvas</span><span class="sxs-lookup"><span data-stu-id="a5c3a-106">You can control in Azure AD who has access tooCanvas</span></span>
- <span data-ttu-id="a5c3a-107">Du kan aktivera din användare tooautomatically get inloggade tooCanvas (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a5c3a-107">You can enable your users tooautomatically get signed-on tooCanvas (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a5c3a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a5c3a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a5c3a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a5c3a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5c3a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a5c3a-110">Prerequisites</span></span>

<span data-ttu-id="a5c3a-111">tooconfigure Azure AD-integrering med arbetsyta, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-111">tooconfigure Azure AD integration with Canvas, you need hello following items:</span></span>

- <span data-ttu-id="a5c3a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a5c3a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a5c3a-113">En arbetsyta enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a5c3a-113">A Canvas single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a5c3a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a5c3a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a5c3a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a5c3a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a5c3a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a5c3a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a5c3a-118">Scenario description</span></span>
<span data-ttu-id="a5c3a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a5c3a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a5c3a-121">Att lägga till arbetsytan från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a5c3a-121">Adding Canvas from hello gallery</span></span>
2. <span data-ttu-id="a5c3a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a5c3a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-canvas-from-hello-gallery"></a><span data-ttu-id="a5c3a-123">Att lägga till arbetsytan från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a5c3a-123">Adding Canvas from hello gallery</span></span>
<span data-ttu-id="a5c3a-124">tooconfigure hello integrering av arbetsytan i Azure AD, behöver du tooadd arbetsytan hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-124">tooconfigure hello integration of Canvas into Azure AD, you need tooadd Canvas from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a5c3a-125">**tooadd arbetsytan från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a5c3a-125">**tooadd Canvas from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a5c3a-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a5c3a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a5c3a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a5c3a-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a5c3a-133">Skriv i sökrutan hello **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-133">In hello search box, type **Canvas**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_search.png)

5. <span data-ttu-id="a5c3a-135">Markera hello resultat på panelen **arbetsytan**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-135">In hello results panel, select **Canvas**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a5c3a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a5c3a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a5c3a-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med arbetsytan baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-138">In this section, you configure and test Azure AD single sign-on with Canvas based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a5c3a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i arbetsytan är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Canvas is tooa user in Azure AD.</span></span> <span data-ttu-id="a5c3a-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i arbetsytan toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-140">In other words, a link relationship between an Azure AD user and hello related user in Canvas needs toobe established.</span></span>

<span data-ttu-id="a5c3a-141">I arbetsytan och tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-141">In Canvas, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a5c3a-142">tooconfigure och testa Azure AD enkel inloggning med arbetsyta, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-142">tooconfigure and test Azure AD single sign-on with Canvas, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a5c3a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a5c3a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a5c3a-145">**[Skapa en arbetsyta testanvändare](#creating-a-canvas-test-user)**  -toohave en motsvarighet för Britta Simon arbetsytan som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-145">**[Creating a Canvas test user](#creating-a-canvas-test-user)** - toohave a counterpart of Britta Simon in Canvas that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a5c3a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a5c3a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a5c3a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a5c3a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a5c3a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Canvas application.</span></span>

<span data-ttu-id="a5c3a-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med arbetsytan:**</span><span class="sxs-lookup"><span data-stu-id="a5c3a-150">**tooconfigure Azure AD single sign-on with Canvas, perform hello following steps:**</span></span>

1. <span data-ttu-id="a5c3a-151">I hello Azure-portalen på hello **arbetsytan** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-151">In hello Azure portal, on hello **Canvas** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a5c3a-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

3. <span data-ttu-id="a5c3a-155">På hello **arbetsytan domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-155">On hello **Canvas Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_url.png)

    <span data-ttu-id="a5c3a-157">a.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-157">a.</span></span> <span data-ttu-id="a5c3a-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.instructure.com`</span><span class="sxs-lookup"><span data-stu-id="a5c3a-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.instructure.com`</span></span>

    <span data-ttu-id="a5c3a-159">b.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-159">b.</span></span> <span data-ttu-id="a5c3a-160">I hello **identifierare** textruta hello TYPVÄRDE med hello följande mönster:`https://<tenant-name>.instructure.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="a5c3a-160">In hello **Identifier** textbox, type hello value using hello following pattern: `https://<tenant-name>.instructure.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a5c3a-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-161">These values are not real.</span></span> <span data-ttu-id="a5c3a-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a5c3a-163">Kontakta [arbetsytan klienten supportteamet](https://community.canvaslms.com/community/help) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-163">Contact [Canvas Client support team](https://community.canvaslms.com/community/help) tooget these values.</span></span> 
 
4. <span data-ttu-id="a5c3a-164">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

5. <span data-ttu-id="a5c3a-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a5c3a-168">På hello **arbetsytan Configuration** klickar du på **konfigurera arbetsytan** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-168">On hello **Canvas Configuration** section, click **Configure Canvas** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a5c3a-169">Kopiera hello **ändra lösenord URL, Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a5c3a-169">Copy hello **Change Password URL, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
7. <span data-ttu-id="a5c3a-171">Logga in tooyour arbetsytan företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-171">In a different web browser window, log in tooyour Canvas company site as an administrator.</span></span>

8. <span data-ttu-id="a5c3a-172">Gå för**kurser \> hanterade konton \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-172">Go too**Courses \> Managed Accounts \> Microsoft**.</span></span>
   
    <span data-ttu-id="a5c3a-173">![Arbetsytan](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "arbetsytan")</span><span class="sxs-lookup"><span data-stu-id="a5c3a-173">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

9. <span data-ttu-id="a5c3a-174">Hello navigeringsfönstret hello vänster och välj **autentisering**, och klicka sedan på **Lägg till ny SAML-Config**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-174">In hello navigation pane on hello left, select **Authentication**, and then click **Add New SAML Config**.</span></span>
   
    <span data-ttu-id="a5c3a-175">![Autentisering](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="a5c3a-175">![Authentication](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentication")</span></span>

10. <span data-ttu-id="a5c3a-176">Utför följande hello hello aktuella Integration på sidan:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-176">On hello Current Integration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="a5c3a-177">![Aktuella Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "aktuella integrering")</span><span class="sxs-lookup"><span data-stu-id="a5c3a-177">![Current Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Current Integration")</span></span>

    <span data-ttu-id="a5c3a-178">a.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-178">a.</span></span> <span data-ttu-id="a5c3a-179">I **IdP enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-179">In **IdP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a5c3a-180">b.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-180">b.</span></span> <span data-ttu-id="a5c3a-181">I **loggen URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-181">In **Log On URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal .</span></span>

    <span data-ttu-id="a5c3a-182">c.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-182">c.</span></span> <span data-ttu-id="a5c3a-183">I **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-183">In **Log Out URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a5c3a-184">d.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-184">d.</span></span> <span data-ttu-id="a5c3a-185">I **ändra lösenord länk** textruta klistra in hello värdet för **ändra lösenord URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-185">In **Change Password Link** textbox, paste hello value of **Change Password URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="a5c3a-186">e.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-186">e.</span></span> <span data-ttu-id="a5c3a-187">I **certifikat fingeravtryck** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-187">In **Certificate Fingerprint** textbox, paste hello **Thumbprint** value of certificate which you have copied from Azure portal.</span></span>      
        
    <span data-ttu-id="a5c3a-188">f.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-188">f.</span></span> <span data-ttu-id="a5c3a-189">Från hello **inloggningen attributet** väljer **NameID**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-189">From hello **Login Attribute** list, select **NameID**.</span></span>

    <span data-ttu-id="a5c3a-190">g.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-190">g.</span></span> <span data-ttu-id="a5c3a-191">Från hello **identifierare Format** väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-191">From hello **Identifier Format** list, select **emailAddress**.</span></span>

    <span data-ttu-id="a5c3a-192">h.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-192">h.</span></span> <span data-ttu-id="a5c3a-193">Klicka på **spara autentiseringsinställningarna**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-193">Click **Save Authentication Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="a5c3a-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a5c3a-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a5c3a-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a5c3a-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a5c3a-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a5c3a-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5c3a-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="a5c3a-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a5c3a-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a5c3a-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a5c3a-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a5c3a-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a5c3a-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a5c3a-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-canvas-lms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a5c3a-209">a.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-209">a.</span></span> <span data-ttu-id="a5c3a-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a5c3a-211">b.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-211">b.</span></span> <span data-ttu-id="a5c3a-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a5c3a-213">c.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-213">c.</span></span> <span data-ttu-id="a5c3a-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a5c3a-215">d.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-215">d.</span></span> <span data-ttu-id="a5c3a-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-216">Click **Create**.</span></span>
 
### <a name="creating-a-canvas-test-user"></a><span data-ttu-id="a5c3a-217">Skapa en arbetsyta testanvändare</span><span class="sxs-lookup"><span data-stu-id="a5c3a-217">Creating a Canvas test user</span></span>

<span data-ttu-id="a5c3a-218">tooenable Azure AD-användare toolog i tooCanvas, måste de etableras i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-218">tooenable Azure AD users toolog in tooCanvas, they must be provisioned into Canvas.</span></span>

<span data-ttu-id="a5c3a-219">Om arbetsytan är användaretablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-219">In case of Canvas, user provisioning is a manual task.</span></span>

<span data-ttu-id="a5c3a-220">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="a5c3a-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="a5c3a-221">Logga in tooyour **arbetsytan** klient.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-221">Log in tooyour **Canvas** tenant.</span></span>

2. <span data-ttu-id="a5c3a-222">Gå för**kurser \> hanterade konton \> Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-222">Go too**Courses \> Managed Accounts \> Microsoft**.</span></span>
   
   <span data-ttu-id="a5c3a-223">![Arbetsytan](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "arbetsytan")</span><span class="sxs-lookup"><span data-stu-id="a5c3a-223">![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")</span></span>

3. <span data-ttu-id="a5c3a-224">Klicka på **användare**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-224">Click **Users**.</span></span>
   
   <span data-ttu-id="a5c3a-225">![Användare](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "användare")</span><span class="sxs-lookup"><span data-stu-id="a5c3a-225">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Users")</span></span>

4. <span data-ttu-id="a5c3a-226">Klicka på **lägga till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-226">Click **Add New User**.</span></span>
   
   <span data-ttu-id="a5c3a-227">![Användare](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "användare")</span><span class="sxs-lookup"><span data-stu-id="a5c3a-227">![Users](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Users")</span></span>

5. <span data-ttu-id="a5c3a-228">Utför följande hello på hello Lägg till en ny användare dialogrutan sida:</span><span class="sxs-lookup"><span data-stu-id="a5c3a-228">On hello Add a New User dialog page, perform hello following steps:</span></span>
   
   <span data-ttu-id="a5c3a-229">![Lägg till användare](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="a5c3a-229">![Add User](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Add User")</span></span>
   
   <span data-ttu-id="a5c3a-230">a.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-230">a.</span></span> <span data-ttu-id="a5c3a-231">I hello **fullständiga namn** textruta anger hello namnet på användaren som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-231">In hello **Full Name** textbox, enter hello name of user like **BrittaSimon**.</span></span>

   <span data-ttu-id="a5c3a-232">b.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-232">b.</span></span> <span data-ttu-id="a5c3a-233">I hello **e-post** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a5c3a-233">In hello **Email** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="a5c3a-234">c.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-234">c.</span></span> <span data-ttu-id="a5c3a-235">I hello **inloggning** textruta ange hello användarens Azure AD e-postadress som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a5c3a-235">In hello **Login** textbox, enter hello user’s Azure AD email address like **brittasimon@contoso.com**.</span></span>

   <span data-ttu-id="a5c3a-236">d.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-236">d.</span></span> <span data-ttu-id="a5c3a-237">Välj **hello användare om det här kontot skapas**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-237">Select **Email hello user about this account creation**.</span></span>

   <span data-ttu-id="a5c3a-238">e.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-238">e.</span></span> <span data-ttu-id="a5c3a-239">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-239">Click **Add User**.</span></span>

>[!NOTE]
><span data-ttu-id="a5c3a-240">Du kan använda något annat arbetsytan användarens konto skapas verktyg eller API: er som tillhandahålls av arbetsytan tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-240">You can use any other Canvas user account creation tools or APIs provided by Canvas tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a5c3a-241">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a5c3a-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a5c3a-242">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCanvas.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCanvas.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a5c3a-244">**tooassign Britta Simon tooCanvas utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a5c3a-244">**tooassign Britta Simon tooCanvas, perform hello following steps:**</span></span>

1. <span data-ttu-id="a5c3a-245">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a5c3a-247">Välj i listan med program hello **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-247">In hello applications list, select **Canvas**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-canvas-lms-tutorial/tutorial_canvaslms_app.png) 

3. <span data-ttu-id="a5c3a-249">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a5c3a-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-251">Click **Add** button.</span></span> <span data-ttu-id="a5c3a-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a5c3a-254">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a5c3a-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a5c3a-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a5c3a-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a5c3a-257">Testing single sign-on</span></span>

<span data-ttu-id="a5c3a-258">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a5c3a-259">Du bör få automatiskt inloggade tooyour arbetsytan programmet när du klickar på hello arbetsytan panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a5c3a-259">When you click hello Canvas tile in hello Access Panel, you should get automatically signed-on tooyour Canvas application.</span></span>
<span data-ttu-id="a5c3a-260">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a5c3a-260">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5c3a-261">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a5c3a-261">Additional resources</span></span>

* [<span data-ttu-id="a5c3a-262">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a5c3a-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a5c3a-263">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a5c3a-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-canvas-lms-tutorial/tutorial_general_203.png

