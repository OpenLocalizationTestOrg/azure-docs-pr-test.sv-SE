---
title: "Självstudier: Azure Active Directory-integrering med BC i hello molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BC i hello molnet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7dc40d2c-6349-40cb-b304-b098bd03a66c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/1/2017
ms.author: jeedes
ms.openlocfilehash: e81ffb522b2c96c7e9b2919abd8d3b199c295eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bc-in-hello-cloud"></a><span data-ttu-id="65aec-103">Självstudier: Azure Active Directory-integrering med BC i hello moln</span><span class="sxs-lookup"><span data-stu-id="65aec-103">Tutorial: Azure Active Directory integration with BC in hello Cloud</span></span>

<span data-ttu-id="65aec-104">I kursen får du lära dig hur toointegrate BC i hello moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="65aec-104">In this tutorial, you learn how toointegrate BC in hello Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="65aec-105">Integrera BC i hello moln med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="65aec-105">Integrating BC in hello Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="65aec-106">Du kan styra i Azure AD som har åtkomst till tooBC i hello moln</span><span class="sxs-lookup"><span data-stu-id="65aec-106">You can control in Azure AD who has access tooBC in hello Cloud</span></span>
- <span data-ttu-id="65aec-107">Du kan aktivera din användare tooautomatically get inloggade tooBC i hello molnet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="65aec-107">You can enable your users tooautomatically get signed-on tooBC in hello Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="65aec-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="65aec-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="65aec-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="65aec-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65aec-110">Krav</span><span class="sxs-lookup"><span data-stu-id="65aec-110">Prerequisites</span></span>

<span data-ttu-id="65aec-111">tooconfigure Azure AD-integrering med BC i hello molnet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="65aec-111">tooconfigure Azure AD integration with BC in hello Cloud, you need hello following items:</span></span>

- <span data-ttu-id="65aec-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="65aec-112">An Azure AD subscription</span></span>
- <span data-ttu-id="65aec-113">En BC i hello molnet enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="65aec-113">A BC in hello Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="65aec-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="65aec-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="65aec-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="65aec-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="65aec-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="65aec-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="65aec-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="65aec-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="65aec-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="65aec-118">Scenario description</span></span>
<span data-ttu-id="65aec-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="65aec-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="65aec-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="65aec-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="65aec-121">Att lägga till BC i hello moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="65aec-121">Adding BC in hello Cloud from hello gallery</span></span>
2. <span data-ttu-id="65aec-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65aec-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bc-in-hello-cloud-from-hello-gallery"></a><span data-ttu-id="65aec-123">Att lägga till BC i hello moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="65aec-123">Adding BC in hello Cloud from hello gallery</span></span>
<span data-ttu-id="65aec-124">tooconfigure hello integrering av BC i hello molnet till Azure AD, behöver du tooadd BC i hello molnet hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="65aec-124">tooconfigure hello integration of BC in hello Cloud into Azure AD, you need tooadd BC in hello Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="65aec-125">**tooadd BC i hello moln från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="65aec-125">**tooadd BC in hello Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="65aec-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="65aec-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="65aec-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="65aec-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="65aec-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="65aec-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="65aec-131">tooadd nya program, klickar du på **Lägg till** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65aec-131">tooadd new application, click **Add** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="65aec-133">Skriv i sökrutan hello **BC i hello molnet**.</span><span class="sxs-lookup"><span data-stu-id="65aec-133">In hello search box, type **BC in hello Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_search.png)

5. <span data-ttu-id="65aec-135">Markera hello resultat på panelen **BC i hello molnet**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="65aec-135">In hello results panel, select **BC in hello Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="65aec-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65aec-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="65aec-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BC i hello molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="65aec-138">In this section, you configure and test Azure AD single sign-on with BC in hello Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="65aec-139">För enkel inloggning toowork Azure AD måste tooknow vilka hello motsvarighet användaren i BC i hello molnet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="65aec-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BC in hello Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="65aec-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BC i hello molnet toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="65aec-140">In other words, a link relationship between an Azure AD user and hello related user in BC in hello Cloud needs toobe established.</span></span>

<span data-ttu-id="65aec-141">Tilldela hello värdet för hello i BC i hello molnet **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="65aec-141">In BC in hello Cloud, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="65aec-142">tooconfigure och testa Azure AD enkel inloggning med BC i hello molnet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="65aec-142">tooconfigure and test Azure AD single sign-on with BC in hello Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="65aec-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="65aec-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="65aec-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65aec-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="65aec-145">**[Skapa en BC i hello molnet testanvändare](#creating-a-bc-in-the-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i BC i hello moln som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="65aec-145">**[Creating a BC in hello Cloud test user](#creating-a-bc-in-the-cloud-test-user)** - toohave a counterpart of Britta Simon in BC in hello Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="65aec-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="65aec-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="65aec-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="65aec-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="65aec-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65aec-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="65aec-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din BC i hello molnapp.</span><span class="sxs-lookup"><span data-stu-id="65aec-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BC in hello Cloud application.</span></span>

<span data-ttu-id="65aec-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med BC i hello molnet:**</span><span class="sxs-lookup"><span data-stu-id="65aec-150">**tooconfigure Azure AD single sign-on with BC in hello Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="65aec-151">I hello Azure-portalen på hello **BC i hello molnet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="65aec-151">In hello Azure portal, on hello **BC in hello Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="65aec-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="65aec-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_samlbase.png)

