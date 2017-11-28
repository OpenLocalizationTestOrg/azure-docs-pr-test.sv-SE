---
title: "Självstudier: Azure Active Directory-integrering med säkra leverera | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och skydda leverera."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fccd5668-fe6f-4e6d-a9ce-ba4f321c33d1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 13699a9abb8be24054b0810a8178cd5ef170c338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a><span data-ttu-id="921b7-103">Självstudier: Azure Active Directory-integrering med säkra leverera</span><span class="sxs-lookup"><span data-stu-id="921b7-103">Tutorial: Azure Active Directory integration with SECURE DELIVER</span></span>

<span data-ttu-id="921b7-104">I kursen får du lära dig hur toointegrate skydda leverera med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="921b7-104">In this tutorial, you learn how toointegrate SECURE DELIVER with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="921b7-105">Integrera SECURE leverera med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="921b7-105">Integrating SECURE DELIVER with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="921b7-106">Du kan styra i Azure AD som har åtkomst till tooSECURE leverera</span><span class="sxs-lookup"><span data-stu-id="921b7-106">You can control in Azure AD who has access tooSECURE DELIVER</span></span>
- <span data-ttu-id="921b7-107">Du kan aktivera din användare tooautomatically get inloggade tooSECURE leverera (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="921b7-107">You can enable your users tooautomatically get signed-on tooSECURE DELIVER (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="921b7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="921b7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="921b7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="921b7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="921b7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="921b7-110">Prerequisites</span></span>

<span data-ttu-id="921b7-111">tooconfigure Azure AD-integrering med säkra leverera måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="921b7-111">tooconfigure Azure AD integration with SECURE DELIVER, you need hello following items:</span></span>

- <span data-ttu-id="921b7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="921b7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="921b7-113">En säker tillhandahålla enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="921b7-113">A SECURE DELIVER single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="921b7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="921b7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="921b7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="921b7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="921b7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="921b7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="921b7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="921b7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="921b7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="921b7-118">Scenario description</span></span>
<span data-ttu-id="921b7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="921b7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="921b7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="921b7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="921b7-121">Att lägga till säker leverera från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="921b7-121">Adding SECURE DELIVER from hello gallery</span></span>
2. <span data-ttu-id="921b7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="921b7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-secure-deliver-from-hello-gallery"></a><span data-ttu-id="921b7-123">Att lägga till säker leverera från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="921b7-123">Adding SECURE DELIVER from hello gallery</span></span>
<span data-ttu-id="921b7-124">tooconfigure hello integrering av säker leverera i Azure AD, behöver du tooadd SECURE leverera hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="921b7-124">tooconfigure hello integration of SECURE DELIVER into Azure AD, you need tooadd SECURE DELIVER from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="921b7-125">**tooadd SECURE leverera från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="921b7-125">**tooadd SECURE DELIVER from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="921b7-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="921b7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="921b7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="921b7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="921b7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="921b7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="921b7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="921b7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="921b7-133">Skriv i sökrutan hello **SECURE leverera**.</span><span class="sxs-lookup"><span data-stu-id="921b7-133">In hello search box, type **SECURE DELIVER**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_search.png)

5. <span data-ttu-id="921b7-135">Markera hello resultat på panelen **SECURE leverera**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="921b7-135">In hello results panel, select **SECURE DELIVER**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="921b7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="921b7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="921b7-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med säkra leverera baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="921b7-138">In this section, you configure and test Azure AD single sign-on with SECURE DELIVER based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="921b7-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i säkra leverera är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="921b7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SECURE DELIVER is tooa user in Azure AD.</span></span> <span data-ttu-id="921b7-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i säkra leverera toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="921b7-140">In other words, a link relationship between an Azure AD user and hello related user in SECURE DELIVER needs toobe established.</span></span>

<span data-ttu-id="921b7-141">Tilldela hello värdet för hello i säkra leverera **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="921b7-141">In SECURE DELIVER, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="921b7-142">tooconfigure och testa Azure AD enkel inloggning med säkra leverera måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="921b7-142">tooconfigure and test Azure AD single sign-on with SECURE DELIVER, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="921b7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="921b7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="921b7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="921b7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="921b7-145">**[Skapa en säker leverera testanvändare](#creating-a-secure-deliver-test-user)**  -toohave en motsvarighet för Britta Simon i säkra levererar som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="921b7-145">**[Creating a SECURE DELIVER test user](#creating-a-secure-deliver-test-user)** - toohave a counterpart of Britta Simon in SECURE DELIVER that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="921b7-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="921b7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="921b7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="921b7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="921b7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="921b7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="921b7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för säker leverera.</span><span class="sxs-lookup"><span data-stu-id="921b7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SECURE DELIVER application.</span></span>

<span data-ttu-id="921b7-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med säkra leverera:**</span><span class="sxs-lookup"><span data-stu-id="921b7-150">**tooconfigure Azure AD single sign-on with SECURE DELIVER, perform hello following steps:**</span></span>

1. <span data-ttu-id="921b7-151">I hello Azure-portalen på hello **SECURE leverera** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="921b7-151">In hello Azure portal, on hello **SECURE DELIVER** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="921b7-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="921b7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_samlbase.png)

3. <span data-ttu-id="921b7-155">På hello **URL: er och skydda leverera domän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="921b7-155">On hello **SECURE DELIVER Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_url.png)

    <span data-ttu-id="921b7-157">a.</span><span class="sxs-lookup"><span data-stu-id="921b7-157">a.</span></span> <span data-ttu-id="921b7-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span><span class="sxs-lookup"><span data-stu-id="921b7-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/jsf/login/sso`</span></span>

    <span data-ttu-id="921b7-159">b.</span><span class="sxs-lookup"><span data-stu-id="921b7-159">b.</span></span> <span data-ttu-id="921b7-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span><span class="sxs-lookup"><span data-stu-id="921b7-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.i-securedeliver.jp/sd/<tenantname>/postResponse`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="921b7-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="921b7-161">These values are not real.</span></span> <span data-ttu-id="921b7-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="921b7-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="921b7-163">Kontakta [SECURE leverera klienten supportteamet](mailto:iw-sd-support@fujifilm.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="921b7-163">Contact [SECURE DELIVER Client support team](mailto:iw-sd-support@fujifilm.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="921b7-164">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="921b7-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_certificate.png) 

