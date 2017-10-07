---
title: "Självstudier: Azure Active Directory-integrering med LCVista | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LCVista."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8db80d6e-3275-419f-aa39-6115a7bc9800
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: 5a4c7eb7d54b8b68c8241a97b9e516a3f6e55c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lcvista"></a><span data-ttu-id="9503d-103">Självstudier: Azure Active Directory-integrering med LCVista</span><span class="sxs-lookup"><span data-stu-id="9503d-103">Tutorial: Azure Active Directory integration with LCVista</span></span>

<span data-ttu-id="9503d-104">I kursen får du lära dig hur toointegrate LCVista med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9503d-104">In this tutorial, you learn how toointegrate LCVista with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9503d-105">Integrera LCVista med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9503d-105">Integrating LCVista with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9503d-106">Du kan styra i Azure AD som har åtkomst till tooLCVista</span><span class="sxs-lookup"><span data-stu-id="9503d-106">You can control in Azure AD who has access tooLCVista</span></span>
- <span data-ttu-id="9503d-107">Du kan aktivera din användare tooautomatically get inloggade tooLCVista (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9503d-107">You can enable your users tooautomatically get signed-on tooLCVista (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9503d-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9503d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="9503d-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9503d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9503d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9503d-110">Prerequisites</span></span>

<span data-ttu-id="9503d-111">tooconfigure Azure AD-integrering med LCVista, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9503d-111">tooconfigure Azure AD integration with LCVista, you need hello following items:</span></span>

- <span data-ttu-id="9503d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9503d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9503d-113">En LCVista enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="9503d-113">A LCVista single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9503d-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9503d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9503d-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9503d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9503d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9503d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9503d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9503d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9503d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9503d-118">Scenario description</span></span>
<span data-ttu-id="9503d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9503d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9503d-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9503d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9503d-121">Att lägga till LCVista från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9503d-121">Adding LCVista from hello gallery</span></span>
2. <span data-ttu-id="9503d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9503d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lcvista-from-hello-gallery"></a><span data-ttu-id="9503d-123">Att lägga till LCVista från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9503d-123">Adding LCVista from hello gallery</span></span>
<span data-ttu-id="9503d-124">tooconfigure hello integrering av LCVista i Azure AD, behöver du tooadd LCVista hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9503d-124">tooconfigure hello integration of LCVista into Azure AD, you need tooadd LCVista from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9503d-125">**tooadd LCVista från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9503d-125">**tooadd LCVista from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9503d-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9503d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9503d-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9503d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9503d-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9503d-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9503d-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9503d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9503d-133">Skriv i sökrutan hello **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="9503d-133">In hello search box, type **LCVista**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_search.png)

5. <span data-ttu-id="9503d-135">Markera hello resultat på panelen **LCVista**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="9503d-135">In hello results panel, select **LCVista**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9503d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9503d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9503d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LCVista baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9503d-138">In this section, you configure and test Azure AD single sign-on with LCVista based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9503d-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LCVista är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9503d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LCVista is tooa user in Azure AD.</span></span> <span data-ttu-id="9503d-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LCVista toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9503d-140">In other words, a link relationship between an Azure AD user and hello related user in LCVista needs toobe established.</span></span>

<span data-ttu-id="9503d-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i LCVista.</span><span class="sxs-lookup"><span data-stu-id="9503d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in LCVista.</span></span>

