---
title: "Självstudier: Azure Active Directory-integrering med ScreenSteps | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ScreenSteps."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: fd041e5fe4552727eeda2dabc1773d8043d410f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a><span data-ttu-id="abff5-103">Självstudier: Azure Active Directory-integrering med ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="abff5-103">Tutorial: Azure Active Directory integration with ScreenSteps</span></span>

<span data-ttu-id="abff5-104">I kursen får du lära dig hur toointegrate ScreenSteps med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="abff5-104">In this tutorial, you learn how toointegrate ScreenSteps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="abff5-105">Integrera ScreenSteps med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="abff5-105">Integrating ScreenSteps with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="abff5-106">Du kan styra i Azure AD som har åtkomst till tooScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="abff5-106">You can control in Azure AD who has access tooScreenSteps.</span></span>
- <span data-ttu-id="abff5-107">Du kan låta dina användare tooautomatically get inloggade tooScreenSteps (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="abff5-107">You can enable your users tooautomatically get signed-on tooScreenSteps (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="abff5-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="abff5-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="abff5-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="abff5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="abff5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="abff5-110">Prerequisites</span></span>

<span data-ttu-id="abff5-111">tooconfigure Azure AD-integrering med ScreenSteps, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="abff5-111">tooconfigure Azure AD integration with ScreenSteps, you need hello following items:</span></span>

- <span data-ttu-id="abff5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="abff5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="abff5-113">En ScreenSteps enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="abff5-113">A ScreenSteps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="abff5-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="abff5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="abff5-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="abff5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="abff5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="abff5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="abff5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abff5-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="abff5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="abff5-118">Scenario description</span></span>
<span data-ttu-id="abff5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="abff5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="abff5-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="abff5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="abff5-121">Att lägga till ScreenSteps från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="abff5-121">Adding ScreenSteps from hello gallery</span></span>
2. <span data-ttu-id="abff5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="abff5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-screensteps-from-hello-gallery"></a><span data-ttu-id="abff5-123">Att lägga till ScreenSteps från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="abff5-123">Adding ScreenSteps from hello gallery</span></span>
<span data-ttu-id="abff5-124">tooconfigure hello integrering av ScreenSteps i Azure AD, behöver du tooadd ScreenSteps hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="abff5-124">tooconfigure hello integration of ScreenSteps into Azure AD, you need tooadd ScreenSteps from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="abff5-125">**tooadd ScreenSteps från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="abff5-125">**tooadd ScreenSteps from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="abff5-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="abff5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="abff5-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="abff5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="abff5-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="abff5-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="abff5-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="abff5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="abff5-133">Skriv i sökrutan hello **ScreenSteps**väljer **ScreenSteps** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="abff5-133">In hello search box, type **ScreenSteps**, select **ScreenSteps** from result panel then click **Add** button tooadd hello application.</span></span>

    ![ScreenSteps i hello resultatlistan](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="abff5-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="abff5-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="abff5-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ScreenSteps baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="abff5-136">In this section, you configure and test Azure AD single sign-on with ScreenSteps based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="abff5-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ScreenSteps är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="abff5-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ScreenSteps is tooa user in Azure AD.</span></span> <span data-ttu-id="abff5-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ScreenSteps toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="abff5-138">In other words, a link relationship between an Azure AD user and hello related user in ScreenSteps needs toobe established.</span></span>

<span data-ttu-id="abff5-139">I ScreenSteps, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="abff5-139">In ScreenSteps, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="abff5-140">tooconfigure och testa Azure AD enkel inloggning med ScreenSteps, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="abff5-140">tooconfigure and test Azure AD single sign-on with ScreenSteps, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="abff5-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="abff5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="abff5-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="abff5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="abff5-143">**[Skapa en testanvändare ScreenSteps](#create-a-screensteps-test-user)**  -toohave en motsvarighet för Britta Simon i ScreenSteps som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="abff5-143">**[Create a ScreenSteps test user](#create-a-screensteps-test-user)** - toohave a counterpart of Britta Simon in ScreenSteps that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="abff5-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="abff5-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="abff5-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="abff5-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="abff5-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="abff5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="abff5-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt ScreenSteps program.</span><span class="sxs-lookup"><span data-stu-id="abff5-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ScreenSteps application.</span></span>

<span data-ttu-id="abff5-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med ScreenSteps:**</span><span class="sxs-lookup"><span data-stu-id="abff5-148">**tooconfigure Azure AD single sign-on with ScreenSteps, perform hello following steps:**</span></span>

1. <span data-ttu-id="abff5-149">I hello Azure-portalen på hello **ScreenSteps** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="abff5-149">In hello Azure portal, on hello **ScreenSteps** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="abff5-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="abff5-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. <span data-ttu-id="abff5-153">På hello **ScreenSteps domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="abff5-153">On hello **ScreenSteps Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och ScreenSteps domän med enkel inloggning information](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    <span data-ttu-id="abff5-155">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.ScreenSteps.com`</span><span class="sxs-lookup"><span data-stu-id="abff5-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.ScreenSteps.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="abff5-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="abff5-156">This value is not real.</span></span> <span data-ttu-id="abff5-157">Uppdatera det här värdet med hello faktiska inloggnings-URL, som beskrivs senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="abff5-157">Update this value with hello actual Sign-On URL, which is explained later in this tutorial.</span></span> 

4. <span data-ttu-id="abff5-158">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="abff5-158">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. <span data-ttu-id="abff5-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="abff5-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="abff5-162">På hello **ScreenSteps Configuration** klickar du på **konfigurera ScreenSteps** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="abff5-162">On hello **ScreenSteps Configuration** section, click **Configure ScreenSteps** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="abff5-163">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="abff5-163">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![ScreenSteps konfiguration](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. <span data-ttu-id="abff5-165">Logga in på webbplatsen ScreenSteps företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="abff5-165">In a different web browser window, log into your ScreenSteps company site as an administrator.</span></span>

8. <span data-ttu-id="abff5-166">Klicka på **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="abff5-166">Click **Account Settings**.</span></span>

    <span data-ttu-id="abff5-167">![Kontohantering](./media/active-directory-saas-screensteps-tutorial/ic778523.png "kontohantering")</span><span class="sxs-lookup"><span data-stu-id="abff5-167">![Account management](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Account management")</span></span>

9. <span data-ttu-id="abff5-168">Klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="abff5-168">Click **Single Sign-on**.</span></span>

    <span data-ttu-id="abff5-169">![Fjärrautentiseringen](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span><span class="sxs-lookup"><span data-stu-id="abff5-169">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span></span>

10. <span data-ttu-id="abff5-170">Klicka på **skapa slutpunkten för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="abff5-170">Click **Create Single Sign-on Endpoint**.</span></span>

    <span data-ttu-id="abff5-171">![Fjärrautentiseringen](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span><span class="sxs-lookup"><span data-stu-id="abff5-171">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span></span>

11. <span data-ttu-id="abff5-172">I hello **skapa enkel inloggning Endpoint** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="abff5-172">In hello **Create Single Sign-on Endpoint** section, perform hello following steps:</span></span>

    <span data-ttu-id="abff5-173">![Skapa en slutpunkt för autentisering](./media/active-directory-saas-screensteps-tutorial/ic778526.png "skapa en slutpunkt för autentisering")</span><span class="sxs-lookup"><span data-stu-id="abff5-173">![Create an authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778526.png "Create an authentication endpoint")</span></span>
    
    <span data-ttu-id="abff5-174">a.</span><span class="sxs-lookup"><span data-stu-id="abff5-174">a.</span></span> <span data-ttu-id="abff5-175">I hello **rubrik** textruta skriver du ett namn.</span><span class="sxs-lookup"><span data-stu-id="abff5-175">In hello **Title** textbox, type a title.</span></span>
    
    <span data-ttu-id="abff5-176">b.</span><span class="sxs-lookup"><span data-stu-id="abff5-176">b.</span></span> <span data-ttu-id="abff5-177">Från hello **läge** väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="abff5-177">From hello **Mode** list, select **SAML**.</span></span>
    
    <span data-ttu-id="abff5-178">c.</span><span class="sxs-lookup"><span data-stu-id="abff5-178">c.</span></span> <span data-ttu-id="abff5-179">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="abff5-179">Click **Create**.</span></span>

12. <span data-ttu-id="abff5-180">**Redigera** hello ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="abff5-180">**Edit** hello new endpoint.</span></span>

    <span data-ttu-id="abff5-181">![Redigera endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "redigera slutpunkt")</span><span class="sxs-lookup"><span data-stu-id="abff5-181">![Edit endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "Edit endpoint")</span></span>

13. <span data-ttu-id="abff5-182">I hello **Redigera enkel inloggning Endpoint** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="abff5-182">In hello **Edit Single Sign-on Endpoint** section, perform hello following steps:</span></span>

    <span data-ttu-id="abff5-183">![Remote autentiseringsslutpunkten](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication slutpunkt")</span><span class="sxs-lookup"><span data-stu-id="abff5-183">![Remote authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication endpoint")</span></span>

    <span data-ttu-id="abff5-184">a.</span><span class="sxs-lookup"><span data-stu-id="abff5-184">a.</span></span> <span data-ttu-id="abff5-185">Klicka på **överför ny SAML certifikatfilen**, och sedan ladda upp hello certifikat som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="abff5-185">Click **Upload new SAML Certificate file**, and then upload hello certificate, which you have downloaded from Azure portal.</span></span>
    
    <span data-ttu-id="abff5-186">b.</span><span class="sxs-lookup"><span data-stu-id="abff5-186">b.</span></span> <span data-ttu-id="abff5-187">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **Remote inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="abff5-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **Remote Login URL** textbox.</span></span>
    
    <span data-ttu-id="abff5-188">c.</span><span class="sxs-lookup"><span data-stu-id="abff5-188">c.</span></span> <span data-ttu-id="abff5-189">Klistra in **Sign-Out URL** -värde som du har kopierat från hello Azure-portalen i hello **URL för utloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="abff5-189">Paste **Sign-Out URL** value, which you have copied from hello Azure portal into hello **Log out URL** textbox.</span></span>
    
    <span data-ttu-id="abff5-190">d.</span><span class="sxs-lookup"><span data-stu-id="abff5-190">d.</span></span> <span data-ttu-id="abff5-191">Välj en **grupp** tooassign användare toowhen de har etablerats.</span><span class="sxs-lookup"><span data-stu-id="abff5-191">Select a **Group** tooassign users toowhen they are provisioned.</span></span>
    
    <span data-ttu-id="abff5-192">e.</span><span class="sxs-lookup"><span data-stu-id="abff5-192">e.</span></span> <span data-ttu-id="abff5-193">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="abff5-193">Click **Update**.</span></span>

    <span data-ttu-id="abff5-194">f.</span><span class="sxs-lookup"><span data-stu-id="abff5-194">f.</span></span> <span data-ttu-id="abff5-195">Kopiera hello **konsument-URL för SAML** toohello Urklipp och klistra in i toohello **inloggnings-URL** TextBox-kontroll i **ScreenSteps domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="abff5-195">Copy hello **SAML Consumer URL** toohello clipboard and paste in toohello **Sign-on URL** textbox in **ScreenSteps Domain and URLs** section.</span></span>
    
    <span data-ttu-id="abff5-196">g.</span><span class="sxs-lookup"><span data-stu-id="abff5-196">g.</span></span> <span data-ttu-id="abff5-197">Returnera toohello **Redigera enkel inloggning Endpoint**.</span><span class="sxs-lookup"><span data-stu-id="abff5-197">Return toohello **Edit Single Sign-on Endpoint**.</span></span>
    
    <span data-ttu-id="abff5-198">h.</span><span class="sxs-lookup"><span data-stu-id="abff5-198">h.</span></span> <span data-ttu-id="abff5-199">Klicka på hello **göra standard för kontot** knappen toouse den här slutpunkten för alla användare som loggar in i ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="abff5-199">Click hello **Make default for account** button toouse this endpoint for all users who log into ScreenSteps.</span></span> <span data-ttu-id="abff5-200">Du kan också klicka på hello **lägga till tooSite** knappen toouse specifika platser i den här slutpunkten **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="abff5-200">Alternatively you can click hello **Add tooSite** button toouse this endpoint for specific sites in **ScreenSteps**.</span></span>

> [!TIP]
> <span data-ttu-id="abff5-201">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="abff5-201">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="abff5-202">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="abff5-202">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="abff5-203">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="abff5-203">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="abff5-204">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="abff5-204">Create an Azure AD test user</span></span>

<span data-ttu-id="abff5-205">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="abff5-205">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="abff5-207">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="abff5-207">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="abff5-208">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="abff5-208">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="abff5-210">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="abff5-210">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="abff5-212">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="abff5-212">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="abff5-214">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="abff5-214">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    <span data-ttu-id="abff5-216">a.</span><span class="sxs-lookup"><span data-stu-id="abff5-216">a.</span></span> <span data-ttu-id="abff5-217">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="abff5-217">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="abff5-218">b.</span><span class="sxs-lookup"><span data-stu-id="abff5-218">b.</span></span> <span data-ttu-id="abff5-219">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="abff5-219">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="abff5-220">c.</span><span class="sxs-lookup"><span data-stu-id="abff5-220">c.</span></span> <span data-ttu-id="abff5-221">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="abff5-221">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="abff5-222">d.</span><span class="sxs-lookup"><span data-stu-id="abff5-222">d.</span></span> <span data-ttu-id="abff5-223">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="abff5-223">Click **Create**.</span></span>
 
### <a name="create-a-screensteps-test-user"></a><span data-ttu-id="abff5-224">Skapa en testanvändare ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="abff5-224">Create a ScreenSteps test user</span></span>

<span data-ttu-id="abff5-225">I det här avsnittet skapar du en användare som kallas Britta Simon i ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="abff5-225">In this section, you create a user called Britta Simon in ScreenSteps.</span></span> <span data-ttu-id="abff5-226">Arbeta med [ScreenSteps klienten supportteamet](https://www.screensteps.com/contact) att lägga till hello användare i hello ScreenSteps plattform.</span><span class="sxs-lookup"><span data-stu-id="abff5-226">Work with [ScreenSteps Client support team](https://www.screensteps.com/contact) to add hello users in hello ScreenSteps platform.</span></span> <span data-ttu-id="abff5-227">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="abff5-227">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="abff5-228">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="abff5-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="abff5-229">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="abff5-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooScreenSteps.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="abff5-231">**tooassign Britta Simon tooScreenSteps utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="abff5-231">**tooassign Britta Simon tooScreenSteps, perform hello following steps:**</span></span>

1. <span data-ttu-id="abff5-232">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="abff5-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="abff5-234">Välj i listan med program hello **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="abff5-234">In hello applications list, select **ScreenSteps**.</span></span>

    ![Hej ScreenSteps länken i listan med program hello](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. <span data-ttu-id="abff5-236">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="abff5-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="abff5-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="abff5-238">Click **Add** button.</span></span> <span data-ttu-id="abff5-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="abff5-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="abff5-241">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="abff5-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="abff5-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="abff5-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="abff5-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="abff5-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="abff5-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="abff5-244">Test single sign-on</span></span>

<span data-ttu-id="abff5-245">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="abff5-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="abff5-246">Du bör få automatiskt inloggade tooyour ScreenSteps programmet när du klickar på hello ScreenSteps panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="abff5-246">When you click hello ScreenSteps tile in hello Access Panel, you should get automatically signed-on tooyour ScreenSteps application.</span></span>
<span data-ttu-id="abff5-247">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="abff5-247">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="abff5-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="abff5-248">Additional resources</span></span>

* [<span data-ttu-id="abff5-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="abff5-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="abff5-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="abff5-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