5. <span data-ttu-id="921b7-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="921b7-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="921b7-168">På hello **SECURE leverera Configuration** klickar du på **konfigurera SECURE leverera** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="921b7-168">On hello **SECURE DELIVER Configuration** section, click **Configure SECURE DELIVER** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="921b7-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="921b7-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_configure.png) 

7. <span data-ttu-id="921b7-171">tooconfigure enkel inloggning på **SECURE leverera** sida, behöver du toosend hello hämtas **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** för[SECURE leverera supportteamet](mailto:iw-sd-support@fujifilm.com).</span><span class="sxs-lookup"><span data-stu-id="921b7-171">tooconfigure single sign-on on **SECURE DELIVER** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com).</span></span> <span data-ttu-id="921b7-172">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="921b7-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="921b7-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="921b7-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="921b7-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="921b7-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="921b7-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="921b7-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="921b7-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="921b7-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="921b7-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="921b7-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="921b7-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="921b7-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="921b7-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="921b7-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="921b7-182">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="921b7-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="921b7-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="921b7-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="921b7-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="921b7-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="921b7-188">a.</span><span class="sxs-lookup"><span data-stu-id="921b7-188">a.</span></span> <span data-ttu-id="921b7-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="921b7-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="921b7-190">b.</span><span class="sxs-lookup"><span data-stu-id="921b7-190">b.</span></span> <span data-ttu-id="921b7-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="921b7-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="921b7-192">c.</span><span class="sxs-lookup"><span data-stu-id="921b7-192">c.</span></span> <span data-ttu-id="921b7-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="921b7-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="921b7-194">d.</span><span class="sxs-lookup"><span data-stu-id="921b7-194">d.</span></span> <span data-ttu-id="921b7-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="921b7-195">Click **Create**.</span></span>
 
### <a name="creating-a-secure-deliver-test-user"></a><span data-ttu-id="921b7-196">Skapa en säker leverera testanvändare</span><span class="sxs-lookup"><span data-stu-id="921b7-196">Creating a SECURE DELIVER test user</span></span>

<span data-ttu-id="921b7-197">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i säkra leverera.</span><span class="sxs-lookup"><span data-stu-id="921b7-197">hello objective of this section is toocreate a user called Britta Simon in SECURE DELIVER.</span></span> <span data-ttu-id="921b7-198">Arbeta med [SECURE leverera supportteamet](mailto:iw-sd-support@fujifilm.com) tooadd hello användare i hello SECURE leverera konto.</span><span class="sxs-lookup"><span data-stu-id="921b7-198">Work with [SECURE DELIVER support team](mailto:iw-sd-support@fujifilm.com) tooadd hello users in hello SECURE DELIVER account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="921b7-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="921b7-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="921b7-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSECURE leverera.</span><span class="sxs-lookup"><span data-stu-id="921b7-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSECURE DELIVER.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="921b7-202">**tooassign Britta Simon tooSECURE leverera, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="921b7-202">**tooassign Britta Simon tooSECURE DELIVER, perform hello following steps:**</span></span>

1. <span data-ttu-id="921b7-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="921b7-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="921b7-205">Välj i listan med program hello **SECURE leverera**.</span><span class="sxs-lookup"><span data-stu-id="921b7-205">In hello applications list, select **SECURE DELIVER**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_app.png) 

3. <span data-ttu-id="921b7-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="921b7-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="921b7-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="921b7-209">Click **Add** button.</span></span> <span data-ttu-id="921b7-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="921b7-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="921b7-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="921b7-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="921b7-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="921b7-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="921b7-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="921b7-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="921b7-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="921b7-215">Testing single sign-on</span></span>

<span data-ttu-id="921b7-216">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="921b7-216">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="921b7-217">Du bör få automatiskt inloggade tooyour SECURE leverera programmet när du klickar på hello SECURE leverera panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="921b7-217">When you click hello SECURE DELIVER tile in hello Access Panel, you should get automatically signed-on tooyour SECURE DELIVER application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="921b7-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="921b7-218">Additional resources</span></span>

* [<span data-ttu-id="921b7-219">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="921b7-219">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="921b7-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="921b7-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png

