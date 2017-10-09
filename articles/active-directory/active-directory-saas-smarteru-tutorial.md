---
title: "Självstudier: Azure Active Directory-integrering med SmarterU | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SmarterU."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: a3b81b557c47e31f09e61bcf75dd23f370e642e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a><span data-ttu-id="0a002-103">Självstudier: Azure Active Directory-integrering med SmarterU</span><span class="sxs-lookup"><span data-stu-id="0a002-103">Tutorial: Azure Active Directory integration with SmarterU</span></span>

<span data-ttu-id="0a002-104">I kursen får du lära dig hur toointegrate SmarterU med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0a002-104">In this tutorial, you learn how toointegrate SmarterU with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0a002-105">Integrera SmarterU med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0a002-105">Integrating SmarterU with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0a002-106">Du kan styra i Azure AD som har åtkomst till tooSmarterU</span><span class="sxs-lookup"><span data-stu-id="0a002-106">You can control in Azure AD who has access tooSmarterU</span></span>
- <span data-ttu-id="0a002-107">Du kan aktivera din användare tooautomatically get inloggade tooSmarterU (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0a002-107">You can enable your users tooautomatically get signed-on tooSmarterU (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0a002-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0a002-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="0a002-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0a002-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a002-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0a002-110">Prerequisites</span></span>

<span data-ttu-id="0a002-111">tooconfigure Azure AD-integrering med SmarterU, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0a002-111">tooconfigure Azure AD integration with SmarterU, you need hello following items:</span></span>

- <span data-ttu-id="0a002-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0a002-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0a002-113">En SmarterU enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="0a002-113">A SmarterU single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0a002-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0a002-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0a002-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0a002-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0a002-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0a002-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0a002-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a002-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0a002-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0a002-118">Scenario description</span></span>
<span data-ttu-id="0a002-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0a002-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0a002-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0a002-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0a002-121">Att lägga till SmarterU från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0a002-121">Adding SmarterU from hello gallery</span></span>
2. <span data-ttu-id="0a002-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a002-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-smarteru-from-hello-gallery"></a><span data-ttu-id="0a002-123">Att lägga till SmarterU från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0a002-123">Adding SmarterU from hello gallery</span></span>
<span data-ttu-id="0a002-124">tooconfigure hello integrering av SmarterU i Azure AD, behöver du tooadd SmarterU hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="0a002-124">tooconfigure hello integration of SmarterU into Azure AD, you need tooadd SmarterU from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0a002-125">**tooadd SmarterU från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0a002-125">**tooadd SmarterU from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a002-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0a002-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0a002-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="0a002-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0a002-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="0a002-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="0a002-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a002-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="0a002-133">Skriv i sökrutan hello **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="0a002-133">In hello search box, type **SmarterU**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_search.png)

5. <span data-ttu-id="0a002-135">Markera hello resultat på panelen **SmarterU**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="0a002-135">In hello results panel, select **SmarterU**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0a002-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a002-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0a002-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SmarterU baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0a002-138">In this section, you configure and test Azure AD single sign-on with SmarterU based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0a002-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SmarterU är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0a002-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SmarterU is tooa user in Azure AD.</span></span> <span data-ttu-id="0a002-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SmarterU toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="0a002-140">In other words, a link relationship between an Azure AD user and hello related user in SmarterU needs toobe established.</span></span>

<span data-ttu-id="0a002-141">I SmarterU, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="0a002-141">In SmarterU, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0a002-142">tooconfigure och testa Azure AD enkel inloggning med SmarterU, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0a002-142">tooconfigure and test Azure AD single sign-on with SmarterU, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0a002-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0a002-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0a002-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0a002-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0a002-145">**[Skapa en testanvändare SmarterU](#creating-a-smarteru-test-user)**  -toohave en motsvarighet för Britta Simon i SmarterU som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="0a002-145">**[Creating a SmarterU test user](#creating-a-smarteru-test-user)** - toohave a counterpart of Britta Simon in SmarterU that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0a002-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0a002-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0a002-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0a002-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0a002-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a002-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0a002-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt SmarterU program.</span><span class="sxs-lookup"><span data-stu-id="0a002-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SmarterU application.</span></span>

<span data-ttu-id="0a002-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SmarterU:**</span><span class="sxs-lookup"><span data-stu-id="0a002-150">**tooconfigure Azure AD single sign-on with SmarterU, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a002-151">I hello Azure-portalen på hello **SmarterU** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="0a002-151">In hello Azure portal, on hello **SmarterU** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="0a002-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0a002-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. <span data-ttu-id="0a002-155">På hello **SmarterU domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a002-155">On hello **SmarterU Domain and URLs** section, perform hello following steps:</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_url.png)

    <span data-ttu-id="0a002-157">I hello **identifierare** textruta typen hello URL:`https://www.smarteru.com/`</span><span class="sxs-lookup"><span data-stu-id="0a002-157">In hello **Identifier** textbox, type hello URL: `https://www.smarteru.com/`</span></span>

