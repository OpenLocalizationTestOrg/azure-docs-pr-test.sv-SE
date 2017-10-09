---
title: "Självstudier: Azure Active Directory-integrering med InsideView | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och InsideView."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 979c0c24f3a18a193616061b8c2e78292233a56d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a><span data-ttu-id="d3a65-103">Självstudier: Azure Active Directory-integrering med InsideView</span><span class="sxs-lookup"><span data-stu-id="d3a65-103">Tutorial: Azure Active Directory integration with InsideView</span></span>

<span data-ttu-id="d3a65-104">I kursen får du lära dig hur toointegrate InsideView med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d3a65-104">In this tutorial, you learn how toointegrate InsideView with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d3a65-105">Integrera InsideView med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d3a65-105">Integrating InsideView with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d3a65-106">Du kan styra i Azure AD som har åtkomst till tooInsideView</span><span class="sxs-lookup"><span data-stu-id="d3a65-106">You can control in Azure AD who has access tooInsideView</span></span>
- <span data-ttu-id="d3a65-107">Du kan aktivera din användare tooautomatically get inloggade tooInsideView (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d3a65-107">You can enable your users tooautomatically get signed-on tooInsideView (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d3a65-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d3a65-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d3a65-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d3a65-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3a65-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d3a65-110">Prerequisites</span></span>

<span data-ttu-id="d3a65-111">tooconfigure Azure AD-integrering med InsideView, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d3a65-111">tooconfigure Azure AD integration with InsideView, you need hello following items:</span></span>

- <span data-ttu-id="d3a65-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d3a65-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d3a65-113">En InsideView enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d3a65-113">A InsideView single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d3a65-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d3a65-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d3a65-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d3a65-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d3a65-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d3a65-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d3a65-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d3a65-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d3a65-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d3a65-118">Scenario description</span></span>
<span data-ttu-id="d3a65-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d3a65-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d3a65-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d3a65-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d3a65-121">Att lägga till InsideView från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d3a65-121">Adding InsideView from hello gallery</span></span>
2. <span data-ttu-id="d3a65-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3a65-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-insideview-from-hello-gallery"></a><span data-ttu-id="d3a65-123">Att lägga till InsideView från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d3a65-123">Adding InsideView from hello gallery</span></span>
<span data-ttu-id="d3a65-124">tooconfigure hello integrering av InsideView i tooAzure AD, behöver du tooadd InsideView hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d3a65-124">tooconfigure hello integration of InsideView in tooAzure AD, you need tooadd InsideView from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d3a65-125">**tooadd InsideView från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d3a65-125">**tooadd InsideView from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3a65-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d3a65-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d3a65-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d3a65-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d3a65-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3a65-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d3a65-133">Skriv i sökrutan hello **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-133">In hello search box, type **InsideView**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_search.png)

5. <span data-ttu-id="d3a65-135">Markera hello resultat på panelen **InsideView**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d3a65-135">In hello results panel, select **InsideView**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d3a65-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3a65-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d3a65-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med InsideView baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d3a65-138">In this section, you configure and test Azure AD single sign-on with InsideView based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d3a65-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i InsideView är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3a65-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in InsideView is tooa user in Azure AD.</span></span> <span data-ttu-id="d3a65-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i InsideView toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d3a65-140">In other words, a link relationship between an Azure AD user and hello related user in InsideView needs toobe established.</span></span>

<span data-ttu-id="d3a65-141">I InsideView, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d3a65-141">In InsideView, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d3a65-142">tooconfigure och testa Azure AD enkel inloggning med InsideView, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d3a65-142">tooconfigure and test Azure AD single sign-on with InsideView, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d3a65-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d3a65-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d3a65-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3a65-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d3a65-145">**[Skapa en testanvändare InsideView](#creating-a-insideview-test-user)**  -toohave en motsvarighet för Britta Simon i InsideView som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d3a65-145">**[Creating a InsideView test user](#creating-a-insideview-test-user)** - toohave a counterpart of Britta Simon in InsideView that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d3a65-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d3a65-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d3a65-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d3a65-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d3a65-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3a65-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d3a65-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt InsideView program.</span><span class="sxs-lookup"><span data-stu-id="d3a65-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your InsideView application.</span></span>

<span data-ttu-id="d3a65-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med InsideView:**</span><span class="sxs-lookup"><span data-stu-id="d3a65-150">**tooconfigure Azure AD single sign-on with InsideView, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3a65-151">I hello Azure-portalen på hello **InsideView** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-151">In hello Azure portal, on hello **InsideView** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d3a65-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d3a65-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_samlbase.png)

3. <span data-ttu-id="d3a65-155">På hello **InsideView domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d3a65-155">On hello **InsideView Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_url.png)
    
    <span data-ttu-id="d3a65-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://my.insideview.com/iv/<STS Name>/login.iv`</span><span class="sxs-lookup"><span data-stu-id="d3a65-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://my.insideview.com/iv/<STS Name>/login.iv`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d3a65-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d3a65-158">This value is not real.</span></span> <span data-ttu-id="d3a65-159">Uppdatera det här värdet med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d3a65-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="d3a65-160">Kontakta [InsideView supportteamet ](mailto:support@insideview.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="d3a65-160">Contact [InsideView support team ](mailto:support@insideview.com) tooget this value.</span></span>
 
4. <span data-ttu-id="d3a65-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="d3a65-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_certificate.png) 

5. <span data-ttu-id="d3a65-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d3a65-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insideview-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d3a65-165">På hello **InsideView Configuration** klickar du på **konfigurera InsideView** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d3a65-165">On hello **InsideView Configuration** section, click **Configure InsideView** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d3a65-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d3a65-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_configure.png) 

7. <span data-ttu-id="d3a65-168">Logga in tooyour InsideView företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="d3a65-168">In a different web browser window, log in tooyour InsideView company site as an administrator.</span></span>

8. <span data-ttu-id="d3a65-169">Klicka i hello verktygsfältet hello längst upp **Admin**, **SingleSignOn inställningar**, och klicka sedan på **lägga till SAML**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-169">In hello toolbar on hello top, click **Admin**, **SingleSignOn Settings**, and then click **Add SAML**.</span></span>
   
   <span data-ttu-id="d3a65-170">![SAML för enkel inloggning inställningar](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML för enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="d3a65-170">![SAML Single Sign On Settings](./media/active-directory-saas-insideview-tutorial/ic794135.png "SAML Single Sign On Settings")</span></span>

9. <span data-ttu-id="d3a65-171">I hello **lägga till en ny SAML** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d3a65-171">In hello **Add a New SAML** section, perform hello following steps:</span></span>

    <span data-ttu-id="d3a65-172">![Lägg till en ny SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "lägga till en ny SAML")</span><span class="sxs-lookup"><span data-stu-id="d3a65-172">![Add a New SAML](./media/active-directory-saas-insideview-tutorial/ic794136.png "Add a New SAML")</span></span>
   
    <span data-ttu-id="d3a65-173">a.</span><span class="sxs-lookup"><span data-stu-id="d3a65-173">a.</span></span> <span data-ttu-id="d3a65-174">I hello **STS Name** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d3a65-174">In hello **STS Name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="d3a65-175">b.</span><span class="sxs-lookup"><span data-stu-id="d3a65-175">b.</span></span> <span data-ttu-id="d3a65-176">I **SamlP/WS-Fed oönskade EndPoint** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d3a65-176">In **SamlP/WS-Fed Unsolicited EndPoint** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="d3a65-177">c.</span><span class="sxs-lookup"><span data-stu-id="d3a65-177">c.</span></span> <span data-ttu-id="d3a65-178">Öppna Base64-kodade certifikatet, som du har hämtat från Azure portal, kopiera hello innehållet i den till Urklipp och klistra in den toohello **STS Certificate** textruta.</span><span class="sxs-lookup"><span data-stu-id="d3a65-178">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **STS Certificate** textbox.</span></span>

    <span data-ttu-id="d3a65-179">d.</span><span class="sxs-lookup"><span data-stu-id="d3a65-179">d.</span></span> <span data-ttu-id="d3a65-180">I hello **Crm användar-Id-mappning** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="d3a65-180">In hello **Crm User Id Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
        
    <span data-ttu-id="d3a65-181">e.</span><span class="sxs-lookup"><span data-stu-id="d3a65-181">e.</span></span> <span data-ttu-id="d3a65-182">I hello **Crm e-mappning** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="d3a65-182">In hello **Crm Email Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="d3a65-183">f.</span><span class="sxs-lookup"><span data-stu-id="d3a65-183">f.</span></span> <span data-ttu-id="d3a65-184">I hello **Crm första mappning** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="d3a65-184">In hello **Crm First Name Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
    
    <span data-ttu-id="d3a65-185">g.</span><span class="sxs-lookup"><span data-stu-id="d3a65-185">g.</span></span> <span data-ttu-id="d3a65-186">I hello **Crm efternamn mappning** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="d3a65-186">In hello **Crm lastName Mapping** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>  

    <span data-ttu-id="d3a65-187">h.</span><span class="sxs-lookup"><span data-stu-id="d3a65-187">h.</span></span> <span data-ttu-id="d3a65-188">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-188">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d3a65-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d3a65-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d3a65-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d3a65-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d3a65-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d3a65-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d3a65-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3a65-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="d3a65-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d3a65-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d3a65-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d3a65-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3a65-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d3a65-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d3a65-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d3a65-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3a65-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d3a65-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d3a65-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-insideview-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d3a65-204">a.</span><span class="sxs-lookup"><span data-stu-id="d3a65-204">a.</span></span> <span data-ttu-id="d3a65-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d3a65-206">b.</span><span class="sxs-lookup"><span data-stu-id="d3a65-206">b.</span></span> <span data-ttu-id="d3a65-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d3a65-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d3a65-208">c.</span><span class="sxs-lookup"><span data-stu-id="d3a65-208">c.</span></span> <span data-ttu-id="d3a65-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d3a65-210">d.</span><span class="sxs-lookup"><span data-stu-id="d3a65-210">d.</span></span> <span data-ttu-id="d3a65-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-211">Click **Create**.</span></span>
 
### <a name="creating-a-insideview-test-user"></a><span data-ttu-id="d3a65-212">Skapa en testanvändare InsideView</span><span class="sxs-lookup"><span data-stu-id="d3a65-212">Creating a InsideView test user</span></span>

<span data-ttu-id="d3a65-213">tooenable Azure AD-användare toolog i tooInsideView, måste de vara etablerade i tooInsideView.</span><span class="sxs-lookup"><span data-stu-id="d3a65-213">tooenable Azure AD users toolog in tooInsideView, they must be provisioned in tooInsideView.</span></span> <span data-ttu-id="d3a65-214">Hello gäller InsideView är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d3a65-214">In hello case of InsideView, provisioning is a manual task.</span></span>

<span data-ttu-id="d3a65-215">tooget användare eller kontakter som skapats i InsideView, kontakta [InsideView supportteamet](mailto:support@insideview.com).</span><span class="sxs-lookup"><span data-stu-id="d3a65-215">tooget users or contacts created in InsideView, Contact [InsideView support team](mailto:support@insideview.com).</span></span>

>[!NOTE]
><span data-ttu-id="d3a65-216">Du kan använda något annat InsideView användarens konto skapas verktyg eller API: er som tillhandahålls av InsideView tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d3a65-216">You can use any other InsideView user account creation tools or APIs provided by InsideView tooprovision Azure AD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d3a65-217">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d3a65-217">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d3a65-218">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooInsideView.</span><span class="sxs-lookup"><span data-stu-id="d3a65-218">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInsideView.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d3a65-220">**tooassign Britta Simon tooInsideView utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d3a65-220">**tooassign Britta Simon tooInsideView, perform hello following steps:**</span></span>

1. <span data-ttu-id="d3a65-221">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-221">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d3a65-223">Välj i listan med program hello **InsideView**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-223">In hello applications list, select **InsideView**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-insideview-tutorial/tutorial_insideview_app.png) 

3. <span data-ttu-id="d3a65-225">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d3a65-225">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d3a65-227">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d3a65-227">Click **Add** button.</span></span> <span data-ttu-id="d3a65-228">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3a65-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d3a65-230">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d3a65-230">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d3a65-231">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3a65-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d3a65-232">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d3a65-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d3a65-233">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d3a65-233">Testing single sign-on</span></span>

<span data-ttu-id="d3a65-234">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d3a65-234">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d3a65-235">Du bör få automatiskt inloggade tooyour InsideView programmet när du klickar på hello InsideView panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d3a65-235">When you click hello InsideView tile in hello Access Panel, you should get automatically signed-on tooyour InsideView application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3a65-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d3a65-236">Additional resources</span></span>

* [<span data-ttu-id="d3a65-237">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d3a65-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d3a65-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d3a65-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insideview-tutorial/tutorial_general_203.png

