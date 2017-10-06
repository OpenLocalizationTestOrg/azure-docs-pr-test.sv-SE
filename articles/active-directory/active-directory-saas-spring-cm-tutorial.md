---
title: "Självstudier: Azure Active Directory-integrering med SpringCM | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SpringCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 12c8ebe765e2c6e61115256e9343d90ec132e1f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a><span data-ttu-id="da050-103">Självstudier: Azure Active Directory-integrering med SpringCM</span><span class="sxs-lookup"><span data-stu-id="da050-103">Tutorial: Azure Active Directory integration with SpringCM</span></span>

<span data-ttu-id="da050-104">I kursen får du lära dig hur toointegrate SpringCM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="da050-104">In this tutorial, you learn how toointegrate SpringCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="da050-105">Integrera SpringCM med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="da050-105">Integrating SpringCM with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="da050-106">Du kan styra i Azure AD som har åtkomst till tooSpringCM</span><span class="sxs-lookup"><span data-stu-id="da050-106">You can control in Azure AD who has access tooSpringCM</span></span>
- <span data-ttu-id="da050-107">Du kan aktivera din användare tooautomatically get inloggade tooSpringCM (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="da050-107">You can enable your users tooautomatically get signed-on tooSpringCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="da050-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="da050-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="da050-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="da050-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da050-110">Krav</span><span class="sxs-lookup"><span data-stu-id="da050-110">Prerequisites</span></span>

<span data-ttu-id="da050-111">tooconfigure Azure AD-integrering med SpringCM, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="da050-111">tooconfigure Azure AD integration with SpringCM, you need hello following items:</span></span>

- <span data-ttu-id="da050-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="da050-112">An Azure AD subscription</span></span>
- <span data-ttu-id="da050-113">En SpringCM enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="da050-113">A SpringCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="da050-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="da050-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="da050-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="da050-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="da050-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="da050-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="da050-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="da050-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="da050-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="da050-118">Scenario description</span></span>
<span data-ttu-id="da050-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="da050-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="da050-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="da050-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="da050-121">Att lägga till SpringCM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="da050-121">Adding SpringCM from hello gallery</span></span>
2. <span data-ttu-id="da050-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="da050-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-springcm-from-hello-gallery"></a><span data-ttu-id="da050-123">Att lägga till SpringCM från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="da050-123">Adding SpringCM from hello gallery</span></span>
<span data-ttu-id="da050-124">tooconfigure hello integrering av SpringCM i Azure AD, behöver du tooadd SpringCM hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="da050-124">tooconfigure hello integration of SpringCM into Azure AD, you need tooadd SpringCM from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="da050-125">**tooadd SpringCM från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="da050-125">**tooadd SpringCM from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="da050-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="da050-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="da050-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="da050-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="da050-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="da050-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="da050-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="da050-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="da050-133">Skriv i sökrutan hello **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="da050-133">In hello search box, type **SpringCM**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. <span data-ttu-id="da050-135">Markera hello resultat på panelen **SpringCM**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="da050-135">In hello results panel, select **SpringCM**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="da050-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="da050-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="da050-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SpringCM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="da050-138">In this section, you configure and test Azure AD single sign-on with SpringCM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="da050-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SpringCM är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="da050-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SpringCM is tooa user in Azure AD.</span></span> <span data-ttu-id="da050-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SpringCM toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="da050-140">In other words, a link relationship between an Azure AD user and hello related user in SpringCM needs toobe established.</span></span>

<span data-ttu-id="da050-141">I SpringCM, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="da050-141">In SpringCM, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="da050-142">tooconfigure och testa Azure AD enkel inloggning med SpringCM, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="da050-142">tooconfigure and test Azure AD single sign-on with SpringCM, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="da050-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="da050-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="da050-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="da050-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="da050-145">**[Skapa en testanvändare SpringCM](#creating-a-springcm-test-user)**  -toohave en motsvarighet för Britta Simon i SpringCM som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="da050-145">**[Creating a SpringCM test user](#creating-a-springcm-test-user)** - toohave a counterpart of Britta Simon in SpringCM that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="da050-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="da050-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="da050-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="da050-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="da050-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="da050-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="da050-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt SpringCM program.</span><span class="sxs-lookup"><span data-stu-id="da050-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SpringCM application.</span></span>

<span data-ttu-id="da050-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SpringCM:**</span><span class="sxs-lookup"><span data-stu-id="da050-150">**tooconfigure Azure AD single sign-on with SpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="da050-151">I hello Azure-portalen på hello **SpringCM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="da050-151">In hello Azure portal, on hello **SpringCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="da050-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="da050-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. <span data-ttu-id="da050-155">På hello **SpringCM domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="da050-155">On hello **SpringCM Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    <span data-ttu-id="da050-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="da050-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="da050-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="da050-158">This value is not real.</span></span> <span data-ttu-id="da050-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="da050-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="da050-160">Kontakta [SpringCM klienten supportteamet](https://knowledge.springcm.com/support) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="da050-160">Contact [SpringCM Client support team](https://knowledge.springcm.com/support) tooget this value.</span></span> 
 
4. <span data-ttu-id="da050-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="da050-161">On hello **SAML Signing Certificate** section, click **Certificate(Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. <span data-ttu-id="da050-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="da050-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="da050-165">På hello **SpringCM Configuration** klickar du på **konfigurera SpringCM** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="da050-165">On hello **SpringCM Configuration** section, click **Configure SpringCM** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="da050-166">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="da050-166">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. <span data-ttu-id="da050-168">Inloggning i en annan webbläsarfönster tooyour **SpringCM** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="da050-168">In a different web browser window, sign on tooyour **SpringCM** company site as administrator.</span></span>

8. <span data-ttu-id="da050-169">Hello-menyn överst hello **gå till**, klickar du på **inställningar**, och sedan, i hello **kontoinställningar** klickar du på **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="da050-169">In hello menu on hello top, click **GO TO**, click **Preferences**, and then, in hello **Account Preferences** section, click **SAML SSO**.</span></span>
   
    <span data-ttu-id="da050-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span><span class="sxs-lookup"><span data-stu-id="da050-170">![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")</span></span>

9. <span data-ttu-id="da050-171">I hello identitet providern konfigurationsavsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="da050-171">In hello Identity Provider Configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="da050-172">![Identitet providerkonfigurationen](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "identitet providerkonfigurationen")</span><span class="sxs-lookup"><span data-stu-id="da050-172">![Identity Provider Configuration](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "Identity Provider Configuration")</span></span>
    
    <span data-ttu-id="da050-173">a.</span><span class="sxs-lookup"><span data-stu-id="da050-173">a.</span></span> <span data-ttu-id="da050-174">tooupload ditt hämtade Azure Active Directory-certifikat klickar du på **Välj utfärdarcertifikatet** eller **ändra utfärdarcertifikatet**.</span><span class="sxs-lookup"><span data-stu-id="da050-174">tooupload your downloaded Azure Active Directory certificate, click **Select Issuer Certificate** or **Change Issuer Certificate**.</span></span>
    
    <span data-ttu-id="da050-175">b.</span><span class="sxs-lookup"><span data-stu-id="da050-175">b.</span></span> <span data-ttu-id="da050-176">Klistra in **SAML enhets-ID** -värde som du har kopierat från Azure-portalen i hello **utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="da050-176">Paste **SAML Entity ID** value, which you have copied from Azure portal into hello **Issuer** textbox.</span></span>
    
    <span data-ttu-id="da050-177">c.</span><span class="sxs-lookup"><span data-stu-id="da050-177">c.</span></span> <span data-ttu-id="da050-178">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **Service Provider (SP) initierade Endpoint** textruta.</span><span class="sxs-lookup"><span data-stu-id="da050-178">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **Service Provider (SP) Initiated Endpoint** textbox.</span></span>
            
    <span data-ttu-id="da050-179">d.</span><span class="sxs-lookup"><span data-stu-id="da050-179">d.</span></span> <span data-ttu-id="da050-180">Välj **SAML aktiverat** som **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="da050-180">Select **SAML Enabled** as **Enable**.</span></span>

    <span data-ttu-id="da050-181">e.</span><span class="sxs-lookup"><span data-stu-id="da050-181">e.</span></span> <span data-ttu-id="da050-182">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="da050-182">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="da050-183">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="da050-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="da050-184">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="da050-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="da050-185">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="da050-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="da050-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="da050-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="da050-187">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="da050-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="da050-189">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="da050-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="da050-190">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="da050-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="da050-192">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="da050-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="da050-194">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="da050-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="da050-196">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="da050-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="da050-198">a.</span><span class="sxs-lookup"><span data-stu-id="da050-198">a.</span></span> <span data-ttu-id="da050-199">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="da050-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="da050-200">b.</span><span class="sxs-lookup"><span data-stu-id="da050-200">b.</span></span> <span data-ttu-id="da050-201">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="da050-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="da050-202">c.</span><span class="sxs-lookup"><span data-stu-id="da050-202">c.</span></span> <span data-ttu-id="da050-203">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="da050-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="da050-204">d.</span><span class="sxs-lookup"><span data-stu-id="da050-204">d.</span></span> <span data-ttu-id="da050-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="da050-205">Click **Create**.</span></span>
 
### <a name="creating-a-springcm-test-user"></a><span data-ttu-id="da050-206">Skapa en testanvändare SpringCM</span><span class="sxs-lookup"><span data-stu-id="da050-206">Creating a SpringCM test user</span></span>

<span data-ttu-id="da050-207">tooenable Azure Active Directory användare toolog i tooSpringCM, måste de etableras i SpringCM.</span><span class="sxs-lookup"><span data-stu-id="da050-207">tooenable Azure Active Directory users toolog in tooSpringCM, they must be provisioned into SpringCM.</span></span> <span data-ttu-id="da050-208">Hello gäller SpringCM är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="da050-208">In hello case of SpringCM, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="da050-209">Mer information finns i [skapa och redigera användare SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span><span class="sxs-lookup"><span data-stu-id="da050-209">For more information, see [Create and Edit a SpringCM User](http://knowledge.springcm.com/create-and-edit-a-springcm-user).</span></span> 

<span data-ttu-id="da050-210">**tooprovision konto tooSpringCM för en användare utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="da050-210">**tooprovision a user account tooSpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="da050-211">Logga in tooyour **SpringCM** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="da050-211">Log in tooyour **SpringCM** company site as administrator.</span></span>

2. <span data-ttu-id="da050-212">Klicka på **GOTO**, och klicka sedan på **ADRESSBOKEN**.</span><span class="sxs-lookup"><span data-stu-id="da050-212">Click **GOTO**, and then click **ADDRESS BOOK**.</span></span>
   
    <span data-ttu-id="da050-213">![Skapa användare](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="da050-213">![Create User](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "Create User")</span></span>

3. <span data-ttu-id="da050-214">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="da050-214">Click **Create User**.</span></span>

4. <span data-ttu-id="da050-215">Välj en **användarrollen**.</span><span class="sxs-lookup"><span data-stu-id="da050-215">Select a **User Role**.</span></span>

5. <span data-ttu-id="da050-216">Välj **skickar Aktiveringsmeddelandet**.</span><span class="sxs-lookup"><span data-stu-id="da050-216">Select **Send Activation Email**.</span></span>

6. <span data-ttu-id="da050-217">Typen hello förnamn, efternamn och e-postadressen till ett giltigt Azure Active Directory-användarkonto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="da050-217">Type hello first name, last name, and email address of a valid Azure Active Directory user account you want tooprovision into hello related textboxes.</span></span>

7. <span data-ttu-id="da050-218">Lägg till hello användaren tooa **säkerhetsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="da050-218">Add hello user tooa **Security group**.</span></span>

8. <span data-ttu-id="da050-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="da050-219">Click **Save**.</span></span>

  >[!NOTE]
  ><span data-ttu-id="da050-220">Du kan använda något annat SpringCM användarens konto skapas verktyg eller API: er som tillhandahålls av SpringCM tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="da050-220">You can use any other SpringCM user account creation tools or APIs provided by SpringCM tooprovision AAD user accounts.</span></span>  
  > 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="da050-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="da050-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="da050-222">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSpringCM.</span><span class="sxs-lookup"><span data-stu-id="da050-222">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSpringCM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="da050-224">**tooassign Britta Simon tooSpringCM utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="da050-224">**tooassign Britta Simon tooSpringCM, perform hello following steps:**</span></span>

1. <span data-ttu-id="da050-225">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="da050-225">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="da050-227">Välj i listan med program hello **SpringCM**.</span><span class="sxs-lookup"><span data-stu-id="da050-227">In hello applications list, select **SpringCM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. <span data-ttu-id="da050-229">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="da050-229">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="da050-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="da050-231">Click **Add** button.</span></span> <span data-ttu-id="da050-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="da050-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="da050-234">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="da050-234">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="da050-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="da050-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="da050-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="da050-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="da050-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="da050-237">Testing single sign-on</span></span>

<span data-ttu-id="da050-238">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="da050-238">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="da050-239">Du bör få automatiskt inloggade tooyour SpringCM programmet när du klickar på hello SpringCM panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="da050-239">When you click hello SpringCM tile in hello Access Panel, you should get automatically signed-on tooyour SpringCM application.</span></span>

<span data-ttu-id="da050-240">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="da050-240">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="da050-241">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="da050-241">Additional resources</span></span>

* [<span data-ttu-id="da050-242">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="da050-242">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="da050-243">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="da050-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png

