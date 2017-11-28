---
title: "Självstudier: Azure Active Directory-integrering med DocuSign | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: e4ef40b8f5af20d811d8d806d2bd7e2039c55052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a><span data-ttu-id="2e8b9-103">Självstudier: Azure Active Directory-integrering med DocuSign</span><span class="sxs-lookup"><span data-stu-id="2e8b9-103">Tutorial: Azure Active Directory integration with DocuSign</span></span>

<span data-ttu-id="2e8b9-104">I kursen får du lära dig hur toointegrate DocuSign med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2e8b9-104">In this tutorial, you learn how toointegrate DocuSign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2e8b9-105">Integrera DocuSign med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-105">Integrating DocuSign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2e8b9-106">Du kan styra i Azure AD som har åtkomst till tooDocuSign</span><span class="sxs-lookup"><span data-stu-id="2e8b9-106">You can control in Azure AD who has access tooDocuSign</span></span>
- <span data-ttu-id="2e8b9-107">Du kan aktivera din användare tooautomatically get inloggade tooDocuSign (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2e8b9-107">You can enable your users tooautomatically get signed-on tooDocuSign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2e8b9-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2e8b9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2e8b9-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2e8b9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e8b9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2e8b9-110">Prerequisites</span></span>

<span data-ttu-id="2e8b9-111">tooconfigure Azure AD-integrering med DocuSign, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-111">tooconfigure Azure AD integration with DocuSign, you need hello following items:</span></span>

- <span data-ttu-id="2e8b9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2e8b9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2e8b9-113">En DocuSign enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2e8b9-113">A DocuSign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2e8b9-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2e8b9-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2e8b9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2e8b9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2e8b9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2e8b9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2e8b9-118">Scenario description</span></span>
<span data-ttu-id="2e8b9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2e8b9-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2e8b9-121">Att lägga till DocuSign från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2e8b9-121">Adding DocuSign from hello gallery</span></span>
2. <span data-ttu-id="2e8b9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e8b9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-docusign-from-hello-gallery"></a><span data-ttu-id="2e8b9-123">Att lägga till DocuSign från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2e8b9-123">Adding DocuSign from hello gallery</span></span>
<span data-ttu-id="2e8b9-124">tooconfigure hello integrering av DocuSign i Azure AD, behöver du tooadd DocuSign hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-124">tooconfigure hello integration of DocuSign into Azure AD, you need tooadd DocuSign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2e8b9-125">**tooadd DocuSign från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2e8b9-125">**tooadd DocuSign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e8b9-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2e8b9-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2e8b9-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2e8b9-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2e8b9-133">Skriv i sökrutan hello **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-133">In hello search box, type **DocuSign**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. <span data-ttu-id="2e8b9-135">Markera hello resultat på panelen **DocuSign**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-135">In hello results panel, select **DocuSign**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2e8b9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e8b9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2e8b9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med DocuSign baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-138">In this section, you configure and test Azure AD single sign-on with DocuSign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2e8b9-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i DocuSign är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in DocuSign is tooa user in Azure AD.</span></span> <span data-ttu-id="2e8b9-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i DocuSign toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-140">In other words, a link relationship between an Azure AD user and hello related user in DocuSign needs toobe established.</span></span>

<span data-ttu-id="2e8b9-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i DocuSign.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in DocuSign.</span></span>

<span data-ttu-id="2e8b9-142">tooconfigure och testa Azure AD enkel inloggning med DocuSign, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-142">tooconfigure and test Azure AD single sign-on with DocuSign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2e8b9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2e8b9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2e8b9-145">**[Skapa en testanvändare DocuSign](#creating-a-docusign-test-user)**  -toohave en motsvarighet för Britta Simon i DocuSign som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-145">**[Creating a DocuSign test user](#creating-a-docusign-test-user)** - toohave a counterpart of Britta Simon in DocuSign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2e8b9-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2e8b9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2e8b9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e8b9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2e8b9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt DocuSign program.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your DocuSign application.</span></span>

<span data-ttu-id="2e8b9-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med DocuSign:**</span><span class="sxs-lookup"><span data-stu-id="2e8b9-150">**tooconfigure Azure AD single sign-on with DocuSign, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e8b9-151">I hello Azure-portalen på hello **DocuSign** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-151">In hello Azure portal, on hello **DocuSign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2e8b9-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. <span data-ttu-id="2e8b9-155">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base 64)** och spara certifikatet på datorn.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-155">On hello **SAML Signing Certificate** section, click **Certificate(Base 64)** and then save certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. <span data-ttu-id="2e8b9-157">På hello **DocuSign Configuration** på Azure portal, klicka på **konfigurera DocuSign** tooopen inloggning konfigurera fönster.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-157">On hello **DocuSign Configuration** section of Azure portal, Click **Configure DocuSign** tooopen Configure sign-on window.</span></span> <span data-ttu-id="2e8b9-158">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="2e8b9-158">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. <span data-ttu-id="2e8b9-160">I en annan webbläsarfönster inloggningen tooyour **DocuSign administrationsportalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-160">In a different web browser window, login tooyour **DocuSign admin portal** as an administrator.</span></span>

6. <span data-ttu-id="2e8b9-161">I hello navigeringsmenyn hello vänster klickar du på **domäner**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-161">In hello navigation menu on hello left, click **Domains**.</span></span>
   
    ![Konfigurera enkel inloggning][51]

7. <span data-ttu-id="2e8b9-163">Hello högra rutan, klicka på **anspråk domän**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-163">On hello right pane, click **Claim Domain**.</span></span>
   
    ![Konfigurera enkel inloggning][52]

8. <span data-ttu-id="2e8b9-165">På hello **anspråk en domän** dialogrutan i hello **domännamn** textruta Skriv företagets domän och klicka sedan på **anspråk**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-165">On hello **Claim a domain** dialog, in hello **Domain Name** textbox, type your company domain, and then click **Claim**.</span></span> <span data-ttu-id="2e8b9-166">Se till att du verifiera hello domän och hello status är aktiv.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-166">Make sure that you verify hello domain and hello status is active.</span></span>
   
    ![Konfigurera enkel inloggning][53]

9. <span data-ttu-id="2e8b9-168">Klicka på menyn hello vänster **identitetsleverantörer**</span><span class="sxs-lookup"><span data-stu-id="2e8b9-168">In menu on hello left side, click **Identity Providers**</span></span>  
   
    ![Konfigurera enkel inloggning][54]
10. <span data-ttu-id="2e8b9-170">I hello högra rutan, klickar du på **lägga till identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-170">In hello right pane, click **Add Identity Provider**.</span></span> 
   
    ![Konfigurera enkel inloggning][55]

11. <span data-ttu-id="2e8b9-172">På hello **identitet providerinställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-172">On hello **Identity Provider Settings** page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning][56]

    <span data-ttu-id="2e8b9-174">a.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-174">a.</span></span> <span data-ttu-id="2e8b9-175">I hello **namn** textruta, ange ett unikt namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-175">In hello **Name** textbox, type a unique name for your configuration.</span></span> <span data-ttu-id="2e8b9-176">Använd inte blanksteg.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-176">Do not use spaces.</span></span>

    <span data-ttu-id="2e8b9-177">b.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-177">b.</span></span> <span data-ttu-id="2e8b9-178">Klistra in **SAML enhets-ID** till hello **identitet providern utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-178">Paste **SAML Entity ID** into hello **Identity Provider Issuer** textbox.</span></span>

    <span data-ttu-id="2e8b9-179">c.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-179">c.</span></span> <span data-ttu-id="2e8b9-180">Klistra in **SAML enkel inloggning Tjänstwebbadress** till hello **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-180">Paste **SAML Single Sign-On Service URL** into hello **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="2e8b9-181">d.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-181">d.</span></span> <span data-ttu-id="2e8b9-182">Klistra in **Sign-Out URL** till hello **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-182">Paste **Sign-Out URL** into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="2e8b9-183">e.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-183">e.</span></span> <span data-ttu-id="2e8b9-184">Välj **logga AuthN begäran**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-184">Select **Sign AuthN Request**.</span></span>

    <span data-ttu-id="2e8b9-185">f.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-185">f.</span></span> <span data-ttu-id="2e8b9-186">Som **skicka AuthN-begäran från**väljer **efter**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-186">As **Send AuthN request by**, select **POST**.</span></span>

    <span data-ttu-id="2e8b9-187">g.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-187">g.</span></span> <span data-ttu-id="2e8b9-188">Som **skicka logga ut begäran från**väljer **hämta**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-188">As **Send logout request by**, select **GET**.</span></span>

12. <span data-ttu-id="2e8b9-189">I hello **anpassad attributmappning** hello fältet väljer du vill toomap med Azure AD-anspråk.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-189">In hello **Custom Attribute Mapping** section, choose hello field you want toomap with Azure AD Claim.</span></span> <span data-ttu-id="2e8b9-190">I det här exemplet hello **emailaddress** anspråk mappas med hello värdet **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-190">In this example, hello **emailaddress** claim is mapped with hello value of **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span> <span data-ttu-id="2e8b9-191">Det är hello anspråk för standardnamnet från Azure AD för e-anspråk.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-191">It is hello default claim name from Azure AD for email claim.</span></span> 
   
    > [!NOTE]
    > <span data-ttu-id="2e8b9-192">Använd hello lämpliga **användar-ID** toomap hello användaren från Azure AD tooDocuSign mappning.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-192">Use hello appropriate **User identifier** toomap hello user from Azure AD tooDocuSign user mapping.</span></span> <span data-ttu-id="2e8b9-193">Välj hello rätt fält och ange hello lämpligt värde baserat på dina organisationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-193">Select hello proper Field and enter hello appropriate value based on your organization settings.</span></span>
          
    ![Konfigurera enkel inloggning][57]

13. <span data-ttu-id="2e8b9-195">I hello **providern identitetscertifikat** klickar du på **Lägg till certifikat**, och sedan ladda upp hello-certifikat som du har hämtat från Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-195">In hello **Identity Provider Certificate** section, click **Add Certificate**, and then upload hello certificate you have downloaded from Azure AD portal.</span></span>   
   
    ![Konfigurera enkel inloggning][58]

14. <span data-ttu-id="2e8b9-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-197">Click **Save**.</span></span>

15. <span data-ttu-id="2e8b9-198">I hello **identitetsleverantörer** klickar du på **åtgärder**, och klicka sedan på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-198">In hello **Identity Providers** section, click **Actions**, and then click **Endpoints**.</span></span>   
   
    ![Konfigurera enkel inloggning][59]
 
16. <span data-ttu-id="2e8b9-200">I hello **visa SAML 2.0 slutpunkter** avsnittet på **DocuSign administrationsportal**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-200">In hello **View SAML 2.0 Endpoints** section on **DocuSign admin portal**, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning][60]
   
    <span data-ttu-id="2e8b9-202">a.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-202">a.</span></span> <span data-ttu-id="2e8b9-203">Kopiera hello **Service Provider utfärdar-URL**, och sedan klistra in i hello **identifierare** textruta på **DocuSign domän och URL: er** avsnitt i hello Azure portal följande hello mönster: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-203">Copy hello **Service Provider Issuer URL**, and then paste into hello **Identifier** textbox on **DocuSign Domain and URLs** section of hello Azure portal following hello pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span></span>
   
    <span data-ttu-id="2e8b9-204">b.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-204">b.</span></span> <span data-ttu-id="2e8b9-205">Kopiera hello **Service Provider inloggnings-URL**, och sedan klistra in i hello **logga URL** textruta på **DocuSign domän och URL: er** avsnitt i hello Azure portal följande hello mönster: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-205">Copy hello **Service Provider Login URL**, and then paste into hello **Sign On URL** textbox on **DocuSign Domain and URLs** section of hello Azure portal following hello pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    <span data-ttu-id="2e8b9-207">c.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-207">c.</span></span>  <span data-ttu-id="2e8b9-208">Klicka på **Stäng**</span><span class="sxs-lookup"><span data-stu-id="2e8b9-208">Click **Close**</span></span>
    
17. <span data-ttu-id="2e8b9-209">Klicka på hello Azure-portalen, **spara**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-209">On hello Azure portal, click **Save**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="2e8b9-211">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="2e8b9-211">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2e8b9-212">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-212">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2e8b9-213">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2e8b9-213">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2e8b9-214">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e8b9-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="2e8b9-215">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2e8b9-217">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2e8b9-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e8b9-218">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2e8b9-220">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-220">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2e8b9-222">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-222">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2e8b9-224">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2e8b9-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2e8b9-226">a.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-226">a.</span></span> <span data-ttu-id="2e8b9-227">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2e8b9-228">b.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-228">b.</span></span> <span data-ttu-id="2e8b9-229">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2e8b9-230">c.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-230">c.</span></span> <span data-ttu-id="2e8b9-231">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2e8b9-232">d.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-232">d.</span></span> <span data-ttu-id="2e8b9-233">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-233">Click **Create**.</span></span>
 
### <a name="creating-a-docusign-test-user"></a><span data-ttu-id="2e8b9-234">Skapa en testanvändare DocuSign</span><span class="sxs-lookup"><span data-stu-id="2e8b9-234">Creating a DocuSign test user</span></span>

<span data-ttu-id="2e8b9-235">Programmet stöder **precis i tid användaretablering** och efter autentisering användare skapas i hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-235">Application supports **Just in time user provisioning** and after authentication users are created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2e8b9-236">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2e8b9-236">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2e8b9-237">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooDocuSign sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-237">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooDocuSign.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2e8b9-239">**tooassign Britta Simon tooDocuSign utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2e8b9-239">**tooassign Britta Simon tooDocuSign, perform hello following steps:**</span></span>

1. <span data-ttu-id="2e8b9-240">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-240">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2e8b9-242">Välj i listan med program hello **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-242">In hello applications list, select **DocuSign**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. <span data-ttu-id="2e8b9-244">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-244">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2e8b9-246">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-246">Click **Add** button.</span></span> <span data-ttu-id="2e8b9-247">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2e8b9-249">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-249">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2e8b9-250">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2e8b9-251">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2e8b9-252">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2e8b9-252">Testing single sign-on</span></span>

<span data-ttu-id="2e8b9-253">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-253">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2e8b9-254">Du bör få automatiskt inloggade tooyour DocuSign programmet när du klickar på hello DocuSign panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2e8b9-254">When you click hello DocuSign tile in hello Access Panel, you should get automatically signed-on tooyour DocuSign application.</span></span>
<span data-ttu-id="2e8b9-255">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2e8b9-255">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2e8b9-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2e8b9-256">Additional resources</span></span>

* [<span data-ttu-id="2e8b9-257">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2e8b9-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2e8b9-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2e8b9-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="2e8b9-259">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="2e8b9-259">Configure User Provisioning</span></span>](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

