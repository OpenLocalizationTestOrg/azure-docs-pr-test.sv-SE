---
title: "Självstudier: Azure Active Directory-integrering med Showpad | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Showpad."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48b6bee0-dbc5-4863-964d-75b25e517741
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 2c8c306b4b94c368a93f92123d3abe9fe35167db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-showpad"></a><span data-ttu-id="d1952-103">Självstudier: Azure Active Directory-integrering med Showpad</span><span class="sxs-lookup"><span data-stu-id="d1952-103">Tutorial: Azure Active Directory integration with Showpad</span></span>

<span data-ttu-id="d1952-104">I kursen får du lära dig hur toointegrate Showpad med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d1952-104">In this tutorial, you learn how toointegrate Showpad with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d1952-105">Integrera Showpad med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d1952-105">Integrating Showpad with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d1952-106">Du kan styra i Azure AD som har åtkomst till tooShowpad</span><span class="sxs-lookup"><span data-stu-id="d1952-106">You can control in Azure AD who has access tooShowpad</span></span>
- <span data-ttu-id="d1952-107">Du kan aktivera din användare tooautomatically get inloggade tooShowpad (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d1952-107">You can enable your users tooautomatically get signed-on tooShowpad (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d1952-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d1952-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d1952-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d1952-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1952-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d1952-110">Prerequisites</span></span>

<span data-ttu-id="d1952-111">tooconfigure Azure AD-integrering med Showpad, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d1952-111">tooconfigure Azure AD integration with Showpad, you need hello following items:</span></span>

- <span data-ttu-id="d1952-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d1952-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d1952-113">En Showpad enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d1952-113">A Showpad single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d1952-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d1952-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d1952-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d1952-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d1952-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d1952-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d1952-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d1952-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d1952-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d1952-118">Scenario description</span></span>
<span data-ttu-id="d1952-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d1952-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d1952-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d1952-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d1952-121">Att lägga till Showpad från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d1952-121">Adding Showpad from hello gallery</span></span>
2. <span data-ttu-id="d1952-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1952-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-showpad-from-hello-gallery"></a><span data-ttu-id="d1952-123">Att lägga till Showpad från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d1952-123">Adding Showpad from hello gallery</span></span>

<span data-ttu-id="d1952-124">tooconfigure hello integrering av Showpad i Azure AD, behöver du tooadd Showpad hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d1952-124">tooconfigure hello integration of Showpad into Azure AD, you need tooadd Showpad from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d1952-125">**tooadd Showpad från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d1952-125">**tooadd Showpad from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1952-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d1952-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d1952-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d1952-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d1952-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d1952-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d1952-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1952-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d1952-133">Skriv i sökrutan hello **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="d1952-133">In hello search box, type **Showpad**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_search.png)

5. <span data-ttu-id="d1952-135">Markera hello resultat på panelen **Showpad**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d1952-135">In hello results panel, select **Showpad**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d1952-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1952-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="d1952-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Showpad baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d1952-138">In this section, you configure and test Azure AD single sign-on with Showpad based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d1952-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Showpad är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d1952-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Showpad is tooa user in Azure AD.</span></span> <span data-ttu-id="d1952-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Showpad toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d1952-140">In other words, a link relationship between an Azure AD user and hello related user in Showpad needs toobe established.</span></span>

