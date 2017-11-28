---
title: "Självstudier: Azure Active Directory-integrering med Litmos | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Litmos."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 026fd10058760f2d63d185ef4aa9d7de3b82525e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a><span data-ttu-id="4522c-103">Självstudier: Azure Active Directory-integrering med Litmos</span><span class="sxs-lookup"><span data-stu-id="4522c-103">Tutorial: Azure Active Directory integration with Litmos</span></span>

<span data-ttu-id="4522c-104">I kursen får du lära dig hur toointegrate Litmos med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4522c-104">In this tutorial, you learn how toointegrate Litmos with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4522c-105">Integrera Litmos med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4522c-105">Integrating Litmos with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4522c-106">Du kan styra i Azure AD som har åtkomst till tooLitmos.</span><span class="sxs-lookup"><span data-stu-id="4522c-106">You can control in Azure AD who has access tooLitmos.</span></span>
- <span data-ttu-id="4522c-107">Du kan låta dina användare tooautomatically get inloggade tooLitmos (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="4522c-107">You can enable your users tooautomatically get signed-on tooLitmos (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="4522c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4522c-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="4522c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4522c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4522c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4522c-110">Prerequisites</span></span>

<span data-ttu-id="4522c-111">tooconfigure Azure AD-integrering med Litmos, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4522c-111">tooconfigure Azure AD integration with Litmos, you need hello following items:</span></span>

- <span data-ttu-id="4522c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4522c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4522c-113">En Litmos enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4522c-113">A Litmos single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4522c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4522c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4522c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4522c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4522c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4522c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4522c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4522c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4522c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4522c-118">Scenario description</span></span>
<span data-ttu-id="4522c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4522c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4522c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4522c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4522c-121">Att lägga till Litmos från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4522c-121">Adding Litmos from hello gallery</span></span>
2. <span data-ttu-id="4522c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4522c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-litmos-from-hello-gallery"></a><span data-ttu-id="4522c-123">Att lägga till Litmos från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4522c-123">Adding Litmos from hello gallery</span></span>
<span data-ttu-id="4522c-124">tooconfigure hello integrering av Litmos i Azure AD, behöver du tooadd Litmos hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4522c-124">tooconfigure hello integration of Litmos into Azure AD, you need tooadd Litmos from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4522c-125">**tooadd Litmos från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4522c-125">**tooadd Litmos from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4522c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4522c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="4522c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4522c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4522c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4522c-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="4522c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4522c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="4522c-133">Skriv i sökrutan hello **Litmos**väljer **Litmos** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4522c-133">In hello search box, type **Litmos**, select **Litmos** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Litmos i hello resultatlistan](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4522c-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4522c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="4522c-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Litmos baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4522c-136">In this section, you configure and test Azure AD single sign-on with Litmos based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4522c-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Litmos är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4522c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Litmos is tooa user in Azure AD.</span></span> <span data-ttu-id="4522c-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Litmos toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4522c-138">In other words, a link relationship between an Azure AD user and hello related user in Litmos needs toobe established.</span></span>

<span data-ttu-id="4522c-139">I Litmos, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4522c-139">In Litmos, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4522c-140">tooconfigure och testa Azure AD enkel inloggning med Litmos, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4522c-140">tooconfigure and test Azure AD single sign-on with Litmos, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4522c-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4522c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4522c-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4522c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4522c-143">**[Skapa en testanvändare Litmos](#create-a-litmos-test-user)**  -toohave en motsvarighet för Britta Simon i Litmos som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4522c-143">**[Create a Litmos test user](#create-a-litmos-test-user)** - toohave a counterpart of Britta Simon in Litmos that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4522c-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4522c-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4522c-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4522c-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="4522c-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4522c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="4522c-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Litmos program.</span><span class="sxs-lookup"><span data-stu-id="4522c-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Litmos application.</span></span>

<span data-ttu-id="4522c-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Litmos:**</span><span class="sxs-lookup"><span data-stu-id="4522c-148">**tooconfigure Azure AD single sign-on with Litmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="4522c-149">I hello Azure-portalen på hello **Litmos** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4522c-149">In hello Azure portal, on hello **Litmos** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="4522c-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4522c-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_samlbase.png)

3. <span data-ttu-id="4522c-153">På hello **Litmos domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4522c-153">On hello **Litmos Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Litmos domän med enkel inloggning information](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_url.png)

    <span data-ttu-id="4522c-155">a.</span><span class="sxs-lookup"><span data-stu-id="4522c-155">a.</span></span> <span data-ttu-id="4522c-156">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.litmos.com/account/Login`</span><span class="sxs-lookup"><span data-stu-id="4522c-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.litmos.com/account/Login`</span></span>

    <span data-ttu-id="4522c-157">b.</span><span class="sxs-lookup"><span data-stu-id="4522c-157">b.</span></span> <span data-ttu-id="4522c-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.litmos.com/integration/samllogin`</span><span class="sxs-lookup"><span data-stu-id="4522c-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.litmos.com/integration/samllogin`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4522c-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4522c-159">These values are not real.</span></span> <span data-ttu-id="4522c-160">Uppdatera dessa värden med hello faktiska identifierare och Reply URL, som beskrivs senare i självstudiekursen eller kontakta [Litmos supportteam](https://www.litmos.com/contact-us/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4522c-160">Update these values with hello actual Identifier and Reply URL, which are explained later in tutorial or contact [Litmos support team](https://www.litmos.com/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="4522c-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="4522c-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_certificate.png)

5. <span data-ttu-id="4522c-163">Som en del av hello konfigurationen, måste toocustomize hello **SAML-Token attribut** för tillämpningsprogrammet Litmos.</span><span class="sxs-lookup"><span data-stu-id="4522c-163">As part of hello configuration, you need toocustomize hello **SAML Token Attributes** for your Litmos application.</span></span>

    ![Attributet avsnitt](./media/active-directory-saas-litmos-tutorial/tutorial_attribute.png)
           
    | <span data-ttu-id="4522c-165">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="4522c-165">Attribute Name</span></span>   | <span data-ttu-id="4522c-166">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="4522c-166">Attribute Value</span></span> |   
    | ---------------  | ----------------|
    | <span data-ttu-id="4522c-167">Förnamn</span><span class="sxs-lookup"><span data-stu-id="4522c-167">FirstName</span></span> |<span data-ttu-id="4522c-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="4522c-168">user.givenname</span></span> |
    | <span data-ttu-id="4522c-169">Efternamn</span><span class="sxs-lookup"><span data-stu-id="4522c-169">LastName</span></span>  |<span data-ttu-id="4522c-170">User.surname</span><span class="sxs-lookup"><span data-stu-id="4522c-170">user.surname</span></span> |
    | <span data-ttu-id="4522c-171">E-post</span><span class="sxs-lookup"><span data-stu-id="4522c-171">Email</span></span> |<span data-ttu-id="4522c-172">User.Mail</span><span class="sxs-lookup"><span data-stu-id="4522c-172">user.mail</span></span> |

    <span data-ttu-id="4522c-173">a.</span><span class="sxs-lookup"><span data-stu-id="4522c-173">a.</span></span> <span data-ttu-id="4522c-174">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4522c-174">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Lägg till attribut](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_04.png)

    ![Lägg till attribut Dailog](./media/active-directory-saas-litmos-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="4522c-177">b.</span><span class="sxs-lookup"><span data-stu-id="4522c-177">b.</span></span> <span data-ttu-id="4522c-178">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="4522c-178">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="4522c-179">c.</span><span class="sxs-lookup"><span data-stu-id="4522c-179">c.</span></span> <span data-ttu-id="4522c-180">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="4522c-180">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="4522c-181">d.</span><span class="sxs-lookup"><span data-stu-id="4522c-181">d.</span></span> <span data-ttu-id="4522c-182">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4522c-182">Click **Ok**.</span></span>     

6. <span data-ttu-id="4522c-183">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4522c-183">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-litmos-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="4522c-185">I ett annat fönster i webbläsaren, inloggning tooyour Litmos företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="4522c-185">In a different browser window, sign-on tooyour Litmos company site as administrator.</span></span>

8. <span data-ttu-id="4522c-186">I navigeringsfältet hello vänster hello klickar du på **konton**.</span><span class="sxs-lookup"><span data-stu-id="4522c-186">In hello navigation bar on hello left side, click **Accounts**.</span></span>
   
    ![Kontoavsnittet på App-sida][22] 

9. <span data-ttu-id="4522c-188">Klicka på hello **integreringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="4522c-188">Click hello **Integrations** tab.</span></span>
   
    ![Fliken Integration][23] 

10. <span data-ttu-id="4522c-190">På hello **integreringar** , bläddrar du ned för**3 part integreringar**, och klicka sedan på **SAML 2.0** fliken.</span><span class="sxs-lookup"><span data-stu-id="4522c-190">On hello **Integrations** tab, scroll down too**3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0 avsnitt][24] 

11. <span data-ttu-id="4522c-192">Kopiera hello värde under **hello SAML-slutpunkt för litmos är:** och klistra in den i hello **Reply URL** TextBox-kontroll i hello **Litmos domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4522c-192">Copy hello value under **hello SAML endpoint for litmos is:** and paste it into hello **Reply URL** textbox in hello **Litmos Domain and URLs** section in Azure portal.</span></span> 
   
    ![SAML-slutpunkt][26] 

12. <span data-ttu-id="4522c-194">I din **Litmos** program, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4522c-194">In your **Litmos** application, perform hello following steps:</span></span>
    
     ![Litmos program][25] 
     
     <span data-ttu-id="4522c-196">a.</span><span class="sxs-lookup"><span data-stu-id="4522c-196">a.</span></span> <span data-ttu-id="4522c-197">Klicka på **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="4522c-197">Click **Enable SAML**.</span></span>
    
     <span data-ttu-id="4522c-198">b.</span><span class="sxs-lookup"><span data-stu-id="4522c-198">b.</span></span> <span data-ttu-id="4522c-199">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **SAML X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="4522c-199">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **SAML X.509 Certificate** textbox.</span></span>
     
     <span data-ttu-id="4522c-200">c.</span><span class="sxs-lookup"><span data-stu-id="4522c-200">c.</span></span> <span data-ttu-id="4522c-201">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="4522c-201">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="4522c-202">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4522c-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4522c-203">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4522c-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4522c-204">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4522c-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4522c-205">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4522c-205">Create an Azure AD test user</span></span>

<span data-ttu-id="4522c-206">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4522c-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="4522c-208">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4522c-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4522c-209">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="4522c-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-litmos-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="4522c-211">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4522c-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-litmos-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="4522c-213">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4522c-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="4522c-215">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4522c-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png)

    <span data-ttu-id="4522c-217">a.</span><span class="sxs-lookup"><span data-stu-id="4522c-217">a.</span></span> <span data-ttu-id="4522c-218">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4522c-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4522c-219">b.</span><span class="sxs-lookup"><span data-stu-id="4522c-219">b.</span></span> <span data-ttu-id="4522c-220">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4522c-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="4522c-221">c.</span><span class="sxs-lookup"><span data-stu-id="4522c-221">c.</span></span> <span data-ttu-id="4522c-222">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="4522c-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="4522c-223">d.</span><span class="sxs-lookup"><span data-stu-id="4522c-223">d.</span></span> <span data-ttu-id="4522c-224">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4522c-224">Click **Create**.</span></span>
  
### <a name="create-a-litmos-test-user"></a><span data-ttu-id="4522c-225">Skapa en testanvändare Litmos</span><span class="sxs-lookup"><span data-stu-id="4522c-225">Create a Litmos test user</span></span>

<span data-ttu-id="4522c-226">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Litmos.</span><span class="sxs-lookup"><span data-stu-id="4522c-226">hello objective of this section is toocreate a user called Britta Simon in Litmos.</span></span>  
<span data-ttu-id="4522c-227">Hej Litmos programmet stöder Just-in-Time-etablering.</span><span class="sxs-lookup"><span data-stu-id="4522c-227">hello Litmos application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="4522c-228">Det innebär ett konto skapas automatiskt om det behövs under ett försök tooaccess hello-program med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4522c-228">This means, a user account is automatically created if necessary during an attempt tooaccess hello application using hello Access Panel.</span></span>

<span data-ttu-id="4522c-229">**toocreate en användare som kallas Britta Simon i Litmos, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="4522c-229">**toocreate a user called Britta Simon in Litmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="4522c-230">I ett annat fönster i webbläsaren, inloggning tooyour Litmos företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="4522c-230">In a different browser window, sign-on tooyour Litmos company site as administrator.</span></span>

2. <span data-ttu-id="4522c-231">I navigeringsfältet hello vänster hello klickar du på **konton**.</span><span class="sxs-lookup"><span data-stu-id="4522c-231">In hello navigation bar on hello left side, click **Accounts**.</span></span>
   
    ![Kontoavsnittet på App-sida][22] 

3. <span data-ttu-id="4522c-233">Klicka på hello **integreringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="4522c-233">Click hello **Integrations** tab.</span></span>
   
    ![Fliken integreringar][23] 

4. <span data-ttu-id="4522c-235">På hello **integreringar** , bläddrar du ned för**3 part integreringar**, och klicka sedan på **SAML 2.0** fliken.</span><span class="sxs-lookup"><span data-stu-id="4522c-235">On hello **Integrations** tab, scroll down too**3rd Party Integrations**, and then click **SAML 2.0** tab.</span></span>
   
    ![SAML 2.0][24] 
    
5. <span data-ttu-id="4522c-237">Välj **Autogenerate användare**</span><span class="sxs-lookup"><span data-stu-id="4522c-237">Select **Autogenerate Users**</span></span>
   
    ![Automatiskt genererat användare][27] 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="4522c-239">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4522c-239">Assign hello Azure AD test user</span></span>

<span data-ttu-id="4522c-240">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooLitmos.</span><span class="sxs-lookup"><span data-stu-id="4522c-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooLitmos.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="4522c-242">**tooassign Britta Simon tooLitmos utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4522c-242">**tooassign Britta Simon tooLitmos, perform hello following steps:**</span></span>

1. <span data-ttu-id="4522c-243">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4522c-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4522c-245">Välj i listan med program hello **Litmos**.</span><span class="sxs-lookup"><span data-stu-id="4522c-245">In hello applications list, select **Litmos**.</span></span>

    ![Hej Litmos länken i listan med program hello](./media/active-directory-saas-litmos-tutorial/tutorial_litmos_app.png)  

3. <span data-ttu-id="4522c-247">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4522c-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="4522c-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4522c-249">Click **Add** button.</span></span> <span data-ttu-id="4522c-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4522c-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="4522c-252">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4522c-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4522c-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4522c-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4522c-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4522c-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4522c-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4522c-255">Test single sign-on</span></span>

<span data-ttu-id="4522c-256">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4522c-256">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  

<span data-ttu-id="4522c-257">Du bör få automatiskt inloggade tooyour Litmos programmet när du klickar på hello Litmos panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4522c-257">When you click hello Litmos tile in hello Access Panel, you should get automatically signed-on tooyour Litmos application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4522c-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4522c-258">Additional resources</span></span>

* [<span data-ttu-id="4522c-259">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4522c-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4522c-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4522c-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png

