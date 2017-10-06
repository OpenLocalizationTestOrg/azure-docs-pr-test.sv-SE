---
title: "Självstudier: Azure Active Directory-integrering med Evernote | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Evernote."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 4d7017e571ed12a0b155aa188c6b0ecb3c9898a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a><span data-ttu-id="1c23e-103">Självstudier: Azure Active Directory-integrering med Evernote</span><span class="sxs-lookup"><span data-stu-id="1c23e-103">Tutorial: Azure Active Directory integration with Evernote</span></span>

<span data-ttu-id="1c23e-104">I kursen får du lära dig hur toointegrate Evernote med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1c23e-104">In this tutorial, you learn how toointegrate Evernote with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1c23e-105">Integrera Evernote med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1c23e-105">Integrating Evernote with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1c23e-106">Du kan styra i Azure AD som har åtkomst till tooEvernote.</span><span class="sxs-lookup"><span data-stu-id="1c23e-106">You can control in Azure AD who has access tooEvernote.</span></span>
- <span data-ttu-id="1c23e-107">Du kan låta dina användare tooautomatically get inloggade tooEvernote (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="1c23e-107">You can enable your users tooautomatically get signed-on tooEvernote (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="1c23e-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="1c23e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1c23e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c23e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1c23e-110">Prerequisites</span></span>

<span data-ttu-id="1c23e-111">tooconfigure Azure AD-integrering med Evernote, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1c23e-111">tooconfigure Azure AD integration with Evernote, you need hello following items:</span></span>

- <span data-ttu-id="1c23e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1c23e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1c23e-113">En Evernote enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1c23e-113">An Evernote single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1c23e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1c23e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1c23e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1c23e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1c23e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1c23e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1c23e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1c23e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1c23e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1c23e-118">Scenario description</span></span>
<span data-ttu-id="1c23e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1c23e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1c23e-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1c23e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1c23e-121">Att lägga till Evernote från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1c23e-121">Adding Evernote from hello gallery</span></span>
2. <span data-ttu-id="1c23e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c23e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evernote-from-hello-gallery"></a><span data-ttu-id="1c23e-123">Att lägga till Evernote från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1c23e-123">Adding Evernote from hello gallery</span></span>
<span data-ttu-id="1c23e-124">tooconfigure hello integrering av Evernote i Azure AD, behöver du tooadd Evernote hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="1c23e-124">tooconfigure hello integration of Evernote into Azure AD, you need tooadd Evernote from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1c23e-125">**tooadd Evernote från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1c23e-125">**tooadd Evernote from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c23e-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1c23e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="1c23e-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1c23e-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="1c23e-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="1c23e-133">Skriv i sökrutan hello **Evernote**väljer **Evernote** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="1c23e-133">In hello search box, type **Evernote**, select **Evernote** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Evernote i hello resultatlistan](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="1c23e-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c23e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="1c23e-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Evernote baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1c23e-136">In this section, you configure and test Azure AD single sign-on with Evernote based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1c23e-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Evernote är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1c23e-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Evernote is tooa user in Azure AD.</span></span> <span data-ttu-id="1c23e-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Evernote toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="1c23e-138">In other words, a link relationship between an Azure AD user and hello related user in Evernote needs toobe established.</span></span>

<span data-ttu-id="1c23e-139">I Evernote, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-139">In Evernote, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1c23e-140">tooconfigure och testa Azure AD enkel inloggning med Evernote, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1c23e-140">tooconfigure and test Azure AD single sign-on with Evernote, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1c23e-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1c23e-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1c23e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1c23e-143">**[Skapa en testanvändare Evernote](#create-an-evernote-test-user)**  -toohave en motsvarighet för Britta Simon i Evernote som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1c23e-143">**[Create an Evernote test user](#create-an-evernote-test-user)** - toohave a counterpart of Britta Simon in Evernote that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1c23e-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c23e-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1c23e-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1c23e-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="1c23e-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c23e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="1c23e-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Evernote program.</span><span class="sxs-lookup"><span data-stu-id="1c23e-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Evernote application.</span></span>

<span data-ttu-id="1c23e-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Evernote:**</span><span class="sxs-lookup"><span data-stu-id="1c23e-148">**tooconfigure Azure AD single sign-on with Evernote, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c23e-149">I hello Azure-portalen på hello **Evernote** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-149">In hello Azure portal, on hello **Evernote** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="1c23e-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c23e-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. <span data-ttu-id="1c23e-153">På hello **Evernote domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet i IDP initierade läge hello:</span><span class="sxs-lookup"><span data-stu-id="1c23e-153">On hello **Evernote Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in IDP initiated mode:</span></span>

    ![URL: er och Evernote domän med enkel inloggning information](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    <span data-ttu-id="1c23e-155">I hello **identifierare** textruta typen hello URL:`https://www.evernote.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="1c23e-155">In hello **Identifier** textbox, type hello URL: `https://www.evernote.com/saml2`</span></span>

4. <span data-ttu-id="1c23e-156">Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="1c23e-156">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![URL: er och Evernote domän med enkel inloggning information](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    <span data-ttu-id="1c23e-158">I hello **inloggning URL** textruta typen hello URL:`https://www.evernote.com/Login.action`</span><span class="sxs-lookup"><span data-stu-id="1c23e-158">In hello **Sign on URL** textbox, type hello URL: `https://www.evernote.com/Login.action`</span></span>   

5. <span data-ttu-id="1c23e-159">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="1c23e-159">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. <span data-ttu-id="1c23e-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="1c23e-163">På hello **Evernote Configuration** klickar du på **konfigurera Evernote** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="1c23e-163">On hello **Evernote Configuration** section, click **Configure Evernote** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="1c23e-164">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="1c23e-164">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Evernote konfiguration](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. <span data-ttu-id="1c23e-166">Logga in på webbplatsen Evernote företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="1c23e-166">In a different web browser window, log into your Evernote company site as an administrator.</span></span>

9. <span data-ttu-id="1c23e-167">Gå för**Admin Console**</span><span class="sxs-lookup"><span data-stu-id="1c23e-167">Go too**'Admin Console'**</span></span>

    ![Administrationskonsolen](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. <span data-ttu-id="1c23e-169">Från hello **Admin Console**, gå för**”säkerhet”** och välj **' Single Sign-On-**</span><span class="sxs-lookup"><span data-stu-id="1c23e-169">From hello **'Admin Console'**, go too**‘Security’** and select **‘Single Sign-On’**</span></span>

    ![SSO-inställning](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. <span data-ttu-id="1c23e-171">Konfigurera hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="1c23e-171">Configure hello following values:</span></span>

    ![Certifikat-inställning](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    <span data-ttu-id="1c23e-173">a.</span><span class="sxs-lookup"><span data-stu-id="1c23e-173">a.</span></span>  <span data-ttu-id="1c23e-174">**Aktivera enkel inloggning:** enkel inloggning är aktiverad som standard (klicka på **inaktivera enkel inloggning** tooremove hello SSO krav)</span><span class="sxs-lookup"><span data-stu-id="1c23e-174">**Enable SSO:** SSO is enabled by default (Click **Disable Single Sign-on** tooremove hello SSO requirement)</span></span>

    <span data-ttu-id="1c23e-175">b.</span><span class="sxs-lookup"><span data-stu-id="1c23e-175">b.</span></span> <span data-ttu-id="1c23e-176">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **SAML HTTP-begäran URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="1c23e-176">Paste **SAML Single sign-on Service URL** value, which you have copied from hello Azure portal into hello **SAML HTTP Request URL** textbox.</span></span>

    <span data-ttu-id="1c23e-177">c.</span><span class="sxs-lookup"><span data-stu-id="1c23e-177">c.</span></span> <span data-ttu-id="1c23e-178">Öppna hello hämtat certifikat från Azure AD i en anteckningar och kopiera hello innehåll inklusive ”BEGIN CERTIFICATE” och ”END CERTIFICATE” och klistra in den i hello **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="1c23e-178">Open hello downloaded certificate from Azure AD in a notepad and copy hello content including "BEGIN CERTIFICATE" and "END CERTIFICATE" and paste it into hello **X.509 Certificate** textbox.</span></span> 

    <span data-ttu-id="1c23e-179">d.Click **spara ändringar**</span><span class="sxs-lookup"><span data-stu-id="1c23e-179">d.Click **Save Changes**</span></span>

> [!TIP]
> <span data-ttu-id="1c23e-180">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="1c23e-180">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1c23e-181">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="1c23e-181">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1c23e-182">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1c23e-182">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="1c23e-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1c23e-183">Create an Azure AD test user</span></span>

<span data-ttu-id="1c23e-184">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1c23e-184">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="1c23e-186">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1c23e-186">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c23e-187">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-187">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="1c23e-189">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-189">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="1c23e-191">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-191">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="1c23e-193">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1c23e-193">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    <span data-ttu-id="1c23e-195">a.</span><span class="sxs-lookup"><span data-stu-id="1c23e-195">a.</span></span> <span data-ttu-id="1c23e-196">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-196">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1c23e-197">b.</span><span class="sxs-lookup"><span data-stu-id="1c23e-197">b.</span></span> <span data-ttu-id="1c23e-198">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1c23e-198">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="1c23e-199">c.</span><span class="sxs-lookup"><span data-stu-id="1c23e-199">c.</span></span> <span data-ttu-id="1c23e-200">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-200">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="1c23e-201">d.</span><span class="sxs-lookup"><span data-stu-id="1c23e-201">d.</span></span> <span data-ttu-id="1c23e-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-202">Click **Create**.</span></span>
 
### <a name="create-an-evernote-test-user"></a><span data-ttu-id="1c23e-203">Skapa en testanvändare Evernote</span><span class="sxs-lookup"><span data-stu-id="1c23e-203">Create an Evernote test user</span></span>

<span data-ttu-id="1c23e-204">I ordning tooenable Azure AD-användare toolog i Evernote, måste de etableras i Evernote.</span><span class="sxs-lookup"><span data-stu-id="1c23e-204">In order tooenable Azure AD users toolog into Evernote, they must be provisioned into Evernote.</span></span>  
<span data-ttu-id="1c23e-205">Hello gäller Evernote är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1c23e-205">In hello case of Evernote, provisioning is a manual task.</span></span>

<span data-ttu-id="1c23e-206">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1c23e-206">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c23e-207">Logga in tooyour Evernote företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="1c23e-207">Log in tooyour Evernote company site as an administrator.</span></span>

2. <span data-ttu-id="1c23e-208">Klicka på hello **Admin Console**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-208">Click hello **'Admin Console'**.</span></span>

    ![Administrationskonsolen](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. <span data-ttu-id="1c23e-210">Från hello **Admin Console**, gå för**'Lägg till användare ”**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-210">From hello **'Admin Console'**, go too**‘Add users’**.</span></span>

    ![Lägg till testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. <span data-ttu-id="1c23e-212">**Lägg till teammedlemmar** i hello **e-post** textruta Skriv hello e-postadressen för användarkontot och klicka på **bjuda in.**</span><span class="sxs-lookup"><span data-stu-id="1c23e-212">**Add team members** in hello **Email** textbox, type hello email address of user account and click **Invite.**</span></span>

    ![Lägg till testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. <span data-ttu-id="1c23e-214">När inbjudan har skickats får hello Azure Active Directory användare ett e-tooaccept hello inbjudan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-214">After invitation is sent, hello Azure Active Directory account holder will receive an email tooaccept hello invitation.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="1c23e-215">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="1c23e-215">Assign hello Azure AD test user</span></span>

<span data-ttu-id="1c23e-216">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooEvernote.</span><span class="sxs-lookup"><span data-stu-id="1c23e-216">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooEvernote.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="1c23e-218">**tooassign Britta Simon tooEvernote utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1c23e-218">**tooassign Britta Simon tooEvernote, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c23e-219">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-219">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1c23e-221">Välj i listan med program hello **Evernote**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-221">In hello applications list, select **Evernote**.</span></span>

    ![Hej Evernote länken i listan med program hello](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. <span data-ttu-id="1c23e-223">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1c23e-223">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="1c23e-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-225">Click **Add** button.</span></span> <span data-ttu-id="1c23e-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="1c23e-228">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-228">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1c23e-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1c23e-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c23e-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="1c23e-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c23e-231">Test single sign-on</span></span>

<span data-ttu-id="1c23e-232">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-232">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1c23e-233">Du bör få inloggade tooyour Evernote programmet när du klickar på hello Evernote panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1c23e-233">When you click hello Evernote tile in hello Access Panel, you should get signed-on tooyour Evernote application.</span></span> <span data-ttu-id="1c23e-234">Du måste logga in som en organisationskonto men måste toolog in med ditt eget konto.</span><span class="sxs-lookup"><span data-stu-id="1c23e-234">You'll be logging in as an Organization account but then need toolog in with your personal account.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1c23e-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1c23e-235">Additional resources</span></span>

* [<span data-ttu-id="1c23e-236">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1c23e-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1c23e-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1c23e-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

