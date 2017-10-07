---
title: "Självstudier: Azure Active Directory-integrering med Panorama9 | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Panorama9."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 548fb6434d920e076db98a0193f8dfdf8a958a91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a><span data-ttu-id="e9417-103">Självstudier: Azure Active Directory-integrering med Panorama9</span><span class="sxs-lookup"><span data-stu-id="e9417-103">Tutorial: Azure Active Directory integration with Panorama9</span></span>

<span data-ttu-id="e9417-104">I kursen får du lära dig hur toointegrate Panorama9 med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e9417-104">In this tutorial, you learn how toointegrate Panorama9 with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e9417-105">Integrera Panorama9 med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e9417-105">Integrating Panorama9 with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="e9417-106">Du kan styra i Azure AD som har åtkomst till tooPanorama9</span><span class="sxs-lookup"><span data-stu-id="e9417-106">You can control in Azure AD who has access tooPanorama9</span></span>
- <span data-ttu-id="e9417-107">Du kan aktivera din användare tooautomatically get inloggade tooPanorama9 (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e9417-107">You can enable your users tooautomatically get signed-on tooPanorama9 (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e9417-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e9417-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="e9417-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e9417-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e9417-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e9417-110">Prerequisites</span></span>

<span data-ttu-id="e9417-111">tooconfigure Azure AD-integrering med Panorama9, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e9417-111">tooconfigure Azure AD integration with Panorama9, you need hello following items:</span></span>

- <span data-ttu-id="e9417-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e9417-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e9417-113">En Panorama9 enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e9417-113">A Panorama9 single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e9417-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e9417-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e9417-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e9417-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e9417-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e9417-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e9417-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e9417-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e9417-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e9417-118">Scenario description</span></span>
<span data-ttu-id="e9417-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e9417-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e9417-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e9417-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e9417-121">Att lägga till Panorama9 från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e9417-121">Adding Panorama9 from hello gallery</span></span>
2. <span data-ttu-id="e9417-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9417-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panorama9-from-hello-gallery"></a><span data-ttu-id="e9417-123">Att lägga till Panorama9 från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e9417-123">Adding Panorama9 from hello gallery</span></span>
<span data-ttu-id="e9417-124">tooconfigure hello integrering av Panorama9 i Azure AD, behöver du tooadd Panorama9 hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e9417-124">tooconfigure hello integration of Panorama9 into Azure AD, you need tooadd Panorama9 from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e9417-125">**tooadd Panorama9 från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e9417-125">**tooadd Panorama9 from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9417-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e9417-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e9417-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e9417-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="e9417-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="e9417-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e9417-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9417-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e9417-133">Skriv i sökrutan hello **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="e9417-133">In hello search box, type **Panorama9**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_search.png)

5. <span data-ttu-id="e9417-135">Markera hello resultat på panelen **Panorama9**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e9417-135">In hello results panel, select **Panorama9**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e9417-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9417-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="e9417-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Panorama9 baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e9417-138">In this section, you configure and test Azure AD single sign-on with Panorama9 based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="e9417-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Panorama9 är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e9417-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Panorama9 is tooa user in Azure AD.</span></span> <span data-ttu-id="e9417-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Panorama9 toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e9417-140">In other words, a link relationship between an Azure AD user and hello related user in Panorama9 needs toobe established.</span></span>

