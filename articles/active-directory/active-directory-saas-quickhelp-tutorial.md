---
title: "Självstudier: Azure Active Directory-integrering med QuickHelp | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och QuickHelp."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 655c9ad3-2076-4e2c-8e47-9ed3bf04be56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: bbde5eb9bdad89680923ccd36c321b6923f91789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a><span data-ttu-id="91042-103">Självstudier: Azure Active Directory-integrering med QuickHelp</span><span class="sxs-lookup"><span data-stu-id="91042-103">Tutorial: Azure Active Directory integration with QuickHelp</span></span>

<span data-ttu-id="91042-104">I kursen får du lära dig hur toointegrate QuickHelp med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="91042-104">In this tutorial, you learn how toointegrate QuickHelp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="91042-105">Integrera QuickHelp med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="91042-105">Integrating QuickHelp with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="91042-106">Du kan styra i Azure AD som har åtkomst till tooQuickHelp</span><span class="sxs-lookup"><span data-stu-id="91042-106">You can control in Azure AD who has access tooQuickHelp</span></span>
- <span data-ttu-id="91042-107">Du kan aktivera din användare tooautomatically get inloggade tooQuickHelp (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="91042-107">You can enable your users tooautomatically get signed-on tooQuickHelp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="91042-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91042-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="91042-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="91042-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91042-110">Krav</span><span class="sxs-lookup"><span data-stu-id="91042-110">Prerequisites</span></span>

<span data-ttu-id="91042-111">tooconfigure Azure AD-integrering med QuickHelp, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="91042-111">tooconfigure Azure AD integration with QuickHelp, you need hello following items:</span></span>

- <span data-ttu-id="91042-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="91042-112">An Azure AD subscription</span></span>
- <span data-ttu-id="91042-113">En QuickHelp enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="91042-113">A QuickHelp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="91042-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="91042-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="91042-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="91042-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="91042-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="91042-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="91042-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="91042-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="91042-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="91042-118">Scenario description</span></span>
<span data-ttu-id="91042-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="91042-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="91042-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="91042-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="91042-121">Att lägga till QuickHelp från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91042-121">Adding QuickHelp from hello gallery</span></span>
2. <span data-ttu-id="91042-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91042-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-quickhelp-from-hello-gallery"></a><span data-ttu-id="91042-123">Att lägga till QuickHelp från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91042-123">Adding QuickHelp from hello gallery</span></span>
<span data-ttu-id="91042-124">tooconfigure hello integrering av QuickHelp i Azure AD, behöver du tooadd QuickHelp hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="91042-124">tooconfigure hello integration of QuickHelp into Azure AD, you need tooadd QuickHelp from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="91042-125">**tooadd QuickHelp från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91042-125">**tooadd QuickHelp from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="91042-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91042-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="91042-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="91042-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="91042-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="91042-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="91042-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91042-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="91042-133">Skriv i sökrutan hello **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="91042-133">In hello search box, type **QuickHelp**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_search.png)

5. <span data-ttu-id="91042-135">Markera hello resultat på panelen **QuickHelp**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="91042-135">In hello results panel, select **QuickHelp**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="91042-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91042-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="91042-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med QuickHelp baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="91042-138">In this section, you configure and test Azure AD single sign-on with QuickHelp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="91042-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i QuickHelp är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="91042-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in QuickHelp is tooa user in Azure AD.</span></span> <span data-ttu-id="91042-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i QuickHelp toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="91042-140">In other words, a link relationship between an Azure AD user and hello related user in QuickHelp needs toobe established.</span></span>

<span data-ttu-id="91042-141">I QuickHelp, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="91042-141">In QuickHelp, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="91042-142">tooconfigure och testa Azure AD enkel inloggning med QuickHelp, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="91042-142">tooconfigure and test Azure AD single sign-on with QuickHelp, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="91042-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="91042-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="91042-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91042-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="91042-145">**[Skapa en testanvändare QuickHelp](#creating-a-quickhelp-test-user)**  -toohave en motsvarighet för Britta Simon i QuickHelp som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="91042-145">**[Creating a QuickHelp test user](#creating-a-quickhelp-test-user)** - toohave a counterpart of Britta Simon in QuickHelp that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="91042-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91042-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="91042-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="91042-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="91042-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91042-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="91042-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt QuickHelp program.</span><span class="sxs-lookup"><span data-stu-id="91042-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your QuickHelp application.</span></span>

<span data-ttu-id="91042-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med QuickHelp:**</span><span class="sxs-lookup"><span data-stu-id="91042-150">**tooconfigure Azure AD single sign-on with QuickHelp, perform hello following steps:**</span></span>

1. <span data-ttu-id="91042-151">I hello Azure-portalen på hello **QuickHelp** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="91042-151">In hello Azure portal, on hello **QuickHelp** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="91042-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91042-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_samlbase.png)

3. <span data-ttu-id="91042-155">På hello **QuickHelp domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91042-155">On hello **QuickHelp Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_url.png)

    <span data-ttu-id="91042-157">a.</span><span class="sxs-lookup"><span data-stu-id="91042-157">a.</span></span> <span data-ttu-id="91042-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://quickhelp.com/<instancename>/#/Login`</span><span class="sxs-lookup"><span data-stu-id="91042-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://quickhelp.com/<instancename>/#/Login`</span></span>

    <span data-ttu-id="91042-159">b.</span><span class="sxs-lookup"><span data-stu-id="91042-159">b.</span></span> <span data-ttu-id="91042-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.quickhelp.com`</span><span class="sxs-lookup"><span data-stu-id="91042-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.quickhelp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="91042-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="91042-161">These values are not real.</span></span> <span data-ttu-id="91042-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="91042-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="91042-163">Kontakta [QuickHelp klienten supportteamet](https://support.quickhelp.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="91042-163">Contact [QuickHelp Client support team](https://support.quickhelp.com/) tooget these values.</span></span> 
 
4. <span data-ttu-id="91042-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="91042-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_certificate.png) 

