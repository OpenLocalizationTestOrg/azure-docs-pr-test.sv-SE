---
title: "Självstudier: Azure Active Directory-integrering med CloudPassage | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och CloudPassage."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 32fb007b90f071626c9b40fb5afc341dd3c1ae99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a><span data-ttu-id="43d41-103">Självstudier: Azure Active Directory-integrering med CloudPassage</span><span class="sxs-lookup"><span data-stu-id="43d41-103">Tutorial: Azure Active Directory integration with CloudPassage</span></span>

<span data-ttu-id="43d41-104">I kursen får du lära dig hur toointegrate CloudPassage med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="43d41-104">In this tutorial, you learn how toointegrate CloudPassage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="43d41-105">Integrera CloudPassage med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="43d41-105">Integrating CloudPassage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="43d41-106">Du kan styra i Azure AD som har åtkomst till tooCloudPassage</span><span class="sxs-lookup"><span data-stu-id="43d41-106">You can control in Azure AD who has access tooCloudPassage</span></span>
- <span data-ttu-id="43d41-107">Du kan aktivera din användare tooautomatically get inloggade tooCloudPassage (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="43d41-107">You can enable your users tooautomatically get signed-on tooCloudPassage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="43d41-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="43d41-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="43d41-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43d41-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43d41-110">Krav</span><span class="sxs-lookup"><span data-stu-id="43d41-110">Prerequisites</span></span>

<span data-ttu-id="43d41-111">tooconfigure Azure AD-integrering med CloudPassage, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="43d41-111">tooconfigure Azure AD integration with CloudPassage, you need hello following items:</span></span>

- <span data-ttu-id="43d41-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="43d41-112">An Azure AD subscription</span></span>
- <span data-ttu-id="43d41-113">En CloudPassage enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="43d41-113">A CloudPassage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="43d41-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="43d41-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="43d41-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="43d41-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="43d41-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="43d41-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="43d41-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43d41-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="43d41-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="43d41-118">Scenario description</span></span>
<span data-ttu-id="43d41-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="43d41-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="43d41-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="43d41-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="43d41-121">Att lägga till CloudPassage från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="43d41-121">Adding CloudPassage from hello gallery</span></span>
2. <span data-ttu-id="43d41-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43d41-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cloudpassage-from-hello-gallery"></a><span data-ttu-id="43d41-123">Att lägga till CloudPassage från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="43d41-123">Adding CloudPassage from hello gallery</span></span>
<span data-ttu-id="43d41-124">tooconfigure hello integrering av CloudPassage i Azure AD, behöver du tooadd CloudPassage hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="43d41-124">tooconfigure hello integration of CloudPassage into Azure AD, you need tooadd CloudPassage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="43d41-125">**tooadd CloudPassage från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="43d41-125">**tooadd CloudPassage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="43d41-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43d41-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="43d41-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="43d41-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="43d41-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="43d41-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="43d41-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43d41-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="43d41-133">Skriv i sökrutan hello **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="43d41-133">In hello search box, type **CloudPassage**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_search.png)

5. <span data-ttu-id="43d41-135">Markera hello resultat på panelen **CloudPassage**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="43d41-135">In hello results panel, select **CloudPassage**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="43d41-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43d41-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="43d41-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CloudPassage baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="43d41-138">In this section, you configure and test Azure AD single sign-on with CloudPassage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="43d41-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i CloudPassage är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43d41-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in CloudPassage is tooa user in Azure AD.</span></span> <span data-ttu-id="43d41-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i CloudPassage toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="43d41-140">In other words, a link relationship between an Azure AD user and hello related user in CloudPassage needs toobe established.</span></span>

<span data-ttu-id="43d41-141">I CloudPassage, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="43d41-141">In CloudPassage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="43d41-142">tooconfigure och testa Azure AD enkel inloggning med CloudPassage, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="43d41-142">tooconfigure and test Azure AD single sign-on with CloudPassage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="43d41-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="43d41-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="43d41-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43d41-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="43d41-145">**[Skapa en testanvändare CloudPassage](#creating-a-cloudpassage-test-user)**  -toohave en motsvarighet för Britta Simon i CloudPassage som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="43d41-145">**[Creating a CloudPassage test user](#creating-a-cloudpassage-test-user)** - toohave a counterpart of Britta Simon in CloudPassage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="43d41-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43d41-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="43d41-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="43d41-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="43d41-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43d41-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="43d41-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt CloudPassage program.</span><span class="sxs-lookup"><span data-stu-id="43d41-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your CloudPassage application.</span></span>

