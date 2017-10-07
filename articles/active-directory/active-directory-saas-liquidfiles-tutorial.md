---
title: "Självstudier: Azure Active Directory-integrering med LiquidFiles | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och LiquidFiles."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cb517134-0b34-4a74-b40c-5a3223ca81b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 67eb888090f81e0ceb791ed45d564b98fe1eb6d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-liquidfiles"></a><span data-ttu-id="12c9f-103">Självstudier: Azure Active Directory-integrering med LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="12c9f-103">Tutorial: Azure Active Directory integration with LiquidFiles</span></span>

<span data-ttu-id="12c9f-104">I kursen får du lära dig hur toointegrate LiquidFiles med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="12c9f-104">In this tutorial, you learn how toointegrate LiquidFiles with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="12c9f-105">Integrera LiquidFiles med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="12c9f-105">Integrating LiquidFiles with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="12c9f-106">Du kan styra i Azure AD som har åtkomst till tooLiquidFiles</span><span class="sxs-lookup"><span data-stu-id="12c9f-106">You can control in Azure AD who has access tooLiquidFiles</span></span>
- <span data-ttu-id="12c9f-107">Du kan aktivera din användare tooautomatically get inloggade tooLiquidFiles (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="12c9f-107">You can enable your users tooautomatically get signed-on tooLiquidFiles (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="12c9f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="12c9f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="12c9f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="12c9f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12c9f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="12c9f-110">Prerequisites</span></span>

<span data-ttu-id="12c9f-111">tooconfigure Azure AD-integrering med LiquidFiles, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="12c9f-111">tooconfigure Azure AD integration with LiquidFiles, you need hello following items:</span></span>

- <span data-ttu-id="12c9f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="12c9f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="12c9f-113">En LiquidFiles enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="12c9f-113">A LiquidFiles single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="12c9f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="12c9f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="12c9f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="12c9f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="12c9f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="12c9f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="12c9f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="12c9f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="12c9f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="12c9f-118">Scenario description</span></span>
<span data-ttu-id="12c9f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="12c9f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="12c9f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="12c9f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="12c9f-121">Att lägga till LiquidFiles från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="12c9f-121">Adding LiquidFiles from hello gallery</span></span>
2. <span data-ttu-id="12c9f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="12c9f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-liquidfiles-from-hello-gallery"></a><span data-ttu-id="12c9f-123">Att lägga till LiquidFiles från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="12c9f-123">Adding LiquidFiles from hello gallery</span></span>
<span data-ttu-id="12c9f-124">tooconfigure hello integrering av LiquidFiles i Azure AD, behöver du tooadd LiquidFiles hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="12c9f-124">tooconfigure hello integration of LiquidFiles into Azure AD, you need tooadd LiquidFiles from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="12c9f-125">**tooadd LiquidFiles från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="12c9f-125">**tooadd LiquidFiles from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="12c9f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="12c9f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="12c9f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="12c9f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="12c9f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="12c9f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="12c9f-133">Skriv i sökrutan hello **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-133">In hello search box, type **LiquidFiles**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_search.png)

5. <span data-ttu-id="12c9f-135">Markera hello resultat på panelen **LiquidFiles**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="12c9f-135">In hello results panel, select **LiquidFiles**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="12c9f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="12c9f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="12c9f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LiquidFiles baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="12c9f-138">In this section, you configure and test Azure AD single sign-on with LiquidFiles based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="12c9f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i LiquidFiles är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12c9f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in LiquidFiles is tooa user in Azure AD.</span></span> <span data-ttu-id="12c9f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i LiquidFiles toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="12c9f-140">In other words, a link relationship between an Azure AD user and hello related user in LiquidFiles needs toobe established.</span></span>

<span data-ttu-id="12c9f-141">I LiquidFiles, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-141">In LiquidFiles, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="12c9f-142">tooconfigure och testa Azure AD enkel inloggning med LiquidFiles, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="12c9f-142">tooconfigure and test Azure AD single sign-on with LiquidFiles, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="12c9f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="12c9f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="12c9f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="12c9f-145">**[Skapa en testanvändare LiquidFiles](#creating-a-liquidfiles-test-user)**  -toohave en motsvarighet för Britta Simon i LiquidFiles som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="12c9f-145">**[Creating a LiquidFiles test user](#creating-a-liquidfiles-test-user)** - toohave a counterpart of Britta Simon in LiquidFiles that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="12c9f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="12c9f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="12c9f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="12c9f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="12c9f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="12c9f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="12c9f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt LiquidFiles program.</span><span class="sxs-lookup"><span data-stu-id="12c9f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your LiquidFiles application.</span></span>

<span data-ttu-id="12c9f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med LiquidFiles:**</span><span class="sxs-lookup"><span data-stu-id="12c9f-150">**tooconfigure Azure AD single sign-on with LiquidFiles, perform hello following steps:**</span></span>

1. <span data-ttu-id="12c9f-151">I hello Azure-portalen på hello **LiquidFiles** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-151">In hello Azure portal, on hello **LiquidFiles** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="12c9f-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="12c9f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_samlbase.png)

3. <span data-ttu-id="12c9f-155">På hello **LiquidFiles domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="12c9f-155">On hello **LiquidFiles Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_url.png)

    <span data-ttu-id="12c9f-157">a.</span><span class="sxs-lookup"><span data-stu-id="12c9f-157">a.</span></span> <span data-ttu-id="12c9f-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<YOUR_SERVER_URL>/saml/init`</span><span class="sxs-lookup"><span data-stu-id="12c9f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>/saml/init`</span></span>

    <span data-ttu-id="12c9f-159">b.</span><span class="sxs-lookup"><span data-stu-id="12c9f-159">b.</span></span> <span data-ttu-id="12c9f-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<YOUR_SERVER_URL>`</span><span class="sxs-lookup"><span data-stu-id="12c9f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>`</span></span>

    <span data-ttu-id="12c9f-161">c.</span><span class="sxs-lookup"><span data-stu-id="12c9f-161">c.</span></span> <span data-ttu-id="12c9f-162">b.</span><span class="sxs-lookup"><span data-stu-id="12c9f-162">b.</span></span> <span data-ttu-id="12c9f-163">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<YOUR_SERVER_URL>/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="12c9f-163">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<YOUR_SERVER_URL>/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="12c9f-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="12c9f-164">These values are not real.</span></span> <span data-ttu-id="12c9f-165">Uppdatera dessa värden med hello faktiska inloggnings-URL, identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="12c9f-165">Update these values with hello actual Sign-On URL, Identifier and, Reply URL.</span></span> <span data-ttu-id="12c9f-166">Kontakta [LiquidFiles klienten supportteamet](https://www.liquidfiles.com/support.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="12c9f-166">Contact [LiquidFiles Client support team](https://www.liquidfiles.com/support.html) tooget these values.</span></span> 
 
4. <span data-ttu-id="12c9f-167">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="12c9f-167">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_certificate.png) 

5. <span data-ttu-id="12c9f-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="12c9f-171">På hello **LiquidFiles Configuration** klickar du på **konfigurera LiquidFiles** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="12c9f-171">On hello **LiquidFiles Configuration** section, click **Configure LiquidFiles** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="12c9f-172">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="12c9f-172">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_configure.png)
 
7. <span data-ttu-id="12c9f-174">Inloggning tooyour LiquidFiles företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="12c9f-174">Sign-on tooyour LiquidFiles company site as administrator.</span></span>

8. <span data-ttu-id="12c9f-175">Klicka på **enkel inloggning** i hello **Admin > Configuration** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="12c9f-175">Click **Single Sign-On** in hello **Admin > Configuration** from hello menu.</span></span>

9. <span data-ttu-id="12c9f-176">På hello **konfiguration för enkel inloggning** utför följande steg hello</span><span class="sxs-lookup"><span data-stu-id="12c9f-176">On hello **Single Sign-On Configuration** page, perform hello following steps</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_single_01.png)

    <span data-ttu-id="12c9f-178">a.</span><span class="sxs-lookup"><span data-stu-id="12c9f-178">a.</span></span> <span data-ttu-id="12c9f-179">Som **enkel inloggning på metoden**väljer **SAML 2**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-179">As **Single Sign On Method**, select **SAML 2**.</span></span>

    <span data-ttu-id="12c9f-180">b.</span><span class="sxs-lookup"><span data-stu-id="12c9f-180">b.</span></span> <span data-ttu-id="12c9f-181">I hello **IDP inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-181">In hello **IDP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="12c9f-182">c.</span><span class="sxs-lookup"><span data-stu-id="12c9f-182">c.</span></span> <span data-ttu-id="12c9f-183">I hello **IDP logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-183">In hello **IDP Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="12c9f-184">d.</span><span class="sxs-lookup"><span data-stu-id="12c9f-184">d.</span></span> <span data-ttu-id="12c9f-185">I hello **IDP Cert fingeravtryck** textruta klistra in hello **TUMAVTRYCKET** värde som du har kopierat från Azure-portalen...</span><span class="sxs-lookup"><span data-stu-id="12c9f-185">In hello **IDP Cert Fingerprint** textbox, paste hello **THUMBPRINT** value which you have copied from Azure portal..</span></span>

    <span data-ttu-id="12c9f-186">e.</span><span class="sxs-lookup"><span data-stu-id="12c9f-186">e.</span></span> <span data-ttu-id="12c9f-187">Ange hello värde i hello identifierare namnformat textruta **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-187">In hello Name Identifier Format textbox, type hello value **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="12c9f-188">f.</span><span class="sxs-lookup"><span data-stu-id="12c9f-188">f.</span></span> <span data-ttu-id="12c9f-189">Ange hello värde i hello Authn kontexten textruta **urn: oasis: namn: tc: SAML:2.0:ac:classes:PasswordProtectedTransport**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-189">In hello Authn Context textbox, type hello value **urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport**.</span></span>

    <span data-ttu-id="12c9f-190">g.</span><span class="sxs-lookup"><span data-stu-id="12c9f-190">g.</span></span> <span data-ttu-id="12c9f-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-191">Click **Save**.</span></span>  

