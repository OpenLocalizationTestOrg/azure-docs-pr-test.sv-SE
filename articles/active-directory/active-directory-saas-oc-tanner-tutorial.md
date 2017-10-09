---
title: "Självstudier: Azure Active Directory-integrering med O.C. Turesson - AppreciateHub | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och O.C. Turesson - AppreciateHub."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dee8fbca-0b60-4a21-8917-1fb6919de5a0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: 45052cf56e35746d7df5910162e40e3bbcad1aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oc-tanner---appreciatehub"></a><span data-ttu-id="44158-105">Självstudier: Azure Active Directory-integrering med O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-105">Tutorial: Azure Active Directory integration with O.C.</span></span> <span data-ttu-id="44158-106">Turesson - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="44158-106">Tanner - AppreciateHub</span></span>

<span data-ttu-id="44158-107">I kursen får du lära dig hur toointegrate O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-107">In this tutorial, you learn how toointegrate O.C.</span></span> <span data-ttu-id="44158-108">Turesson - AppreciateHub med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="44158-108">Tanner - AppreciateHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="44158-109">Integrera O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-109">Integrating O.C.</span></span> <span data-ttu-id="44158-110">Turesson - AppreciateHub med Azure AD innehåller hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="44158-110">Tanner - AppreciateHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="44158-111">Du kan styra i Azure AD som har åtkomst till tooO.C.</span><span class="sxs-lookup"><span data-stu-id="44158-111">You can control in Azure AD who has access tooO.C.</span></span> <span data-ttu-id="44158-112">Turesson - AppreciateHub</span><span class="sxs-lookup"><span data-stu-id="44158-112">Tanner - AppreciateHub</span></span>
- <span data-ttu-id="44158-113">Du kan låta dina användare tooautomatically get inloggade tooO.C.</span><span class="sxs-lookup"><span data-stu-id="44158-113">You can enable your users tooautomatically get signed-on tooO.C.</span></span> <span data-ttu-id="44158-114">Turesson - AppreciateHub (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="44158-114">Tanner - AppreciateHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="44158-115">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="44158-115">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="44158-116">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="44158-116">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44158-117">Krav</span><span class="sxs-lookup"><span data-stu-id="44158-117">Prerequisites</span></span>

<span data-ttu-id="44158-118">tooconfigure Azure AD-integrering med O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-118">tooconfigure Azure AD integration with O.C.</span></span> <span data-ttu-id="44158-119">Turesson - AppreciateHub, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="44158-119">Tanner - AppreciateHub, you need hello following items:</span></span>

- <span data-ttu-id="44158-120">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="44158-120">An Azure AD subscription</span></span>
- <span data-ttu-id="44158-121">EN O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-121">A O.C.</span></span> <span data-ttu-id="44158-122">Turesson - AppreciateHub enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="44158-122">Tanner - AppreciateHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="44158-123">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="44158-123">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="44158-124">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="44158-124">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="44158-125">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="44158-125">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="44158-126">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="44158-126">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="44158-127">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="44158-127">Scenario description</span></span>
<span data-ttu-id="44158-128">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="44158-128">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="44158-129">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="44158-129">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="44158-130">Lägga till O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-130">Adding O.C.</span></span> <span data-ttu-id="44158-131">Turesson - AppreciateHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="44158-131">Tanner - AppreciateHub from hello gallery</span></span>
2. <span data-ttu-id="44158-132">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44158-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oc-tanner---appreciatehub-from-hello-gallery"></a><span data-ttu-id="44158-133">Lägga till O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-133">Adding O.C.</span></span> <span data-ttu-id="44158-134">Turesson - AppreciateHub från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="44158-134">Tanner - AppreciateHub from hello gallery</span></span>
<span data-ttu-id="44158-135">tooconfigure hello integrering av O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-135">tooconfigure hello integration of O.C.</span></span> <span data-ttu-id="44158-136">Turesson - AppreciateHub till Azure AD, behöver du tooadd O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-136">Tanner - AppreciateHub into Azure AD, you need tooadd O.C.</span></span> <span data-ttu-id="44158-137">Turesson - AppreciateHub hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="44158-137">Tanner - AppreciateHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="44158-138">**tooadd O.C. Turesson - AppreciateHub från galleriet hello utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44158-138">**tooadd O.C. Tanner - AppreciateHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="44158-139">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="44158-139">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="44158-141">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="44158-141">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="44158-142">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="44158-142">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="44158-144">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44158-144">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="44158-146">Skriv i sökrutan hello **O.C. Turesson - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="44158-146">In hello search box, type **O.C. Tanner - AppreciateHub**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_search.png)