<span data-ttu-id="9503d-142">tooconfigure och testa Azure AD enkel inloggning med LCVista, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9503d-142">tooconfigure and test Azure AD single sign-on with LCVista, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9503d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9503d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9503d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9503d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9503d-145">**[Skapa en testanvändare LCVista](#creating-a-lcvista-test-user)**  -toohave en motsvarighet för Britta Simon i LCVista som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9503d-145">**[Creating a LCVista test user](#creating-a-lcvista-test-user)** - toohave a counterpart of Britta Simon in LCVista that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9503d-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9503d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9503d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9503d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9503d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9503d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9503d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LCVista program.</span><span class="sxs-lookup"><span data-stu-id="9503d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LCVista application.</span></span>

<span data-ttu-id="9503d-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LCVista:**</span><span class="sxs-lookup"><span data-stu-id="9503d-150">**tooconfigure Azure AD single sign-on with LCVista, perform hello following steps:**</span></span>

1. <span data-ttu-id="9503d-151">I hello Azure-portalen på hello **LCVista** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9503d-151">In hello Azure portal, on hello **LCVista** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9503d-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9503d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_samlbase.png)

3. <span data-ttu-id="9503d-155">På hello **LCVista domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9503d-155">On hello **LCVista Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_url.png)

    <span data-ttu-id="9503d-157">a.</span><span class="sxs-lookup"><span data-stu-id="9503d-157">a.</span></span> <span data-ttu-id="9503d-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.lcvista.com/rainier/login`</span><span class="sxs-lookup"><span data-stu-id="9503d-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.lcvista.com/rainier/login`</span></span>

    <span data-ttu-id="9503d-159">b.</span><span class="sxs-lookup"><span data-stu-id="9503d-159">b.</span></span> <span data-ttu-id="9503d-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.lcvista.com`</span><span class="sxs-lookup"><span data-stu-id="9503d-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.lcvista.com`</span></span> 
     
    > [!NOTE] 
    > <span data-ttu-id="9503d-161">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="9503d-161">These values are not hello real.</span></span> <span data-ttu-id="9503d-162">Uppdatera dessa värden med hello faktiska identifierare och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="9503d-162">Update these values with hello actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="9503d-163">Kontakta [LCVista klienten supportteamet](https://lcvista.com/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9503d-163">Contact [LCVista Client support team](https://lcvista.com/contact) tooget these values.</span></span> 

4. <span data-ttu-id="9503d-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="9503d-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_certificate.png) 

5. <span data-ttu-id="9503d-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9503d-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="9503d-168">På hello **LCVista Configuration** klickar du på **konfigurera LCVista** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9503d-168">On hello **LCVista Configuration** section, click **Configure LCVista** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="9503d-169">Kopiera hello **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="9503d-169">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_configure.png) 

7.  <span data-ttu-id="9503d-171">Logga in tooyour LCVista programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="9503d-171">Sign on tooyour LCVista application as an administrator.</span></span>

8. <span data-ttu-id="9503d-172">I hello **SAML-Config** avsnittet markerar hello **aktivera SAML inloggningen** och ange hello information som anges i under bilden.</span><span class="sxs-lookup"><span data-stu-id="9503d-172">In hello **SAML Config** section, check hello **Enable SAML login** and enter hello details as mentioned in below image.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_config.png)

    <span data-ttu-id="9503d-174">a.</span><span class="sxs-lookup"><span data-stu-id="9503d-174">a.</span></span> <span data-ttu-id="9503d-175">Klistra in hello **utfärdar-URL** som du har kopierat från Azure AD i hello **enhets-ID** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9503d-175">Paste hello **Issuer URL** which you have copied from Azure AD in hello **Entity ID** section.</span></span> 

    <span data-ttu-id="9503d-176">b.</span><span class="sxs-lookup"><span data-stu-id="9503d-176">b.</span></span> <span data-ttu-id="9503d-177">Klistra in hello **inloggning tjänst-URL för enkel** som du har kopierat från Azure AD i hello **URL** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9503d-177">Paste hello **Single Sign-On Service URL** which you have copied from Azure AD in hello **URL** section.</span></span>

    <span data-ttu-id="9503d-178">c.</span><span class="sxs-lookup"><span data-stu-id="9503d-178">c.</span></span> <span data-ttu-id="9503d-179">Från Metadata (XML) som du har hämtat från Azure-portalen, kopiera hello värde **X509Certificate** och klistra in den i hello **x509 certifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9503d-179">From Metadata (XML) which you have downloaded from Azure portal, copy hello value **X509Certificate** and paste it in hello **x509 Certificate** section.</span></span>

    <span data-ttu-id="9503d-180">d.</span><span class="sxs-lookup"><span data-stu-id="9503d-180">d.</span></span> <span data-ttu-id="9503d-181">I hello **förnamn attributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="9503d-181">In hello **First name attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>

    <span data-ttu-id="9503d-182">e.</span><span class="sxs-lookup"><span data-stu-id="9503d-182">e.</span></span> <span data-ttu-id="9503d-183">I hello **senaste namnattributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="9503d-183">In hello **Last name attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>

    <span data-ttu-id="9503d-184">f.</span><span class="sxs-lookup"><span data-stu-id="9503d-184">f.</span></span> <span data-ttu-id="9503d-185">I hello **e-attributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="9503d-185">In hello **Email attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="9503d-186">g.</span><span class="sxs-lookup"><span data-stu-id="9503d-186">g.</span></span> <span data-ttu-id="9503d-187">I hello **Username-attributet** textruta klistra in hello värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="9503d-187">In hello **Username attribute** textbox, paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>

    <span data-ttu-id="9503d-188">e.</span><span class="sxs-lookup"><span data-stu-id="9503d-188">e.</span></span> <span data-ttu-id="9503d-189">Klicka på **spara** toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="9503d-189">Click **Save** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="9503d-190">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9503d-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9503d-191">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9503d-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9503d-192">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9503d-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9503d-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9503d-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="9503d-194">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9503d-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9503d-196">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9503d-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9503d-197">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9503d-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9503d-199">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9503d-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9503d-201">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9503d-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9503d-203">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9503d-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9503d-205">a.</span><span class="sxs-lookup"><span data-stu-id="9503d-205">a.</span></span> <span data-ttu-id="9503d-206">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9503d-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9503d-207">b.</span><span class="sxs-lookup"><span data-stu-id="9503d-207">b.</span></span> <span data-ttu-id="9503d-208">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9503d-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9503d-209">c.</span><span class="sxs-lookup"><span data-stu-id="9503d-209">c.</span></span> <span data-ttu-id="9503d-210">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9503d-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="9503d-211">d.</span><span class="sxs-lookup"><span data-stu-id="9503d-211">d.</span></span> <span data-ttu-id="9503d-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9503d-212">Click **Create**.</span></span>
 
