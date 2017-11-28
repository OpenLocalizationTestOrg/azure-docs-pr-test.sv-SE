---
title: "Självstudier: Azure Active Directory-integrering med kontrakt ASC | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ASC kontrakt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7f54202-1581-4e55-a97e-02633ff9382d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: jeedes
ms.openlocfilehash: 8320af8acfda3e3d37e589c9887cd697d5ab651c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asc-contracts"></a><span data-ttu-id="f4a9b-103">Självstudier: Azure Active Directory-integrering med ASC kontrakt</span><span class="sxs-lookup"><span data-stu-id="f4a9b-103">Tutorial: Azure Active Directory integration with ASC Contracts</span></span>

<span data-ttu-id="f4a9b-104">I kursen får du lära dig hur toointegrate ASC kontrakt med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f4a9b-104">In this tutorial, you learn how toointegrate ASC Contracts with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f4a9b-105">Integrera ASC kontrakt med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f4a9b-105">Integrating ASC Contracts with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f4a9b-106">Du kan styra i Azure AD som har åtkomst tooASC kontrakt</span><span class="sxs-lookup"><span data-stu-id="f4a9b-106">You can control in Azure AD who has access tooASC Contracts</span></span>
- <span data-ttu-id="f4a9b-107">Du kan aktivera din användare tooautomatically hämta inloggade tooASC kontrakt (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f4a9b-107">You can enable your users tooautomatically get signed-on tooASC Contracts (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f4a9b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f4a9b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f4a9b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f4a9b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4a9b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f4a9b-110">Prerequisites</span></span>

<span data-ttu-id="f4a9b-111">tooconfigure Azure AD-integrering med ASC kontrakt måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="f4a9b-111">tooconfigure Azure AD integration with ASC Contracts, you need hello following items:</span></span>

- <span data-ttu-id="f4a9b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f4a9b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f4a9b-113">Ett kontrakt som ASC enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f4a9b-113">An ASC Contracts single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f4a9b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f4a9b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f4a9b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f4a9b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f4a9b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f4a9b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f4a9b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f4a9b-118">Scenario description</span></span>
<span data-ttu-id="f4a9b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f4a9b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f4a9b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f4a9b-121">Att lägga till ASC kontrakt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f4a9b-121">Adding ASC Contracts from hello gallery</span></span>
2. <span data-ttu-id="f4a9b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4a9b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asc-contracts-from-hello-gallery"></a><span data-ttu-id="f4a9b-123">Att lägga till ASC kontrakt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="f4a9b-123">Adding ASC Contracts from hello gallery</span></span>
<span data-ttu-id="f4a9b-124">tooconfigure hello integrering av ASC kontrakt i Azure AD, behöver du tooadd ASC kontrakt hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-124">tooconfigure hello integration of ASC Contracts into Azure AD, you need tooadd ASC Contracts from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f4a9b-125">**tooadd ASC kontrakt från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f4a9b-125">**tooadd ASC Contracts from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4a9b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f4a9b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f4a9b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f4a9b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f4a9b-133">Skriv i sökrutan hello **ASC kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-133">In hello search box, type **ASC Contracts**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_search.png)

5. <span data-ttu-id="f4a9b-135">Markera hello resultat på panelen **ASC kontrakt**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-135">In hello results panel, select **ASC Contracts**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f4a9b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4a9b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f4a9b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ASC kontrakt som bygger på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-138">In this section, you configure and test Azure AD single sign-on with ASC Contracts based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f4a9b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ASC kontrakt är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ASC Contracts is tooa user in Azure AD.</span></span> <span data-ttu-id="f4a9b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i ASC kontrakt toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-140">In other words, a link relationship between an Azure AD user and hello related user in ASC Contracts needs toobe established.</span></span>

<span data-ttu-id="f4a9b-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** ASC kontrakt.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ASC Contracts.</span></span>

<span data-ttu-id="f4a9b-142">tooconfigure och testa Azure AD enkel inloggning med ASC kontrakt, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f4a9b-142">tooconfigure and test Azure AD single sign-on with ASC Contracts, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f4a9b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f4a9b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f4a9b-145">**[Skapa en testanvändare ASC kontrakt](#creating-an-asc-contracts-test-user)**  -toohave en motsvarighet för Britta Simon i ASC kontrakt som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-145">**[Creating an ASC Contracts test user](#creating-an-asc-contracts-test-user)** - toohave a counterpart of Britta Simon in ASC Contracts that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f4a9b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f4a9b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f4a9b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4a9b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f4a9b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet ASC kontrakt.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ASC Contracts application.</span></span>

<span data-ttu-id="f4a9b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ASC kontrakt:**</span><span class="sxs-lookup"><span data-stu-id="f4a9b-150">**tooconfigure Azure AD single sign-on with ASC Contracts, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4a9b-151">I hello Azure-portalen på hello **ASC kontrakt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-151">In hello Azure portal, on hello **ASC Contracts** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f4a9b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_samlbase.png)

3. <span data-ttu-id="f4a9b-155">På hello **ASC kontrakt domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f4a9b-155">On hello **ASC Contracts Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_url.png)

    <span data-ttu-id="f4a9b-157">a.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-157">a.</span></span> <span data-ttu-id="f4a9b-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.asccontracts.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="f4a9b-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.asccontracts.com/shibboleth`</span></span>

    <span data-ttu-id="f4a9b-159">b.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-159">b.</span></span> <span data-ttu-id="f4a9b-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span><span class="sxs-lookup"><span data-stu-id="f4a9b-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f4a9b-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-161">These values are not real.</span></span> <span data-ttu-id="f4a9b-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="f4a9b-163">Kontakta ASC nätverk Inc. (ASC)-teamet på **613.599.6178** tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-163">Contact ASC Networks Inc. (ASC) team at **613.599.6178** tooget these values.</span></span>

4. <span data-ttu-id="f4a9b-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_certificate.png) 

5. <span data-ttu-id="f4a9b-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f4a9b-168">tooconfigure enkel inloggning på **ASC kontrakt** sida, anropa ASC nätverk Inc. (ASC) support på **613.599.6178** och ge dem hello hämtas **XML-Metadata för**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-168">tooconfigure single sign-on on **ASC Contracts** side, call ASC Networks Inc. (ASC) support at **613.599.6178** and provide them with hello downloaded **Metadata XML**.</span></span> <span data-ttu-id="f4a9b-169">De kan ange det här programmet in toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-169">They set this application up toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f4a9b-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="f4a9b-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f4a9b-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f4a9b-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f4a9b-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f4a9b-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4a9b-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="f4a9b-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f4a9b-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f4a9b-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4a9b-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f4a9b-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f4a9b-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f4a9b-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f4a9b-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f4a9b-185">a.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-185">a.</span></span> <span data-ttu-id="f4a9b-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f4a9b-187">b.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-187">b.</span></span> <span data-ttu-id="f4a9b-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f4a9b-189">c.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-189">c.</span></span> <span data-ttu-id="f4a9b-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f4a9b-191">d.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-191">d.</span></span> <span data-ttu-id="f4a9b-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-192">Click **Create**.</span></span>
 
### <a name="creating-an-asc-contracts-test-user"></a><span data-ttu-id="f4a9b-193">Skapa en testanvändare ASC kontrakt</span><span class="sxs-lookup"><span data-stu-id="f4a9b-193">Creating an ASC Contracts test user</span></span>

<span data-ttu-id="f4a9b-194">Arbeta med ASC nätverk Inc. (ASC) support på **613.599.6178** tooget hello användare som lagts till i hello ASC kontrakt plattform.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-194">Work with ASC Networks Inc. (ASC) support team at **613.599.6178** tooget hello users added in hello ASC Contracts platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f4a9b-195">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4a9b-195">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f4a9b-196">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooASC kontrakt.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-196">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooASC Contracts.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f4a9b-198">**tooassign Britta Simon tooASC kontrakt, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f4a9b-198">**tooassign Britta Simon tooASC Contracts, perform hello following steps:**</span></span>

1. <span data-ttu-id="f4a9b-199">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-199">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f4a9b-201">Välj i listan med program hello **ASC kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-201">In hello applications list, select **ASC Contracts**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_app.png) 

3. <span data-ttu-id="f4a9b-203">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-203">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f4a9b-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-205">Click **Add** button.</span></span> <span data-ttu-id="f4a9b-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f4a9b-208">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-208">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f4a9b-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f4a9b-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f4a9b-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4a9b-211">Testing single sign-on</span></span>

<span data-ttu-id="f4a9b-212">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-212">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f4a9b-213">Du bör få automatiskt inloggade tooyour ASC kontrakt programmet när du klickar på hello ASC kontrakt panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-213">When you click hello ASC Contracts tile in hello Access Panel, you should get automatically signed-on tooyour ASC Contracts application.</span></span> <span data-ttu-id="f4a9b-214">Mer information om åtkomstpanelen finns i.</span><span class="sxs-lookup"><span data-stu-id="f4a9b-214">For more information about the Access Panel, see.</span></span> <span data-ttu-id="f4a9b-215">[Introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="f4a9b-215">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4a9b-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f4a9b-216">Additional resources</span></span>

* [<span data-ttu-id="f4a9b-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f4a9b-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f4a9b-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f4a9b-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_203.png

