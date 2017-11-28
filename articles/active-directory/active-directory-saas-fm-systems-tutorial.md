---
title: "Självstudier: Azure Active Directory-integrering med FM:Systems | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och FM:Systems."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 677ef74dac663a43835d65a4d4f4fd031a0078cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a><span data-ttu-id="a3653-103">Självstudier: Azure Active Directory-integrering med FM:Systems</span><span class="sxs-lookup"><span data-stu-id="a3653-103">Tutorial: Azure Active Directory integration with FM:Systems</span></span>

<span data-ttu-id="a3653-104">I kursen får du lära dig hur toointegrate FM:Systems med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a3653-104">In this tutorial, you learn how toointegrate FM:Systems with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a3653-105">Integrera FM:Systems med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a3653-105">Integrating FM:Systems with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a3653-106">Du kan styra i Azure AD som har åtkomst till tooFM:Systems</span><span class="sxs-lookup"><span data-stu-id="a3653-106">You can control in Azure AD who has access tooFM:Systems</span></span>
- <span data-ttu-id="a3653-107">Du kan aktivera din användare tooautomatically get inloggade tooFM:Systems (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a3653-107">You can enable your users tooautomatically get signed-on tooFM:Systems (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a3653-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a3653-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a3653-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a3653-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3653-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a3653-110">Prerequisites</span></span>

<span data-ttu-id="a3653-111">tooconfigure Azure AD-integrering med FM:Systems, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a3653-111">tooconfigure Azure AD integration with FM:Systems, you need hello following items:</span></span>

- <span data-ttu-id="a3653-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a3653-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a3653-113">En FM:Systems enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a3653-113">An FM:Systems single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a3653-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a3653-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a3653-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a3653-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a3653-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a3653-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a3653-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a3653-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a3653-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a3653-118">Scenario description</span></span>
<span data-ttu-id="a3653-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a3653-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a3653-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a3653-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a3653-121">Att lägga till FM:Systems från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a3653-121">Adding FM:Systems from hello gallery</span></span>
2. <span data-ttu-id="a3653-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3653-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-fmsystems-from-hello-gallery"></a><span data-ttu-id="a3653-123">Att lägga till FM:Systems från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a3653-123">Adding FM:Systems from hello gallery</span></span>
<span data-ttu-id="a3653-124">tooconfigure hello integrering av FM:Systems i Azure AD, behöver du tooadd FM:Systems hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a3653-124">tooconfigure hello integration of FM:Systems into Azure AD, you need tooadd FM:Systems from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a3653-125">**tooadd FM:Systems från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a3653-125">**tooadd FM:Systems from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3653-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a3653-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a3653-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a3653-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a3653-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a3653-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a3653-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3653-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a3653-133">Skriv i sökrutan hello **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="a3653-133">In hello search box, type **FM:Systems**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_search.png)

5. <span data-ttu-id="a3653-135">Markera hello resultat på panelen **FM:Systems**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a3653-135">In hello results panel, select **FM:Systems**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a3653-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3653-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a3653-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FM:Systems baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a3653-138">In this section, you configure and test Azure AD single sign-on with FM:Systems based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a3653-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i FM:Systems är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a3653-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in FM:Systems is tooa user in Azure AD.</span></span> <span data-ttu-id="a3653-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i FM:Systems toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a3653-140">In other words, a link relationship between an Azure AD user and hello related user in FM:Systems needs toobe established.</span></span>