### <a name="creating-a-lcvista-test-user"></a><span data-ttu-id="9503d-213">Skapa en testanvändare LCVista</span><span class="sxs-lookup"><span data-stu-id="9503d-213">Creating a LCVista test user</span></span>

<span data-ttu-id="9503d-214">I det här avsnittet skapar du en användare som kallas Britta Simon i LCVista.</span><span class="sxs-lookup"><span data-stu-id="9503d-214">In this section, you create a user called Britta Simon in LCVista.</span></span> <span data-ttu-id="9503d-215">Du behöver toocontact [LCVista klienten supportteamet](https://lcvista.com/contact) tooadd hello användare i hello LCVista program.</span><span class="sxs-lookup"><span data-stu-id="9503d-215">You need toocontact [LCVista Client support team](https://lcvista.com/contact) tooadd hello users in hello LCVista application.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="9503d-216">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9503d-216">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="9503d-217">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLCVista.</span><span class="sxs-lookup"><span data-stu-id="9503d-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLCVista.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9503d-219">**tooassign Britta Simon tooLCVista utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9503d-219">**tooassign Britta Simon tooLCVista, perform hello following steps:**</span></span>

1. <span data-ttu-id="9503d-220">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9503d-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9503d-222">Välj i listan med program hello **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="9503d-222">In hello applications list, select **LCVista**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_app.png) 

3. <span data-ttu-id="9503d-224">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9503d-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9503d-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9503d-226">Click **Add** button.</span></span> <span data-ttu-id="9503d-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9503d-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9503d-229">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9503d-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9503d-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9503d-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9503d-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9503d-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9503d-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9503d-232">Testing single sign-on</span></span>

<span data-ttu-id="9503d-233">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9503d-233">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span> <span data-ttu-id="9503d-234">Klicka på hello LCVista panelen i hello åtkomstpanelen, kommer du att omdirigeras tooOrganization logga in på sidan.</span><span class="sxs-lookup"><span data-stu-id="9503d-234">Click hello LCVista tile in hello Access Panel, you will be redirected tooOrganization sign on page.</span></span> <span data-ttu-id="9503d-235">Du kommer att inloggade tooyour LCVista program efter genomförd inloggning.</span><span class="sxs-lookup"><span data-stu-id="9503d-235">After successful login, you will be signed-on tooyour LCVista application.</span></span> <span data-ttu-id="9503d-236">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="9503d-236">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9503d-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9503d-237">Additional resources</span></span>

* [<span data-ttu-id="9503d-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9503d-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9503d-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9503d-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_203.png

