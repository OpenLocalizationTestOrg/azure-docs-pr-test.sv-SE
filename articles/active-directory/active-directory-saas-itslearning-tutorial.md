---
title: "Självstudier: Azure Active Directory-integrering med itslearning | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och itslearning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 60587ba3-1396-4b8a-9ac1-e22a98e5e0ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 4ee6c8d450cc3972a87da67fc79890473cfa498a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itslearning"></a><span data-ttu-id="62a37-103">Självstudier: Azure Active Directory-integrering med itslearning</span><span class="sxs-lookup"><span data-stu-id="62a37-103">Tutorial: Azure Active Directory integration with itslearning</span></span>

<span data-ttu-id="62a37-104">I kursen får du lära dig hur toointegrate itslearning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="62a37-104">In this tutorial, you learn how toointegrate itslearning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="62a37-105">Integrera itslearning med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="62a37-105">Integrating itslearning with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="62a37-106">Du kan styra i Azure AD som har åtkomst till tooitslearning</span><span class="sxs-lookup"><span data-stu-id="62a37-106">You can control in Azure AD who has access tooitslearning</span></span>
- <span data-ttu-id="62a37-107">Du kan aktivera din användare tooautomatically get inloggade tooitslearning (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="62a37-107">You can enable your users tooautomatically get signed-on tooitslearning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="62a37-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="62a37-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="62a37-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="62a37-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62a37-110">Krav</span><span class="sxs-lookup"><span data-stu-id="62a37-110">Prerequisites</span></span>

<span data-ttu-id="62a37-111">tooconfigure Azure AD-integrering med itslearning, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="62a37-111">tooconfigure Azure AD integration with itslearning, you need hello following items:</span></span>

- <span data-ttu-id="62a37-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="62a37-112">An Azure AD subscription</span></span>
- <span data-ttu-id="62a37-113">En itslearning enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="62a37-113">An itslearning single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="62a37-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="62a37-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="62a37-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="62a37-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="62a37-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="62a37-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="62a37-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="62a37-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="62a37-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="62a37-118">Scenario description</span></span>
<span data-ttu-id="62a37-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="62a37-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="62a37-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="62a37-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="62a37-121">Att lägga till itslearning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="62a37-121">Adding itslearning from hello gallery</span></span>
2. <span data-ttu-id="62a37-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a37-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itslearning-from-hello-gallery"></a><span data-ttu-id="62a37-123">Att lägga till itslearning från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="62a37-123">Adding itslearning from hello gallery</span></span>
<span data-ttu-id="62a37-124">tooconfigure hello integrering av itslearning i Azure AD, behöver du tooadd itslearning hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="62a37-124">tooconfigure hello integration of itslearning into Azure AD, you need tooadd itslearning from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="62a37-125">**tooadd itslearning från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="62a37-125">**tooadd itslearning from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a37-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="62a37-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="62a37-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="62a37-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="62a37-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="62a37-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="62a37-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a37-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="62a37-133">Skriv i sökrutan hello **itslearning**.</span><span class="sxs-lookup"><span data-stu-id="62a37-133">In hello search box, type **itslearning**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_search.png)

5. <span data-ttu-id="62a37-135">Markera hello resultat på panelen **itslearning**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="62a37-135">In hello results panel, select **itslearning**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="62a37-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a37-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="62a37-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med itslearning baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="62a37-138">In this section, you configure and test Azure AD single sign-on with itslearning based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="62a37-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i itslearning är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62a37-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in itslearning is tooa user in Azure AD.</span></span> <span data-ttu-id="62a37-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i itslearning toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="62a37-140">In other words, a link relationship between an Azure AD user and hello related user in itslearning needs toobe established.</span></span>

<span data-ttu-id="62a37-141">I itslearning, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="62a37-141">In itslearning, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="62a37-142">tooconfigure och testa Azure AD enkel inloggning med itslearning, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="62a37-142">tooconfigure and test Azure AD single sign-on with itslearning, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="62a37-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="62a37-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="62a37-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="62a37-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="62a37-145">**[Skapa en testanvändare itslearning](#creating-an-itslearning-test-user)**  -toohave en motsvarighet för Britta Simon i itslearning som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="62a37-145">**[Creating an itslearning test user](#creating-an-itslearning-test-user)** - toohave a counterpart of Britta Simon in itslearning that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="62a37-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="62a37-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="62a37-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="62a37-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="62a37-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a37-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="62a37-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt itslearning program.</span><span class="sxs-lookup"><span data-stu-id="62a37-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your itslearning application.</span></span>

<span data-ttu-id="62a37-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med itslearning:**</span><span class="sxs-lookup"><span data-stu-id="62a37-150">**tooconfigure Azure AD single sign-on with itslearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a37-151">I hello Azure-portalen på hello **itslearning** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="62a37-151">In hello Azure portal, on hello **itslearning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="62a37-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="62a37-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_samlbase.png)

