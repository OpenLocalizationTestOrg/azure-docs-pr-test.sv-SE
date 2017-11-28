---
title: "Självstudier: Azure Active Directory-integrering med Wikispaces | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Wikispaces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 665b95aa-f7f5-4406-9e2a-6fc299a1599c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: eb5b72e60b415cb657b70ba530df731ae14b0425
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wikispaces"></a><span data-ttu-id="85942-103">Självstudier: Azure Active Directory-integrering med Wikispaces</span><span class="sxs-lookup"><span data-stu-id="85942-103">Tutorial: Azure Active Directory integration with Wikispaces</span></span>

<span data-ttu-id="85942-104">I kursen får du lära dig hur toointegrate Wikispaces med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="85942-104">In this tutorial, you learn how toointegrate Wikispaces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="85942-105">Integrera Wikispaces med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="85942-105">Integrating Wikispaces with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="85942-106">Du kan styra i Azure AD som har åtkomst till tooWikispaces</span><span class="sxs-lookup"><span data-stu-id="85942-106">You can control in Azure AD who has access tooWikispaces</span></span>
- <span data-ttu-id="85942-107">Du kan aktivera din användare tooautomatically get inloggade tooWikispaces (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="85942-107">You can enable your users tooautomatically get signed-on tooWikispaces (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="85942-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="85942-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="85942-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="85942-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="85942-110">Krav</span><span class="sxs-lookup"><span data-stu-id="85942-110">Prerequisites</span></span>

<span data-ttu-id="85942-111">tooconfigure Azure AD-integrering med Wikispaces, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="85942-111">tooconfigure Azure AD integration with Wikispaces, you need hello following items:</span></span>

- <span data-ttu-id="85942-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="85942-112">An Azure AD subscription</span></span>
- <span data-ttu-id="85942-113">En Wikispaces enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="85942-113">A Wikispaces single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="85942-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="85942-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="85942-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="85942-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="85942-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="85942-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="85942-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="85942-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="85942-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="85942-118">Scenario description</span></span>
<span data-ttu-id="85942-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="85942-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="85942-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="85942-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="85942-121">Att lägga till Wikispaces från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="85942-121">Adding Wikispaces from hello gallery</span></span>
2. <span data-ttu-id="85942-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85942-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wikispaces-from-hello-gallery"></a><span data-ttu-id="85942-123">Att lägga till Wikispaces från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="85942-123">Adding Wikispaces from hello gallery</span></span>
<span data-ttu-id="85942-124">tooconfigure hello integrering av Wikispaces i Azure AD, behöver du tooadd Wikispaces hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="85942-124">tooconfigure hello integration of Wikispaces into Azure AD, you need tooadd Wikispaces from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="85942-125">**tooadd Wikispaces från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="85942-125">**tooadd Wikispaces from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="85942-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="85942-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="85942-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="85942-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="85942-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="85942-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="85942-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85942-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="85942-133">Skriv i sökrutan hello **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="85942-133">In hello search box, type **Wikispaces**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_search.png)

5. <span data-ttu-id="85942-135">Markera hello resultat på panelen **Wikispaces**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="85942-135">In hello results panel, select **Wikispaces**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="85942-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85942-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="85942-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Wikispaces baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="85942-138">In this section, you configure and test Azure AD single sign-on with Wikispaces based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="85942-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Wikispaces är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="85942-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Wikispaces is tooa user in Azure AD.</span></span> <span data-ttu-id="85942-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Wikispaces toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="85942-140">In other words, a link relationship between an Azure AD user and hello related user in Wikispaces needs toobe established.</span></span>

<span data-ttu-id="85942-141">I Wikispaces, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="85942-141">In Wikispaces, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="85942-142">tooconfigure och testa Azure AD enkel inloggning med Wikispaces, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="85942-142">tooconfigure and test Azure AD single sign-on with Wikispaces, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="85942-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="85942-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="85942-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="85942-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="85942-145">**[Skapa en testanvändare Wikispaces](#creating-a-wikispaces-test-user)**  -toohave en motsvarighet för Britta Simon i Wikispaces som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="85942-145">**[Creating a Wikispaces test user](#creating-a-wikispaces-test-user)** - toohave a counterpart of Britta Simon in Wikispaces that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="85942-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="85942-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="85942-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="85942-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="85942-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85942-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="85942-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Wikispaces program.</span><span class="sxs-lookup"><span data-stu-id="85942-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Wikispaces application.</span></span>

<span data-ttu-id="85942-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Wikispaces:**</span><span class="sxs-lookup"><span data-stu-id="85942-150">**tooconfigure Azure AD single sign-on with Wikispaces, perform hello following steps:**</span></span>

1. <span data-ttu-id="85942-151">I hello Azure-portalen på hello **Wikispaces** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="85942-151">In hello Azure portal, on hello **Wikispaces** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="85942-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="85942-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_samlbase.png)

3. <span data-ttu-id="85942-155">På hello **Wikispaces domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="85942-155">On hello **Wikispaces Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_url.png)

    <span data-ttu-id="85942-157">a.</span><span class="sxs-lookup"><span data-stu-id="85942-157">a.</span></span> <span data-ttu-id="85942-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.wikispaces.net`</span><span class="sxs-lookup"><span data-stu-id="85942-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.wikispaces.net`</span></span>

    <span data-ttu-id="85942-159">b.</span><span class="sxs-lookup"><span data-stu-id="85942-159">b.</span></span> <span data-ttu-id="85942-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://session.wikispaces.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="85942-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://session.wikispaces.net/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="85942-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="85942-161">These values are not real.</span></span> <span data-ttu-id="85942-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="85942-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="85942-163">Kontakta [Wikispaces klienten supportteamet](https://www.wikispaces.com/site/help) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="85942-163">Contact [Wikispaces Client support team](https://www.wikispaces.com/site/help) tooget these values.</span></span> 

4. <span data-ttu-id="85942-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="85942-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_certificate.png) 

5. <span data-ttu-id="85942-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="85942-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="85942-168">tooconfigure enkel inloggning på **Wikispaces** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Wikispaces supportteam](https://www.wikispaces.com/site/help).</span><span class="sxs-lookup"><span data-stu-id="85942-168">tooconfigure single sign-on on **Wikispaces** side, you need toosend hello downloaded **Metadata XML** too[Wikispaces support team](https://www.wikispaces.com/site/help).</span></span> <span data-ttu-id="85942-169">Du får ett meddelande som hello konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="85942-169">You will get a notification as soon as hello configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="85942-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="85942-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="85942-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="85942-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="85942-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="85942-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="85942-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="85942-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="85942-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="85942-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="85942-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="85942-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="85942-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="85942-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="85942-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="85942-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="85942-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85942-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="85942-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="85942-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="85942-185">a.</span><span class="sxs-lookup"><span data-stu-id="85942-185">a.</span></span> <span data-ttu-id="85942-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="85942-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="85942-187">b.</span><span class="sxs-lookup"><span data-stu-id="85942-187">b.</span></span> <span data-ttu-id="85942-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="85942-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="85942-189">c.</span><span class="sxs-lookup"><span data-stu-id="85942-189">c.</span></span> <span data-ttu-id="85942-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="85942-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="85942-191">d.</span><span class="sxs-lookup"><span data-stu-id="85942-191">d.</span></span> <span data-ttu-id="85942-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="85942-192">Click **Create**.</span></span>
 
### <a name="creating-a-wikispaces-test-user"></a><span data-ttu-id="85942-193">Skapa en testanvändare Wikispaces</span><span class="sxs-lookup"><span data-stu-id="85942-193">Creating a Wikispaces test user</span></span>

<span data-ttu-id="85942-194">I ordning tooenable Azure AD-användare toolog i tooWikispaces, måste de etableras i Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="85942-194">In order tooenable Azure AD users toolog in tooWikispaces, they must be provisioned into Wikispaces.</span></span> <span data-ttu-id="85942-195">Hello gäller Wikispaces är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="85942-195">In hello case of Wikispaces, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-accounts-perform-hello-following-steps"></a><span data-ttu-id="85942-196">tooprovision användarkonton, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="85942-196">tooprovision a user accounts, perform hello following steps:</span></span>
1. <span data-ttu-id="85942-197">Logga in tooyour **Wikispaces** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="85942-197">Log in tooyour **Wikispaces** company site as an administrator.</span></span>

2. <span data-ttu-id="85942-198">Gå för**medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="85942-198">Go too**Members**.</span></span>
   
    <span data-ttu-id="85942-199">![Medlemmar](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "medlemmar")</span><span class="sxs-lookup"><span data-stu-id="85942-199">![Members](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "Members")</span></span>

3. <span data-ttu-id="85942-200">Klicka på hello **bjuda in personer**.</span><span class="sxs-lookup"><span data-stu-id="85942-200">Click hello **Invite People**.</span></span>
   
    <span data-ttu-id="85942-201">![Bjuda in](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="85942-201">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "Invite People")</span></span>

4. <span data-ttu-id="85942-202">I hello **bjuda in personer** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="85942-202">In hello **Invite People** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="85942-203">![Bjuda in](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="85942-203">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "Invite People")</span></span>
   
    <span data-ttu-id="85942-204">a.</span><span class="sxs-lookup"><span data-stu-id="85942-204">a.</span></span> <span data-ttu-id="85942-205">Typen hello **användarnamn eller e-postadress** av en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="85942-205">Type hello **Usernames or Email Address** of a valid AAD account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="85942-206">b.</span><span class="sxs-lookup"><span data-stu-id="85942-206">b.</span></span> <span data-ttu-id="85942-207">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="85942-207">Click **Send**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="85942-208">hello Azure Active Directory användare får ett e-postmeddelande inklusive en länk tooconfirm hello innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="85942-208">hello Azure Active Directory account holder receives an email including a link tooconfirm hello account before it becomes active.</span></span>
    
> [!NOTE]
> <span data-ttu-id="85942-209">Du kan använda något annat Wikispaces användarens konto skapas verktyg eller API: er som tillhandahålls av Wikispaces tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="85942-209">You can use any other Wikispaces user account creation tools or APIs provided by Wikispaces tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="85942-210">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="85942-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="85942-211">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWikispaces.</span><span class="sxs-lookup"><span data-stu-id="85942-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWikispaces.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="85942-213">**tooassign Britta Simon tooWikispaces utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="85942-213">**tooassign Britta Simon tooWikispaces, perform hello following steps:**</span></span>

1. <span data-ttu-id="85942-214">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="85942-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="85942-216">Välj i listan med program hello **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="85942-216">In hello applications list, select **Wikispaces**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_app.png) 

3. <span data-ttu-id="85942-218">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="85942-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="85942-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="85942-220">Click **Add** button.</span></span> <span data-ttu-id="85942-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85942-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="85942-223">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="85942-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="85942-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85942-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="85942-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="85942-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="85942-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="85942-226">Testing single sign-on</span></span>

<span data-ttu-id="85942-227">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="85942-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="85942-228">Du bör få automatiskt inloggade tooyour Wikispaces programmet när du klickar på hello Wikispaces panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="85942-228">When you click hello Wikispaces tile in hello Access Panel, you should get automatically signed-on tooyour Wikispaces application.</span></span>
<span data-ttu-id="85942-229">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="85942-229">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85942-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="85942-230">Additional resources</span></span>

* [<span data-ttu-id="85942-231">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="85942-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="85942-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="85942-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_203.png

