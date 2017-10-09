---
title: "Självstudier: Azure Active Directory-integrering med Adobe kreativa molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Adobe kreativa moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5e66255e9785465974a23cd3ef79c24e28c0250f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a><span data-ttu-id="84e53-103">Självstudier: Azure Active Directory-integrering med Adobe kreativa moln</span><span class="sxs-lookup"><span data-stu-id="84e53-103">Tutorial: Azure Active Directory integration with Adobe Creative Cloud</span></span>

<span data-ttu-id="84e53-104">I kursen får du lära dig hur toointegrate Adobe kreativa moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="84e53-104">In this tutorial, you learn how toointegrate Adobe Creative Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84e53-105">Integrera Adobe kreativa moln med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="84e53-105">Integrating Adobe Creative Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="84e53-106">Du kan styra i Azure AD som har åtkomst tooAdobe kreativa moln</span><span class="sxs-lookup"><span data-stu-id="84e53-106">You can control in Azure AD who has access tooAdobe Creative Cloud</span></span>
- <span data-ttu-id="84e53-107">Du kan aktivera din användare tooautomatically get inloggade tooAdobe kreativa molnet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="84e53-107">You can enable your users tooautomatically get signed-on tooAdobe Creative Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84e53-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="84e53-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="84e53-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84e53-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84e53-110">Krav</span><span class="sxs-lookup"><span data-stu-id="84e53-110">Prerequisites</span></span>

<span data-ttu-id="84e53-111">tooconfigure Azure AD-integrering med Adobe kreativa molnet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="84e53-111">tooconfigure Azure AD integration with Adobe Creative Cloud, you need hello following items:</span></span>

- <span data-ttu-id="84e53-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="84e53-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84e53-113">En Adobe kreativa molnet enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="84e53-113">A Adobe Creative Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84e53-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="84e53-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84e53-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="84e53-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84e53-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="84e53-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="84e53-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84e53-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84e53-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="84e53-118">Scenario description</span></span>
<span data-ttu-id="84e53-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="84e53-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84e53-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="84e53-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84e53-121">Lägga till Adobe kreativa moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="84e53-121">Adding Adobe Creative Cloud from hello gallery</span></span>
2. <span data-ttu-id="84e53-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e53-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adobe-creative-cloud-from-hello-gallery"></a><span data-ttu-id="84e53-123">Lägga till Adobe kreativa moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="84e53-123">Adding Adobe Creative Cloud from hello gallery</span></span>
<span data-ttu-id="84e53-124">tooconfigure hello integrering av Adobe kreativa moln i Azure AD, behöver du tooadd Adobe kreativa molnet hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="84e53-124">tooconfigure hello integration of Adobe Creative Cloud into Azure AD, you need tooadd Adobe Creative Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="84e53-125">**tooadd Adobe kreativa moln från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84e53-125">**tooadd Adobe Creative Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e53-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84e53-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84e53-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="84e53-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="84e53-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="84e53-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="84e53-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e53-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="84e53-133">Skriv i sökrutan hello **Adobe kreativa molnet**.</span><span class="sxs-lookup"><span data-stu-id="84e53-133">In hello search box, type **Adobe Creative Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. <span data-ttu-id="84e53-135">Markera hello resultat på panelen **Adobe kreativa molnet**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="84e53-135">In hello results panel, select **Adobe Creative Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84e53-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e53-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84e53-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Adobe kreativa molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="84e53-138">In this section, you configure and test Azure AD single sign-on with Adobe Creative Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="84e53-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Adobe kreativa molnet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="84e53-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adobe Creative Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="84e53-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Adobe kreativa molnet toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="84e53-140">In other words, a link relationship between an Azure AD user and hello related user in Adobe Creative Cloud needs toobe established.</span></span>

<span data-ttu-id="84e53-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Adobe kreativa molnet.</span><span class="sxs-lookup"><span data-stu-id="84e53-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Adobe Creative Cloud.</span></span>

