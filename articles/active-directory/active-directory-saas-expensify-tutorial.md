---
title: "Självstudier: Azure Active Directory-integrering med Expensify | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Expensify."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: jeedes
ms.openlocfilehash: 141513ef27c90dae2d77a52ecab2f89c4e5a55ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a><span data-ttu-id="bd96b-103">Självstudier: Azure Active Directory-integrering med Expensify</span><span class="sxs-lookup"><span data-stu-id="bd96b-103">Tutorial: Azure Active Directory integration with Expensify</span></span>

<span data-ttu-id="bd96b-104">I kursen får du lära dig hur toointegrate Expensify med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bd96b-104">In this tutorial, you learn how toointegrate Expensify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bd96b-105">Integrera Expensify med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bd96b-105">Integrating Expensify with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="bd96b-106">Du kan styra i Azure AD som har åtkomst till tooExpensify</span><span class="sxs-lookup"><span data-stu-id="bd96b-106">You can control in Azure AD who has access tooExpensify</span></span>
- <span data-ttu-id="bd96b-107">Du kan aktivera din användare tooautomatically get inloggade tooExpensify (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bd96b-107">You can enable your users tooautomatically get signed-on tooExpensify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bd96b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bd96b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="bd96b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bd96b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bd96b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bd96b-110">Prerequisites</span></span>

<span data-ttu-id="bd96b-111">tooconfigure Azure AD-integrering med Expensify, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="bd96b-111">tooconfigure Azure AD integration with Expensify, you need hello following items:</span></span>

- <span data-ttu-id="bd96b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd96b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bd96b-113">En Expensify enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="bd96b-113">An Expensify single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bd96b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd96b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bd96b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bd96b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bd96b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bd96b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bd96b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bd96b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bd96b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bd96b-118">Scenario description</span></span>
<span data-ttu-id="bd96b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bd96b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bd96b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd96b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bd96b-121">Att lägga till Expensify från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bd96b-121">Adding Expensify from hello gallery</span></span>
2. <span data-ttu-id="bd96b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd96b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-expensify-from-hello-gallery"></a><span data-ttu-id="bd96b-123">Att lägga till Expensify från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="bd96b-123">Adding Expensify from hello gallery</span></span>
<span data-ttu-id="bd96b-124">tooconfigure hello integrering av Expensify i Azure AD, behöver du tooadd Expensify hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="bd96b-124">tooconfigure hello integration of Expensify into Azure AD, you need tooadd Expensify from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="bd96b-125">**tooadd Expensify från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd96b-125">**tooadd Expensify from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd96b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd96b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bd96b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="bd96b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bd96b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd96b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bd96b-133">Skriv i sökrutan hello **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-133">In hello search box, type **Expensify**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_search.png)

5. <span data-ttu-id="bd96b-135">Markera hello resultat på panelen **Expensify**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="bd96b-135">In hello results panel, select **Expensify**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bd96b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd96b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bd96b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Expensify baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bd96b-138">In this section, you configure and test Azure AD single sign-on with Expensify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bd96b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Expensify är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bd96b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Expensify is tooa user in Azure AD.</span></span> <span data-ttu-id="bd96b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Expensify toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="bd96b-140">In other words, a link relationship between an Azure AD user and hello related user in Expensify needs toobe established.</span></span>

