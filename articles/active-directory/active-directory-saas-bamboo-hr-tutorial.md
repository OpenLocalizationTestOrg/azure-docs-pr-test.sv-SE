---
title: "Självstudier: Azure Active Directory-integrering med BambooHR | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och BambooHR."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: jeedes
ms.openlocfilehash: f9083f846beb3a4bf4cebbf18b42aba2dfef2472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a><span data-ttu-id="dce04-103">Självstudier: Azure Active Directory-integrering med BambooHR</span><span class="sxs-lookup"><span data-stu-id="dce04-103">Tutorial: Azure Active Directory integration with BambooHR</span></span>

<span data-ttu-id="dce04-104">I kursen får du lära dig hur toointegrate BambooHR med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="dce04-104">In this tutorial, you learn how toointegrate BambooHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dce04-105">Integrera BambooHR med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="dce04-105">Integrating BambooHR with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="dce04-106">Du kan styra i Azure AD som har åtkomst till tooBambooHR</span><span class="sxs-lookup"><span data-stu-id="dce04-106">You can control in Azure AD who has access tooBambooHR</span></span>
- <span data-ttu-id="dce04-107">Du kan aktivera din användare tooautomatically get inloggade tooBambooHR (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="dce04-107">You can enable your users tooautomatically get signed-on tooBambooHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dce04-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dce04-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="dce04-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dce04-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dce04-110">Krav</span><span class="sxs-lookup"><span data-stu-id="dce04-110">Prerequisites</span></span>

<span data-ttu-id="dce04-111">tooconfigure Azure AD-integrering med BambooHR, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="dce04-111">tooconfigure Azure AD integration with BambooHR, you need hello following items:</span></span>

- <span data-ttu-id="dce04-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dce04-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dce04-113">En BambooHR enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="dce04-113">A BambooHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dce04-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="dce04-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dce04-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="dce04-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dce04-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="dce04-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dce04-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dce04-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dce04-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="dce04-118">Scenario description</span></span>
<span data-ttu-id="dce04-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="dce04-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dce04-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="dce04-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dce04-121">Att lägga till BambooHR från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dce04-121">Adding BambooHR from hello gallery</span></span>
2. <span data-ttu-id="dce04-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dce04-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bamboohr-from-hello-gallery"></a><span data-ttu-id="dce04-123">Att lägga till BambooHR från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dce04-123">Adding BambooHR from hello gallery</span></span>
<span data-ttu-id="dce04-124">tooconfigure hello integrering av BambooHR i Azure AD, behöver du tooadd BambooHR hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="dce04-124">tooconfigure hello integration of BambooHR into Azure AD, you need tooadd BambooHR from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dce04-125">**tooadd BambooHR från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="dce04-125">**tooadd BambooHR from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="dce04-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dce04-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dce04-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="dce04-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="dce04-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="dce04-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="dce04-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dce04-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="dce04-133">Skriv i sökrutan hello **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="dce04-133">In hello search box, type **BambooHR**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_search.png)

5. <span data-ttu-id="dce04-135">Markera hello resultat på panelen **BambooHR**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="dce04-135">In hello results panel, select **BambooHR**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dce04-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dce04-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dce04-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BambooHR baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="dce04-138">In this section, you configure and test Azure AD single sign-on with BambooHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dce04-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i BambooHR är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dce04-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BambooHR is tooa user in Azure AD.</span></span> <span data-ttu-id="dce04-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i BambooHR toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="dce04-140">In other words, a link relationship between an Azure AD user and hello related user in BambooHR needs toobe established.</span></span>