4. <span data-ttu-id="0a002-158">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="0a002-158">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. <span data-ttu-id="0a002-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="0a002-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0a002-162">Logga in tooyour SmarterU företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="0a002-162">In a different web browser window, log in tooyour SmarterU company site as an administrator.</span></span>

7. <span data-ttu-id="0a002-163">Klicka i hello verktygsfältet hello längst upp **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="0a002-163">In hello toolbar on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="0a002-164">![Kontoinställningar](./media/active-directory-saas-smarteru-tutorial/IC777326.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="0a002-164">![Account Settings](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")</span></span>

8. <span data-ttu-id="0a002-165">På konfigurationssidan för hello konto utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a002-165">On hello account configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="0a002-166">![Externa auktorisering](./media/active-directory-saas-smarteru-tutorial/IC777327.png "externa auktorisering")</span><span class="sxs-lookup"><span data-stu-id="0a002-166">![External Authorization](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")</span></span> 
 
      <span data-ttu-id="0a002-167">a.</span><span class="sxs-lookup"><span data-stu-id="0a002-167">a.</span></span> <span data-ttu-id="0a002-168">Välj **Aktivera externa auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="0a002-168">Select **Enable External Authorization**.</span></span>
  
      <span data-ttu-id="0a002-169">b.</span><span class="sxs-lookup"><span data-stu-id="0a002-169">b.</span></span> <span data-ttu-id="0a002-170">I hello **Master inloggningen kontrollen** avsnitt, Välj hello **SmarterU** fliken.</span><span class="sxs-lookup"><span data-stu-id="0a002-170">In hello **Master Login Control** section, select hello **SmarterU** tab.</span></span>
  
      <span data-ttu-id="0a002-171">c.</span><span class="sxs-lookup"><span data-stu-id="0a002-171">c.</span></span> <span data-ttu-id="0a002-172">I hello **standard användarinloggning** avsnitt, Välj hello **SmarterU** fliken.</span><span class="sxs-lookup"><span data-stu-id="0a002-172">In hello **User Default Login** section, select hello **SmarterU** tab.</span></span>
  
      <span data-ttu-id="0a002-173">d.</span><span class="sxs-lookup"><span data-stu-id="0a002-173">d.</span></span> <span data-ttu-id="0a002-174">Välj **aktivera Okta**.</span><span class="sxs-lookup"><span data-stu-id="0a002-174">Select **Enable Okta**.</span></span>
  
      <span data-ttu-id="0a002-175">e.</span><span class="sxs-lookup"><span data-stu-id="0a002-175">e.</span></span> <span data-ttu-id="0a002-176">Kopiera hello innehållet för hello hämtade metadatafil och klistra in den i hello **Okta Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="0a002-176">Copy hello content of hello downloaded metadata file, and then paste it into hello **Okta Metadata** textbox.</span></span>
  
      <span data-ttu-id="0a002-177">f.</span><span class="sxs-lookup"><span data-stu-id="0a002-177">f.</span></span> <span data-ttu-id="0a002-178">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0a002-178">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="0a002-179">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="0a002-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0a002-180">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="0a002-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0a002-181">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0a002-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0a002-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a002-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="0a002-183">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0a002-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="0a002-185">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0a002-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a002-186">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="0a002-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0a002-188">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="0a002-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0a002-190">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a002-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0a002-192">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0a002-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0a002-194">a.</span><span class="sxs-lookup"><span data-stu-id="0a002-194">a.</span></span> <span data-ttu-id="0a002-195">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0a002-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0a002-196">b.</span><span class="sxs-lookup"><span data-stu-id="0a002-196">b.</span></span> <span data-ttu-id="0a002-197">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0a002-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0a002-198">c.</span><span class="sxs-lookup"><span data-stu-id="0a002-198">c.</span></span> <span data-ttu-id="0a002-199">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0a002-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="0a002-200">d.</span><span class="sxs-lookup"><span data-stu-id="0a002-200">d.</span></span> <span data-ttu-id="0a002-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0a002-201">Click **Create**.</span></span>
 
### <a name="creating-a-smarteru-test-user"></a><span data-ttu-id="0a002-202">Skapa en testanvändare SmarterU</span><span class="sxs-lookup"><span data-stu-id="0a002-202">Creating a SmarterU test user</span></span>

<span data-ttu-id="0a002-203">tooenable Azure AD-användare toolog i tooSmarterU, måste de etableras i SmarterU.</span><span class="sxs-lookup"><span data-stu-id="0a002-203">tooenable Azure AD users toolog in tooSmarterU, they must be provisioned into SmarterU.</span></span>

<span data-ttu-id="0a002-204">När SmarterU, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0a002-204">When SmarterU, provisioning is a manual task.</span></span>

<span data-ttu-id="0a002-205">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="0a002-205">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a002-206">Logga in tooyour **SmarterU** klient.</span><span class="sxs-lookup"><span data-stu-id="0a002-206">Log in tooyour **SmarterU** tenant.</span></span>

2. <span data-ttu-id="0a002-207">Gå för**användare**.</span><span class="sxs-lookup"><span data-stu-id="0a002-207">Go too**Users**.</span></span>

3. <span data-ttu-id="0a002-208">Utför följande hello under hello användare:</span><span class="sxs-lookup"><span data-stu-id="0a002-208">In hello user section, perform hello following steps:</span></span>
   
    <span data-ttu-id="0a002-209">![Ny användare](./media/active-directory-saas-smarteru-tutorial/IC777329.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="0a002-209">![New User](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")</span></span>  

    <span data-ttu-id="0a002-210">a.</span><span class="sxs-lookup"><span data-stu-id="0a002-210">a.</span></span> <span data-ttu-id="0a002-211">Klicka på **+ användaren**.</span><span class="sxs-lookup"><span data-stu-id="0a002-211">Click **+User**.</span></span>
    
    <span data-ttu-id="0a002-212">b.</span><span class="sxs-lookup"><span data-stu-id="0a002-212">b.</span></span> <span data-ttu-id="0a002-213">Typen hello relaterade attributvärden för hello Azure AD-användarkonto i hello följande textrutor: **primära e-post**, **anställnings-ID**, **lösenord**,  **Bekräfta lösenord**, **Förnamn**, **efternamn**.</span><span class="sxs-lookup"><span data-stu-id="0a002-213">Type hello related attribute values of hello Azure AD user account into hello following textboxes: **Primary Email**, **Employee ID**, **Password**, **Verify Password**, **Given Name**, **Surname**.</span></span>
    
    <span data-ttu-id="0a002-214">c.</span><span class="sxs-lookup"><span data-stu-id="0a002-214">c.</span></span> <span data-ttu-id="0a002-215">Klicka på **Active**.</span><span class="sxs-lookup"><span data-stu-id="0a002-215">Click **Active**.</span></span> 
    
    <span data-ttu-id="0a002-216">d.</span><span class="sxs-lookup"><span data-stu-id="0a002-216">d.</span></span> <span data-ttu-id="0a002-217">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="0a002-217">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="0a002-218">Du kan använda något annat SmarterU användarens konto skapas verktyg eller API: er som tillhandahålls av SmarterU tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="0a002-218">You can use any other SmarterU user account creation tools or APIs provided by SmarterU tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0a002-219">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a002-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0a002-220">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSmarterU.</span><span class="sxs-lookup"><span data-stu-id="0a002-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSmarterU.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0a002-222">**tooassign Britta Simon tooSmarterU utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0a002-222">**tooassign Britta Simon tooSmarterU, perform hello following steps:**</span></span>

1. <span data-ttu-id="0a002-223">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="0a002-223">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0a002-225">Välj i listan med program hello **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="0a002-225">In hello applications list, select **SmarterU**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_app.png) 

3. <span data-ttu-id="0a002-227">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="0a002-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="0a002-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="0a002-229">Click **Add** button.</span></span> <span data-ttu-id="0a002-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a002-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="0a002-232">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="0a002-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0a002-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a002-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0a002-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0a002-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0a002-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0a002-235">Testing single sign-on</span></span>

<span data-ttu-id="0a002-236">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0a002-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>
 
<span data-ttu-id="0a002-237">Du bör få automatiskt inloggade tooyour SmarterU programmet när du klickar på hello SmarterU panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0a002-237">When you click hello SmarterU tile in hello Access Panel, you should get automatically signed-on tooyour SmarterU application.</span></span>
<span data-ttu-id="0a002-238">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0a002-238">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 


## <a name="additional-resources"></a><span data-ttu-id="0a002-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0a002-239">Additional resources</span></span>

* [<span data-ttu-id="0a002-240">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0a002-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0a002-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0a002-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_203.png

