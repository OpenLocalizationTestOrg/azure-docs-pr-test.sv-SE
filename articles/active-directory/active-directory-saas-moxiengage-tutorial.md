---
title: "Självstudier: Azure Active Directory-integrering med Moxi engagera | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Moxi engagera."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1135a879-8f00-43b0-ac8a-831593d9586d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jeedes
ms.openlocfilehash: ff3242a0981aff6dff9ec6e3f66f0e7c4a9b5b20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moxi-engage"></a><span data-ttu-id="c4ad1-103">Självstudier: Azure Active Directory-integrering med Moxi engagera</span><span class="sxs-lookup"><span data-stu-id="c4ad1-103">Tutorial: Azure Active Directory integration with Moxi Engage</span></span>

<span data-ttu-id="c4ad1-104">I kursen får du lära dig hur toointegrate Moxi interagera med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c4ad1-104">In this tutorial, you learn how toointegrate Moxi Engage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c4ad1-105">Integrera Moxi interagera med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c4ad1-105">Integrating Moxi Engage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="c4ad1-106">Du kan styra i Azure AD som har åtkomst tooMoxi starta</span><span class="sxs-lookup"><span data-stu-id="c4ad1-106">You can control in Azure AD who has access tooMoxi Engage</span></span>
- <span data-ttu-id="c4ad1-107">Du kan aktivera din användare tooautomatically get inloggade tooMoxi starta (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="c4ad1-107">You can enable your users tooautomatically get signed-on tooMoxi Engage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c4ad1-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c4ad1-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="c4ad1-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c4ad1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4ad1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c4ad1-110">Prerequisites</span></span>

<span data-ttu-id="c4ad1-111">tooconfigure Azure AD-integrering med Moxi engagera måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="c4ad1-111">tooconfigure Azure AD integration with Moxi Engage, you need hello following items:</span></span>

- <span data-ttu-id="c4ad1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c4ad1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c4ad1-113">En Moxi engagera enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c4ad1-113">A Moxi Engage single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c4ad1-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c4ad1-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c4ad1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c4ad1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c4ad1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c4ad1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c4ad1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c4ad1-118">Scenario description</span></span>
<span data-ttu-id="c4ad1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c4ad1-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c4ad1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c4ad1-121">Att lägga till Moxi interaktion från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c4ad1-121">Adding Moxi Engage from hello gallery</span></span>
2. <span data-ttu-id="c4ad1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4ad1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moxi-engage-from-hello-gallery"></a><span data-ttu-id="c4ad1-123">Att lägga till Moxi interaktion från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="c4ad1-123">Adding Moxi Engage from hello gallery</span></span>
<span data-ttu-id="c4ad1-124">tooconfigure hello integrering av Moxi delta i Azure AD, behöver du tooadd Moxi engagera hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-124">tooconfigure hello integration of Moxi Engage into Azure AD, you need tooadd Moxi Engage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="c4ad1-125">**tooadd Moxi interaktion från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c4ad1-125">**tooadd Moxi Engage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4ad1-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c4ad1-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="c4ad1-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="c4ad1-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="c4ad1-133">Skriv i sökrutan hello **Moxi engagera**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-133">In hello search box, type **Moxi Engage**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_search.png)

5. <span data-ttu-id="c4ad1-135">Markera hello resultat på panelen **Moxi engagera**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-135">In hello results panel, select **Moxi Engage**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c4ad1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4ad1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c4ad1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Moxi engagera baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-138">In this section, you configure and test Azure AD single sign-on with Moxi Engage based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c4ad1-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Moxi engagera är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Moxi Engage is tooa user in Azure AD.</span></span> <span data-ttu-id="c4ad1-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Moxi engagera toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-140">In other words, a link relationship between an Azure AD user and hello related user in Moxi Engage needs toobe established.</span></span>

<span data-ttu-id="c4ad1-141">Tilldela hello värdet för hello i Moxi engagera **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-141">In Moxi Engage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="c4ad1-142">tooconfigure och testa Azure AD enkel inloggning med Moxi engagera, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c4ad1-142">tooconfigure and test Azure AD single sign-on with Moxi Engage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="c4ad1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="c4ad1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c4ad1-145">**[Skapa en testanvändare Moxi engagera](#creating-a-moxi-engage-test-user)**  -toohave en motsvarighet för Britta Simon i Moxi engagera som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-145">**[Creating a Moxi Engage test user](#creating-a-moxi-engage-test-user)** - toohave a counterpart of Britta Simon in Moxi Engage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="c4ad1-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c4ad1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c4ad1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4ad1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c4ad1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Moxi engagera program.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Moxi Engage application.</span></span>

<span data-ttu-id="c4ad1-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Moxi engagera:**</span><span class="sxs-lookup"><span data-stu-id="c4ad1-150">**tooconfigure Azure AD single sign-on with Moxi Engage, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4ad1-151">I hello Azure-portalen på hello **Moxi engagera** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-151">In hello Azure portal, on hello **Moxi Engage** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="c4ad1-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_samlbase.png)

3. <span data-ttu-id="c4ad1-155">På hello **Moxi engagera domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c4ad1-155">On hello **Moxi Engage Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_url.png)

    <span data-ttu-id="c4ad1-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://svc.<moxiworks-integration-domain>/service/v1/auth/inbound/saml/aad`</span><span class="sxs-lookup"><span data-stu-id="c4ad1-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://svc.<moxiworks-integration-domain>/service/v1/auth/inbound/saml/aad`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c4ad1-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-158">This value is not real.</span></span> <span data-ttu-id="c4ad1-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="c4ad1-160">Kontakta [Moxi engagera klienten supportteamet](mailto:support@moxiworks.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-160">Contact [Moxi Engage Client support team](mailto:support@moxiworks.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="c4ad1-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_certificate.png) 

5. <span data-ttu-id="c4ad1-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxiengage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c4ad1-165">tooconfigure enkel inloggning på **Moxi engagera** sida, behöver du toosend hello hämtas **XML-Metadata för** för[Moxi engagera supportteamet](mailto:support@moxiworks.com).</span><span class="sxs-lookup"><span data-stu-id="c4ad1-165">tooconfigure single sign-on on **Moxi Engage** side, you need toosend hello downloaded **Metadata XML** too[Moxi Engage support team](mailto:support@moxiworks.com).</span></span> <span data-ttu-id="c4ad1-166">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-166">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="c4ad1-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="c4ad1-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="c4ad1-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="c4ad1-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c4ad1-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c4ad1-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4ad1-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="c4ad1-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="c4ad1-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c4ad1-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4ad1-174">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c4ad1-176">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c4ad1-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c4ad1-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="c4ad1-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-moxiengage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c4ad1-182">a.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-182">a.</span></span> <span data-ttu-id="c4ad1-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c4ad1-184">b.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-184">b.</span></span> <span data-ttu-id="c4ad1-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c4ad1-186">c.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-186">c.</span></span> <span data-ttu-id="c4ad1-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="c4ad1-188">d.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-188">d.</span></span> <span data-ttu-id="c4ad1-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-189">Click **Create**.</span></span>
 
### <a name="creating-a-moxi-engage-test-user"></a><span data-ttu-id="c4ad1-190">Skapa en Moxi engagera testanvändare</span><span class="sxs-lookup"><span data-stu-id="c4ad1-190">Creating a Moxi Engage test user</span></span>

<span data-ttu-id="c4ad1-191">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Moxi engagera.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-191">In this section, you create a user called Britta Simon in Moxi Engage.</span></span> <span data-ttu-id="c4ad1-192">Arbeta med [Moxi engagera supportteamet](mailto:support@moxiworks.com) att lägga till hello användare i hello Moxi engagera plattform.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-192">Work with [Moxi Engage support team](mailto:support@moxiworks.com) to add hello users in hello Moxi Engage platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="c4ad1-193">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="c4ad1-193">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="c4ad1-194">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMoxi starta.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMoxi Engage.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c4ad1-196">**tooassign Britta Simon tooMoxi starta, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="c4ad1-196">**tooassign Britta Simon tooMoxi Engage, perform hello following steps:**</span></span>

1. <span data-ttu-id="c4ad1-197">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c4ad1-199">Välj i listan med program hello **Moxi engagera**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-199">In hello applications list, select **Moxi Engage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moxiengage-tutorial/tutorial_moxiengage_app.png) 

3. <span data-ttu-id="c4ad1-201">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c4ad1-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-203">Click **Add** button.</span></span> <span data-ttu-id="c4ad1-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c4ad1-206">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="c4ad1-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c4ad1-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c4ad1-209">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c4ad1-209">Testing single sign-on</span></span>

<span data-ttu-id="c4ad1-210">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="c4ad1-211">Du bör få automatisk inloggning tooMoxi genomföra programmet när du klickar på hello Moxi engagera panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="c4ad1-211">When you click hello Moxi Engage tile in hello Access Panel, you should get automatic login tooMoxi Engage application.</span></span>
<span data-ttu-id="c4ad1-212">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c4ad1-212">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c4ad1-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c4ad1-213">Additional resources</span></span>

* [<span data-ttu-id="c4ad1-214">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c4ad1-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c4ad1-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c4ad1-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moxiengage-tutorial/tutorial_general_203.png

