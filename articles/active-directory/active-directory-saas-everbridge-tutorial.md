---
title: "Självstudier: Azure Active Directory-integrering med EverBridge | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och EverBridge."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: a260298279407ed709bc2e685a104410f9836a74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a><span data-ttu-id="60090-103">Självstudier: Azure Active Directory-integrering med EverBridge</span><span class="sxs-lookup"><span data-stu-id="60090-103">Tutorial: Azure Active Directory integration with EverBridge</span></span>

<span data-ttu-id="60090-104">I kursen får du lära dig hur toointegrate EverBridge med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="60090-104">In this tutorial, you learn how toointegrate EverBridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="60090-105">Integrera EverBridge med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="60090-105">Integrating EverBridge with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="60090-106">Du kan styra i Azure AD som har åtkomst till tooEverBridge</span><span class="sxs-lookup"><span data-stu-id="60090-106">You can control in Azure AD who has access tooEverBridge</span></span>
- <span data-ttu-id="60090-107">Du kan aktivera din användare tooautomatically get inloggade tooEverBridge (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="60090-107">You can enable your users tooautomatically get signed-on tooEverBridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="60090-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="60090-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="60090-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="60090-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="60090-110">Krav</span><span class="sxs-lookup"><span data-stu-id="60090-110">Prerequisites</span></span>

<span data-ttu-id="60090-111">tooconfigure Azure AD-integrering med EverBridge, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="60090-111">tooconfigure Azure AD integration with EverBridge, you need hello following items:</span></span>

- <span data-ttu-id="60090-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="60090-112">An Azure AD subscription</span></span>
- <span data-ttu-id="60090-113">En EverBridge enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="60090-113">An EverBridge single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="60090-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="60090-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="60090-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="60090-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="60090-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="60090-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="60090-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="60090-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="60090-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="60090-118">Scenario description</span></span>
<span data-ttu-id="60090-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="60090-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="60090-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="60090-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="60090-121">Att lägga till EverBridge från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="60090-121">Adding EverBridge from hello gallery</span></span>
2. <span data-ttu-id="60090-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="60090-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-everbridge-from-hello-gallery"></a><span data-ttu-id="60090-123">Att lägga till EverBridge från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="60090-123">Adding EverBridge from hello gallery</span></span>
<span data-ttu-id="60090-124">tooconfigure hello integrering av EverBridge i Azure AD, behöver du tooadd EverBridge hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="60090-124">tooconfigure hello integration of EverBridge into Azure AD, you need tooadd EverBridge from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="60090-125">**tooadd EverBridge från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="60090-125">**tooadd EverBridge from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="60090-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="60090-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="60090-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="60090-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="60090-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="60090-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="60090-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="60090-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="60090-133">Skriv i sökrutan hello **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="60090-133">In hello search box, type **EverBridge**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_search.png)

5. <span data-ttu-id="60090-135">Markera hello resultat på panelen **EverBridge**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="60090-135">In hello results panel, select **EverBridge**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="60090-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="60090-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="60090-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med EverBridge baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="60090-138">In this section, you configure and test Azure AD single sign-on with EverBridge based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="60090-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i EverBridge är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="60090-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in EverBridge is tooa user in Azure AD.</span></span> <span data-ttu-id="60090-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i EverBridge toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="60090-140">In other words, a link relationship between an Azure AD user and hello related user in EverBridge needs toobe established.</span></span>

<span data-ttu-id="60090-141">I EverBridge, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="60090-141">In EverBridge, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="60090-142">tooconfigure och testa Azure AD enkel inloggning med EverBridge, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="60090-142">tooconfigure and test Azure AD single sign-on with EverBridge, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="60090-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="60090-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="60090-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60090-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="60090-145">**[Skapa en testanvändare EverBridge](#creating-an-everbridge-test-user)**  -toohave en motsvarighet för Britta Simon i EverBridge som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="60090-145">**[Creating an EverBridge test user](#creating-an-everbridge-test-user)** - toohave a counterpart of Britta Simon in EverBridge that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="60090-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="60090-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="60090-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="60090-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="60090-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="60090-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="60090-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt EverBridge program.</span><span class="sxs-lookup"><span data-stu-id="60090-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your EverBridge application.</span></span>

