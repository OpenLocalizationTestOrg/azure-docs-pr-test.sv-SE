---
title: "Självstudier: Azure Active Directory-integrering med Direct | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och direkt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c2cd1f0-d14c-42f0-94a8-9b800008b285
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ac663070b39e55eade2c43814b63a9d0374c7316
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-direct"></a><span data-ttu-id="44c7c-103">Självstudier: Azure Active Directory-integrering med Direct</span><span class="sxs-lookup"><span data-stu-id="44c7c-103">Tutorial: Azure Active Directory integration with Direct</span></span>

<span data-ttu-id="44c7c-104">I kursen får du lära dig hur toointegrate direkt med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="44c7c-104">In this tutorial, you learn how toointegrate Direct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="44c7c-105">Integrera direkt med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="44c7c-105">Integrating Direct with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="44c7c-106">Du kan styra i Azure AD som har åtkomst till tooDirect</span><span class="sxs-lookup"><span data-stu-id="44c7c-106">You can control in Azure AD who has access tooDirect</span></span>
- <span data-ttu-id="44c7c-107">Du kan aktivera din användare tooautomatically get inloggade tooDirect (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="44c7c-107">You can enable your users tooautomatically get signed-on tooDirect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="44c7c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="44c7c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="44c7c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="44c7c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44c7c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="44c7c-110">Prerequisites</span></span>

<span data-ttu-id="44c7c-111">tooconfigure Azure AD-integrering med direkta, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="44c7c-111">tooconfigure Azure AD integration with Direct, you need hello following items:</span></span>

- <span data-ttu-id="44c7c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="44c7c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="44c7c-113">En direkt enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="44c7c-113">A Direct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="44c7c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="44c7c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="44c7c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="44c7c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="44c7c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="44c7c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="44c7c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="44c7c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="44c7c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="44c7c-118">Scenario description</span></span>
<span data-ttu-id="44c7c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="44c7c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="44c7c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="44c7c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="44c7c-121">Lägger till direkt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="44c7c-121">Adding Direct from hello gallery</span></span>
2. <span data-ttu-id="44c7c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44c7c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-direct-from-hello-gallery"></a><span data-ttu-id="44c7c-123">Lägger till direkt från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="44c7c-123">Adding Direct from hello gallery</span></span>
<span data-ttu-id="44c7c-124">tooconfigure hello integrering av direkt i Azure AD, behöver du tooadd direkt från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="44c7c-124">tooconfigure hello integration of Direct into Azure AD, you need tooadd Direct from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="44c7c-125">**tooadd direkt från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44c7c-125">**tooadd Direct from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="44c7c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="44c7c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="44c7c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="44c7c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="44c7c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44c7c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="44c7c-133">Skriv i sökrutan hello **direkt**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-133">In hello search box, type **Direct**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_search.png)

5. <span data-ttu-id="44c7c-135">Markera hello resultat på panelen **direkt**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="44c7c-135">In hello results panel, select **Direct**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/tutorial_direct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="44c7c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44c7c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="44c7c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Direct baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="44c7c-138">In this section, you configure and test Azure AD single sign-on with Direct based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="44c7c-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Direct är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="44c7c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Direct is tooa user in Azure AD.</span></span> <span data-ttu-id="44c7c-140">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i direkta behov toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="44c7c-140">In other words, a link relationship between an Azure AD user and hello related user in Direct needs toobe established.</span></span>

<span data-ttu-id="44c7c-141">I direkt tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="44c7c-141">In Direct, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="44c7c-142">tooconfigure och testa Azure AD enkel inloggning med direkta, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="44c7c-142">tooconfigure and test Azure AD single sign-on with Direct, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="44c7c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="44c7c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="44c7c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44c7c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="44c7c-145">**[Skapa en direkt testanvändare](#creating-a-direct-test-user)**  -toohave en motsvarighet för Britta Simon i Direct som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="44c7c-145">**[Creating a Direct test user](#creating-a-direct-test-user)** - toohave a counterpart of Britta Simon in Direct that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="44c7c-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="44c7c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="44c7c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="44c7c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="44c7c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44c7c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="44c7c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet direkt.</span><span class="sxs-lookup"><span data-stu-id="44c7c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Direct application.</span></span>

<span data-ttu-id="44c7c-150">**tooconfigure Azure AD enkel inloggning med direkta, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="44c7c-150">**tooconfigure Azure AD single sign-on with Direct, perform hello following steps:**</span></span>

1. <span data-ttu-id="44c7c-151">I hello Azure-portalen på hello **direkt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-151">In hello Azure portal, on hello **Direct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="44c7c-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="44c7c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_samlbase.png)

3. <span data-ttu-id="44c7c-155">På hello **direkt domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="44c7c-155">On hello **Direct Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_url.png)

    <span data-ttu-id="44c7c-157">I hello **identifierare** textruta typen hello URL:`https://direct4b.com/`</span><span class="sxs-lookup"><span data-stu-id="44c7c-157">In hello **Identifier** textbox, type hello URL: `https://direct4b.com/`</span></span>

4. <span data-ttu-id="44c7c-158">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="44c7c-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_url1.png)

     <span data-ttu-id="44c7c-160">I hello **inloggnings-URL** textruta typen hello URL:`https://direct4b.com/sso`</span><span class="sxs-lookup"><span data-stu-id="44c7c-160">In hello **Sign-on URL** textbox, type hello URL: `https://direct4b.com/sso`</span></span> 
    
5. <span data-ttu-id="44c7c-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="44c7c-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_certificate.png) 