<span data-ttu-id="43d41-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med CloudPassage:**</span><span class="sxs-lookup"><span data-stu-id="43d41-150">**tooconfigure Azure AD single sign-on with CloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="43d41-151">I hello Azure-portalen på hello **CloudPassage** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="43d41-151">In hello Azure portal, on hello **CloudPassage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="43d41-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43d41-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

3. <span data-ttu-id="43d41-155">På hello **CloudPassage domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="43d41-155">On hello **CloudPassage Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    <span data-ttu-id="43d41-157">a.</span><span class="sxs-lookup"><span data-stu-id="43d41-157">a.</span></span> <span data-ttu-id="43d41-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://portal.cloudpassage.com/saml/init/accountid`</span><span class="sxs-lookup"><span data-stu-id="43d41-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://portal.cloudpassage.com/saml/init/accountid`</span></span>

    <span data-ttu-id="43d41-159">b.</span><span class="sxs-lookup"><span data-stu-id="43d41-159">b.</span></span> <span data-ttu-id="43d41-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret: `https://portal.cloudpassage.com/saml/consume/accountid`.</span><span class="sxs-lookup"><span data-stu-id="43d41-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://portal.cloudpassage.com/saml/consume/accountid`.</span></span> <span data-ttu-id="43d41-161">Du kan hämta värdet för det här attributet genom att klicka på **SSO installationsprogrammet dokumentationen** i hello **inställningar för enkel inloggning** på CloudPassage-portal.</span><span class="sxs-lookup"><span data-stu-id="43d41-161">You can get your value for this attribute by clicking **SSO Setup documentation** in hello **Single Sign-on Settings** section of your CloudPassage portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > <span data-ttu-id="43d41-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="43d41-163">These values are not real.</span></span> <span data-ttu-id="43d41-164">Uppdatera dessa värden med hello faktiska Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="43d41-164">Update these values with hello actual Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="43d41-165">Kontakta [CloudPassage klienten supportteamet](https://www.cloudpassage.com/company/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="43d41-165">Contact [CloudPassage Client support team](https://www.cloudpassage.com/company/contact/) tooget these values.</span></span> 

4. <span data-ttu-id="43d41-166">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="43d41-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

5. <span data-ttu-id="43d41-168">Tillämpningsprogrammet CloudPassage förväntar hello SAML intyg i ett specifikt format, vilket kräver att du tooadd attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="43d41-168">Your CloudPassage application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span>   
<span data-ttu-id="43d41-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="43d41-169">hello following screenshot shows an example for this.</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

6. <span data-ttu-id="43d41-171">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="43d41-171">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>

    | <span data-ttu-id="43d41-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="43d41-172">Attribute Name</span></span> | <span data-ttu-id="43d41-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="43d41-173">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="43d41-174">Förnamn</span><span class="sxs-lookup"><span data-stu-id="43d41-174">firstname</span></span> |<span data-ttu-id="43d41-175">User.givenName</span><span class="sxs-lookup"><span data-stu-id="43d41-175">user.givenname</span></span> |
    | <span data-ttu-id="43d41-176">Efternamn</span><span class="sxs-lookup"><span data-stu-id="43d41-176">lastname</span></span> |<span data-ttu-id="43d41-177">User.surname</span><span class="sxs-lookup"><span data-stu-id="43d41-177">user.surname</span></span> |
    | <span data-ttu-id="43d41-178">E-post</span><span class="sxs-lookup"><span data-stu-id="43d41-178">email</span></span> |<span data-ttu-id="43d41-179">User.Mail</span><span class="sxs-lookup"><span data-stu-id="43d41-179">user.mail</span></span> |
    
    <span data-ttu-id="43d41-180">a.</span><span class="sxs-lookup"><span data-stu-id="43d41-180">a.</span></span> <span data-ttu-id="43d41-181">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43d41-181">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="43d41-184">b.</span><span class="sxs-lookup"><span data-stu-id="43d41-184">b.</span></span> <span data-ttu-id="43d41-185">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="43d41-185">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="43d41-186">c.</span><span class="sxs-lookup"><span data-stu-id="43d41-186">c.</span></span> <span data-ttu-id="43d41-187">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="43d41-187">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="43d41-188">d.</span><span class="sxs-lookup"><span data-stu-id="43d41-188">d.</span></span> <span data-ttu-id="43d41-189">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="43d41-189">Click **Ok**.</span></span>

7. <span data-ttu-id="43d41-190">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="43d41-190">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="43d41-192">På hello **CloudPassage Configuration** klickar du på **konfigurera CloudPassage** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="43d41-192">On hello **CloudPassage Configuration** section, click **Configure CloudPassage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="43d41-193">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="43d41-193">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

