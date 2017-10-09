---
title: "Självstudier: Azure Active Directory-integrering med identifiera | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och identifiera."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: f33fc3959f72f875b8c5c4f0abd4e9b6737ca615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a><span data-ttu-id="f37ce-103">Självstudier: Azure Active Directory-integrering med identifiera</span><span class="sxs-lookup"><span data-stu-id="f37ce-103">Tutorial: Azure Active Directory integration with Recognize</span></span>

<span data-ttu-id="f37ce-104">I kursen får du lära dig hur toointegrate känner igen med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f37ce-104">In this tutorial, you learn how toointegrate Recognize with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f37ce-105">Identifiera integrera med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f37ce-105">Integrating Recognize with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f37ce-106">Du kan styra i Azure AD som har åtkomst till tooRecognize</span><span class="sxs-lookup"><span data-stu-id="f37ce-106">You can control in Azure AD who has access tooRecognize</span></span>
- <span data-ttu-id="f37ce-107">Du kan aktivera din användare tooautomatically get inloggade tooRecognize (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f37ce-107">You can enable your users tooautomatically get signed-on tooRecognize (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f37ce-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f37ce-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f37ce-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f37ce-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f37ce-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f37ce-110">Prerequisites</span></span>

<span data-ttu-id="f37ce-111">tooconfigure Azure AD-integrering med identifiera, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f37ce-111">tooconfigure Azure AD integration with Recognize, you need hello following items:</span></span>

- <span data-ttu-id="f37ce-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f37ce-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f37ce-113">En identifiera enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="f37ce-113">A Recognize single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f37ce-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f37ce-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f37ce-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f37ce-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f37ce-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f37ce-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f37ce-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f37ce-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f37ce-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f37ce-118">Scenario description</span></span>
<span data-ttu-id="f37ce-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f37ce-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f37ce-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f37ce-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f37ce-121">Att lägga till identifiera från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f37ce-121">Adding Recognize from hello gallery</span></span>
2. <span data-ttu-id="f37ce-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f37ce-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-recognize-from-hello-gallery"></a><span data-ttu-id="f37ce-123">Att lägga till identifiera från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f37ce-123">Adding Recognize from hello gallery</span></span>
<span data-ttu-id="f37ce-124">tooconfigure hello integrering av identifiera i Azure AD, behöver du tooadd identifiera hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f37ce-124">tooconfigure hello integration of Recognize into Azure AD, you need tooadd Recognize from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f37ce-125">**tooadd identifiera från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f37ce-125">**tooadd Recognize from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f37ce-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f37ce-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f37ce-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f37ce-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f37ce-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f37ce-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f37ce-133">Skriv i sökrutan hello **identifiera**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-133">In hello search box, type **Recognize**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. <span data-ttu-id="f37ce-135">Markera hello resultat på panelen **identifiera**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f37ce-135">In hello results panel, select **Recognize**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f37ce-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f37ce-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f37ce-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med identifiera baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f37ce-138">In this section, you configure and test Azure AD single sign-on with Recognize based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f37ce-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i identifiera är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f37ce-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Recognize is tooa user in Azure AD.</span></span> <span data-ttu-id="f37ce-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i identifiera toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="f37ce-140">In other words, a link relationship between an Azure AD user and hello related user in Recognize needs toobe established.</span></span>

<span data-ttu-id="f37ce-141">I identifiera, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-141">In Recognize, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f37ce-142">tooconfigure och testa Azure AD enkel inloggning med identifiera, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f37ce-142">tooconfigure and test Azure AD single sign-on with Recognize, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f37ce-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f37ce-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f37ce-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f37ce-145">**[Skapa en testanvändare identifiera](#creating-a-recognize-test-user)**  -toohave en motsvarighet för Britta Simon i identifiera som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f37ce-145">**[Creating a Recognize test user](#creating-a-recognize-test-user)** - toohave a counterpart of Britta Simon in Recognize that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f37ce-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f37ce-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f37ce-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f37ce-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f37ce-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f37ce-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f37ce-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i identifiera programmet.</span><span class="sxs-lookup"><span data-stu-id="f37ce-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Recognize application.</span></span>

<span data-ttu-id="f37ce-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med identifiera:**</span><span class="sxs-lookup"><span data-stu-id="f37ce-150">**tooconfigure Azure AD single sign-on with Recognize, perform hello following steps:**</span></span>

1. <span data-ttu-id="f37ce-151">I hello Azure-portalen på hello **identifiera** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-151">In hello Azure portal, on hello **Recognize** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f37ce-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f37ce-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. <span data-ttu-id="f37ce-155">På hello **URL: er och identifiera domänen** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f37ce-155">On hello **Recognize Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    <span data-ttu-id="f37ce-157">a.</span><span class="sxs-lookup"><span data-stu-id="f37ce-157">a.</span></span> <span data-ttu-id="f37ce-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://recognizeapp.com/<your-domain>/saml/sso`</span><span class="sxs-lookup"><span data-stu-id="f37ce-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://recognizeapp.com/<your-domain>/saml/sso`</span></span>

    <span data-ttu-id="f37ce-159">b.</span><span class="sxs-lookup"><span data-stu-id="f37ce-159">b.</span></span> <span data-ttu-id="f37ce-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://recognizeapp.com/<your-domain>`</span><span class="sxs-lookup"><span data-stu-id="f37ce-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://recognizeapp.com/<your-domain>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f37ce-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f37ce-161">These values are not real.</span></span> <span data-ttu-id="f37ce-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="f37ce-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f37ce-163">Kontakta [identifiera klienten supportteamet](mailto:support@recognizeapp.com) få inloggnings-URL och du kan hämta ID-värde genom att öppna hello URL för tjänstmetadata providern från hello SSO inställningar som beskrivs senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="f37ce-163">Contact [Recognize Client support team](mailto:support@recognizeapp.com) to get Sign-On URL and you can get Identifier value by opening hello Service Provider Metadata URL from hello SSO Settings section that is explained later in hello tutorial.</span></span> <span data-ttu-id="f37ce-164">.</span><span class="sxs-lookup"><span data-stu-id="f37ce-164">.</span></span> 
 
4. <span data-ttu-id="f37ce-165">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="f37ce-165">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. <span data-ttu-id="f37ce-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f37ce-169">På hello **identifiera Configuration** klickar du på **konfigurera identifiera** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f37ce-169">On hello **Recognize Configuration** section, click **Configure Recognize** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f37ce-170">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f37ce-170">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. <span data-ttu-id="f37ce-172">I en annan webbläsarfönstret, inloggning tooyour identifiera klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="f37ce-172">In a different web browser window, sign-on tooyour Recognize tenant as an administrator.</span></span>

8. <span data-ttu-id="f37ce-173">Klicka på hello övre högra hörnet **menyn**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-173">On hello upper right corner, click **Menu**.</span></span> <span data-ttu-id="f37ce-174">Gå för**företagsadministratör**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-174">Go too**Company Admin**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. <span data-ttu-id="f37ce-176">Klicka på hello vänstra navigeringsfönstret **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-176">On hello left navigation pane, click **Settings**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. <span data-ttu-id="f37ce-178">Utför följande steg hello **SSO-inställningarna** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f37ce-178">Perform hello following steps on **SSO Settings** section.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    <span data-ttu-id="f37ce-180">a.</span><span class="sxs-lookup"><span data-stu-id="f37ce-180">a.</span></span> <span data-ttu-id="f37ce-181">Som **aktivera enkel inloggning**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-181">As **Enable SSO**, select **ON**.</span></span>

    <span data-ttu-id="f37ce-182">b.</span><span class="sxs-lookup"><span data-stu-id="f37ce-182">b.</span></span> <span data-ttu-id="f37ce-183">I hello **IDP enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-183">In hello **IDP Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="f37ce-184">c.</span><span class="sxs-lookup"><span data-stu-id="f37ce-184">c.</span></span> <span data-ttu-id="f37ce-185">I hello **mål-url för Sso** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-185">In hello **Sso target url** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="f37ce-186">d.</span><span class="sxs-lookup"><span data-stu-id="f37ce-186">d.</span></span> <span data-ttu-id="f37ce-187">I hello **servicenivåmål mål-url** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-187">In hello **Slo target url** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span> 
    
    <span data-ttu-id="f37ce-188">e.</span><span class="sxs-lookup"><span data-stu-id="f37ce-188">e.</span></span> <span data-ttu-id="f37ce-189">Öppna din hämtade **certifikat (Base64)** filen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="f37ce-189">Open your downloaded **Certificate (Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Certificate** textbox.</span></span>
    
    <span data-ttu-id="f37ce-190">f.</span><span class="sxs-lookup"><span data-stu-id="f37ce-190">f.</span></span> <span data-ttu-id="f37ce-191">Klicka på hello **Spara inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-191">Click hello **Save settings** button.</span></span> 

11. <span data-ttu-id="f37ce-192">Bredvid hello **SSO inställningar** avsnittet, kopiera hello URL under **url för tjänstmetadata providern**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-192">Beside hello **SSO Settings** section, copy hello URL under **Service Provider Metadata url**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. <span data-ttu-id="f37ce-194">Öppna hello **Metadata URL-länk** under en tom webbläsare toodownload hello metadata dokument.</span><span class="sxs-lookup"><span data-stu-id="f37ce-194">Open hello **Metadata URL link** under a blank browser toodownload hello metadata document.</span></span> <span data-ttu-id="f37ce-195">Kopiera hello EntityDescriptor value(entityID) från hello-filen och klistra in den i **identifierare** TextBox-kontroll i **URL: er och identifiera domänen avsnittet** på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-195">Then copy hello EntityDescriptor value(entityID) from hello file and paste it in **Identifier** textbox in **Recognize Domain and URLs section** on Azure portal.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> <span data-ttu-id="f37ce-197">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f37ce-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f37ce-198">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f37ce-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f37ce-199">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f37ce-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f37ce-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f37ce-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="f37ce-201">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f37ce-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f37ce-203">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f37ce-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f37ce-204">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f37ce-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f37ce-206">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f37ce-208">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f37ce-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f37ce-210">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f37ce-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f37ce-212">a.</span><span class="sxs-lookup"><span data-stu-id="f37ce-212">a.</span></span> <span data-ttu-id="f37ce-213">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f37ce-214">b.</span><span class="sxs-lookup"><span data-stu-id="f37ce-214">b.</span></span> <span data-ttu-id="f37ce-215">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f37ce-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f37ce-216">c.</span><span class="sxs-lookup"><span data-stu-id="f37ce-216">c.</span></span> <span data-ttu-id="f37ce-217">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f37ce-218">d.</span><span class="sxs-lookup"><span data-stu-id="f37ce-218">d.</span></span> <span data-ttu-id="f37ce-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-219">Click **Create**.</span></span>
 
### <a name="creating-a-recognize-test-user"></a><span data-ttu-id="f37ce-220">Skapa en testanvändare identifiera</span><span class="sxs-lookup"><span data-stu-id="f37ce-220">Creating a Recognize test user</span></span>

<span data-ttu-id="f37ce-221">I ordning tooenable Azure AD-användare toolog till identifiera, måste de etableras i identifiera.</span><span class="sxs-lookup"><span data-stu-id="f37ce-221">In order tooenable Azure AD users toolog into Recognize, they must be provisioned into Recognize.</span></span> <span data-ttu-id="f37ce-222">Identifiera hello gäller är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="f37ce-222">In hello case of Recognize, provisioning is a manual task.</span></span>

<span data-ttu-id="f37ce-223">Den här appen stöder inte SCIM etablering men har en annan användare sync som etablerar användare.</span><span class="sxs-lookup"><span data-stu-id="f37ce-223">This app doesn't support SCIM provisioning but has an alternate user sync that provisions users.</span></span> 

<span data-ttu-id="f37ce-224">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="f37ce-224">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="f37ce-225">Logga in på webbplatsen identifiera företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="f37ce-225">Log into your Recognize company site as an administrator.</span></span>

2. <span data-ttu-id="f37ce-226">Klicka på hello övre högra hörnet **menyn**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-226">On hello upper right corner, click **Menu**.</span></span> <span data-ttu-id="f37ce-227">Gå för**företagsadministratör**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-227">Go too**Company Admin**.</span></span>

3. <span data-ttu-id="f37ce-228">Klicka på hello vänstra navigeringsfönstret **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-228">On hello left navigation pane, click **Settings**.</span></span>

4. <span data-ttu-id="f37ce-229">Utför följande steg hello **användaren Sync** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f37ce-229">Perform hello following steps on **User Sync** section.</span></span>
   
   <span data-ttu-id="f37ce-230">![Ny användare](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="f37ce-230">![New User](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "New User")</span></span>
   
   <span data-ttu-id="f37ce-231">a.</span><span class="sxs-lookup"><span data-stu-id="f37ce-231">a.</span></span> <span data-ttu-id="f37ce-232">Som **synkronisering aktiverat**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-232">As **Sync Enabled**, select **ON**.</span></span>
   
   <span data-ttu-id="f37ce-233">b.</span><span class="sxs-lookup"><span data-stu-id="f37ce-233">b.</span></span> <span data-ttu-id="f37ce-234">Som **Välj sync providern**väljer **Microsoft / Office 365**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-234">As **Choose sync provider**, select **Microsoft / Office 365**.</span></span>
   
   <span data-ttu-id="f37ce-235">c.</span><span class="sxs-lookup"><span data-stu-id="f37ce-235">c.</span></span> <span data-ttu-id="f37ce-236">Klicka på **köra användaren synkronisering**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-236">Click **Run User Sync**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f37ce-237">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f37ce-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f37ce-238">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooRecognize.</span><span class="sxs-lookup"><span data-stu-id="f37ce-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRecognize.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f37ce-240">**tooassign Britta Simon tooRecognize utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f37ce-240">**tooassign Britta Simon tooRecognize, perform hello following steps:**</span></span>

1. <span data-ttu-id="f37ce-241">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f37ce-243">Välj i listan med program hello **identifiera**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-243">In hello applications list, select **Recognize**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. <span data-ttu-id="f37ce-245">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f37ce-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f37ce-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-247">Click **Add** button.</span></span> <span data-ttu-id="f37ce-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f37ce-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f37ce-250">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f37ce-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f37ce-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f37ce-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f37ce-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f37ce-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f37ce-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f37ce-253">Testing single sign-on</span></span>

<span data-ttu-id="f37ce-254">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-254">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f37ce-255">Du bör få automatiskt inloggade tooyour identifiera programmet när du klickar på hello identifiera panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f37ce-255">When you click hello Recognize tile in hello Access Panel, you should get automatically signed-on tooyour Recognize application.</span></span> <span data-ttu-id="f37ce-256">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f37ce-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f37ce-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f37ce-257">Additional resources</span></span>

* [<span data-ttu-id="f37ce-258">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f37ce-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f37ce-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f37ce-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png