<span data-ttu-id="60090-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med EverBridge:**</span><span class="sxs-lookup"><span data-stu-id="60090-150">**tooconfigure Azure AD single sign-on with EverBridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="60090-151">I hello Azure-portalen på hello **EverBridge** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="60090-151">In hello Azure portal, on hello **EverBridge** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="60090-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="60090-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_samlbase.png)

3. <span data-ttu-id="60090-155">På hello **EverBridge domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="60090-155">On hello **EverBridge Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_url.png)

    <span data-ttu-id="60090-157">a.</span><span class="sxs-lookup"><span data-stu-id="60090-157">a.</span></span> <span data-ttu-id="60090-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://sso.everbridge.net/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="60090-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://sso.everbridge.net/<companyname>`</span></span>

    <span data-ttu-id="60090-159">b.</span><span class="sxs-lookup"><span data-stu-id="60090-159">b.</span></span> <span data-ttu-id="60090-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span><span class="sxs-lookup"><span data-stu-id="60090-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="60090-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="60090-161">These values are not real.</span></span> <span data-ttu-id="60090-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="60090-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="60090-163">Kontakta [EverBridge supportteamet](mailto:support@everbridge.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="60090-163">Contact [EverBridge support team](mailto:support@everbridge.com) tooget these values.</span></span>
 
4. <span data-ttu-id="60090-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="60090-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_certificate.png) 

5. <span data-ttu-id="60090-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="60090-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="60090-168">På hello **EverBridge Configuration** klickar du på **konfigurera EverBridge** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="60090-168">On hello **EverBridge Configuration** section, click **Configure EverBridge** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="60090-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="60090-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_configure.png) 

6. <span data-ttu-id="60090-171">tooget SSO konfigurerats för ditt program måste toosign på tooyour Everbridge klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="60090-171">tooget SSO configured for your application, you need toosign-on tooyour Everbridge tenant as an administrator.</span></span>

