---
title: "Självstudier: Azure Active Directory-integrering med ClickTime | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ClickTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: a0259e31164cad6c6c77ed8aac1c50cd9a3e46ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a><span data-ttu-id="acc11-103">Självstudier: Azure Active Directory-integrering med ClickTime</span><span class="sxs-lookup"><span data-stu-id="acc11-103">Tutorial: Azure Active Directory integration with ClickTime</span></span>

<span data-ttu-id="acc11-104">I kursen får du lära dig hur toointegrate ClickTime med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="acc11-104">In this tutorial, you learn how toointegrate ClickTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="acc11-105">Integrera ClickTime med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="acc11-105">Integrating ClickTime with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="acc11-106">Du kan styra i Azure AD som har åtkomst till tooClickTime</span><span class="sxs-lookup"><span data-stu-id="acc11-106">You can control in Azure AD who has access tooClickTime</span></span>
- <span data-ttu-id="acc11-107">Du kan aktivera din användare tooautomatically get inloggade tooClickTime (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="acc11-107">You can enable your users tooautomatically get signed-on tooClickTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="acc11-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="acc11-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="acc11-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="acc11-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="acc11-110">Krav</span><span class="sxs-lookup"><span data-stu-id="acc11-110">Prerequisites</span></span>

<span data-ttu-id="acc11-111">tooconfigure Azure AD-integrering med ClickTime, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="acc11-111">tooconfigure Azure AD integration with ClickTime, you need hello following items:</span></span>

- <span data-ttu-id="acc11-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="acc11-112">An Azure AD subscription</span></span>
- <span data-ttu-id="acc11-113">En ClickTime enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="acc11-113">A ClickTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="acc11-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="acc11-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="acc11-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="acc11-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="acc11-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="acc11-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="acc11-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="acc11-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="acc11-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="acc11-118">Scenario description</span></span>
<span data-ttu-id="acc11-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="acc11-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="acc11-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="acc11-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="acc11-121">Att lägga till ClickTime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="acc11-121">Adding ClickTime from hello gallery</span></span>
2. <span data-ttu-id="acc11-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acc11-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clicktime-from-hello-gallery"></a><span data-ttu-id="acc11-123">Att lägga till ClickTime från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="acc11-123">Adding ClickTime from hello gallery</span></span>
<span data-ttu-id="acc11-124">tooconfigure hello integrering av ClickTime i Azure AD, behöver du tooadd ClickTime hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="acc11-124">tooconfigure hello integration of ClickTime into Azure AD, you need tooadd ClickTime from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="acc11-125">**tooadd ClickTime från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="acc11-125">**tooadd ClickTime from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="acc11-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="acc11-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="acc11-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="acc11-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="acc11-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="acc11-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="acc11-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acc11-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="acc11-133">Skriv i sökrutan hello **ClickTime**väljer **ClickTime** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="acc11-133">In hello search box, type **ClickTime**, select **ClickTime** from result panel then click **Add** button tooadd hello application.</span></span>

    ![ClickTime i hello resultatlistan](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="acc11-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acc11-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="acc11-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ClickTime baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="acc11-136">In this section, you configure and test Azure AD single sign-on with ClickTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="acc11-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ClickTime är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acc11-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ClickTime is tooa user in Azure AD.</span></span> <span data-ttu-id="acc11-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ClickTime toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="acc11-138">In other words, a link relationship between an Azure AD user and hello related user in ClickTime needs toobe established.</span></span>

<span data-ttu-id="acc11-139">I ClickTime, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="acc11-139">In ClickTime, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="acc11-140">tooconfigure och testa Azure AD enkel inloggning med ClickTime, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="acc11-140">tooconfigure and test Azure AD single sign-on with ClickTime, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="acc11-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="acc11-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="acc11-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="acc11-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="acc11-143">**[Skapa en testanvändare ClickTime](#create-a-clicktime-test-user)**  -toohave en motsvarighet för Britta Simon i ClickTime som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="acc11-143">**[Create a ClickTime test user](#create-a-clicktime-test-user)** - toohave a counterpart of Britta Simon in ClickTime that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="acc11-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="acc11-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="acc11-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="acc11-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="acc11-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acc11-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="acc11-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ClickTime program.</span><span class="sxs-lookup"><span data-stu-id="acc11-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ClickTime application.</span></span>

<span data-ttu-id="acc11-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ClickTime:**</span><span class="sxs-lookup"><span data-stu-id="acc11-148">**tooconfigure Azure AD single sign-on with ClickTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="acc11-149">I hello Azure-portalen på hello **ClickTime** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="acc11-149">In hello Azure portal, on hello **ClickTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="acc11-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="acc11-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. <span data-ttu-id="acc11-153">På hello **ClickTime domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acc11-153">On hello **ClickTime Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och ClickTime domän med enkel inloggning information](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    <span data-ttu-id="acc11-155">a.</span><span class="sxs-lookup"><span data-stu-id="acc11-155">a.</span></span> <span data-ttu-id="acc11-156">I hello **identifierare** textruta Skriv en URL som:`https://app.clicktime.com/sp/`</span><span class="sxs-lookup"><span data-stu-id="acc11-156">In hello **Identifier** textbox, type a URL as: `https://app.clicktime.com/sp/`</span></span>
    
    <span data-ttu-id="acc11-157">b.</span><span class="sxs-lookup"><span data-stu-id="acc11-157">b.</span></span> <span data-ttu-id="acc11-158">I hello **Reply URL** textruta, ange en Webbadress med hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="acc11-158">In hello **Reply URL** textbox, type a URL using hello following patterns:</span></span> 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. <span data-ttu-id="acc11-159">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="acc11-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. <span data-ttu-id="acc11-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="acc11-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="acc11-163">På hello **ClickTime Configuration** klickar du på **konfigurera ClickTime** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="acc11-163">On hello **ClickTime Configuration** section, click **Configure ClickTime** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="acc11-164">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="acc11-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![ClickTime konfiguration](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. <span data-ttu-id="acc11-166">Logga in på webbplatsen ClickTime företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="acc11-166">In a different web browser window, log into your ClickTime company site as an administrator.</span></span>

8. <span data-ttu-id="acc11-167">Klicka i hello verktygsfältet hello längst upp **inställningar**, och klicka sedan på **säkerhetsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="acc11-167">In hello toolbar on hello top, click **Preferences**, and then click **Security Settings**.</span></span>

9. <span data-ttu-id="acc11-168">I hello **inställningar för enkel inloggning** konfigurationen och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acc11-168">In hello **Single Sign-On Preferences** configuration section, perform hello following steps:</span></span>
   
    <span data-ttu-id="acc11-169">![Säkerhetsinställningar](./media/active-directory-saas-clicktime-tutorial/tic777280.png "säkerhetsinställningar")</span><span class="sxs-lookup"><span data-stu-id="acc11-169">![Security Settings](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Security Settings")</span></span>
   
    <span data-ttu-id="acc11-170">a.</span><span class="sxs-lookup"><span data-stu-id="acc11-170">a.</span></span>  <span data-ttu-id="acc11-171">Välj **Tillåt** logga in med enkel inloggning (SSO) med **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="acc11-171">Select **Allow** sign-in using Single Sign-On (SSO) with **Azure AD**.</span></span>
   
    <span data-ttu-id="acc11-172">b.</span><span class="sxs-lookup"><span data-stu-id="acc11-172">b.</span></span> <span data-ttu-id="acc11-173">I hello **identitet Leverantörsslutpunkt** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="acc11-173">In hello **Identity Provider Endpoint** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="acc11-174">c.</span><span class="sxs-lookup"><span data-stu-id="acc11-174">c.</span></span>  <span data-ttu-id="acc11-175">Öppna hello **Base64-kodat certifikat** hämtas från Azure-portalen i **anteckningar**, kopiera hello innehållet och klistra in den i hello **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="acc11-175">Open hello **base-64 encoded certificate** downloaded from Azure portal in **Notepad**, copy hello content, and then paste it into hello **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="acc11-176">d.</span><span class="sxs-lookup"><span data-stu-id="acc11-176">d.</span></span>  <span data-ttu-id="acc11-177">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="acc11-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="acc11-178">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="acc11-178">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="acc11-179">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="acc11-179">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="acc11-180">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="acc11-180">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="acc11-181">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="acc11-181">Create an Azure AD test user</span></span>
<span data-ttu-id="acc11-182">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="acc11-182">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="acc11-184">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="acc11-184">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="acc11-185">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="acc11-185">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="acc11-187">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="acc11-187">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="acc11-189">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acc11-189">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="acc11-191">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acc11-191">In hello **User** dialog box, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="acc11-193">a.</span><span class="sxs-lookup"><span data-stu-id="acc11-193">a.</span></span> <span data-ttu-id="acc11-194">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="acc11-194">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="acc11-195">b.</span><span class="sxs-lookup"><span data-stu-id="acc11-195">b.</span></span> <span data-ttu-id="acc11-196">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="acc11-196">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="acc11-197">c.</span><span class="sxs-lookup"><span data-stu-id="acc11-197">c.</span></span> <span data-ttu-id="acc11-198">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="acc11-198">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="acc11-199">d.</span><span class="sxs-lookup"><span data-stu-id="acc11-199">d.</span></span> <span data-ttu-id="acc11-200">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="acc11-200">Click **Create**.</span></span>
 
### <a name="create-a-clicktime-test-user"></a><span data-ttu-id="acc11-201">Skapa en testanvändare ClickTime</span><span class="sxs-lookup"><span data-stu-id="acc11-201">Create a ClickTime test user</span></span>

<span data-ttu-id="acc11-202">I ordning tooenable Azure AD-användare toolog i ClickTime, måste de etableras i ClickTime.</span><span class="sxs-lookup"><span data-stu-id="acc11-202">In order tooenable Azure AD users toolog into ClickTime, they must be provisioned into ClickTime.</span></span>  
<span data-ttu-id="acc11-203">Hello gäller ClickTime är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="acc11-203">In hello case of ClickTime, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="acc11-204">Du kan använda något annat ClickTime användarens konto skapas verktyg eller API: er som tillhandahålls av ClickTime tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acc11-204">You can use any other ClickTime user account creation tools or APIs provided by ClickTime tooprovision Azure AD user accounts.</span></span>

<span data-ttu-id="acc11-205">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="acc11-205">**tooprovision a user account, perform hello following steps:**</span></span>
1. <span data-ttu-id="acc11-206">Logga in tooyour **ClickTime** klient.</span><span class="sxs-lookup"><span data-stu-id="acc11-206">Log in tooyour **ClickTime** tenant.</span></span>
2. <span data-ttu-id="acc11-207">Klicka i hello verktygsfältet hello längst upp **företagets**, och klicka sedan på **personer**.</span><span class="sxs-lookup"><span data-stu-id="acc11-207">In hello toolbar on hello top, click **Company**, and then click **People**.</span></span>
   
    <span data-ttu-id="acc11-208">![Personer](./media/active-directory-saas-clicktime-tutorial/tic777282.png "personer")</span><span class="sxs-lookup"><span data-stu-id="acc11-208">![People](./media/active-directory-saas-clicktime-tutorial/tic777282.png "People")</span></span>
3. <span data-ttu-id="acc11-209">Klicka på **lägga till personen**.</span><span class="sxs-lookup"><span data-stu-id="acc11-209">Click **Add Person**.</span></span>
   
    <span data-ttu-id="acc11-210">![Lägga till personen](./media/active-directory-saas-clicktime-tutorial/tic777283.png "lägga till Person")</span><span class="sxs-lookup"><span data-stu-id="acc11-210">![Add Person](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Add Person")</span></span>
4. <span data-ttu-id="acc11-211">I hello ny Person avsnitt, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="acc11-211">In hello New Person section, perform hello following steps:</span></span>
   
    <span data-ttu-id="acc11-212">![Personer](./media/active-directory-saas-clicktime-tutorial/tic777284.png "personer")</span><span class="sxs-lookup"><span data-stu-id="acc11-212">![People](./media/active-directory-saas-clicktime-tutorial/tic777284.png "People")</span></span>
   
    <span data-ttu-id="acc11-213">a.</span><span class="sxs-lookup"><span data-stu-id="acc11-213">a.</span></span>  <span data-ttu-id="acc11-214">I hello **fullständigt namn** textruta typen fullständigt namn för användaren som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="acc11-214">In hello **full name** textbox, type full name of user like **Britta Simon**.</span></span> 
  
    <span data-ttu-id="acc11-215">b.</span><span class="sxs-lookup"><span data-stu-id="acc11-215">b.</span></span>  <span data-ttu-id="acc11-216">I hello **e-postadress** textruta hello e-post för användare som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="acc11-216">In hello **email address** textbox, type hello email of user like **brittasimon@contoso.com**.</span></span>
       
    > [!NOTE]
    > <span data-ttu-id="acc11-217">Om du vill kan ange du ytterligare egenskaper hello nytt person objekt.</span><span class="sxs-lookup"><span data-stu-id="acc11-217">If you want to, you can set additional properties of hello new person object.</span></span>
   
    <span data-ttu-id="acc11-218">c.</span><span class="sxs-lookup"><span data-stu-id="acc11-218">c.</span></span>  <span data-ttu-id="acc11-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="acc11-219">Click **Save**.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="acc11-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="acc11-220">Assign hello Azure AD test user</span></span>

<span data-ttu-id="acc11-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooClickTime.</span><span class="sxs-lookup"><span data-stu-id="acc11-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooClickTime.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="acc11-223">**tooassign Britta Simon tooClickTime utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="acc11-223">**tooassign Britta Simon tooClickTime, perform hello following steps:**</span></span>

1. <span data-ttu-id="acc11-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="acc11-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="acc11-226">Välj i listan med program hello **ClickTime**.</span><span class="sxs-lookup"><span data-stu-id="acc11-226">In hello applications list, select **ClickTime**.</span></span>

    ![ClickTimne länken i listan med program hello](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. <span data-ttu-id="acc11-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="acc11-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="acc11-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="acc11-230">Click **Add** button.</span></span> <span data-ttu-id="acc11-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acc11-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="acc11-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="acc11-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="acc11-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acc11-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="acc11-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="acc11-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="acc11-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="acc11-236">Test single sign-on</span></span>

<span data-ttu-id="acc11-237">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="acc11-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="acc11-238">Du bör få automatiskt inloggade tooyour ClickTime programmet när du klickar på hello ClickTime panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="acc11-238">When you click hello ClickTime tile in hello Access Panel, you should get automatically signed-on tooyour ClickTime application.</span></span>
<span data-ttu-id="acc11-239">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="acc11-239">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="acc11-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="acc11-240">Additional resources</span></span>

* [<span data-ttu-id="acc11-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="acc11-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="acc11-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="acc11-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

