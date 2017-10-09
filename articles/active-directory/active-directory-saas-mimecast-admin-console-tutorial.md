---
title: "Självstudier: Azure Active Directory-integrering med Mimecast administratörskonsolen | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mimecast-administrationskonsolen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5a04a5abd9ff30d484bce0a5c97a1d3e48b69e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a><span data-ttu-id="27055-103">Självstudier: Azure Active Directory-integrering med Mimecast-administrationskonsolen</span><span class="sxs-lookup"><span data-stu-id="27055-103">Tutorial: Azure Active Directory integration with Mimecast Admin Console</span></span>

<span data-ttu-id="27055-104">I kursen får du lära dig hur toointegrate Mimecast administrationskonsolen med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="27055-104">In this tutorial, you learn how toointegrate Mimecast Admin Console with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="27055-105">Integrera Mimecast administratörskonsolen med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="27055-105">Integrating Mimecast Admin Console with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="27055-106">Du kan styra i Azure AD som har åtkomst tooMimecast administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="27055-106">You can control in Azure AD who has access tooMimecast Admin Console.</span></span>
- <span data-ttu-id="27055-107">Du kan låta dina användare tooautomatically get inloggade tooMimecast administrationskonsolen (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="27055-107">You can enable your users tooautomatically get signed-on tooMimecast Admin Console (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="27055-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="27055-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="27055-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="27055-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27055-110">Krav</span><span class="sxs-lookup"><span data-stu-id="27055-110">Prerequisites</span></span>

<span data-ttu-id="27055-111">tooconfigure Azure AD-integrering med Mimecast administratörskonsolen måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="27055-111">tooconfigure Azure AD integration with Mimecast Admin Console, you need hello following items:</span></span>

- <span data-ttu-id="27055-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="27055-112">An Azure AD subscription</span></span>
- <span data-ttu-id="27055-113">En Mimecast administratörskonsolen enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="27055-113">A Mimecast Admin Console single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="27055-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="27055-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="27055-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="27055-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="27055-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="27055-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="27055-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="27055-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="27055-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="27055-118">Scenario description</span></span>
<span data-ttu-id="27055-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="27055-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="27055-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="27055-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="27055-121">Att lägga till Mimecast administratörskonsolen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="27055-121">Adding Mimecast Admin Console from hello gallery</span></span>
2. <span data-ttu-id="27055-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27055-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-admin-console-from-hello-gallery"></a><span data-ttu-id="27055-123">Att lägga till Mimecast administratörskonsolen från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="27055-123">Adding Mimecast Admin Console from hello gallery</span></span>
<span data-ttu-id="27055-124">tooconfigure hello integrering av Mimecast administrationskonsolen i Azure AD, behöver du tooadd Mimecast administratörskonsolen hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="27055-124">tooconfigure hello integration of Mimecast Admin Console into Azure AD, you need tooadd Mimecast Admin Console from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="27055-125">**tooadd Mimecast administratörskonsolen från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27055-125">**tooadd Mimecast Admin Console from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="27055-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="27055-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="27055-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="27055-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="27055-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="27055-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="27055-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27055-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="27055-133">Skriv i sökrutan hello **Mimecast administratörskonsolen**väljer **Mimecast administratörskonsolen** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="27055-133">In hello search box, type **Mimecast Admin Console**, select **Mimecast Admin Console** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Mimecast administrationskonsolen i hello resultatlistan](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="27055-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27055-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="27055-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Mimecast administratörskonsolen baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="27055-136">In this section, you configure and test Azure AD single sign-on with Mimecast Admin Console based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="27055-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i administratörskonsolen för Mimecast är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="27055-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mimecast Admin Console is tooa user in Azure AD.</span></span> <span data-ttu-id="27055-138">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i administratörskonsolen för Mimecast toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="27055-138">In other words, a link relationship between an Azure AD user and hello related user in Mimecast Admin Console needs toobe established.</span></span>

<span data-ttu-id="27055-139">I administratörskonsolen för Mimecast tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="27055-139">In Mimecast Admin Console, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="27055-140">tooconfigure och testa Azure AD enkel inloggning med Mimecast administratörskonsolen, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="27055-140">tooconfigure and test Azure AD single sign-on with Mimecast Admin Console, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="27055-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="27055-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="27055-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27055-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="27055-143">**[Skapa en testanvändare Mimecast administratörskonsolen](#create-a-mimecast-admin-console-test-user)**  -toohave en motsvarighet för Britta Simon i Mimecast Admin-konsolen som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="27055-143">**[Create a Mimecast Admin Console test user](#create-a-mimecast-admin-console-test-user)** - toohave a counterpart of Britta Simon in Mimecast Admin Console that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="27055-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="27055-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="27055-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="27055-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="27055-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27055-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="27055-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Mimecast-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="27055-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mimecast Admin Console application.</span></span>

<span data-ttu-id="27055-148">**tooconfigure Azure AD enkel inloggning med Mimecast administrationskonsolen utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27055-148">**tooconfigure Azure AD single sign-on with Mimecast Admin Console, perform hello following steps:**</span></span>

1. <span data-ttu-id="27055-149">I hello Azure-portalen på hello **Mimecast administratörskonsolen** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="27055-149">In hello Azure portal, on hello **Mimecast Admin Console** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="27055-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="27055-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. <span data-ttu-id="27055-153">På hello **Mimecast Admin Console domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="27055-153">On hello **Mimecast Admin Console Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och domänen för Mimecast Admin-konsolen med enkel inloggning information](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    <span data-ttu-id="27055-155">I hello **inloggnings-URL** textruta typen hello URL:</span><span class="sxs-lookup"><span data-stu-id="27055-155">In hello **Sign-on URL** textbox, type hello URL:</span></span>
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > <span data-ttu-id="27055-156">URL: en hello inloggning är regionspecifika.</span><span class="sxs-lookup"><span data-stu-id="27055-156">hello sign on URL is region specific.</span></span>

4. <span data-ttu-id="27055-157">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="27055-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. <span data-ttu-id="27055-159">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="27055-159">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="27055-161">På hello **Mimecast konsolen Administratörskonfigurationen** klickar du på **konfigurera Mimecast administratörskonsolen** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="27055-161">On hello **Mimecast Admin Console Configuration** section, click **Configure Mimecast Admin Console** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="27055-162">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="27055-162">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Mimecast Administratörskonfigurationen-konsolen](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. <span data-ttu-id="27055-164">Logga in på ditt Mimecast administratörskonsolen som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="27055-164">In a different web browser window, log into your Mimecast Admin Console as an administrator.</span></span>

8. <span data-ttu-id="27055-165">Gå för**Services \> programmet**.</span><span class="sxs-lookup"><span data-stu-id="27055-165">Go too**Services \> Application**.</span></span>

    <span data-ttu-id="27055-166">![Tjänster](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "tjänster")</span><span class="sxs-lookup"><span data-stu-id="27055-166">![Services](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Services")</span></span>

9. <span data-ttu-id="27055-167">Klicka på **autentisering profiler**.</span><span class="sxs-lookup"><span data-stu-id="27055-167">Click **Authentication Profiles**.</span></span>

    <span data-ttu-id="27055-168">![Profiler för autentisering](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "profiler för autentisering")</span><span class="sxs-lookup"><span data-stu-id="27055-168">![Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "Authentication Profiles")</span></span>
    
10. <span data-ttu-id="27055-169">Klicka på **nya autentiseringsprofil**.</span><span class="sxs-lookup"><span data-stu-id="27055-169">Click **New Authentication Profile**.</span></span>

    <span data-ttu-id="27055-170">![Nya autentisering profiler](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "nya profiler för autentisering")</span><span class="sxs-lookup"><span data-stu-id="27055-170">![New Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "New Authentication Profiles")</span></span>

11. <span data-ttu-id="27055-171">I hello **autentiseringsprofil** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="27055-171">In hello **Authentication Profile** section, perform hello following steps:</span></span>

    <span data-ttu-id="27055-172">![Autentiseringsprofil](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "autentiseringsprofil")</span><span class="sxs-lookup"><span data-stu-id="27055-172">![Authentication Profile](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "Authentication Profile")</span></span>
    
    <span data-ttu-id="27055-173">a.</span><span class="sxs-lookup"><span data-stu-id="27055-173">a.</span></span> <span data-ttu-id="27055-174">I hello **beskrivning** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="27055-174">In hello **Description** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="27055-175">b.</span><span class="sxs-lookup"><span data-stu-id="27055-175">b.</span></span> <span data-ttu-id="27055-176">Välj **genomdriva SAML-autentisering för Mimecast administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="27055-176">Select **Enforce SAML Authentication for Mimecast Admin Console**.</span></span>
    
    <span data-ttu-id="27055-177">c.</span><span class="sxs-lookup"><span data-stu-id="27055-177">c.</span></span> <span data-ttu-id="27055-178">Som **Provider**väljer **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="27055-178">As **Provider**, select **Azure Active Directory**.</span></span>
    
    <span data-ttu-id="27055-179">d.</span><span class="sxs-lookup"><span data-stu-id="27055-179">d.</span></span> <span data-ttu-id="27055-180">Klistra in **SAML enhets-ID**, som du har kopierat från hello Azure-portalen i hello **utfärdar-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="27055-180">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **Issuer URL** textbox.</span></span>
    
    <span data-ttu-id="27055-181">e.</span><span class="sxs-lookup"><span data-stu-id="27055-181">e.</span></span> <span data-ttu-id="27055-182">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="27055-182">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Login URL** textbox.</span></span>

    <span data-ttu-id="27055-183">f.</span><span class="sxs-lookup"><span data-stu-id="27055-183">f.</span></span> <span data-ttu-id="27055-184">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="27055-184">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Logout URL** textbox.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="27055-185">hello är inloggnings-URL och hello logga ut URL-värdet för hello Mimecast administratörskonsolen hello samma.</span><span class="sxs-lookup"><span data-stu-id="27055-185">hello Login URL value and hello Logout URL value are for hello Mimecast Admin Console hello same.</span></span>
    
    <span data-ttu-id="27055-186">g.</span><span class="sxs-lookup"><span data-stu-id="27055-186">g.</span></span> <span data-ttu-id="27055-187">Öppna base-64-certifikat hämtas från Azure-portalen i anteckningar, ta bort hello (”*--*”) och hello sista raden (”*--*”), kopiera hello återstående innehållet i den i Urklipp, och sedan klistra in den toohello **identitetscertifikat Provider (Metadata)** textruta.</span><span class="sxs-lookup"><span data-stu-id="27055-187">Open your base-64 certificate downloaded from Azure portal in notepad, remove hello first line (“*--*“) and hello last line (“*--*“), copy hello remaining content of it into your clipboard, and then paste it toohello **Identity Provider Certificate (Metadata)** textbox.</span></span>
    
    <span data-ttu-id="27055-188">h.</span><span class="sxs-lookup"><span data-stu-id="27055-188">h.</span></span> <span data-ttu-id="27055-189">Välj **tillåta enkel inloggning på**.</span><span class="sxs-lookup"><span data-stu-id="27055-189">Select **Allow Single Sign On**.</span></span>
    
    <span data-ttu-id="27055-190">Jag.</span><span class="sxs-lookup"><span data-stu-id="27055-190">i.</span></span> <span data-ttu-id="27055-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="27055-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="27055-192">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="27055-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="27055-193">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="27055-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="27055-194">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="27055-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="27055-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="27055-195">Create an Azure AD test user</span></span>

<span data-ttu-id="27055-196">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27055-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="27055-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27055-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="27055-199">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="27055-199">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="27055-201">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="27055-201">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="27055-203">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27055-203">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="27055-205">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="27055-205">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    <span data-ttu-id="27055-207">a.</span><span class="sxs-lookup"><span data-stu-id="27055-207">a.</span></span> <span data-ttu-id="27055-208">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="27055-208">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="27055-209">b.</span><span class="sxs-lookup"><span data-stu-id="27055-209">b.</span></span> <span data-ttu-id="27055-210">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="27055-210">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="27055-211">c.</span><span class="sxs-lookup"><span data-stu-id="27055-211">c.</span></span> <span data-ttu-id="27055-212">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="27055-212">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="27055-213">d.</span><span class="sxs-lookup"><span data-stu-id="27055-213">d.</span></span> <span data-ttu-id="27055-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="27055-214">Click **Create**.</span></span>
 
### <a name="create-a-mimecast-admin-console-test-user"></a><span data-ttu-id="27055-215">Skapa en testanvändare Mimecast administratörskonsolen</span><span class="sxs-lookup"><span data-stu-id="27055-215">Create a Mimecast Admin Console test user</span></span>

<span data-ttu-id="27055-216">I ordning tooenable Azure AD-användare toolog i administratörskonsolen för Mimecast, måste de etableras i Mimecast-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="27055-216">In order tooenable Azure AD users toolog into Mimecast Admin Console, they must be provisioned into Mimecast Admin Console.</span></span> <span data-ttu-id="27055-217">Hello gäller Mimecast administratörskonsolen är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="27055-217">In hello case of Mimecast Admin Console, provisioning is a manual task.</span></span>

* <span data-ttu-id="27055-218">Du måste tooregister en domän innan du kan skapa användare.</span><span class="sxs-lookup"><span data-stu-id="27055-218">You need tooregister a domain before you can create users.</span></span>

<span data-ttu-id="27055-219">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="27055-219">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="27055-220">Logga in tooyour **Mimecast administratörskonsolen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="27055-220">Sign on tooyour **Mimecast Admin Console** as administrator.</span></span>
2. <span data-ttu-id="27055-221">Gå för**kataloger \> internt**.</span><span class="sxs-lookup"><span data-stu-id="27055-221">Go too**Directories \> Internal**.</span></span>
   
   <span data-ttu-id="27055-222">![Kataloger](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "kataloger")</span><span class="sxs-lookup"><span data-stu-id="27055-222">![Directories](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "Directories")</span></span>
3. <span data-ttu-id="27055-223">Klicka på **registrera nya domän**.</span><span class="sxs-lookup"><span data-stu-id="27055-223">Click **Register New Domain**.</span></span>
   
   <span data-ttu-id="27055-224">![Registrera en ny domän](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "registrera ny domän")</span><span class="sxs-lookup"><span data-stu-id="27055-224">![Register New Domain](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "Register New Domain")</span></span>
4. <span data-ttu-id="27055-225">När den nya domänen har skapats, klickar du på **ny adress**.</span><span class="sxs-lookup"><span data-stu-id="27055-225">After your new domain has been created, click **New Address**.</span></span>
   
   <span data-ttu-id="27055-226">![Ny adress](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "ny adress")</span><span class="sxs-lookup"><span data-stu-id="27055-226">![New Address](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "New Address")</span></span>
5. <span data-ttu-id="27055-227">I hello nya adress dialog, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="27055-227">In hello new address dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="27055-228">![Spara](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "spara")</span><span class="sxs-lookup"><span data-stu-id="27055-228">![Save](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "Save")</span></span>
   
   <span data-ttu-id="27055-229">a.</span><span class="sxs-lookup"><span data-stu-id="27055-229">a.</span></span> <span data-ttu-id="27055-230">Typen hello **e-postadress**, **globalt namn**, **lösenord**, och **Bekräfta lösenord** attribut för en giltig Azure AD-kontot du vill tooprovision i hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="27055-230">Type hello **Email Address**, **Global Name**, **Password**, and **Confirm Password** attributes of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span>

   <span data-ttu-id="27055-231">b.</span><span class="sxs-lookup"><span data-stu-id="27055-231">b.</span></span> <span data-ttu-id="27055-232">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="27055-232">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="27055-233">Du kan använda andra Mimecast administratörskonsolen användare skapa verktyg eller API: er som tillhandahålls av Mimecast administratörskonsolen tooprovision Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="27055-233">You can use any other Mimecast Admin Console user account creation tools or APIs provided by Mimecast Admin Console tooprovision Azure AD user accounts.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="27055-234">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="27055-234">Assign hello Azure AD test user</span></span>

<span data-ttu-id="27055-235">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMimecast administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="27055-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMimecast Admin Console.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="27055-237">**tooassign Britta Simon tooMimecast administrationskonsolen utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="27055-237">**tooassign Britta Simon tooMimecast Admin Console, perform hello following steps:**</span></span>

1. <span data-ttu-id="27055-238">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="27055-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="27055-240">Välj i listan med program hello **Mimecast administratörskonsolen**.</span><span class="sxs-lookup"><span data-stu-id="27055-240">In hello applications list, select **Mimecast Admin Console**.</span></span>

    ![Hej Mimecast administratörskonsolen länken i listan med program hello](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. <span data-ttu-id="27055-242">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="27055-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="27055-244">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="27055-244">Click **Add** button.</span></span> <span data-ttu-id="27055-245">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27055-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="27055-247">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="27055-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="27055-248">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27055-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="27055-249">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="27055-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="27055-250">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="27055-250">Test single sign-on</span></span>

<span data-ttu-id="27055-251">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="27055-251">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="27055-252">Du bör få automatiskt inloggade tooyour Mimecast administratörskonsolen programmet när du klickar på hello Mimecast administratörskonsolen panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="27055-252">When you click hello Mimecast Admin Console tile in hello Access Panel, you should get automatically signed-on tooyour Mimecast Admin Console application.</span></span>
<span data-ttu-id="27055-253">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="27055-253">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="27055-254">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="27055-254">Additional resources</span></span>

* [<span data-ttu-id="27055-255">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="27055-255">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="27055-256">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="27055-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

