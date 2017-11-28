---
title: "Självstudier: Azure Active Directory-integrering med växthusgaser | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och växthusgaser."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 1a7cdd00c4f2b15a1afc89522d79af22f4c5d866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a><span data-ttu-id="c2379-103">Självstudier: Azure Active Directory-integrering med växthusgaser</span><span class="sxs-lookup"><span data-stu-id="c2379-103">Tutorial: Azure Active Directory integration with Greenhouse</span></span>

<span data-ttu-id="c2379-104">I kursen får du lära dig hur toointegrate växthusgaser med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c2379-104">In this tutorial, you learn how toointegrate Greenhouse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c2379-105">Integrera växthusgaser med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c2379-105">Integrating Greenhouse with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c2379-106">Du kan styra i Azure AD som har åtkomst till tooGreenhouse.</span><span class="sxs-lookup"><span data-stu-id="c2379-106">You can control in Azure AD who has access tooGreenhouse.</span></span>
- <span data-ttu-id="c2379-107">Du kan låta dina användare tooautomatically get inloggade tooGreenhouse (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="c2379-107">You can enable your users tooautomatically get signed-on tooGreenhouse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c2379-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c2379-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="c2379-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c2379-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2379-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c2379-110">Prerequisites</span></span>

<span data-ttu-id="c2379-111">tooconfigure Azure AD-integrering med växthusgaser måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c2379-111">tooconfigure Azure AD integration with Greenhouse, you need hello following items:</span></span>

- <span data-ttu-id="c2379-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c2379-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c2379-113">En växthusgaser enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="c2379-113">A Greenhouse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c2379-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c2379-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c2379-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c2379-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c2379-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c2379-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c2379-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c2379-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c2379-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c2379-118">Scenario description</span></span>
<span data-ttu-id="c2379-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c2379-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c2379-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2379-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c2379-121">Att lägga till växthusgaser från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c2379-121">Adding Greenhouse from hello gallery</span></span>
2. <span data-ttu-id="c2379-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2379-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-greenhouse-from-hello-gallery"></a><span data-ttu-id="c2379-123">Att lägga till växthusgaser från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c2379-123">Adding Greenhouse from hello gallery</span></span>
<span data-ttu-id="c2379-124">tooconfigure hello-integrering världens Azure AD, behöver du tooadd växthusgaser hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c2379-124">tooconfigure hello integration of Greenhouse into Azure AD, you need tooadd Greenhouse from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c2379-125">**tooadd växthusgaser från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2379-125">**tooadd Greenhouse from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2379-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c2379-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="c2379-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c2379-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c2379-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2379-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="c2379-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2379-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="c2379-133">Skriv i sökrutan hello **växthusgaser**väljer **växthusgaser** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c2379-133">In hello search box, type **Greenhouse**, select **Greenhouse** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Växthusgaser i hello resultatlistan](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c2379-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2379-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c2379-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med växthusgaser baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c2379-136">In this section, you configure and test Azure AD single sign-on with Greenhouse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c2379-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i växthusgaser är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c2379-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Greenhouse is tooa user in Azure AD.</span></span> <span data-ttu-id="c2379-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i växthusgaser toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c2379-138">In other words, a link relationship between an Azure AD user and hello related user in Greenhouse needs toobe established.</span></span>

<span data-ttu-id="c2379-139">I växthus, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c2379-139">In Greenhouse, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c2379-140">tooconfigure och testa Azure AD enkel inloggning med växthusgaser, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c2379-140">tooconfigure and test Azure AD single sign-on with Greenhouse, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c2379-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c2379-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c2379-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2379-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c2379-143">**[Skapa en testanvändare växthusgaser](#create-a-greenhouse-test-user)**  -toohave en motsvarighet för Britta Simon i växthusgaser som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c2379-143">**[Create a Greenhouse test user](#create-a-greenhouse-test-user)** - toohave a counterpart of Britta Simon in Greenhouse that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c2379-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2379-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c2379-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c2379-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c2379-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2379-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c2379-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet växthusgaser.</span><span class="sxs-lookup"><span data-stu-id="c2379-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Greenhouse application.</span></span>

<span data-ttu-id="c2379-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med växthusgaser:**</span><span class="sxs-lookup"><span data-stu-id="c2379-148">**tooconfigure Azure AD single sign-on with Greenhouse, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2379-149">I hello Azure-portalen på hello **växthusgaser** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c2379-149">In hello Azure portal, on hello **Greenhouse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="c2379-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c2379-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

3. <span data-ttu-id="c2379-153">På hello **växthusgaser domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2379-153">On hello **Greenhouse Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och växthusgaser domän med enkel inloggning information](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_url.png)

    <span data-ttu-id="c2379-155">a.</span><span class="sxs-lookup"><span data-stu-id="c2379-155">a.</span></span> <span data-ttu-id="c2379-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="c2379-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.greenhouse.io`</span></span>

    <span data-ttu-id="c2379-157">b.</span><span class="sxs-lookup"><span data-stu-id="c2379-157">b.</span></span> <span data-ttu-id="c2379-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="c2379-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.greenhouse.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c2379-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c2379-159">These values are not real.</span></span> <span data-ttu-id="c2379-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="c2379-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="c2379-161">Kontakta [växthusgaser klienten supportteamet](https://www.greenhouse.io/contact) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c2379-161">Contact [Greenhouse Client support team](https://www.greenhouse.io/contact) tooget these values.</span></span> 
 


4. <span data-ttu-id="c2379-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="c2379-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

5. <span data-ttu-id="c2379-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2379-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-greenhouse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c2379-166">tooconfigure enkel inloggning på **växthusgaser** sida, behöver du toosend hello hämtas **XML-Metadata för** för[växthusgaser supportteamet](http://www.greenhouse.io/contact).</span><span class="sxs-lookup"><span data-stu-id="c2379-166">tooconfigure single sign-on on **Greenhouse** side, you need toosend hello downloaded **Metadata XML** too[Greenhouse support team](http://www.greenhouse.io/contact).</span></span>

> [!TIP]
> <span data-ttu-id="c2379-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c2379-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c2379-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c2379-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c2379-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c2379-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c2379-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2379-170">Create an Azure AD test user</span></span>

<span data-ttu-id="c2379-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2379-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="c2379-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2379-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2379-174">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2379-174">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c2379-176">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c2379-176">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c2379-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2379-178">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c2379-180">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2379-180">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c2379-182">a.</span><span class="sxs-lookup"><span data-stu-id="c2379-182">a.</span></span> <span data-ttu-id="c2379-183">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c2379-183">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c2379-184">b.</span><span class="sxs-lookup"><span data-stu-id="c2379-184">b.</span></span> <span data-ttu-id="c2379-185">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c2379-185">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="c2379-186">c.</span><span class="sxs-lookup"><span data-stu-id="c2379-186">c.</span></span> <span data-ttu-id="c2379-187">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="c2379-187">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="c2379-188">d.</span><span class="sxs-lookup"><span data-stu-id="c2379-188">d.</span></span> <span data-ttu-id="c2379-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c2379-189">Click **Create**.</span></span>
 
### <a name="create-a-greenhouse-test-user"></a><span data-ttu-id="c2379-190">Skapa en testanvändare växthusgaser</span><span class="sxs-lookup"><span data-stu-id="c2379-190">Create a Greenhouse test user</span></span>

<span data-ttu-id="c2379-191">I ordning tooenable Azure AD-användare toolog till växthusgaser, måste de etableras i växthusgaser.</span><span class="sxs-lookup"><span data-stu-id="c2379-191">In order tooenable Azure AD users toolog into Greenhouse, they must be provisioned into Greenhouse.</span></span> <span data-ttu-id="c2379-192">I fallet hello världens är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="c2379-192">In hello case of Greenhouse, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="c2379-193">Du kan använda något annat växthusgaser användarens konto skapas verktyg eller API: er som tillhandahålls av växthusgaser tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="c2379-193">You can use any other Greenhouse user account creation tools or APIs provided by Greenhouse tooprovision AAD user accounts.</span></span> 

<span data-ttu-id="c2379-194">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2379-194">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2379-195">Logga in tooyour **växthusgaser** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="c2379-195">Log in tooyour **Greenhouse** company site as an administrator.</span></span>

2. <span data-ttu-id="c2379-196">Hello-menyn överst hello **konfigurera**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="c2379-196">In hello menu on hello top, click **Configure**, and then click **Users**.</span></span>
   
   <span data-ttu-id="c2379-197">![Användare](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "användare")</span><span class="sxs-lookup"><span data-stu-id="c2379-197">![Users](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "Users")</span></span>

3. <span data-ttu-id="c2379-198">Klicka på **nya användare**.</span><span class="sxs-lookup"><span data-stu-id="c2379-198">Click **New Users**.</span></span>
   
   <span data-ttu-id="c2379-199">![Ny användare](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="c2379-199">![New User](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "New User")</span></span>

4. <span data-ttu-id="c2379-200">I hello **Lägg till nya användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c2379-200">In hello **Add New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="c2379-201">![Lägga till nya användare](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "lägga till nya användare")</span><span class="sxs-lookup"><span data-stu-id="c2379-201">![Add New User](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "Add New User")</span></span>

   <span data-ttu-id="c2379-202">a.</span><span class="sxs-lookup"><span data-stu-id="c2379-202">a.</span></span> <span data-ttu-id="c2379-203">I hello **ange användarens e-postmeddelanden** textruta typen hello e-postadress för ett giltigt Azure Active Directory-konto som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="c2379-203">In hello **Enter user emails** textbox, type hello email address of a valid Azure Active Directory account you want tooprovision.</span></span>

   <span data-ttu-id="c2379-204">b.</span><span class="sxs-lookup"><span data-stu-id="c2379-204">b.</span></span> <span data-ttu-id="c2379-205">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c2379-205">Click **Save**.</span></span>    
   
      >[!NOTE]
      ><span data-ttu-id="c2379-206">hello Azure Active Directory-konto innehavare får ett e-postmeddelande inklusive en länk tooconfirm hello innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="c2379-206">hello Azure Active Directory account holders will receive an email including a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="c2379-207">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c2379-207">Assign hello Azure AD test user</span></span>

<span data-ttu-id="c2379-208">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooGreenhouse.</span><span class="sxs-lookup"><span data-stu-id="c2379-208">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooGreenhouse.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="c2379-210">**tooassign Britta Simon tooGreenhouse utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c2379-210">**tooassign Britta Simon tooGreenhouse, perform hello following steps:**</span></span>

1. <span data-ttu-id="c2379-211">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c2379-211">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c2379-213">Välj i listan med program hello **växthusgaser**.</span><span class="sxs-lookup"><span data-stu-id="c2379-213">In hello applications list, select **Greenhouse**.</span></span>

    ![hello växthusgaser länken i listan med program hello](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_app.png)  

3. <span data-ttu-id="c2379-215">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c2379-215">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="c2379-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c2379-217">Click **Add** button.</span></span> <span data-ttu-id="c2379-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2379-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="c2379-220">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c2379-220">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c2379-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2379-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c2379-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c2379-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c2379-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c2379-223">Test single sign-on</span></span>

<span data-ttu-id="c2379-224">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2379-224">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c2379-225">Du bör få automatiskt inloggade tooyour växthusgaser programmet när du klickar på hello växthusgaser panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c2379-225">When you click hello Greenhouse tile in hello Access Panel, you should get automatically signed-on tooyour Greenhouse application.</span></span>
<span data-ttu-id="c2379-226">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c2379-226">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2379-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c2379-227">Additional resources</span></span>

* [<span data-ttu-id="c2379-228">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c2379-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c2379-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c2379-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_203.png

