---
title: "Självstudier: Azure Active Directory-integrering med Oneteam | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Oneteam."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2e94916c-64ae-4e1a-a8b5-bc6ef7d28c29
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 7964aaaf9b9570d460f28d86de34b5e87693ba93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oneteam"></a><span data-ttu-id="164b0-103">Självstudier: Azure Active Directory-integrering med Oneteam</span><span class="sxs-lookup"><span data-stu-id="164b0-103">Tutorial: Azure Active Directory integration with Oneteam</span></span>

<span data-ttu-id="164b0-104">I kursen får du lära dig hur toointegrate Oneteam med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="164b0-104">In this tutorial, you learn how toointegrate Oneteam with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="164b0-105">Integrera Oneteam med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="164b0-105">Integrating Oneteam with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="164b0-106">Du kan styra i Azure AD som har åtkomst till tooOneteam</span><span class="sxs-lookup"><span data-stu-id="164b0-106">You can control in Azure AD who has access tooOneteam</span></span>
- <span data-ttu-id="164b0-107">Du kan aktivera din användare tooautomatically get inloggade tooOneteam (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="164b0-107">You can enable your users tooautomatically get signed-on tooOneteam (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="164b0-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="164b0-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="164b0-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="164b0-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="164b0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="164b0-110">Prerequisites</span></span>

<span data-ttu-id="164b0-111">tooconfigure Azure AD-integrering med Oneteam, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="164b0-111">tooconfigure Azure AD integration with Oneteam, you need hello following items:</span></span>

- <span data-ttu-id="164b0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="164b0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="164b0-113">En Oneteam enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="164b0-113">A Oneteam single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="164b0-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="164b0-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="164b0-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="164b0-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="164b0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="164b0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="164b0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="164b0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="164b0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="164b0-118">Scenario description</span></span>
<span data-ttu-id="164b0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="164b0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="164b0-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="164b0-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="164b0-121">Att lägga till Oneteam från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="164b0-121">Adding Oneteam from hello gallery</span></span>
2. <span data-ttu-id="164b0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="164b0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oneteam-from-hello-gallery"></a><span data-ttu-id="164b0-123">Att lägga till Oneteam från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="164b0-123">Adding Oneteam from hello gallery</span></span>
<span data-ttu-id="164b0-124">tooconfigure hello integrering av Oneteam i Azure AD, behöver du tooadd Oneteam hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="164b0-124">tooconfigure hello integration of Oneteam into Azure AD, you need tooadd Oneteam from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="164b0-125">**tooadd Oneteam från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="164b0-125">**tooadd Oneteam from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="164b0-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="164b0-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="164b0-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="164b0-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="164b0-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="164b0-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="164b0-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="164b0-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="164b0-133">Skriv i sökrutan hello **Oneteam**.</span><span class="sxs-lookup"><span data-stu-id="164b0-133">In hello search box, type **Oneteam**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_search.png)

5. <span data-ttu-id="164b0-135">Markera hello resultat på panelen **Oneteam**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="164b0-135">In hello results panel, select **Oneteam**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="164b0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="164b0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="164b0-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Oneteam baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="164b0-138">In this section, you configure and test Azure AD single sign-on with Oneteam based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="164b0-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Oneteam är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="164b0-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Oneteam is tooa user in Azure AD.</span></span> <span data-ttu-id="164b0-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Oneteam toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="164b0-140">In other words, a link relationship between an Azure AD user and hello related user in Oneteam needs toobe established.</span></span>

<span data-ttu-id="164b0-141">I Oneteam, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="164b0-141">In Oneteam, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="164b0-142">tooconfigure och testa Azure AD enkel inloggning med Oneteam, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="164b0-142">tooconfigure and test Azure AD single sign-on with Oneteam, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="164b0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="164b0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="164b0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="164b0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="164b0-145">**[Skapa en testanvändare Oneteam](#creating-a-oneteam-test-user)**  -toohave en motsvarighet för Britta Simon i Oneteam som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="164b0-145">**[Creating a Oneteam test user](#creating-a-oneteam-test-user)** - toohave a counterpart of Britta Simon in Oneteam that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="164b0-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="164b0-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="164b0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="164b0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="164b0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="164b0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="164b0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Oneteam program.</span><span class="sxs-lookup"><span data-stu-id="164b0-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Oneteam application.</span></span>

<span data-ttu-id="164b0-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Oneteam:**</span><span class="sxs-lookup"><span data-stu-id="164b0-150">**tooconfigure Azure AD single sign-on with Oneteam, perform hello following steps:**</span></span>

1. <span data-ttu-id="164b0-151">I hello Azure-portalen på hello **Oneteam** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="164b0-151">In hello Azure portal, on hello **Oneteam** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="164b0-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="164b0-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_samlbase.png)

3. <span data-ttu-id="164b0-155">På hello **Oneteam domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="164b0-155">On hello **Oneteam Domain and URLs** section, if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_url.png)

    <span data-ttu-id="164b0-157">a.</span><span class="sxs-lookup"><span data-stu-id="164b0-157">a.</span></span> <span data-ttu-id="164b0-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://api.one-team.io/teams/<team name>`</span><span class="sxs-lookup"><span data-stu-id="164b0-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://api.one-team.io/teams/<team name>`</span></span>

    <span data-ttu-id="164b0-159">b.</span><span class="sxs-lookup"><span data-stu-id="164b0-159">b.</span></span> <span data-ttu-id="164b0-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://api.one-team.io/teams/<team name>/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="164b0-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://api.one-team.io/teams/<team name>/auth/saml/callback`</span></span>

