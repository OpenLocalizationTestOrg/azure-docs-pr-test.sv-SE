---
title: "Självstudier: Azure Active Directory-integrering med Pingboard | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 0a916b1f9ef32d8124aa11284d2115bb4fc0bbc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a><span data-ttu-id="c976f-103">Självstudier: Azure Active Directory-integrering med Pingboard</span><span class="sxs-lookup"><span data-stu-id="c976f-103">Tutorial: Azure Active Directory integration with Pingboard</span></span>

<span data-ttu-id="c976f-104">I kursen får du lära dig hur toointegrate Pingboard med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c976f-104">In this tutorial, you learn how toointegrate Pingboard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c976f-105">Integrera Pingboard med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c976f-105">Integrating Pingboard with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c976f-106">Du kan styra i Azure AD som har åtkomst till tooPingboard</span><span class="sxs-lookup"><span data-stu-id="c976f-106">You can control in Azure AD who has access tooPingboard</span></span>
- <span data-ttu-id="c976f-107">Du kan aktivera din användare tooautomatically get inloggade tooPingboard (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c976f-107">You can enable your users tooautomatically get signed-on tooPingboard (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c976f-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="c976f-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="c976f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c976f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c976f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c976f-110">Prerequisites</span></span>

<span data-ttu-id="c976f-111">tooconfigure Azure AD-integrering med Pingboard, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c976f-111">tooconfigure Azure AD integration with Pingboard, you need hello following items:</span></span>

- <span data-ttu-id="c976f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c976f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c976f-113">En Pingboard enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c976f-113">A Pingboard single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c976f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c976f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c976f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c976f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c976f-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c976f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="c976f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c976f-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c976f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c976f-118">Scenario description</span></span>
<span data-ttu-id="c976f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c976f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c976f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c976f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c976f-121">Att lägga till Pingboard från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c976f-121">Adding Pingboard from hello gallery</span></span>
2. <span data-ttu-id="c976f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c976f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pingboard-from-hello-gallery"></a><span data-ttu-id="c976f-123">Att lägga till Pingboard från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c976f-123">Adding Pingboard from hello gallery</span></span>
<span data-ttu-id="c976f-124">tooconfigure hello integrering av Pingboard i Azure AD, behöver du tooadd Pingboard hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c976f-124">tooconfigure hello integration of Pingboard into Azure AD, you need tooadd Pingboard from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c976f-125">**tooadd Pingboard från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c976f-125">**tooadd Pingboard from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c976f-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c976f-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c976f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c976f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c976f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c976f-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c976f-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c976f-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c976f-133">Skriv i sökrutan hello **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="c976f-133">In hello search box, type **Pingboard**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. <span data-ttu-id="c976f-135">Markera hello resultat på panelen **Pingboard**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c976f-135">In hello results panel, select **Pingboard**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c976f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c976f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c976f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pingboard baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c976f-138">In this section, you configure and test Azure AD single sign-on with Pingboard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c976f-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Pingboard är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c976f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Pingboard is tooa user in Azure AD.</span></span> <span data-ttu-id="c976f-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Pingboard toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c976f-140">In other words, a link relationship between an Azure AD user and hello related user in Pingboard needs toobe established.</span></span>

<span data-ttu-id="c976f-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Pingboard.</span><span class="sxs-lookup"><span data-stu-id="c976f-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Pingboard.</span></span>

<span data-ttu-id="c976f-142">tooconfigure och testa Azure AD enkel inloggning med Pingboard, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c976f-142">tooconfigure and test Azure AD single sign-on with Pingboard, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c976f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c976f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c976f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c976f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c976f-145">**[Skapa en testanvändare Pingboard](#creating-a-pingboard-test-user)**  -toohave en motsvarighet för Britta Simon i Pingboard som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="c976f-145">**[Creating a Pingboard test user](#creating-a-pingboard-test-user)** - toohave a counterpart of Britta Simon in Pingboard that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="c976f-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c976f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c976f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c976f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c976f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c976f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c976f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt Pingboard program.</span><span class="sxs-lookup"><span data-stu-id="c976f-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Pingboard application.</span></span>

<span data-ttu-id="c976f-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Pingboard:**</span><span class="sxs-lookup"><span data-stu-id="c976f-150">**tooconfigure Azure AD single sign-on with Pingboard, perform hello following steps:**</span></span>

1. <span data-ttu-id="c976f-151">I hello Azure Management portal på hello **Pingboard** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c976f-151">In hello Azure Management portal, on hello **Pingboard** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c976f-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c976f-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. <span data-ttu-id="c976f-155">På hello **Pingboard domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="c976f-155">On hello **Pingboard Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    <span data-ttu-id="c976f-157">a.</span><span class="sxs-lookup"><span data-stu-id="c976f-157">a.</span></span> <span data-ttu-id="c976f-158">I hello **identifierare** textruta hello TYPVÄRDE som:`http://<entity-id>.pingboard.com/sp`</span><span class="sxs-lookup"><span data-stu-id="c976f-158">In hello **Identifier** textbox, type hello value as: `http://<entity-id>.pingboard.com/sp`</span></span>

    <span data-ttu-id="c976f-159">b.</span><span class="sxs-lookup"><span data-stu-id="c976f-159">b.</span></span> <span data-ttu-id="c976f-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<entity-id>.pingboard.com/auth/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="c976f-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<entity-id>.pingboard.com/auth/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c976f-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="c976f-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="c976f-162">Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="c976f-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="c976f-163">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="c976f-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="c976f-164">Kontakta [Pingboard klienten supportteamet](https://support.pingboard.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c976f-164">Contact [Pingboard Client support team](https://support.pingboard.com/) tooget these values.</span></span> 

4. <span data-ttu-id="c976f-165">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="c976f-165">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    <span data-ttu-id="c976f-167">a.</span><span class="sxs-lookup"><span data-stu-id="c976f-167">a.</span></span> <span data-ttu-id="c976f-168">I hello **inloggnings-URL** textruta hello TYPVÄRDE som:`http://<sub-domain>.pingboard.com/sign_in`</span><span class="sxs-lookup"><span data-stu-id="c976f-168">In hello **Sign-on URL** textbox, type hello value as: `http://<sub-domain>.pingboard.com/sign_in`</span></span>
     
5. <span data-ttu-id="c976f-169">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c976f-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. <span data-ttu-id="c976f-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c976f-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="c976f-173">tooconfigure SSO på Pingboard sida, öppna ett nytt fönster i webbläsaren och logga in tooyour Pingboard konto.</span><span class="sxs-lookup"><span data-stu-id="c976f-173">tooconfigure SSO on Pingboard side, open a new browser window and log in tooyour Pingboard Account.</span></span> <span data-ttu-id="c976f-174">Du måste vara en Pingboard admin tooset in enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="c976f-174">You must be a Pingboard admin tooset up single sign on.</span></span>

8. <span data-ttu-id="c976f-175">Välj hello översta menyn **appar > integreringar**</span><span class="sxs-lookup"><span data-stu-id="c976f-175">From hello top menu select **Apps > Integrations**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  <span data-ttu-id="c976f-177">På hello **integreringar** kan hitta hello **”Azure Active Directory”** panelen och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="c976f-177">On hello **Integrations** page, find hello **"Azure Active Directory"** tile, and click it.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. <span data-ttu-id="c976f-179">I hello spärrad som följer Klicka **”konfigurera”**</span><span class="sxs-lookup"><span data-stu-id="c976f-179">In hello modal that follows click **"Configure"**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. <span data-ttu-id="c976f-181">På hello följande sida, ser du att ”Azure SSO-integrering är aktiverad”..</span><span class="sxs-lookup"><span data-stu-id="c976f-181">On hello following page, you will notice that "Azure SSO Integration is enabled.".</span></span> <span data-ttu-id="c976f-182">Öppna hello hämtas Metadata XML-fil i en anteckningar och klistra in hello innehåll i **IDP Metadata**.</span><span class="sxs-lookup"><span data-stu-id="c976f-182">Open hello downloaded Metadata XML file in a notepad and paste hello content in **IDP Metadata**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. <span data-ttu-id="c976f-184">hello-filen kommer att valideras och om allt stämmer aktiveras nu för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c976f-184">hello file will be validated, and if everything is correct, single sign on will now be enabled</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c976f-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c976f-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="c976f-186">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c976f-186">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c976f-188">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c976f-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c976f-189">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c976f-189">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c976f-191">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="c976f-191">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c976f-193">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c976f-193">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c976f-195">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c976f-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c976f-197">a.</span><span class="sxs-lookup"><span data-stu-id="c976f-197">a.</span></span> <span data-ttu-id="c976f-198">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c976f-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c976f-199">b.</span><span class="sxs-lookup"><span data-stu-id="c976f-199">b.</span></span> <span data-ttu-id="c976f-200">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c976f-200">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c976f-201">c.</span><span class="sxs-lookup"><span data-stu-id="c976f-201">c.</span></span> <span data-ttu-id="c976f-202">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c976f-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c976f-203">d.</span><span class="sxs-lookup"><span data-stu-id="c976f-203">d.</span></span> <span data-ttu-id="c976f-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c976f-204">Click **Create**.</span></span>
 
### <a name="creating-a-pingboard-test-user"></a><span data-ttu-id="c976f-205">Skapa en testanvändare Pingboard</span><span class="sxs-lookup"><span data-stu-id="c976f-205">Creating a Pingboard test user</span></span>

<span data-ttu-id="c976f-206">I ordning tooenable Azure AD-användare toolog i Pingboard, måste de etableras i Pingboard.</span><span class="sxs-lookup"><span data-stu-id="c976f-206">In order tooenable Azure AD users toolog into Pingboard, they must be provisioned into Pingboard.</span></span>  
<span data-ttu-id="c976f-207">Hello gäller Pingboard är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c976f-207">In hello case of Pingboard, provisioning is a manual task.</span></span>

<span data-ttu-id="c976f-208">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c976f-208">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="c976f-209">Logga in tooyour Pingboard företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c976f-209">Log in tooyour Pingboard company site as an administrator.</span></span>

2. <span data-ttu-id="c976f-210">Klicka på **”Lägg till medarbetare”** knappen på **Directory** sidan.</span><span class="sxs-lookup"><span data-stu-id="c976f-210">Click **“Add Employee”** button on **Directory** page.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. <span data-ttu-id="c976f-212">På hello **”Lägg till medarbetare”** dialogrutan utför hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="c976f-212">On hello **“Add Employee”** dialog page, perform hello following steps.</span></span>

    ![Bjud in personer](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    <span data-ttu-id="c976f-214">a.</span><span class="sxs-lookup"><span data-stu-id="c976f-214">a.</span></span> <span data-ttu-id="c976f-215">I hello **fullständiga namn** textruta hello fullständiga typnamnet för Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c976f-215">In hello **Full Name** textbox, type hello full name of Britta Simon.</span></span>

    <span data-ttu-id="c976f-216">b.</span><span class="sxs-lookup"><span data-stu-id="c976f-216">b.</span></span> <span data-ttu-id="c976f-217">I hello **e-post** textruta typen hello e-postadress för Britta Simon konto.</span><span class="sxs-lookup"><span data-stu-id="c976f-217">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="c976f-218">c.</span><span class="sxs-lookup"><span data-stu-id="c976f-218">c.</span></span> <span data-ttu-id="c976f-219">I hello **befattning** textruta typen hello befattningen Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c976f-219">In hello **Job Title** textbox, type hello job title of Britta Simon.</span></span>

    <span data-ttu-id="c976f-220">d.</span><span class="sxs-lookup"><span data-stu-id="c976f-220">d.</span></span> <span data-ttu-id="c976f-221">I hello **plats** listrutan, Välj hello platsen för Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c976f-221">In hello **Location** dropdown, select hello location  of Britta Simon.</span></span>
    
    <span data-ttu-id="c976f-222">e.</span><span class="sxs-lookup"><span data-stu-id="c976f-222">e.</span></span> <span data-ttu-id="c976f-223">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c976f-223">Click **Add**.</span></span>   

4. <span data-ttu-id="c976f-224">En bekräftelseskärm kommer nu att tooconfirm hello lägga till användaren.</span><span class="sxs-lookup"><span data-stu-id="c976f-224">A confirmation screen will come up tooconfirm hello addition of user.</span></span>
    
    ![Bekräfta](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > <span data-ttu-id="c976f-226">hello Azure Active Directory-konto innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="c976f-226">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c976f-227">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c976f-227">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c976f-228">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooPingboard sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c976f-228">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooPingboard.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c976f-230">**tooassign Britta Simon tooPingboard utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c976f-230">**tooassign Britta Simon tooPingboard, perform hello following steps:**</span></span>

1. <span data-ttu-id="c976f-231">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c976f-231">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c976f-233">Välj i listan med program hello **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="c976f-233">In hello applications list, select **Pingboard**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. <span data-ttu-id="c976f-235">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c976f-235">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c976f-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c976f-237">Click **Add** button.</span></span> <span data-ttu-id="c976f-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c976f-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c976f-240">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c976f-240">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c976f-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c976f-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c976f-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c976f-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c976f-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c976f-243">Testing single sign-on</span></span>

<span data-ttu-id="c976f-244">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c976f-244">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c976f-245">Du bör få automatiskt inloggade tooyour Pingboard programmet när du klickar på hello Pingboard panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c976f-245">When you click hello Pingboard tile in hello Access Panel, you should get automatically signed-on tooyour Pingboard application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c976f-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c976f-246">Additional resources</span></span>

* [<span data-ttu-id="c976f-247">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c976f-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c976f-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c976f-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
