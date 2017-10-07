---
title: "Självstudier: Azure Active Directory-integrering med LearnUpon | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LearnUpon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: fdb9c62172327a539f0459c98aa20e63fa441e4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a><span data-ttu-id="faf66-103">Självstudier: Azure Active Directory-integrering med LearnUpon</span><span class="sxs-lookup"><span data-stu-id="faf66-103">Tutorial: Azure Active Directory integration with LearnUpon</span></span>

<span data-ttu-id="faf66-104">I kursen får du lära dig hur toointegrate LearnUpon med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="faf66-104">In this tutorial, you learn how toointegrate LearnUpon with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="faf66-105">Integrera LearnUpon med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="faf66-105">Integrating LearnUpon with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="faf66-106">Du kan styra i Azure AD som har åtkomst till tooLearnUpon</span><span class="sxs-lookup"><span data-stu-id="faf66-106">You can control in Azure AD who has access tooLearnUpon</span></span>
- <span data-ttu-id="faf66-107">Du kan aktivera din användare tooautomatically get inloggade tooLearnUpon (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="faf66-107">You can enable your users tooautomatically get signed-on tooLearnUpon (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="faf66-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="faf66-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="faf66-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="faf66-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="faf66-110">Krav</span><span class="sxs-lookup"><span data-stu-id="faf66-110">Prerequisites</span></span>

<span data-ttu-id="faf66-111">tooconfigure Azure AD-integrering med LearnUpon, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="faf66-111">tooconfigure Azure AD integration with LearnUpon, you need hello following items:</span></span>

- <span data-ttu-id="faf66-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="faf66-112">An Azure AD subscription</span></span>
- <span data-ttu-id="faf66-113">En LearnUpon enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="faf66-113">A LearnUpon single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="faf66-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="faf66-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="faf66-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="faf66-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="faf66-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="faf66-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="faf66-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="faf66-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="faf66-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="faf66-118">Scenario description</span></span>
<span data-ttu-id="faf66-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="faf66-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="faf66-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="faf66-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="faf66-121">Att lägga till LearnUpon från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="faf66-121">Adding LearnUpon from hello gallery</span></span>
2. <span data-ttu-id="faf66-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="faf66-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learnupon-from-hello-gallery"></a><span data-ttu-id="faf66-123">Att lägga till LearnUpon från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="faf66-123">Adding LearnUpon from hello gallery</span></span>
<span data-ttu-id="faf66-124">tooconfigure hello integrering av LearnUpon i Azure AD, behöver du tooadd LearnUpon hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="faf66-124">tooconfigure hello integration of LearnUpon into Azure AD, you need tooadd LearnUpon from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="faf66-125">**tooadd LearnUpon från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="faf66-125">**tooadd LearnUpon from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="faf66-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="faf66-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="faf66-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="faf66-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="faf66-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="faf66-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="faf66-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="faf66-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="faf66-133">Skriv i sökrutan hello **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="faf66-133">In hello search box, type **LearnUpon**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. <span data-ttu-id="faf66-135">Markera hello resultat på panelen **LearnUpon**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="faf66-135">In hello results panel, select **LearnUpon**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="faf66-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="faf66-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="faf66-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LearnUpon baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="faf66-138">In this section, you configure and test Azure AD single sign-on with LearnUpon based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="faf66-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LearnUpon är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="faf66-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LearnUpon is tooa user in Azure AD.</span></span> <span data-ttu-id="faf66-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LearnUpon toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="faf66-140">In other words, a link relationship between an Azure AD user and hello related user in LearnUpon needs toobe established.</span></span>

<span data-ttu-id="faf66-141">I LearnUpon, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="faf66-141">In LearnUpon, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="faf66-142">tooconfigure och testa Azure AD enkel inloggning med LearnUpon, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="faf66-142">tooconfigure and test Azure AD single sign-on with LearnUpon, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="faf66-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="faf66-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="faf66-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="faf66-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="faf66-145">**[Skapa en testanvändare LearnUpon](#creating-a-learnupon-test-user)**  -toohave en motsvarighet för Britta Simon i LearnUpon som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="faf66-145">**[Creating a LearnUpon test user](#creating-a-learnupon-test-user)** - toohave a counterpart of Britta Simon in LearnUpon that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="faf66-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="faf66-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="faf66-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="faf66-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="faf66-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="faf66-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="faf66-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LearnUpon program.</span><span class="sxs-lookup"><span data-stu-id="faf66-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LearnUpon application.</span></span>

<span data-ttu-id="faf66-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LearnUpon:**</span><span class="sxs-lookup"><span data-stu-id="faf66-150">**tooconfigure Azure AD single sign-on with LearnUpon, perform hello following steps:**</span></span>

1. <span data-ttu-id="faf66-151">I hello Azure-portalen på hello **LearnUpon** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="faf66-151">In hello Azure portal, on hello **LearnUpon** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="faf66-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="faf66-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. <span data-ttu-id="faf66-155">På hello **LearnUpon domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="faf66-155">On hello **LearnUpon Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    <span data-ttu-id="faf66-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.learnupon.com/saml/consumer`</span><span class="sxs-lookup"><span data-stu-id="faf66-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.learnupon.com/saml/consumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="faf66-158">Observera att detta inte är hello verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="faf66-158">Please note that this is not hello real value.</span></span> <span data-ttu-id="faf66-159">du har tooupdate värdet med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="faf66-159">you have tooupdate this value with hello actual Reply URL.</span></span> <span data-ttu-id="faf66-160">tooget värdet Kontakta [LearnUpon supportteamet](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="faf66-160">tooget this value Contact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span>



4. <span data-ttu-id="faf66-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="faf66-161">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. <span data-ttu-id="faf66-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="faf66-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="faf66-165">På hello **LearnUpon Configuration** klickar du på **konfigurera LearnUpon** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="faf66-165">On hello **LearnUpon Configuration** section, click **Configure LearnUpon** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="faf66-166">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="faf66-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. <span data-ttu-id="faf66-168">Öppna en annan instans för webbläsaren och logga in till LearnUpon med ett administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="faf66-168">Open another browser instance and login into LearnUpon with an administrator account.</span></span> 

8. <span data-ttu-id="faf66-169">Klicka på hello **inställningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="faf66-169">Click hello **settings** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. <span data-ttu-id="faf66-171">Klicka på **Single Sign On - SAML**, och klicka sedan på **allmänna inställningar** tooconfigure SAML-inställningar.</span><span class="sxs-lookup"><span data-stu-id="faf66-171">Click **Single Sign On - SAML**, and then click **General Settings** tooconfigure SAML settings.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. <span data-ttu-id="faf66-173">I hello **allmänna inställningar** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="faf66-173">In hello **General Settings** section, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    <span data-ttu-id="faf66-175">a.</span><span class="sxs-lookup"><span data-stu-id="faf66-175">a.</span></span> <span data-ttu-id="faf66-176">Välj **aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="faf66-176">Select **Enabled**.</span></span>

    <span data-ttu-id="faf66-177">b.</span><span class="sxs-lookup"><span data-stu-id="faf66-177">b.</span></span> <span data-ttu-id="faf66-178">Välj **Version** som **2.0**.</span><span class="sxs-lookup"><span data-stu-id="faf66-178">Select **Version** as **2.0**.</span></span>

    <span data-ttu-id="faf66-179">c.</span><span class="sxs-lookup"><span data-stu-id="faf66-179">c.</span></span> <span data-ttu-id="faf66-180">Välj **hoppa över villkor** som **nr**.</span><span class="sxs-lookup"><span data-stu-id="faf66-180">Select **Skip conditions** as **No**.</span></span>

    <span data-ttu-id="faf66-181">d.</span><span class="sxs-lookup"><span data-stu-id="faf66-181">d.</span></span> <span data-ttu-id="faf66-182">I hello **SAML-Token efter param namnet** textruta hello-typnamn för begäran post parametern toohello SAML konsument-URL som nämns ovan som innehåller hello SAML Assertion toobe verifieras och autentiserad – till exempel  **SAMLResponse**.</span><span class="sxs-lookup"><span data-stu-id="faf66-182">In hello **SAML Token Post param name** textbox, type hello name of request post parameter toohello SAML consumer URL indicated above that contains hello SAML Assertion toobe verified and authenticated - for example **SAMLResponse**.</span></span>

    <span data-ttu-id="faf66-183">e.</span><span class="sxs-lookup"><span data-stu-id="faf66-183">e.</span></span> <span data-ttu-id="faf66-184">I hello **identifierare namnformat** textruta typen hello-värde som anger där användarna SAML Assertion hello identifierare (e-postadress) finns – till exempel **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="faf66-184">In hello **Name Identifier Format** textbox, type hello value that indicates where in your SAML Assertion hello users identifier (Email address) resides - for example **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>
  
    <span data-ttu-id="faf66-185">f.</span><span class="sxs-lookup"><span data-stu-id="faf66-185">f.</span></span> <span data-ttu-id="faf66-186">I hello **identifiera leverantörsplatsen** textruta, typen hello-värde som anger om hello användare skickas tooif de Klicka på din överförda ikon från Azure portal inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="faf66-186">In hello **Identify Provider Location** textbox, type hello value that indicates where hello users are sent tooif they click on your uploaded icon from your Azure portal login screen.</span></span>
  
    <span data-ttu-id="faf66-187">g.</span><span class="sxs-lookup"><span data-stu-id="faf66-187">g.</span></span> <span data-ttu-id="faf66-188">I hello **logga ut URL** textruta klistra in hello **Sign-Out URL** som du har kopierat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="faf66-188">In hello **Sign out URL** textbox, paste hello **Sign-Out URL** which you have copied from hello Azure portal.</span></span>
    
    <span data-ttu-id="faf66-189">h.</span><span class="sxs-lookup"><span data-stu-id="faf66-189">h.</span></span> <span data-ttu-id="faf66-190">Klicka på **hantera fingeravtrycksläsare utskrifter**, och sedan ladda upp hello fingeravtryck av hämtade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="faf66-190">Click **Manage finger prints**, and then upload hello finger print of your downloaded certificate.</span></span>

11. <span data-ttu-id="faf66-191">Klicka på **användarinställningar**, och utför sedan hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="faf66-191">Click **User Settings**, and then perform hello following steps:</span></span>
   
     ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    <span data-ttu-id="faf66-193">a.</span><span class="sxs-lookup"><span data-stu-id="faf66-193">a.</span></span> <span data-ttu-id="faf66-194">I hello **förnamn identifierare Format** textruta hello TYPVÄRDE som talar om för oss var i din SAML Assertion hello användare firstname finns – till exempel: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span><span class="sxs-lookup"><span data-stu-id="faf66-194">In hello **First Name Identifier Format** textbox, type hello value that tells us where in your SAML Assertion hello users firstname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.</span></span>
  
    <span data-ttu-id="faf66-195">b.</span><span class="sxs-lookup"><span data-stu-id="faf66-195">b.</span></span> <span data-ttu-id="faf66-196">I hello **senaste namnformat identifierare** textruta hello TYPVÄRDE som talar om för oss var i din SAML Assertion hello användare efternamn finns – till exempel: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span><span class="sxs-lookup"><span data-stu-id="faf66-196">In hello **Last Name Identifier Format** textbox, type hello value that tells us where in your SAML Assertion hello users lastname resides - for example: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.</span></span>

> [!TIP]
> <span data-ttu-id="faf66-197">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="faf66-197">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="faf66-198">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="faf66-198">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="faf66-199">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="faf66-199">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="faf66-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="faf66-200">Creating an Azure AD test user</span></span>
<span data-ttu-id="faf66-201">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="faf66-201">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="faf66-203">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="faf66-203">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="faf66-204">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="faf66-204">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="faf66-206">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="faf66-206">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="faf66-208">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="faf66-208">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="faf66-210">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="faf66-210">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="faf66-212">a.</span><span class="sxs-lookup"><span data-stu-id="faf66-212">a.</span></span> <span data-ttu-id="faf66-213">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="faf66-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="faf66-214">b.</span><span class="sxs-lookup"><span data-stu-id="faf66-214">b.</span></span> <span data-ttu-id="faf66-215">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="faf66-215">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="faf66-216">c.</span><span class="sxs-lookup"><span data-stu-id="faf66-216">c.</span></span> <span data-ttu-id="faf66-217">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="faf66-217">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="faf66-218">d.</span><span class="sxs-lookup"><span data-stu-id="faf66-218">d.</span></span> <span data-ttu-id="faf66-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="faf66-219">Click **Create**.</span></span>
 
### <a name="creating-a-learnupon-test-user"></a><span data-ttu-id="faf66-220">Skapa en testanvändare LearnUpon</span><span class="sxs-lookup"><span data-stu-id="faf66-220">Creating a LearnUpon test user</span></span>

<span data-ttu-id="faf66-221">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i LearnUpon.</span><span class="sxs-lookup"><span data-stu-id="faf66-221">hello objective of this section is toocreate a user called Britta Simon in LearnUpon.</span></span> <span data-ttu-id="faf66-222">LearnUpon stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="faf66-222">LearnUpon supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="faf66-223">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="faf66-223">There is no action item for you in this section.</span></span> <span data-ttu-id="faf66-224">En ny användare skapas under ett försök tooaccess LearnUpon om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="faf66-224">A new user will be created during an attempt tooaccess LearnUpon if it doesn't exist yet.</span></span> <span data-ttu-id="faf66-225">[Konfigurera Azure AD-Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="faf66-225">[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="faf66-226">Om du behöver toocreate en användare manuellt, måste toocontact [LearnUpon supportteamet](https://www.learnupon.com/features/support/).</span><span class="sxs-lookup"><span data-stu-id="faf66-226">If you need toocreate an user manually, you need toocontact [LearnUpon support team](https://www.learnupon.com/features/support/).</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="faf66-227">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="faf66-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="faf66-228">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLearnUpon.</span><span class="sxs-lookup"><span data-stu-id="faf66-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLearnUpon.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="faf66-230">**tooassign Britta Simon tooLearnUpon utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="faf66-230">**tooassign Britta Simon tooLearnUpon, perform hello following steps:**</span></span>

1. <span data-ttu-id="faf66-231">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="faf66-231">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="faf66-233">Välj i listan med program hello **LearnUpon**.</span><span class="sxs-lookup"><span data-stu-id="faf66-233">In hello applications list, select **LearnUpon**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. <span data-ttu-id="faf66-235">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="faf66-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="faf66-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="faf66-237">Click **Add** button.</span></span> <span data-ttu-id="faf66-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="faf66-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="faf66-240">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="faf66-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="faf66-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="faf66-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="faf66-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="faf66-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="faf66-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="faf66-243">Testing single sign-on</span></span>

<span data-ttu-id="faf66-244">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="faf66-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="faf66-245">Du bör få automatiskt inloggade tooyour LearnUpon programmet när du klickar på hello LearnUpon panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="faf66-245">When you click hello LearnUpon tile in hello Access Panel, you should get automatically signed-on tooyour LearnUpon application.</span></span>
<span data-ttu-id="faf66-246">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="faf66-246">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="faf66-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="faf66-247">Additional resources</span></span>

* [<span data-ttu-id="faf66-248">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="faf66-248">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="faf66-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="faf66-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png