4. <span data-ttu-id="164b0-161">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="164b0-161">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_url1.png)

    <span data-ttu-id="164b0-163">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<team name>.one-team.io/`</span><span class="sxs-lookup"><span data-stu-id="164b0-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<team name>.one-team.io/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="164b0-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="164b0-164">These values are not real.</span></span> <span data-ttu-id="164b0-165">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="164b0-165">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="164b0-166">Kontakta [Oneteam klienten supportteamet](https://support.one-team.com/hc/requests/new) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="164b0-166">Contact [Oneteam Client support team](https://support.one-team.com/hc/requests/new) tooget these values.</span></span> 



5. <span data-ttu-id="164b0-167">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="164b0-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_certificate.png) 

6. <span data-ttu-id="164b0-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="164b0-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="164b0-171">tooget SSO konfigurerats för ditt program, kan du höja hello-supportärende med [Oneteam supportteamet](https://support.one-team.com/hc/requests/new) och ge dem hello hämtas **Metadata**.</span><span class="sxs-lookup"><span data-stu-id="164b0-171">tooget SSO configured for your application, you can raise hello support ticket with [Oneteam support team](https://support.one-team.com/hc/requests/new) and provide them hello downloaded **Metadata**.</span></span> 

> [!TIP]
> <span data-ttu-id="164b0-172">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="164b0-172">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="164b0-173">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="164b0-173">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="164b0-174">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="164b0-174">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="164b0-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="164b0-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="164b0-176">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="164b0-176">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="164b0-178">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="164b0-178">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="164b0-179">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="164b0-179">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="164b0-181">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="164b0-181">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="164b0-183">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="164b0-183">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="164b0-185">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="164b0-185">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="164b0-187">a.</span><span class="sxs-lookup"><span data-stu-id="164b0-187">a.</span></span> <span data-ttu-id="164b0-188">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="164b0-188">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="164b0-189">b.</span><span class="sxs-lookup"><span data-stu-id="164b0-189">b.</span></span> <span data-ttu-id="164b0-190">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="164b0-190">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="164b0-191">c.</span><span class="sxs-lookup"><span data-stu-id="164b0-191">c.</span></span> <span data-ttu-id="164b0-192">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="164b0-192">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="164b0-193">d.</span><span class="sxs-lookup"><span data-stu-id="164b0-193">d.</span></span> <span data-ttu-id="164b0-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="164b0-194">Click **Create**.</span></span>
 
### <a name="creating-a-oneteam-test-user"></a><span data-ttu-id="164b0-195">Skapa en testanvändare Oneteam</span><span class="sxs-lookup"><span data-stu-id="164b0-195">Creating a Oneteam test user</span></span>

<span data-ttu-id="164b0-196">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Oneteam.</span><span class="sxs-lookup"><span data-stu-id="164b0-196">hello objective of this section is toocreate a user called Britta Simon in Oneteam.</span></span> <span data-ttu-id="164b0-197">Oneteam stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="164b0-197">Oneteam supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="164b0-198">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="164b0-198">There is no action item for you in this section.</span></span> <span data-ttu-id="164b0-199">En ny användare skapas under ett försök tooaccess Oneteam, om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="164b0-199">A new user will be created during an attempt tooaccess Oneteam, if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="164b0-200">Om du behöver toocreate en användare manuellt kan du öka hello-supportärende med [Oneteam supportteamet](https://support.one-team.com/hc/requests/new).</span><span class="sxs-lookup"><span data-stu-id="164b0-200">If you need toocreate an user manually, you can raise hello support ticket with [Oneteam support team](https://support.one-team.com/hc/requests/new).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="164b0-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="164b0-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="164b0-202">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooOneteam.</span><span class="sxs-lookup"><span data-stu-id="164b0-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOneteam.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="164b0-204">**tooassign Britta Simon tooOneteam utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="164b0-204">**tooassign Britta Simon tooOneteam, perform hello following steps:**</span></span>

1. <span data-ttu-id="164b0-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="164b0-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="164b0-207">Välj i listan med program hello **Oneteam**.</span><span class="sxs-lookup"><span data-stu-id="164b0-207">In hello applications list, select **Oneteam**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_app.png) 

3. <span data-ttu-id="164b0-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="164b0-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="164b0-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="164b0-211">Click **Add** button.</span></span> <span data-ttu-id="164b0-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="164b0-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="164b0-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="164b0-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="164b0-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="164b0-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="164b0-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="164b0-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="164b0-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="164b0-217">Testing single sign-on</span></span>

<span data-ttu-id="164b0-218">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="164b0-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="164b0-219">Du bör få automatiskt inloggade tooyour Oneteam programmet när du klickar på hello Oneteam panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="164b0-219">When you click hello Oneteam tile in hello Access Panel, you should get automatically signed-on tooyour Oneteam application.</span></span>
<span data-ttu-id="164b0-220">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="164b0-220">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="164b0-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="164b0-221">Additional resources</span></span>

* [<span data-ttu-id="164b0-222">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="164b0-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="164b0-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="164b0-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_203.png

