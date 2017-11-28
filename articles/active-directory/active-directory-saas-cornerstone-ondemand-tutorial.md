---
title: "Självstudier: Azure Active Directory-integrering med hörnstenarna OnDemand | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och hörnstenarna OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 85fd6eb4e93010d8f7477df236403e205e9f2d83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a><span data-ttu-id="8aa25-103">Självstudier: Azure Active Directory-integrering med hörnstenarna OnDemand</span><span class="sxs-lookup"><span data-stu-id="8aa25-103">Tutorial: Azure Active Directory integration with Cornerstone OnDemand</span></span>

<span data-ttu-id="8aa25-104">I kursen får du lära dig hur toointegrate hörnstenarna OnDemand med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8aa25-104">In this tutorial, you learn how toointegrate Cornerstone OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8aa25-105">Integrera hörnstenarna OnDemand med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8aa25-105">Integrating Cornerstone OnDemand with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8aa25-106">Du kan styra i Azure AD som har åtkomst tooCornerstone OnDemand</span><span class="sxs-lookup"><span data-stu-id="8aa25-106">You can control in Azure AD who has access tooCornerstone OnDemand</span></span>
- <span data-ttu-id="8aa25-107">Du kan aktivera din användare tooautomatically get inloggade tooCornerstone OnDemand (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8aa25-107">You can enable your users tooautomatically get signed-on tooCornerstone OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8aa25-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8aa25-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8aa25-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8aa25-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8aa25-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8aa25-110">Prerequisites</span></span>

<span data-ttu-id="8aa25-111">tooconfigure Azure AD-integrering med hörnstenarna OnDemand måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8aa25-111">tooconfigure Azure AD integration with Cornerstone OnDemand, you need hello following items:</span></span>

- <span data-ttu-id="8aa25-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8aa25-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8aa25-113">En hörnsten OnDemand enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8aa25-113">A Cornerstone OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8aa25-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8aa25-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8aa25-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8aa25-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8aa25-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8aa25-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8aa25-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8aa25-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8aa25-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8aa25-118">Scenario description</span></span>
<span data-ttu-id="8aa25-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8aa25-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8aa25-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8aa25-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8aa25-121">Att lägga till hörnstenarna OnDemand från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8aa25-121">Adding Cornerstone OnDemand from hello gallery</span></span>
2. <span data-ttu-id="8aa25-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8aa25-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cornerstone-ondemand-from-hello-gallery"></a><span data-ttu-id="8aa25-123">Att lägga till hörnstenarna OnDemand från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8aa25-123">Adding Cornerstone OnDemand from hello gallery</span></span>
<span data-ttu-id="8aa25-124">tooconfigure hello integrering av hörnstenarna OnDemand i Azure AD, behöver du tooadd hörnstenarna OnDemand hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="8aa25-124">tooconfigure hello integration of Cornerstone OnDemand into Azure AD, you need tooadd Cornerstone OnDemand from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8aa25-125">**tooadd hörnstenarna OnDemand från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8aa25-125">**tooadd Cornerstone OnDemand from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8aa25-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8aa25-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8aa25-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8aa25-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8aa25-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8aa25-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8aa25-133">Skriv i sökrutan hello **hörnstenarna OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-133">In hello search box, type **Cornerstone OnDemand**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. <span data-ttu-id="8aa25-135">Markera hello resultat på panelen **hörnstenarna OnDemand**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="8aa25-135">In hello results panel, select **Cornerstone OnDemand**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8aa25-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8aa25-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8aa25-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med hörnstenarna OnDemand baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8aa25-138">In this section, you configure and test Azure AD single sign-on with Cornerstone OnDemand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8aa25-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i hörnstenarna OnDemand är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8aa25-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cornerstone OnDemand is tooa user in Azure AD.</span></span> <span data-ttu-id="8aa25-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i hörnstenarna OnDemand toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="8aa25-140">In other words, a link relationship between an Azure AD user and hello related user in Cornerstone OnDemand needs toobe established.</span></span>

<span data-ttu-id="8aa25-141">Tilldela hello värdet för hello i hörnstenarna OnDemand **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8aa25-141">In Cornerstone OnDemand, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8aa25-142">tooconfigure och testa Azure AD enkel inloggning med hörnstenarna OnDemand, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8aa25-142">tooconfigure and test Azure AD single sign-on with Cornerstone OnDemand, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8aa25-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8aa25-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8aa25-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8aa25-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8aa25-145">**[Skapa en testanvändare hörnstenarna OnDemand](#creating-a-cornerstone-ondemand-test-user)**  -toohave en motsvarighet för Britta Simon i hörnstenarna OnDemand som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8aa25-145">**[Creating a Cornerstone OnDemand test user](#creating-a-cornerstone-ondemand-test-user)** - toohave a counterpart of Britta Simon in Cornerstone OnDemand that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8aa25-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8aa25-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8aa25-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8aa25-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8aa25-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8aa25-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8aa25-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt hörnstenarna OnDemand-program.</span><span class="sxs-lookup"><span data-stu-id="8aa25-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cornerstone OnDemand application.</span></span>

<span data-ttu-id="8aa25-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med hörnstenarna OnDemand:**</span><span class="sxs-lookup"><span data-stu-id="8aa25-150">**tooconfigure Azure AD single sign-on with Cornerstone OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="8aa25-151">I hello Azure-portalen på hello **hörnstenarna OnDemand** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-151">In hello Azure portal, on hello **Cornerstone OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8aa25-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8aa25-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. <span data-ttu-id="8aa25-155">På hello **hörnstenarna OnDemand domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8aa25-155">On hello **Cornerstone OnDemand Domain and URLs** section, perform hello following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    <span data-ttu-id="8aa25-157">a.</span><span class="sxs-lookup"><span data-stu-id="8aa25-157">a.</span></span> <span data-ttu-id="8aa25-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="8aa25-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.csod.com`</span></span>

    <span data-ttu-id="8aa25-159">b.</span><span class="sxs-lookup"><span data-stu-id="8aa25-159">b.</span></span> <span data-ttu-id="8aa25-160">I **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="8aa25-160">In **Identifier** textbox, type a URL using hello following pattern: `https://<company>.csod.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8aa25-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="8aa25-161">These values are not real.</span></span> <span data-ttu-id="8aa25-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="8aa25-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8aa25-163">Kontakta [hörnstenarna OnDemand klienten supportteamet](mailTo:moreinfo@csod.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8aa25-163">Contact [Cornerstone OnDemand Client support team](mailTo:moreinfo@csod.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="8aa25-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="8aa25-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. <span data-ttu-id="8aa25-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8aa25-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8aa25-168">På hello **hörnstenarna OnDemand Configuration** klickar du på **konfigurera hörnstenarna OnDemand** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="8aa25-168">On hello **Cornerstone OnDemand Configuration** section, click **Configure Cornerstone OnDemand** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8aa25-169">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="8aa25-169">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. <span data-ttu-id="8aa25-171">tooconfigure enkel inloggning på **hörnstenarna OnDemand** sida, behöver du toosend hello hämtas **certifikat**, **Sign-Out URL** och **SAML enda URL för inloggning** för[hörnstenarna OnDemand supportteamet](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="8aa25-171">tooconfigure single sign-on on **Cornerstone OnDemand** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL** and **SAML Single Sign-On Service URL**  too[Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span> <span data-ttu-id="8aa25-172">De kan ange den här inställningen toohave hello SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="8aa25-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="8aa25-173">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="8aa25-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8aa25-174">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="8aa25-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8aa25-175">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8aa25-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8aa25-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8aa25-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="8aa25-177">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8aa25-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8aa25-179">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8aa25-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8aa25-180">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8aa25-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8aa25-182">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8aa25-184">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8aa25-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8aa25-186">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8aa25-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8aa25-188">a.</span><span class="sxs-lookup"><span data-stu-id="8aa25-188">a.</span></span> <span data-ttu-id="8aa25-189">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8aa25-190">b.</span><span class="sxs-lookup"><span data-stu-id="8aa25-190">b.</span></span> <span data-ttu-id="8aa25-191">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8aa25-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8aa25-192">c.</span><span class="sxs-lookup"><span data-stu-id="8aa25-192">c.</span></span> <span data-ttu-id="8aa25-193">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8aa25-194">d.</span><span class="sxs-lookup"><span data-stu-id="8aa25-194">d.</span></span> <span data-ttu-id="8aa25-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-195">Click **Create**.</span></span>
 
### <a name="creating-a-cornerstone-ondemand-test-user"></a><span data-ttu-id="8aa25-196">Skapa en OnDemand hörnstenarna testanvändare</span><span class="sxs-lookup"><span data-stu-id="8aa25-196">Creating a Cornerstone OnDemand test user</span></span>

<span data-ttu-id="8aa25-197">I ordning tooenable Azure AD-användare toolog till hörnstenarna OnDemand, måste de etableras i hörnstenarna OnDemand.</span><span class="sxs-lookup"><span data-stu-id="8aa25-197">In order tooenable Azure AD users toolog into Cornerstone OnDemand, they must be provisioned into Cornerstone OnDemand.</span></span> <span data-ttu-id="8aa25-198">Hello gäller hörnstenarna OnDemand är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="8aa25-198">In hello case of Cornerstone OnDemand, provisioning is a manual task.</span></span>

<span data-ttu-id="8aa25-199">användaretablering för tooconfigure, skicka hello information (t.ex.: namn, e-post) om hello Azure AD-användare som du vill tooprovision toohello [hörnstenarna OnDemand supportteamet](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="8aa25-199">tooconfigure user provisioning, send hello information (e.g.: Name, Email) about hello Azure AD user you want tooprovision toohello [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span>

>[!NOTE]
><span data-ttu-id="8aa25-200">Du kan använda något annat hörnstenarna OnDemand användarens konto skapas verktyg eller API: er som tillhandahålls av hörnstenarna OnDemand tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="8aa25-200">You can use any other Cornerstone OnDemand user account creation tools or APIs provided by Cornerstone OnDemand tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8aa25-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="8aa25-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8aa25-202">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCornerstone OnDemand.</span><span class="sxs-lookup"><span data-stu-id="8aa25-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCornerstone OnDemand.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8aa25-204">**tooassign Britta Simon tooCornerstone OnDemand, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="8aa25-204">**tooassign Britta Simon tooCornerstone OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="8aa25-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8aa25-207">Välj i listan med program hello **hörnstenarna OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-207">In hello applications list, select **Cornerstone OnDemand**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. <span data-ttu-id="8aa25-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8aa25-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8aa25-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8aa25-211">Click **Add** button.</span></span> <span data-ttu-id="8aa25-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8aa25-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8aa25-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="8aa25-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8aa25-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8aa25-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8aa25-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8aa25-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8aa25-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8aa25-217">Testing single sign-on</span></span>

<span data-ttu-id="8aa25-218">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8aa25-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8aa25-219">Du bör få automatiskt inloggade tooyour hörnstenarna OnDemand programmet när du klickar på hello hörnstenarna OnDemand-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8aa25-219">When you click hello Cornerstone OnDemand tile in hello Access Panel, you should get automatically signed-on tooyour Cornerstone OnDemand application.</span></span>
<span data-ttu-id="8aa25-220">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8aa25-220">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8aa25-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8aa25-221">Additional resources</span></span>

* [<span data-ttu-id="8aa25-222">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8aa25-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8aa25-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8aa25-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_203.png

