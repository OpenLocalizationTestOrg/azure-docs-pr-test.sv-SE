---
title: "Självstudier: Azure Active Directory-integrering med Yonyx interaktiva stödlinjer | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Yonyx interaktiva guider och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: 24e30d243143651b8d32535c76dc300931ae5746
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a><span data-ttu-id="afec4-103">Självstudier: Azure Active Directory-integrering med Yonyx interaktiva stödlinjer</span><span class="sxs-lookup"><span data-stu-id="afec4-103">Tutorial: Azure Active Directory integration with Yonyx Interactive Guides</span></span>

<span data-ttu-id="afec4-104">I kursen får du lära dig hur toointegrate Yonyx interaktiv hjälper med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="afec4-104">In this tutorial, you learn how toointegrate Yonyx Interactive Guides with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="afec4-105">Integrera Yonyx interaktiva guider med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="afec4-105">Integrating Yonyx Interactive Guides with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="afec4-106">Du kan styra i Azure AD som har åtkomst tooYonyx interaktiva guider</span><span class="sxs-lookup"><span data-stu-id="afec4-106">You can control in Azure AD who has access tooYonyx Interactive Guides</span></span>
- <span data-ttu-id="afec4-107">Du kan aktivera din användare tooautomatically get inloggade tooYonyx interaktiva guider (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="afec4-107">You can enable your users tooautomatically get signed-on tooYonyx Interactive Guides (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="afec4-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="afec4-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="afec4-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="afec4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="afec4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="afec4-110">Prerequisites</span></span>

<span data-ttu-id="afec4-111">tooconfigure Azure AD-integrering med Yonyx interaktiva guider måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="afec4-111">tooconfigure Azure AD integration with Yonyx Interactive Guides, you need hello following items:</span></span>

- <span data-ttu-id="afec4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="afec4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="afec4-113">En Yonyx interaktiva guider enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="afec4-113">A Yonyx Interactive Guides single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="afec4-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="afec4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="afec4-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="afec4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="afec4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="afec4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="afec4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="afec4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="afec4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="afec4-118">Scenario description</span></span>
<span data-ttu-id="afec4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="afec4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="afec4-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="afec4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="afec4-121">Att lägga till Yonyx interaktiva guider från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="afec4-121">Adding Yonyx Interactive Guides from hello gallery</span></span>
2. <span data-ttu-id="afec4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="afec4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-yonyx-interactive-guides-from-hello-gallery"></a><span data-ttu-id="afec4-123">Att lägga till Yonyx interaktiva guider från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="afec4-123">Adding Yonyx Interactive Guides from hello gallery</span></span>
<span data-ttu-id="afec4-124">tooconfigure hello integrering av Yonyx interaktiva guider i Azure AD, behöver du tooadd Yonyx interaktiva guider hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="afec4-124">tooconfigure hello integration of Yonyx Interactive Guides into Azure AD, you need tooadd Yonyx Interactive Guides from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="afec4-125">**tooadd Yonyx interaktiva guider från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="afec4-125">**tooadd Yonyx Interactive Guides from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="afec4-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="afec4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="afec4-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="afec4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="afec4-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="afec4-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="afec4-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="afec4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="afec4-133">Skriv i sökrutan hello **Yonyx interaktiva guider**väljer **Yonyx interaktiva guider** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="afec4-133">In hello search box, type **Yonyx Interactive Guides**, select  **Yonyx Interactive Guides**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Yonyx interaktiva guider i hello resultatlistan](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="afec4-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="afec4-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="afec4-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Yonyx interaktiva guider baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="afec4-136">In this section, you configure and test Azure AD single sign-on with Yonyx Interactive Guides based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="afec4-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Yonyx interaktiva guider är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="afec4-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Yonyx Interactive Guides is tooa user in Azure AD.</span></span> <span data-ttu-id="afec4-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Yonyx interaktiva guider toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="afec4-138">In other words, a link relationship between an Azure AD user and hello related user in Yonyx Interactive Guides needs toobe established.</span></span>

<span data-ttu-id="afec4-139">I Yonyx interaktiva distributionsguider, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="afec4-139">In Yonyx Interactive Guides, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="afec4-140">tooconfigure och testa Azure AD enkel inloggning med Yonyx interaktiva stödlinjer, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="afec4-140">tooconfigure and test Azure AD single sign-on with Yonyx Interactive Guides, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="afec4-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="afec4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="afec4-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="afec4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="afec4-143">**[Skapa en testanvändare Yonyx interaktiva guider](#create-a-yonyx-interactive-guides-test-user)**  -toohave en motsvarighet för Britta Simon i Yonyx interaktiva guider som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="afec4-143">**[Create a Yonyx Interactive Guides test user](#create-a-yonyx-interactive-guides-test-user)** - toohave a counterpart of Britta Simon in Yonyx Interactive Guides that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="afec4-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="afec4-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="afec4-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="afec4-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="afec4-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="afec4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="afec4-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Yonyx interaktiva guider.</span><span class="sxs-lookup"><span data-stu-id="afec4-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="afec4-148">**tooconfigure Azure AD enkel inloggning med Yonyx interaktiva stödlinjer, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="afec4-148">**tooconfigure Azure AD single sign-on with Yonyx Interactive Guides, perform hello following steps:**</span></span>

1. <span data-ttu-id="afec4-149">I hello Azure-portalen på hello **Yonyx interaktiva guider** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="afec4-149">In hello Azure portal, on hello **Yonyx Interactive Guides** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="afec4-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="afec4-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_samlbase.png)

3. <span data-ttu-id="afec4-153">På hello **Yonyx interaktiva guider domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="afec4-153">On hello **Yonyx Interactive Guides Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Yonyx interaktiva guider domän med enkel inloggning information](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_url.png)

    <span data-ttu-id="afec4-155">a.</span><span class="sxs-lookup"><span data-stu-id="afec4-155">a.</span></span> <span data-ttu-id="afec4-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span><span class="sxs-lookup"><span data-stu-id="afec4-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`</span></span>

    <span data-ttu-id="afec4-157">b.</span><span class="sxs-lookup"><span data-stu-id="afec4-157">b.</span></span> <span data-ttu-id="afec4-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.yonyx.com`</span><span class="sxs-lookup"><span data-stu-id="afec4-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.yonyx.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="afec4-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="afec4-159">These values are not real.</span></span> <span data-ttu-id="afec4-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="afec4-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="afec4-161">Kontakta [Yonyx interaktiva guider klienten supportteamet](mailto:support@yonyx.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="afec4-161">Contact [Yonyx Interactive Guides Client support team](mailto:support@yonyx.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="afec4-162">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="afec4-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_certificate.png) 

5. <span data-ttu-id="afec4-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="afec4-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-yonyx-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="afec4-166">På hello **Yonyx interaktiva guider konfiguration** klickar du på **konfigurera Yonyx interaktiva guider** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="afec4-166">On hello **Yonyx Interactive Guides Configuration** section, click **Configure Yonyx Interactive Guides** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="afec4-167">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="afec4-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Yonyx interaktiva guider konfiguration](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_configure.png) 

7. <span data-ttu-id="afec4-169">tooconfigure enkel inloggning på **Yonyx interaktiva guider** sida, behöver du toosend hello hämtas **Certificate(Base64)**, **Sign-Out URL**, **SAML Enkel inloggning Tjänstwebbadress** **SAML enhets-ID** för[Yonyx interaktiva guider supportteam](mailto:support@yonyx.com).</span><span class="sxs-lookup"><span data-stu-id="afec4-169">tooconfigure single sign-on on **Yonyx Interactive Guides** side, you need toosend hello downloaded **Certificate(Base64)**, **Sign-Out URL**, **SAML Single Sign-On Service URL** **SAML Entity ID** too[Yonyx Interactive Guides support team](mailto:support@yonyx.com).</span></span> <span data-ttu-id="afec4-170">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="afec4-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="afec4-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="afec4-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="afec4-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="afec4-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="afec4-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="afec4-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="afec4-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="afec4-174">Create an Azure AD test user</span></span>

<span data-ttu-id="afec4-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="afec4-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

  ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="afec4-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="afec4-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="afec4-178">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="afec4-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="afec4-180">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="afec4-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-yonyx-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="afec4-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="afec4-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="afec4-184">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="afec4-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="afec4-186">a.</span><span class="sxs-lookup"><span data-stu-id="afec4-186">a.</span></span> <span data-ttu-id="afec4-187">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="afec4-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="afec4-188">b.</span><span class="sxs-lookup"><span data-stu-id="afec4-188">b.</span></span> <span data-ttu-id="afec4-189">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="afec4-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="afec4-190">c.</span><span class="sxs-lookup"><span data-stu-id="afec4-190">c.</span></span> <span data-ttu-id="afec4-191">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="afec4-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="afec4-192">d.</span><span class="sxs-lookup"><span data-stu-id="afec4-192">d.</span></span> <span data-ttu-id="afec4-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="afec4-193">Click **Create**.</span></span>
 
### <a name="create-a-yonyx-interactive-guides-test-user"></a><span data-ttu-id="afec4-194">Skapa en testanvändare Yonyx interaktiva guider</span><span class="sxs-lookup"><span data-stu-id="afec4-194">Create a Yonyx Interactive Guides test user</span></span>

<span data-ttu-id="afec4-195">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Yonyx interaktiva guider.</span><span class="sxs-lookup"><span data-stu-id="afec4-195">hello objective of this section is toocreate a user called Britta Simon in Yonyx Interactive Guides.</span></span> <span data-ttu-id="afec4-196">Yonyx interaktiva guider stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="afec4-196">Yonyx Interactive Guides supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="afec4-197">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="afec4-197">There is no action item for you in this section.</span></span> <span data-ttu-id="afec4-198">En ny användare skapas under ett försök tooaccess Yonyx interaktiva guider om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="afec4-198">A new user is created during an attempt tooaccess Yonyx Interactive Guides if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="afec4-199">Om du behöver toocreate en användare manuellt, måste toocontact hello Yonyx interaktiva guider supportteam via < mailto:support@yonyx.com >.</span><span class="sxs-lookup"><span data-stu-id="afec4-199">If you need toocreate a user manually, you need toocontact hello Yonyx Interactive Guides support team via <mailto:support@yonyx.com>.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="afec4-200">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="afec4-200">Assign hello Azure AD test user</span></span>

<span data-ttu-id="afec4-201">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooYonyx interaktiva guider.</span><span class="sxs-lookup"><span data-stu-id="afec4-201">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooYonyx Interactive Guides.</span></span>

![Tilldela hello användarroll][200]

<span data-ttu-id="afec4-203">**tooassign Britta Simon tooYonyx interaktiva guider utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="afec4-203">**tooassign Britta Simon tooYonyx Interactive Guides, perform hello following steps:**</span></span>

1. <span data-ttu-id="afec4-204">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="afec4-204">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="afec4-206">Välj i listan med program hello **Yonyx interaktiva guider**.</span><span class="sxs-lookup"><span data-stu-id="afec4-206">In hello applications list, select **Yonyx Interactive Guides**.</span></span>

    ![Hej Yonyx interaktiva guider länken i listan med program hello](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyxinteractiveguides_app.png) 

3. <span data-ttu-id="afec4-208">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="afec4-208">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="afec4-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="afec4-210">Click **Add** button.</span></span> <span data-ttu-id="afec4-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="afec4-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="afec4-213">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="afec4-213">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="afec4-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="afec4-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="afec4-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="afec4-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="afec4-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="afec4-216">Test single sign-on</span></span>

<span data-ttu-id="afec4-217">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="afec4-217">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="afec4-218">Du bör få automatiskt inloggade tooyour Yonyx interaktiva guider programmet när du klickar på hello Yonyx interaktiva guider panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="afec4-218">When you click hello Yonyx Interactive Guides tile in hello Access Panel, you should get automatically signed-on tooyour Yonyx Interactive Guides application.</span></span>

<span data-ttu-id="afec4-219">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="afec4-219">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="afec4-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="afec4-220">Additional resources</span></span>

* [<span data-ttu-id="afec4-221">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="afec4-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="afec4-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="afec4-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png

