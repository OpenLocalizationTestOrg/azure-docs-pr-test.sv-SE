---
title: "Självstudier: Azure Active Directory-integrering med Aha! | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Aha!."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: d844db3c0a035560e6fb275017215171743fba56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a><span data-ttu-id="7b163-104">Självstudier: Azure Active Directory-integrering med Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-104">Tutorial: Azure Active Directory integration with Aha!</span></span>

<span data-ttu-id="7b163-105">I kursen får du lära dig hur toointegrate Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-105">In this tutorial, you learn how toointegrate Aha!</span></span> <span data-ttu-id="7b163-106">med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7b163-106">with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7b163-107">Integrera Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-107">Integrating Aha!</span></span> <span data-ttu-id="7b163-108">med Azure AD innehåller hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7b163-108">with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7b163-109">Du kan styra i Azure AD som har åtkomst tooAha!</span><span class="sxs-lookup"><span data-stu-id="7b163-109">You can control in Azure AD who has access tooAha!</span></span>
- <span data-ttu-id="7b163-110">Du kan aktivera din användare tooautomatically get inloggade tooAha!</span><span class="sxs-lookup"><span data-stu-id="7b163-110">You can enable your users tooautomatically get signed-on tooAha!</span></span> <span data-ttu-id="7b163-111">(Enkel inloggning) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7b163-111">(Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7b163-112">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7b163-112">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7b163-113">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7b163-113">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7b163-114">Krav</span><span class="sxs-lookup"><span data-stu-id="7b163-114">Prerequisites</span></span>

<span data-ttu-id="7b163-115">tooconfigure Azure AD-integrering med Aha!, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7b163-115">tooconfigure Azure AD integration with Aha!, you need hello following items:</span></span>

- <span data-ttu-id="7b163-116">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7b163-116">An Azure AD subscription</span></span>
- <span data-ttu-id="7b163-117">En Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-117">An Aha!</span></span> <span data-ttu-id="7b163-118">enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7b163-118">single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7b163-119">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7b163-119">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7b163-120">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7b163-120">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7b163-121">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7b163-121">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7b163-122">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7b163-122">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7b163-123">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7b163-123">Scenario description</span></span>
<span data-ttu-id="7b163-124">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7b163-124">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7b163-125">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7b163-125">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7b163-126">Lägga till Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-126">Adding Aha!</span></span> <span data-ttu-id="7b163-127">från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7b163-127">from hello gallery</span></span>
2. <span data-ttu-id="7b163-128">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b163-128">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-aha-from-hello-gallery"></a><span data-ttu-id="7b163-129">Lägga till Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-129">Adding Aha!</span></span> <span data-ttu-id="7b163-130">från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7b163-130">from hello gallery</span></span>
<span data-ttu-id="7b163-131">tooconfigure hello integrering av Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-131">tooconfigure hello integration of Aha!</span></span> <span data-ttu-id="7b163-132">Du behöver tooadd Aha till Azure AD!</span><span class="sxs-lookup"><span data-stu-id="7b163-132">into Azure AD, you need tooadd Aha!</span></span> <span data-ttu-id="7b163-133">hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7b163-133">from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7b163-134">**tooadd Aha! Utför följande hello från galleriet hello:**</span><span class="sxs-lookup"><span data-stu-id="7b163-134">**tooadd Aha! from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b163-135">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7b163-135">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7b163-137">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7b163-137">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7b163-138">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7b163-138">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7b163-140">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b163-140">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7b163-142">Skriv i sökrutan hello **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="7b163-142">In hello search box, type **Aha!**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_search.png)

5. <span data-ttu-id="7b163-144">Markera hello resultat på panelen **Aha!**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7b163-144">In hello results panel, select **Aha!**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/tutorial_aha_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7b163-146">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b163-146">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7b163-147">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-147">In this section, you configure and test Azure AD single sign-on with Aha!</span></span> <span data-ttu-id="7b163-148">baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7b163-148">based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7b163-149">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användare i Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-149">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Aha!</span></span> <span data-ttu-id="7b163-150">är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7b163-150">is tooa user in Azure AD.</span></span> <span data-ttu-id="7b163-151">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-151">In other words, a link relationship between an Azure AD user and hello related user in Aha!</span></span> <span data-ttu-id="7b163-152">måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="7b163-152">needs toobe established.</span></span>

