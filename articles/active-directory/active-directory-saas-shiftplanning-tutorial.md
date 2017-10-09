---
title: "Självstudier: Azure Active Directory-integrering med mänskligheten | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och mänskligheten."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 7d8a04a2eb3c997f86f1e199c47809fa3dad60e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a><span data-ttu-id="b9b2e-103">Självstudier: Azure Active Directory-integrering med mänskligheten</span><span class="sxs-lookup"><span data-stu-id="b9b2e-103">Tutorial: Azure Active Directory integration with Humanity</span></span>

<span data-ttu-id="b9b2e-104">I kursen får du lära dig hur toointegrate mänskligheten med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b9b2e-104">In this tutorial, you learn how toointegrate Humanity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b9b2e-105">Integrera mänskligheten med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-105">Integrating Humanity with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b9b2e-106">Du kan styra i Azure AD som har åtkomst till tooHumanity</span><span class="sxs-lookup"><span data-stu-id="b9b2e-106">You can control in Azure AD who has access tooHumanity</span></span>
- <span data-ttu-id="b9b2e-107">Du kan aktivera din användare tooautomatically get inloggade tooHumanity (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b9b2e-107">You can enable your users tooautomatically get signed-on tooHumanity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b9b2e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b9b2e-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b9b2e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b9b2e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9b2e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b9b2e-110">Prerequisites</span></span>

<span data-ttu-id="b9b2e-111">tooconfigure Azure AD-integrering med mänskligheten, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-111">tooconfigure Azure AD integration with Humanity, you need hello following items:</span></span>

- <span data-ttu-id="b9b2e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b9b2e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b9b2e-113">En mänskligheten enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="b9b2e-113">A Humanity single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b9b2e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b9b2e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b9b2e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b9b2e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b9b2e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b9b2e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b9b2e-118">Scenario description</span></span>
<span data-ttu-id="b9b2e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b9b2e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b9b2e-121">Att lägga till mänskligheten från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b9b2e-121">Adding Humanity from hello gallery</span></span>
2. <span data-ttu-id="b9b2e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9b2e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-humanity-from-hello-gallery"></a><span data-ttu-id="b9b2e-123">Att lägga till mänskligheten från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b9b2e-123">Adding Humanity from hello gallery</span></span>
<span data-ttu-id="b9b2e-124">tooconfigure hello integrering av mänskligheten till Azure AD, behöver du tooadd mänskligheten hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-124">tooconfigure hello integration of Humanity into Azure AD, you need tooadd Humanity from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b9b2e-125">**tooadd mänskligheten från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b9b2e-125">**tooadd Humanity from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9b2e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b9b2e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b9b2e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b9b2e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b9b2e-133">Skriv i sökrutan hello **mänskligheten**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-133">In hello search box, type **Humanity**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_search.png)

5. <span data-ttu-id="b9b2e-135">Markera hello resultat på panelen **mänskligheten**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-135">In hello results panel, select **Humanity**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b9b2e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9b2e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b9b2e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med mänskligheten baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-138">In this section, you configure and test Azure AD single sign-on with Humanity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="b9b2e-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i mänskligheten är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Humanity is tooa user in Azure AD.</span></span> <span data-ttu-id="b9b2e-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i mänskligheten toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-140">In other words, a link relationship between an Azure AD user and hello related user in Humanity needs toobe established.</span></span>

<span data-ttu-id="b9b2e-141">I mänskligheten, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-141">In Humanity, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b9b2e-142">tooconfigure och testa Azure AD enkel inloggning med mänskligheten, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-142">tooconfigure and test Azure AD single sign-on with Humanity, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b9b2e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b9b2e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b9b2e-145">**[Skapa en testanvändare mänskligheten](#creating-a-humanity-test-user)**  -toohave en motsvarighet för Britta Simon i mänskligheten som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-145">**[Creating a Humanity test user](#creating-a-humanity-test-user)** - toohave a counterpart of Britta Simon in Humanity that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b9b2e-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b9b2e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b9b2e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9b2e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b9b2e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet mänskligheten.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Humanity application.</span></span>

<span data-ttu-id="b9b2e-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med mänskligheten:**</span><span class="sxs-lookup"><span data-stu-id="b9b2e-150">**tooconfigure Azure AD single sign-on with Humanity, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9b2e-151">I hello Azure-portalen på hello **mänskligheten** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-151">In hello Azure portal, on hello **Humanity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b9b2e-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. <span data-ttu-id="b9b2e-155">På hello **mänskligheten domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-155">On hello **Humanity Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_url.png)

    <span data-ttu-id="b9b2e-157">a.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-157">a.</span></span> <span data-ttu-id="b9b2e-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://company.humanity.com/includes/saml/`</span><span class="sxs-lookup"><span data-stu-id="b9b2e-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://company.humanity.com/includes/saml/`</span></span>

    <span data-ttu-id="b9b2e-159">b.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-159">b.</span></span> <span data-ttu-id="b9b2e-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://company.humanity.com/app/`</span><span class="sxs-lookup"><span data-stu-id="b9b2e-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://company.humanity.com/app/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b9b2e-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-161">These values are not real.</span></span> <span data-ttu-id="b9b2e-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b9b2e-163">Kontakta [mänskligheten klienten supportteamet](https://www.humanity.com/support/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-163">Contact [Humanity Client support team](https://www.humanity.com/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="b9b2e-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. <span data-ttu-id="b9b2e-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b9b2e-168">På hello **mänskligheten Configuration** klickar du på **konfigurera mänskligheten** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-168">On hello **Humanity Configuration** section, click **Configure Humanity** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b9b2e-169">Kopiera hello **SAML inloggning tjänst-URL för enkel och Sign-Out URL** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b9b2e-169">Copy hello **SAML Single Sign-On Service URL, and Sign-Out URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. <span data-ttu-id="b9b2e-171">Logga in tooyour i ett annat webbläsarfönster **mänskligheten** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-171">In a different web browser window, log in tooyour **Humanity** company site as an administrator.</span></span>

8. <span data-ttu-id="b9b2e-172">Hello-menyn överst hello **Admin**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-172">In hello menu on hello top, click **Admin**.</span></span>
   
    <span data-ttu-id="b9b2e-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="b9b2e-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

9. <span data-ttu-id="b9b2e-174">Under **integrering**, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-174">Under **Integration**, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="b9b2e-175">![Enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b9b2e-175">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "Single Sign-On")</span></span>