<span data-ttu-id="dce04-141">I BambooHR, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="dce04-141">In BambooHR, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="dce04-142">tooconfigure och testa Azure AD enkel inloggning med BambooHR, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="dce04-142">tooconfigure and test Azure AD single sign-on with BambooHR, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="dce04-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="dce04-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="dce04-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dce04-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dce04-145">**[Skapa en testanvändare BambooHR](#creating-a-bamboohr-test-user)**  -toohave en motsvarighet för Britta Simon i BambooHR som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="dce04-145">**[Creating a BambooHR test user](#creating-a-bamboohr-test-user)** - toohave a counterpart of Britta Simon in BambooHR that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="dce04-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dce04-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dce04-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="dce04-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dce04-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dce04-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dce04-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt BambooHR program.</span><span class="sxs-lookup"><span data-stu-id="dce04-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BambooHR application.</span></span>

<span data-ttu-id="dce04-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med BambooHR:**</span><span class="sxs-lookup"><span data-stu-id="dce04-150">**tooconfigure Azure AD single sign-on with BambooHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="dce04-151">I hello Azure-portalen på hello **BambooHR** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dce04-151">In hello Azure portal, on hello **BambooHR** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="dce04-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dce04-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. <span data-ttu-id="dce04-155">På hello **BambooHR domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dce04-155">On hello **BambooHR Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    <span data-ttu-id="dce04-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.bamboohr.com`</span><span class="sxs-lookup"><span data-stu-id="dce04-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.bamboohr.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="dce04-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="dce04-158">This value is not real.</span></span> <span data-ttu-id="dce04-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="dce04-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="dce04-160">Kontakta [BambooHR klienten supportteamet](https://www.bamboohr.com/contact.php) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="dce04-160">Contact [BambooHR Client support team](https://www.bamboohr.com/contact.php) tooget this value.</span></span> 
 
4. <span data-ttu-id="dce04-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="dce04-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. <span data-ttu-id="dce04-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="dce04-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dce04-165">På hello **BambooHR Configuration** klickar du på **konfigurera BambooHR** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="dce04-165">On hello **BambooHR Configuration** section, click **Configure BambooHR** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="dce04-166">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="dce04-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

6. <span data-ttu-id="dce04-168">Logga in på webbplatsen BambooHR företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="dce04-168">In a different web browser window, log into your BambooHR company site as an administrator.</span></span>

7. <span data-ttu-id="dce04-169">Utför följande hello på hello webbsida:</span><span class="sxs-lookup"><span data-stu-id="dce04-169">On hello homepage, perform hello following steps:</span></span>
   
    <span data-ttu-id="dce04-170">![Enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="dce04-170">![Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Single Sign-On")</span></span>   

    <span data-ttu-id="dce04-171">a.</span><span class="sxs-lookup"><span data-stu-id="dce04-171">a.</span></span> <span data-ttu-id="dce04-172">Klicka på **appar**.</span><span class="sxs-lookup"><span data-stu-id="dce04-172">Click **Apps**.</span></span>
   
    <span data-ttu-id="dce04-173">b.</span><span class="sxs-lookup"><span data-stu-id="dce04-173">b.</span></span> <span data-ttu-id="dce04-174">Hello appar menyn hello vänster, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dce04-174">In hello apps menu on hello left, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="dce04-175">c.</span><span class="sxs-lookup"><span data-stu-id="dce04-175">c.</span></span> <span data-ttu-id="dce04-176">Klicka på **SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dce04-176">Click **SAML Single Sign-On**.</span></span>

8. <span data-ttu-id="dce04-177">I hello **SAML enkel inloggning** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dce04-177">In hello **SAML Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="dce04-178">![SAML enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="dce04-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML Single Sign-On")</span></span>
   
    <span data-ttu-id="dce04-179">a.</span><span class="sxs-lookup"><span data-stu-id="dce04-179">a.</span></span> <span data-ttu-id="dce04-180">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** värdet till hello **inloggnings-Url för SSO** textruta.</span><span class="sxs-lookup"><span data-stu-id="dce04-180">Paste hello **SAML Single Sign-On Service URL** value into hello **SSO Login Url** textbox.</span></span>
      
    <span data-ttu-id="dce04-181">b.</span><span class="sxs-lookup"><span data-stu-id="dce04-181">b.</span></span> <span data-ttu-id="dce04-182">Öppna Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **X.509-certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="dce04-182">Open base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>
   
    <span data-ttu-id="dce04-183">c.</span><span class="sxs-lookup"><span data-stu-id="dce04-183">c.</span></span> <span data-ttu-id="dce04-184">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dce04-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="dce04-185">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="dce04-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="dce04-186">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="dce04-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="dce04-187">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dce04-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dce04-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="dce04-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="dce04-189">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dce04-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="dce04-191">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="dce04-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="dce04-192">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dce04-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dce04-194">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="dce04-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dce04-196">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dce04-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dce04-198">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dce04-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dce04-200">a.</span><span class="sxs-lookup"><span data-stu-id="dce04-200">a.</span></span> <span data-ttu-id="dce04-201">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dce04-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dce04-202">b.</span><span class="sxs-lookup"><span data-stu-id="dce04-202">b.</span></span> <span data-ttu-id="dce04-203">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dce04-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dce04-204">c.</span><span class="sxs-lookup"><span data-stu-id="dce04-204">c.</span></span> <span data-ttu-id="dce04-205">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="dce04-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="dce04-206">d.</span><span class="sxs-lookup"><span data-stu-id="dce04-206">d.</span></span> <span data-ttu-id="dce04-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dce04-207">Click **Create**.</span></span>
 
### <a name="creating-a-bamboohr-test-user"></a><span data-ttu-id="dce04-208">Skapa en testanvändare BambooHR</span><span class="sxs-lookup"><span data-stu-id="dce04-208">Creating a BambooHR test user</span></span>

<span data-ttu-id="dce04-209">tooenable Azure AD-användare toolog i tooBambooHR, måste de etableras i BambooHR.</span><span class="sxs-lookup"><span data-stu-id="dce04-209">tooenable Azure AD users toolog in tooBambooHR, they must be provisioned into BambooHR.</span></span>  

<span data-ttu-id="dce04-210">Hello gäller BambooHR är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="dce04-210">In hello case of BambooHR, provisioning is a manual task.</span></span>

<span data-ttu-id="dce04-211">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="dce04-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="dce04-212">Logga in tooyour **BambooHR** plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="dce04-212">Log in tooyour **BambooHR** site as administrator.</span></span>

2. <span data-ttu-id="dce04-213">Klicka i hello verktygsfältet hello längst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="dce04-213">In hello toolbar on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="dce04-214">![Ange](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "inställning")</span><span class="sxs-lookup"><span data-stu-id="dce04-214">![Setting](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "Setting")</span></span>

3. <span data-ttu-id="dce04-215">Klicka på **översikt**.</span><span class="sxs-lookup"><span data-stu-id="dce04-215">Click **Overview**.</span></span>

4. <span data-ttu-id="dce04-216">Hello vänstra navigeringsfönstret, gå för**säkerhet \> användare**.</span><span class="sxs-lookup"><span data-stu-id="dce04-216">In hello left navigation pane, go too**Security \> Users**.</span></span>

5. <span data-ttu-id="dce04-217">Skriv hello användarnamn, lösenord och e-postadressen till en giltig AAD-konto som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="dce04-217">Type hello user name, password, and email address of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

6. <span data-ttu-id="dce04-218">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dce04-218">Click **Save**.</span></span>
        
>[!NOTE]
><span data-ttu-id="dce04-219">Du kan använda något annat BambooHR användarens konto skapas verktyg eller API: er som tillhandahålls av BambooHR tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="dce04-219">You can use any other BambooHR user account creation tools or APIs provided by BambooHR tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="dce04-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="dce04-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="dce04-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBambooHR.</span><span class="sxs-lookup"><span data-stu-id="dce04-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBambooHR.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="dce04-223">**tooassign Britta Simon tooBambooHR utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="dce04-223">**tooassign Britta Simon tooBambooHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="dce04-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dce04-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="dce04-226">Välj i listan med program hello **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="dce04-226">In hello applications list, select **BambooHR**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png) 

3. <span data-ttu-id="dce04-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dce04-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="dce04-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="dce04-230">Click **Add** button.</span></span> <span data-ttu-id="dce04-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dce04-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="dce04-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="dce04-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="dce04-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dce04-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dce04-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dce04-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dce04-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dce04-236">Testing single sign-on</span></span>

<span data-ttu-id="dce04-237">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="dce04-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="dce04-238">Du bör få automatiskt inloggade tooyour BambooHR programmet när du klickar på hello BambooHR panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="dce04-238">When you click hello BambooHR tile in hello Access Panel, you should get automatically signed-on tooyour BambooHR application.</span></span>
<span data-ttu-id="dce04-239">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dce04-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="dce04-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="dce04-240">Additional resources</span></span>

* [<span data-ttu-id="dce04-241">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dce04-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dce04-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dce04-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

