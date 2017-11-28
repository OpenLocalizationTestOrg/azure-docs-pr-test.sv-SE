---
title: "Självstudier: Azure Active Directory-integrering med Capriza plattform | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Capriza plattform."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 48d92247-f00a-47b9-8d4e-137028d9e200
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 1c4adb737bb5ba4690bbf74688010238c5c83f3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-capriza-platform"></a><span data-ttu-id="67d7a-103">Självstudier: Azure Active Directory-integrering med Capriza plattform</span><span class="sxs-lookup"><span data-stu-id="67d7a-103">Tutorial: Azure Active Directory integration with Capriza Platform</span></span>

<span data-ttu-id="67d7a-104">I kursen får du lära dig hur toointegrate Capriza plattform med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="67d7a-104">In this tutorial, you learn how toointegrate Capriza Platform with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="67d7a-105">Integrera Capriza plattform med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="67d7a-105">Integrating Capriza Platform with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="67d7a-106">Du kan styra i Azure AD som har åtkomst tooCapriza plattform</span><span class="sxs-lookup"><span data-stu-id="67d7a-106">You can control in Azure AD who has access tooCapriza Platform</span></span>
- <span data-ttu-id="67d7a-107">Du kan aktivera din användare tooautomatically get inloggade tooCapriza plattform (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="67d7a-107">You can enable your users tooautomatically get signed-on tooCapriza Platform (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="67d7a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="67d7a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="67d7a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="67d7a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67d7a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="67d7a-110">Prerequisites</span></span>

<span data-ttu-id="67d7a-111">tooconfigure Azure AD-integrering med Capriza plattform, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="67d7a-111">tooconfigure Azure AD integration with Capriza Platform, you need hello following items:</span></span>

- <span data-ttu-id="67d7a-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="67d7a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="67d7a-113">En Capriza plattform enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="67d7a-113">A Capriza Platform single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="67d7a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="67d7a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="67d7a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="67d7a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="67d7a-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="67d7a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="67d7a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="67d7a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="67d7a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="67d7a-118">Scenario description</span></span>
<span data-ttu-id="67d7a-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="67d7a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="67d7a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="67d7a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="67d7a-121">Att lägga till Capriza plattform från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="67d7a-121">Adding Capriza Platform from hello gallery</span></span>
2. <span data-ttu-id="67d7a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="67d7a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-capriza-platform-from-hello-gallery"></a><span data-ttu-id="67d7a-123">Att lägga till Capriza plattform från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="67d7a-123">Adding Capriza Platform from hello gallery</span></span>
<span data-ttu-id="67d7a-124">tooconfigure hello integrering av Capriza plattform i Azure AD, behöver du tooadd Capriza plattform hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="67d7a-124">tooconfigure hello integration of Capriza Platform into Azure AD, you need tooadd Capriza Platform from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="67d7a-125">**tooadd Capriza plattform från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="67d7a-125">**tooadd Capriza Platform from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="67d7a-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="67d7a-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="67d7a-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="67d7a-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="67d7a-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="67d7a-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="67d7a-133">Skriv i sökrutan hello **Capriza plattform**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-133">In hello search box, type **Capriza Platform**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_search.png)

5. <span data-ttu-id="67d7a-135">Markera hello resultat på panelen **Capriza plattform**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="67d7a-135">In hello results panel, select **Capriza Platform**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="67d7a-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="67d7a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="67d7a-138">I det här avsnittet du konfigurera och testa Azure AD enkel inloggning med Capriza plattform baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="67d7a-138">In this section, you configure and test Azure AD single sign-on with Capriza Platform based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="67d7a-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Capriza Platform är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="67d7a-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Capriza Platform is tooa user in Azure AD.</span></span> <span data-ttu-id="67d7a-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i Capriza Platform toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="67d7a-140">In other words, a link relationship between an Azure AD user and hello related user in Capriza Platform needs toobe established.</span></span>

<span data-ttu-id="67d7a-141">Tilldela hello värdet hello i Capriza Platform **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="67d7a-141">In Capriza Platform, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="67d7a-142">tooconfigure och testa Azure AD enkel inloggning med Capriza plattform, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="67d7a-142">tooconfigure and test Azure AD single sign-on with Capriza Platform, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="67d7a-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="67d7a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="67d7a-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="67d7a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="67d7a-145">**[Skapa en testanvändare Capriza plattform](#creating-a-capriza-platform-test-user)**  -toohave en motsvarighet för Britta Simon i Capriza plattform som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="67d7a-145">**[Creating a Capriza Platform test user](#creating-a-capriza-platform-test-user)** - toohave a counterpart of Britta Simon in Capriza Platform that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="67d7a-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="67d7a-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="67d7a-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="67d7a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="67d7a-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="67d7a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="67d7a-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Capriza plattform.</span><span class="sxs-lookup"><span data-stu-id="67d7a-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Capriza Platform application.</span></span>

<span data-ttu-id="67d7a-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Capriza plattform:**</span><span class="sxs-lookup"><span data-stu-id="67d7a-150">**tooconfigure Azure AD single sign-on with Capriza Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="67d7a-151">I hello Azure-portalen på hello **Capriza plattform** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-151">In hello Azure portal, on hello **Capriza Platform** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="67d7a-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="67d7a-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_samlbase.png)

3. <span data-ttu-id="67d7a-155">På hello **Capriza plattform domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="67d7a-155">On hello **Capriza Platform Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_url.png)

    <span data-ttu-id="67d7a-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.capriza.com/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="67d7a-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.capriza.com/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="67d7a-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="67d7a-158">This value is not real.</span></span> <span data-ttu-id="67d7a-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="67d7a-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="67d7a-160">Kontakta [Capriza plattform klienten supportteamet](mailTo:support@capriza.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="67d7a-160">Contact [Capriza Platform Client support team](mailTo:support@capriza.com) tooget this value.</span></span> 

4. <span data-ttu-id="67d7a-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="67d7a-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_certificate.png) 