9. <span data-ttu-id="43d41-195">I ett annat fönster i webbläsaren, inloggning tooyour CloudPassage företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="43d41-195">In a different browser window, sign-on tooyour CloudPassage company site as administrator.</span></span>

10. <span data-ttu-id="43d41-196">Hello-menyn överst hello **inställningar**, och klicka sedan på **Platsadministration**.</span><span class="sxs-lookup"><span data-stu-id="43d41-196">In hello menu on hello top, click **Settings**, and then click **Site Administration**.</span></span> 
   
    ![Konfigurera enkel inloggning][12]

11. <span data-ttu-id="43d41-198">Klicka på hello **autentiseringsinställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="43d41-198">Click hello **Authentication Settings** tab.</span></span> 
   
    ![Konfigurera enkel inloggning][13]

12. <span data-ttu-id="43d41-200">I hello **inställningar för enkel inloggning** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="43d41-200">In hello **Single Sign-on Settings** section, perform hello following steps:</span></span> 
   
    ![Konfigurera enkel inloggning][14]

    <span data-ttu-id="43d41-202">a.</span><span class="sxs-lookup"><span data-stu-id="43d41-202">a.</span></span> <span data-ttu-id="43d41-203">Välj **aktivera enkel sign-on(SSO) (SSO installationsprogrammet dokumentation)** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="43d41-203">Select **Enable Single sign-on(SSO)(SSO Setup Documentation)** checkbox.</span></span>
    
    <span data-ttu-id="43d41-204">b.</span><span class="sxs-lookup"><span data-stu-id="43d41-204">b.</span></span> <span data-ttu-id="43d41-205">Klistra in **SAML enhets-ID** till hello **utfärdar-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="43d41-205">Paste **SAML Entity ID** into hello **SAML issuer URL** textbox.</span></span>
  
    <span data-ttu-id="43d41-206">c.</span><span class="sxs-lookup"><span data-stu-id="43d41-206">c.</span></span> <span data-ttu-id="43d41-207">Klistra in **SAML enkel inloggning Tjänstwebbadress** till hello **slutpunkts-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="43d41-207">Paste **SAML Single Sign-On Service URL** into hello **SAML endpoint URL** textbox.</span></span>
  
    <span data-ttu-id="43d41-208">d.</span><span class="sxs-lookup"><span data-stu-id="43d41-208">d.</span></span> <span data-ttu-id="43d41-209">Klistra in **Sign-Out URL** till hello **logga ut landningssida** textruta.</span><span class="sxs-lookup"><span data-stu-id="43d41-209">Paste **Sign-Out URL** into hello **Logout landing page** textbox.</span></span>
  
    <span data-ttu-id="43d41-210">e.</span><span class="sxs-lookup"><span data-stu-id="43d41-210">e.</span></span> <span data-ttu-id="43d41-211">Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll för hämtat certifikat till Urklipp, och klistra in den i hello **x 509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="43d41-211">Open your downloaded certificate in notepad, copy hello content of downloaded certificate into your clipboard, and then paste it into hello **x 509 certificate** textbox.</span></span>
  
    <span data-ttu-id="43d41-212">f.</span><span class="sxs-lookup"><span data-stu-id="43d41-212">f.</span></span> <span data-ttu-id="43d41-213">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="43d41-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="43d41-214">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="43d41-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="43d41-215">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="43d41-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="43d41-216">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="43d41-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="43d41-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="43d41-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="43d41-218">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43d41-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="43d41-220">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="43d41-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="43d41-221">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43d41-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="43d41-223">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="43d41-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="43d41-225">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43d41-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="43d41-227">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="43d41-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="43d41-229">a.</span><span class="sxs-lookup"><span data-stu-id="43d41-229">a.</span></span> <span data-ttu-id="43d41-230">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="43d41-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="43d41-231">b.</span><span class="sxs-lookup"><span data-stu-id="43d41-231">b.</span></span> <span data-ttu-id="43d41-232">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="43d41-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="43d41-233">c.</span><span class="sxs-lookup"><span data-stu-id="43d41-233">c.</span></span> <span data-ttu-id="43d41-234">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="43d41-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="43d41-235">d.</span><span class="sxs-lookup"><span data-stu-id="43d41-235">d.</span></span> <span data-ttu-id="43d41-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="43d41-236">Click **Create**.</span></span>
 
### <a name="creating-a-cloudpassage-test-user"></a><span data-ttu-id="43d41-237">Skapa en testanvändare CloudPassage</span><span class="sxs-lookup"><span data-stu-id="43d41-237">Creating a CloudPassage test user</span></span>

