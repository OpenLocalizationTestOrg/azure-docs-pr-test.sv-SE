---
title: "Självstudier: Azure Active Directory-integrering med Adobe | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Adobe logga."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: b4b07907f30a0890003554a02a76d968400b43ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a><span data-ttu-id="5ae38-103">Självstudier: Azure Active Directory-integrering med Adobe</span><span class="sxs-lookup"><span data-stu-id="5ae38-103">Tutorial: Azure Active Directory integration with Adobe Sign</span></span>

<span data-ttu-id="5ae38-104">I kursen får du lära dig hur toointegrate Adobe logga med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5ae38-104">In this tutorial, you learn how toointegrate Adobe Sign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5ae38-105">Integrera Adobe inloggning med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5ae38-105">Integrating Adobe Sign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5ae38-106">Du kan styra i Azure AD som har åtkomst tooAdobe inloggning</span><span class="sxs-lookup"><span data-stu-id="5ae38-106">You can control in Azure AD who has access tooAdobe Sign</span></span>
- <span data-ttu-id="5ae38-107">Du kan aktivera din användare tooautomatically get inloggade tooAdobe logga (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5ae38-107">You can enable your users tooautomatically get signed-on tooAdobe Sign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5ae38-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5ae38-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5ae38-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5ae38-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ae38-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5ae38-110">Prerequisites</span></span>

<span data-ttu-id="5ae38-111">tooconfigure Azure AD-integrering med Adobe, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5ae38-111">tooconfigure Azure AD integration with Adobe Sign, you need hello following items:</span></span>

- <span data-ttu-id="5ae38-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5ae38-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5ae38-113">En Adobe logga enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5ae38-113">An Adobe Sign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5ae38-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5ae38-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5ae38-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5ae38-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5ae38-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5ae38-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5ae38-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5ae38-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5ae38-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5ae38-118">Scenario description</span></span>
<span data-ttu-id="5ae38-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5ae38-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5ae38-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5ae38-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5ae38-121">Lägga till Adobe logga från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5ae38-121">Adding Adobe Sign from hello gallery</span></span>
2. <span data-ttu-id="5ae38-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ae38-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-sign-from-hello-gallery"></a><span data-ttu-id="5ae38-123">Lägga till Adobe logga från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5ae38-123">Adding Adobe Sign from hello gallery</span></span>
<span data-ttu-id="5ae38-124">tooconfigure hello integrering av Adobe inloggning i Azure AD, behöver du tooadd Adobe logga hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5ae38-124">tooconfigure hello integration of Adobe Sign into Azure AD, you need tooadd Adobe Sign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5ae38-125">**tooadd Adobe logga från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5ae38-125">**tooadd Adobe Sign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ae38-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5ae38-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5ae38-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5ae38-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5ae38-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ae38-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5ae38-133">Skriv i sökrutan hello **Adobe logga**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-133">In hello search box, type **Adobe Sign**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. <span data-ttu-id="5ae38-135">Markera hello resultat på panelen **Adobe logga**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5ae38-135">In hello results panel, select **Adobe Sign**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5ae38-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ae38-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5ae38-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Adobe baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5ae38-138">In this section, you configure and test Azure AD single sign-on with Adobe Sign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5ae38-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Adobe logga är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5ae38-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adobe Sign is tooa user in Azure AD.</span></span> <span data-ttu-id="5ae38-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Adobe logga toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5ae38-140">In other words, a link relationship between an Azure AD user and hello related user in Adobe Sign needs toobe established.</span></span>

<span data-ttu-id="5ae38-141">I Adobe logga tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5ae38-141">In Adobe Sign, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5ae38-142">tooconfigure och testa Azure AD enkel inloggning med Adobe, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5ae38-142">tooconfigure and test Azure AD single sign-on with Adobe Sign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5ae38-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5ae38-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5ae38-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ae38-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5ae38-145">**[Skapa en testanvändare Adobe logga](#creating-an-adobe-sign-test-user)**  -toohave en motsvarighet för Britta Simon i Adobe tecken som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5ae38-145">**[Creating an Adobe Sign test user](#creating-an-adobe-sign-test-user)** - toohave a counterpart of Britta Simon in Adobe Sign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5ae38-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5ae38-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5ae38-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5ae38-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5ae38-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ae38-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5ae38-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Adobe signera programmet.</span><span class="sxs-lookup"><span data-stu-id="5ae38-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Adobe Sign application.</span></span>

<span data-ttu-id="5ae38-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Adobe tecken:**</span><span class="sxs-lookup"><span data-stu-id="5ae38-150">**tooconfigure Azure AD single sign-on with Adobe Sign, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ae38-151">I hello Azure-portalen på hello **Adobe logga** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-151">In hello Azure portal, on hello **Adobe Sign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5ae38-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5ae38-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. <span data-ttu-id="5ae38-155">På hello **Adobe logga domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ae38-155">On hello **Adobe Sign Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_url.png)

    <span data-ttu-id="5ae38-157">a.</span><span class="sxs-lookup"><span data-stu-id="5ae38-157">a.</span></span> <span data-ttu-id="5ae38-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.echosign.com/`</span><span class="sxs-lookup"><span data-stu-id="5ae38-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.echosign.com/`</span></span>

    <span data-ttu-id="5ae38-159">b.</span><span class="sxs-lookup"><span data-stu-id="5ae38-159">b.</span></span> <span data-ttu-id="5ae38-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.echosign.com`</span><span class="sxs-lookup"><span data-stu-id="5ae38-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.echosign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5ae38-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5ae38-161">These values are not real.</span></span> <span data-ttu-id="5ae38-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="5ae38-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="5ae38-163">Kontakta [Adobe logga klienten supportteamet](https://helpx.adobe.com/in/contact/support.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5ae38-163">Contact [Adobe Sign Client support team](https://helpx.adobe.com/in/contact/support.html) tooget these values.</span></span> 
 
4. <span data-ttu-id="5ae38-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="5ae38-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. <span data-ttu-id="5ae38-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5ae38-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5ae38-168">På hello **Adobe logga Configuration** klickar du på **Konfigurera Adobe logga** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5ae38-168">On hello **Adobe Sign Configuration** section, click **Configure Adobe Sign** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5ae38-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5ae38-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_configure.png) 


7. <span data-ttu-id="5ae38-171">Logga in tooyour Adobe logga företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="5ae38-171">In a different web browser window, log in tooyour Adobe Sign company site as an administrator.</span></span>

8. <span data-ttu-id="5ae38-172">Hello-menyn överst hello **konto**, och hello navigeringsfönstret hello vänster, klicka **SAML inställningar** under **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-172">In hello menu on hello top, click **Account**, and then, in hello navigation pane on hello left side, click **SAML Settings** under **Account Settings**.</span></span>
   
   <span data-ttu-id="5ae38-173">![Kontot](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "konto")</span><span class="sxs-lookup"><span data-stu-id="5ae38-173">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789520.png "Account")</span></span>

9. <span data-ttu-id="5ae38-174">I hello SAML-inställningar, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ae38-174">In hello SAML Settings section, perform hello following steps:</span></span>
   
   <span data-ttu-id="5ae38-175">![Inställningar för SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="5ae38-175">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789521.png "SAML Settings")</span></span>
   
   <span data-ttu-id="5ae38-176">a.</span><span class="sxs-lookup"><span data-stu-id="5ae38-176">a.</span></span> <span data-ttu-id="5ae38-177">Som **SAML läge**väljer **SAML obligatoriska**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-177">As **SAML Mode**, select **SAML Mandatory**.</span></span>
   
   <span data-ttu-id="5ae38-178">b.</span><span class="sxs-lookup"><span data-stu-id="5ae38-178">b.</span></span> <span data-ttu-id="5ae38-179">Välj **Tillåt EchoSign Kontoadministratörer toolog i med hjälp av autentiseringsuppgifterna EchoSign**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-179">Select **Allow EchoSign Account Administrators toolog in using their EchoSign Credentials**.</span></span>
   
   <span data-ttu-id="5ae38-180">c.</span><span class="sxs-lookup"><span data-stu-id="5ae38-180">c.</span></span> <span data-ttu-id="5ae38-181">Som **Användarskapelse**väljer **Lägg automatiskt till användare som autentiseras via SAML**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-181">As **User Creation**, select **Automatically add users authenticated through SAML**.</span></span>

10. <span data-ttu-id="5ae38-182">Flytta, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ae38-182">Move on, performing hello following steps:</span></span>

       <span data-ttu-id="5ae38-183">![Inställningar för SAML](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML-inställningar")</span><span class="sxs-lookup"><span data-stu-id="5ae38-183">![SAML Settings](./media/active-directory-saas-adobe-echosign-tutorial/ic789522.png "SAML Settings")</span></span>

    <span data-ttu-id="5ae38-184">a.</span><span class="sxs-lookup"><span data-stu-id="5ae38-184">a.</span></span> <span data-ttu-id="5ae38-185">Klistra in **SAML enhets-ID**, som du har kopierat från Azure-portalen i hello **IdP enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="5ae38-185">Paste **SAML Entity ID**, which you have copied from Azure portal into hello **IdP Entity ID** textbox.</span></span>
    
    <span data-ttu-id="5ae38-186">b.</span><span class="sxs-lookup"><span data-stu-id="5ae38-186">b.</span></span> <span data-ttu-id="5ae38-187">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från Azure-portalen i hello **IdP inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="5ae38-187">Paste **SAML Single Sign-On Service URL**, which you have copied from Azure portal into hello **IdP Login URL** textbox.</span></span>
   
    <span data-ttu-id="5ae38-188">c.</span><span class="sxs-lookup"><span data-stu-id="5ae38-188">c.</span></span> <span data-ttu-id="5ae38-189">Klistra in **Sign-Out URL**, som du har kopierat från Azure-portalen i hello **IdP logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="5ae38-189">Paste **Sign-Out URL**, which you have copied from Azure portal into hello **IdP Logout URL** textbox.</span></span>

    <span data-ttu-id="5ae38-190">d.</span><span class="sxs-lookup"><span data-stu-id="5ae38-190">d.</span></span> <span data-ttu-id="5ae38-191">Öppna din hämtade **Certificate(Base64)** filen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **IdP certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="5ae38-191">Open your downloaded **Certificate(Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Certificate** textbox</span></span>

    <span data-ttu-id="5ae38-192">e.</span><span class="sxs-lookup"><span data-stu-id="5ae38-192">e.</span></span> <span data-ttu-id="5ae38-193">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-193">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="5ae38-194">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5ae38-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5ae38-195">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5ae38-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5ae38-196">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5ae38-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5ae38-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5ae38-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="5ae38-198">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5ae38-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5ae38-200">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5ae38-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ae38-201">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5ae38-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5ae38-203">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5ae38-205">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ae38-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5ae38-207">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ae38-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-echosign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5ae38-209">a.</span><span class="sxs-lookup"><span data-stu-id="5ae38-209">a.</span></span> <span data-ttu-id="5ae38-210">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5ae38-211">b.</span><span class="sxs-lookup"><span data-stu-id="5ae38-211">b.</span></span> <span data-ttu-id="5ae38-212">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5ae38-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5ae38-213">c.</span><span class="sxs-lookup"><span data-stu-id="5ae38-213">c.</span></span> <span data-ttu-id="5ae38-214">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5ae38-215">d.</span><span class="sxs-lookup"><span data-stu-id="5ae38-215">d.</span></span> <span data-ttu-id="5ae38-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-216">Click **Create**.</span></span>
 
### <a name="creating-an-adobe-sign-test-user"></a><span data-ttu-id="5ae38-217">Skapa en Adobe logga testanvändare</span><span class="sxs-lookup"><span data-stu-id="5ae38-217">Creating an Adobe Sign test user</span></span>

<span data-ttu-id="5ae38-218">tooenable Azure AD-användare toolog i tooAdobe logga de att etableras i Adobe logga.</span><span class="sxs-lookup"><span data-stu-id="5ae38-218">tooenable Azure AD users toolog in tooAdobe Sign, they must be provisioned into Adobe Sign.</span></span> <span data-ttu-id="5ae38-219">I hello fallet Adobe tecken är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5ae38-219">In hello case of Adobe Sign, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="5ae38-220">Du kan använda något annat Adobe logga användarens konto skapas verktyg eller API: er som tillhandahålls av Adobe logga tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="5ae38-220">You can use any other Adobe Sign user account creation tools or APIs provided by Adobe Sign tooprovision AAD user accounts.</span></span> 

<span data-ttu-id="5ae38-221">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="5ae38-221">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ae38-222">Logga in tooyour **Adobe logga** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="5ae38-222">Log in tooyour **Adobe Sign** company site as administrator.</span></span>

2. <span data-ttu-id="5ae38-223">Hello-menyn överst hello **konto**, och hello navigeringsfönstret hello vänster, klicka **användare och grupper**, och klicka sedan på **skapa en ny användare**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-223">In hello menu on hello top, click **Account**, and then, in hello navigation pane on hello left side, click **Users & Groups**, and then, click **Create a new user**.</span></span>
   
   <span data-ttu-id="5ae38-224">![Kontot](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "konto")</span><span class="sxs-lookup"><span data-stu-id="5ae38-224">![Account](./media/active-directory-saas-adobe-echosign-tutorial/ic789524.png "Account")</span></span>
   
3. <span data-ttu-id="5ae38-225">I hello **skapa nya användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5ae38-225">In hello **Create New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="5ae38-226">![Skapa användare](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="5ae38-226">![Create User](./media/active-directory-saas-adobe-echosign-tutorial/ic789525.png "Create User")</span></span>
   
   <span data-ttu-id="5ae38-227">a.</span><span class="sxs-lookup"><span data-stu-id="5ae38-227">a.</span></span> <span data-ttu-id="5ae38-228">Typen hello **e-postadress**, **Förnamn**, och **efternamn** av en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="5ae38-228">Type hello **Email Address**, **First Name**, and **Last Name** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
   <span data-ttu-id="5ae38-229">b.</span><span class="sxs-lookup"><span data-stu-id="5ae38-229">b.</span></span> <span data-ttu-id="5ae38-230">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-230">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="5ae38-231">hello Azure Active Directory användare får ett e-postmeddelande som innehåller en länk tooconfirm hello-konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5ae38-231">hello Azure Active Directory account holder receives an email that includes a link tooconfirm hello account before it becomes active.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5ae38-232">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5ae38-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5ae38-233">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAdobe logga.</span><span class="sxs-lookup"><span data-stu-id="5ae38-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAdobe Sign.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5ae38-235">**tooassign Britta Simon tooAdobe signera, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="5ae38-235">**tooassign Britta Simon tooAdobe Sign, perform hello following steps:**</span></span>

1. <span data-ttu-id="5ae38-236">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5ae38-238">Välj i listan med program hello **Adobe logga**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-238">In hello applications list, select **Adobe Sign**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. <span data-ttu-id="5ae38-240">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5ae38-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5ae38-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5ae38-242">Click **Add** button.</span></span> <span data-ttu-id="5ae38-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ae38-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5ae38-245">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5ae38-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5ae38-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ae38-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5ae38-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5ae38-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5ae38-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5ae38-248">Testing single sign-on</span></span>

<span data-ttu-id="5ae38-249">När du klickar på hello Adobe logga panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Adobe signera programmet.</span><span class="sxs-lookup"><span data-stu-id="5ae38-249">When you click hello Adobe Sign tile in hello Access Panel, you should get automatically signed-on tooyour Adobe Sign application.</span></span>
<span data-ttu-id="5ae38-250">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5ae38-250">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5ae38-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5ae38-251">Additional resources</span></span>

* [<span data-ttu-id="5ae38-252">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5ae38-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5ae38-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5ae38-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-echosign-tutorial/tutorial_general_203.png

