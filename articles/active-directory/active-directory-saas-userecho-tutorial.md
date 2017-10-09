---
title: "Självstudier: Azure Active Directory-integrering med UserEcho | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och UserEcho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: efe4a94ed6e5d22d153565d4782850eac4dff37b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a><span data-ttu-id="dda89-103">Självstudier: Azure Active Directory-integrering med UserEcho</span><span class="sxs-lookup"><span data-stu-id="dda89-103">Tutorial: Azure Active Directory integration with UserEcho</span></span>

<span data-ttu-id="dda89-104">I kursen får du lära dig hur toointegrate UserEcho med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="dda89-104">In this tutorial, you learn how toointegrate UserEcho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dda89-105">Integrera UserEcho med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="dda89-105">Integrating UserEcho with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="dda89-106">Du kan styra i Azure AD som har åtkomst till tooUserEcho</span><span class="sxs-lookup"><span data-stu-id="dda89-106">You can control in Azure AD who has access tooUserEcho</span></span>
- <span data-ttu-id="dda89-107">Du kan aktivera din användare tooautomatically get inloggade tooUserEcho (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="dda89-107">You can enable your users tooautomatically get signed-on tooUserEcho (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dda89-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dda89-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="dda89-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dda89-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dda89-110">Krav</span><span class="sxs-lookup"><span data-stu-id="dda89-110">Prerequisites</span></span>

<span data-ttu-id="dda89-111">tooconfigure Azure AD-integrering med UserEcho, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="dda89-111">tooconfigure Azure AD integration with UserEcho, you need hello following items:</span></span>

- <span data-ttu-id="dda89-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dda89-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dda89-113">En UserEcho enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="dda89-113">A UserEcho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dda89-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="dda89-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dda89-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="dda89-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dda89-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="dda89-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dda89-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dda89-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dda89-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="dda89-118">Scenario description</span></span>
<span data-ttu-id="dda89-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="dda89-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dda89-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="dda89-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dda89-121">Att lägga till UserEcho från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dda89-121">Adding UserEcho from hello gallery</span></span>
2. <span data-ttu-id="dda89-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dda89-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-userecho-from-hello-gallery"></a><span data-ttu-id="dda89-123">Att lägga till UserEcho från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="dda89-123">Adding UserEcho from hello gallery</span></span>
<span data-ttu-id="dda89-124">tooconfigure hello integrering av UserEcho i Azure AD, behöver du tooadd UserEcho hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="dda89-124">tooconfigure hello integration of UserEcho into Azure AD, you need tooadd UserEcho from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="dda89-125">**tooadd UserEcho från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="dda89-125">**tooadd UserEcho from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="dda89-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dda89-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dda89-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="dda89-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="dda89-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="dda89-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="dda89-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dda89-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="dda89-133">Skriv i sökrutan hello **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="dda89-133">In hello search box, type **UserEcho**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. <span data-ttu-id="dda89-135">Markera hello resultat på panelen **UserEcho**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="dda89-135">In hello results panel, select **UserEcho**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dda89-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dda89-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dda89-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med UserEcho baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="dda89-138">In this section, you configure and test Azure AD single sign-on with UserEcho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dda89-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i UserEcho är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="dda89-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UserEcho is tooa user in Azure AD.</span></span> <span data-ttu-id="dda89-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i UserEcho toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="dda89-140">In other words, a link relationship between an Azure AD user and hello related user in UserEcho needs toobe established.</span></span>

<span data-ttu-id="dda89-141">I UserEcho, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="dda89-141">In UserEcho, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="dda89-142">tooconfigure och testa Azure AD enkel inloggning med UserEcho, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="dda89-142">tooconfigure and test Azure AD single sign-on with UserEcho, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="dda89-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="dda89-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="dda89-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dda89-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dda89-145">**[Skapa en testanvändare UserEcho](#creating-a-userecho-test-user)**  -toohave en motsvarighet för Britta Simon i UserEcho som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="dda89-145">**[Creating a UserEcho test user](#creating-a-userecho-test-user)** - toohave a counterpart of Britta Simon in UserEcho that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="dda89-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dda89-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dda89-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="dda89-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dda89-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dda89-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dda89-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt UserEcho program.</span><span class="sxs-lookup"><span data-stu-id="dda89-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UserEcho application.</span></span>

<span data-ttu-id="dda89-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med UserEcho:**</span><span class="sxs-lookup"><span data-stu-id="dda89-150">**tooconfigure Azure AD single sign-on with UserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="dda89-151">I hello Azure-portalen på hello **UserEcho** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dda89-151">In hello Azure portal, on hello **UserEcho** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="dda89-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dda89-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. <span data-ttu-id="dda89-155">På hello **UserEcho domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dda89-155">On hello **UserEcho Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    <span data-ttu-id="dda89-157">a.</span><span class="sxs-lookup"><span data-stu-id="dda89-157">a.</span></span> <span data-ttu-id="dda89-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.userecho.com/`</span><span class="sxs-lookup"><span data-stu-id="dda89-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.userecho.com/`</span></span>

    <span data-ttu-id="dda89-159">b.</span><span class="sxs-lookup"><span data-stu-id="dda89-159">b.</span></span> <span data-ttu-id="dda89-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.userecho.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="dda89-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.userecho.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dda89-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="dda89-161">These values are not real.</span></span> <span data-ttu-id="dda89-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="dda89-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="dda89-163">Kontakta [UserEcho klienten supportteamet](https://feedback.userecho.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="dda89-163">Contact [UserEcho Client support team](https://feedback.userecho.com/) tooget these values.</span></span> 

4. <span data-ttu-id="dda89-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="dda89-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. <span data-ttu-id="dda89-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="dda89-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dda89-168">På hello **UserEcho Configuration** klickar du på **konfigurera UserEcho** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="dda89-168">On hello **UserEcho Configuration** section, click **Configure UserEcho** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="dda89-169">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="dda89-169">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. <span data-ttu-id="dda89-171">Logga in tooyour UserEcho företagets webbplats som en administratör i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="dda89-171">In another browser window, sign on tooyour UserEcho company site as an administrator.</span></span>

8. <span data-ttu-id="dda89-172">Klicka på din användare namn tooexpand hello-menyn i hello verktygsfältet hello överst och klicka sedan på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="dda89-172">In hello toolbar on hello top, click your user name tooexpand hello menu, and then click **Setup**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. <span data-ttu-id="dda89-174">Klicka på **integreringar**.</span><span class="sxs-lookup"><span data-stu-id="dda89-174">Click **Integrations**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. <span data-ttu-id="dda89-176">Klicka på **webbplats**, och klicka sedan på **enkel inloggning (SAML2)**.</span><span class="sxs-lookup"><span data-stu-id="dda89-176">Click **Website**, and then click **Single sign-on (SAML2)**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. <span data-ttu-id="dda89-178">På hello **enkel inloggning (SAML)** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dda89-178">On hello **Single sign-on (SAML)** page, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    <span data-ttu-id="dda89-180">a.</span><span class="sxs-lookup"><span data-stu-id="dda89-180">a.</span></span> <span data-ttu-id="dda89-181">Som **SAML-aktiverade**väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="dda89-181">As **SAML-enabled**, select **Yes**.</span></span>
    
    <span data-ttu-id="dda89-182">b.</span><span class="sxs-lookup"><span data-stu-id="dda89-182">b.</span></span> <span data-ttu-id="dda89-183">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **SAML SSO URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="dda89-183">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **SAML SSO URL** textbox.</span></span>
    
    <span data-ttu-id="dda89-184">c.</span><span class="sxs-lookup"><span data-stu-id="dda89-184">c.</span></span> <span data-ttu-id="dda89-185">Klistra in **Sign-Out URL**, som du har kopierat från hello Azure-portalen i hello **Remote logoout URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="dda89-185">Paste **Sign-Out URL**, which you have copied from hello Azure portal into hello **Remote logoout URL** textbox.</span></span>
    
    <span data-ttu-id="dda89-186">d.</span><span class="sxs-lookup"><span data-stu-id="dda89-186">d.</span></span> <span data-ttu-id="dda89-187">Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="dda89-187">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **X.509 Certificate** textbox.</span></span>
    
    <span data-ttu-id="dda89-188">e.</span><span class="sxs-lookup"><span data-stu-id="dda89-188">e.</span></span> <span data-ttu-id="dda89-189">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dda89-189">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="dda89-190">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="dda89-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="dda89-191">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="dda89-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="dda89-192">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dda89-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dda89-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="dda89-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="dda89-194">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dda89-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="dda89-196">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="dda89-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="dda89-197">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dda89-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dda89-199">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="dda89-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dda89-201">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dda89-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dda89-203">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dda89-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dda89-205">a.</span><span class="sxs-lookup"><span data-stu-id="dda89-205">a.</span></span> <span data-ttu-id="dda89-206">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dda89-206">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dda89-207">b.</span><span class="sxs-lookup"><span data-stu-id="dda89-207">b.</span></span> <span data-ttu-id="dda89-208">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dda89-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dda89-209">c.</span><span class="sxs-lookup"><span data-stu-id="dda89-209">c.</span></span> <span data-ttu-id="dda89-210">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="dda89-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="dda89-211">d.</span><span class="sxs-lookup"><span data-stu-id="dda89-211">d.</span></span> <span data-ttu-id="dda89-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dda89-212">Click **Create**.</span></span>
 
### <a name="creating-a-userecho-test-user"></a><span data-ttu-id="dda89-213">Skapa en testanvändare UserEcho</span><span class="sxs-lookup"><span data-stu-id="dda89-213">Creating a UserEcho test user</span></span>

<span data-ttu-id="dda89-214">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i UserEcho.</span><span class="sxs-lookup"><span data-stu-id="dda89-214">hello objective of this section is toocreate a user called Britta Simon in UserEcho.</span></span>

<span data-ttu-id="dda89-215">**toocreate en användare som kallas Britta Simon i UserEcho, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="dda89-215">**toocreate a user called Britta Simon in UserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="dda89-216">Inloggning tooyour UserEcho företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="dda89-216">Sign-on tooyour UserEcho company site as an administrator.</span></span>

2. <span data-ttu-id="dda89-217">Klicka på din användare namn tooexpand hello-menyn i hello verktygsfältet hello överst och klicka sedan på **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="dda89-217">In hello toolbar on hello top, click your user name tooexpand hello menu, and then click **Setup**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. <span data-ttu-id="dda89-219">Klicka på **användare**, tooexpand hello **användare** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="dda89-219">Click **Users**, tooexpand hello **Users** section.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. <span data-ttu-id="dda89-221">Klicka på **användare**.</span><span class="sxs-lookup"><span data-stu-id="dda89-221">Click **Users**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. <span data-ttu-id="dda89-223">Klicka på **bjuda in en ny användare**.</span><span class="sxs-lookup"><span data-stu-id="dda89-223">Click **Invite a new user**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. <span data-ttu-id="dda89-225">På hello **bjuda in en ny användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="dda89-225">On hello **Invite a new user** dialog, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    <span data-ttu-id="dda89-227">a.</span><span class="sxs-lookup"><span data-stu-id="dda89-227">a.</span></span> <span data-ttu-id="dda89-228">I hello **namn** textruta, ange namnet på hello användaren som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dda89-228">In hello **Name** textbox, type name of hello user like Britta Simon.</span></span>
    
    <span data-ttu-id="dda89-229">b.</span><span class="sxs-lookup"><span data-stu-id="dda89-229">b.</span></span>  <span data-ttu-id="dda89-230">I hello **e-post** textruta typen hello användarens e-postadress som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="dda89-230">In hello **Email** textbox, type hello email address of user like Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="dda89-231">c.</span><span class="sxs-lookup"><span data-stu-id="dda89-231">c.</span></span> <span data-ttu-id="dda89-232">Klicka på **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="dda89-232">Click **Invite**.</span></span>

<span data-ttu-id="dda89-233">En inbjudan har skickats tooBritta, vilket gör att hennes toostart med UserEcho.</span><span class="sxs-lookup"><span data-stu-id="dda89-233">An invitation is sent tooBritta, which enables her toostart using UserEcho.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="dda89-234">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="dda89-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="dda89-235">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooUserEcho.</span><span class="sxs-lookup"><span data-stu-id="dda89-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUserEcho.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="dda89-237">**tooassign Britta Simon tooUserEcho utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="dda89-237">**tooassign Britta Simon tooUserEcho, perform hello following steps:**</span></span>

1. <span data-ttu-id="dda89-238">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dda89-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="dda89-240">Välj i listan med program hello **UserEcho**.</span><span class="sxs-lookup"><span data-stu-id="dda89-240">In hello applications list, select **UserEcho**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. <span data-ttu-id="dda89-242">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dda89-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="dda89-244">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="dda89-244">Click **Add** button.</span></span> <span data-ttu-id="dda89-245">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dda89-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="dda89-247">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="dda89-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="dda89-248">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dda89-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dda89-249">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dda89-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dda89-250">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dda89-250">Testing single sign-on</span></span>

<span data-ttu-id="dda89-251">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="dda89-251">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="dda89-252">Du bör få automatiskt inloggade tooyour UserEcho programmet när du klickar på hello UserEcho panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="dda89-252">When you click hello UserEcho tile in hello Access Panel, you should get automatically signed-on tooyour UserEcho application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dda89-253">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="dda89-253">Additional resources</span></span>

* [<span data-ttu-id="dda89-254">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dda89-254">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dda89-255">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dda89-255">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png