<span data-ttu-id="43d41-238">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i CloudPassage.</span><span class="sxs-lookup"><span data-stu-id="43d41-238">hello objective of this section is toocreate a user called Britta Simon in CloudPassage.</span></span>

<span data-ttu-id="43d41-239">**toocreate en användare som kallas Britta Simon i CloudPassage, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="43d41-239">**toocreate a user called Britta Simon in CloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="43d41-240">Inloggning tooyour **CloudPassage** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="43d41-240">Sign-on tooyour **CloudPassage** company site as an administrator.</span></span> 

2. <span data-ttu-id="43d41-241">Klicka i hello verktygsfältet hello längst upp **inställningar**, och klicka sedan på **Platsadministration**.</span><span class="sxs-lookup"><span data-stu-id="43d41-241">In hello toolbar on hello top, click **Settings**, and then click **Site Administration**.</span></span> 
   
   ![Skapa en testanvändare CloudPassage][22] 

3. <span data-ttu-id="43d41-243">Klicka på hello **användare** fliken och klicka sedan på **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="43d41-243">Click hello **Users** tab, and then click **Add New User**.</span></span> 
   
   ![Skapa en testanvändare CloudPassage][23]

4. <span data-ttu-id="43d41-245">I hello **Lägg till nya användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="43d41-245">In hello **Add New User** section, perform hello following steps:</span></span> 
   
   ![Skapa en testanvändare CloudPassage][24]
    
    <span data-ttu-id="43d41-247">a.</span><span class="sxs-lookup"><span data-stu-id="43d41-247">a.</span></span> <span data-ttu-id="43d41-248">I hello **Förnamn** textruta skriver Britta.</span><span class="sxs-lookup"><span data-stu-id="43d41-248">In hello **First Name** textbox, type Britta.</span></span> 
  
    <span data-ttu-id="43d41-249">b.</span><span class="sxs-lookup"><span data-stu-id="43d41-249">b.</span></span> <span data-ttu-id="43d41-250">I hello **efternamn** textruta skriver Simon.</span><span class="sxs-lookup"><span data-stu-id="43d41-250">In hello **Last Name** textbox, type Simon.</span></span>
  
    <span data-ttu-id="43d41-251">c.</span><span class="sxs-lookup"><span data-stu-id="43d41-251">c.</span></span> <span data-ttu-id="43d41-252">I hello **användarnamn** textruta hello **e-post** textruta och hello **Skriv e-** textruta anger Brittas användarnamn i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43d41-252">In hello **Username** textbox, hello **Email** textbox and hello **Retype Email** textbox, type Britta's user name in Azure AD.</span></span>
  
    <span data-ttu-id="43d41-253">d.</span><span class="sxs-lookup"><span data-stu-id="43d41-253">d.</span></span> <span data-ttu-id="43d41-254">Som **behörighet**väljer **aktivera Halo Portal åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="43d41-254">As **Access Type**, select **Enable Halo Portal Access**.</span></span>
  
    <span data-ttu-id="43d41-255">e.</span><span class="sxs-lookup"><span data-stu-id="43d41-255">e.</span></span> <span data-ttu-id="43d41-256">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="43d41-256">Click **Add**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="43d41-257">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="43d41-257">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="43d41-258">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCloudPassage.</span><span class="sxs-lookup"><span data-stu-id="43d41-258">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCloudPassage.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="43d41-260">**tooassign Britta Simon tooCloudPassage utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="43d41-260">**tooassign Britta Simon tooCloudPassage, perform hello following steps:**</span></span>

1. <span data-ttu-id="43d41-261">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="43d41-261">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="43d41-263">Välj i listan med program hello **CloudPassage**.</span><span class="sxs-lookup"><span data-stu-id="43d41-263">In hello applications list, select **CloudPassage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

3. <span data-ttu-id="43d41-265">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="43d41-265">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="43d41-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="43d41-267">Click **Add** button.</span></span> <span data-ttu-id="43d41-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43d41-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="43d41-270">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="43d41-270">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="43d41-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43d41-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="43d41-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43d41-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="43d41-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43d41-273">Testing single sign-on</span></span>

<span data-ttu-id="43d41-274">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="43d41-274">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="43d41-275">Du bör få automatiskt inloggade tooyour CloudPassage programmet när du klickar på hello CloudPassage panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="43d41-275">When you click hello CloudPassage tile in hello Access Panel, you should get automatically signed-on tooyour CloudPassage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43d41-276">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="43d41-276">Additional resources</span></span>

* [<span data-ttu-id="43d41-277">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="43d41-277">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="43d41-278">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="43d41-278">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_203.png

