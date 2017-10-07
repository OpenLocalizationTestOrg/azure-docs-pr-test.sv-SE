---
title: "Självstudier: Azure Active Directory-integrering med TOPdesk - offentliga | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TOPdesk - allmänheten."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ef0dd06157ecc3b33814590039f5cbae64e8c916
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a><span data-ttu-id="228f0-103">Självstudier: Azure Active Directory-integrering med TOPdesk - offentliga</span><span class="sxs-lookup"><span data-stu-id="228f0-103">Tutorial: Azure Active Directory integration with TOPdesk - Public</span></span>

<span data-ttu-id="228f0-104">I kursen får du lära dig hur toointegrate TOPdesk - offentliga med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="228f0-104">In this tutorial, you learn how toointegrate TOPdesk - Public with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="228f0-105">Integrera TOPdesk - offentliga med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="228f0-105">Integrating TOPdesk - Public with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="228f0-106">Du kan styra i Azure AD som har åtkomst till tooTOPdesk - allmänheten.</span><span class="sxs-lookup"><span data-stu-id="228f0-106">You can control in Azure AD who has access tooTOPdesk - Public.</span></span>
- <span data-ttu-id="228f0-107">Du kan låta dina användare tooautomatically get inloggade tooTOPdesk - offentlig (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="228f0-107">You can enable your users tooautomatically get signed-on tooTOPdesk - Public (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="228f0-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="228f0-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="228f0-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="228f0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="228f0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="228f0-110">Prerequisites</span></span>

<span data-ttu-id="228f0-111">tooconfigure Azure AD-integrering med TOPdesk - offentliga måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="228f0-111">tooconfigure Azure AD integration with TOPdesk - Public, you need hello following items:</span></span>

- <span data-ttu-id="228f0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="228f0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="228f0-113">En TOPdesk - offentliga enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="228f0-113">A TOPdesk - Public single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="228f0-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="228f0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="228f0-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="228f0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="228f0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="228f0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="228f0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="228f0-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="228f0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="228f0-118">Scenario description</span></span>
<span data-ttu-id="228f0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="228f0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="228f0-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="228f0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="228f0-121">Lägger till TOPdesk - offentliga från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="228f0-121">Adding TOPdesk - Public from hello gallery</span></span>
2. <span data-ttu-id="228f0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="228f0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-topdesk---public-from-hello-gallery"></a><span data-ttu-id="228f0-123">Lägger till TOPdesk - offentliga från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="228f0-123">Adding TOPdesk - Public from hello gallery</span></span>
<span data-ttu-id="228f0-124">tooconfigure hello integrering av TOPdesk - offentliga till Azure AD måste tooadd TOPdesk - offentliga hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="228f0-124">tooconfigure hello integration of TOPdesk - Public into Azure AD, you need tooadd TOPdesk - Public from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="228f0-125">**tooadd TOPdesk - offentliga från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="228f0-125">**tooadd TOPdesk - Public from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="228f0-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="228f0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="228f0-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="228f0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="228f0-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="228f0-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="228f0-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="228f0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="228f0-133">Skriv i sökrutan hello **TOPdesk - offentliga**väljer **TOPdesk - offentliga** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="228f0-133">In hello search box, type **TOPdesk - Public**, select **TOPdesk - Public** from result panel then click **Add** button tooadd hello application.</span></span>

    ![TOPdesk - allmänheten i hello resultatlistan](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="228f0-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="228f0-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="228f0-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TOPdesk - offentliga baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="228f0-136">In this section, you configure and test Azure AD single sign-on with TOPdesk - Public based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="228f0-137">Azure AD måste tooknow vilka hello motsvarighet användare i TOPdesk för enkel inloggning toowork - offentliga är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="228f0-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TOPdesk - Public is tooa user in Azure AD.</span></span> <span data-ttu-id="228f0-138">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i TOPdesk - offentliga måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="228f0-138">In other words, a link relationship between an Azure AD user and hello related user in TOPdesk - Public needs toobe established.</span></span>

<span data-ttu-id="228f0-139">I TOPdesk - offentliga, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="228f0-139">In TOPdesk - Public, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="228f0-140">tooconfigure och testa Azure AD enkel inloggning med TOPdesk - offentlig, måste du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="228f0-140">tooconfigure and test Azure AD single sign-on with TOPdesk - Public, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="228f0-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="228f0-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="228f0-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="228f0-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="228f0-143">**[Skapa en TOPdesk - offentliga testanvändare](#create-a-topdesk---public-test-user)**  - toohave en motsvarighet för Britta Simon i TOPdesk - offentliga som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="228f0-143">**[Create a TOPdesk - Public test user](#create-a-topdesk---public-test-user)** - toohave a counterpart of Britta Simon in TOPdesk - Public that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="228f0-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="228f0-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="228f0-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="228f0-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="228f0-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="228f0-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="228f0-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din TOPdesk - offentliga program.</span><span class="sxs-lookup"><span data-stu-id="228f0-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TOPdesk - Public application.</span></span>

<span data-ttu-id="228f0-148">**tooconfigure Azure AD enkel inloggning med TOPdesk - offentliga utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="228f0-148">**tooconfigure Azure AD single sign-on with TOPdesk - Public, perform hello following steps:**</span></span>

1. <span data-ttu-id="228f0-149">I hello Azure-portalen på hello **TOPdesk - offentliga** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="228f0-149">In hello Azure portal, on hello **TOPdesk - Public** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="228f0-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="228f0-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. <span data-ttu-id="228f0-153">På hello **TOPdesk - URL: er och den offentliga domänen** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="228f0-153">On hello **TOPdesk - Public Domain and URLs** section, perform hello following steps:</span></span>

    ![TOPdesk - URL: er och den offentliga domänen enkel inloggning information](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    <span data-ttu-id="228f0-155">a.</span><span class="sxs-lookup"><span data-stu-id="228f0-155">a.</span></span> <span data-ttu-id="228f0-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.topdesk.net`</span><span class="sxs-lookup"><span data-stu-id="228f0-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net`</span></span>
    
    <span data-ttu-id="228f0-157">b.</span><span class="sxs-lookup"><span data-stu-id="228f0-157">b.</span></span> <span data-ttu-id="228f0-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.topdesk.net/tas/public/login/verify`</span><span class="sxs-lookup"><span data-stu-id="228f0-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net/tas/public/login/verify`</span></span>

    <span data-ttu-id="228f0-159">c.</span><span class="sxs-lookup"><span data-stu-id="228f0-159">c.</span></span> <span data-ttu-id="228f0-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.topdesk.net/tas/public/login/saml`</span><span class="sxs-lookup"><span data-stu-id="228f0-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.topdesk.net/tas/public/login/saml`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="228f0-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="228f0-161">These values are not real.</span></span> <span data-ttu-id="228f0-162">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="228f0-162">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="228f0-163">Svars-URL är explaned senare i självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="228f0-163">Reply URL is explaned later in tutorial.</span></span> <span data-ttu-id="228f0-164">Kontakta [TOPdesk - offentliga klienten supportteamet](https://help.topdesk.com/saas/enterprise/user/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="228f0-164">Contact [TOPdesk - Public Client support team](https://help.topdesk.com/saas/enterprise/user/) tooget these values.</span></span>  

4. <span data-ttu-id="228f0-165">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="228f0-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. <span data-ttu-id="228f0-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="228f0-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="228f0-169">På hello **TOPdesk - offentliga konfiguration** klickar du på **konfigurera TOPdesk - offentliga** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="228f0-169">On hello **TOPdesk - Public Configuration** section, click **Configure TOPdesk - Public** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="228f0-170">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="228f0-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TOPdesk - offentliga konfiguration](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. <span data-ttu-id="228f0-172">Logga in tooyour **TOPdesk - offentliga** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="228f0-172">Sign on tooyour **TOPdesk - Public** company site as an administrator.</span></span>

8. <span data-ttu-id="228f0-173">I hello **TOPdesk** -menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="228f0-173">In hello **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="228f0-174">![Inställningar för](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="228f0-174">![Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "Settings")</span></span>

9. <span data-ttu-id="228f0-175">Klicka på **inloggningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="228f0-175">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="228f0-176">![Inloggningsinställningar](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "inloggningsinställningar")</span><span class="sxs-lookup"><span data-stu-id="228f0-176">![Login Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "Login Settings")</span></span>

10. <span data-ttu-id="228f0-177">Expandera hello **inloggningsinställningar** -menyn och klicka sedan på **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="228f0-177">Expand hello **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="228f0-178">![Allmän](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "Allmänt")</span><span class="sxs-lookup"><span data-stu-id="228f0-178">![General](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "General")</span></span>

11. <span data-ttu-id="228f0-179">I hello **offentliga** avsnitt i hello **SAML inloggningen** configuration avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="228f0-179">In hello **Public** section of hello **SAML login** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="228f0-180">![Tekniska inställningar](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "tekniska inställningar")</span><span class="sxs-lookup"><span data-stu-id="228f0-180">![Technical Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "Technical Settings")</span></span>
   
    <span data-ttu-id="228f0-181">a.</span><span class="sxs-lookup"><span data-stu-id="228f0-181">a.</span></span> <span data-ttu-id="228f0-182">Klicka på **hämta** toodownload hello offentliga metadatafil och sedan spara det lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="228f0-182">Click **Download** toodownload hello public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="228f0-183">b.</span><span class="sxs-lookup"><span data-stu-id="228f0-183">b.</span></span> <span data-ttu-id="228f0-184">Öppna hello hämtas metadatafilen och leta upp hello **AssertionConsumerService** nod.</span><span class="sxs-lookup"><span data-stu-id="228f0-184">Open hello downloaded metadata file, and then locate hello **AssertionConsumerService** node.</span></span>

    <span data-ttu-id="228f0-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span><span class="sxs-lookup"><span data-stu-id="228f0-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span></span>
   
    <span data-ttu-id="228f0-186">c.</span><span class="sxs-lookup"><span data-stu-id="228f0-186">c.</span></span> <span data-ttu-id="228f0-187">Kopiera hello **AssertionConsumerService** värde, klistra in det här värdet i hello **Reply URL** TextBox-kontroll i **TOPdesk - URL: er och den offentliga domänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="228f0-187">Copy hello **AssertionConsumerService** value, paste this value in hello **Reply URL** textbox in **TOPdesk - Public Domain and URLs** section.</span></span>      
   
12. <span data-ttu-id="228f0-188">toocreate en certifikatfil utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="228f0-188">toocreate a certificate file, perform hello following steps:</span></span>
    
    <span data-ttu-id="228f0-189">![Certifikatet](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "certifikat")</span><span class="sxs-lookup"><span data-stu-id="228f0-189">![Certificate](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "Certificate")</span></span>
    
    <span data-ttu-id="228f0-190">a.</span><span class="sxs-lookup"><span data-stu-id="228f0-190">a.</span></span> <span data-ttu-id="228f0-191">Öppna hello ned metadatafil från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="228f0-191">Open hello downloaded metadata file from Azure portal.</span></span>
    
    <span data-ttu-id="228f0-192">b.</span><span class="sxs-lookup"><span data-stu-id="228f0-192">b.</span></span> <span data-ttu-id="228f0-193">Expandera hello **RoleDescriptor** nod som har en **xsi: type** av **aggregeringsdesignprocessen: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="228f0-193">Expand hello **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    
    <span data-ttu-id="228f0-194">c.</span><span class="sxs-lookup"><span data-stu-id="228f0-194">c.</span></span> <span data-ttu-id="228f0-195">Kopiera hello värdet för hello **X509Certificate** nod.</span><span class="sxs-lookup"><span data-stu-id="228f0-195">Copy hello value of hello **X509Certificate** node.</span></span>
    
    <span data-ttu-id="228f0-196">d.</span><span class="sxs-lookup"><span data-stu-id="228f0-196">d.</span></span> <span data-ttu-id="228f0-197">Spara hello kopieras **X509Certificate** värdet lokalt på din dator i en fil.</span><span class="sxs-lookup"><span data-stu-id="228f0-197">Save hello copied **X509Certificate** value locally on your computer in a file.</span></span>

13. <span data-ttu-id="228f0-198">I hello **offentliga** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="228f0-198">In hello **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="228f0-199">![SAML-inloggningen](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-inloggning")</span><span class="sxs-lookup"><span data-stu-id="228f0-199">![SAML Login](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML Login")</span></span>

14. <span data-ttu-id="228f0-200">På hello **SAML configuration assistenten** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="228f0-200">On hello **SAML configuration assistant** dialog page, perform hello following steps:</span></span>
    
    <span data-ttu-id="228f0-201">![SAML Configuration assistenten](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "assistenten för SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="228f0-201">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="228f0-202">a.</span><span class="sxs-lookup"><span data-stu-id="228f0-202">a.</span></span> <span data-ttu-id="228f0-203">tooupload hämtade metadata filen från Azure-portalen under **Federationsmetadata**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="228f0-203">tooupload your downloaded metadata file from Azure portal, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="228f0-204">b.</span><span class="sxs-lookup"><span data-stu-id="228f0-204">b.</span></span> <span data-ttu-id="228f0-205">ditt certifikat filen under tooupload **certifikat (RSA)**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="228f0-205">tooupload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="228f0-206">c.</span><span class="sxs-lookup"><span data-stu-id="228f0-206">c.</span></span> <span data-ttu-id="228f0-207">tooupload hello logotypfilen som du har fått från supportteamet för hello TOPdesk under **logotypen ikonen**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="228f0-207">tooupload hello logo file you got from hello TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="228f0-208">d.</span><span class="sxs-lookup"><span data-stu-id="228f0-208">d.</span></span> <span data-ttu-id="228f0-209">I hello **användarnamn** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="228f0-209">In hello **User name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="228f0-210">e.</span><span class="sxs-lookup"><span data-stu-id="228f0-210">e.</span></span> <span data-ttu-id="228f0-211">I hello **visningsnamn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="228f0-211">In hello **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="228f0-212">f.</span><span class="sxs-lookup"><span data-stu-id="228f0-212">f.</span></span> <span data-ttu-id="228f0-213">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="228f0-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="228f0-214">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="228f0-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="228f0-215">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="228f0-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="228f0-216">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="228f0-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="228f0-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="228f0-217">Create an Azure AD test user</span></span>

<span data-ttu-id="228f0-218">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="228f0-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="228f0-220">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="228f0-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="228f0-221">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="228f0-221">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="228f0-223">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="228f0-223">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="228f0-225">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="228f0-225">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="228f0-227">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="228f0-227">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    <span data-ttu-id="228f0-229">a.</span><span class="sxs-lookup"><span data-stu-id="228f0-229">a.</span></span> <span data-ttu-id="228f0-230">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="228f0-230">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="228f0-231">b.</span><span class="sxs-lookup"><span data-stu-id="228f0-231">b.</span></span> <span data-ttu-id="228f0-232">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="228f0-232">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="228f0-233">c.</span><span class="sxs-lookup"><span data-stu-id="228f0-233">c.</span></span> <span data-ttu-id="228f0-234">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="228f0-234">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="228f0-235">d.</span><span class="sxs-lookup"><span data-stu-id="228f0-235">d.</span></span> <span data-ttu-id="228f0-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="228f0-236">Click **Create**.</span></span>
 
### <a name="create-a-topdesk---public-test-user"></a><span data-ttu-id="228f0-237">Skapa en TOPdesk - offentliga testanvändare</span><span class="sxs-lookup"><span data-stu-id="228f0-237">Create a TOPdesk - Public test user</span></span>

<span data-ttu-id="228f0-238">I ordning tooenable Azure AD-användare toolog i TOPdesk - offentliga de måste etableras i TOPdesk - allmänheten.</span><span class="sxs-lookup"><span data-stu-id="228f0-238">In order tooenable Azure AD users toolog into TOPdesk - Public, they must be provisioned into TOPdesk - Public.</span></span>  
<span data-ttu-id="228f0-239">Hello gäller TOPdesk - offentliga etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="228f0-239">In hello case of TOPdesk - Public, provisioning is a manual task.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="228f0-240">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="228f0-240">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="228f0-241">Logga in tooyour **TOPdesk - offentliga** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="228f0-241">Sign on tooyour **TOPdesk - Public** company site as administrator.</span></span>

2. <span data-ttu-id="228f0-242">Hello-menyn överst hello **TOPdesk \> ny \> stödfiler \> Person**.</span><span class="sxs-lookup"><span data-stu-id="228f0-242">In hello menu on hello top, click **TOPdesk \> New \> Support Files \> Person**.</span></span>
   
    <span data-ttu-id="228f0-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span><span class="sxs-lookup"><span data-stu-id="228f0-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span></span>

3. <span data-ttu-id="228f0-244">Utför följande hello på hello ny Person dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="228f0-244">On hello New Person dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="228f0-245">![Ny Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "ny Person")</span><span class="sxs-lookup"><span data-stu-id="228f0-245">![New Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "New Person")</span></span>
   
    <span data-ttu-id="228f0-246">a.</span><span class="sxs-lookup"><span data-stu-id="228f0-246">a.</span></span> <span data-ttu-id="228f0-247">Klicka på hello på fliken Allmänt.</span><span class="sxs-lookup"><span data-stu-id="228f0-247">Click hello General tab.</span></span>

    <span data-ttu-id="228f0-248">b.</span><span class="sxs-lookup"><span data-stu-id="228f0-248">b.</span></span> <span data-ttu-id="228f0-249">I hello **efternamn** textruta Ange efternamn hello användaren som Simon</span><span class="sxs-lookup"><span data-stu-id="228f0-249">In hello **Surname** textbox, type Surname of hello user like Simon</span></span>
 
    <span data-ttu-id="228f0-250">c.</span><span class="sxs-lookup"><span data-stu-id="228f0-250">c.</span></span> <span data-ttu-id="228f0-251">Välj en **plats** för hello-kontot.</span><span class="sxs-lookup"><span data-stu-id="228f0-251">Select a **Site** for hello account.</span></span>
 
    <span data-ttu-id="228f0-252">d.</span><span class="sxs-lookup"><span data-stu-id="228f0-252">d.</span></span> <span data-ttu-id="228f0-253">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="228f0-253">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="228f0-254">Du kan använda andra TOPdesk - offentliga användare skapa verktyg eller API: er som tillhandahålls av TOPdesk - offentliga tooprovision Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="228f0-254">You can use any other TOPdesk - Public user account creation tools or APIs provided by TOPdesk - Public tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="228f0-255">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="228f0-255">Assign hello Azure AD test user</span></span>

<span data-ttu-id="228f0-256">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTOPdesk - offentliga.</span><span class="sxs-lookup"><span data-stu-id="228f0-256">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTOPdesk - Public.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="228f0-258">**tooassign Britta Simon tooTOPdesk - offentliga utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="228f0-258">**tooassign Britta Simon tooTOPdesk - Public, perform hello following steps:**</span></span>

1. <span data-ttu-id="228f0-259">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="228f0-259">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="228f0-261">Välj i listan med program hello **TOPdesk - offentliga**.</span><span class="sxs-lookup"><span data-stu-id="228f0-261">In hello applications list, select **TOPdesk - Public**.</span></span>

    ![Hej TOPdesk - offentliga länken i listan med program hello](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. <span data-ttu-id="228f0-263">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="228f0-263">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="228f0-265">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="228f0-265">Click **Add** button.</span></span> <span data-ttu-id="228f0-266">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="228f0-266">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="228f0-268">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="228f0-268">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="228f0-269">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="228f0-269">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="228f0-270">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="228f0-270">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="228f0-271">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="228f0-271">Test single sign-on</span></span>

<span data-ttu-id="228f0-272">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="228f0-272">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="228f0-273">När du klickar på hello TOPdesk - ett offentligt panelen i hello åtkomstpanelen får du automatiskt inloggade tooyour TOPdesk - offentliga program.</span><span class="sxs-lookup"><span data-stu-id="228f0-273">When you click hello TOPdesk - Public tile in hello Access Panel, you should get automatically signed-on tooyour TOPdesk - Public application.</span></span>
<span data-ttu-id="228f0-274">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="228f0-274">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="228f0-275">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="228f0-275">Additional resources</span></span>

* [<span data-ttu-id="228f0-276">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="228f0-276">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="228f0-277">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="228f0-277">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