> [!TIP]
> <span data-ttu-id="12c9f-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="12c9f-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="12c9f-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="12c9f-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="12c9f-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="12c9f-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="12c9f-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="12c9f-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="12c9f-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="12c9f-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="12c9f-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="12c9f-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="12c9f-199">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="12c9f-199">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="12c9f-201">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-201">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="12c9f-203">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="12c9f-203">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="12c9f-205">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="12c9f-205">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-liquidfiles-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="12c9f-207">a.</span><span class="sxs-lookup"><span data-stu-id="12c9f-207">a.</span></span> <span data-ttu-id="12c9f-208">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-208">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="12c9f-209">b.</span><span class="sxs-lookup"><span data-stu-id="12c9f-209">b.</span></span> <span data-ttu-id="12c9f-210">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="12c9f-210">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="12c9f-211">c.</span><span class="sxs-lookup"><span data-stu-id="12c9f-211">c.</span></span> <span data-ttu-id="12c9f-212">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-212">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="12c9f-213">d.</span><span class="sxs-lookup"><span data-stu-id="12c9f-213">d.</span></span> <span data-ttu-id="12c9f-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-214">Click **Create**.</span></span>
 
### <a name="creating-a-liquidfiles-test-user"></a><span data-ttu-id="12c9f-215">Skapa en testanvändare LiquidFiles</span><span class="sxs-lookup"><span data-stu-id="12c9f-215">Creating a LiquidFiles test user</span></span>

