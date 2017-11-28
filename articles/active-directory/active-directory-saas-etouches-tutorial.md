---
title: "Självstudier: Azure Active Directory-integrering med etouches | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och etouches."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 5f3ff7550e660b0fc52612140ca55061504b5edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a><span data-ttu-id="c9318-103">Självstudier: Azure Active Directory-integrering med etouches</span><span class="sxs-lookup"><span data-stu-id="c9318-103">Tutorial: Azure Active Directory integration with etouches</span></span>

<span data-ttu-id="c9318-104">I kursen får du lära dig hur toointegrate etouches med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c9318-104">In this tutorial, you learn how toointegrate etouches with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c9318-105">Integrera etouches med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c9318-105">Integrating etouches with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c9318-106">Du kan styra i Azure AD som har åtkomst till tooetouches</span><span class="sxs-lookup"><span data-stu-id="c9318-106">You can control in Azure AD who has access tooetouches</span></span>
- <span data-ttu-id="c9318-107">Du kan aktivera din användare tooautomatically get inloggade tooetouches (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c9318-107">You can enable your users tooautomatically get signed-on tooetouches (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c9318-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c9318-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c9318-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c9318-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9318-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c9318-110">Prerequisites</span></span>

<span data-ttu-id="c9318-111">tooconfigure Azure AD-integrering med etouches, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c9318-111">tooconfigure Azure AD integration with etouches, you need hello following items:</span></span>

- <span data-ttu-id="c9318-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c9318-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c9318-113">En etouches enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c9318-113">An etouches single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c9318-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c9318-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c9318-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c9318-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c9318-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c9318-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c9318-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c9318-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c9318-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c9318-118">Scenario description</span></span>
<span data-ttu-id="c9318-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c9318-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c9318-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c9318-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c9318-121">Att lägga till etouches från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c9318-121">Adding etouches from hello gallery</span></span>
2. <span data-ttu-id="c9318-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9318-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-etouches-from-hello-gallery"></a><span data-ttu-id="c9318-123">Att lägga till etouches från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c9318-123">Adding etouches from hello gallery</span></span>
<span data-ttu-id="c9318-124">tooconfigure hello integrering av etouches i Azure AD, behöver du tooadd etouches hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c9318-124">tooconfigure hello integration of etouches into Azure AD, you need tooadd etouches from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c9318-125">**tooadd etouches från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c9318-125">**tooadd etouches from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9318-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c9318-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="c9318-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c9318-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c9318-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c9318-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="c9318-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9318-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="c9318-133">Skriv i sökrutan hello **etouches**väljer **etouches** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c9318-133">In hello search box, type **etouches**, select **etouches** from result panel then click **Add** button tooadd hello application.</span></span>

    ![etouches i hello resultatlistan](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c9318-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9318-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="c9318-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med etouches baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c9318-136">In this section, you configure and test Azure AD single sign-on with etouches based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c9318-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i etouches är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9318-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in etouches is tooa user in Azure AD.</span></span> <span data-ttu-id="c9318-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i etouches toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c9318-138">In other words, a link relationship between an Azure AD user and hello related user in etouches needs toobe established.</span></span>

<span data-ttu-id="c9318-139">I etouches, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c9318-139">In etouches, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c9318-140">tooconfigure och testa Azure AD enkel inloggning med etouches, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c9318-140">tooconfigure and test Azure AD single sign-on with etouches, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c9318-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c9318-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c9318-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9318-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c9318-143">**[Skapa en testanvändare etouches](#create-an-etouches-test-user)**  -toohave en motsvarighet för Britta Simon i etouches som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c9318-143">**[Create an etouches test user](#create-an-etouches-test-user)** - toohave a counterpart of Britta Simon in etouches that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c9318-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9318-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c9318-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c9318-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c9318-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9318-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c9318-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt etouches program.</span><span class="sxs-lookup"><span data-stu-id="c9318-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your etouches application.</span></span>

<span data-ttu-id="c9318-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med etouches:**</span><span class="sxs-lookup"><span data-stu-id="c9318-148">**tooconfigure Azure AD single sign-on with etouches, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9318-149">I hello Azure-portalen på hello **etouches** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c9318-149">In hello Azure portal, on hello **etouches** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c9318-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9318-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_samlbase.png)

3. <span data-ttu-id="c9318-153">På hello **etouches domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9318-153">On hello **etouches Domain and URLs** section, perform hello following steps:</span></span>

    ![etouches domän och URL: er med enkel inloggning information](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_url.png)

    <span data-ttu-id="c9318-155">a.</span><span class="sxs-lookup"><span data-stu-id="c9318-155">a.</span></span> <span data-ttu-id="c9318-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span><span class="sxs-lookup"><span data-stu-id="c9318-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`</span></span>

    <span data-ttu-id="c9318-157">b.</span><span class="sxs-lookup"><span data-stu-id="c9318-157">b.</span></span> <span data-ttu-id="c9318-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.eiseverywhere.com/<instance name>`</span><span class="sxs-lookup"><span data-stu-id="c9318-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://www.eiseverywhere.com/<instance name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c9318-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c9318-159">These values are not real.</span></span> <span data-ttu-id="c9318-160">Du uppdaterar hello värdet med hello faktiska inloggning URL och identifierare som beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="c9318-160">You update hello value with hello actual Sign on URL and Identifier, which is explained later in hello tutorial.</span></span>
    > 

4. <span data-ttu-id="c9318-161">etouches program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="c9318-161">etouches application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="c9318-162">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="c9318-162">Configure hello following claims for this application.</span></span> <span data-ttu-id="c9318-163">Du kan hantera hello värden för attributen från hello **användarattribut** av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="c9318-163">You can manage hello values of these attributes from hello **User Attribute** of hello application.</span></span> <span data-ttu-id="c9318-164">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="c9318-164">hello following screenshot shows an example for this.</span></span> 

    ![Användarattribut](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_attribute.png) 

5. <span data-ttu-id="c9318-166">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9318-166">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="c9318-167">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="c9318-167">Attribute Name</span></span> | <span data-ttu-id="c9318-168">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="c9318-168">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="c9318-169">E-post</span><span class="sxs-lookup"><span data-stu-id="c9318-169">Email</span></span> | <span data-ttu-id="c9318-170">User.Mail</span><span class="sxs-lookup"><span data-stu-id="c9318-170">user.mail</span></span> |    
    
    <span data-ttu-id="c9318-171">a.</span><span class="sxs-lookup"><span data-stu-id="c9318-171">a.</span></span> <span data-ttu-id="c9318-172">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9318-172">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Lägg till attribut](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_04.png)

    ![Attributet dialogrutan Lägg till](./media/active-directory-saas-etouches-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="c9318-175">b.</span><span class="sxs-lookup"><span data-stu-id="c9318-175">b.</span></span> <span data-ttu-id="c9318-176">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c9318-176">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="c9318-177">c.</span><span class="sxs-lookup"><span data-stu-id="c9318-177">c.</span></span> <span data-ttu-id="c9318-178">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c9318-178">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="c9318-179">d.</span><span class="sxs-lookup"><span data-stu-id="c9318-179">d.</span></span> <span data-ttu-id="c9318-180">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9318-180">Click **Ok**.</span></span> 

6. <span data-ttu-id="c9318-181">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="c9318-181">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_certificate.png) 

7. <span data-ttu-id="c9318-183">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c9318-183">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-etouches-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="c9318-185">tooget SSO konfigurerats för ditt program, utför följande steg i hello etouches programmet hello:</span><span class="sxs-lookup"><span data-stu-id="c9318-185">tooget SSO configured for your application, perform hello following steps in hello etouches application:</span></span> 

    ![etouches konfiguration](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png) 

    <span data-ttu-id="c9318-187">a.</span><span class="sxs-lookup"><span data-stu-id="c9318-187">a.</span></span> <span data-ttu-id="c9318-188">Inloggning för**etouches** program med hjälp av hello administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="c9318-188">Login too**etouches** application using hello Admin rights.</span></span>
   
    <span data-ttu-id="c9318-189">b.</span><span class="sxs-lookup"><span data-stu-id="c9318-189">b.</span></span> <span data-ttu-id="c9318-190">Gå toohello **SAML** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c9318-190">Go toohello **SAML** Configuration.</span></span>

    <span data-ttu-id="c9318-191">c.</span><span class="sxs-lookup"><span data-stu-id="c9318-191">c.</span></span> <span data-ttu-id="c9318-192">I hello **allmänna inställningar** avsnittet, öppna hämtade certifikatet från Azure-portalen i anteckningar, kopiera hello innehåll, och klistra in den i textrutan för hello IDP-metadata.</span><span class="sxs-lookup"><span data-stu-id="c9318-192">In hello **General Settings** section, open your downloaded certificate from Azure portal in notepad, copy hello content, and then paste it into hello IDP metadata textbox.</span></span> 

    <span data-ttu-id="c9318-193">d.</span><span class="sxs-lookup"><span data-stu-id="c9318-193">d.</span></span> <span data-ttu-id="c9318-194">Klicka på hello **Spara & förblir** knappen.</span><span class="sxs-lookup"><span data-stu-id="c9318-194">Click on hello **Save & Stay** button.</span></span>
  
    <span data-ttu-id="c9318-195">e.</span><span class="sxs-lookup"><span data-stu-id="c9318-195">e.</span></span> <span data-ttu-id="c9318-196">Klicka på hello **uppdateringsmetadata** knapp i hello SAML Metadata-avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c9318-196">Click on hello **Update Metadata** button in hello SAML Metadata section.</span></span> 

    <span data-ttu-id="c9318-197">f.</span><span class="sxs-lookup"><span data-stu-id="c9318-197">f.</span></span> <span data-ttu-id="c9318-198">Detta öppnar hello sidan och utföra en enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c9318-198">This opens hello page and perform SSO.</span></span> <span data-ttu-id="c9318-199">Hello SSO fungerar en gång och sedan kan du ställa in hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="c9318-199">Once hello SSO is working then you can set up hello username.</span></span>

    <span data-ttu-id="c9318-200">g.</span><span class="sxs-lookup"><span data-stu-id="c9318-200">g.</span></span> <span data-ttu-id="c9318-201">Välj hello hello användarnamnfältet **e-postadress** enligt hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="c9318-201">In hello Username field, select hello **emailaddress** as shown in hello image below.</span></span> 

    <span data-ttu-id="c9318-202">h.</span><span class="sxs-lookup"><span data-stu-id="c9318-202">h.</span></span> <span data-ttu-id="c9318-203">Kopiera hello **SP enhets-ID** värdet och klistrar in den i hello **identifierare** textruta som finns i **etouches domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c9318-203">Copy hello **SP entity ID** value and paste it into hello **Identifier**  textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="c9318-204">Jag.</span><span class="sxs-lookup"><span data-stu-id="c9318-204">i.</span></span> <span data-ttu-id="c9318-205">Kopiera hello **SSO URL / ACS** värdet och klistrar in den i hello **inloggning URL** textruta som finns i **etouches domän och URL: er** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c9318-205">Copy hello **SSO URL / ACS** value and paste it into hello **Sign on URL** textbox, which is in **etouches Domain and URLs** section on Azure portal.</span></span>
   
> [!TIP]
> <span data-ttu-id="c9318-206">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c9318-206">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c9318-207">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c9318-207">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c9318-208">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c9318-208">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c9318-209">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9318-209">Create an Azure AD test user</span></span>
<span data-ttu-id="c9318-210">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c9318-210">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="c9318-212">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c9318-212">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9318-213">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c9318-213">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-etouches-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c9318-215">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c9318-215">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-etouches-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c9318-217">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9318-217">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c9318-219">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c9318-219">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c9318-221">a.</span><span class="sxs-lookup"><span data-stu-id="c9318-221">a.</span></span> <span data-ttu-id="c9318-222">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c9318-222">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c9318-223">b.</span><span class="sxs-lookup"><span data-stu-id="c9318-223">b.</span></span> <span data-ttu-id="c9318-224">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c9318-224">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c9318-225">c.</span><span class="sxs-lookup"><span data-stu-id="c9318-225">c.</span></span> <span data-ttu-id="c9318-226">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c9318-226">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c9318-227">d.</span><span class="sxs-lookup"><span data-stu-id="c9318-227">d.</span></span> <span data-ttu-id="c9318-228">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c9318-228">Click **Create**.</span></span>
 
### <a name="create-an-etouches-test-user"></a><span data-ttu-id="c9318-229">Skapa en testanvändare etouches</span><span class="sxs-lookup"><span data-stu-id="c9318-229">Create an etouches test user</span></span>

<span data-ttu-id="c9318-230">I det här avsnittet skapar du en användare som kallas Britta Simon i etouches.</span><span class="sxs-lookup"><span data-stu-id="c9318-230">In this section, you create a user called Britta Simon in etouches.</span></span> <span data-ttu-id="c9318-231">Arbeta med [etouches klienten supportteam](https://www.etouches.com/event-software/support/customer-support/) tooadd hello användare i hello etouches plattform.</span><span class="sxs-lookup"><span data-stu-id="c9318-231">Work with [etouches Client support team](https://www.etouches.com/event-software/support/customer-support/) tooadd hello users in hello etouches platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c9318-232">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c9318-232">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c9318-233">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooetouches.</span><span class="sxs-lookup"><span data-stu-id="c9318-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooetouches.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="c9318-235">**tooassign Britta Simon tooetouches utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c9318-235">**tooassign Britta Simon tooetouches, perform hello following steps:**</span></span>

1. <span data-ttu-id="c9318-236">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c9318-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c9318-238">Välj i listan med program hello **etouches**.</span><span class="sxs-lookup"><span data-stu-id="c9318-238">In hello applications list, select **etouches**.</span></span>

    ![Hej etouches länken i listan med program hello](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_app.png) 

3. <span data-ttu-id="c9318-240">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c9318-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="c9318-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c9318-242">Click **Add** button.</span></span> <span data-ttu-id="c9318-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9318-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="c9318-245">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c9318-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c9318-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9318-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c9318-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c9318-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c9318-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c9318-248">Test single sign-on</span></span>


<span data-ttu-id="c9318-249">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c9318-249">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c9318-250">Du bör få automatiskt inloggade tooyour etouches programmet när du klickar på hello etouches panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c9318-250">When you click hello etouches tile in hello Access Panel, you should get automatically signed-on tooyour etouches application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9318-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c9318-251">Additional resources</span></span>

* [<span data-ttu-id="c9318-252">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c9318-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c9318-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c9318-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png