5. <span data-ttu-id="91042-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="91042-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="91042-168">Inloggning tooyour QuickHelp företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="91042-168">Sign-on tooyour QuickHelp company site as administrator.</span></span>

7. <span data-ttu-id="91042-169">Hello-menyn överst hello **Admin**.</span><span class="sxs-lookup"><span data-stu-id="91042-169">In hello menu on hello top, click **Admin**.</span></span>
   
    ![Konfigurera enkel inloggning][21]

8. <span data-ttu-id="91042-171">I hello **QuickHelp Admin** -menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="91042-171">In hello **QuickHelp Admin** menu, click **Settings**.</span></span>
   
    ![Konfigurera enkel inloggning][22]

9. <span data-ttu-id="91042-173">Klicka på **autentiseringsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="91042-173">Click **Authentication Settings**.</span></span>

10. <span data-ttu-id="91042-174">På hello **autentiseringsinställningar** utför följande steg hello</span><span class="sxs-lookup"><span data-stu-id="91042-174">On hello **Authentication Settings** page, perform hello following steps</span></span>
   
    ![Konfigurera enkel inloggning][23]
   
    <span data-ttu-id="91042-176">a.</span><span class="sxs-lookup"><span data-stu-id="91042-176">a.</span></span> <span data-ttu-id="91042-177">Som **SSO typen**väljer **WSFederation**.</span><span class="sxs-lookup"><span data-stu-id="91042-177">As **SSO Type**, select **WSFederation**.</span></span>
   
    <span data-ttu-id="91042-178">b.</span><span class="sxs-lookup"><span data-stu-id="91042-178">b.</span></span> <span data-ttu-id="91042-179">tooupload hämtade Azure metadatafil klickar du på **Bläddra**, navigera toohello fil, sista Klicka **överföra Metadata**.</span><span class="sxs-lookup"><span data-stu-id="91042-179">tooupload your downloaded Azure metadata file, click **Browse**, navigate toohello file, end then click **Upload Metadata**.</span></span>
   
    <span data-ttu-id="91042-180">c.</span><span class="sxs-lookup"><span data-stu-id="91042-180">c.</span></span> <span data-ttu-id="91042-181">I hello **e-post** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="91042-181">In hello **Email** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
   
    <span data-ttu-id="91042-182">d.</span><span class="sxs-lookup"><span data-stu-id="91042-182">d.</span></span> <span data-ttu-id="91042-183">I hello **Förnamn** textruta `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="91042-183">In hello **First Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>
   
    <span data-ttu-id="91042-184">e.</span><span class="sxs-lookup"><span data-stu-id="91042-184">e.</span></span> <span data-ttu-id="91042-185">I hello **efternamn** textruta `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="91042-185">In hello **Last Name** textbox, `type http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>
   
    <span data-ttu-id="91042-186">f.</span><span class="sxs-lookup"><span data-stu-id="91042-186">f.</span></span> <span data-ttu-id="91042-187">I hello **Åtgärdsfältet**, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="91042-187">In hello **Action Bar**, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="91042-188">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="91042-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="91042-189">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="91042-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="91042-190">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="91042-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="91042-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="91042-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="91042-192">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91042-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="91042-194">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91042-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="91042-195">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91042-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="91042-197">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="91042-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="91042-199">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91042-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="91042-201">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91042-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="91042-203">a.</span><span class="sxs-lookup"><span data-stu-id="91042-203">a.</span></span> <span data-ttu-id="91042-204">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="91042-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="91042-205">b.</span><span class="sxs-lookup"><span data-stu-id="91042-205">b.</span></span> <span data-ttu-id="91042-206">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="91042-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="91042-207">c.</span><span class="sxs-lookup"><span data-stu-id="91042-207">c.</span></span> <span data-ttu-id="91042-208">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="91042-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="91042-209">d.</span><span class="sxs-lookup"><span data-stu-id="91042-209">d.</span></span> <span data-ttu-id="91042-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="91042-210">Click **Create**.</span></span>
 
### <a name="creating-a-quickhelp-test-user"></a><span data-ttu-id="91042-211">Skapa en testanvändare QuickHelp</span><span class="sxs-lookup"><span data-stu-id="91042-211">Creating a QuickHelp test user</span></span>

<span data-ttu-id="91042-212">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i QuickHelp.</span><span class="sxs-lookup"><span data-stu-id="91042-212">hello objective of this section is toocreate a user called Britta Simon in QuickHelp.</span></span>
<span data-ttu-id="91042-213">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i QuickHelp tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="91042-213">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in QuickHelp tooa user in Azure AD is.</span></span> <span data-ttu-id="91042-214">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i QuickHelp toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="91042-214">In other words, a link relationship between an Azure AD user and hello related user in QuickHelp needs toobe established.</span></span>

<span data-ttu-id="91042-215">QuickHelp stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="91042-215">QuickHelp supports just-in-time provisioning.</span></span> <span data-ttu-id="91042-216">Det innebär om det behövs ett konto skapas automatiskt i QuickHelp och hello-kontot är länkade toohello Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="91042-216">This means, if necessary, a user account is automatically created in QuickHelp and hello account is linked toohello Azure AD account.</span></span>

<span data-ttu-id="91042-217">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="91042-217">There is no action item for you in this section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="91042-218">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="91042-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="91042-219">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooQuickHelp.</span><span class="sxs-lookup"><span data-stu-id="91042-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooQuickHelp.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="91042-221">**tooassign Britta Simon tooQuickHelp utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91042-221">**tooassign Britta Simon tooQuickHelp, perform hello following steps:**</span></span>

1. <span data-ttu-id="91042-222">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="91042-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="91042-224">Välj i listan med program hello **QuickHelp**.</span><span class="sxs-lookup"><span data-stu-id="91042-224">In hello applications list, select **QuickHelp**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_app.png) 

3. <span data-ttu-id="91042-226">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="91042-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="91042-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="91042-228">Click **Add** button.</span></span> <span data-ttu-id="91042-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91042-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="91042-231">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="91042-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="91042-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91042-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="91042-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91042-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="91042-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91042-234">Testing single sign-on</span></span>

<span data-ttu-id="91042-235">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="91042-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="91042-236">Du bör få automatiskt inloggade tooyour QuickHelp programmet när du klickar på hello QuickHelp panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="91042-236">When you click hello QuickHelp tile in hello Access Panel, you should get automatically signed-on tooyour QuickHelp application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="91042-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="91042-237">Additional resources</span></span>

* [<span data-ttu-id="91042-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="91042-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="91042-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="91042-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