<span data-ttu-id="7b163-153">I Aha!, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7b163-153">In Aha!, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7b163-154">tooconfigure och testa Azure AD enkel inloggning med Aha!, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7b163-154">tooconfigure and test Azure AD single sign-on with Aha!, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7b163-155">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7b163-155">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7b163-156">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b163-156">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7b163-157">**[Skapa en Aha! testanvändare](#creating-an-aha-test-user)**  -toohave en motsvarighet för Britta Simon i Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-157">**[Creating an Aha! test user](#creating-an-aha-test-user)** - toohave a counterpart of Britta Simon in Aha!</span></span> <span data-ttu-id="7b163-158">som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7b163-158">that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7b163-159">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7b163-159">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7b163-160">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7b163-160">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7b163-161">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b163-161">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7b163-162">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-162">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Aha!</span></span> <span data-ttu-id="7b163-163">Programmet.</span><span class="sxs-lookup"><span data-stu-id="7b163-163">application.</span></span>

<span data-ttu-id="7b163-164">**tooconfigure Azure AD enkel inloggning med Aha!, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="7b163-164">**tooconfigure Azure AD single sign-on with Aha!, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b163-165">I hello Azure-portalen på hello **Aha!**</span><span class="sxs-lookup"><span data-stu-id="7b163-165">In hello Azure portal, on hello **Aha!**</span></span> <span data-ttu-id="7b163-166">programmet integrationssidan klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7b163-166">application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7b163-168">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7b163-168">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_samlbase.png)