<span data-ttu-id="12c9f-216">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i LiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="12c9f-216">hello objective of this section is toocreate a user called Britta Simon in LiquidFiles.</span></span> <span data-ttu-id="12c9f-217">Arbeta med din LiquidFiles server-administratören tooget själv läggas till som en användare innan du loggar in tooyour LiquidFiles program.</span><span class="sxs-lookup"><span data-stu-id="12c9f-217">Work with your LiquidFiles server administrator tooget yourself added as a user before logging in tooyour LiquidFiles application.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="12c9f-218">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="12c9f-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="12c9f-219">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLiquidFiles.</span><span class="sxs-lookup"><span data-stu-id="12c9f-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLiquidFiles.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="12c9f-221">**tooassign Britta Simon tooLiquidFiles utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="12c9f-221">**tooassign Britta Simon tooLiquidFiles, perform hello following steps:**</span></span>

1. <span data-ttu-id="12c9f-222">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="12c9f-224">Välj i listan med program hello **LiquidFiles**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-224">In hello applications list, select **LiquidFiles**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-liquidfiles-tutorial/tutorial_liquidfiles_app.png) 

3. <span data-ttu-id="12c9f-226">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="12c9f-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="12c9f-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-228">Click **Add** button.</span></span> <span data-ttu-id="12c9f-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="12c9f-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="12c9f-231">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="12c9f-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="12c9f-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="12c9f-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="12c9f-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="12c9f-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="12c9f-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="12c9f-234">Testing single sign-on</span></span>

<span data-ttu-id="12c9f-235">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="12c9f-236">Du bör få automatiskt inloggade tooyour LiquidFiles programmet när du klickar på hello LiquidFiles panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="12c9f-236">When you click hello LiquidFiles tile in hello Access Panel, you should get automatically signed-on tooyour LiquidFiles application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="12c9f-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="12c9f-237">Additional resources</span></span>

* [<span data-ttu-id="12c9f-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="12c9f-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="12c9f-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="12c9f-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-liquidfiles-tutorial/tutorial_general_203.png

