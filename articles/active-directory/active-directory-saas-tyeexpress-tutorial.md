---
title: "Självstudier: Azure Active Directory-integrering med T & E Express | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och T & E Express."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 9a568ace8dbc75fadbf37554996b1b597a813d56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a><span data-ttu-id="d0232-103">Självstudier: Azure Active Directory-integrering med T & E Express</span><span class="sxs-lookup"><span data-stu-id="d0232-103">Tutorial: Azure Active Directory integration with T&E Express</span></span>

<span data-ttu-id="d0232-104">I kursen får du lära dig hur toointegrate T & E Express med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d0232-104">In this tutorial, you learn how toointegrate T&E Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d0232-105">Integrera T & E Express med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d0232-105">Integrating T&E Express with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d0232-106">Du kan styra i Azure AD som har åtkomst till Tool & E Express</span><span class="sxs-lookup"><span data-stu-id="d0232-106">You can control in Azure AD who has access tooT&E Express</span></span>
- <span data-ttu-id="d0232-107">Du kan aktivera din användare tooautomatically get inloggade Tool & E Express (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d0232-107">You can enable your users tooautomatically get signed-on tooT&E Express (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d0232-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="d0232-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="d0232-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d0232-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0232-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d0232-110">Prerequisites</span></span>

<span data-ttu-id="d0232-111">tooconfigure Azure AD-integrering med T & E Express, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d0232-111">tooconfigure Azure AD integration with T&E Express, you need hello following items:</span></span>

- <span data-ttu-id="d0232-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d0232-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d0232-113">En T & E Express enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d0232-113">A T&E Express single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d0232-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d0232-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d0232-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d0232-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d0232-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d0232-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="d0232-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0232-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d0232-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d0232-118">Scenario description</span></span>
<span data-ttu-id="d0232-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d0232-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d0232-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d0232-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d0232-121">Att lägga till d & E Express från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d0232-121">Adding T&E Express from hello gallery</span></span>
2. <span data-ttu-id="d0232-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0232-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-te-express-from-hello-gallery"></a><span data-ttu-id="d0232-123">Att lägga till d & E Express från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d0232-123">Adding T&E Express from hello gallery</span></span>
<span data-ttu-id="d0232-124">tooconfigure hello integrering av d & E Express i Azure AD, behöver du tooadd T & E Express hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d0232-124">tooconfigure hello integration of T&E Express into Azure AD, you need tooadd T&E Express from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d0232-125">**tooadd T & E Express från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d0232-125">**tooadd T&E Express from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0232-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d0232-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d0232-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d0232-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d0232-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d0232-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d0232-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0232-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d0232-133">Skriv i sökrutan hello **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="d0232-133">In hello search box, type **T&E Express**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. <span data-ttu-id="d0232-135">Markera hello resultat på panelen **T & E Express**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d0232-135">In hello results panel, select **T&E Express**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d0232-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0232-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d0232-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med T & E Express baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d0232-138">In this section, you configure and test Azure AD single sign-on with T&E Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d0232-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i T & E Express är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d0232-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in T&E Express is tooa user in Azure AD.</span></span> <span data-ttu-id="d0232-140">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i T & E Express behov toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d0232-140">In other words, a link relationship between an Azure AD user and hello related user in T&E Express needs toobe established.</span></span>

<span data-ttu-id="d0232-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i T & E Express.</span><span class="sxs-lookup"><span data-stu-id="d0232-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in T&E Express.</span></span>

<span data-ttu-id="d0232-142">tooconfigure och testa Azure AD enkel inloggning med T & E Express, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d0232-142">tooconfigure and test Azure AD single sign-on with T&E Express, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d0232-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d0232-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d0232-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d0232-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d0232-145">**[Skapa en testanvändare T & E Express](#creating-a-te-express-test-user)**  -toohave en motsvarighet för Britta Simon i T & E Express som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="d0232-145">**[Creating a T&E Express test user](#creating-a-te-express-test-user)** - toohave a counterpart of Britta Simon in T&E Express that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="d0232-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0232-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d0232-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d0232-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d0232-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0232-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d0232-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i T & E Express-program.</span><span class="sxs-lookup"><span data-stu-id="d0232-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your T&E Express application.</span></span>

<span data-ttu-id="d0232-150">**tooconfigure Azure AD enkel inloggning med T & E Express, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d0232-150">**tooconfigure Azure AD single sign-on with T&E Express, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0232-151">I hello Azure Management portal på hello **T & E Express** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d0232-151">In hello Azure Management portal, on hello **T&E Express** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d0232-153">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0232-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. <span data-ttu-id="d0232-155">På hello **& E Express domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d0232-155">On hello **T&E Express Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    <span data-ttu-id="d0232-157">a.</span><span class="sxs-lookup"><span data-stu-id="d0232-157">a.</span></span> <span data-ttu-id="d0232-158">I hello **identifierare** textruta hello TYPVÄRDE som:`https://<domain>.tyeexpress.com`</span><span class="sxs-lookup"><span data-stu-id="d0232-158">In hello **Identifier** textbox, type hello value as: `https://<domain>.tyeexpress.com`</span></span>

    <span data-ttu-id="d0232-159">b.</span><span class="sxs-lookup"><span data-stu-id="d0232-159">b.</span></span> <span data-ttu-id="d0232-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span><span class="sxs-lookup"><span data-stu-id="d0232-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d0232-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="d0232-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="d0232-162">Du har tooupdate dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="d0232-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="d0232-163">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="d0232-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="d0232-164">Kontakta [T & E Express supportteam](http://www.tyeexpress.com/contacto.aspx) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d0232-164">Contact [T&E Express support team](http://www.tyeexpress.com/contacto.aspx) tooget these values.</span></span>

5. <span data-ttu-id="d0232-165">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d0232-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. <span data-ttu-id="d0232-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d0232-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="d0232-169">tooconfigure enkel inloggning på **T & E snabb** sida, inloggning toohello T & E express program utan SAML enkel inloggning med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d0232-169">tooconfigure single sign-on on **T&E Express** side, login toohello T&E express application without SAML single sign on using admin credentials.</span></span>

9. <span data-ttu-id="d0232-170">Under hello **Admin** klickar du på **SAML domän** tooOpen hello SAML inställningssidan.</span><span class="sxs-lookup"><span data-stu-id="d0232-170">Under hello **Admin** Tab, Click on **SAML domain** tooOpen hello SAML settings page.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. <span data-ttu-id="d0232-172">Välj hello **Activar(Activate)** alternativet från **nr** för**SI(Yes)**.</span><span class="sxs-lookup"><span data-stu-id="d0232-172">Select hello **Activar(Activate)** option from **No** too**SI(Yes)**.</span></span> <span data-ttu-id="d0232-173">I hello **identitet providern Metadata** textruta klistra in hello metadata XML som du har donwloaded från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d0232-173">In hello **Identity Provider Metadata** textbox, paste hello metadata XML which you have donwloaded from Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. <span data-ttu-id="d0232-175">Klicka på hello **Guardar(Save)** knappen toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="d0232-175">Click on hello **Guardar(Save)** button toosave hello settings.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d0232-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d0232-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="d0232-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d0232-177">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d0232-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d0232-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0232-180">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d0232-180">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d0232-182">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="d0232-182">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d0232-184">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0232-184">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d0232-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d0232-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d0232-188">a.</span><span class="sxs-lookup"><span data-stu-id="d0232-188">a.</span></span> <span data-ttu-id="d0232-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d0232-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d0232-190">b.</span><span class="sxs-lookup"><span data-stu-id="d0232-190">b.</span></span> <span data-ttu-id="d0232-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d0232-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d0232-192">c.</span><span class="sxs-lookup"><span data-stu-id="d0232-192">c.</span></span> <span data-ttu-id="d0232-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d0232-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d0232-194">d.</span><span class="sxs-lookup"><span data-stu-id="d0232-194">d.</span></span> <span data-ttu-id="d0232-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d0232-195">Click **Create**.</span></span>
 
### <a name="creating-a-te-express-test-user"></a><span data-ttu-id="d0232-196">Skapa en testanvändare T & E Express</span><span class="sxs-lookup"><span data-stu-id="d0232-196">Creating a T&E Express test user</span></span>

<span data-ttu-id="d0232-197">I ordning tooenable Azure AD-användare toolog i T & E Express, måste de etableras i T & E Express.</span><span class="sxs-lookup"><span data-stu-id="d0232-197">In order tooenable Azure AD users toolog into T&E Express, they must be provisioned into T&E Express.</span></span>  
<span data-ttu-id="d0232-198">Vid T & E Express är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d0232-198">In case of T&E Express, provisioning is a manual task.</span></span>

<span data-ttu-id="d0232-199">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d0232-199">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0232-200">Logga in tooyour T & E Express företagets plats som en administratör.</span><span class="sxs-lookup"><span data-stu-id="d0232-200">Log in tooyour T&E Express company site as an administrator.</span></span>

2. <span data-ttu-id="d0232-201">Klicka på huvudsidan för användare tooopen hello användare under Admin-taggen.</span><span class="sxs-lookup"><span data-stu-id="d0232-201">Under Admin tag, click on Users tooopen hello Users master page.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. <span data-ttu-id="d0232-203">På startsidan för hello, klickar du på  **+**  tooadd hello användare.</span><span class="sxs-lookup"><span data-stu-id="d0232-203">On hello home page, click on **+** tooadd hello users.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. <span data-ttu-id="d0232-205">Ange alla obligatoriska hello-information som frågan i hello form och klicka på hello spara knappen toosave hello information.</span><span class="sxs-lookup"><span data-stu-id="d0232-205">Enter all hello mandatory details as asked in hello form and click hello save button toosave hello details.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d0232-208">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d0232-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d0232-209">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att ge sina access Tool & E Express.</span><span class="sxs-lookup"><span data-stu-id="d0232-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooT&E Express.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d0232-211">**tooassign Britta Simon Tool & E Express, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d0232-211">**tooassign Britta Simon tooT&E Express, perform hello following steps:**</span></span>

1. <span data-ttu-id="d0232-212">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d0232-212">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d0232-214">Välj i listan med program hello **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="d0232-214">In hello applications list, select **T&E Express**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. <span data-ttu-id="d0232-216">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d0232-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d0232-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d0232-218">Click **Add** button.</span></span> <span data-ttu-id="d0232-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0232-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d0232-221">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d0232-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d0232-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0232-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d0232-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0232-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d0232-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0232-224">Testing single sign-on</span></span>

<span data-ttu-id="d0232-225">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d0232-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d0232-226">Du bör få automatiskt inloggade tooyour T & E Express programmet när du klickar på hello T & E Express panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d0232-226">When you click hello T&E Express tile in hello Access Panel, you should get automatically signed-on tooyour T&E Express application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0232-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d0232-227">Additional resources</span></span>

* [<span data-ttu-id="d0232-228">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d0232-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d0232-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d0232-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png

