---
title: "Självstudier: Azure Active Directory-integrering med Synergi | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Synergi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 73c970e1-f1ba-420b-b225-414fdf93b140
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5d4a9a596db2a60dda5bcac2c86b88b460a2e2cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-synergi"></a><span data-ttu-id="3d002-103">Självstudier: Azure Active Directory-integrering med Synergi</span><span class="sxs-lookup"><span data-stu-id="3d002-103">Tutorial: Azure Active Directory integration with Synergi</span></span>

<span data-ttu-id="3d002-104">I kursen får du lära dig hur toointegrate Synergi med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3d002-104">In this tutorial, you learn how toointegrate Synergi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3d002-105">Integrera Synergi med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3d002-105">Integrating Synergi with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3d002-106">Du kan styra i Azure AD som har åtkomst till tooSynergi.</span><span class="sxs-lookup"><span data-stu-id="3d002-106">You can control in Azure AD who has access tooSynergi.</span></span>
- <span data-ttu-id="3d002-107">Du kan låta dina användare tooautomatically get inloggade tooSynergi (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="3d002-107">You can enable your users tooautomatically get signed-on tooSynergi (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="3d002-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3d002-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="3d002-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3d002-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d002-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3d002-110">Prerequisites</span></span>

<span data-ttu-id="3d002-111">tooconfigure Azure AD-integrering med Synergi måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3d002-111">tooconfigure Azure AD integration with Synergi, you need hello following items:</span></span>

- <span data-ttu-id="3d002-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3d002-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3d002-113">En Synergi enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3d002-113">A Synergi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3d002-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3d002-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3d002-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3d002-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3d002-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3d002-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3d002-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d002-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3d002-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3d002-118">Scenario description</span></span>
<span data-ttu-id="3d002-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3d002-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3d002-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3d002-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3d002-121">Att lägga till Synergi från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3d002-121">Adding Synergi from hello gallery</span></span>
2. <span data-ttu-id="3d002-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d002-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-synergi-from-hello-gallery"></a><span data-ttu-id="3d002-123">Att lägga till Synergi från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3d002-123">Adding Synergi from hello gallery</span></span>
<span data-ttu-id="3d002-124">tooconfigure hello integrering av Synergi i Azure AD, behöver du tooadd Synergi hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3d002-124">tooconfigure hello integration of Synergi into Azure AD, you need tooadd Synergi from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3d002-125">**tooadd Synergi från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3d002-125">**tooadd Synergi from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d002-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3d002-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="3d002-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3d002-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3d002-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3d002-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="3d002-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d002-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="3d002-133">Skriv i sökrutan hello **Synergi**väljer **Synergi** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3d002-133">In hello search box, type **Synergi**, select **Synergi** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Synergi i hello resultatlistan](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3d002-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d002-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="3d002-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Synergi baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3d002-136">In this section, you configure and test Azure AD single sign-on with Synergi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3d002-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Synergi är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3d002-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Synergi is tooa user in Azure AD.</span></span> <span data-ttu-id="3d002-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Synergi toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="3d002-138">In other words, a link relationship between an Azure AD user and hello related user in Synergi needs toobe established.</span></span>

<span data-ttu-id="3d002-139">I Synergi, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3d002-139">In Synergi, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3d002-140">tooconfigure och testa Azure AD enkel inloggning med Synergi, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3d002-140">tooconfigure and test Azure AD single sign-on with Synergi, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3d002-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3d002-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3d002-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d002-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3d002-143">**[Skapa en testanvändare Synergi](#create-a-synergi-test-user)**  -toohave en motsvarighet för Britta Simon i Synergi som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3d002-143">**[Create a Synergi test user](#create-a-synergi-test-user)** - toohave a counterpart of Britta Simon in Synergi that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3d002-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3d002-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3d002-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3d002-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3d002-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d002-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3d002-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Synergi.</span><span class="sxs-lookup"><span data-stu-id="3d002-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Synergi application.</span></span>

<span data-ttu-id="3d002-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Synergi:**</span><span class="sxs-lookup"><span data-stu-id="3d002-148">**tooconfigure Azure AD single sign-on with Synergi, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d002-149">I hello Azure-portalen på hello **Synergi** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3d002-149">In hello Azure portal, on hello **Synergi** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="3d002-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3d002-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_samlbase.png)

3. <span data-ttu-id="3d002-153">På hello **Synergi domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3d002-153">On hello **Synergi Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och synergi domän med enkel inloggning information](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_url.png)

    <span data-ttu-id="3d002-155">a.</span><span class="sxs-lookup"><span data-stu-id="3d002-155">a.</span></span> <span data-ttu-id="3d002-156">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.irmsecurity.com`</span><span class="sxs-lookup"><span data-stu-id="3d002-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.irmsecurity.com`</span></span>

    <span data-ttu-id="3d002-157">b.</span><span class="sxs-lookup"><span data-stu-id="3d002-157">b.</span></span> <span data-ttu-id="3d002-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.irmsecurity.com/sso/<organization id>`</span><span class="sxs-lookup"><span data-stu-id="3d002-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.irmsecurity.com/sso/<organization id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3d002-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3d002-159">These values are not real.</span></span> <span data-ttu-id="3d002-160">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="3d002-160">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="3d002-161">Kontakta [Synergi supportteamet](https://www.irmsecurity.com/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3d002-161">Contact [Synergi support team](https://www.irmsecurity.com/contact/) tooget these values.</span></span>

4. <span data-ttu-id="3d002-162">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="3d002-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_certificate.png) 

5. <span data-ttu-id="3d002-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3d002-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-synergi-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3d002-166">På hello **Synergi Configuration** klickar du på **konfigurera Synergi** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3d002-166">On hello **Synergi Configuration** section, click **Configure Synergi** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3d002-167">Kopiera hello **Sign-Out URL och SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3d002-167">Copy hello **Sign-Out URL and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Synergi konfiguration](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_configure.png) 

7. <span data-ttu-id="3d002-169">tooconfigure enkel inloggning på **Synergi** sida, behöver du toosend hello hämtas **Certificate(Base64) Sign-Out URL och SAML enhets-ID** för[Synergi supportteamet](https://www.irmsecurity.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="3d002-169">tooconfigure single sign-on on **Synergi** side, you need toosend hello downloaded **Certificate(Base64), Sign-Out URL, and SAML Entity ID** too[Synergi support team](https://www.irmsecurity.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="3d002-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="3d002-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3d002-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="3d002-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3d002-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3d002-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3d002-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3d002-173">Create an Azure AD test user</span></span>

<span data-ttu-id="3d002-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d002-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="3d002-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3d002-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d002-177">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="3d002-177">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-synergi-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="3d002-179">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3d002-179">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-synergi-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="3d002-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d002-181">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-synergi-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="3d002-183">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3d002-183">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-synergi-tutorial/create_aaduser_04.png)

    <span data-ttu-id="3d002-185">a.</span><span class="sxs-lookup"><span data-stu-id="3d002-185">a.</span></span> <span data-ttu-id="3d002-186">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3d002-186">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3d002-187">b.</span><span class="sxs-lookup"><span data-stu-id="3d002-187">b.</span></span> <span data-ttu-id="3d002-188">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d002-188">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="3d002-189">c.</span><span class="sxs-lookup"><span data-stu-id="3d002-189">c.</span></span> <span data-ttu-id="3d002-190">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="3d002-190">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="3d002-191">d.</span><span class="sxs-lookup"><span data-stu-id="3d002-191">d.</span></span> <span data-ttu-id="3d002-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3d002-192">Click **Create**.</span></span>
  
### <a name="create-a-synergi-test-user"></a><span data-ttu-id="3d002-193">Skapa en testanvändare Synergi</span><span class="sxs-lookup"><span data-stu-id="3d002-193">Create a Synergi test user</span></span>

<span data-ttu-id="3d002-194">I det här avsnittet skapar du en användare som kallas Britta Simon i Synergi.</span><span class="sxs-lookup"><span data-stu-id="3d002-194">In this section, you create a user called Britta Simon in Synergi.</span></span> <span data-ttu-id="3d002-195">Arbeta med [Synergi supportteamet](https://www.irmsecurity.com/contact/) att lägga till hello användare i hello Synergi plattform.</span><span class="sxs-lookup"><span data-stu-id="3d002-195">Work with [Synergi support team](https://www.irmsecurity.com/contact/) to add hello users in hello Synergi platform.</span></span> <span data-ttu-id="3d002-196">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3d002-196">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="3d002-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3d002-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="3d002-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSynergi.</span><span class="sxs-lookup"><span data-stu-id="3d002-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSynergi.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="3d002-200">**tooassign Britta Simon tooSynergi utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3d002-200">**tooassign Britta Simon tooSynergi, perform hello following steps:**</span></span>

1. <span data-ttu-id="3d002-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3d002-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3d002-203">Välj i listan med program hello **Synergi**.</span><span class="sxs-lookup"><span data-stu-id="3d002-203">In hello applications list, select **Synergi**.</span></span>

    ![hello Synergi länken i listan med program hello](./media/active-directory-saas-synergi-tutorial/tutorial_synergi_app.png)  

3. <span data-ttu-id="3d002-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3d002-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="3d002-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3d002-207">Click **Add** button.</span></span> <span data-ttu-id="3d002-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d002-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="3d002-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3d002-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3d002-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d002-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3d002-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d002-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3d002-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d002-213">Test single sign-on</span></span>

<span data-ttu-id="3d002-214">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3d002-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3d002-215">Du bör få automatiskt inloggade tooyour Synergi programmet när du klickar på hello Synergi panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3d002-215">When you click hello Synergi tile in hello Access Panel, you should get automatically signed-on tooyour Synergi application.</span></span>
<span data-ttu-id="3d002-216">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3d002-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3d002-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3d002-217">Additional resources</span></span>

* [<span data-ttu-id="3d002-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3d002-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3d002-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3d002-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-synergi-tutorial/tutorial_general_203.png

