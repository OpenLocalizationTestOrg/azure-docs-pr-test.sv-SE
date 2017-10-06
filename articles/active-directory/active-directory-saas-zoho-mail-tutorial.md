---
title: "Självstudier: Azure Active Directory-integrering med Zoho | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Zoho."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 5d1c196d3b97babab196f509ea68e7d61523a7e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoho"></a><span data-ttu-id="fa21c-103">Självstudier: Azure Active Directory-integrering med Zoho</span><span class="sxs-lookup"><span data-stu-id="fa21c-103">Tutorial: Azure Active Directory integration with Zoho</span></span>

<span data-ttu-id="fa21c-104">I kursen får du lära dig hur toointegrate Zoho med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="fa21c-104">In this tutorial, you learn how toointegrate Zoho with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fa21c-105">Integrera Zoho med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="fa21c-105">Integrating Zoho with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="fa21c-106">Du kan styra i Azure AD som har åtkomst till tooZoho.</span><span class="sxs-lookup"><span data-stu-id="fa21c-106">You can control in Azure AD who has access tooZoho.</span></span>
- <span data-ttu-id="fa21c-107">Du kan låta dina användare tooautomatically get inloggade tooZoho (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="fa21c-107">You can enable your users tooautomatically get signed-on tooZoho (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="fa21c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="fa21c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fa21c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa21c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="fa21c-110">Prerequisites</span></span>

<span data-ttu-id="fa21c-111">tooconfigure Azure AD-integrering med Zoho, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="fa21c-111">tooconfigure Azure AD integration with Zoho, you need hello following items:</span></span>

- <span data-ttu-id="fa21c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="fa21c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fa21c-113">En Zoho enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="fa21c-113">A Zoho single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fa21c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fa21c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fa21c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="fa21c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fa21c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="fa21c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fa21c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fa21c-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fa21c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="fa21c-118">Scenario description</span></span>
<span data-ttu-id="fa21c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="fa21c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fa21c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="fa21c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fa21c-121">Att lägga till Zoho från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fa21c-121">Adding Zoho from hello gallery</span></span>
2. <span data-ttu-id="fa21c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fa21c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoho-from-hello-gallery"></a><span data-ttu-id="fa21c-123">Att lägga till Zoho från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="fa21c-123">Adding Zoho from hello gallery</span></span>
<span data-ttu-id="fa21c-124">tooconfigure hello integrering av Zoho i Azure AD, behöver du tooadd Zoho hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="fa21c-124">tooconfigure hello integration of Zoho into Azure AD, you need tooadd Zoho from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="fa21c-125">**tooadd Zoho från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fa21c-125">**tooadd Zoho from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa21c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="fa21c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="fa21c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="fa21c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="fa21c-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fa21c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="fa21c-133">Skriv i sökrutan hello **Zoho**väljer **Zoho** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="fa21c-133">In hello search box, type **Zoho**, select **Zoho** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Zoho i hello resultatlistan](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fa21c-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fa21c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="fa21c-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zoho baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="fa21c-136">In this section, you configure and test Azure AD single sign-on with Zoho based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fa21c-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Zoho är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fa21c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zoho is tooa user in Azure AD.</span></span> <span data-ttu-id="fa21c-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Zoho toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="fa21c-138">In other words, a link relationship between an Azure AD user and hello related user in Zoho needs toobe established.</span></span>

<span data-ttu-id="fa21c-139">I Zoho, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-139">In Zoho, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="fa21c-140">tooconfigure och testa Azure AD enkel inloggning med Zoho, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="fa21c-140">tooconfigure and test Azure AD single sign-on with Zoho, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="fa21c-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="fa21c-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fa21c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fa21c-143">**[Skapa en testanvändare Zoho](#create-a-zoho-test-user)**  -toohave en motsvarighet för Britta Simon i Zoho som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="fa21c-143">**[Create a Zoho test user](#create-a-zoho-test-user)** - toohave a counterpart of Britta Simon in Zoho that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="fa21c-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fa21c-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fa21c-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="fa21c-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fa21c-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fa21c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fa21c-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Zoho program.</span><span class="sxs-lookup"><span data-stu-id="fa21c-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zoho application.</span></span>

<span data-ttu-id="fa21c-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Zoho:**</span><span class="sxs-lookup"><span data-stu-id="fa21c-148">**tooconfigure Azure AD single sign-on with Zoho, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa21c-149">I hello Azure-portalen på hello **Zoho** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-149">In hello Azure portal, on hello **Zoho** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="fa21c-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="fa21c-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_samlbase.png)

3. <span data-ttu-id="fa21c-153">På hello **Zoho domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fa21c-153">On hello **Zoho Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Zoho domän med enkel inloggning information](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_url.png)

    <span data-ttu-id="fa21c-155">a.</span><span class="sxs-lookup"><span data-stu-id="fa21c-155">a.</span></span> <span data-ttu-id="fa21c-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.zohomail.com`</span><span class="sxs-lookup"><span data-stu-id="fa21c-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.zohomail.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fa21c-157">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="fa21c-157">This value is not real.</span></span> <span data-ttu-id="fa21c-158">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="fa21c-158">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="fa21c-159">Kontakta [Zoho klienten supportteamet](https://www.zoho.com/mail/contact.html) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="fa21c-159">Contact [Zoho Client support team](https://www.zoho.com/mail/contact.html) tooget this value.</span></span> 
 
4. <span data-ttu-id="fa21c-160">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="fa21c-160">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_certificate.png) 

5. <span data-ttu-id="fa21c-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fa21c-164">På hello **Zoho Configuration** klickar du på **konfigurera Zoho** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="fa21c-164">On hello **Zoho Configuration** section, click **Configure Zoho** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="fa21c-165">Kopiera hello **Sign-Out URL, ändra lösenord URL och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="fa21c-165">Copy hello **Sign-Out URL, Change Password URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Zoho konfiguration](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_configure.png) 

7. <span data-ttu-id="fa21c-167">Logga in på webbplatsen för företagets Zoho e-post som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="fa21c-167">In a different web browser window, log into your Zoho Mail company site as an administrator.</span></span>

8. <span data-ttu-id="fa21c-168">Gå toohello **Kontrollpanelen**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-168">Go toohello **Control panel**.</span></span>
   
    <span data-ttu-id="fa21c-169">![Kontrollpanelen](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Kontrollpanelen")</span><span class="sxs-lookup"><span data-stu-id="fa21c-169">![Control Panel](./media/active-directory-saas-zoho-mail-tutorial/ic789607.png "Control Panel")</span></span>

9. <span data-ttu-id="fa21c-170">Klicka på hello **SAML-autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="fa21c-170">Click hello **SAML Authentication** tab.</span></span>
   
    <span data-ttu-id="fa21c-171">![SAML-autentisering](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="fa21c-171">![SAML Authentication](./media/active-directory-saas-zoho-mail-tutorial/ic789608.png "SAML Authentication")</span></span>

10. <span data-ttu-id="fa21c-172">I hello **information om autentisering av SAML** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fa21c-172">In hello **SAML Authentication Details** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="fa21c-173">![Information om autentisering av SAML](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML-autentisering-information")</span><span class="sxs-lookup"><span data-stu-id="fa21c-173">![SAML Authentication Details](./media/active-directory-saas-zoho-mail-tutorial/ic789609.png "SAML Authentication Details")</span></span>
   
    <span data-ttu-id="fa21c-174">a.</span><span class="sxs-lookup"><span data-stu-id="fa21c-174">a.</span></span> <span data-ttu-id="fa21c-175">I hello **Inloggningswebbadressen** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-175">In hello **Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="fa21c-176">b.</span><span class="sxs-lookup"><span data-stu-id="fa21c-176">b.</span></span> <span data-ttu-id="fa21c-177">I hello **logga ut URL** textruta klistra in **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-177">In hello **Logout URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="fa21c-178">c.</span><span class="sxs-lookup"><span data-stu-id="fa21c-178">c.</span></span> <span data-ttu-id="fa21c-179">I hello **ändra lösenord URL** textruta klistra in **ändra lösenord URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-179">In hello **Change Password URL** textbox, paste **Change Password URL** which you have copied from Azure portal.</span></span>
       
    <span data-ttu-id="fa21c-180">d.</span><span class="sxs-lookup"><span data-stu-id="fa21c-180">d.</span></span> <span data-ttu-id="fa21c-181">Öppna din Base64-kodade certifikat hämtas från Azure-portalen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **PublicKey** textruta.</span><span class="sxs-lookup"><span data-stu-id="fa21c-181">Open your base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **PublicKey** textbox.</span></span>
   
    <span data-ttu-id="fa21c-182">e.</span><span class="sxs-lookup"><span data-stu-id="fa21c-182">e.</span></span> <span data-ttu-id="fa21c-183">Som **algoritmen**väljer **RSA**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-183">As **Algorithm**, select **RSA**.</span></span>
   
    <span data-ttu-id="fa21c-184">f.</span><span class="sxs-lookup"><span data-stu-id="fa21c-184">f.</span></span> <span data-ttu-id="fa21c-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-185">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="fa21c-186">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="fa21c-186">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="fa21c-187">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="fa21c-187">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="fa21c-188">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fa21c-188">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fa21c-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="fa21c-189">Create an Azure AD test user</span></span>

<span data-ttu-id="fa21c-190">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fa21c-190">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="fa21c-192">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fa21c-192">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa21c-193">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-193">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="fa21c-195">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-195">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="fa21c-197">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fa21c-197">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="fa21c-199">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fa21c-199">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-zoho-mail-tutorial/create_aaduser_04.png)

    <span data-ttu-id="fa21c-201">a.</span><span class="sxs-lookup"><span data-stu-id="fa21c-201">a.</span></span> <span data-ttu-id="fa21c-202">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-202">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fa21c-203">b.</span><span class="sxs-lookup"><span data-stu-id="fa21c-203">b.</span></span> <span data-ttu-id="fa21c-204">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fa21c-204">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="fa21c-205">c.</span><span class="sxs-lookup"><span data-stu-id="fa21c-205">c.</span></span> <span data-ttu-id="fa21c-206">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="fa21c-206">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="fa21c-207">d.</span><span class="sxs-lookup"><span data-stu-id="fa21c-207">d.</span></span> <span data-ttu-id="fa21c-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-208">Click **Create**.</span></span>
 
### <a name="create-a-zoho-test-user"></a><span data-ttu-id="fa21c-209">Skapa en testanvändare Zoho</span><span class="sxs-lookup"><span data-stu-id="fa21c-209">Create a Zoho test user</span></span>

<span data-ttu-id="fa21c-210">I ordning tooenable Azure AD-användare toolog till Zoho e-post, måste de etableras i Zoho e-post.</span><span class="sxs-lookup"><span data-stu-id="fa21c-210">In order tooenable Azure AD users toolog into Zoho Mail, they must be provisioned into Zoho Mail.</span></span> <span data-ttu-id="fa21c-211">Etablering är en manuell aktivitet i hello fallet Zoho e-post.</span><span class="sxs-lookup"><span data-stu-id="fa21c-211">In hello case of Zoho Mail, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="fa21c-212">Du kan använda något annat Zoho e användarens konto skapas verktyg eller API: er som tillhandahålls av Zoho e tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="fa21c-212">You can use any other Zoho Mail user account creation tools or APIs provided by Zoho Mail tooprovision AAD user accounts.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="fa21c-213">tooprovision ett användarkonto, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="fa21c-213">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="fa21c-214">Logga in tooyour **Zoho e** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="fa21c-214">Log in tooyour **Zoho Mail** company site as an administrator.</span></span>

2. <span data-ttu-id="fa21c-215">Gå för**Kontrollpanelen \> e-post och dokument**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-215">Go too**Control Panel \> Mail & Docs**.</span></span>

3. <span data-ttu-id="fa21c-216">Gå för**användarinformation \> Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-216">Go too**User Details \> Add User**.</span></span>
   
    <span data-ttu-id="fa21c-217">![Lägg till användare](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="fa21c-217">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789611.png "Add User")</span></span>

4. <span data-ttu-id="fa21c-218">På hello **lägga till användare** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="fa21c-218">On hello **Add users** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="fa21c-219">![Lägg till användare](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="fa21c-219">![Add User](./media/active-directory-saas-zoho-mail-tutorial/ic789612.png "Add User")</span></span>
   
    <span data-ttu-id="fa21c-220">a.</span><span class="sxs-lookup"><span data-stu-id="fa21c-220">a.</span></span> <span data-ttu-id="fa21c-221">I hello **Förnamn** textruta hello första-typnamn för användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-221">In hello **First Name** textbox, type hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="fa21c-222">b.</span><span class="sxs-lookup"><span data-stu-id="fa21c-222">b.</span></span> <span data-ttu-id="fa21c-223">I hello **efternamn** textruta Skriv hello efternamn för användaren som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-223">In hello **Last Name** textbox, type hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="fa21c-224">c.</span><span class="sxs-lookup"><span data-stu-id="fa21c-224">c.</span></span> <span data-ttu-id="fa21c-225">I hello **e-post-ID** textruta hello e-post-id för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="fa21c-225">In hello **Email ID** textbox, type hello email id of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="fa21c-226">d.</span><span class="sxs-lookup"><span data-stu-id="fa21c-226">d.</span></span> <span data-ttu-id="fa21c-227">I hello **lösenord** textruta ange lösenord för användaren.</span><span class="sxs-lookup"><span data-stu-id="fa21c-227">In hello **Password** textbox, enter password of user.</span></span>
   
    <span data-ttu-id="fa21c-228">e.</span><span class="sxs-lookup"><span data-stu-id="fa21c-228">e.</span></span> <span data-ttu-id="fa21c-229">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-229">Click **OK**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="fa21c-230">hello Azure Active Directory användare får ett e-postmeddelande med en länk tooconfirm hello-konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="fa21c-230">hello Azure Active Directory account holder will receive an email with a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="fa21c-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="fa21c-231">Assign hello Azure AD test user</span></span>

<span data-ttu-id="fa21c-232">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooZoho.</span><span class="sxs-lookup"><span data-stu-id="fa21c-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZoho.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="fa21c-234">**tooassign Britta Simon tooZoho utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="fa21c-234">**tooassign Britta Simon tooZoho, perform hello following steps:**</span></span>

1. <span data-ttu-id="fa21c-235">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="fa21c-237">Välj i listan med program hello **Zoho**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-237">In hello applications list, select **Zoho**.</span></span>

    ![Hej Zoho länken i listan med program hello](./media/active-directory-saas-zoho-mail-tutorial/tutorial_zoho_app.png)  

3. <span data-ttu-id="fa21c-239">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="fa21c-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="fa21c-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-241">Click **Add** button.</span></span> <span data-ttu-id="fa21c-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fa21c-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="fa21c-244">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="fa21c-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="fa21c-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fa21c-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fa21c-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="fa21c-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fa21c-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="fa21c-247">Test single sign-on</span></span>

<span data-ttu-id="fa21c-248">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-248">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="fa21c-249">Du bör få automatiskt inloggade tooyour Zoho programmet när du klickar på hello Zoho panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="fa21c-249">When you click hello Zoho tile in hello Access Panel, you should get automatically signed-on tooyour Zoho application.</span></span>
<span data-ttu-id="fa21c-250">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fa21c-250">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fa21c-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="fa21c-251">Additional resources</span></span>

* [<span data-ttu-id="fa21c-252">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fa21c-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fa21c-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="fa21c-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoho-mail-tutorial/tutorial_general_203.png

