---
title: "Självstudier: Azure Active Directory-integrering med Nomadic | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Nomadic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13d02b1c-d98a-40b1-824f-afa45a2deb6a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 8c1d3e350ce03c373cf475b2786ec299a7ce5f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadic"></a><span data-ttu-id="aa953-103">Självstudier: Azure Active Directory-integrering med Nomadic</span><span class="sxs-lookup"><span data-stu-id="aa953-103">Tutorial: Azure Active Directory integration with Nomadic</span></span>

<span data-ttu-id="aa953-104">I kursen får du lära dig hur toointegrate Nomadic med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="aa953-104">In this tutorial, you learn how toointegrate Nomadic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aa953-105">Integrera Nomadic med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="aa953-105">Integrating Nomadic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="aa953-106">Du kan styra i Azure AD som har åtkomst till tooNomadic.</span><span class="sxs-lookup"><span data-stu-id="aa953-106">You can control in Azure AD who has access tooNomadic.</span></span>
- <span data-ttu-id="aa953-107">Du kan låta dina användare tooautomatically get inloggade tooNomadic (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="aa953-107">You can enable your users tooautomatically get signed-on tooNomadic (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="aa953-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aa953-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="aa953-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aa953-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa953-110">Krav</span><span class="sxs-lookup"><span data-stu-id="aa953-110">Prerequisites</span></span>

<span data-ttu-id="aa953-111">tooconfigure Azure AD-integrering med Nomadic, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="aa953-111">tooconfigure Azure AD integration with Nomadic, you need hello following items:</span></span>

- <span data-ttu-id="aa953-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="aa953-112">An Azure AD subscription</span></span>
- <span data-ttu-id="aa953-113">En Nomadic enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="aa953-113">A Nomadic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aa953-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="aa953-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aa953-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="aa953-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aa953-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="aa953-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aa953-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aa953-117">If you don't have an Azure AD trial environment, you can [get a one-month trial here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aa953-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="aa953-118">Scenario description</span></span>
<span data-ttu-id="aa953-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="aa953-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aa953-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="aa953-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aa953-121">Lägg till Nomadic från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="aa953-121">Add Nomadic from hello gallery</span></span>
2. <span data-ttu-id="aa953-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa953-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-nomadic-from-hello-gallery"></a><span data-ttu-id="aa953-123">Lägg till Nomadic från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="aa953-123">Add Nomadic from hello gallery</span></span>
<span data-ttu-id="aa953-124">tooconfigure hello integrering av Nomadic i Azure AD, behöver du tooadd Nomadic hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="aa953-124">tooconfigure hello integration of Nomadic into Azure AD, you need tooadd Nomadic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="aa953-125">**tooadd Nomadic från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aa953-125">**tooadd Nomadic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa953-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="aa953-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="aa953-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="aa953-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="aa953-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="aa953-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="aa953-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa953-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="aa953-133">Skriv i sökrutan hello **Nomadic**väljer **Nomadic** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="aa953-133">In hello search box, type **Nomadic**, select **Nomadic** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Nomadic i hello resultatlistan](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="aa953-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa953-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="aa953-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Nomadic baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="aa953-136">In this section, you configure and test Azure AD single sign-on with Nomadic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="aa953-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Nomadic är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa953-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Nomadic is tooa user in Azure AD.</span></span> <span data-ttu-id="aa953-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Nomadic toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="aa953-138">In other words, a link relationship between an Azure AD user and hello related user in Nomadic needs toobe established.</span></span>

<span data-ttu-id="aa953-139">I Nomadic, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="aa953-139">In Nomadic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="aa953-140">tooconfigure och testa Azure AD enkel inloggning med Nomadic, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="aa953-140">tooconfigure and test Azure AD single sign-on with Nomadic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="aa953-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="aa953-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="aa953-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa953-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aa953-143">**[Skapa en Nomadic testanvändare](#create-a-nomadic-test-user)**  -toohave en motsvarighet för Britta Simon i Nomadic som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="aa953-143">**[Create a Nomadic test user](#create-a-nomadic-test-user)** - toohave a counterpart of Britta Simon in Nomadic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="aa953-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa953-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aa953-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="aa953-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="aa953-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa953-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="aa953-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Nomadic program.</span><span class="sxs-lookup"><span data-stu-id="aa953-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Nomadic application.</span></span>

<span data-ttu-id="aa953-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Nomadic:**</span><span class="sxs-lookup"><span data-stu-id="aa953-148">**tooconfigure Azure AD single sign-on with Nomadic, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa953-149">I hello Azure-portalen på hello **Nomadic** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="aa953-149">In hello Azure portal, on hello **Nomadic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="aa953-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aa953-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_samlbase.png)

3. <span data-ttu-id="aa953-153">På hello **Nomadic domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa953-153">On hello **Nomadic Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och nomadic domän med enkel inloggning information](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_url.png)

    <span data-ttu-id="aa953-155">a.</span><span class="sxs-lookup"><span data-stu-id="aa953-155">a.</span></span> <span data-ttu-id="aa953-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.nomadic.fm/signin`</span><span class="sxs-lookup"><span data-stu-id="aa953-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.nomadic.fm/signin`</span></span>

    <span data-ttu-id="aa953-157">b.</span><span class="sxs-lookup"><span data-stu-id="aa953-157">b.</span></span> <span data-ttu-id="aa953-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret: `https://<company name>.nomadic.fm/auth/saml2/sp`,`https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span><span class="sxs-lookup"><span data-stu-id="aa953-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.nomadic.fm/auth/saml2/sp`, `https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="aa953-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="aa953-159">These values are not real.</span></span> <span data-ttu-id="aa953-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="aa953-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="aa953-161">Kontakta [Nomadic klienten supportteamet](mailto:help@nomadic.fm) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="aa953-161">Contact [Nomadic Client support team](mailto:help@nomadic.fm) tooget these values.</span></span> 
 


4. <span data-ttu-id="aa953-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="aa953-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_certificate.png) 

5. <span data-ttu-id="aa953-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa953-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-nomadic-tutorial/tutorial_general_400.png)

6.  <span data-ttu-id="aa953-166">tooget SSO konfigurerats för ditt program bör du kontakta [Nomadic supportteamet](mailto:help@nomadic.fm) och ge dem hello hämtas **metadata**.</span><span class="sxs-lookup"><span data-stu-id="aa953-166">tooget SSO configured for your application, contact [Nomadic support team](mailto:help@nomadic.fm) and provide them with hello downloaded **metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="aa953-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="aa953-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="aa953-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="aa953-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="aa953-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="aa953-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="aa953-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa953-170">Create an Azure AD test user</span></span>

<span data-ttu-id="aa953-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa953-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="aa953-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aa953-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa953-174">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa953-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-nomadic-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="aa953-176">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="aa953-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-nomadic-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="aa953-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa953-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-nomadic-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="aa953-180">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aa953-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-nomadic-tutorial/create_aaduser_04.png)

    <span data-ttu-id="aa953-182">a.</span><span class="sxs-lookup"><span data-stu-id="aa953-182">a.</span></span> <span data-ttu-id="aa953-183">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aa953-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aa953-184">b.</span><span class="sxs-lookup"><span data-stu-id="aa953-184">b.</span></span> <span data-ttu-id="aa953-185">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aa953-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="aa953-186">c.</span><span class="sxs-lookup"><span data-stu-id="aa953-186">c.</span></span> <span data-ttu-id="aa953-187">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="aa953-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="aa953-188">d.</span><span class="sxs-lookup"><span data-stu-id="aa953-188">d.</span></span> <span data-ttu-id="aa953-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="aa953-189">Click **Create**.</span></span>
 
### <a name="create-a-nomadic-test-user"></a><span data-ttu-id="aa953-190">Skapa en Nomadic testanvändare</span><span class="sxs-lookup"><span data-stu-id="aa953-190">Create a Nomadic test user</span></span>

<span data-ttu-id="aa953-191">I det här avsnittet skapar du en användare som kallas Britta Simon i Nomadic.</span><span class="sxs-lookup"><span data-stu-id="aa953-191">In this section, you create a user called Britta Simon in Nomadic.</span></span> <span data-ttu-id="aa953-192">Se tillsammans med [Nomadic supportteamet](mailto:help@nomadic.fm) tooadd hello användare i hello Nomadic plattform.</span><span class="sxs-lookup"><span data-stu-id="aa953-192">Please work with [Nomadic support team](mailto:help@nomadic.fm) tooadd hello users in hello Nomadic platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="aa953-193">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="aa953-193">Assign hello Azure AD test user</span></span>

<span data-ttu-id="aa953-194">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNomadic.</span><span class="sxs-lookup"><span data-stu-id="aa953-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNomadic.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="aa953-196">**tooassign Britta Simon tooNomadic utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aa953-196">**tooassign Britta Simon tooNomadic, perform hello following steps:**</span></span>

1. <span data-ttu-id="aa953-197">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="aa953-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="aa953-199">Välj i listan med program hello **Nomadic**.</span><span class="sxs-lookup"><span data-stu-id="aa953-199">In hello applications list, select **Nomadic**.</span></span>

    ![Hej Nomadic länken i listan med program hello](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_app.png)  

3. <span data-ttu-id="aa953-201">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="aa953-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="aa953-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="aa953-203">Click **Add** button.</span></span> <span data-ttu-id="aa953-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa953-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="aa953-206">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="aa953-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="aa953-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa953-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aa953-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa953-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="aa953-209">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aa953-209">Test single sign-on</span></span>

<span data-ttu-id="aa953-210">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="aa953-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="aa953-211">När du klickar på hello Nomadic panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour Nomadic program.</span><span class="sxs-lookup"><span data-stu-id="aa953-211">When you click hello Nomadic tile in hello Access Panel, you should get automatically signed-on tooyour Nomadic application.</span></span>
<span data-ttu-id="aa953-212">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="aa953-212">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="aa953-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="aa953-213">Additional resources</span></span>

* [<span data-ttu-id="aa953-214">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aa953-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aa953-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="aa953-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_203.png