<span data-ttu-id="84e53-142">tooconfigure och testa Azure AD enkel inloggning med Adobe kreativa molnet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="84e53-142">tooconfigure and test Azure AD single sign-on with Adobe Creative Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="84e53-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="84e53-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="84e53-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84e53-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84e53-145">**[Skapa en testanvändare Adobe kreativa molnet](#creating-an-adobe-creative-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Adobe kreativa moln som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="84e53-145">**[Creating an Adobe Creative Cloud test user](#creating-an-adobe-creative-cloud-test-user)** - toohave a counterpart of Britta Simon in Adobe Creative Cloud that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="84e53-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84e53-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84e53-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="84e53-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84e53-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e53-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84e53-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i tillämpningsprogrammet Adobe kreativa molnet.</span><span class="sxs-lookup"><span data-stu-id="84e53-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Adobe Creative Cloud application.</span></span>

<span data-ttu-id="84e53-150">**tooconfigure Azure AD enkel inloggning med Adobe kreativa moln, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="84e53-150">**tooconfigure Azure AD single sign-on with Adobe Creative Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e53-151">I hello Azure Management portal på hello **Adobe kreativa molnet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="84e53-151">In hello Azure Management portal, on hello **Adobe Creative Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="84e53-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84e53-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. <span data-ttu-id="84e53-155">På hello **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="84e53-155">On hello **Adobe Creative Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    <span data-ttu-id="84e53-157">a.</span><span class="sxs-lookup"><span data-stu-id="84e53-157">a.</span></span> <span data-ttu-id="84e53-158">I hello **identifierare** textruta hello TYPVÄRDE som:`https://www.okta.com/saml2/service-provider/<token>`</span><span class="sxs-lookup"><span data-stu-id="84e53-158">In hello **Identifier** textbox, type hello value as: `https://www.okta.com/saml2/service-provider/<token>`</span></span>

    <span data-ttu-id="84e53-159">b.</span><span class="sxs-lookup"><span data-stu-id="84e53-159">b.</span></span> <span data-ttu-id="84e53-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.okta.com/auth/saml20/accauthlinktest`</span><span class="sxs-lookup"><span data-stu-id="84e53-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.okta.com/auth/saml20/accauthlinktest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="84e53-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="84e53-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="84e53-162">Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="84e53-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="84e53-163">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="84e53-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="84e53-164">Om du behöver toocreate en användare manuellt, måste toocontact hello Adobe kreativa molnet supportteamet.</span><span class="sxs-lookup"><span data-stu-id="84e53-164">If you need toocreate an user manually, you need toocontact hello Adobe Creative Cloud support team.</span></span>

4. <span data-ttu-id="84e53-165">På hello **Adobe kreativa molnet domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="84e53-165">On hello **Adobe Creative Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    <span data-ttu-id="84e53-167">a.</span><span class="sxs-lookup"><span data-stu-id="84e53-167">a.</span></span> <span data-ttu-id="84e53-168">Klicka på hello **visa avancerade inställningar för URL: en** alternativet</span><span class="sxs-lookup"><span data-stu-id="84e53-168">Click on hello **Show advanced URL settings** option</span></span>

    <span data-ttu-id="84e53-169">b.</span><span class="sxs-lookup"><span data-stu-id="84e53-169">b.</span></span> <span data-ttu-id="84e53-170">I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`https://adobe.com`</span><span class="sxs-lookup"><span data-stu-id="84e53-170">In hello **Sign-on URL** textbox, type hello value as: `https://adobe.com`</span></span>

5. <span data-ttu-id="84e53-171">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="84e53-171">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. <span data-ttu-id="84e53-173">På hello **Adobe kreativa Molnkonfigurationen** klickar du på **Konfigurera Adobe kreativa molnet** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="84e53-173">On hello **Adobe Creative Cloud Configuration** section, click **Configure Adobe Creative Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="84e53-174">Kopiera hello **SAML enhets-Id** och **URL för SAML SSO-Service** från Snabbreferens avsnitt.</span><span class="sxs-lookup"><span data-stu-id="84e53-174">Please copy hello **SAML Entity Id** and **SAML SSO Service URL** from Quick Reference section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. <span data-ttu-id="84e53-176">I en annan webbläsarfönstret, inloggning tooyour Adobe kreativa molnet klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="84e53-176">In a different web browser window, sign-on tooyour Adobe Creative Cloud tenant as an administrator.</span></span>

8.  <span data-ttu-id="84e53-177">Gå för**identitet** hello vänstra navigeringsfönstret och klicka på din domän.</span><span class="sxs-lookup"><span data-stu-id="84e53-177">Go too**Identity** on hello left navigation pane and click your domain.</span></span> <span data-ttu-id="84e53-178">Utför följande steg hello **enkel inloggning på konfiguration krävs** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="84e53-178">Then perform hello following steps on **Single Sign On Configuration Required** section.</span></span>

    <span data-ttu-id="84e53-179">![Inställningar för](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="84e53-179">![Settings](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Settings")</span></span>

9. <span data-ttu-id="84e53-180">Klicka på **Bläddra** tooupload hello hämtat certifikat från Azure AD för**IDP certifikat**.</span><span class="sxs-lookup"><span data-stu-id="84e53-180">Click **Browse** tooupload hello downloaded certificate from Azure AD too**IDP Certificate**.</span></span>

10. <span data-ttu-id="84e53-181">I hello **IDP utfärdaren** textruta placera hello värdet för **SAML enhets-Id** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="84e53-181">In hello **IDP issuer** textbox, put hello value of **SAML Entity Id** which you copied from **Configure sign-on** section in Azure portal.</span></span>

11. <span data-ttu-id="84e53-182">I hello **IDP inloggnings-URL** textruta placera hello värdet för **URL för SAML SSO-Service** som du kopierade från **konfigurera inloggning** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="84e53-182">In hello **IDP Login URL** textbox, put hello value of **SAML SSO Service URL** which you copied from **Configure sign-on** section in Azure portal.</span></span>

12. <span data-ttu-id="84e53-183">Välj **HTTP - omdirigering** som **IDP bindning**.</span><span class="sxs-lookup"><span data-stu-id="84e53-183">Select **HTTP - Redirect** as **IDP Binding**.</span></span>

13. <span data-ttu-id="84e53-184">Välj **e-postadress** som **inloggningen Användarinställning**.</span><span class="sxs-lookup"><span data-stu-id="84e53-184">Select **Email Address** as **User Login Setting**.</span></span>
 
14. <span data-ttu-id="84e53-185">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="84e53-185">Click **Save** button.</span></span>

15. <span data-ttu-id="84e53-186">hello instrumentpanelen visas nu hello XML **”hämta Metadata”** filen.</span><span class="sxs-lookup"><span data-stu-id="84e53-186">hello dashboard will now present hello XML **"Download Metadata"** file.</span></span> <span data-ttu-id="84e53-187">Den innehåller Adobe EntityDescriptor Webbadressen och AssertionConsumerService.</span><span class="sxs-lookup"><span data-stu-id="84e53-187">It contains Adobe’s EntityDescriptor URL and AssertionConsumerService URL.</span></span> <span data-ttu-id="84e53-188">Öppna hello-filen och konfigurera dem i hello Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="84e53-188">Please open hello file and configure them in hello Azure AD application.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    <span data-ttu-id="84e53-191">a.</span><span class="sxs-lookup"><span data-stu-id="84e53-191">a.</span></span> <span data-ttu-id="84e53-192">Använd hello EntityDescriptor värdet Adobe tillhandahålls för **identifierare** på hello **konfigurera Appinställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e53-192">Use hello EntityDescriptor value Adobe provided you for **Identifier** on hello **Configure App Settings** dialog.</span></span>

    <span data-ttu-id="84e53-193">b.</span><span class="sxs-lookup"><span data-stu-id="84e53-193">b.</span></span> <span data-ttu-id="84e53-194">Använd hello AssertionConsumerService värdet Adobe tillhandahålls för **Reply URL** på hello **konfigurera Appinställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e53-194">Use hello AssertionConsumerService value Adobe provided you for **Reply URL** on hello **Configure App Settings** dialog.</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84e53-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="84e53-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="84e53-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84e53-196">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="84e53-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84e53-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e53-199">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84e53-199">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84e53-201">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="84e53-201">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84e53-203">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e53-203">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84e53-205">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="84e53-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84e53-207">a.</span><span class="sxs-lookup"><span data-stu-id="84e53-207">a.</span></span> <span data-ttu-id="84e53-208">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84e53-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84e53-209">b.</span><span class="sxs-lookup"><span data-stu-id="84e53-209">b.</span></span> <span data-ttu-id="84e53-210">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84e53-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84e53-211">c.</span><span class="sxs-lookup"><span data-stu-id="84e53-211">c.</span></span> <span data-ttu-id="84e53-212">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="84e53-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="84e53-213">d.</span><span class="sxs-lookup"><span data-stu-id="84e53-213">d.</span></span> <span data-ttu-id="84e53-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="84e53-214">Click **Create**.</span></span> 

### <a name="creating-an-adobe-creative-cloud-test-user"></a><span data-ttu-id="84e53-215">Skapa en testanvändare Adobe kreativa moln</span><span class="sxs-lookup"><span data-stu-id="84e53-215">Creating an Adobe Creative Cloud test user</span></span>

<span data-ttu-id="84e53-216">I ordning tooenable Azure AD-användare toolog i Adobe kreativa moln, måste de etableras i Adobe kreativa moln.</span><span class="sxs-lookup"><span data-stu-id="84e53-216">In order tooenable Azure AD users toolog into Adobe Creative Cloud, they must be provisioned into Adobe Creative Cloud.</span></span>  
<span data-ttu-id="84e53-217">I hello fallet med Adobe kreativa moln är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="84e53-217">In hello case of Adobe Creative Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="84e53-218">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84e53-218">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e53-219">Logga in tooyour Adobe kreativa molnet företagets platsen som en administratör.</span><span class="sxs-lookup"><span data-stu-id="84e53-219">Log in tooyour Adobe Creative Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="84e53-220">Klicka på **personer**.</span><span class="sxs-lookup"><span data-stu-id="84e53-220">Click **People**.</span></span>

    <span data-ttu-id="84e53-221">![Personer](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "personer")</span><span class="sxs-lookup"><span data-stu-id="84e53-221">![People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "People")</span></span>

3. <span data-ttu-id="84e53-222">Klicka på **bjuda in användare**.</span><span class="sxs-lookup"><span data-stu-id="84e53-222">Click **Invite User**.</span></span>

    <span data-ttu-id="84e53-223">![Bjuda in användare](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "bjuda in användare")</span><span class="sxs-lookup"><span data-stu-id="84e53-223">![Invite Users](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "Invite Users")</span></span>

4. <span data-ttu-id="84e53-224">På hello **bjuda in personer** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="84e53-224">On hello **Invite People** dialog page, perform hello following steps:</span></span>

    <span data-ttu-id="84e53-225">![Bjuda in](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="84e53-225">![Invite People](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "Invite People")</span></span>

    <span data-ttu-id="84e53-226">a.</span><span class="sxs-lookup"><span data-stu-id="84e53-226">a.</span></span> <span data-ttu-id="84e53-227">I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.</span><span class="sxs-lookup"><span data-stu-id="84e53-227">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>
    
    <span data-ttu-id="84e53-228">b.</span><span class="sxs-lookup"><span data-stu-id="84e53-228">b.</span></span> <span data-ttu-id="84e53-229">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="84e53-229">Click **Invite**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="84e53-230">hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="84e53-230">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="84e53-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="84e53-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="84e53-232">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att ge sina access tooAdobe kreativa molnet.</span><span class="sxs-lookup"><span data-stu-id="84e53-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooAdobe Creative Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="84e53-234">**tooassign Britta Simon tooAdobe kreativa moln, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84e53-234">**tooassign Britta Simon tooAdobe Creative Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e53-235">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="84e53-235">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="84e53-237">Välj i listan med program hello **Adobe kreativa molnet**.</span><span class="sxs-lookup"><span data-stu-id="84e53-237">In hello applications list, select **Adobe Creative Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. <span data-ttu-id="84e53-239">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="84e53-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="84e53-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="84e53-241">Click **Add** button.</span></span> <span data-ttu-id="84e53-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e53-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="84e53-244">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="84e53-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="84e53-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e53-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84e53-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e53-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="84e53-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e53-247">Testing single sign-on</span></span>

<span data-ttu-id="84e53-248">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="84e53-248">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="84e53-249">När du klickar på hello Adobe kreativa molnet panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Adobe kreativa molnapp.</span><span class="sxs-lookup"><span data-stu-id="84e53-249">When you click hello Adobe Creative Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Adobe Creative Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="84e53-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="84e53-250">Additional resources</span></span>

* [<span data-ttu-id="84e53-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="84e53-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84e53-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="84e53-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png