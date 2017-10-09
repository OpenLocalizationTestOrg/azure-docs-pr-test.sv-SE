---
title: "Självstudier: Azure Active Directory-integrering med Intralinks | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Intralinks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 6fa49c932d0c48d4b48e04fe91af9fc86a0c1cdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a><span data-ttu-id="84e2b-103">Självstudier: Azure Active Directory-integrering med Intralinks</span><span class="sxs-lookup"><span data-stu-id="84e2b-103">Tutorial: Azure Active Directory integration with Intralinks</span></span>

<span data-ttu-id="84e2b-104">I kursen får du lära dig hur toointegrate Intralinks med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="84e2b-104">In this tutorial, you learn how toointegrate Intralinks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84e2b-105">Integrera Intralinks med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="84e2b-105">Integrating Intralinks with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="84e2b-106">Du kan styra i Azure AD som har åtkomst till tooIntralinks</span><span class="sxs-lookup"><span data-stu-id="84e2b-106">You can control in Azure AD who has access tooIntralinks</span></span>
- <span data-ttu-id="84e2b-107">Du kan aktivera din användare tooautomatically get inloggade tooIntralinks (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="84e2b-107">You can enable your users tooautomatically get signed-on tooIntralinks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84e2b-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="84e2b-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="84e2b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84e2b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84e2b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="84e2b-110">Prerequisites</span></span>

<span data-ttu-id="84e2b-111">tooconfigure Azure AD-integrering med Intralinks, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="84e2b-111">tooconfigure Azure AD integration with Intralinks, you need hello following items:</span></span>

- <span data-ttu-id="84e2b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="84e2b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84e2b-113">En Intralinks enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="84e2b-113">An Intralinks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84e2b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="84e2b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84e2b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="84e2b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84e2b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="84e2b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84e2b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84e2b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84e2b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="84e2b-118">Scenario description</span></span>
<span data-ttu-id="84e2b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="84e2b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84e2b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="84e2b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84e2b-121">Att lägga till Intralinks från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="84e2b-121">Adding Intralinks from hello gallery</span></span>
2. <span data-ttu-id="84e2b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e2b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intralinks-from-hello-gallery"></a><span data-ttu-id="84e2b-123">Att lägga till Intralinks från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="84e2b-123">Adding Intralinks from hello gallery</span></span>
<span data-ttu-id="84e2b-124">tooconfigure hello integrering av Intralinks i Azure AD, behöver du tooadd Intralinks hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="84e2b-124">tooconfigure hello integration of Intralinks into Azure AD, you need tooadd Intralinks from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="84e2b-125">**tooadd Intralinks från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84e2b-125">**tooadd Intralinks from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e2b-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84e2b-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84e2b-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="84e2b-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="84e2b-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="84e2b-133">Skriv i sökrutan hello **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-133">In hello search box, type **Intralinks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="84e2b-135">Markera hello resultat på panelen **Intralinks**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="84e2b-135">In hello results panel, select **Intralinks**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84e2b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e2b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84e2b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Intralinks baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="84e2b-138">In this section, you configure and test Azure AD single sign-on with Intralinks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="84e2b-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Intralinks är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="84e2b-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Intralinks is tooa user in Azure AD.</span></span> <span data-ttu-id="84e2b-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Intralinks toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="84e2b-140">In other words, a link relationship between an Azure AD user and hello related user in Intralinks needs toobe established.</span></span>

<span data-ttu-id="84e2b-141">I Intralinks, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-141">In Intralinks, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="84e2b-142">tooconfigure och testa Azure AD enkel inloggning med Intralinks, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="84e2b-142">tooconfigure and test Azure AD single sign-on with Intralinks, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="84e2b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="84e2b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84e2b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84e2b-145">**[Skapa en testanvändare Intralinks](#creating-an-intralinks-test-user)**  -toohave en motsvarighet för Britta Simon i Intralinks som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="84e2b-145">**[Creating an Intralinks test user](#creating-an-intralinks-test-user)** - toohave a counterpart of Britta Simon in Intralinks that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="84e2b-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84e2b-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84e2b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="84e2b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84e2b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e2b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84e2b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Intralinks program.</span><span class="sxs-lookup"><span data-stu-id="84e2b-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Intralinks application.</span></span>

<span data-ttu-id="84e2b-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Intralinks:**</span><span class="sxs-lookup"><span data-stu-id="84e2b-150">**tooconfigure Azure AD single sign-on with Intralinks, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e2b-151">I hello Azure-portalen på hello **Intralinks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-151">In hello Azure portal, on hello **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="84e2b-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84e2b-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. <span data-ttu-id="84e2b-155">På hello **Intralinks domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="84e2b-155">On hello **Intralinks Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    <span data-ttu-id="84e2b-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span><span class="sxs-lookup"><span data-stu-id="84e2b-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern:  `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="84e2b-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="84e2b-158">This value is not real.</span></span> <span data-ttu-id="84e2b-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="84e2b-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="84e2b-160">Kontakta [Intralinks klienten supportteamet](https://www.intralinks.com/contact-1) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="84e2b-160">Contact [Intralinks Client support team](https://www.intralinks.com/contact-1) tooget this value.</span></span> 
 
4. <span data-ttu-id="84e2b-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="84e2b-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. <span data-ttu-id="84e2b-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="84e2b-165">tooconfigure enkel inloggning på **Intralinks** sida, behöver du toosend hello hämtas **XML-Metadata för** [Intralinks supportteam](https://www.intralinks.com/contact-1).</span><span class="sxs-lookup"><span data-stu-id="84e2b-165">tooconfigure single sign-on on **Intralinks** side, you need toosend hello downloaded **Metadata XML** [Intralinks support team](https://www.intralinks.com/contact-1).</span></span> <span data-ttu-id="84e2b-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="84e2b-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="84e2b-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="84e2b-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="84e2b-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="84e2b-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="84e2b-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84e2b-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84e2b-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="84e2b-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="84e2b-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84e2b-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="84e2b-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84e2b-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e2b-174">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84e2b-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84e2b-176">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84e2b-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84e2b-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="84e2b-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84e2b-182">a.</span><span class="sxs-lookup"><span data-stu-id="84e2b-182">a.</span></span> <span data-ttu-id="84e2b-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84e2b-184">b.</span><span class="sxs-lookup"><span data-stu-id="84e2b-184">b.</span></span> <span data-ttu-id="84e2b-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84e2b-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84e2b-186">c.</span><span class="sxs-lookup"><span data-stu-id="84e2b-186">c.</span></span> <span data-ttu-id="84e2b-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="84e2b-188">d.</span><span class="sxs-lookup"><span data-stu-id="84e2b-188">d.</span></span> <span data-ttu-id="84e2b-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-189">Click **Create**.</span></span>
 
### <a name="creating-an-intralinks-test-user"></a><span data-ttu-id="84e2b-190">Skapa en testanvändare Intralinks</span><span class="sxs-lookup"><span data-stu-id="84e2b-190">Creating an Intralinks test user</span></span>

<span data-ttu-id="84e2b-191">I det här avsnittet skapar du en användare som kallas Britta Simon i Intralinks.</span><span class="sxs-lookup"><span data-stu-id="84e2b-191">In this section, you create a user called Britta Simon in Intralinks.</span></span> <span data-ttu-id="84e2b-192">Se tillsammans med [Intralinks supportteam](https://www.intralinks.com/contact-1) tooadd hello användare i hello Intralinks plattform.</span><span class="sxs-lookup"><span data-stu-id="84e2b-192">Please work with [Intralinks support team](https://www.intralinks.com/contact-1) tooadd hello users in hello Intralinks platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="84e2b-193">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="84e2b-193">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="84e2b-194">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooIntralinks.</span><span class="sxs-lookup"><span data-stu-id="84e2b-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIntralinks.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="84e2b-196">**tooassign Britta Simon tooIntralinks utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84e2b-196">**tooassign Britta Simon tooIntralinks, perform hello following steps:**</span></span>

1. <span data-ttu-id="84e2b-197">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="84e2b-199">Välj i listan med program hello **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-199">In hello applications list, select **Intralinks**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. <span data-ttu-id="84e2b-201">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="84e2b-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-203">Click **Add** button.</span></span> <span data-ttu-id="84e2b-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="84e2b-206">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="84e2b-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84e2b-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-208">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="add-intralinks-via-or-elite-application"></a><span data-ttu-id="84e2b-209">Lägg till Intralinks VIA eller Elite program</span><span class="sxs-lookup"><span data-stu-id="84e2b-209">Add Intralinks VIA or Elite application</span></span>

<span data-ttu-id="84e2b-210">Intralinks använder hello samma SSO-identitetsplattformen för alla andra Intralinks program utom affären Nexus program.</span><span class="sxs-lookup"><span data-stu-id="84e2b-210">Intralinks uses hello same SSO identity platform for all other Intralinks applications excluding Deal Nexus application.</span></span> <span data-ttu-id="84e2b-211">Så om du planerar toouse något annat program för Intralinks sedan du först tooconfigure enkel inloggning för en primär Intralinks program med hjälp av hello proceduren ovan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-211">So if you plan toouse any other Intralinks application then first you have tooconfigure SSO for one Primary Intralinks application using hello procedure described above.</span></span>

<span data-ttu-id="84e2b-212">Efter det att du kan följa hello under proceduren tooadd ett annat Intralinks program i din klient som kan använda den här primära programmet för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84e2b-212">After that you can follow hello below procedure tooadd another Intralinks application in your tenant which can leverage this primary application for SSO.</span></span> 

>[!NOTE]
><span data-ttu-id="84e2b-213">Den här funktionen är tillgänglig endast tooAzure AD Premium-SKU kunder och inte tillgängliga för kostnadsfri eller grundläggande SKU-kunder.</span><span class="sxs-lookup"><span data-stu-id="84e2b-213">This feature is available only tooAzure AD Premium SKU Customers and not available for Free or Basic SKU customers.</span></span>

1. <span data-ttu-id="84e2b-214">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84e2b-214">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]


2. <span data-ttu-id="84e2b-216">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-216">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="84e2b-217">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-217">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="84e2b-219">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-219">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="84e2b-221">Skriv i sökrutan hello **Intralinks**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-221">In hello search box, type **Intralinks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. <span data-ttu-id="84e2b-223">På **Intralinks Lägg till app** utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="84e2b-223">On **Intralinks Add app** perform hello following steps:</span></span>

    ![Lägga till Intralinks VIA eller Elite program](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    <span data-ttu-id="84e2b-225">a.</span><span class="sxs-lookup"><span data-stu-id="84e2b-225">a.</span></span> <span data-ttu-id="84e2b-226">I **namn** textruta, t.ex. Ange rätt namn av programmet hello **Intralinks Elite**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-226">In **Name** textbox, enter appropriate name of hello application e.g. **Intralinks Elite**.</span></span>

    <span data-ttu-id="84e2b-227">b.</span><span class="sxs-lookup"><span data-stu-id="84e2b-227">b.</span></span> <span data-ttu-id="84e2b-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-228">Click **Add** button.</span></span>

6.  <span data-ttu-id="84e2b-229">I hello Azure-portalen på hello **Intralinks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-229">In hello Azure portal, on hello **Intralinks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

7. <span data-ttu-id="84e2b-231">På hello **enkel inloggning** markerar **läge** som **inloggning länkade**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-231">On hello **Single sign-on** dialog, select **Mode** as   **Linked Sign-on**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. <span data-ttu-id="84e2b-233">Hämta hello hello SP initierade SSO-URL från [Intralinks team](https://www.intralinks.com/contact-1) för hello andra Intralinks program och ange den i **konfigurera inloggnings-URL** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="84e2b-233">Get hello hello SP Initiated SSO URL from [Intralinks team](https://www.intralinks.com/contact-1) for hello other Intralinks application and enter it in **Configure Sign-on URL** as shown below.</span></span> 
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     <span data-ttu-id="84e2b-235">Skriv hello-URL som används av dina användare toosign på tooyour Intralinks program med hjälp av hello efter mönster i hello logga på URL: en textruta:</span><span class="sxs-lookup"><span data-stu-id="84e2b-235">In hello Sign On URL textbox, type hello URL used by your users toosign-on tooyour Intralinks application using hello following pattern:</span></span>
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. <span data-ttu-id="84e2b-236">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-236">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. <span data-ttu-id="84e2b-238">Tilldela hello programmet toouser eller grupper som visas i avsnittet hello  **[tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**.</span><span class="sxs-lookup"><span data-stu-id="84e2b-238">Assign hello application toouser or groups as shown in hello section **[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)**.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="84e2b-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84e2b-239">Testing single sign-on</span></span>

<span data-ttu-id="84e2b-240">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="84e2b-241">Du bör få automatiskt inloggade tooyour Intralinks programmet när du klickar på hello Intralinks panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="84e2b-241">When you click hello Intralinks tile in hello Access Panel, you should get automatically signed-on tooyour Intralinks application.</span></span>
<span data-ttu-id="84e2b-242">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="84e2b-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="84e2b-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="84e2b-243">Additional resources</span></span>

* [<span data-ttu-id="84e2b-244">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="84e2b-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84e2b-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="84e2b-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png

