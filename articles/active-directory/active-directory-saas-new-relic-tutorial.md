---
title: "Självstudier: Azure Active Directory-integrering med New Relic | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och New Relic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: dc8f0df3b18a32bde155e8911a581fc5f91af217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a><span data-ttu-id="2365c-103">Självstudier: Azure Active Directory-integrering med New Relic</span><span class="sxs-lookup"><span data-stu-id="2365c-103">Tutorial: Azure Active Directory integration with New Relic</span></span>

<span data-ttu-id="2365c-104">I kursen får du lära dig hur toointegrate New Relic med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2365c-104">In this tutorial, you learn how toointegrate New Relic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2365c-105">Integrera New Relic med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2365c-105">Integrating New Relic with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2365c-106">Du kan styra i Azure AD som har åtkomst tooNew Relic</span><span class="sxs-lookup"><span data-stu-id="2365c-106">You can control in Azure AD who has access tooNew Relic</span></span>
- <span data-ttu-id="2365c-107">Du kan aktivera din användare tooautomatically get inloggade tooNew Relic (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2365c-107">You can enable your users tooautomatically get signed-on tooNew Relic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2365c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2365c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2365c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2365c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2365c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2365c-110">Prerequisites</span></span>

<span data-ttu-id="2365c-111">tooconfigure Azure AD-integrering med New Relic måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2365c-111">tooconfigure Azure AD integration with New Relic, you need hello following items:</span></span>

- <span data-ttu-id="2365c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2365c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2365c-113">Ett New Relic enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2365c-113">A New Relic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2365c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2365c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2365c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2365c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2365c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2365c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2365c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2365c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2365c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2365c-118">Scenario description</span></span>
<span data-ttu-id="2365c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2365c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2365c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2365c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2365c-121">Att lägga till New Relic från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2365c-121">Adding New Relic from hello gallery</span></span>
2. <span data-ttu-id="2365c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2365c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-new-relic-from-hello-gallery"></a><span data-ttu-id="2365c-123">Att lägga till New Relic från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2365c-123">Adding New Relic from hello gallery</span></span>
<span data-ttu-id="2365c-124">tooconfigure hello integrering av New Relic i Azure AD, behöver du tooadd New Relic hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2365c-124">tooconfigure hello integration of New Relic into Azure AD, you need tooadd New Relic from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2365c-125">**tooadd New Relic från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2365c-125">**tooadd New Relic from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2365c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2365c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2365c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2365c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2365c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="2365c-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2365c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2365c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2365c-133">Skriv i sökrutan hello **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="2365c-133">In hello search box, type **New Relic**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. <span data-ttu-id="2365c-135">Markera hello resultat på panelen **New Relic**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2365c-135">In hello results panel, select **New Relic**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2365c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2365c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2365c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med New Relic baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2365c-138">In this section, you configure and test Azure AD single sign-on with New Relic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2365c-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i New Relic är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2365c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in New Relic is tooa user in Azure AD.</span></span> <span data-ttu-id="2365c-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i New Relic toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2365c-140">In other words, a link relationship between an Azure AD user and hello related user in New Relic needs toobe established.</span></span>

<span data-ttu-id="2365c-141">Tilldela hello värdet för hello i New Relic **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2365c-141">In New Relic, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2365c-142">tooconfigure och testa Azure AD enkel inloggning med New Relic, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2365c-142">tooconfigure and test Azure AD single sign-on with New Relic, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2365c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2365c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2365c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2365c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2365c-145">**[Skapa en testanvändare New Relic](#creating-a-new-relic-test-user)**  -toohave en motsvarighet för Britta Simon i New Relic som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2365c-145">**[Creating a New Relic test user](#creating-a-new-relic-test-user)** - toohave a counterpart of Britta Simon in New Relic that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2365c-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2365c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2365c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2365c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2365c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2365c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2365c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i New Relic-program.</span><span class="sxs-lookup"><span data-stu-id="2365c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your New Relic application.</span></span>

<span data-ttu-id="2365c-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med New Relic:**</span><span class="sxs-lookup"><span data-stu-id="2365c-150">**tooconfigure Azure AD single sign-on with New Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="2365c-151">I hello Azure-portalen på hello **New Relic** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2365c-151">In hello Azure portal, on hello **New Relic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2365c-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2365c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. <span data-ttu-id="2365c-155">På hello **ny Relic domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2365c-155">On hello **New Relic Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    <span data-ttu-id="2365c-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.newrelic.com`</span><span class="sxs-lookup"><span data-stu-id="2365c-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.newrelic.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2365c-158">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2365c-158">hello value is not real.</span></span> <span data-ttu-id="2365c-159">Hello uppdateringsvärde med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="2365c-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="2365c-160">Kontakta [ny Relic klient supportteamet](https://support.newrelic.com/) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="2365c-160">Contact [New Relic Client support team](https://support.newrelic.com/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="2365c-161">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="2365c-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. <span data-ttu-id="2365c-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2365c-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2365c-165">På hello **nya Relic konfigurationen** klickar du på **konfigurera New Relic** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="2365c-165">On hello **New Relic Configuration** section, click **Configure New Relic** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2365c-166">Kopiera hello **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="2365c-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. <span data-ttu-id="2365c-168">Inloggning i en annan webbläsarfönster tooyour **New Relic** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="2365c-168">In a different web browser window, sign on tooyour **New Relic** company site as administrator.</span></span>

8. <span data-ttu-id="2365c-169">Hello-menyn överst hello **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="2365c-169">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="2365c-170">![Kontoinställningar](./media/active-directory-saas-new-relic-tutorial/ic797036.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="2365c-170">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Account Settings")</span></span>

9. <span data-ttu-id="2365c-171">Klicka på hello **säkerhet och autentisering** fliken och klicka sedan på hello **enkel inloggning** fliken.</span><span class="sxs-lookup"><span data-stu-id="2365c-171">Click hello **Security and authentication** tab, and then click hello **Single sign on** tab.</span></span>
   
    <span data-ttu-id="2365c-172">![Enkel inloggning](./media/active-directory-saas-new-relic-tutorial/ic797037.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="2365c-172">![Single Sign-On](./media/active-directory-saas-new-relic-tutorial/ic797037.png "Single Sign-On")</span></span>

10. <span data-ttu-id="2365c-173">Utför följande hello hello SAML dialogrutan på sidan:</span><span class="sxs-lookup"><span data-stu-id="2365c-173">On hello SAML dialog page, perform hello following steps:</span></span>
   
    <span data-ttu-id="2365c-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="2365c-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span></span>
   
   <span data-ttu-id="2365c-175">a.</span><span class="sxs-lookup"><span data-stu-id="2365c-175">a.</span></span> <span data-ttu-id="2365c-176">Klicka på **Välj fil** tooupload ditt hämtade Azure Active Directory-certifikat.</span><span class="sxs-lookup"><span data-stu-id="2365c-176">Click **Choose File** tooupload your downloaded Azure Active Directory certificate.</span></span>

   <span data-ttu-id="2365c-177">b.</span><span class="sxs-lookup"><span data-stu-id="2365c-177">b.</span></span> <span data-ttu-id="2365c-178">I hello **fjärrinloggning URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2365c-178">In hello **Remote login URL** textbox,  paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="2365c-179">c.</span><span class="sxs-lookup"><span data-stu-id="2365c-179">c.</span></span> <span data-ttu-id="2365c-180">I hello **logga ut hamnar URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2365c-180">In hello **Logout landing URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

   <span data-ttu-id="2365c-181">d.</span><span class="sxs-lookup"><span data-stu-id="2365c-181">d.</span></span> <span data-ttu-id="2365c-182">Klicka på **spara mina ändringar**.</span><span class="sxs-lookup"><span data-stu-id="2365c-182">Click **Save my changes**.</span></span>

> [!TIP]
> <span data-ttu-id="2365c-183">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="2365c-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2365c-184">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2365c-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2365c-185">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2365c-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2365c-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2365c-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="2365c-187">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2365c-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2365c-189">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2365c-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2365c-190">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2365c-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2365c-192">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2365c-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2365c-194">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2365c-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2365c-196">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2365c-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2365c-198">a.</span><span class="sxs-lookup"><span data-stu-id="2365c-198">a.</span></span> <span data-ttu-id="2365c-199">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2365c-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2365c-200">b.</span><span class="sxs-lookup"><span data-stu-id="2365c-200">b.</span></span> <span data-ttu-id="2365c-201">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2365c-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2365c-202">c.</span><span class="sxs-lookup"><span data-stu-id="2365c-202">c.</span></span> <span data-ttu-id="2365c-203">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2365c-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2365c-204">d.</span><span class="sxs-lookup"><span data-stu-id="2365c-204">d.</span></span> <span data-ttu-id="2365c-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2365c-205">Click **Create**.</span></span>
 
### <a name="creating-a-new-relic-test-user"></a><span data-ttu-id="2365c-206">Skapa en New Relic testanvändare</span><span class="sxs-lookup"><span data-stu-id="2365c-206">Creating a New Relic test user</span></span>

<span data-ttu-id="2365c-207">I ordning tooenable Azure Active Directory användare toolog i tooNew Relic, måste de etableras till New Relic.</span><span class="sxs-lookup"><span data-stu-id="2365c-207">In order tooenable Azure Active Directory users toolog in tooNew Relic, they must be provisioned into New Relic.</span></span> <span data-ttu-id="2365c-208">New Relic hello gäller är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2365c-208">In hello case of New Relic, provisioning is a manual task.</span></span>

<span data-ttu-id="2365c-209">**tooprovision en användare konto tooNew Relic, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2365c-209">**tooprovision a user account tooNew Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="2365c-210">Logga in tooyour **New Relic** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="2365c-210">Log in tooyour **New Relic** company site as administrator.</span></span>

2. <span data-ttu-id="2365c-211">Hello-menyn överst hello **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="2365c-211">In hello menu on hello top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="2365c-212">![Kontoinställningar](./media/active-directory-saas-new-relic-tutorial/ic797040.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="2365c-212">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Account Settings")</span></span>

3. <span data-ttu-id="2365c-213">I hello **konto** rutan på hello vänster sida klickar du på **sammanfattning**, och klicka sedan på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="2365c-213">In hello **Account** pane on hello left side, click **Summary**, and then click **Add user**.</span></span>
   
    <span data-ttu-id="2365c-214">![Kontoinställningar](./media/active-directory-saas-new-relic-tutorial/ic797041.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="2365c-214">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Account Settings")</span></span>

4. <span data-ttu-id="2365c-215">På hello **aktiva användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2365c-215">On hello **Active users** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="2365c-216">![Aktiva användare](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktiva användare")</span><span class="sxs-lookup"><span data-stu-id="2365c-216">![Active Users](./media/active-directory-saas-new-relic-tutorial/ic797042.png "Active Users")</span></span>
   
    <span data-ttu-id="2365c-217">a.</span><span class="sxs-lookup"><span data-stu-id="2365c-217">a.</span></span> <span data-ttu-id="2365c-218">I hello **e-post** textruta typen hello e-postadressen till en giltig Azure Active Directory-användare som du vill tooprovision.</span><span class="sxs-lookup"><span data-stu-id="2365c-218">In hello **Email** textbox, type hello email address of a valid Azure Active Directory user you want tooprovision.</span></span>

    <span data-ttu-id="2365c-219">b.</span><span class="sxs-lookup"><span data-stu-id="2365c-219">b.</span></span> <span data-ttu-id="2365c-220">Som **rollen** Välj **användaren**.</span><span class="sxs-lookup"><span data-stu-id="2365c-220">As **Role** select **User**.</span></span>

    <span data-ttu-id="2365c-221">c.</span><span class="sxs-lookup"><span data-stu-id="2365c-221">c.</span></span> <span data-ttu-id="2365c-222">Klicka på **Lägg till användaren**.</span><span class="sxs-lookup"><span data-stu-id="2365c-222">Click **Add this user**.</span></span>

>[!NOTE]
><span data-ttu-id="2365c-223">Du kan använda något annat New Relic användarens konto skapas verktyg eller API: er som tillhandahålls av New Relic tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="2365c-223">You can use any other New Relic user account creation tools or APIs provided by New Relic tooprovision AAD user accounts.</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2365c-224">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2365c-224">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2365c-225">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooNew Relic.</span><span class="sxs-lookup"><span data-stu-id="2365c-225">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNew Relic.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2365c-227">**tooassign Britta Simon tooNew Relic, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="2365c-227">**tooassign Britta Simon tooNew Relic, perform hello following steps:**</span></span>

1. <span data-ttu-id="2365c-228">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2365c-228">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2365c-230">Välj i listan med program hello **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="2365c-230">In hello applications list, select **New Relic**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. <span data-ttu-id="2365c-232">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2365c-232">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2365c-234">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2365c-234">Click **Add** button.</span></span> <span data-ttu-id="2365c-235">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2365c-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2365c-237">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="2365c-237">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2365c-238">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2365c-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2365c-239">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2365c-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2365c-240">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2365c-240">Testing single sign-on</span></span>

<span data-ttu-id="2365c-241">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2365c-241">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2365c-242">När du klickar på hello New Relic-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour New Relic-programmet.</span><span class="sxs-lookup"><span data-stu-id="2365c-242">When you click hello New Relic tile in hello Access Panel, you should get automatically signed-on tooyour New Relic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2365c-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2365c-243">Additional resources</span></span>

* [<span data-ttu-id="2365c-244">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2365c-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2365c-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2365c-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