<span data-ttu-id="e9417-141">I Panorama9, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e9417-141">In Panorama9, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="e9417-142">tooconfigure och testa Azure AD enkel inloggning med Panorama9, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e9417-142">tooconfigure and test Azure AD single sign-on with Panorama9, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e9417-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e9417-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e9417-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9417-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e9417-145">**[Skapa en testanvändare Panorama9](#creating-a-panorama9-test-user)**  -toohave en motsvarighet för Britta Simon i Panorama9 som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e9417-145">**[Creating a Panorama9 test user](#creating-a-panorama9-test-user)** - toohave a counterpart of Britta Simon in Panorama9 that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="e9417-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e9417-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e9417-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e9417-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e9417-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9417-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e9417-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Panorama9 program.</span><span class="sxs-lookup"><span data-stu-id="e9417-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Panorama9 application.</span></span>

<span data-ttu-id="e9417-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Panorama9:**</span><span class="sxs-lookup"><span data-stu-id="e9417-150">**tooconfigure Azure AD single sign-on with Panorama9, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9417-151">I hello Azure-portalen på hello **Panorama9** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e9417-151">In hello Azure portal, on hello **Panorama9** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e9417-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e9417-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_samlbase.png)

3. <span data-ttu-id="e9417-155">På hello **Panorama9 domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9417-155">On hello **Panorama9 Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_url.png)

    <span data-ttu-id="e9417-157">a.</span><span class="sxs-lookup"><span data-stu-id="e9417-157">a.</span></span> <span data-ttu-id="e9417-158">I hello **inloggnings-URL** textruta Skriv en URL som:`https://dashboard.panorama9.com/saml/access/3262`</span><span class="sxs-lookup"><span data-stu-id="e9417-158">In hello **Sign-on URL** textbox, type a URL as: `https://dashboard.panorama9.com/saml/access/3262`</span></span>

    <span data-ttu-id="e9417-159">b.</span><span class="sxs-lookup"><span data-stu-id="e9417-159">b.</span></span> <span data-ttu-id="e9417-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`http://www.panorama9.com/saml20/<tenant-name>`</span><span class="sxs-lookup"><span data-stu-id="e9417-160">In hello **Identifier** textbox, type a URL using hello following pattern: `http://www.panorama9.com/saml20/<tenant-name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e9417-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e9417-161">These values are not real.</span></span> <span data-ttu-id="e9417-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="e9417-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e9417-163">Kontakta [Panorama9 klienten supportteamet](https://support.panorama9.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e9417-163">Contact [Panorama9 Client support team](https://support.panorama9.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="e9417-164">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="e9417-164">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_certificate.png) 

5. <span data-ttu-id="e9417-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e9417-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e9417-168">På hello **Panorama9 Configuration** klickar du på **konfigurera Panorama9** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e9417-168">On hello **Panorama9 Configuration** section, click **Configure Panorama9** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="e9417-169">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e9417-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_configure.png) 

5. <span data-ttu-id="e9417-171">Logga in på webbplatsen Panorama9 företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="e9417-171">In a different web browser window, log into your Panorama9 company site as an administrator.</span></span>

6. <span data-ttu-id="e9417-172">Klicka i hello verktygsfältet hello längst upp **hantera**, och klicka sedan på **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="e9417-172">In hello toolbar on hello top, click **Manage**, and then click **Extensions**.</span></span>
   
   <span data-ttu-id="e9417-173">![Tillägg](./media/active-directory-saas-panorama9-tutorial/ic790023.png "tillägg")</span><span class="sxs-lookup"><span data-stu-id="e9417-173">![Extensions](./media/active-directory-saas-panorama9-tutorial/ic790023.png "Extensions")</span></span>
7. <span data-ttu-id="e9417-174">På hello **tillägg** dialogrutan klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e9417-174">On hello **Extensions** dialog, click **Single Sign-On**.</span></span>
   
   <span data-ttu-id="e9417-175">![Enkel inloggning](./media/active-directory-saas-panorama9-tutorial/ic790024.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="e9417-175">![Single Sign-On](./media/active-directory-saas-panorama9-tutorial/ic790024.png "Single Sign-On")</span></span>
8. <span data-ttu-id="e9417-176">I hello **inställningar** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9417-176">In hello **Settings** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="e9417-177">![Inställningar för](./media/active-directory-saas-panorama9-tutorial/ic790025.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="e9417-177">![Settings](./media/active-directory-saas-panorama9-tutorial/ic790025.png "Settings")</span></span>
   
    <span data-ttu-id="e9417-178">a.</span><span class="sxs-lookup"><span data-stu-id="e9417-178">a.</span></span> <span data-ttu-id="e9417-179">I **identitet providern URL** textruta klistra in hello värdet för **inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e9417-179">In **Identity provider URL** textbox, paste hello value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="e9417-180">b.</span><span class="sxs-lookup"><span data-stu-id="e9417-180">b.</span></span> <span data-ttu-id="e9417-181">I **certifikat fingeravtryck** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e9417-181">In **Certificate fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>    
         
9. <span data-ttu-id="e9417-182">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e9417-182">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e9417-183">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="e9417-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="e9417-184">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="e9417-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="e9417-185">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e9417-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e9417-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e9417-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="e9417-187">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e9417-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e9417-189">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e9417-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9417-190">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e9417-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e9417-192">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e9417-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e9417-194">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9417-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e9417-196">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e9417-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panorama9-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e9417-198">a.</span><span class="sxs-lookup"><span data-stu-id="e9417-198">a.</span></span> <span data-ttu-id="e9417-199">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e9417-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e9417-200">b.</span><span class="sxs-lookup"><span data-stu-id="e9417-200">b.</span></span> <span data-ttu-id="e9417-201">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e9417-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e9417-202">c.</span><span class="sxs-lookup"><span data-stu-id="e9417-202">c.</span></span> <span data-ttu-id="e9417-203">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e9417-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="e9417-204">d.</span><span class="sxs-lookup"><span data-stu-id="e9417-204">d.</span></span> <span data-ttu-id="e9417-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e9417-205">Click **Create**.</span></span>
 
### <a name="creating-a-panorama9-test-user"></a><span data-ttu-id="e9417-206">Skapa en testanvändare Panorama9</span><span class="sxs-lookup"><span data-stu-id="e9417-206">Creating a Panorama9 test user</span></span>

<span data-ttu-id="e9417-207">I ordning tooenable Azure AD-användare toolog i Panorama9, måste de etableras i Panorama9.</span><span class="sxs-lookup"><span data-stu-id="e9417-207">In order tooenable Azure AD users toolog into Panorama9, they must be provisioned into Panorama9.</span></span>  

<span data-ttu-id="e9417-208">Hello gäller Panorama9 är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e9417-208">In hello case of Panorama9, provisioning is a manual task.</span></span>

<span data-ttu-id="e9417-209">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="e9417-209">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9417-210">Logga in tooyour **Panorama9** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="e9417-210">Log in tooyour **Panorama9** company site as an administrator.</span></span>

2. <span data-ttu-id="e9417-211">Hello-menyn överst hello **hantera**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="e9417-211">In hello menu on hello top, click **Manage**, and then click **Users**.</span></span>
   
  <span data-ttu-id="e9417-212">![Användare](./media/active-directory-saas-panorama9-tutorial/ic790027.png "användare")</span><span class="sxs-lookup"><span data-stu-id="e9417-212">![Users](./media/active-directory-saas-panorama9-tutorial/ic790027.png "Users")</span></span>

3. <span data-ttu-id="e9417-213">I hello avsnittet användare, klickar du på  **+**  tooadd ny användare.</span><span class="sxs-lookup"><span data-stu-id="e9417-213">In hello Users section, Click **+** tooadd new user.</span></span>

 <span data-ttu-id="e9417-214">![Användare](./media/active-directory-saas-panorama9-tutorial/ic790028.png "användare")</span><span class="sxs-lookup"><span data-stu-id="e9417-214">![Users](./media/active-directory-saas-panorama9-tutorial/ic790028.png "Users")</span></span>

4. <span data-ttu-id="e9417-215">Gå toohello användaren dataavsnittet typen hello e-postadressen till en giltig Azure Active Directory-användare som du vill tooprovision till hello **e-post** textruta.</span><span class="sxs-lookup"><span data-stu-id="e9417-215">Go toohello User data section, type hello email address of a valid Azure Active Directory user you want tooprovision into hello **Email** textbox.</span></span>

5. <span data-ttu-id="e9417-216">Komma toohello avsnittet för användare, klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="e9417-216">Come toohello Users section, Click **Save**.</span></span>
   
> [!NOTE]
    > <span data-ttu-id="e9417-217">hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="e9417-217">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="e9417-218">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e9417-218">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="e9417-219">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPanorama9.</span><span class="sxs-lookup"><span data-stu-id="e9417-219">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPanorama9.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e9417-221">**tooassign Britta Simon tooPanorama9 utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e9417-221">**tooassign Britta Simon tooPanorama9, perform hello following steps:**</span></span>

1. <span data-ttu-id="e9417-222">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e9417-222">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e9417-224">Välj i listan med program hello **Panorama9**.</span><span class="sxs-lookup"><span data-stu-id="e9417-224">In hello applications list, select **Panorama9**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panorama9-tutorial/tutorial_panorama9_app.png) 

3. <span data-ttu-id="e9417-226">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e9417-226">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e9417-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e9417-228">Click **Add** button.</span></span> <span data-ttu-id="e9417-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9417-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e9417-231">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="e9417-231">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="e9417-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9417-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e9417-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e9417-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e9417-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e9417-234">Testing single sign-on</span></span>

<span data-ttu-id="e9417-235">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9417-235">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="e9417-236">Du bör få automatiskt inloggade tooPanorama9 programmet när du klickar på hello Panorama9 panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9417-236">When you click hello Panorama9 tile in hello Access Panel, you should get automatically signed-on tooPanorama9 application.</span></span>
<span data-ttu-id="e9417-237">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e9417-237">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e9417-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e9417-238">Additional resources</span></span>

* [<span data-ttu-id="e9417-239">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e9417-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e9417-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e9417-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panorama9-tutorial/tutorial_general_203.png