3. <span data-ttu-id="7b163-170">På hello **Aha! Domänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7b163-170">On hello **Aha! Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_url.png)

    <span data-ttu-id="7b163-172">a.</span><span class="sxs-lookup"><span data-stu-id="7b163-172">a.</span></span> <span data-ttu-id="7b163-173">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.aha.io/session/new`</span><span class="sxs-lookup"><span data-stu-id="7b163-173">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.aha.io/session/new`</span></span>

    <span data-ttu-id="7b163-174">b.</span><span class="sxs-lookup"><span data-stu-id="7b163-174">b.</span></span> <span data-ttu-id="7b163-175">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.aha.io`</span><span class="sxs-lookup"><span data-stu-id="7b163-175">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.aha.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7b163-176">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7b163-176">These values are not real.</span></span> <span data-ttu-id="7b163-177">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7b163-177">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7b163-178">Kontakta [Aha! Klienten supportteamet](https://www.aha.io/company/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7b163-178">Contact [Aha! Client support team](https://www.aha.io/company/contact) tooget these values.</span></span> 
 
4. <span data-ttu-id="7b163-179">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="7b163-179">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_certificate.png) 

5. <span data-ttu-id="7b163-181">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7b163-181">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7b163-183">Logga in tooyour Aha i ett annat webbläsarfönster!</span><span class="sxs-lookup"><span data-stu-id="7b163-183">In a different web browser window, log in tooyour Aha!</span></span> <span data-ttu-id="7b163-184">företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="7b163-184">company site as an administrator.</span></span>

7. <span data-ttu-id="7b163-185">Hello-menyn överst hello **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="7b163-185">In hello menu on hello top, click **Settings**.</span></span>

    <span data-ttu-id="7b163-186">![Inställningar för](./media/active-directory-saas-aha-tutorial/IC798950.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="7b163-186">![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")</span></span>

8. <span data-ttu-id="7b163-187">Klicka på **konto**.</span><span class="sxs-lookup"><span data-stu-id="7b163-187">Click **Account**.</span></span>
   
    <span data-ttu-id="7b163-188">![Profilen](./media/active-directory-saas-aha-tutorial/IC798951.png "profil")</span><span class="sxs-lookup"><span data-stu-id="7b163-188">![Profile](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")</span></span>

9. <span data-ttu-id="7b163-189">Klicka på **säkerhet och enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7b163-189">Click **Security and single sign-on**.</span></span>
   
    <span data-ttu-id="7b163-190">![Säkerhet och enkel inloggning](./media/active-directory-saas-aha-tutorial/IC798952.png "säkerhet och enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7b163-190">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")</span></span>

10. <span data-ttu-id="7b163-191">I **enkel inloggning** avsnittet som **identitetsleverantör**väljer **SAML2.0**.</span><span class="sxs-lookup"><span data-stu-id="7b163-191">In **Single Sign-On** section, as **Identity Provider**, select **SAML2.0**.</span></span>
   
    <span data-ttu-id="7b163-192">![Säkerhet och enkel inloggning](./media/active-directory-saas-aha-tutorial/IC798953.png "säkerhet och enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7b163-192">![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")</span></span>

11. <span data-ttu-id="7b163-193">På hello **enkel inloggning** konfigurationen utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7b163-193">On hello **Single Sign-On** configuration page, perform hello following steps:</span></span>
    
    <span data-ttu-id="7b163-194">![Enkel inloggning](./media/active-directory-saas-aha-tutorial/IC798954.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7b163-194">![Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")</span></span>
    
       <span data-ttu-id="7b163-195">a.</span><span class="sxs-lookup"><span data-stu-id="7b163-195">a.</span></span> <span data-ttu-id="7b163-196">I hello **namn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7b163-196">In hello **Name** textbox, type a name for your configuration.</span></span>

       <span data-ttu-id="7b163-197">b.</span><span class="sxs-lookup"><span data-stu-id="7b163-197">b.</span></span> <span data-ttu-id="7b163-198">För **konfigurera med hjälp av**väljer **metadatafil**.</span><span class="sxs-lookup"><span data-stu-id="7b163-198">For **Configure using**, select **Metadata File**.</span></span>
   
       <span data-ttu-id="7b163-199">c.</span><span class="sxs-lookup"><span data-stu-id="7b163-199">c.</span></span> <span data-ttu-id="7b163-200">tooupload metadata för hämtade filer, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="7b163-200">tooupload your downloaded metadata file, click **Browse**.</span></span>
   
       <span data-ttu-id="7b163-201">d.</span><span class="sxs-lookup"><span data-stu-id="7b163-201">d.</span></span> <span data-ttu-id="7b163-202">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="7b163-202">Click **Update**.</span></span>

> [!TIP]
> <span data-ttu-id="7b163-203">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7b163-203">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7b163-204">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7b163-204">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7b163-205">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7b163-205">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7b163-206">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b163-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="7b163-207">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b163-207">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7b163-209">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7b163-209">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b163-210">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7b163-210">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7b163-212">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7b163-212">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7b163-214">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b163-214">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7b163-216">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7b163-216">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-aha-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7b163-218">a.</span><span class="sxs-lookup"><span data-stu-id="7b163-218">a.</span></span> <span data-ttu-id="7b163-219">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7b163-219">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7b163-220">b.</span><span class="sxs-lookup"><span data-stu-id="7b163-220">b.</span></span> <span data-ttu-id="7b163-221">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7b163-221">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7b163-222">c.</span><span class="sxs-lookup"><span data-stu-id="7b163-222">c.</span></span> <span data-ttu-id="7b163-223">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7b163-223">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7b163-224">d.</span><span class="sxs-lookup"><span data-stu-id="7b163-224">d.</span></span> <span data-ttu-id="7b163-225">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7b163-225">Click **Create**.</span></span>
 
### <a name="creating-an-aha-test-user"></a><span data-ttu-id="7b163-226">Skapa en Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-226">Creating an Aha!</span></span> <span data-ttu-id="7b163-227">testanvändare</span><span class="sxs-lookup"><span data-stu-id="7b163-227">test user</span></span>

<span data-ttu-id="7b163-228">tooenable Azure AD-användare toolog i tooAha!, de måste etableras i Aha!.</span><span class="sxs-lookup"><span data-stu-id="7b163-228">tooenable Azure AD users toolog in tooAha!, they must be provisioned into Aha!.</span></span>  

<span data-ttu-id="7b163-229">Hello gäller Aha!, etablering är en automatisk uppgift.</span><span class="sxs-lookup"><span data-stu-id="7b163-229">In hello case of Aha!, provisioning is an automated task.</span></span> <span data-ttu-id="7b163-230">Det finns ingen åtgärd-objekt.</span><span class="sxs-lookup"><span data-stu-id="7b163-230">There is no action item for you.</span></span>

<span data-ttu-id="7b163-231">Användare skapas automatiskt om det behövs under hello första enkel inloggning försöket.</span><span class="sxs-lookup"><span data-stu-id="7b163-231">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

>[!NOTE]
><span data-ttu-id="7b163-232">Du kan använda andra Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-232">You can use any other Aha!</span></span> <span data-ttu-id="7b163-233">Verktyg för att skapa användaren konto eller API: er som tillhandahålls av Aha!</span><span class="sxs-lookup"><span data-stu-id="7b163-233">user account creation tools or APIs provided by Aha!</span></span> <span data-ttu-id="7b163-234">tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="7b163-234">tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7b163-235">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7b163-235">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7b163-236">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAha!.</span><span class="sxs-lookup"><span data-stu-id="7b163-236">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAha!.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7b163-238">**tooassign Britta Simon tooAha!, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="7b163-238">**tooassign Britta Simon tooAha!, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b163-239">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7b163-239">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7b163-241">Välj i listan med program hello **Aha!**.</span><span class="sxs-lookup"><span data-stu-id="7b163-241">In hello applications list, select **Aha!**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-aha-tutorial/tutorial_aha_app.png) 

3. <span data-ttu-id="7b163-243">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7b163-243">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7b163-245">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7b163-245">Click **Add** button.</span></span> <span data-ttu-id="7b163-246">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b163-246">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7b163-248">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7b163-248">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7b163-249">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b163-249">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7b163-250">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7b163-250">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7b163-251">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7b163-251">Testing single sign-on</span></span>

<span data-ttu-id="7b163-252">Om du vill tootest dina inställningar för enkel inloggning, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7b163-252">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="7b163-253">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7b163-253">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b163-254">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7b163-254">Additional resources</span></span>

* [<span data-ttu-id="7b163-255">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7b163-255">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7b163-256">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7b163-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-aha-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-aha-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-aha-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-aha-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-aha-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-aha-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-aha-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-aha-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-aha-tutorial/tutorial_general_203.png