7. <span data-ttu-id="60090-172">Hello hello överst klickar du på menyn hello **inställningar** fliken och markera **enkel inloggning** under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="60090-172">In hello menu on hello top, click hello **Settings** tab and select **Single Sign-On** under **Security**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)
   
    <span data-ttu-id="60090-174">a.</span><span class="sxs-lookup"><span data-stu-id="60090-174">a.</span></span> <span data-ttu-id="60090-175">I hello **namn** textruta hello-typnamn för identifierare Provider (till exempel: företagets namn).</span><span class="sxs-lookup"><span data-stu-id="60090-175">In hello **Name** textbox, type hello name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="60090-176">b.</span><span class="sxs-lookup"><span data-stu-id="60090-176">b.</span></span> <span data-ttu-id="60090-177">I hello **API-namnet** textruta hello-typnamn för API: et.</span><span class="sxs-lookup"><span data-stu-id="60090-177">In hello **API Name** textbox, type hello name of API.</span></span>
   
    <span data-ttu-id="60090-178">c.</span><span class="sxs-lookup"><span data-stu-id="60090-178">c.</span></span> <span data-ttu-id="60090-179">Klicka på **Välj fil** knappen tooupload hello metadata-fil som du hämtade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="60090-179">Click **Choose File** button tooupload hello metadata file which you downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="60090-180">d.</span><span class="sxs-lookup"><span data-stu-id="60090-180">d.</span></span> <span data-ttu-id="60090-181">Markera i hello SAML identitet plats, **identitet är i hello NameIdentifier elementet i hello ämne instruktionen**.</span><span class="sxs-lookup"><span data-stu-id="60090-181">In hello SAML Identity Location, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>
   
    <span data-ttu-id="60090-182">e.</span><span class="sxs-lookup"><span data-stu-id="60090-182">e.</span></span> <span data-ttu-id="60090-183">I hello **identitet providern inloggnings-URL** textruta klistra in hello värde för SAML SSO-URL från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="60090-183">In hello **Identity Provider Login URL** textbox, paste hello value of SAML SSO URL from Azure AD.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)
   
    <span data-ttu-id="60090-185">f.</span><span class="sxs-lookup"><span data-stu-id="60090-185">f.</span></span> <span data-ttu-id="60090-186">I hello Service Provider initierade begära bindning, väljer **HTTP-omdirigering**.</span><span class="sxs-lookup"><span data-stu-id="60090-186">In hello Service Provider Initiated Request Binding, select **HTTP Redirect**.</span></span>

    <span data-ttu-id="60090-187">g.</span><span class="sxs-lookup"><span data-stu-id="60090-187">g.</span></span> <span data-ttu-id="60090-188">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="60090-188">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="60090-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="60090-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="60090-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="60090-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="60090-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="60090-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="60090-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="60090-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="60090-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="60090-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="60090-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="60090-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="60090-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="60090-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="60090-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="60090-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="60090-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="60090-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="60090-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="60090-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="60090-204">a.</span><span class="sxs-lookup"><span data-stu-id="60090-204">a.</span></span> <span data-ttu-id="60090-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="60090-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="60090-206">b.</span><span class="sxs-lookup"><span data-stu-id="60090-206">b.</span></span> <span data-ttu-id="60090-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="60090-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="60090-208">c.</span><span class="sxs-lookup"><span data-stu-id="60090-208">c.</span></span> <span data-ttu-id="60090-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="60090-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="60090-210">d.</span><span class="sxs-lookup"><span data-stu-id="60090-210">d.</span></span> <span data-ttu-id="60090-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="60090-211">Click **Create**.</span></span>
 
### <a name="creating-an-everbridge-test-user"></a><span data-ttu-id="60090-212">Skapa en testanvändare EverBridge</span><span class="sxs-lookup"><span data-stu-id="60090-212">Creating an EverBridge test user</span></span>

<span data-ttu-id="60090-213">I det här avsnittet skapar du en användare som kallas Britta Simon i Everbridge.</span><span class="sxs-lookup"><span data-stu-id="60090-213">In this section, you create a user called Britta Simon in Everbridge.</span></span> <span data-ttu-id="60090-214">Arbeta med [EverBridge supportteamet](mailto:support@everbridge.com) tooadd hello användare i hello Everbridge plattform.</span><span class="sxs-lookup"><span data-stu-id="60090-214">Work with [EverBridge support team](mailto:support@everbridge.com) tooadd hello users in hello Everbridge platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="60090-215">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="60090-215">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="60090-216">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEverBridge.</span><span class="sxs-lookup"><span data-stu-id="60090-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEverBridge.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="60090-218">**tooassign Britta Simon tooEverBridge utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="60090-218">**tooassign Britta Simon tooEverBridge, perform hello following steps:**</span></span>

1. <span data-ttu-id="60090-219">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="60090-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="60090-221">Välj i listan med program hello **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="60090-221">In hello applications list, select **EverBridge**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_app.png) 

3. <span data-ttu-id="60090-223">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="60090-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="60090-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="60090-225">Click **Add** button.</span></span> <span data-ttu-id="60090-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="60090-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="60090-228">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="60090-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="60090-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="60090-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="60090-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="60090-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="60090-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="60090-231">Testing single sign-on</span></span>

<span data-ttu-id="60090-232">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="60090-232">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="60090-233">Du bör få automatiskt inloggade tooyour Everbridge programmet när du klickar på hello Everbridge panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="60090-233">When you click hello Everbridge tile in hello Access Panel, you should get automatically signed-on tooyour Everbridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60090-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="60090-234">Additional resources</span></span>

* [<span data-ttu-id="60090-235">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="60090-235">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="60090-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="60090-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png

