---
title: "Självstudier: Azure Active Directory-integrering med Envoy | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Envoy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71f7afcc-1033-4098-9b7e-4f9f2b26f734
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 93add7c1f3cf1fc163acc505f11e34bd696c571c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-envoy"></a><span data-ttu-id="79ebd-103">Självstudier: Azure Active Directory-integrering med Envoy</span><span class="sxs-lookup"><span data-stu-id="79ebd-103">Tutorial: Azure Active Directory integration with Envoy</span></span>

<span data-ttu-id="79ebd-104">I kursen får du lära dig hur toointegrate Envoy med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="79ebd-104">In this tutorial, you learn how toointegrate Envoy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="79ebd-105">Integrera Envoy med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="79ebd-105">Integrating Envoy with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="79ebd-106">Du kan styra i Azure AD som har åtkomst till tooEnvoy.</span><span class="sxs-lookup"><span data-stu-id="79ebd-106">You can control in Azure AD who has access tooEnvoy.</span></span>
- <span data-ttu-id="79ebd-107">Du kan låta dina användare tooautomatically get inloggade tooEnvoy (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="79ebd-107">You can enable your users tooautomatically get signed-on tooEnvoy (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="79ebd-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="79ebd-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="79ebd-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79ebd-110">Krav</span><span class="sxs-lookup"><span data-stu-id="79ebd-110">Prerequisites</span></span>

<span data-ttu-id="79ebd-111">tooconfigure Azure AD-integrering med Envoy måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="79ebd-111">tooconfigure Azure AD integration with Envoy, you need hello following items:</span></span>

- <span data-ttu-id="79ebd-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="79ebd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="79ebd-113">En Envoy enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="79ebd-113">An Envoy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="79ebd-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="79ebd-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="79ebd-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="79ebd-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="79ebd-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="79ebd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="79ebd-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="79ebd-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="79ebd-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="79ebd-118">Scenario description</span></span>
<span data-ttu-id="79ebd-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="79ebd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="79ebd-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="79ebd-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="79ebd-121">Att lägga till Envoy från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="79ebd-121">Adding Envoy from hello gallery</span></span>
2. <span data-ttu-id="79ebd-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79ebd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-envoy-from-hello-gallery"></a><span data-ttu-id="79ebd-123">Att lägga till Envoy från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="79ebd-123">Adding Envoy from hello gallery</span></span>
<span data-ttu-id="79ebd-124">tooconfigure hello integrering av Envoy i Azure AD, behöver du tooadd Envoy hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="79ebd-124">tooconfigure hello integration of Envoy into Azure AD, you need tooadd Envoy from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="79ebd-125">**tooadd Envoy från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="79ebd-125">**tooadd Envoy from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="79ebd-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="79ebd-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="79ebd-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="79ebd-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="79ebd-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79ebd-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="79ebd-133">Skriv i sökrutan hello **Envoy**väljer **Envoy** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="79ebd-133">In hello search box, type **Envoy**, select **Envoy** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Envoy i hello resultatlistan](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="79ebd-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79ebd-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="79ebd-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Envoy baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="79ebd-136">In this section, you configure and test Azure AD single sign-on with Envoy based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="79ebd-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Envoy är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79ebd-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Envoy is tooa user in Azure AD.</span></span> <span data-ttu-id="79ebd-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Envoy toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="79ebd-138">In other words, a link relationship between an Azure AD user and hello related user in Envoy needs toobe established.</span></span>

<span data-ttu-id="79ebd-139">I Envoy, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-139">In Envoy, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="79ebd-140">tooconfigure och testa Azure AD enkel inloggning med Envoy, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="79ebd-140">tooconfigure and test Azure AD single sign-on with Envoy, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="79ebd-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="79ebd-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79ebd-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="79ebd-143">**[Skapa en testanvändare Envoy](#create-an-envoy-test-user)**  -toohave en motsvarighet för Britta Simon i Envoy som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="79ebd-143">**[Create an Envoy test user](#create-an-envoy-test-user)** - toohave a counterpart of Britta Simon in Envoy that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="79ebd-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="79ebd-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="79ebd-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="79ebd-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="79ebd-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79ebd-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="79ebd-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Envoy.</span><span class="sxs-lookup"><span data-stu-id="79ebd-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Envoy application.</span></span>

<span data-ttu-id="79ebd-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Envoy:**</span><span class="sxs-lookup"><span data-stu-id="79ebd-148">**tooconfigure Azure AD single sign-on with Envoy, perform hello following steps:**</span></span>

1. <span data-ttu-id="79ebd-149">I hello Azure-portalen på hello **Envoy** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-149">In hello Azure portal, on hello **Envoy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="79ebd-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="79ebd-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_samlbase.png)

3. <span data-ttu-id="79ebd-153">På hello **Envoy domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="79ebd-153">On hello **Envoy Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Envoy domän med enkel inloggning information](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_url.png)

    <span data-ttu-id="79ebd-155">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.Envoy.com`</span><span class="sxs-lookup"><span data-stu-id="79ebd-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.Envoy.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="79ebd-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="79ebd-156">This value is not real.</span></span> <span data-ttu-id="79ebd-157">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="79ebd-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="79ebd-158">Kontakta [Envoy klienten supportteamet](https://envoy.com/contact/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="79ebd-158">Contact [Envoy Client support team](https://envoy.com/contact/) tooget this value.</span></span>

4. <span data-ttu-id="79ebd-159">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikat...</span><span class="sxs-lookup"><span data-stu-id="79ebd-159">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate..</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_certificate.png) 

5. <span data-ttu-id="79ebd-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-envoy-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="79ebd-163">På hello **Envoy Configuration** klickar du på **konfigurera Envoy** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="79ebd-163">On hello **Envoy Configuration** section, click **Configure Envoy** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="79ebd-164">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="79ebd-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Envoy konfiguration](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_configure.png)

7. <span data-ttu-id="79ebd-166">Logga in på webbplatsen Envoy företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="79ebd-166">In a different web browser window, log into your Envoy company site as an administrator.</span></span>

8. <span data-ttu-id="79ebd-167">Klicka i hello verktygsfältet hello längst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-167">In hello toolbar on hello top, click **Settings**.</span></span>

    <span data-ttu-id="79ebd-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span><span class="sxs-lookup"><span data-stu-id="79ebd-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span></span>

9. <span data-ttu-id="79ebd-169">Klicka på **företagets**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-169">Click **Company**.</span></span>

    <span data-ttu-id="79ebd-170">![Företagets](./media/active-directory-saas-envoy-tutorial/ic776783.png "företag")</span><span class="sxs-lookup"><span data-stu-id="79ebd-170">![Company](./media/active-directory-saas-envoy-tutorial/ic776783.png "Company")</span></span>

10. <span data-ttu-id="79ebd-171">Klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-171">Click **SAML**.</span></span>

    <span data-ttu-id="79ebd-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="79ebd-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span></span>

11. <span data-ttu-id="79ebd-173">I hello **SAML-autentisering** konfigurationen och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="79ebd-173">In hello **SAML Authentication** configuration section, perform hello following steps:</span></span>

    <span data-ttu-id="79ebd-174">![SAML-autentisering](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="79ebd-174">![SAML authentication](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML authentication")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="79ebd-175">hello-värdet för hello HQ plats-ID är genereras av hello program automatiskt.</span><span class="sxs-lookup"><span data-stu-id="79ebd-175">hello value for hello HQ location ID is auto generated by hello application.</span></span>
    
    <span data-ttu-id="79ebd-176">a.</span><span class="sxs-lookup"><span data-stu-id="79ebd-176">a.</span></span> <span data-ttu-id="79ebd-177">I **fingeravtryck** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-177">In **Fingerprint** textbox, paste hello **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="79ebd-178">b.</span><span class="sxs-lookup"><span data-stu-id="79ebd-178">b.</span></span> <span data-ttu-id="79ebd-179">Klistra in **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat formuläret hello Azure portal till hello **identitet HTTP-URL-PROVIDERN SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="79ebd-179">Paste **SAML Single Sign-On Service URL** value, which you have copied form hello Azure portal into hello **IDENTITY PROVIDER HTTP SAML URL** textbox.</span></span>
    
    <span data-ttu-id="79ebd-180">c.</span><span class="sxs-lookup"><span data-stu-id="79ebd-180">c.</span></span> <span data-ttu-id="79ebd-181">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-181">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="79ebd-182">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="79ebd-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="79ebd-183">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="79ebd-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="79ebd-184">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="79ebd-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="79ebd-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="79ebd-185">Create an Azure AD test user</span></span>

<span data-ttu-id="79ebd-186">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79ebd-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="79ebd-188">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="79ebd-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="79ebd-189">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-189">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-envoy-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="79ebd-191">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-191">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-envoy-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="79ebd-193">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79ebd-193">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-envoy-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="79ebd-195">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="79ebd-195">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-envoy-tutorial/create_aaduser_04.png)

    <span data-ttu-id="79ebd-197">a.</span><span class="sxs-lookup"><span data-stu-id="79ebd-197">a.</span></span> <span data-ttu-id="79ebd-198">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-198">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="79ebd-199">b.</span><span class="sxs-lookup"><span data-stu-id="79ebd-199">b.</span></span> <span data-ttu-id="79ebd-200">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79ebd-200">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="79ebd-201">c.</span><span class="sxs-lookup"><span data-stu-id="79ebd-201">c.</span></span> <span data-ttu-id="79ebd-202">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="79ebd-202">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="79ebd-203">d.</span><span class="sxs-lookup"><span data-stu-id="79ebd-203">d.</span></span> <span data-ttu-id="79ebd-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-204">Click **Create**.</span></span>
 
### <a name="create-an-envoy-test-user"></a><span data-ttu-id="79ebd-205">Skapa en testanvändare Envoy</span><span class="sxs-lookup"><span data-stu-id="79ebd-205">Create an Envoy test user</span></span>

<span data-ttu-id="79ebd-206">Det finns inga uppgiften för du tooconfigure användaretablering tooEnvoy.</span><span class="sxs-lookup"><span data-stu-id="79ebd-206">There is no action item for you tooconfigure user provisioning tooEnvoy.</span></span> <span data-ttu-id="79ebd-207">När en tilldelad användare försöker toolog till Envoy hello åtkomst panelen, kontrollerar Envoy om hello användaren finns.</span><span class="sxs-lookup"><span data-stu-id="79ebd-207">When an assigned user tries toolog into Envoy using hello access panel, Envoy checks whether hello user exists.</span></span> <span data-ttu-id="79ebd-208">Om det finns inget användarkonto ännu, skapas den automatiskt av Envoy.</span><span class="sxs-lookup"><span data-stu-id="79ebd-208">If there is no user account available yet, it is automatically created by Envoy.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="79ebd-209">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="79ebd-209">Assign hello Azure AD test user</span></span>

<span data-ttu-id="79ebd-210">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEnvoy.</span><span class="sxs-lookup"><span data-stu-id="79ebd-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEnvoy.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="79ebd-212">**tooassign Britta Simon tooEnvoy utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="79ebd-212">**tooassign Britta Simon tooEnvoy, perform hello following steps:**</span></span>

1. <span data-ttu-id="79ebd-213">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="79ebd-215">Välj i listan med program hello **Envoy**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-215">In hello applications list, select **Envoy**.</span></span>

    ![hello Envoy länken i listan med program hello](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_app.png)  

3. <span data-ttu-id="79ebd-217">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="79ebd-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="79ebd-219">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-219">Click **Add** button.</span></span> <span data-ttu-id="79ebd-220">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79ebd-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="79ebd-222">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="79ebd-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="79ebd-223">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79ebd-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="79ebd-224">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79ebd-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="79ebd-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79ebd-225">Test single sign-on</span></span>

<span data-ttu-id="79ebd-226">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="79ebd-227">Du bör få automatiskt inloggade tooyour Envoy programmet när du klickar på hello Envoy panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="79ebd-227">When you click hello Envoy tile in hello Access Panel, you should get automatically signed-on tooyour Envoy application.</span></span>
<span data-ttu-id="79ebd-228">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="79ebd-228">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="79ebd-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="79ebd-229">Additional resources</span></span>

* [<span data-ttu-id="79ebd-230">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79ebd-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="79ebd-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="79ebd-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_203.png

