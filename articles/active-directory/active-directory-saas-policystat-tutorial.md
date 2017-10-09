---
title: "Självstudier: Azure Active Directory-integrering med PolicyStat | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PolicyStat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 868053cd0d37359fd9b4aeb93dba42cbbaa09845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a><span data-ttu-id="263c2-103">Självstudier: Azure Active Directory-integrering med PolicyStat</span><span class="sxs-lookup"><span data-stu-id="263c2-103">Tutorial: Azure Active Directory integration with PolicyStat</span></span>

<span data-ttu-id="263c2-104">I kursen får du lära dig hur toointegrate PolicyStat med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="263c2-104">In this tutorial, you learn how toointegrate PolicyStat with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="263c2-105">Integrera PolicyStat med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="263c2-105">Integrating PolicyStat with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="263c2-106">Du kan styra i Azure AD som har åtkomst till tooPolicyStat</span><span class="sxs-lookup"><span data-stu-id="263c2-106">You can control in Azure AD who has access tooPolicyStat</span></span>
- <span data-ttu-id="263c2-107">Du kan aktivera din användare tooautomatically get inloggade tooPolicyStat (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="263c2-107">You can enable your users tooautomatically get signed-on tooPolicyStat (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="263c2-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="263c2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="263c2-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="263c2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="263c2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="263c2-110">Prerequisites</span></span>

<span data-ttu-id="263c2-111">tooconfigure Azure AD-integrering med PolicyStat, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="263c2-111">tooconfigure Azure AD integration with PolicyStat, you need hello following items:</span></span>

- <span data-ttu-id="263c2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="263c2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="263c2-113">En PolicyStat enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="263c2-113">A PolicyStat single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="263c2-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="263c2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="263c2-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="263c2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="263c2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="263c2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="263c2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="263c2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="263c2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="263c2-118">Scenario description</span></span>
<span data-ttu-id="263c2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="263c2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="263c2-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="263c2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="263c2-121">Att lägga till PolicyStat från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="263c2-121">Adding PolicyStat from hello gallery</span></span>
2. <span data-ttu-id="263c2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="263c2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-policystat-from-hello-gallery"></a><span data-ttu-id="263c2-123">Att lägga till PolicyStat från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="263c2-123">Adding PolicyStat from hello gallery</span></span>
<span data-ttu-id="263c2-124">tooconfigure hello integrering av PolicyStat i Azure AD, behöver du tooadd PolicyStat hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="263c2-124">tooconfigure hello integration of PolicyStat into Azure AD, you need tooadd PolicyStat from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="263c2-125">**tooadd PolicyStat från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="263c2-125">**tooadd PolicyStat from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="263c2-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="263c2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="263c2-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="263c2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="263c2-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="263c2-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="263c2-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="263c2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="263c2-133">Skriv i sökrutan hello **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="263c2-133">In hello search box, type **PolicyStat**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. <span data-ttu-id="263c2-135">Markera hello resultat på panelen **PolicyStat**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="263c2-135">In hello results panel, select **PolicyStat**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="263c2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="263c2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="263c2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PolicyStat baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="263c2-138">In this section, you configure and test Azure AD single sign-on with PolicyStat based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="263c2-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PolicyStat är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="263c2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PolicyStat is tooa user in Azure AD.</span></span> <span data-ttu-id="263c2-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PolicyStat toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="263c2-140">In other words, a link relationship between an Azure AD user and hello related user in PolicyStat needs toobe established.</span></span>

<span data-ttu-id="263c2-141">I PolicyStat, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="263c2-141">In PolicyStat, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="263c2-142">tooconfigure och testa Azure AD enkel inloggning med PolicyStat, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="263c2-142">tooconfigure and test Azure AD single sign-on with PolicyStat, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="263c2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="263c2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="263c2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="263c2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="263c2-145">**[Skapa en testanvändare PolicyStat](#creating-a-policystat-test-user)**  -toohave en motsvarighet för Britta Simon i PolicyStat som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="263c2-145">**[Creating a PolicyStat test user](#creating-a-policystat-test-user)** - toohave a counterpart of Britta Simon in PolicyStat that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="263c2-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="263c2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="263c2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="263c2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="263c2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="263c2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="263c2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt PolicyStat program.</span><span class="sxs-lookup"><span data-stu-id="263c2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your PolicyStat application.</span></span>

<span data-ttu-id="263c2-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PolicyStat:**</span><span class="sxs-lookup"><span data-stu-id="263c2-150">**tooconfigure Azure AD single sign-on with PolicyStat, perform hello following steps:**</span></span>

1. <span data-ttu-id="263c2-151">I hello Azure-portalen på hello **PolicyStat** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="263c2-151">In hello Azure portal, on hello **PolicyStat** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="263c2-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="263c2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. <span data-ttu-id="263c2-155">På hello **PolicyStat domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="263c2-155">On hello **PolicyStat Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    <span data-ttu-id="263c2-157">a.</span><span class="sxs-lookup"><span data-stu-id="263c2-157">a.</span></span> <span data-ttu-id="263c2-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.policystat.com`</span><span class="sxs-lookup"><span data-stu-id="263c2-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.policystat.com`</span></span>

    <span data-ttu-id="263c2-159">b.</span><span class="sxs-lookup"><span data-stu-id="263c2-159">b.</span></span> <span data-ttu-id="263c2-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.policystat.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="263c2-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.policystat.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="263c2-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="263c2-161">These values are not real.</span></span> <span data-ttu-id="263c2-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="263c2-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="263c2-163">Kontakta [PolicyStat klienten supportteamet](http://www.policystat.com/support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="263c2-163">Contact [PolicyStat Client support team](http://www.policystat.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="263c2-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="263c2-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. <span data-ttu-id="263c2-166">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooPolicyStat med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="263c2-166">hello objective of this section is toooutline how tooenable users tooauthenticate tooPolicyStat with their account in Azure AD using federation based on hello SAML protocol.</span></span>

    <span data-ttu-id="263c2-167">Hej PolicyStat program förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **SAML-Token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="263c2-167">hello PolicyStat application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span>  

     <span data-ttu-id="263c2-168">hello följande skärmbild visar ett exempel på detta.</span><span class="sxs-lookup"><span data-stu-id="263c2-168">hello following screenshot shows an example of this.</span></span>

     <span data-ttu-id="263c2-169">![Attribut](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="263c2-169">![Attributes](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "Attributes")</span></span>

6. <span data-ttu-id="263c2-170">mappningar av tooadd hello krävs, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="263c2-170">tooadd hello required attribute mappings, perform hello following steps:</span></span>

    | <span data-ttu-id="263c2-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="263c2-171">Attribute Name</span></span>    |   <span data-ttu-id="263c2-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="263c2-172">Attribute Value</span></span> |
    |------------------- | -------------------- |
    | <span data-ttu-id="263c2-173">UID</span><span class="sxs-lookup"><span data-stu-id="263c2-173">uid</span></span> | <span data-ttu-id="263c2-174">ExtractMailPrefix([mail])</span><span class="sxs-lookup"><span data-stu-id="263c2-174">ExtractMailPrefix([mail])</span></span> |
    
    <span data-ttu-id="263c2-175">a.</span><span class="sxs-lookup"><span data-stu-id="263c2-175">a.</span></span> <span data-ttu-id="263c2-176">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="263c2-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    <span data-ttu-id="263c2-179">b.</span><span class="sxs-lookup"><span data-stu-id="263c2-179">b.</span></span> <span data-ttu-id="263c2-180">I hello **attributnamn** textruta typen **uid**.</span><span class="sxs-lookup"><span data-stu-id="263c2-180">In hello **Attribute Name** textbox, type **uid**.</span></span>

    <span data-ttu-id="263c2-181">c.</span><span class="sxs-lookup"><span data-stu-id="263c2-181">c.</span></span> <span data-ttu-id="263c2-182">I hello **attributvärdet** textruta väljer **ExtractMailPrefix()**.</span><span class="sxs-lookup"><span data-stu-id="263c2-182">In hello **Attribute Value** textbox, select **ExtractMailPrefix()**.</span></span>    
   
    <span data-ttu-id="263c2-183">d.</span><span class="sxs-lookup"><span data-stu-id="263c2-183">d.</span></span> <span data-ttu-id="263c2-184">Från hello **e** väljer **User.mail**.</span><span class="sxs-lookup"><span data-stu-id="263c2-184">From hello **Mail** list, select **User.mail**.</span></span>
    
    <span data-ttu-id="263c2-185">e.</span><span class="sxs-lookup"><span data-stu-id="263c2-185">e.</span></span> <span data-ttu-id="263c2-186">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="263c2-186">Click **Ok**</span></span>

7. <span data-ttu-id="263c2-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="263c2-187">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="263c2-189">Logga in på webbplatsen PolicyStat företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="263c2-189">In a different web browser window, log into your PolicyStat company site as an administrator.</span></span>

9. <span data-ttu-id="263c2-190">Klicka på hello **Admin** fliken och klicka sedan på **konfiguration för enkel inloggning** i vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="263c2-190">Click hello **Admin** tab, and then click **Single Sign-On Configuration** in left navigation pane.</span></span>
   
    <span data-ttu-id="263c2-191">![Administratörsmenyn](./media/active-directory-saas-policystat-tutorial/ic808633.png "administratör-menyn")</span><span class="sxs-lookup"><span data-stu-id="263c2-191">![Administrator Menu](./media/active-directory-saas-policystat-tutorial/ic808633.png "Administrator Menu")</span></span>

10. <span data-ttu-id="263c2-192">I hello **installationsprogrammet** väljer **aktivera enkel inloggning Integration**.</span><span class="sxs-lookup"><span data-stu-id="263c2-192">In hello **Setup** section, select **Enable Single Sign-on Integration**.</span></span>
   
    <span data-ttu-id="263c2-193">![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="263c2-193">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "Single Sign-On Configuration")</span></span>

11. <span data-ttu-id="263c2-194">Klicka på **Konfigurera attribut**, och sedan, i hello **Konfigurera attribut** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="263c2-194">Click **Configure Attributes**, and then, in hello **Configure Attributes** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="263c2-195">![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="263c2-195">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="263c2-196">a.</span><span class="sxs-lookup"><span data-stu-id="263c2-196">a.</span></span> <span data-ttu-id="263c2-197">I hello **Username-attributet** textruta typen **uid**.</span><span class="sxs-lookup"><span data-stu-id="263c2-197">In hello **Username Attribute** textbox, type **uid**.</span></span>

    <span data-ttu-id="263c2-198">b.</span><span class="sxs-lookup"><span data-stu-id="263c2-198">b.</span></span> <span data-ttu-id="263c2-199">I hello **förnamn attributet** textruta typen **Förnamn** för användare **Britta**.</span><span class="sxs-lookup"><span data-stu-id="263c2-199">In hello **First Name Attribute** textbox, type **firstname** of user **Britta**.</span></span>

    <span data-ttu-id="263c2-200">c.</span><span class="sxs-lookup"><span data-stu-id="263c2-200">c.</span></span> <span data-ttu-id="263c2-201">I hello **senaste namnattributet** textruta typen **efternamn** för användare **Simon**.</span><span class="sxs-lookup"><span data-stu-id="263c2-201">In hello **Last Name Attribute** textbox, type **lastname** of user **Simon**.</span></span>

    <span data-ttu-id="263c2-202">d.</span><span class="sxs-lookup"><span data-stu-id="263c2-202">d.</span></span> <span data-ttu-id="263c2-203">I hello **e-attributet** textruta typen **emailaddress** för användare  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="263c2-203">In hello **Email Attribute** textbox, type **emailaddress** of user **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="263c2-204">e.</span><span class="sxs-lookup"><span data-stu-id="263c2-204">e.</span></span> <span data-ttu-id="263c2-205">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="263c2-205">Click **Save Changes**.</span></span>

12. <span data-ttu-id="263c2-206">Klicka på **din IDP Metadata**, och sedan, i hello **din IDP Metadata** avsnittet, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="263c2-206">Click **Your IDP Metadata**, and then, in hello **Your IDP Metadata** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="263c2-207">![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="263c2-207">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="263c2-208">a.</span><span class="sxs-lookup"><span data-stu-id="263c2-208">a.</span></span> <span data-ttu-id="263c2-209">Öppna din hämtade metadatafil, kopiera hello innehåll och klistra in den i hello **din identitet providern Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="263c2-209">Open your downloaded metadata file, copy hello content, and  then paste it into hello **Your Identity Provider Metadata** textbox.</span></span>

    <span data-ttu-id="263c2-210">b.</span><span class="sxs-lookup"><span data-stu-id="263c2-210">b.</span></span> <span data-ttu-id="263c2-211">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="263c2-211">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="263c2-212">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="263c2-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="263c2-213">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="263c2-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="263c2-214">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="263c2-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="263c2-215">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="263c2-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="263c2-216">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="263c2-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="263c2-218">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="263c2-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="263c2-219">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="263c2-219">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="263c2-221">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="263c2-221">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="263c2-223">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="263c2-223">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="263c2-225">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="263c2-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="263c2-227">a.</span><span class="sxs-lookup"><span data-stu-id="263c2-227">a.</span></span> <span data-ttu-id="263c2-228">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="263c2-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="263c2-229">b.</span><span class="sxs-lookup"><span data-stu-id="263c2-229">b.</span></span> <span data-ttu-id="263c2-230">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="263c2-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="263c2-231">c.</span><span class="sxs-lookup"><span data-stu-id="263c2-231">c.</span></span> <span data-ttu-id="263c2-232">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="263c2-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="263c2-233">d.</span><span class="sxs-lookup"><span data-stu-id="263c2-233">d.</span></span> <span data-ttu-id="263c2-234">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="263c2-234">Click **Create**.</span></span>
 
### <a name="creating-a-policystat-test-user"></a><span data-ttu-id="263c2-235">Skapa en testanvändare PolicyStat</span><span class="sxs-lookup"><span data-stu-id="263c2-235">Creating a PolicyStat test user</span></span>

<span data-ttu-id="263c2-236">I ordning tooenable Azure AD-användare toolog i PolicyStat, måste de etableras i PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="263c2-236">In order tooenable Azure AD users toolog into PolicyStat, they must be provisioned into PolicyStat.</span></span>  

<span data-ttu-id="263c2-237">PolicyStat stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="263c2-237">PolicyStat supports just in time user provisioning.</span></span> <span data-ttu-id="263c2-238">Det innebär du inte behöver tooadd hello användare manuellt tooPolicyStat.</span><span class="sxs-lookup"><span data-stu-id="263c2-238">This means, you do not need tooadd hello users manually tooPolicyStat.</span></span> <span data-ttu-id="263c2-239">hello användare ska få automatiskt läggas till på deras första inloggning via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="263c2-239">hello users will get added automatically on their first login through SSO.</span></span>

>[!NOTE]
><span data-ttu-id="263c2-240">Du kan använda något annat PolicyStat användarens konto skapas verktyg eller API: er som tillhandahålls av PolicyStat tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="263c2-240">You can use any other PolicyStat user account creation tools or APIs provided by PolicyStat tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="263c2-241">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="263c2-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="263c2-242">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPolicyStat.</span><span class="sxs-lookup"><span data-stu-id="263c2-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPolicyStat.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="263c2-244">**tooassign Britta Simon tooPolicyStat utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="263c2-244">**tooassign Britta Simon tooPolicyStat, perform hello following steps:**</span></span>

1. <span data-ttu-id="263c2-245">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="263c2-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="263c2-247">Välj i listan med program hello **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="263c2-247">In hello applications list, select **PolicyStat**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. <span data-ttu-id="263c2-249">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="263c2-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="263c2-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="263c2-251">Click **Add** button.</span></span> <span data-ttu-id="263c2-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="263c2-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="263c2-254">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="263c2-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="263c2-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="263c2-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="263c2-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="263c2-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="263c2-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="263c2-257">Testing single sign-on</span></span>

<span data-ttu-id="263c2-258">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="263c2-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="263c2-259">Du bör få automatiskt inloggade tooyour PolicyStat programmet när du klickar på hello PolicyStat panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="263c2-259">When you click hello PolicyStat tile in hello Access Panel, you should get automatically signed-on tooyour PolicyStat application.</span></span>
<span data-ttu-id="263c2-260">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="263c2-260">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="263c2-261">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="263c2-261">Additional resources</span></span>

* [<span data-ttu-id="263c2-262">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="263c2-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="263c2-263">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="263c2-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

