---
title: "Självstudier: Azure Active Directory-integrering med Veritas Enterprise Vault.cloud SSO | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Veritas Enterprise Vault.cloud enkel inloggning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c47894b1-f5df-4755-845d-f12f4c602dc4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 1037e70515686091460ac41e9e5a7951639bb520
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a><span data-ttu-id="a3926-103">Självstudier: Azure Active Directory-integrering med Veritas Enterprise Vault.cloud enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3926-103">Tutorial: Azure Active Directory integration with Veritas Enterprise Vault.cloud SSO</span></span>

<span data-ttu-id="a3926-104">I kursen får du lära dig hur toointegrate Veritas Enterprise Vault.cloud enkel inloggning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a3926-104">In this tutorial, you learn how toointegrate Veritas Enterprise Vault.cloud SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a3926-105">Integrera Veritas Enterprise Vault.cloud enkel inloggning med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a3926-105">Integrating Veritas Enterprise Vault.cloud SSO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a3926-106">Du kan styra i Azure AD som har åtkomst tooVeritas Enterprise Vault.cloud SSO</span><span class="sxs-lookup"><span data-stu-id="a3926-106">You can control in Azure AD who has access tooVeritas Enterprise Vault.cloud SSO</span></span>
- <span data-ttu-id="a3926-107">Du kan aktivera din användare tooautomatically get inloggade tooVeritas Enterprise Vault.cloud SSO (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a3926-107">You can enable your users tooautomatically get signed-on tooVeritas Enterprise Vault.cloud SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a3926-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a3926-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a3926-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a3926-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a3926-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a3926-110">Prerequisites</span></span>

<span data-ttu-id="a3926-111">tooconfigure Azure AD-integrering med Veritas Enterprise Vault.cloud enkel inloggning måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a3926-111">tooconfigure Azure AD integration with Veritas Enterprise Vault.cloud SSO, you need hello following items:</span></span>

- <span data-ttu-id="a3926-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a3926-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a3926-113">En Veritas Enterprise Vault.cloud SSO enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a3926-113">A Veritas Enterprise Vault.cloud SSO single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a3926-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a3926-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a3926-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a3926-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a3926-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a3926-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a3926-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a3926-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a3926-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a3926-118">Scenario description</span></span>
<span data-ttu-id="a3926-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a3926-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a3926-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a3926-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a3926-121">Att lägga till Veritas Enterprise Vault.cloud SSO från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a3926-121">Adding Veritas Enterprise Vault.cloud SSO from hello gallery</span></span>
2. <span data-ttu-id="a3926-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3926-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-hello-gallery"></a><span data-ttu-id="a3926-123">Att lägga till Veritas Enterprise Vault.cloud SSO från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a3926-123">Adding Veritas Enterprise Vault.cloud SSO from hello gallery</span></span>
<span data-ttu-id="a3926-124">tooconfigure hello integrering av Veritas Enterprise Vault.cloud SSO i Azure AD, behöver du tooadd Veritas Enterprise Vault.cloud SSO hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a3926-124">tooconfigure hello integration of Veritas Enterprise Vault.cloud SSO into Azure AD, you need tooadd Veritas Enterprise Vault.cloud SSO from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a3926-125">**tooadd Veritas Enterprise Vault.cloud SSO från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a3926-125">**tooadd Veritas Enterprise Vault.cloud SSO from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3926-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a3926-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a3926-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a3926-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a3926-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="a3926-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a3926-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3926-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a3926-133">Skriv i sökrutan hello **Veritas Enterprise Vault.cloud SSO**.</span><span class="sxs-lookup"><span data-stu-id="a3926-133">In hello search box, type **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_search.png)

5. <span data-ttu-id="a3926-135">Markera hello resultat på panelen **Veritas Enterprise Vault.cloud SSO**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a3926-135">In hello results panel, select **Veritas Enterprise Vault.cloud SSO**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a3926-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3926-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a3926-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Veritas Enterprise Vault.cloud SSO baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a3926-138">In this section, you configure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a3926-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren Veritas Enterprise Vault.cloud SSO är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a3926-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Veritas Enterprise Vault.cloud SSO is tooa user in Azure AD.</span></span> <span data-ttu-id="a3926-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren Veritas Enterprise Vault.cloud SSO toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a3926-140">In other words, a link relationship between an Azure AD user and hello related user in Veritas Enterprise Vault.cloud SSO needs toobe established.</span></span>

<span data-ttu-id="a3926-141">Tilldela hello värdet hello Veritas Enterprise Vault.cloud SSO **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a3926-141">In Veritas Enterprise Vault.cloud SSO, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a3926-142">tooconfigure och testa Azure AD enkel inloggning med Veritas Enterprise Vault.cloud enkel inloggning, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a3926-142">tooconfigure and test Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a3926-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a3926-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a3926-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a3926-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a3926-145">**[Skapa en testanvändare Veritas Enterprise Vault.cloud SSO](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)**  -toohave en motsvarighet för Britta Simon Veritas Enterprise Vault.cloud SSO som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a3926-145">**[Creating a Veritas Enterprise Vault.cloud SSO test user](#creating-a-veritas-enterprise-vaultcloud-sso-test-user)** - toohave a counterpart of Britta Simon in Veritas Enterprise Vault.cloud SSO that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a3926-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3926-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a3926-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a3926-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a3926-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3926-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a3926-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Veritas Enterprise Vault.cloud SSO-program.</span><span class="sxs-lookup"><span data-stu-id="a3926-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Veritas Enterprise Vault.cloud SSO application.</span></span>

<span data-ttu-id="a3926-150">**tooconfigure Azure AD enkel inloggning med Veritas Enterprise Vault.cloud enkel inloggning, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="a3926-150">**tooconfigure Azure AD single sign-on with Veritas Enterprise Vault.cloud SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3926-151">I hello Azure-portalen på hello **Veritas Enterprise Vault.cloud SSO** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a3926-151">In hello Azure portal, on hello **Veritas Enterprise Vault.cloud SSO** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a3926-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3926-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_samlbase.png)