<span data-ttu-id="a3653-141">I FM:Systems, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a3653-141">In FM:Systems, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a3653-142">tooconfigure och testa Azure AD enkel inloggning med FM:Systems, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a3653-142">tooconfigure and test Azure AD single sign-on with FM:Systems, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a3653-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a3653-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a3653-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a3653-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a3653-145">**[Skapa en testanvändare FM:Systems](#creating-an-fmsystems-test-user)**  -toohave en motsvarighet för Britta Simon i FM:Systems som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a3653-145">**[Creating an FM:Systems test user](#creating-an-fmsystems-test-user)** - toohave a counterpart of Britta Simon in FM:Systems that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a3653-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3653-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a3653-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a3653-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a3653-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3653-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a3653-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt FM:Systems program.</span><span class="sxs-lookup"><span data-stu-id="a3653-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your FM:Systems application.</span></span>

<span data-ttu-id="a3653-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med FM:Systems:**</span><span class="sxs-lookup"><span data-stu-id="a3653-150">**tooconfigure Azure AD single sign-on with FM:Systems, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3653-151">I hello Azure-portalen på hello **FM:Systems** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a3653-151">In hello Azure portal, on hello **FM:Systems** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a3653-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3653-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

3. <span data-ttu-id="a3653-155">På hello **FM:Systems domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a3653-155">On hello **FM:Systems Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_url.png)

    <span data-ttu-id="a3653-157">I hello **Reply URL** textruta Skriv din FM:Systems **Reply URL**, typen hello URL med hello följande mönster:`https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span><span class="sxs-lookup"><span data-stu-id="a3653-157">In hello **Reply URL** textbox, type your FM:Systems **Reply URL**, type hello URL using hello following pattern: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a3653-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a3653-158">This value is not real.</span></span> <span data-ttu-id="a3653-159">Uppdatera det här värdet med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="a3653-159">Update this value with hello actual Reply URL.</span></span> <span data-ttu-id="a3653-160">Kontakta [FM:Systems supportteamet](https://fmsystems.com/ask-us/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="a3653-160">Contact [FM:Systems support team](https://fmsystems.com/ask-us/) tooget this value.</span></span>
 
4. <span data-ttu-id="a3653-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="a3653-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

5. <span data-ttu-id="a3653-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3653-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a3653-165">tooconfigure enkel inloggning på **FM:Systems** sida, behöver du toosend hello hämtas **XML-Metadata för** för[FM:Systems supportteamet](https://fmsystems.com/ask-us/).</span><span class="sxs-lookup"><span data-stu-id="a3653-165">tooconfigure single sign-on on **FM:Systems** side, you need toosend hello downloaded **Metadata XML** too[FM:Systems support team](https://fmsystems.com/ask-us/).</span></span> <span data-ttu-id="a3653-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="a3653-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span> <span data-ttu-id="a3653-167">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a3653-167">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="a3653-168">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a3653-168">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a3653-169">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a3653-169">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a3653-170">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a3653-170">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a3653-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a3653-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="a3653-172">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a3653-172">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a3653-174">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a3653-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3653-175">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a3653-175">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a3653-177">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a3653-177">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a3653-179">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3653-179">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a3653-181">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a3653-181">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a3653-183">a.</span><span class="sxs-lookup"><span data-stu-id="a3653-183">a.</span></span> <span data-ttu-id="a3653-184">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a3653-184">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a3653-185">b.</span><span class="sxs-lookup"><span data-stu-id="a3653-185">b.</span></span> <span data-ttu-id="a3653-186">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a3653-186">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a3653-187">c.</span><span class="sxs-lookup"><span data-stu-id="a3653-187">c.</span></span> <span data-ttu-id="a3653-188">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a3653-188">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a3653-189">d.</span><span class="sxs-lookup"><span data-stu-id="a3653-189">d.</span></span> <span data-ttu-id="a3653-190">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a3653-190">Click **Create**.</span></span>
 
### <a name="creating-an-fmsystems-test-user"></a><span data-ttu-id="a3653-191">Skapa en testanvändare FM:Systems</span><span class="sxs-lookup"><span data-stu-id="a3653-191">Creating an FM:Systems test user</span></span>

1. <span data-ttu-id="a3653-192">Logga in på webbplatsen FM:Systems företag som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a3653-192">In a web browser window, log into your FM:Systems company site as an administrator.</span></span>

2. <span data-ttu-id="a3653-193">Gå för**systemadministration \> Hantera säkerhet \> användare \> användarlistan**.</span><span class="sxs-lookup"><span data-stu-id="a3653-193">Go too**System Administration \> Manage Security \> Users \> User list**.</span></span>
   
    <span data-ttu-id="a3653-194">![Systemadministration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "systemadministration")</span><span class="sxs-lookup"><span data-stu-id="a3653-194">![System Administration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "System Administration")</span></span>

3. <span data-ttu-id="a3653-195">Klicka på **skapa nya användare**.</span><span class="sxs-lookup"><span data-stu-id="a3653-195">Click **Create new user**.</span></span>
   
    <span data-ttu-id="a3653-196">![Skapa ny användare](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Skapa ny användare")</span><span class="sxs-lookup"><span data-stu-id="a3653-196">![Create New User](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Create New User")</span></span>

4. <span data-ttu-id="a3653-197">I hello **skapa användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a3653-197">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="a3653-198">![Skapa användare](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="a3653-198">![Create User](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "Create User")</span></span>
   
    <span data-ttu-id="a3653-199">a.</span><span class="sxs-lookup"><span data-stu-id="a3653-199">a.</span></span> <span data-ttu-id="a3653-200">Typen hello **användarnamn**, hello **lösenord**, **Bekräfta lösenord**, **e-post** och hello **anställnings-ID**av en giltig Azure Active Directory-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="a3653-200">Type hello **UserName**, hello **Password**, **Confirm Password**, **E-mail** and hello **Employee ID** of a valid Azure Active Directory account you want tooprovision into hello related textboxes.</span></span>
   
    <span data-ttu-id="a3653-201">b.</span><span class="sxs-lookup"><span data-stu-id="a3653-201">b.</span></span> <span data-ttu-id="a3653-202">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a3653-202">Click **Next**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a3653-203">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a3653-203">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a3653-204">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooFM:Systems.</span><span class="sxs-lookup"><span data-stu-id="a3653-204">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFM:Systems.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a3653-206">**tooassign Britta Simon tooFM:Systems utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a3653-206">**tooassign Britta Simon tooFM:Systems, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3653-207">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a3653-207">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a3653-209">Välj i listan med program hello **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="a3653-209">In hello applications list, select **FM:Systems**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_app.png) 

3. <span data-ttu-id="a3653-211">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a3653-211">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a3653-213">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3653-213">Click **Add** button.</span></span> <span data-ttu-id="a3653-214">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3653-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a3653-216">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a3653-216">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a3653-217">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3653-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a3653-218">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3653-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a3653-219">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3653-219">Testing single sign-on</span></span>

<span data-ttu-id="a3653-220">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a3653-220">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a3653-221">Du bör få automatiskt inloggade tooyour FM:Systems programmet när du klickar på hello FM:Systems panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a3653-221">When you click hello FM:Systems tile in hello Access Panel, you should get automatically signed-on tooyour FM:Systems application.</span></span>
<span data-ttu-id="a3653-222">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a3653-222">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3653-223">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a3653-223">Additional resources</span></span>

* [<span data-ttu-id="a3653-224">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a3653-224">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a3653-225">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a3653-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_203.png