5. <span data-ttu-id="67d7a-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="67d7a-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-capriza-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="67d7a-165">På hello **Capriza plattformskonfigurationen** klickar du på **konfigurera Capriza plattform** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="67d7a-165">On hello **Capriza Platform Configuration** section, click **Configure Capriza Platform** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="67d7a-166">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="67d7a-166">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_configure.png) 

7. <span data-ttu-id="67d7a-168">tooconfigure enkel inloggning på **Capriza plattform** sida, behöver du toosend hello hämtas **certifikat**, **Sign-Out URL**, **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** för[Capriza plattform supportteamet](mailTo:support@capriza.com).</span><span class="sxs-lookup"><span data-stu-id="67d7a-168">tooconfigure single sign-on on **Capriza Platform** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL**, **SAML Entity ID** and **SAML Single Sign-On Service URL** too[Capriza Platform support team](mailTo:support@capriza.com).</span></span> <span data-ttu-id="67d7a-169">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="67d7a-169">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="67d7a-170">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="67d7a-170">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="67d7a-171">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="67d7a-171">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="67d7a-172">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="67d7a-172">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="67d7a-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="67d7a-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="67d7a-174">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="67d7a-174">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="67d7a-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="67d7a-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="67d7a-177">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="67d7a-177">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="67d7a-179">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-179">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="67d7a-181">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="67d7a-181">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="67d7a-183">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="67d7a-183">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-capriza-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="67d7a-185">a.</span><span class="sxs-lookup"><span data-stu-id="67d7a-185">a.</span></span> <span data-ttu-id="67d7a-186">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-186">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="67d7a-187">b.</span><span class="sxs-lookup"><span data-stu-id="67d7a-187">b.</span></span> <span data-ttu-id="67d7a-188">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="67d7a-188">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="67d7a-189">c.</span><span class="sxs-lookup"><span data-stu-id="67d7a-189">c.</span></span> <span data-ttu-id="67d7a-190">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-190">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="67d7a-191">d.</span><span class="sxs-lookup"><span data-stu-id="67d7a-191">d.</span></span> <span data-ttu-id="67d7a-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-192">Click **Create**.</span></span>
 
### <a name="creating-a-capriza-platform-test-user"></a><span data-ttu-id="67d7a-193">Skapa en testanvändare Capriza plattform</span><span class="sxs-lookup"><span data-stu-id="67d7a-193">Creating a Capriza Platform test user</span></span>

<span data-ttu-id="67d7a-194">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Capriza.</span><span class="sxs-lookup"><span data-stu-id="67d7a-194">hello objective of this section is toocreate a user called Britta Simon in Capriza.</span></span> <span data-ttu-id="67d7a-195">Capriza stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="67d7a-195">Capriza supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="67d7a-196">**Kontrollera att domännamnet är konfigurerad med Capriza för användaretablering. Efter att endast hello fungerar-in-time-användaretablering.**</span><span class="sxs-lookup"><span data-stu-id="67d7a-196">**Please make sure that your domain name is configured with Capriza for user provisioning. After that only hello just-in-time user provisioning will work.**</span></span>

<span data-ttu-id="67d7a-197">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="67d7a-197">There is no action item for you in this section.</span></span> <span data-ttu-id="67d7a-198">En ny användare skapas under ett försök tooaccess Capriza om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="67d7a-198">A new user will be created during an attempt tooaccess Capriza if it doesn't exist yet.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="67d7a-199">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="67d7a-199">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="67d7a-200">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCapriza plattform.</span><span class="sxs-lookup"><span data-stu-id="67d7a-200">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCapriza Platform.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="67d7a-202">**tooassign Britta Simon tooCapriza plattform, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="67d7a-202">**tooassign Britta Simon tooCapriza Platform, perform hello following steps:**</span></span>

1. <span data-ttu-id="67d7a-203">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-203">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="67d7a-205">Välj i listan med program hello **Capriza plattform**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-205">In hello applications list, select **Capriza Platform**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-capriza-tutorial/tutorial_caprizaplatform_app.png) 

3. <span data-ttu-id="67d7a-207">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="67d7a-207">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="67d7a-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="67d7a-209">Click **Add** button.</span></span> <span data-ttu-id="67d7a-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="67d7a-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="67d7a-212">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="67d7a-212">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="67d7a-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="67d7a-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="67d7a-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="67d7a-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="67d7a-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="67d7a-215">Testing single sign-on</span></span>

<span data-ttu-id="67d7a-216">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="67d7a-216">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="67d7a-217">Du bör få automatiskt inloggade tooyour Capriza programmet när du klickar på hello Capriza plattform panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="67d7a-217">When you click hello Capriza Platform tile in hello Access Panel, you should get automatically signed-on tooyour Capriza application.</span></span> <span data-ttu-id="67d7a-218">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="67d7a-218">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="67d7a-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="67d7a-219">Additional resources</span></span>

* [<span data-ttu-id="67d7a-220">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="67d7a-220">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="67d7a-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="67d7a-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-capriza-tutorial/tutorial_general_203.png