3. <span data-ttu-id="65aec-155">På hello **BC i URL: er och hello molnet domän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="65aec-155">On hello **BC in hello Cloud Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_url.png)

    <span data-ttu-id="65aec-157">a.</span><span class="sxs-lookup"><span data-stu-id="65aec-157">a.</span></span> <span data-ttu-id="65aec-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span><span class="sxs-lookup"><span data-stu-id="65aec-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span></span>

    <span data-ttu-id="65aec-159">b.</span><span class="sxs-lookup"><span data-stu-id="65aec-159">b.</span></span> <span data-ttu-id="65aec-160">I hello **identifierare** textruta Skriv en URL som:`https://app.bcinthecloud.com`</span><span class="sxs-lookup"><span data-stu-id="65aec-160">In hello **Identifier** textbox, type a URL as: `https://app.bcinthecloud.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="65aec-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="65aec-161">This value is not real.</span></span> <span data-ttu-id="65aec-162">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="65aec-162">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="65aec-163">Kontakta [BC i hello Cloud klienten supportteamet](https://www.bcinthecloud.com/supportcenter/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="65aec-163">Contact [BC in hello Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) tooget this value.</span></span> 
 
4. <span data-ttu-id="65aec-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="65aec-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_certificate.png) 

5. <span data-ttu-id="65aec-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="65aec-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="65aec-168">tooconfigure enkel inloggning på **BC i hello molnet** sida, behöver du toosend hello hämtas **XML-Metadata för** för[BC i hello molnet supportteamet](https://www.bcinthecloud.com/supportcenter/).</span><span class="sxs-lookup"><span data-stu-id="65aec-168">tooconfigure single sign-on on **BC in hello Cloud** side, you need toosend hello downloaded **Metadata XML** too[BC in hello Cloud support team](https://www.bcinthecloud.com/supportcenter/).</span></span>

> [!TIP]
> <span data-ttu-id="65aec-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="65aec-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="65aec-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="65aec-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="65aec-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="65aec-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="65aec-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="65aec-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="65aec-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65aec-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="65aec-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="65aec-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="65aec-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="65aec-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="65aec-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="65aec-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="65aec-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65aec-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="65aec-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="65aec-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="65aec-184">a.</span><span class="sxs-lookup"><span data-stu-id="65aec-184">a.</span></span> <span data-ttu-id="65aec-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="65aec-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="65aec-186">b.</span><span class="sxs-lookup"><span data-stu-id="65aec-186">b.</span></span> <span data-ttu-id="65aec-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="65aec-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="65aec-188">c.</span><span class="sxs-lookup"><span data-stu-id="65aec-188">c.</span></span> <span data-ttu-id="65aec-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="65aec-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="65aec-190">d.</span><span class="sxs-lookup"><span data-stu-id="65aec-190">d.</span></span> <span data-ttu-id="65aec-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="65aec-191">Click **Create**.</span></span>
 
### <a name="creating-a-bc-in-hello-cloud-test-user"></a><span data-ttu-id="65aec-192">Skapa en BC i hello molnet testanvändare</span><span class="sxs-lookup"><span data-stu-id="65aec-192">Creating a BC in hello Cloud test user</span></span>

<span data-ttu-id="65aec-193">I det här avsnittet kan du skapa en användare som kallas Britta Simon i BC i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="65aec-193">In this section, you create a user called Britta Simon in BC in hello Cloud.</span></span> <span data-ttu-id="65aec-194">Arbeta med [BC i hello Cloud klienten supportteamet](https://www.bcinthecloud.com/supportcenter/) att lägga till hello användare i hello BC i hello molnapp.</span><span class="sxs-lookup"><span data-stu-id="65aec-194">Work with [BC in hello Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) to add hello users in hello BC in hello Cloud application.</span></span> <span data-ttu-id="65aec-195">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="65aec-195">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="65aec-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="65aec-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="65aec-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBC i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="65aec-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBC in hello Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="65aec-199">**tooassign Britta Simon tooBC i hello moln, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="65aec-199">**tooassign Britta Simon tooBC in hello Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="65aec-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="65aec-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="65aec-202">Välj i listan med program hello **BC i hello molnet**.</span><span class="sxs-lookup"><span data-stu-id="65aec-202">In hello applications list, select **BC in hello Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_app.png) 

3. <span data-ttu-id="65aec-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="65aec-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="65aec-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="65aec-206">Click **Add** button.</span></span> <span data-ttu-id="65aec-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65aec-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="65aec-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="65aec-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="65aec-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65aec-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="65aec-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65aec-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="65aec-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65aec-212">Testing single sign-on</span></span>

<span data-ttu-id="65aec-213">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="65aec-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

 <span data-ttu-id="65aec-214">När du klickar på hello BC i hello molnet panelen i hello åtkomstpanelen bör du hämta automatiskt inloggade tooyour BC i hello molnapp.</span><span class="sxs-lookup"><span data-stu-id="65aec-214">When you click hello BC in hello Cloud tile in hello Access Panel, you should get automatically signed-on tooyour BC in hello Cloud application.</span></span> <span data-ttu-id="65aec-215">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="65aec-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65aec-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="65aec-216">Additional resources</span></span>

* [<span data-ttu-id="65aec-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="65aec-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="65aec-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="65aec-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_203.png

