---
title: "Självstudier: Azure Active Directory-integrering med TigerText Secure Messenger | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TigerText Secure Messenger."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: a3d7bb9598658c75c567c15751740d885fe4fc27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a><span data-ttu-id="5c49c-103">Självstudier: Azure Active Directory-integrering med TigerText Secure Messenger</span><span class="sxs-lookup"><span data-stu-id="5c49c-103">Tutorial: Azure Active Directory integration with TigerText Secure Messenger</span></span>

<span data-ttu-id="5c49c-104">I kursen får du lära dig hur toointegrate TigerText Secure Messenger med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5c49c-104">In this tutorial, you learn how toointegrate TigerText Secure Messenger with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5c49c-105">Integrera TigerText Secure Messenger med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5c49c-105">Integrating TigerText Secure Messenger with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5c49c-106">Du kan styra i Azure AD som har åtkomst tooTigerText säker Messenger</span><span class="sxs-lookup"><span data-stu-id="5c49c-106">You can control in Azure AD who has access tooTigerText Secure Messenger</span></span>
- <span data-ttu-id="5c49c-107">Du kan aktivera din användare tooautomatically get inloggade tooTigerText Secure Messenger (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5c49c-107">You can enable your users tooautomatically get signed-on tooTigerText Secure Messenger (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5c49c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5c49c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5c49c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5c49c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c49c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5c49c-110">Prerequisites</span></span>

<span data-ttu-id="5c49c-111">tooconfigure Azure AD-integrering med TigerText Secure Messenger, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5c49c-111">tooconfigure Azure AD integration with TigerText Secure Messenger, you need hello following items:</span></span>

- <span data-ttu-id="5c49c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c49c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5c49c-113">En TigerText Secure Messenger enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c49c-113">A TigerText Secure Messenger single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5c49c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c49c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5c49c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5c49c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5c49c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5c49c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5c49c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c49c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5c49c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5c49c-118">Scenario description</span></span>
<span data-ttu-id="5c49c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c49c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5c49c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c49c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5c49c-121">Lägg till TigerText Secure Messenger från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5c49c-121">Add TigerText Secure Messenger from hello gallery</span></span>
2. <span data-ttu-id="5c49c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c49c-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tigertext-secure-messenger-from-hello-gallery"></a><span data-ttu-id="5c49c-123">Lägg till TigerText Secure Messenger från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5c49c-123">Add TigerText Secure Messenger from hello gallery</span></span>
<span data-ttu-id="5c49c-124">tooconfigure hello integrering av TigerText säkra Messenger i Azure AD, behöver du tooadd TigerText Secure Messenger hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5c49c-124">tooconfigure hello integration of TigerText Secure Messenger into Azure AD, you need tooadd TigerText Secure Messenger from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5c49c-125">**tooadd TigerText Secure Messenger från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c49c-125">**tooadd TigerText Secure Messenger from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c49c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c49c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5c49c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5c49c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5c49c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c49c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5c49c-133">Skriv i sökrutan hello **TigerText Secure Messenger**väljer **TigerText Secure Messenger** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5c49c-133">In hello search box, type **TigerText Secure Messenger**, select **TigerText Secure Messenger** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Lägg till från galleriet](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5c49c-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c49c-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="5c49c-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med TigerText Secure Messenger baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5c49c-136">In this section, you configure and test Azure AD single sign-on with TigerText Secure Messenger based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5c49c-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TigerText Secure Messenger är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c49c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TigerText Secure Messenger is tooa user in Azure AD.</span></span> <span data-ttu-id="5c49c-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användaren i TigerText Secure Messenger toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5c49c-138">In other words, a link relationship between an Azure AD user and hello related user in TigerText Secure Messenger needs toobe established.</span></span>

<span data-ttu-id="5c49c-139">Tilldela hello värdet hello i TigerText Secure Messenger **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5c49c-139">In TigerText Secure Messenger, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="5c49c-140">tooconfigure och testa Azure AD enkel inloggning med TigerText Secure Messenger, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c49c-140">tooconfigure and test Azure AD single sign-on with TigerText Secure Messenger, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5c49c-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5c49c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5c49c-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c49c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5c49c-143">**[Skapa en testanvändare TigerText Secure Messenger](#create-a-tigertext-secure-messenger-test-user)**  -toohave en motsvarighet för Britta Simon i TigerText Secure Messenger som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5c49c-143">**[Create a TigerText Secure Messenger test user](#create-a-tigertext-secure-messenger-test-user)** - toohave a counterpart of Britta Simon in TigerText Secure Messenger that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5c49c-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c49c-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5c49c-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5c49c-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="5c49c-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c49c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="5c49c-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i TigerText Secure Messenger-programmet.</span><span class="sxs-lookup"><span data-stu-id="5c49c-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TigerText Secure Messenger application.</span></span>

<span data-ttu-id="5c49c-148">**tooconfigure Azure AD enkel inloggning med TigerText Secure Messenger utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c49c-148">**tooconfigure Azure AD single sign-on with TigerText Secure Messenger, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c49c-149">I hello Azure-portalen på hello **TigerText Secure Messenger** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-149">In hello Azure portal, on hello **TigerText Secure Messenger** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5c49c-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c49c-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_samlbase.png)

3. <span data-ttu-id="5c49c-153">På hello **TigerText Secure Messenger domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c49c-153">On hello **TigerText Secure Messenger Domain and URLs** section, perform hello following steps:</span></span>

    ![Avsnittet TigerText Secure Messenger domän och URL: er](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_url.png)

    <span data-ttu-id="5c49c-155">a.</span><span class="sxs-lookup"><span data-stu-id="5c49c-155">a.</span></span> <span data-ttu-id="5c49c-156">I hello **inloggnings-URL** textruta typen URL: en som:`https://home.tigertext.com`</span><span class="sxs-lookup"><span data-stu-id="5c49c-156">In hello **Sign-on URL** textbox, type URL as: `https://home.tigertext.com`</span></span>

    <span data-ttu-id="5c49c-157">b.</span><span class="sxs-lookup"><span data-stu-id="5c49c-157">b.</span></span> <span data-ttu-id="5c49c-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span><span class="sxs-lookup"><span data-stu-id="5c49c-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5c49c-159">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5c49c-159">This value is not real.</span></span> <span data-ttu-id="5c49c-160">Uppdatera det här värdet med hello faktiska identifierare.</span><span class="sxs-lookup"><span data-stu-id="5c49c-160">Update this value with hello actual Identifier.</span></span> <span data-ttu-id="5c49c-161">Kontakta [TigerText Secure Messenger-klienten supportteamet](mailTo:prosupport@tigertext.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="5c49c-161">Contact [TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="5c49c-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="5c49c-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_certificate.png) 

5. <span data-ttu-id="5c49c-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c49c-164">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-tigertext-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5c49c-166">tooget enkel inloggning konfigurerats för ditt program, kontakta [TigerText Secure Messenger supportteamet](mailTo:prosupport@tigertext.com) och ge dem hello **hämtade metadata**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-166">tooget single sign-on configured for your application, contact [TigerText Secure Messenger support team](mailTo:prosupport@tigertext.com) and provide them hello **Downloaded metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="5c49c-167">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5c49c-167">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5c49c-168">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5c49c-168">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5c49c-169">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5c49c-169">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5c49c-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c49c-170">Create an Azure AD test user</span></span>
<span data-ttu-id="5c49c-171">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c49c-171">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5c49c-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c49c-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c49c-174">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c49c-174">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tigertext-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5c49c-176">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-176">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Användare och grupper -> alla användare](./media/active-directory-saas-tigertext-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5c49c-178">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c49c-178">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-tigertext-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5c49c-180">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c49c-180">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Användardialogrutan](./media/active-directory-saas-tigertext-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5c49c-182">a.</span><span class="sxs-lookup"><span data-stu-id="5c49c-182">a.</span></span> <span data-ttu-id="5c49c-183">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-183">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5c49c-184">b.</span><span class="sxs-lookup"><span data-stu-id="5c49c-184">b.</span></span> <span data-ttu-id="5c49c-185">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5c49c-185">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5c49c-186">c.</span><span class="sxs-lookup"><span data-stu-id="5c49c-186">c.</span></span> <span data-ttu-id="5c49c-187">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-187">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5c49c-188">d.</span><span class="sxs-lookup"><span data-stu-id="5c49c-188">d.</span></span> <span data-ttu-id="5c49c-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-189">Click **Create**.</span></span>
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a><span data-ttu-id="5c49c-190">Skapa en testanvändare TigerText Secure Messenger</span><span class="sxs-lookup"><span data-stu-id="5c49c-190">Create a TigerText Secure Messenger test user</span></span>

<span data-ttu-id="5c49c-191">I det här avsnittet skapar du en användare som kallas Britta Simon i TigerText.</span><span class="sxs-lookup"><span data-stu-id="5c49c-191">In this section, you create a user called Britta Simon in TigerText.</span></span> <span data-ttu-id="5c49c-192">Kontakta för[TigerText Secure Messenger-klienten supportteamet](mailTo:prosupport@tigertext.com) tooadd hello användare i hello TigerText plattform.</span><span class="sxs-lookup"><span data-stu-id="5c49c-192">Please reach out too[TigerText Secure Messenger Client support team](mailTo:prosupport@tigertext.com) tooadd hello users in hello TigerText platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="5c49c-193">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c49c-193">Assign hello Azure AD test user</span></span>

<span data-ttu-id="5c49c-194">I det här avsnittet kan du aktivera Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTigerText Secure Messenger.</span><span class="sxs-lookup"><span data-stu-id="5c49c-194">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTigerText Secure Messenger.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5c49c-196">**tooassign Britta Simon tooTigerText Secure Messenger, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c49c-196">**tooassign Britta Simon tooTigerText Secure Messenger, perform hello following steps:**</span></span>

1. <span data-ttu-id="5c49c-197">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-197">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5c49c-199">Välj i listan med program hello **TigerText Secure Messenger**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-199">In hello applications list, select **TigerText Secure Messenger**.</span></span>

    ![TigerText Secure Messenger i listan över appar](./media/active-directory-saas-tigertext-tutorial/tutorial_tigertext_app.png) 

3. <span data-ttu-id="5c49c-201">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5c49c-201">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5c49c-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c49c-203">Click **Add** button.</span></span> <span data-ttu-id="5c49c-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c49c-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5c49c-206">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5c49c-206">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5c49c-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c49c-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5c49c-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c49c-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="5c49c-209">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c49c-209">Test single sign-on</span></span>

<span data-ttu-id="5c49c-210">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5c49c-210">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5c49c-211">Du bör få automatiskt inloggade tooyour TigerText programmet när du klickar på hello TigerText panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5c49c-211">When you click hello TigerText tile in hello Access Panel, you should get automatically signed-on tooyour TigerText application.</span></span> <span data-ttu-id="5c49c-212">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5c49c-212">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c49c-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5c49c-213">Additional resources</span></span>

* [<span data-ttu-id="5c49c-214">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c49c-214">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c49c-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5c49c-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tigertext-tutorial/tutorial_general_203.png