3. <span data-ttu-id="62a37-155">På hello **itslearning domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="62a37-155">On hello **itslearning Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_url.png)

    <span data-ttu-id="62a37-157">a.</span><span class="sxs-lookup"><span data-stu-id="62a37-157">a.</span></span> <span data-ttu-id="62a37-158">I hello **inloggnings-URL** textruta Skriv en URL som:</span><span class="sxs-lookup"><span data-stu-id="62a37-158">In hello **Sign-on URL** textbox, type a URL as:</span></span>
    | |
    |--| 
    | `https://www.itslearning.com/index.aspx`|
    | `https://us1.itslearning.com/index.aspx`|

    <span data-ttu-id="62a37-159">b.</span><span class="sxs-lookup"><span data-stu-id="62a37-159">b.</span></span> <span data-ttu-id="62a37-160">I hello **identifierare** textruta Skriv en URL som:`urn:mace:saml2v2.no:services:com.itslearning`</span><span class="sxs-lookup"><span data-stu-id="62a37-160">In hello **Identifier** textbox, type a URL as: `urn:mace:saml2v2.no:services:com.itslearning`</span></span>

4. <span data-ttu-id="62a37-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="62a37-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_certificate.png) 

5. <span data-ttu-id="62a37-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="62a37-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itslearning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="62a37-165">tooconfigure enkel inloggning på **itslearning** sida, behöver du toosend hello hämtas **XML-Metadata för** för[itslearning supportteamet](mailto:support@itslearning.com).</span><span class="sxs-lookup"><span data-stu-id="62a37-165">tooconfigure single sign-on on **itslearning** side, you need toosend hello downloaded **Metadata XML** too[itslearning support team](mailto:support@itslearning.com).</span></span> <span data-ttu-id="62a37-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="62a37-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="62a37-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="62a37-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="62a37-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="62a37-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="62a37-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="62a37-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="62a37-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="62a37-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="62a37-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="62a37-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="62a37-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="62a37-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a37-174">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="62a37-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="62a37-176">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="62a37-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="62a37-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a37-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="62a37-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="62a37-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itslearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="62a37-182">a.</span><span class="sxs-lookup"><span data-stu-id="62a37-182">a.</span></span> <span data-ttu-id="62a37-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="62a37-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="62a37-184">b.</span><span class="sxs-lookup"><span data-stu-id="62a37-184">b.</span></span> <span data-ttu-id="62a37-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="62a37-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="62a37-186">c.</span><span class="sxs-lookup"><span data-stu-id="62a37-186">c.</span></span> <span data-ttu-id="62a37-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="62a37-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="62a37-188">d.</span><span class="sxs-lookup"><span data-stu-id="62a37-188">d.</span></span> <span data-ttu-id="62a37-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="62a37-189">Click **Create**.</span></span>
 
### <a name="creating-an-itslearning-test-user"></a><span data-ttu-id="62a37-190">Skapa en testanvändare itslearning</span><span class="sxs-lookup"><span data-stu-id="62a37-190">Creating an itslearning test user</span></span>

<span data-ttu-id="62a37-191">I det här avsnittet skapar du en användare som kallas Britta Simon i itslearning.</span><span class="sxs-lookup"><span data-stu-id="62a37-191">In this section, you create a user called Britta Simon in itslearning.</span></span> <span data-ttu-id="62a37-192">Arbeta med [itslearning klienten supportteamet](mailto:support@itslearning.com) att lägga till hello användare i hello itslearning plattform.</span><span class="sxs-lookup"><span data-stu-id="62a37-192">Work with [itslearning Client support team](mailto:support@itslearning.com) to add hello users in hello itslearning platform.</span></span> <span data-ttu-id="62a37-193">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="62a37-193">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="62a37-194">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="62a37-194">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="62a37-195">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooitslearning.</span><span class="sxs-lookup"><span data-stu-id="62a37-195">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooitslearning.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="62a37-197">**tooassign Britta Simon tooitslearning utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="62a37-197">**tooassign Britta Simon tooitslearning, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a37-198">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="62a37-198">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="62a37-200">Välj i listan med program hello **itslearning**.</span><span class="sxs-lookup"><span data-stu-id="62a37-200">In hello applications list, select **itslearning**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itslearning-tutorial/tutorial_itslearning_app.png) 

3. <span data-ttu-id="62a37-202">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="62a37-202">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="62a37-204">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="62a37-204">Click **Add** button.</span></span> <span data-ttu-id="62a37-205">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a37-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="62a37-207">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="62a37-207">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="62a37-208">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a37-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="62a37-209">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a37-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="62a37-210">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a37-210">Testing single sign-on</span></span>

<span data-ttu-id="62a37-211">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="62a37-211">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="62a37-212">När du klickar på hello itslearning panelen i hello åtkomstpanelen bör du hämta inloggningssidan för itslearning program.</span><span class="sxs-lookup"><span data-stu-id="62a37-212">When you click hello itslearning tile in hello Access Panel, you should get login page of itslearning application.</span></span> <span data-ttu-id="62a37-213">Klicka på **logga in med Windows Azure ACS1** för lyckad inloggning till hello program.</span><span class="sxs-lookup"><span data-stu-id="62a37-213">Click **Log in with Windows Azure ACS1** for successful login into hello application.</span></span>

  ![Inloggning](./media/active-directory-saas-itslearning-tutorial/login.png)

<span data-ttu-id="62a37-215">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="62a37-215">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62a37-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="62a37-216">Additional resources</span></span>

* [<span data-ttu-id="62a37-217">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62a37-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="62a37-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="62a37-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itslearning-tutorial/tutorial_general_203.png