<span data-ttu-id="d1952-141">I Showpad, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d1952-141">In Showpad, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d1952-142">tooconfigure och testa Azure AD enkel inloggning med Showpad, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d1952-142">tooconfigure and test Azure AD single sign-on with Showpad, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d1952-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d1952-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d1952-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1952-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d1952-145">**[Skapa en testanvändare Showpad](#creating-a-showpad-test-user)**  -toohave en motsvarighet för Britta Simon i Showpad som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d1952-145">**[Creating a Showpad test user](#creating-a-showpad-test-user)** - toohave a counterpart of Britta Simon in Showpad that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d1952-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d1952-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d1952-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d1952-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d1952-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1952-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d1952-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Showpad program.</span><span class="sxs-lookup"><span data-stu-id="d1952-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Showpad application.</span></span>

<span data-ttu-id="d1952-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Showpad:**</span><span class="sxs-lookup"><span data-stu-id="d1952-150">**tooconfigure Azure AD single sign-on with Showpad, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1952-151">I hello Azure-portalen på hello **Showpad** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d1952-151">In hello Azure portal, on hello **Showpad** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d1952-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d1952-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_samlbase.png)

3. <span data-ttu-id="d1952-155">På hello **Showpad domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1952-155">On hello **Showpad Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_url.png)

    <span data-ttu-id="d1952-157">a.</span><span class="sxs-lookup"><span data-stu-id="d1952-157">a.</span></span> <span data-ttu-id="d1952-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<comapany-name>.showpad.biz/login`</span><span class="sxs-lookup"><span data-stu-id="d1952-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<comapany-name>.showpad.biz/login`</span></span>

    <span data-ttu-id="d1952-159">b.</span><span class="sxs-lookup"><span data-stu-id="d1952-159">b.</span></span> <span data-ttu-id="d1952-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company-name>.showpad.biz`</span><span class="sxs-lookup"><span data-stu-id="d1952-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company-name>.showpad.biz`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d1952-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d1952-161">These values are not real.</span></span> <span data-ttu-id="d1952-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="d1952-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d1952-163">Kontakta [Showpad supportteamet](https://help.showpad.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d1952-163">Contact [Showpad support team](https://help.showpad.com) tooget these values.</span></span> 
 


4. <span data-ttu-id="d1952-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="d1952-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_certificate.png) 

5. <span data-ttu-id="d1952-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1952-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d1952-168">Inloggning tooyour Showpad innehavaren som administratör.</span><span class="sxs-lookup"><span data-stu-id="d1952-168">Sign-on tooyour Showpad tenant as an administrator.</span></span>

7. <span data-ttu-id="d1952-169">Hello hello överst klickar du på menyn hello **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d1952-169">In hello menu on hello top, click hello **Settings**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

8. <span data-ttu-id="d1952-171">Navigera för ”**enkel inloggning**” och klicka på ”**aktivera**”.</span><span class="sxs-lookup"><span data-stu-id="d1952-171">Navigate too"**Single Sign-On**" and click "**Enable**."</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

9. <span data-ttu-id="d1952-173">På hello **lägga till en tjänst för SAML 2.0** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1952-173">On hello **Add a SAML 2.0 Service** dialog, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 
   
    <span data-ttu-id="d1952-175">a.</span><span class="sxs-lookup"><span data-stu-id="d1952-175">a.</span></span> <span data-ttu-id="d1952-176">I hello **namn** textruta hello-typnamn för identifierare Provider (till exempel: företagets namn).</span><span class="sxs-lookup"><span data-stu-id="d1952-176">In hello **Name** textbox, type hello name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="d1952-177">b.</span><span class="sxs-lookup"><span data-stu-id="d1952-177">b.</span></span> <span data-ttu-id="d1952-178">Som **Metadatakälla**väljer **XML**.</span><span class="sxs-lookup"><span data-stu-id="d1952-178">As **Metadata Source**, select **XML**.</span></span>
   
    <span data-ttu-id="d1952-179">c.</span><span class="sxs-lookup"><span data-stu-id="d1952-179">c.</span></span> <span data-ttu-id="d1952-180">Kopiera hello innehållet metadata XML-filen som du har hämtat från hello Azure-portalen, och klistra in den i hello **XML-Metadata för** textruta.</span><span class="sxs-lookup"><span data-stu-id="d1952-180">Copy hello content of metadata XML file, which you have downloaded from hello Azure portal, and then paste it into hello **Metadata XML** textbox.</span></span>
   
    <span data-ttu-id="d1952-181">d.</span><span class="sxs-lookup"><span data-stu-id="d1952-181">d.</span></span> <span data-ttu-id="d1952-182">Välj **automatiskt etablera konton för nya användare när de loggar in**.</span><span class="sxs-lookup"><span data-stu-id="d1952-182">Select **Auto-provision accounts for new users when they log in**.</span></span>
   
    <span data-ttu-id="d1952-183">e.</span><span class="sxs-lookup"><span data-stu-id="d1952-183">e.</span></span> <span data-ttu-id="d1952-184">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="d1952-184">Click **Submit**.</span></span>

> [!TIP]
> <span data-ttu-id="d1952-185">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d1952-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d1952-186">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d1952-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d1952-187">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d1952-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d1952-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d1952-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="d1952-189">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1952-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d1952-191">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d1952-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1952-192">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d1952-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d1952-194">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d1952-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d1952-196">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1952-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d1952-198">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1952-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d1952-200">a.</span><span class="sxs-lookup"><span data-stu-id="d1952-200">a.</span></span> <span data-ttu-id="d1952-201">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d1952-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d1952-202">b.</span><span class="sxs-lookup"><span data-stu-id="d1952-202">b.</span></span> <span data-ttu-id="d1952-203">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d1952-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d1952-204">c.</span><span class="sxs-lookup"><span data-stu-id="d1952-204">c.</span></span> <span data-ttu-id="d1952-205">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d1952-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d1952-206">d.</span><span class="sxs-lookup"><span data-stu-id="d1952-206">d.</span></span> <span data-ttu-id="d1952-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d1952-207">Click **Create**.</span></span>
 
### <a name="creating-a-showpad-test-user"></a><span data-ttu-id="d1952-208">Skapa en testanvändare Showpad</span><span class="sxs-lookup"><span data-stu-id="d1952-208">Creating a Showpad test user</span></span>

<span data-ttu-id="d1952-209">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Showpad.</span><span class="sxs-lookup"><span data-stu-id="d1952-209">hello objective of this section is toocreate a user called Britta Simon in Showpad.</span></span> 

<span data-ttu-id="d1952-210">Showpad stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="d1952-210">Showpad supports just-in-time provisioning.</span></span> <span data-ttu-id="d1952-211">Du har aktiverat allokering i  **[konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**.</span><span class="sxs-lookup"><span data-stu-id="d1952-211">You have enabled provisioning in **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**.</span></span> 

<span data-ttu-id="d1952-212">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="d1952-212">There is no action item for you in this section.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d1952-213">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d1952-213">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d1952-214">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooShowpad.</span><span class="sxs-lookup"><span data-stu-id="d1952-214">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooShowpad.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d1952-216">**tooassign Britta Simon tooShowpad utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d1952-216">**tooassign Britta Simon tooShowpad, perform hello following steps:**</span></span>

1. <span data-ttu-id="d1952-217">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d1952-217">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d1952-219">Välj i listan med program hello **Showpad**.</span><span class="sxs-lookup"><span data-stu-id="d1952-219">In hello applications list, select **Showpad**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_app.png) 

3. <span data-ttu-id="d1952-221">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d1952-221">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d1952-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1952-223">Click **Add** button.</span></span> <span data-ttu-id="d1952-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1952-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d1952-226">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d1952-226">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d1952-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1952-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d1952-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1952-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d1952-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1952-229">Testing single sign-on</span></span>

<span data-ttu-id="d1952-230">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d1952-230">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d1952-231">Du bör få automatiskt inloggade tooShowpad programmet när du klickar på hello Showpad panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d1952-231">When you click hello Showpad tile in hello Access Panel, you should get automatically signed-on tooShowpad application.</span></span>
<span data-ttu-id="d1952-232">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d1952-232">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1952-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d1952-233">Additional resources</span></span>

* [<span data-ttu-id="d1952-234">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d1952-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d1952-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d1952-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png