5. <span data-ttu-id="44158-148">Markera hello resultat på panelen **O.C. Turesson - AppreciateHub**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="44158-148">In hello results panel, select **O.C. Tanner - AppreciateHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="44158-150">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44158-150">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="44158-151">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-151">In this section, you configure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="44158-152">Turesson - AppreciateHub baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="44158-152">Tanner - AppreciateHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="44158-153">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användare i O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-153">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in O.C.</span></span> <span data-ttu-id="44158-154">Turesson - AppreciateHub är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="44158-154">Tanner - AppreciateHub is tooa user in Azure AD.</span></span> <span data-ttu-id="44158-155">Med andra ord relaterade en länk relationen mellan en Azure AD-användare och hello användare i O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-155">In other words, a link relationship between an Azure AD user and hello related user in O.C.</span></span> <span data-ttu-id="44158-156">Turesson - AppreciateHub måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="44158-156">Tanner - AppreciateHub needs toobe established.</span></span>

<span data-ttu-id="44158-157">I O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-157">In O.C.</span></span> <span data-ttu-id="44158-158">Turesson - AppreciateHub, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="44158-158">Tanner - AppreciateHub, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="44158-159">tooconfigure och testa Azure AD enkel inloggning med O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-159">tooconfigure and test Azure AD single sign-on with O.C.</span></span> <span data-ttu-id="44158-160">Turesson - AppreciateHub, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="44158-160">Tanner - AppreciateHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="44158-161">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="44158-161">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="44158-162">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44158-162">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="44158-163">**[Skapa en O.C. Turesson - AppreciateHub testanvändare](#creating-a-oc-tanner---appreciatehub-test-user)**  -toohave en motsvarighet för Britta Simon i O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-163">**[Creating a O.C. Tanner - AppreciateHub test user](#creating-a-oc-tanner---appreciatehub-test-user)** - toohave a counterpart of Britta Simon in O.C.</span></span> <span data-ttu-id="44158-164">Turesson - AppreciateHub som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="44158-164">Tanner - AppreciateHub that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="44158-165">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="44158-165">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="44158-166">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="44158-166">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="44158-167">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44158-167">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="44158-168">I det här avsnittet kan du aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-168">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your O.C.</span></span> <span data-ttu-id="44158-169">Turesson - AppreciateHub program.</span><span class="sxs-lookup"><span data-stu-id="44158-169">Tanner - AppreciateHub application.</span></span>

<span data-ttu-id="44158-170">**tooconfigure Azure AD enkel inloggning med O.C. Turesson - AppreciateHub, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44158-170">**tooconfigure Azure AD single sign-on with O.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="44158-171">I hello Azure-portalen på hello **O.C. Turesson - AppreciateHub** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="44158-171">In hello Azure portal, on hello **O.C. Tanner - AppreciateHub** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="44158-173">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="44158-173">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_samlbase.png)

3. <span data-ttu-id="44158-175">På hello **O.C. Turesson - URL: er och AppreciateHub domän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="44158-175">On hello **O.C. Tanner - AppreciateHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_url.png)

    <span data-ttu-id="44158-177">a.</span><span class="sxs-lookup"><span data-stu-id="44158-177">a.</span></span> <span data-ttu-id="44158-178">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span><span class="sxs-lookup"><span data-stu-id="44158-178">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.appreciatehub.com/fed/sp/authnResponse20`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="44158-179">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="44158-179">This value is not real.</span></span> <span data-ttu-id="44158-180">Uppdatera det här värdet med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="44158-180">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="44158-181">Kontakta [O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="44158-181">Contact [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) tooget this value.</span></span>

    <span data-ttu-id="44158-182">b.</span><span class="sxs-lookup"><span data-stu-id="44158-182">b.</span></span> <span data-ttu-id="44158-183">Öppna hello metadatafil med hello följande länk: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span><span class="sxs-lookup"><span data-stu-id="44158-183">Open hello metadata file using hello following link: [https://fed.appreciatehub.com/fed/sp/metadata](https://fed.appreciatehub.com/fed/sp/metadata).</span></span>
   
    <span data-ttu-id="44158-184">c.</span><span class="sxs-lookup"><span data-stu-id="44158-184">c.</span></span> <span data-ttu-id="44158-185">Leta upp hello **md:AssertionConsumerService** nod.</span><span class="sxs-lookup"><span data-stu-id="44158-185">Locate hello **md:AssertionConsumerService** node.</span></span> 
   
    <span data-ttu-id="44158-186">d.</span><span class="sxs-lookup"><span data-stu-id="44158-186">d.</span></span> <span data-ttu-id="44158-187">Kopiera hello värdet för hello **plats** attribut.</span><span class="sxs-lookup"><span data-stu-id="44158-187">Copy hello value of hello **Location** attribute.</span></span> 
   
    ![Konfigurera Appinställningar för][12]
   
    <span data-ttu-id="44158-189">e.</span><span class="sxs-lookup"><span data-stu-id="44158-189">e.</span></span> <span data-ttu-id="44158-190">I hello **logga URL** textruta tidigare hello-värde som du har fått i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="44158-190">In hello **Sign On URL** textbox, past hello value you have obtained in hello previous step.</span></span>

4. <span data-ttu-id="44158-191">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="44158-191">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_certificate.png) 

5. <span data-ttu-id="44158-193">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="44158-193">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="44158-195">tooconfigure enkel inloggning på **O.C. Turesson - AppreciateHub** sida, behöver du toosend hello hämtas **XML-Metadata för** för[O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com).</span><span class="sxs-lookup"><span data-stu-id="44158-195">tooconfigure single sign-on on **O.C. Tanner - AppreciateHub** side, you need toosend hello downloaded **Metadata XML** too[O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com).</span></span>

> [!TIP]
> <span data-ttu-id="44158-196">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="44158-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="44158-197">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="44158-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="44158-198">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="44158-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="44158-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="44158-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="44158-200">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44158-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="44158-202">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44158-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="44158-203">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="44158-203">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="44158-205">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="44158-205">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="44158-207">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44158-207">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="44158-209">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="44158-209">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oc-tanner-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="44158-211">a.</span><span class="sxs-lookup"><span data-stu-id="44158-211">a.</span></span> <span data-ttu-id="44158-212">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="44158-212">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="44158-213">b.</span><span class="sxs-lookup"><span data-stu-id="44158-213">b.</span></span> <span data-ttu-id="44158-214">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="44158-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="44158-215">c.</span><span class="sxs-lookup"><span data-stu-id="44158-215">c.</span></span> <span data-ttu-id="44158-216">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="44158-216">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="44158-217">d.</span><span class="sxs-lookup"><span data-stu-id="44158-217">d.</span></span> <span data-ttu-id="44158-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="44158-218">Click **Create**.</span></span>
 
### <a name="creating-a-oc-tanner---appreciatehub-test-user"></a><span data-ttu-id="44158-219">Skapa en O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-219">Creating a O.C.</span></span> <span data-ttu-id="44158-220">Turesson - AppreciateHub testanvändare</span><span class="sxs-lookup"><span data-stu-id="44158-220">Tanner - AppreciateHub test user</span></span>

<span data-ttu-id="44158-221">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-221">hello objective of this section is toocreate a user called Britta Simon in O.C.</span></span> <span data-ttu-id="44158-222">Turesson - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="44158-222">Tanner - AppreciateHub.</span></span>

<span data-ttu-id="44158-223">**toocreate en användare kallas Britta Simon i O.C. Turesson - AppreciateHub, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44158-223">**toocreate a user called Britta Simon in O.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

<span data-ttu-id="44158-224">Be din [O.C. Turesson - AppreciateHub supportteamet](mailto:sso@octanner.com) toocreate en användare som har som nameID attributet hello samma värde som hello användarnamn för Britta Simon i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="44158-224">Ask your [O.C. Tanner - AppreciateHub support team](mailto:sso@octanner.com) toocreate a user that has as nameID attribute hello same value as hello user name of Britta Simon in Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="44158-225">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="44158-225">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="44158-226">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooO.C.</span><span class="sxs-lookup"><span data-stu-id="44158-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooO.C.</span></span> <span data-ttu-id="44158-227">Turesson - AppreciateHub.</span><span class="sxs-lookup"><span data-stu-id="44158-227">Tanner - AppreciateHub.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="44158-229">**tooassign Britta Simon tooO.C. Turesson - AppreciateHub, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44158-229">**tooassign Britta Simon tooO.C. Tanner - AppreciateHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="44158-230">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="44158-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="44158-232">Välj i listan med program hello **O.C. Turesson - AppreciateHub**.</span><span class="sxs-lookup"><span data-stu-id="44158-232">In hello applications list, select **O.C. Tanner - AppreciateHub**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oc-tanner-tutorial/tutorial_octannerappreciatehub_app.png) 

3. <span data-ttu-id="44158-234">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="44158-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="44158-236">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="44158-236">Click **Add** button.</span></span> <span data-ttu-id="44158-237">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44158-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="44158-239">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="44158-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="44158-240">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44158-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="44158-241">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44158-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="44158-242">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44158-242">Testing single sign-on</span></span>

<span data-ttu-id="44158-243">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="44158-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="44158-244">När du klickar på hello O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-244">When you click hello O.C.</span></span> <span data-ttu-id="44158-245">Turesson - AppreciateHub panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour O.C.</span><span class="sxs-lookup"><span data-stu-id="44158-245">Tanner - AppreciateHub tile in hello Access Panel, you should get automatically signed-on tooyour O.C.</span></span> <span data-ttu-id="44158-246">Turesson - AppreciateHub program.</span><span class="sxs-lookup"><span data-stu-id="44158-246">Tanner - AppreciateHub application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="44158-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="44158-247">Additional resources</span></span>

* [<span data-ttu-id="44158-248">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44158-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="44158-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="44158-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_octanner_08.png

[100]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oc-tanner-tutorial/tutorial_general_203.png

