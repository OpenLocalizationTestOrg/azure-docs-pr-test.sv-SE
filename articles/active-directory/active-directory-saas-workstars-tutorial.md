---
title: "Självstudier: Azure Active Directory-integrering med Workstars | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Workstars."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 86250d7538f058d2a821ded7dda0b2fc185d80df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a><span data-ttu-id="4a779-103">Självstudier: Azure Active Directory-integrering med Workstars</span><span class="sxs-lookup"><span data-stu-id="4a779-103">Tutorial: Azure Active Directory integration with Workstars</span></span>

<span data-ttu-id="4a779-104">I kursen får du lära dig hur toointegrate Workstars med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4a779-104">In this tutorial, you learn how toointegrate Workstars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4a779-105">Integrera Workstars med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4a779-105">Integrating Workstars with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4a779-106">Du kan styra i Azure AD som har åtkomst till tooWorkstars.</span><span class="sxs-lookup"><span data-stu-id="4a779-106">You can control in Azure AD who has access tooWorkstars.</span></span>
- <span data-ttu-id="4a779-107">Du kan låta dina användare tooautomatically get inloggade tooWorkstars (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="4a779-107">You can enable your users tooautomatically get signed-on tooWorkstars (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="4a779-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a779-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="4a779-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4a779-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4a779-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4a779-110">Prerequisites</span></span>

<span data-ttu-id="4a779-111">tooconfigure Azure AD-integrering med Workstars, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4a779-111">tooconfigure Azure AD integration with Workstars, you need hello following items:</span></span>

- <span data-ttu-id="4a779-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4a779-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4a779-113">En Workstars enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4a779-113">A Workstars single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4a779-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4a779-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4a779-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4a779-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4a779-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4a779-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4a779-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a779-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4a779-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4a779-118">Scenario description</span></span>
<span data-ttu-id="4a779-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4a779-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4a779-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4a779-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4a779-121">Att lägga till Workstars från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4a779-121">Adding Workstars from hello gallery</span></span>
2. <span data-ttu-id="4a779-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4a779-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workstars-from-hello-gallery"></a><span data-ttu-id="4a779-123">Att lägga till Workstars från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4a779-123">Adding Workstars from hello gallery</span></span>
<span data-ttu-id="4a779-124">tooconfigure hello integrering av Workstars i Azure AD, behöver du tooadd Workstars hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4a779-124">tooconfigure hello integration of Workstars into Azure AD, you need tooadd Workstars from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4a779-125">**tooadd Workstars från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4a779-125">**tooadd Workstars from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a779-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4a779-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="4a779-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4a779-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4a779-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4a779-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="4a779-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4a779-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="4a779-133">Skriv i sökrutan hello **Workstars**väljer **Workstars** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4a779-133">In hello search box, type **Workstars**, select **Workstars** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Workstars i hello resultatlistan](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4a779-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4a779-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="4a779-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workstars baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4a779-136">In this section, you configure and test Azure AD single sign-on with Workstars based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4a779-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Workstars är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4a779-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Workstars is tooa user in Azure AD.</span></span> <span data-ttu-id="4a779-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Workstars toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4a779-138">In other words, a link relationship between an Azure AD user and hello related user in Workstars needs toobe established.</span></span>

<span data-ttu-id="4a779-139">I Workstars, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4a779-139">In Workstars, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4a779-140">tooconfigure och testa Azure AD enkel inloggning med Workstars, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4a779-140">tooconfigure and test Azure AD single sign-on with Workstars, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4a779-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4a779-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4a779-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a779-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4a779-143">**[Skapa en testanvändare Workstars](#create-a-workstars-test-user)**  -toohave en motsvarighet för Britta Simon i Workstars som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4a779-143">**[Create a Workstars test user](#create-a-workstars-test-user)** - toohave a counterpart of Britta Simon in Workstars that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4a779-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a779-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4a779-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4a779-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="4a779-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4a779-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="4a779-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Workstars program.</span><span class="sxs-lookup"><span data-stu-id="4a779-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Workstars application.</span></span>

<span data-ttu-id="4a779-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Workstars:**</span><span class="sxs-lookup"><span data-stu-id="4a779-148">**tooconfigure Azure AD single sign-on with Workstars, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a779-149">I hello Azure-portalen på hello **Workstars** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4a779-149">In hello Azure portal, on hello **Workstars** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="4a779-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4a779-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_samlbase.png)

3. <span data-ttu-id="4a779-153">På hello **Workstars domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4a779-153">On hello **Workstars Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Workstars domän med enkel inloggning information](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_url.png)

    <span data-ttu-id="4a779-155">a.</span><span class="sxs-lookup"><span data-stu-id="4a779-155">a.</span></span> <span data-ttu-id="4a779-156">I hello **identifierare** textruta typen hello URL:`https://workstars.com`</span><span class="sxs-lookup"><span data-stu-id="4a779-156">In hello **Identifier** textbox, type hello URL: `https://workstars.com`</span></span>

    <span data-ttu-id="4a779-157">b.</span><span class="sxs-lookup"><span data-stu-id="4a779-157">b.</span></span> <span data-ttu-id="4a779-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.workstars.com/saml/login_check`</span><span class="sxs-lookup"><span data-stu-id="4a779-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<subdomain>.workstars.com/saml/login_check`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4a779-159">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4a779-159">hello value is not real.</span></span> <span data-ttu-id="4a779-160">Hello uppdateringsvärde med hello faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="4a779-160">Update hello value with hello actual Reply URL.</span></span> <span data-ttu-id="4a779-161">Kontakta [Workstars supportteam](https://support.workstars.com) tooget hello värde.</span><span class="sxs-lookup"><span data-stu-id="4a779-161">Contact [Workstars support team](https://support.workstars.com) tooget hello value.</span></span>
 
4. <span data-ttu-id="4a779-162">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="4a779-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_certificate.png) 

5. <span data-ttu-id="4a779-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4a779-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-workstars-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4a779-166">På hello **Workstars Configuration** klickar du på **konfigurera Workstars** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4a779-166">On hello **Workstars Configuration** section, click **Configure Workstars** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4a779-167">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4a779-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Workstars konfiguration](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_configure.png) 

7. <span data-ttu-id="4a779-169">Logga in tooyour Workstars företagets webbplats som en administratör i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="4a779-169">In another browser window, sign on tooyour Workstars company site as an administrator.</span></span>

8. <span data-ttu-id="4a779-170">I hello verktygsfältet klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="4a779-170">In hello main toolbar, click **Settings**.</span></span>

    ![Workstars sett](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_sett.png)

9. <span data-ttu-id="4a779-172">Gå för**inloggning** > **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="4a779-172">Go too**Sign On** > **Settings**.</span></span>

    ![Workstars inloggning](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_signon.png)

    ![Workstars inställningar](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_settings.png)

10. <span data-ttu-id="4a779-175">På hello **enkel inloggning på (SAML) - inställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4a779-175">On hello **Single Sign On (SAML) - Settings** page, perform hello following steps:</span></span>
    
    ![Workstars saml](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_saml.png)

    <span data-ttu-id="4a779-177">a.</span><span class="sxs-lookup"><span data-stu-id="4a779-177">a.</span></span> <span data-ttu-id="4a779-178">I **identitet providernamn** textruta typen **Office 365**.</span><span class="sxs-lookup"><span data-stu-id="4a779-178">In **Identity Provider Name** textbox, type **Office 365**.</span></span>

    <span data-ttu-id="4a779-179">b.</span><span class="sxs-lookup"><span data-stu-id="4a779-179">b.</span></span> <span data-ttu-id="4a779-180">I hello **identitet providern enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a779-180">In hello **Identity Provider Entity ID** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="4a779-181">c.</span><span class="sxs-lookup"><span data-stu-id="4a779-181">c.</span></span> <span data-ttu-id="4a779-182">Kopiera hello innehållet hello hämtat certifikatfilen i anteckningar och klistra in den i hello **x509 certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="4a779-182">Copy hello content of hello downloaded certificate file in notepad, and then paste it into hello **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="4a779-183">d.</span><span class="sxs-lookup"><span data-stu-id="4a779-183">d.</span></span> <span data-ttu-id="4a779-184">I hello **SAML SSO URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a779-184">In hello **SAML SSO URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="4a779-185">e.</span><span class="sxs-lookup"><span data-stu-id="4a779-185">e.</span></span> <span data-ttu-id="4a779-186">I hello **Remote logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4a779-186">In hello **Remote Logout URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="4a779-187">f.</span><span class="sxs-lookup"><span data-stu-id="4a779-187">f.</span></span> <span data-ttu-id="4a779-188">Välj **namn-ID** som **e-post (standard)**.</span><span class="sxs-lookup"><span data-stu-id="4a779-188">select **Name ID** as **Email (Default)**.</span></span>

    <span data-ttu-id="4a779-189">g.</span><span class="sxs-lookup"><span data-stu-id="4a779-189">g.</span></span> <span data-ttu-id="4a779-190">Klicka på **Bekräfta**.</span><span class="sxs-lookup"><span data-stu-id="4a779-190">Click **Confirm**.</span></span>
    
> [!TIP]
> <span data-ttu-id="4a779-191">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4a779-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4a779-192">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4a779-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4a779-193">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4a779-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4a779-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a779-194">Create an Azure AD test user</span></span>

<span data-ttu-id="4a779-195">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a779-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="4a779-197">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4a779-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a779-198">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="4a779-198">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-workstars-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="4a779-200">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4a779-200">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-workstars-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="4a779-202">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4a779-202">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-workstars-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="4a779-204">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4a779-204">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-workstars-tutorial/create_aaduser_04.png)

    <span data-ttu-id="4a779-206">a.</span><span class="sxs-lookup"><span data-stu-id="4a779-206">a.</span></span> <span data-ttu-id="4a779-207">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4a779-207">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4a779-208">b.</span><span class="sxs-lookup"><span data-stu-id="4a779-208">b.</span></span> <span data-ttu-id="4a779-209">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4a779-209">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="4a779-210">c.</span><span class="sxs-lookup"><span data-stu-id="4a779-210">c.</span></span> <span data-ttu-id="4a779-211">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="4a779-211">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="4a779-212">d.</span><span class="sxs-lookup"><span data-stu-id="4a779-212">d.</span></span> <span data-ttu-id="4a779-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4a779-213">Click **Create**.</span></span>
  
### <a name="create-a-workstars-test-user"></a><span data-ttu-id="4a779-214">Skapa en testanvändare Workstars</span><span class="sxs-lookup"><span data-stu-id="4a779-214">Create a Workstars test user</span></span>

<span data-ttu-id="4a779-215">I det här avsnittet skapar du en användare som kallas Britta Simon i Workstars.</span><span class="sxs-lookup"><span data-stu-id="4a779-215">In this section, you create a user called Britta Simon in Workstars.</span></span> <span data-ttu-id="4a779-216">Arbeta med [Workstars supportteam](https://support.workstars.com) tooadd hello användare i hello Workstars plattform.</span><span class="sxs-lookup"><span data-stu-id="4a779-216">Work with [Workstars support team](https://support.workstars.com) tooadd hello users in hello Workstars platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="4a779-217">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4a779-217">Assign hello Azure AD test user</span></span>

<span data-ttu-id="4a779-218">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooWorkstars.</span><span class="sxs-lookup"><span data-stu-id="4a779-218">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooWorkstars.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="4a779-220">**tooassign Britta Simon tooWorkstars utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4a779-220">**tooassign Britta Simon tooWorkstars, perform hello following steps:**</span></span>

1. <span data-ttu-id="4a779-221">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4a779-221">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4a779-223">Välj i listan med program hello **Workstars**.</span><span class="sxs-lookup"><span data-stu-id="4a779-223">In hello applications list, select **Workstars**.</span></span>

    ![Hej Workstars länken i listan med program hello](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_app.png)  

3. <span data-ttu-id="4a779-225">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4a779-225">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="4a779-227">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4a779-227">Click **Add** button.</span></span> <span data-ttu-id="4a779-228">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4a779-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="4a779-230">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4a779-230">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4a779-231">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4a779-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4a779-232">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4a779-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4a779-233">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4a779-233">Test single sign-on</span></span>

<span data-ttu-id="4a779-234">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4a779-234">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4a779-235">Du bör få automatiskt inloggade tooyour Workstars programmet när du klickar på hello Workstars panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4a779-235">When you click hello Workstars tile in hello Access Panel, you should get automatically signed-on tooyour Workstars application.</span></span>
<span data-ttu-id="4a779-236">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4a779-236">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4a779-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4a779-237">Additional resources</span></span>

* [<span data-ttu-id="4a779-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4a779-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4a779-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4a779-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_203.png

