---
title: "Självstudier: Azure Active Directory-integrering med Rightscale | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Rightscale."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 53b927804a1e0f895778a164386459a4ea816f98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a><span data-ttu-id="5c3a9-103">Självstudier: Azure Active Directory-integrering med Rightscale</span><span class="sxs-lookup"><span data-stu-id="5c3a9-103">Tutorial: Azure Active Directory integration with Rightscale</span></span>

<span data-ttu-id="5c3a9-104">I kursen får du lära dig hur toointegrate Rightscale med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5c3a9-104">In this tutorial, you learn how toointegrate Rightscale with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5c3a9-105">Integrera Rightscale med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5c3a9-105">Integrating Rightscale with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5c3a9-106">Du kan styra i Azure AD som har åtkomst till tooRightscale</span><span class="sxs-lookup"><span data-stu-id="5c3a9-106">You can control in Azure AD who has access tooRightscale</span></span>
- <span data-ttu-id="5c3a9-107">Du kan aktivera din användare tooautomatically get inloggade tooRightscale (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5c3a9-107">You can enable your users tooautomatically get signed-on tooRightscale (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5c3a9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5c3a9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5c3a9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5c3a9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c3a9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5c3a9-110">Prerequisites</span></span>

<span data-ttu-id="5c3a9-111">tooconfigure Azure AD-integrering med Rightscale, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5c3a9-111">tooconfigure Azure AD integration with Rightscale, you need hello following items:</span></span>

- <span data-ttu-id="5c3a9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c3a9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5c3a9-113">En Rightscale enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c3a9-113">A Rightscale single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5c3a9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5c3a9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5c3a9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5c3a9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5c3a9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c3a9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5c3a9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5c3a9-118">Scenario description</span></span>
<span data-ttu-id="5c3a9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5c3a9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c3a9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5c3a9-121">Att lägga till Rightscale från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5c3a9-121">Adding Rightscale from hello gallery</span></span>
2. <span data-ttu-id="5c3a9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c3a9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rightscale-from-hello-gallery"></a><span data-ttu-id="5c3a9-123">Att lägga till Rightscale från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5c3a9-123">Adding Rightscale from hello gallery</span></span>
<span data-ttu-id="5c3a9-124">tooconfigure hello integrering av Rightscale i Azure AD, behöver du tooadd Rightscale hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-124">tooconfigure hello integration of Rightscale into Azure AD, you need tooadd Rightscale from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5c3a9-125">**tooadd Rightscale från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c3a9-125">**tooadd Rightscale from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c3a9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5c3a9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5c3a9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5c3a9-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5c3a9-133">Skriv i sökrutan hello **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-133">In hello search box, type **Rightscale**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. <span data-ttu-id="5c3a9-135">Markera hello resultat på panelen **Rightscale**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-135">In hello results panel, select **Rightscale**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5c3a9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c3a9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5c3a9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Rightscale baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-138">In this section, you configure and test Azure AD single sign-on with Rightscale based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5c3a9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Rightscale är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Rightscale is tooa user in Azure AD.</span></span> <span data-ttu-id="5c3a9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Rightscale toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-140">In other words, a link relationship between an Azure AD user and hello related user in Rightscale needs toobe established.</span></span>

<span data-ttu-id="5c3a9-141">I Rightscale, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-141">In Rightscale, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5c3a9-142">tooconfigure och testa Azure AD enkel inloggning med Rightscale, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c3a9-142">tooconfigure and test Azure AD single sign-on with Rightscale, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5c3a9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5c3a9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5c3a9-145">**[Skapa en testanvändare Rightscale](#creating-a-rightscale-test-user)**  -toohave en motsvarighet för Britta Simon i Rightscale som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-145">**[Creating a Rightscale test user](#creating-a-rightscale-test-user)** - toohave a counterpart of Britta Simon in Rightscale that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5c3a9-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5c3a9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5c3a9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c3a9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5c3a9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Rightscale program.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Rightscale application.</span></span>

<span data-ttu-id="5c3a9-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Rightscale:**</span><span class="sxs-lookup"><span data-stu-id="5c3a9-150">**tooconfigure Azure AD single sign-on with Rightscale, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c3a9-151">I hello Azure-portalen på hello **Rightscale** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-151">In hello Azure portal, on hello **Rightscale** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5c3a9-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. <span data-ttu-id="5c3a9-155">På hello **Rightscale domän och URL: er** om du vill tooconfigure hello programmet i **IDP initierade läge** behöver du inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-155">On hello **Rightscale Domain and URLs** section, if you wish tooconfigure hello application in **IDP initiated mode** you do not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. <span data-ttu-id="5c3a9-157">På hello **Rightscale domän och URL: er** om du vill tooconfigure hello programmet i **SP initierade läge**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c3a9-157">On hello **Rightscale Domain and URLs** section, if you wish tooconfigure hello application in **SP initiated mode**, perform hello following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    <span data-ttu-id="5c3a9-159">a.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-159">a.</span></span> <span data-ttu-id="5c3a9-160">Klicka på hello **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-160">Click on hello **Show advanced URL settings**.</span></span>

    <span data-ttu-id="5c3a9-161">b.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-161">b.</span></span> <span data-ttu-id="5c3a9-162">I hello **logga URL** textruta typen hello URL:`https://login.rightscale.com/`</span><span class="sxs-lookup"><span data-stu-id="5c3a9-162">In hello **Sign On URL** textbox, type hello URL: `https://login.rightscale.com/`</span></span>

5. <span data-ttu-id="5c3a9-163">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-163">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. <span data-ttu-id="5c3a9-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="5c3a9-167">På hello **Rightscale Configuration** klickar du på **konfigurera Rightscale** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-167">On hello **Rightscale Configuration** section, click **Configure Rightscale** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5c3a9-168">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5c3a9-168">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="5c3a9-169">![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="5c3a9-169">![Configure Single Sign-On](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS></span></span>
8. <span data-ttu-id="5c3a9-170">tooget SSO konfigurerats för ditt program måste toosign på tooyour RightScale klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-170">tooget SSO configured for your application, you need toosign-on tooyour RightScale tenant as an administrator.</span></span>

    <span data-ttu-id="5c3a9-171">a.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-171">a.</span></span> <span data-ttu-id="5c3a9-172">Hello hello överst klickar du på menyn hello **inställningar** fliken och markera **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-172">In hello menu on hello top, click hello **Settings** tab and select **Single Sign-On**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    <span data-ttu-id="5c3a9-174">b.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-174">b.</span></span> <span data-ttu-id="5c3a9-175">Klicka på hello ”**nya**” knappen tooadd **din SAML identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-175">Click hello "**new**" button tooadd **Your SAML Identity Providers**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    <span data-ttu-id="5c3a9-177">c.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-177">c.</span></span> <span data-ttu-id="5c3a9-178">Hello textrutan för med **visningsnamn**, ange företagets namn.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-178">In hello textbox of **Display Name**, input your company name.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    <span data-ttu-id="5c3a9-180">d.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-180">d.</span></span> <span data-ttu-id="5c3a9-181">Välj **Tillåt RightScale-initierad SSO med hjälp av en identifiering ledtråd** och inkommande din **domännamn** i hello under textrutan.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-181">Select **Allow RightScale-initiated SSO using a discovery hint** and input your **domain name** in hello below textbox.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    <span data-ttu-id="5c3a9-183">e.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-183">e.</span></span> <span data-ttu-id="5c3a9-184">Klistra in hello värdet för **SAML enkel inloggning Tjänstwebbadress** som du har kopierat från Azure-portalen i **SAML SSO Endpoint** i RightScale.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-184">Paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal into **SAML SSO Endpoint** in RightScale.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    <span data-ttu-id="5c3a9-186">f.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-186">f.</span></span> <span data-ttu-id="5c3a9-187">Klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen i **ID för SAML entiteterna** i RightScale.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-187">Paste hello value of **SAML Entity ID** which you have copied from Azure portal into **SAML EntityID** in RightScale.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    <span data-ttu-id="5c3a9-189">g.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-189">g.</span></span> <span data-ttu-id="5c3a9-190">Klicka på **webbläsare** knappen tooupload hello certifikat som du laddade ned från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-190">Click **Browser** button tooupload hello certificate which you downloaded from Azure portal.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    <span data-ttu-id="5c3a9-192">h.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-192">h.</span></span> <span data-ttu-id="5c3a9-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-193">Click **Save**.</span></span>
<CE>
> [!TIP]
> <span data-ttu-id="5c3a9-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5c3a9-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5c3a9-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5c3a9-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5c3a9-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5c3a9-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c3a9-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="5c3a9-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5c3a9-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c3a9-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c3a9-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5c3a9-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5c3a9-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5c3a9-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c3a9-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5c3a9-209">a.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-209">a.</span></span> <span data-ttu-id="5c3a9-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5c3a9-211">b.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-211">b.</span></span> <span data-ttu-id="5c3a9-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5c3a9-213">c.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-213">c.</span></span> <span data-ttu-id="5c3a9-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5c3a9-215">d.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-215">d.</span></span> <span data-ttu-id="5c3a9-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-216">Click **Create**.</span></span>
 
### <a name="creating-a-rightscale-test-user"></a><span data-ttu-id="5c3a9-217">Skapa en testanvändare Rightscale</span><span class="sxs-lookup"><span data-stu-id="5c3a9-217">Creating a Rightscale test user</span></span>

<span data-ttu-id="5c3a9-218">I det här avsnittet skapar du en användare som kallas Britta Simon i RightScale.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-218">In this section, you create a user called Britta Simon in RightScale.</span></span> <span data-ttu-id="5c3a9-219">Arbeta med [Rightscale klienten supportteamet](mailto:support@rightscale.com) tooadd hello användare i hello RightScale plattform.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-219">Work with [Rightscale Client support team](mailto:support@rightscale.com) tooadd hello users in hello RightScale platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5c3a9-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c3a9-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5c3a9-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRightscale.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRightscale.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5c3a9-223">**tooassign Britta Simon tooRightscale utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c3a9-223">**tooassign Britta Simon tooRightscale, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c3a9-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5c3a9-226">Välj i listan med program hello **Rightscale**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-226">In hello applications list, select **Rightscale**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. <span data-ttu-id="5c3a9-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5c3a9-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-230">Click **Add** button.</span></span> <span data-ttu-id="5c3a9-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5c3a9-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5c3a9-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5c3a9-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5c3a9-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c3a9-236">Testing single sign-on</span></span>

<span data-ttu-id="5c3a9-237">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-237">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="5c3a9-238">Du bör få automatiskt inloggade tooyour RightScale programmet när du klickar på hello RightScale panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5c3a9-238">When you click hello RightScale tile in hello Access Panel, you should get automatically signed-on tooyour RightScale application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c3a9-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5c3a9-239">Additional resources</span></span>

* [<span data-ttu-id="5c3a9-240">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c3a9-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c3a9-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5c3a9-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png

