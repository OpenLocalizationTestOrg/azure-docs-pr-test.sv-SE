---
title: "Självstudier: Azure Active Directory-integrering med ThousandEyes | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ThousandEyes."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: fbfbfb71809355b1b138762757a851907737730b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a><span data-ttu-id="93591-103">Självstudier: Azure Active Directory-integrering med ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="93591-103">Tutorial: Azure Active Directory integration with ThousandEyes</span></span>

<span data-ttu-id="93591-104">I kursen får du lära dig hur toointegrate ThousandEyes med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="93591-104">In this tutorial, you learn how toointegrate ThousandEyes with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="93591-105">Integrera ThousandEyes med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="93591-105">Integrating ThousandEyes with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="93591-106">Du kan styra i Azure AD som har åtkomst till tooThousandEyes</span><span class="sxs-lookup"><span data-stu-id="93591-106">You can control in Azure AD who has access tooThousandEyes</span></span>
- <span data-ttu-id="93591-107">Du kan aktivera din användare tooautomatically get inloggade tooThousandEyes (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="93591-107">You can enable your users tooautomatically get signed-on tooThousandEyes (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="93591-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="93591-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="93591-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="93591-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93591-110">Krav</span><span class="sxs-lookup"><span data-stu-id="93591-110">Prerequisites</span></span>

<span data-ttu-id="93591-111">tooconfigure Azure AD-integrering med ThousandEyes, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="93591-111">tooconfigure Azure AD integration with ThousandEyes, you need hello following items:</span></span>

- <span data-ttu-id="93591-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="93591-112">An Azure AD subscription</span></span>
- <span data-ttu-id="93591-113">En ThousandEyes enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="93591-113">A ThousandEyes single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="93591-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="93591-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="93591-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="93591-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="93591-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="93591-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="93591-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="93591-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="93591-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="93591-118">Scenario description</span></span>
<span data-ttu-id="93591-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="93591-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="93591-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="93591-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="93591-121">Att lägga till ThousandEyes från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="93591-121">Adding ThousandEyes from hello gallery</span></span>
2. <span data-ttu-id="93591-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93591-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thousandeyes-from-hello-gallery"></a><span data-ttu-id="93591-123">Att lägga till ThousandEyes från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="93591-123">Adding ThousandEyes from hello gallery</span></span>
<span data-ttu-id="93591-124">tooconfigure hello integrering av ThousandEyes i Azure AD, behöver du tooadd ThousandEyes hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="93591-124">tooconfigure hello integration of ThousandEyes into Azure AD, you need tooadd ThousandEyes from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="93591-125">**tooadd ThousandEyes från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="93591-125">**tooadd ThousandEyes from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="93591-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="93591-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="93591-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="93591-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="93591-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="93591-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="93591-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93591-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="93591-133">Skriv i sökrutan hello **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="93591-133">In hello search box, type **ThousandEyes**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. <span data-ttu-id="93591-135">Markera hello resultat på panelen **ThousandEyes**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="93591-135">In hello results panel, select **ThousandEyes**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="93591-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93591-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="93591-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ThousandEyes baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="93591-138">In this section, you configure and test Azure AD single sign-on with ThousandEyes based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="93591-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ThousandEyes är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="93591-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ThousandEyes is tooa user in Azure AD.</span></span> <span data-ttu-id="93591-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ThousandEyes toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="93591-140">In other words, a link relationship between an Azure AD user and hello related user in ThousandEyes needs toobe established.</span></span>

<span data-ttu-id="93591-141">I ThousandEyes, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="93591-141">In ThousandEyes, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="93591-142">tooconfigure och testa Azure AD enkel inloggning med ThousandEyes, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="93591-142">tooconfigure and test Azure AD single sign-on with ThousandEyes, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="93591-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="93591-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="93591-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="93591-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="93591-145">**[Skapa en testanvändare ThousandEyes](#creating-a-thousandeyes-test-user)**  -toohave en motsvarighet för Britta Simon i ThousandEyes som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="93591-145">**[Creating a ThousandEyes test user](#creating-a-thousandeyes-test-user)** - toohave a counterpart of Britta Simon in ThousandEyes that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="93591-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="93591-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="93591-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="93591-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="93591-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93591-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="93591-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ThousandEyes program.</span><span class="sxs-lookup"><span data-stu-id="93591-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ThousandEyes application.</span></span>

<span data-ttu-id="93591-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ThousandEyes:**</span><span class="sxs-lookup"><span data-stu-id="93591-150">**tooconfigure Azure AD single sign-on with ThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="93591-151">I hello Azure-portalen på hello **ThousandEyes** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="93591-151">In hello Azure portal, on hello **ThousandEyes** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="93591-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="93591-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. <span data-ttu-id="93591-155">På hello **ThousandEyes domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="93591-155">On hello **ThousandEyes Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    <span data-ttu-id="93591-157">I hello **inloggnings-URL** textruta Skriv en URL som:`https://app.thousandeyes.com/login/sso`</span><span class="sxs-lookup"><span data-stu-id="93591-157">In hello **Sign-on URL** textbox, type a URL as: `https://app.thousandeyes.com/login/sso`</span></span>

4. <span data-ttu-id="93591-158">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="93591-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. <span data-ttu-id="93591-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="93591-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="93591-162">På hello **ThousandEyes Configuration** klickar du på **konfigurera ThousandEyes** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="93591-162">On hello **ThousandEyes Configuration** section, click **Configure ThousandEyes** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="93591-163">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="93591-163">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. <span data-ttu-id="93591-165">Inloggning i en annan webbläsarfönster tooyour **ThousandEyes** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="93591-165">In a different web browser window, sign on tooyour **ThousandEyes** company site as an administrator.</span></span>

8. <span data-ttu-id="93591-166">Hello-menyn överst hello **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="93591-166">In hello menu on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="93591-167">![Inställningar för](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="93591-167">![Settings](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "Settings")</span></span>

9. <span data-ttu-id="93591-168">Klicka på **konto**</span><span class="sxs-lookup"><span data-stu-id="93591-168">Click **Account**</span></span>
   
    <span data-ttu-id="93591-169">![Kontot](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "konto")</span><span class="sxs-lookup"><span data-stu-id="93591-169">![Account](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "Account")</span></span>

10. <span data-ttu-id="93591-170">Klicka på hello **säkerhets- och** fliken.</span><span class="sxs-lookup"><span data-stu-id="93591-170">Click hello **Security & Authentication** tab.</span></span>
   
    <span data-ttu-id="93591-171">![Säkerhets- och](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "säkerhets- och autentisering")</span><span class="sxs-lookup"><span data-stu-id="93591-171">![Security & Authentication](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "Security & Authentication")</span></span>

11. <span data-ttu-id="93591-172">I hello **installationsprogrammet för enkel inloggning** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="93591-172">In hello **Setup Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="93591-173">![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="93591-173">![Setup Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Setup Single Sign-On")</span></span>
  
    <span data-ttu-id="93591-174">a.</span><span class="sxs-lookup"><span data-stu-id="93591-174">a.</span></span> <span data-ttu-id="93591-175">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="93591-175">Select **Enable Single Sign-On**.</span></span>
  
    <span data-ttu-id="93591-176">b.</span><span class="sxs-lookup"><span data-stu-id="93591-176">b.</span></span> <span data-ttu-id="93591-177">I **URL för inloggningssidan** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93591-177">In **Login Page URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="93591-178">c.</span><span class="sxs-lookup"><span data-stu-id="93591-178">c.</span></span> <span data-ttu-id="93591-179">I **logga ut Sidadress** textruta klistra in **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93591-179">In **Logout Page URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="93591-180">d.</span><span class="sxs-lookup"><span data-stu-id="93591-180">d.</span></span> <span data-ttu-id="93591-181">**Identitet providern utfärdaren** textruta klistra in **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93591-181">**Identity Provider Issuer** textbox, paste **SAML Entity ID** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="93591-182">e.</span><span class="sxs-lookup"><span data-stu-id="93591-182">e.</span></span> <span data-ttu-id="93591-183">I **verifieringscertifikat**, klickar du på **Välj fil**, och sedan ladda upp hello-certifikat som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93591-183">In **Verification Certificate**, click **Choose file**, and then upload hello certificate you have downloaded from Azure portal.</span></span>
  
    <span data-ttu-id="93591-184">f.</span><span class="sxs-lookup"><span data-stu-id="93591-184">f.</span></span> <span data-ttu-id="93591-185">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="93591-185">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="93591-186">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="93591-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="93591-187">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="93591-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="93591-188">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="93591-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="93591-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="93591-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="93591-190">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="93591-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="93591-192">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="93591-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="93591-193">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="93591-193">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="93591-195">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="93591-195">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="93591-197">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93591-197">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="93591-199">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="93591-199">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="93591-201">a.</span><span class="sxs-lookup"><span data-stu-id="93591-201">a.</span></span> <span data-ttu-id="93591-202">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="93591-202">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="93591-203">b.</span><span class="sxs-lookup"><span data-stu-id="93591-203">b.</span></span> <span data-ttu-id="93591-204">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="93591-204">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="93591-205">c.</span><span class="sxs-lookup"><span data-stu-id="93591-205">c.</span></span> <span data-ttu-id="93591-206">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="93591-206">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="93591-207">d.</span><span class="sxs-lookup"><span data-stu-id="93591-207">d.</span></span> <span data-ttu-id="93591-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="93591-208">Click **Create**.</span></span>
 
### <a name="creating-a-thousandeyes-test-user"></a><span data-ttu-id="93591-209">Skapa en testanvändare ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="93591-209">Creating a ThousandEyes test user</span></span>

<span data-ttu-id="93591-210">I ordning tooenable Azure AD-användare toolog i ThousandEyes, måste de etableras i ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="93591-210">In order tooenable Azure AD users toolog into ThousandEyes, they must be provisioned into ThousandEyes.</span></span>  
<span data-ttu-id="93591-211">Hello gäller ThousandEyes är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="93591-211">In hello case of ThousandEyes, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="93591-212">Du kan använda något annat ThousandEyes användarens konto skapas verktyg eller API: er som tillhandahålls av ThousandEyes tooprovision Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="93591-212">You can use any other ThousandEyes user account creation tools or APIs provided by ThousandEyes tooprovision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="93591-213">**tooprovision konto tooThousandEyes för en användare utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="93591-213">**tooprovision a user account tooThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="93591-214">Logga in på webbplatsen ThousandEyes företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="93591-214">Log into your ThousandEyes company site as an administrator.</span></span>

2. <span data-ttu-id="93591-215">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="93591-215">Click **Settings**.</span></span>
   
    <span data-ttu-id="93591-216">![Inställningar för](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="93591-216">![Settings](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")</span></span>

3. <span data-ttu-id="93591-217">Klicka på **konto**.</span><span class="sxs-lookup"><span data-stu-id="93591-217">Click **Account**.</span></span>
   
    <span data-ttu-id="93591-218">![Kontot](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "konto")</span><span class="sxs-lookup"><span data-stu-id="93591-218">![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")</span></span>

4. <span data-ttu-id="93591-219">Klicka på hello **konton och användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="93591-219">Click hello **Accounts & Users** tab.</span></span>
   
    <span data-ttu-id="93591-220">![Konton och användare](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "konton och användare")</span><span class="sxs-lookup"><span data-stu-id="93591-220">![Accounts & Users](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")</span></span>

5. <span data-ttu-id="93591-221">I hello **lägga till användare och konton** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="93591-221">In hello **Add Users & Accounts** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="93591-222">![Lägga till användarkonton](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "lägga till användarkonton")</span><span class="sxs-lookup"><span data-stu-id="93591-222">![Add User Accounts](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")</span></span>   
  
    <span data-ttu-id="93591-223">a.</span><span class="sxs-lookup"><span data-stu-id="93591-223">a.</span></span> <span data-ttu-id="93591-224">I **namn** textruta hello-typnamn för användaren som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="93591-224">In **Name** textbox, type hello name of user like **Britta Simon**.</span></span>

    <span data-ttu-id="93591-225">b.</span><span class="sxs-lookup"><span data-stu-id="93591-225">b.</span></span> <span data-ttu-id="93591-226">I **e-post** textruta hello e-post för användare som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="93591-226">In **Email** textbox, type hello email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="93591-227">b.</span><span class="sxs-lookup"><span data-stu-id="93591-227">b.</span></span> <span data-ttu-id="93591-228">Klicka på **Lägg till nya användare tooAccount**.</span><span class="sxs-lookup"><span data-stu-id="93591-228">Click **Add New User tooAccount**.</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="93591-229">hello Azure Active Directory användare kommer att få ett e-postmeddelande inklusive en länk tooconfirm och aktivera hello-konto.</span><span class="sxs-lookup"><span data-stu-id="93591-229">hello Azure Active Directory account holder will get an email including a link tooconfirm and activate hello account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="93591-230">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="93591-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="93591-231">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="93591-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooThousandEyes.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="93591-233">**tooassign Britta Simon tooThousandEyes utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="93591-233">**tooassign Britta Simon tooThousandEyes, perform hello following steps:**</span></span>

1. <span data-ttu-id="93591-234">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="93591-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="93591-236">Välj i listan med program hello **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="93591-236">In hello applications list, select **ThousandEyes**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. <span data-ttu-id="93591-238">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="93591-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="93591-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="93591-240">Click **Add** button.</span></span> <span data-ttu-id="93591-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93591-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="93591-243">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="93591-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="93591-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93591-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="93591-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="93591-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="93591-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="93591-246">Testing single sign-on</span></span>

<span data-ttu-id="93591-247">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="93591-247">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="93591-248">Du bör få automatiskt inloggade tooyour ThousandEyes programmet när du klickar på hello ThousandEyes panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="93591-248">When you click hello ThousandEyes tile in hello Access Panel, you should get automatically signed-on tooyour ThousandEyes application.</span></span>

<span data-ttu-id="93591-249">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="93591-249">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93591-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="93591-250">Additional resources</span></span>

* [<span data-ttu-id="93591-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="93591-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="93591-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="93591-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png