6. <span data-ttu-id="44c7c-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="44c7c-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="44c7c-165">tooconfigure enkel inloggning på **direkt** sida, behöver du toosend hello hämtas **XML-Metadata för** för[direkt supportteamet](https://direct4b.com/ja/support.html#inquiry).</span><span class="sxs-lookup"><span data-stu-id="44c7c-165">tooconfigure single sign-on on **Direct** side, you need toosend hello downloaded **Metadata XML** too[Direct support team](https://direct4b.com/ja/support.html#inquiry).</span></span> 

> [!TIP]
> <span data-ttu-id="44c7c-166">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="44c7c-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="44c7c-167">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="44c7c-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="44c7c-168">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="44c7c-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="44c7c-169">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="44c7c-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="44c7c-170">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="44c7c-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="44c7c-172">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44c7c-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="44c7c-173">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="44c7c-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="44c7c-175">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-175">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="44c7c-177">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44c7c-177">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="44c7c-179">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="44c7c-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-direct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="44c7c-181">a.</span><span class="sxs-lookup"><span data-stu-id="44c7c-181">a.</span></span> <span data-ttu-id="44c7c-182">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-182">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="44c7c-183">b.</span><span class="sxs-lookup"><span data-stu-id="44c7c-183">b.</span></span> <span data-ttu-id="44c7c-184">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="44c7c-184">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="44c7c-185">c.</span><span class="sxs-lookup"><span data-stu-id="44c7c-185">c.</span></span> <span data-ttu-id="44c7c-186">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="44c7c-187">d.</span><span class="sxs-lookup"><span data-stu-id="44c7c-187">d.</span></span> <span data-ttu-id="44c7c-188">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-188">Click **Create**.</span></span>
 
### <a name="creating-a-direct-test-user"></a><span data-ttu-id="44c7c-189">Skapa en direkt testanvändare</span><span class="sxs-lookup"><span data-stu-id="44c7c-189">Creating a Direct test user</span></span>

<span data-ttu-id="44c7c-190">I det här avsnittet kan du skapa en användare som kallas Britta Simon i direkt.</span><span class="sxs-lookup"><span data-stu-id="44c7c-190">In this section, you create a user called Britta Simon in Direct.</span></span> <span data-ttu-id="44c7c-191">Arbeta med [direkt supportteamet](https://direct4b.com/ja/support.html#inquiry) att lägga till hello användare i hello direkt plattform.</span><span class="sxs-lookup"><span data-stu-id="44c7c-191">Work with [Direct support team](https://direct4b.com/ja/support.html#inquiry) to add hello users in hello Direct platform.</span></span> <span data-ttu-id="44c7c-192">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="44c7c-192">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="44c7c-193">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="44c7c-193">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="44c7c-194">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDirect.</span><span class="sxs-lookup"><span data-stu-id="44c7c-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDirect.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="44c7c-196">**tooassign Britta Simon tooDirect utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="44c7c-196">**tooassign Britta Simon tooDirect, perform hello following steps:**</span></span>

1. <span data-ttu-id="44c7c-197">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="44c7c-199">Välj i listan med program hello **direkt**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-199">In hello applications list, select **Direct**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-direct-tutorial/tutorial_direct_app.png) 

3. <span data-ttu-id="44c7c-201">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="44c7c-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="44c7c-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="44c7c-203">Click **Add** button.</span></span> <span data-ttu-id="44c7c-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44c7c-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="44c7c-206">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="44c7c-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="44c7c-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44c7c-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="44c7c-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="44c7c-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="44c7c-209">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="44c7c-209">Testing single sign-on</span></span>

<span data-ttu-id="44c7c-210">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="44c7c-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="44c7c-211">Om du inte vill tootest i **IDP initierade läge**:</span><span class="sxs-lookup"><span data-stu-id="44c7c-211">If you wish tootest in **IDP Initiated Mode**:</span></span>

    <span data-ttu-id="44c7c-212">När du klickar på hello **direkt** panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour **direkt** program.</span><span class="sxs-lookup"><span data-stu-id="44c7c-212">When you click hello **Direct** tile in hello Access Panel, you should get automatically signed-on tooyour **Direct** application.</span></span>

2. <span data-ttu-id="44c7c-213">Om du inte vill tootest i **SP-initierad läge**:</span><span class="sxs-lookup"><span data-stu-id="44c7c-213">If you wish tootest in **SP Initiated Mode**:</span></span>
    
    <span data-ttu-id="44c7c-214">a.</span><span class="sxs-lookup"><span data-stu-id="44c7c-214">a.</span></span> <span data-ttu-id="44c7c-215">Klicka på hello **direkt** panelen i hello åtkomstpanelen och du kommer att omdirigerade toohello inloggning sidan program.</span><span class="sxs-lookup"><span data-stu-id="44c7c-215">Click on hello **Direct** tile in hello Access Panel and you will be redirected toohello application sign-on page.</span></span>

    <span data-ttu-id="44c7c-216">b.</span><span class="sxs-lookup"><span data-stu-id="44c7c-216">b.</span></span> <span data-ttu-id="44c7c-217">Ange din `subdomain` i hello textrutan som visas och tryck på '次へ (nästa), och du bör få automatiskt inloggade tooyour **direkt** program.</span><span class="sxs-lookup"><span data-stu-id="44c7c-217">Input your `subdomain` in hello textbox displayed and press '次へ (Next)' and you should get automatically signed-on tooyour **Direct** application .</span></span>
    
<span data-ttu-id="44c7c-218">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="44c7c-218">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="44c7c-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="44c7c-219">Additional resources</span></span>

* [<span data-ttu-id="44c7c-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="44c7c-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="44c7c-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="44c7c-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-direct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-direct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-direct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-direct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-direct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-direct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-direct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-direct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-direct-tutorial/tutorial_general_203.png