3. <span data-ttu-id="a3926-155">På hello **Veritas företagsdomänen Vault.cloud SSO och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a3926-155">On hello **Veritas Enterprise Vault.cloud SSO Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_url.png)

    <span data-ttu-id="a3926-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span><span class="sxs-lookup"><span data-stu-id="a3926-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="a3926-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a3926-158">This value is not real.</span></span> <span data-ttu-id="a3926-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a3926-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="a3926-160">Kontakta [Veritas Enterprise Vault.cloud klientfilerna supportteamet](https://www.veritas.com/support/.html) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="a3926-160">Contact [Veritas Enterprise Vault.cloud SSO Client support team](https://www.veritas.com/support/.html) tooget this value.</span></span> 

4. <span data-ttu-id="a3926-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="a3926-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_certificate.png) 

5. <span data-ttu-id="a3926-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3926-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-veritas-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a3926-165">På hello **Veritas företagskonfiguration Vault.cloud SSO** klickar du på **konfigurera Veritas Enterprise Vault.cloud SSO** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a3926-165">On hello **Veritas Enterprise Vault.cloud SSO Configuration** section, click **Configure Veritas Enterprise Vault.cloud SSO** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a3926-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a3926-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_configure.png) 

7. <span data-ttu-id="a3926-168">tooconfigure enkel inloggning på **Veritas Enterprise Vault.cloud SSO** sida, behöver du toosend hello hämtas **Certificate(Base64)** och **SAML inloggning tjänst-URL för enkel**för[Veritas Enterprise Vault.cloud SSO supportteamet](https://www.veritas.com/support/.html).</span><span class="sxs-lookup"><span data-stu-id="a3926-168">tooconfigure single sign-on on **Veritas Enterprise Vault.cloud SSO** side, you need toosend hello downloaded **Certificate(Base64)** and **SAML Single Sign-On Service URL** too[Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html).</span></span>

> [!TIP]
> <span data-ttu-id="a3926-169">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="a3926-169">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a3926-170">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="a3926-170">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a3926-171">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a3926-171">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a3926-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a3926-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="a3926-173">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a3926-173">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a3926-175">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a3926-175">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3926-176">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a3926-176">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a3926-178">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a3926-178">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a3926-180">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3926-180">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a3926-182">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a3926-182">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-veritas-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a3926-184">a.</span><span class="sxs-lookup"><span data-stu-id="a3926-184">a.</span></span> <span data-ttu-id="a3926-185">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a3926-185">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a3926-186">b.</span><span class="sxs-lookup"><span data-stu-id="a3926-186">b.</span></span> <span data-ttu-id="a3926-187">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a3926-187">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a3926-188">c.</span><span class="sxs-lookup"><span data-stu-id="a3926-188">c.</span></span> <span data-ttu-id="a3926-189">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a3926-189">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a3926-190">d.</span><span class="sxs-lookup"><span data-stu-id="a3926-190">d.</span></span> <span data-ttu-id="a3926-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a3926-191">Click **Create**.</span></span>
 
### <a name="creating-a-veritas-enterprise-vaultcloud-sso-test-user"></a><span data-ttu-id="a3926-192">Skapa en testanvändare Veritas Enterprise Vault.cloud SSO</span><span class="sxs-lookup"><span data-stu-id="a3926-192">Creating a Veritas Enterprise Vault.cloud SSO test user</span></span>

<span data-ttu-id="a3926-193">I det här avsnittet skapar du en användare som kallas Britta Simon Enterprise Vault.cloud sso.</span><span class="sxs-lookup"><span data-stu-id="a3926-193">In this section, you create a user called Britta Simon in Enterprise Vault.cloud SSO.</span></span> <span data-ttu-id="a3926-194">Arbeta med [Veritas Enterprise Vault.cloud SSO supportteamet](https://www.veritas.com/support/.html) att lägga till hello användare i hello Enterprise Vault.cloud SSO-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a3926-194">Work with [Veritas Enterprise Vault.cloud SSO support team](https://www.veritas.com/support/.html) to add hello users in hello Enterprise Vault.cloud SSO platform.</span></span> <span data-ttu-id="a3926-195">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a3926-195">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a3926-196">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a3926-196">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a3926-197">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooVeritas Enterprise Vault.cloud SSO.</span><span class="sxs-lookup"><span data-stu-id="a3926-197">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVeritas Enterprise Vault.cloud SSO.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a3926-199">**tooassign Britta Simon tooVeritas Enterprise Vault.cloud SSO utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a3926-199">**tooassign Britta Simon tooVeritas Enterprise Vault.cloud SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="a3926-200">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a3926-200">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a3926-202">Välj i listan med program hello **Veritas Enterprise Vault.cloud SSO**.</span><span class="sxs-lookup"><span data-stu-id="a3926-202">In hello applications list, select **Veritas Enterprise Vault.cloud SSO**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-veritas-tutorial/tutorial_veritas_app.png) 

3. <span data-ttu-id="a3926-204">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a3926-204">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a3926-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a3926-206">Click **Add** button.</span></span> <span data-ttu-id="a3926-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3926-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a3926-209">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="a3926-209">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a3926-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3926-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a3926-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a3926-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a3926-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a3926-212">Testing single sign-on</span></span>

<span data-ttu-id="a3926-213">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a3926-213">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a3926-214">När du klickar på hello Veritas Enterprise Vault.cloud SSO-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Veritas Enterprise Vault.cloud SSO-program.</span><span class="sxs-lookup"><span data-stu-id="a3926-214">When you click hello Veritas Enterprise Vault.cloud SSO tile in hello Access Panel, you should get automatically signed-on tooyour Veritas Enterprise Vault.cloud SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3926-215">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a3926-215">Additional resources</span></span>

* [<span data-ttu-id="a3926-216">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a3926-216">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a3926-217">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a3926-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veritas-tutorial/tutorial_general_203.png