10. <span data-ttu-id="b9b2e-176">I hello **enkel inloggning** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-176">In hello **Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b9b2e-177">![Enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b9b2e-177">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="b9b2e-178">a.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-178">a.</span></span> <span data-ttu-id="b9b2e-179">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-179">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="b9b2e-180">b.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-180">b.</span></span> <span data-ttu-id="b9b2e-181">Välj **Tillåt lösenord inloggningen**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-181">Select **Allow Password Login**.</span></span>

    <span data-ttu-id="b9b2e-182">c.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-182">c.</span></span> <span data-ttu-id="b9b2e-183">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värdet till hello **utfärdar-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-183">Paste hello **SAML Single Sign-On Service URL** value into hello **SAML Issuer URL** textbox.</span></span>

    <span data-ttu-id="b9b2e-184">d.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-184">d.</span></span> <span data-ttu-id="b9b2e-185">Klistra in hello **Sign-Out URL** värdet till hello **Remote logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-185">Paste hello **Sign-Out URL** value into hello **Remote Logout URL** textbox.</span></span>
   
    <span data-ttu-id="b9b2e-186">e.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-186">e.</span></span> <span data-ttu-id="b9b2e-187">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-187">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox.</span></span>

11. <span data-ttu-id="b9b2e-188">Klicka på **Spara inställningar**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-188">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="b9b2e-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b9b2e-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b9b2e-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b9b2e-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b9b2e-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b9b2e-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9b2e-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="b9b2e-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b9b2e-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b9b2e-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9b2e-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b9b2e-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b9b2e-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b9b2e-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b9b2e-204">a.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-204">a.</span></span> <span data-ttu-id="b9b2e-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b9b2e-206">b.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-206">b.</span></span> <span data-ttu-id="b9b2e-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b9b2e-208">c.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-208">c.</span></span> <span data-ttu-id="b9b2e-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b9b2e-210">d.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-210">d.</span></span> <span data-ttu-id="b9b2e-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-211">Click **Create**.</span></span>
 
### <a name="creating-a-humanity-test-user"></a><span data-ttu-id="b9b2e-212">Skapa en testanvändare mänskligheten</span><span class="sxs-lookup"><span data-stu-id="b9b2e-212">Creating a Humanity test user</span></span>

<span data-ttu-id="b9b2e-213">I ordning tooenable Azure AD-användare toolog i tooHumanity, måste de etableras i mänskligheten.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-213">In order tooenable Azure AD users toolog in tooHumanity, they must be provisioned into Humanity.</span></span> <span data-ttu-id="b9b2e-214">Hello gäller mänskligheten är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-214">In hello case of Humanity, provisioning is a manual task.</span></span>

<span data-ttu-id="b9b2e-215">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b9b2e-215">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9b2e-216">Logga in tooyour **mänskligheten** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-216">Log in tooyour **Humanity** company site as an administrator.</span></span>

2. <span data-ttu-id="b9b2e-217">Klicka på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-217">Click **Admin**.</span></span>
   
    <span data-ttu-id="b9b2e-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="b9b2e-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

3. <span data-ttu-id="b9b2e-219">Klicka på **personal**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-219">Click **Staff**.</span></span>
   
    <span data-ttu-id="b9b2e-220">![Personal](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "personal")</span><span class="sxs-lookup"><span data-stu-id="b9b2e-220">![Staff](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "Staff")</span></span>

4. <span data-ttu-id="b9b2e-221">Under **åtgärder**, klickar du på **lägga till anställda**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-221">Under **Actions**, click **Add Employees**.</span></span>
   
    <span data-ttu-id="b9b2e-222">![Lägg till medarbetare](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "lägga till anställda")</span><span class="sxs-lookup"><span data-stu-id="b9b2e-222">![Add Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "Add Employees")</span></span>

5. <span data-ttu-id="b9b2e-223">I hello **lägga till anställda** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b9b2e-223">In hello **Add Employees** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="b9b2e-224">![Spara anställda](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "spara anställda")</span><span class="sxs-lookup"><span data-stu-id="b9b2e-224">![Save Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "Save Employees")</span></span>
   
    <span data-ttu-id="b9b2e-225">a.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-225">a.</span></span> <span data-ttu-id="b9b2e-226">Typen hello **Förnamn**, **efternamn**, och **e-post** av en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-226">Type hello **First Name**, **Last Name**, and **Email** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

    <span data-ttu-id="b9b2e-227">b.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-227">b.</span></span> <span data-ttu-id="b9b2e-228">Klicka på **spara anställda**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-228">Click **Save Employees**.</span></span>

>[!NOTE]
><span data-ttu-id="b9b2e-229">Du kan använda något annat mänskligheten användarens konto skapas verktyg eller API: er som tillhandahålls av mänskligheten tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-229">You can use any other Humanity user account creation tools or APIs provided by Humanity tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b9b2e-230">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b9b2e-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b9b2e-231">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHumanity.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHumanity.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b9b2e-233">**tooassign Britta Simon tooHumanity utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b9b2e-233">**tooassign Britta Simon tooHumanity, perform hello following steps:**</span></span>

1. <span data-ttu-id="b9b2e-234">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b9b2e-236">Välj i listan med program hello **mänskligheten**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-236">In hello applications list, select **Humanity**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_app.png) 

3. <span data-ttu-id="b9b2e-238">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b9b2e-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-240">Click **Add** button.</span></span> <span data-ttu-id="b9b2e-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b9b2e-243">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b9b2e-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b9b2e-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b9b2e-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b9b2e-246">Testing single sign-on</span></span>

<span data-ttu-id="b9b2e-247">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-247">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b9b2e-248">Du bör få automatiskt inloggade tooyour mänskligheten programmet när du klickar på hello mänskligheten panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b9b2e-248">When you click hello Humanity tile in hello Access Panel, you should get automatically signed-on tooyour Humanity application.</span></span>
<span data-ttu-id="b9b2e-249">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b9b2e-249">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9b2e-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b9b2e-250">Additional resources</span></span>

* [<span data-ttu-id="b9b2e-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b9b2e-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b9b2e-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b9b2e-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_203.png

