---
title: "Självstudier: Azure Active Directory-integrering med Ceridian Dayforce HCM | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Ceridian Dayforce HCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4d72f29b4e5e30ef8881806d789f6676fc541e2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a><span data-ttu-id="4e7c3-103">Självstudier: Azure Active Directory-integrering med Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="4e7c3-103">Tutorial: Azure Active Directory integration with Ceridian Dayforce HCM</span></span>

<span data-ttu-id="4e7c3-104">I kursen får du lära dig hur toointegrate Ceridian Dayforce HCM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4e7c3-104">In this tutorial, you learn how toointegrate Ceridian Dayforce HCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4e7c3-105">Integrera Ceridian Dayforce HCM med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-105">Integrating Ceridian Dayforce HCM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4e7c3-106">Du kan styra i Azure AD som har åtkomst tooCeridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-106">You can control in Azure AD who has access tooCeridian Dayforce HCM.</span></span>
- <span data-ttu-id="4e7c3-107">Du kan låta dina användare tooautomatically get inloggade tooCeridian Dayforce HCM (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-107">You can enable your users tooautomatically get signed-on tooCeridian Dayforce HCM (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="4e7c3-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="4e7c3-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4e7c3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e7c3-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4e7c3-110">Prerequisites</span></span>

<span data-ttu-id="4e7c3-111">tooconfigure Azure AD-integrering med Ceridian Dayforce HCM måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-111">tooconfigure Azure AD integration with Ceridian Dayforce HCM, you need hello following items:</span></span>

- <span data-ttu-id="4e7c3-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4e7c3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4e7c3-113">En Ceridian Dayforce HCM enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="4e7c3-113">A Ceridian Dayforce HCM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4e7c3-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4e7c3-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4e7c3-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4e7c3-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4e7c3-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4e7c3-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4e7c3-118">Scenario description</span></span>
<span data-ttu-id="4e7c3-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4e7c3-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4e7c3-121">Att lägga till Ceridian Dayforce HCM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4e7c3-121">Adding Ceridian Dayforce HCM from hello gallery</span></span>
2. <span data-ttu-id="4e7c3-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e7c3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ceridian-dayforce-hcm-from-hello-gallery"></a><span data-ttu-id="4e7c3-123">Att lägga till Ceridian Dayforce HCM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4e7c3-123">Adding Ceridian Dayforce HCM from hello gallery</span></span>
<span data-ttu-id="4e7c3-124">tooconfigure hello integrering av Ceridian Dayforce HCM i Azure AD, behöver du tooadd Ceridian Dayforce HCM hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-124">tooconfigure hello integration of Ceridian Dayforce HCM into Azure AD, you need tooadd Ceridian Dayforce HCM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4e7c3-125">**tooadd Ceridian Dayforce HCM från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e7c3-125">**tooadd Ceridian Dayforce HCM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e7c3-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="4e7c3-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4e7c3-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="4e7c3-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="4e7c3-133">Skriv i sökrutan hello **Ceridian Dayforce HCM**väljer **Ceridian Dayforce HCM** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-133">In hello search box, type **Ceridian Dayforce HCM**, select **Ceridian Dayforce HCM** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Ceridian Dayforce HCM i hello resultatlistan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4e7c3-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e7c3-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="4e7c3-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Ceridian Dayforce HCM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-136">In this section, you configure and test Azure AD single sign-on with Ceridian Dayforce HCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4e7c3-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Ceridian Dayforce HCM är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Ceridian Dayforce HCM is tooa user in Azure AD.</span></span> <span data-ttu-id="4e7c3-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Ceridian Dayforce HCM toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-138">In other words, a link relationship between an Azure AD user and hello related user in Ceridian Dayforce HCM needs toobe established.</span></span>

<span data-ttu-id="4e7c3-139">Tilldela hello värdet för hello i Ceridian Dayforce HCM **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-139">In Ceridian Dayforce HCM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4e7c3-140">tooconfigure och testa Azure AD enkel inloggning med Ceridian Dayforce HCM, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-140">tooconfigure and test Azure AD single sign-on with Ceridian Dayforce HCM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4e7c3-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4e7c3-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4e7c3-143">**[Skapa en testanvändare Ceridian Dayforce HCM](#create-a-ceridian-dayforce-hcm-test-user)**  -toohave en motsvarighet för Britta Simon i Ceridian Dayforce HCM som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-143">**[Create a Ceridian Dayforce HCM test user](#create-a-ceridian-dayforce-hcm-test-user)** - toohave a counterpart of Britta Simon in Ceridian Dayforce HCM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4e7c3-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4e7c3-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="4e7c3-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e7c3-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="4e7c3-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Ceridian Dayforce HCM-program.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Ceridian Dayforce HCM application.</span></span>

<span data-ttu-id="4e7c3-148">**tooconfigure Azure AD enkel inloggning med Ceridian Dayforce HCM utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e7c3-148">**tooconfigure Azure AD single sign-on with Ceridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e7c3-149">I hello Azure-portalen på hello **Ceridian Dayforce HCM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-149">In hello Azure portal, on hello **Ceridian Dayforce HCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="4e7c3-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. <span data-ttu-id="4e7c3-153">På hello **Ceridian Dayforce HCM domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-153">On hello **Ceridian Dayforce HCM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    <span data-ttu-id="4e7c3-155">a.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-155">a.</span></span> <span data-ttu-id="4e7c3-156">I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour Ceridian Dayforce HCM-programmet.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-156">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Ceridian Dayforce HCM application.</span></span>
    
    | <span data-ttu-id="4e7c3-157">Miljö</span><span class="sxs-lookup"><span data-stu-id="4e7c3-157">Environment</span></span> | <span data-ttu-id="4e7c3-158">URL: EN</span><span class="sxs-lookup"><span data-stu-id="4e7c3-158">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="4e7c3-159">För produktion</span><span class="sxs-lookup"><span data-stu-id="4e7c3-159">For production</span></span> | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | <span data-ttu-id="4e7c3-160">För testet</span><span class="sxs-lookup"><span data-stu-id="4e7c3-160">For test</span></span> | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    <span data-ttu-id="4e7c3-161">b.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-161">b.</span></span> <span data-ttu-id="4e7c3-162">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-162">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    
    | <span data-ttu-id="4e7c3-163">Miljö</span><span class="sxs-lookup"><span data-stu-id="4e7c3-163">Environment</span></span> | <span data-ttu-id="4e7c3-164">URL: EN</span><span class="sxs-lookup"><span data-stu-id="4e7c3-164">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="4e7c3-165">För produktion</span><span class="sxs-lookup"><span data-stu-id="4e7c3-165">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp` |
    | <span data-ttu-id="4e7c3-166">För testet</span><span class="sxs-lookup"><span data-stu-id="4e7c3-166">For test</span></span> | `https://fs-test.dayforcehcm.com/sp` |
    
    <span data-ttu-id="4e7c3-167">c.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-167">c.</span></span> <span data-ttu-id="4e7c3-168">I hello **Reply URL** textruta typen hello URL som används av Azure AD toopost hello svar.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-168">In hello **Reply URL** textbox, type hello URL used by Azure AD toopost hello response.</span></span>
    
    | <span data-ttu-id="4e7c3-169">Miljö</span><span class="sxs-lookup"><span data-stu-id="4e7c3-169">Environment</span></span> | <span data-ttu-id="4e7c3-170">URL: EN</span><span class="sxs-lookup"><span data-stu-id="4e7c3-170">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="4e7c3-171">För produktion</span><span class="sxs-lookup"><span data-stu-id="4e7c3-171">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | <span data-ttu-id="4e7c3-172">För testet</span><span class="sxs-lookup"><span data-stu-id="4e7c3-172">For test</span></span> | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > <span data-ttu-id="4e7c3-173">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-173">These values are not real.</span></span> <span data-ttu-id="4e7c3-174">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-174">Update these values with hello actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="4e7c3-175">Kontakta [Ceridian Dayforce HCM klienten supportteamet](https://www.ceridian.com/contact-us/index.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-175">Contact [Ceridian Dayforce HCM Client support team](https://www.ceridian.com/contact-us/index.html) tooget these values.</span></span>

4. <span data-ttu-id="4e7c3-176">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-176">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. <span data-ttu-id="4e7c3-178">Tillämpningsprogrammet Ceridian Dayforce HCM förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-178">Your Ceridian Dayforce HCM application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="4e7c3-179">Arbeta med [Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html) första tooidentify hello rätt användar-ID.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-179">Work with [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) first tooidentify hello correct user identifier.</span></span> <span data-ttu-id="4e7c3-180">Microsoft rekommenderar att du använder hello **”name”** attribut som användaridentifierare.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-180">Microsoft recommends using hello **"name"** attribute as user identifier.</span></span> <span data-ttu-id="4e7c3-181">Du kan hantera hello värden för attributen från hello **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-181">You can manage hello values of these attributes from hello **User Attributes** section on application integration page.</span></span> <span data-ttu-id="4e7c3-182">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-182">hello following screenshot shows an example for this.</span></span>  

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. <span data-ttu-id="4e7c3-184">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-184">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="4e7c3-185">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="4e7c3-185">Attribute Name</span></span>  | <span data-ttu-id="4e7c3-186">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="4e7c3-186">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="4e7c3-187">namn</span><span class="sxs-lookup"><span data-stu-id="4e7c3-187">name</span></span>  | <span data-ttu-id="4e7c3-188">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="4e7c3-188">user.extensionattribute2</span></span> |    

    <span data-ttu-id="4e7c3-189">a.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-189">a.</span></span> <span data-ttu-id="4e7c3-190">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-190">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="4e7c3-193">b.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-193">b.</span></span> <span data-ttu-id="4e7c3-194">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-194">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="4e7c3-195">c.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-195">c.</span></span> <span data-ttu-id="4e7c3-196">I hello **värdet** listan, Välj hello användarattribut som du vill använda toouse för din implementering.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-196">In hello **Value** list, select hello user attribute you want toouse for your implementation.</span></span>
    <span data-ttu-id="4e7c3-197">Till exempel om du vill toouse hello EmployeeID som unikt identifierare och du har lagrat hello attributvärdet i hello ExtensionAttribute2, väljer du **user.extensionattribute2**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-197">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select **user.extensionattribute2**.</span></span>
    
    <span data-ttu-id="4e7c3-198">d.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-198">d.</span></span> <span data-ttu-id="4e7c3-199">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-199">Click **Ok**.</span></span>

7. <span data-ttu-id="4e7c3-200">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-200">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="4e7c3-202">På hello **Ceridian Dayforce HCM Configuration** klickar du på **konfigurera Ceridian Dayforce HCM** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-202">On hello **Ceridian Dayforce HCM Configuration** section, click **Configure Ceridian Dayforce HCM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4e7c3-203">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4e7c3-203">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Ceridian Dayforce HCM-konfiguration](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. <span data-ttu-id="4e7c3-205">tooconfigure enkel inloggning på **Ceridian Dayforce HCM** sida, behöver du toosend hello hämtas **XML-Metadata för** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html).</span><span class="sxs-lookup"><span data-stu-id="4e7c3-205">tooconfigure single sign-on on **Ceridian Dayforce HCM** side, you need toosend hello downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html).</span></span>

> [!TIP]
> <span data-ttu-id="4e7c3-206">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4e7c3-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4e7c3-207">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4e7c3-208">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4e7c3-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4e7c3-209">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e7c3-209">Create an Azure AD test user</span></span>

<span data-ttu-id="4e7c3-210">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="4e7c3-212">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e7c3-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e7c3-213">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-213">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="4e7c3-215">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-215">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="4e7c3-217">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-217">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="4e7c3-219">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e7c3-219">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    <span data-ttu-id="4e7c3-221">a.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-221">a.</span></span> <span data-ttu-id="4e7c3-222">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-222">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4e7c3-223">b.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-223">b.</span></span> <span data-ttu-id="4e7c3-224">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-224">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="4e7c3-225">c.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-225">c.</span></span> <span data-ttu-id="4e7c3-226">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-226">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="4e7c3-227">d.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-227">d.</span></span> <span data-ttu-id="4e7c3-228">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-228">Click **Create**.</span></span>
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a><span data-ttu-id="4e7c3-229">Skapa en Ceridian Dayforce HCM testanvändare</span><span class="sxs-lookup"><span data-stu-id="4e7c3-229">Create a Ceridian Dayforce HCM test user</span></span>

<span data-ttu-id="4e7c3-230">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-230">hello objective of this section is toocreate a user called Britta Simon in Ceridian Dayforce HCM.</span></span> <span data-ttu-id="4e7c3-231">Arbeta med hello [Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html) tooget användare som lagts till i hello Ceridian Dayforce HCM-programmet.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-231">Work with hello [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) tooget users added in hello Ceridian Dayforce HCM application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4e7c3-232">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e7c3-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4e7c3-233">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCeridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCeridian Dayforce HCM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4e7c3-235">**tooassign Britta Simon tooCeridian Dayforce HCM utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e7c3-235">**tooassign Britta Simon tooCeridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e7c3-236">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4e7c3-238">Välj i listan med program hello **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-238">In hello applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. <span data-ttu-id="4e7c3-240">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4e7c3-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-242">Click **Add** button.</span></span> <span data-ttu-id="4e7c3-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4e7c3-245">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4e7c3-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4e7c3-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-247">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="4e7c3-248">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e7c3-248">Assign hello Azure AD test user</span></span>

<span data-ttu-id="4e7c3-249">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCeridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-249">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCeridian Dayforce HCM.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="4e7c3-251">**tooassign Britta Simon tooCeridian Dayforce HCM utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e7c3-251">**tooassign Britta Simon tooCeridian Dayforce HCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e7c3-252">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-252">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4e7c3-254">Välj i listan med program hello **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-254">In hello applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Hej Ceridian Dayforce HCM länken i listan med program hello](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. <span data-ttu-id="4e7c3-256">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-256">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="4e7c3-258">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-258">Click **Add** button.</span></span> <span data-ttu-id="4e7c3-259">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="4e7c3-261">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-261">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4e7c3-262">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4e7c3-263">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4e7c3-264">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e7c3-264">Test single sign-on</span></span>

<span data-ttu-id="4e7c3-265">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-265">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="4e7c3-266">Du bör få automatiskt inloggade tooyour Ceridian Dayforce HCM programmet när du klickar på hello Ceridian Dayforce HCM-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4e7c3-266">When you click hello Ceridian Dayforce HCM tile in hello Access Panel, you should get automatically signed-on tooyour Ceridian Dayforce HCM application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4e7c3-267">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4e7c3-267">Additional resources</span></span>

* [<span data-ttu-id="4e7c3-268">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4e7c3-268">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4e7c3-269">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4e7c3-269">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