<span data-ttu-id="bd96b-141">I Expensify, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="bd96b-141">In Expensify, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="bd96b-142">tooconfigure och testa Azure AD enkel inloggning med Expensify, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bd96b-142">tooconfigure and test Azure AD single sign-on with Expensify, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="bd96b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bd96b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="bd96b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd96b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bd96b-145">**[Skapa en testanvändare Expensify](#creating-an-expensify-test-user)**  -toohave en motsvarighet för Britta Simon i Expensify som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bd96b-145">**[Creating an Expensify test user](#creating-an-expensify-test-user)** - toohave a counterpart of Britta Simon in Expensify that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="bd96b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd96b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bd96b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bd96b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bd96b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd96b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bd96b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Expensify program.</span><span class="sxs-lookup"><span data-stu-id="bd96b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Expensify application.</span></span>

<span data-ttu-id="bd96b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Expensify:**</span><span class="sxs-lookup"><span data-stu-id="bd96b-150">**tooconfigure Azure AD single sign-on with Expensify, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd96b-151">I hello Azure-portalen på hello **Expensify** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-151">In hello Azure portal, on hello **Expensify** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bd96b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bd96b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_samlbase.png)

3. <span data-ttu-id="bd96b-155">På hello **Expensify domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd96b-155">On hello **Expensify Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_url.png)

    <span data-ttu-id="bd96b-157">a.</span><span class="sxs-lookup"><span data-stu-id="bd96b-157">a.</span></span> <span data-ttu-id="bd96b-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.expensify.com/authentication/saml/login`</span><span class="sxs-lookup"><span data-stu-id="bd96b-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://www.expensify.com/authentication/saml/login`</span></span>

    <span data-ttu-id="bd96b-159">b.</span><span class="sxs-lookup"><span data-stu-id="bd96b-159">b.</span></span> <span data-ttu-id="bd96b-160">I hello **Identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://www.<companyname>.expensify.com/`</span><span class="sxs-lookup"><span data-stu-id="bd96b-160">In hello **Identifier URL** textbox, type a URL using hello following pattern: `https://www.<companyname>.expensify.com/`</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="bd96b-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bd96b-161">These values are not real.</span></span> <span data-ttu-id="bd96b-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare URL.</span><span class="sxs-lookup"><span data-stu-id="bd96b-162">Update these values with hello actual Sign-On URL and Identifier URL.</span></span> <span data-ttu-id="bd96b-163">Kontakta [Expensify klienten supportteamet](mailto:help@expensify.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="bd96b-163">Contact [Expensify Client support team](mailto:help@expensify.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="bd96b-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="bd96b-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_certificate.png) 

5. <span data-ttu-id="bd96b-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd96b-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bd96b-168">tooenable SSO i Expensify, måste du först tooenable **domänens kontroll** i hello program.</span><span class="sxs-lookup"><span data-stu-id="bd96b-168">tooenable SSO in Expensify, you first need tooenable **Domain Control** in hello application.</span></span> <span data-ttu-id="bd96b-169">Du kan aktivera domänens kontroll i hello program via hello steg [här](http://help.expensify.com/domain-control).</span><span class="sxs-lookup"><span data-stu-id="bd96b-169">You can enable Domain Control in hello application through hello steps listed [here](http://help.expensify.com/domain-control).</span></span> <span data-ttu-id="bd96b-170">Arbeta med ytterligare support [Expensify klienten supportteamet](mailto:help@expensify.com).</span><span class="sxs-lookup"><span data-stu-id="bd96b-170">For additional support, work with [Expensify Client support team](mailto:help@expensify.com).</span></span> <span data-ttu-id="bd96b-171">När du har domän kontroll är aktiverat, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="bd96b-171">Once you have Domain Control enabled, follow these steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png)
    
    <span data-ttu-id="bd96b-173">a.</span><span class="sxs-lookup"><span data-stu-id="bd96b-173">a.</span></span> <span data-ttu-id="bd96b-174">Logga in tooyour Expensify program.</span><span class="sxs-lookup"><span data-stu-id="bd96b-174">Sign on tooyour Expensify application.</span></span>
    
    <span data-ttu-id="bd96b-175">b.</span><span class="sxs-lookup"><span data-stu-id="bd96b-175">b.</span></span> <span data-ttu-id="bd96b-176">Klicka i hello verktygsfältet hello längst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-176">In hello toolbar on hello top, click **Admin**.</span></span>
    
    <span data-ttu-id="bd96b-177">c.</span><span class="sxs-lookup"><span data-stu-id="bd96b-177">c.</span></span> <span data-ttu-id="bd96b-178">I hello vänstra panelen, klickar du på **domän**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-178">In hello left panel, click **Domain**.</span></span>
    
    <span data-ttu-id="bd96b-179">d.</span><span class="sxs-lookup"><span data-stu-id="bd96b-179">d.</span></span> <span data-ttu-id="bd96b-180">Klicka på ett verifierat domännamn.</span><span class="sxs-lookup"><span data-stu-id="bd96b-180">Click your verified domain name.</span></span>
    
    <span data-ttu-id="bd96b-181">e.</span><span class="sxs-lookup"><span data-stu-id="bd96b-181">e.</span></span> <span data-ttu-id="bd96b-182">I hello vänstra panelen, klickar du på **SAML**, och välj sedan **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-182">In hello left panel, click **SAML**, and then select **Enabled**.</span></span>
    
    <span data-ttu-id="bd96b-183">f.</span><span class="sxs-lookup"><span data-stu-id="bd96b-183">f.</span></span> <span data-ttu-id="bd96b-184">Öppna hello hämtas Federation Metadata från Azure AD i anteckningar, kopiera hello innehåll, och klistra in den i hello **identitet providern Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="bd96b-184">Open hello downloaded Federation Metadata from Azure AD in notepad, copy hello content, and then paste it into hello **Identity Provider Metadata** textbox.</span></span>

> [!TIP]
> <span data-ttu-id="bd96b-185">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="bd96b-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="bd96b-186">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="bd96b-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="bd96b-187">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bd96b-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bd96b-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd96b-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="bd96b-189">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bd96b-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bd96b-191">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd96b-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd96b-192">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bd96b-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bd96b-194">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bd96b-196">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd96b-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bd96b-198">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bd96b-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bd96b-200">a.</span><span class="sxs-lookup"><span data-stu-id="bd96b-200">a.</span></span> <span data-ttu-id="bd96b-201">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bd96b-202">b.</span><span class="sxs-lookup"><span data-stu-id="bd96b-202">b.</span></span> <span data-ttu-id="bd96b-203">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bd96b-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bd96b-204">c.</span><span class="sxs-lookup"><span data-stu-id="bd96b-204">c.</span></span> <span data-ttu-id="bd96b-205">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="bd96b-206">d.</span><span class="sxs-lookup"><span data-stu-id="bd96b-206">d.</span></span> <span data-ttu-id="bd96b-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-207">Click **Create**.</span></span>
 
### <a name="creating-an-expensify-test-user"></a><span data-ttu-id="bd96b-208">Skapa en testanvändare Expensify</span><span class="sxs-lookup"><span data-stu-id="bd96b-208">Creating an Expensify test user</span></span>

<span data-ttu-id="bd96b-209">I det här avsnittet skapar du en användare som kallas Britta Simon i Expensify.</span><span class="sxs-lookup"><span data-stu-id="bd96b-209">In this section, you create a user called Britta Simon in Expensify.</span></span> <span data-ttu-id="bd96b-210">Arbeta med [Expensify klienten supportteamet](mailto:help@expensify.com) tooadd hello användare i hello Expensify plattform.</span><span class="sxs-lookup"><span data-stu-id="bd96b-210">Work with [Expensify Client support team](mailto:help@expensify.com) tooadd hello users in hello Expensify platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="bd96b-211">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="bd96b-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="bd96b-212">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooExpensify.</span><span class="sxs-lookup"><span data-stu-id="bd96b-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooExpensify.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bd96b-214">**tooassign Britta Simon tooExpensify utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bd96b-214">**tooassign Britta Simon tooExpensify, perform hello following steps:**</span></span>

1. <span data-ttu-id="bd96b-215">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-215">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bd96b-217">Välj i listan med program hello **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-217">In hello applications list, select **Expensify**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_app.png) 

3. <span data-ttu-id="bd96b-219">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bd96b-219">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bd96b-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bd96b-221">Click **Add** button.</span></span> <span data-ttu-id="bd96b-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd96b-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bd96b-224">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="bd96b-224">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="bd96b-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd96b-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bd96b-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bd96b-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bd96b-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bd96b-227">Testing single sign-on</span></span>

<span data-ttu-id="bd96b-228">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bd96b-228">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="bd96b-229">Du bör få automatiskt inloggade tooyour Expensify programmet när du klickar på hello Expensify panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bd96b-229">When you click hello Expensify tile in hello Access Panel, you should get automatically signed-on tooyour Expensify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd96b-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bd96b-230">Additional resources</span></span>

* [<span data-ttu-id="bd96b-231">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bd96b-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bd96b-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bd96b-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png

