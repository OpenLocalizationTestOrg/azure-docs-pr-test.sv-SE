---
title: "Självstudier: Azure Active Directory-integrering med uppfattning USA (icke-UltiPro) | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och uppfattning USA (icke-UltiPro)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 874b5da277b9c68504c4af2ac87ed90d2bbd93b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a><span data-ttu-id="37114-103">Självstudier: Azure Active Directory-integrering med uppfattning USA (icke-UltiPro)</span><span class="sxs-lookup"><span data-stu-id="37114-103">Tutorial: Azure Active Directory integration with Perception United States (Non-UltiPro)</span></span>

<span data-ttu-id="37114-104">I kursen får du lära dig hur toointegrate uppfattning USA (icke-UltiPro) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="37114-104">In this tutorial, you learn how toointegrate Perception United States (Non-UltiPro) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="37114-105">Integrera uppfattning USA (icke-UltiPro) med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="37114-105">Integrating Perception United States (Non-UltiPro) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="37114-106">Du kan styra i Azure AD som har åtkomst tooPerception USA (icke-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="37114-106">You can control in Azure AD who has access tooPerception United States (Non-UltiPro).</span></span>
- <span data-ttu-id="37114-107">Du kan låta dina användare tooautomatically get inloggade tooPerception USA (icke-UltiPro) (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="37114-107">You can enable your users tooautomatically get signed-on tooPerception United States (Non-UltiPro) (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="37114-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="37114-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="37114-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="37114-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37114-110">Krav</span><span class="sxs-lookup"><span data-stu-id="37114-110">Prerequisites</span></span>

<span data-ttu-id="37114-111">tooconfigure Azure AD-integrering med uppfattning USA (icke-UltiPro), behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="37114-111">tooconfigure Azure AD integration with Perception United States (Non-UltiPro), you need hello following items:</span></span>

- <span data-ttu-id="37114-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="37114-112">An Azure AD subscription</span></span>
- <span data-ttu-id="37114-113">En uppfattning USA (icke-UltiPro) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="37114-113">A Perception United States (Non-UltiPro) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="37114-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="37114-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="37114-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="37114-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="37114-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="37114-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="37114-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="37114-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="37114-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="37114-118">Scenario description</span></span>
<span data-ttu-id="37114-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="37114-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="37114-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="37114-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="37114-121">Att lägga till uppfattning USA (icke-UltiPro) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="37114-121">Adding Perception United States (Non-UltiPro) from hello gallery</span></span>
2. <span data-ttu-id="37114-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37114-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-perception-united-states-non-ultipro-from-hello-gallery"></a><span data-ttu-id="37114-123">Att lägga till uppfattning USA (icke-UltiPro) från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="37114-123">Adding Perception United States (Non-UltiPro) from hello gallery</span></span>
<span data-ttu-id="37114-124">tooconfigure hello integrering av uppfattning USA (icke-UltiPro) i Azure AD, behöver du tooadd uppfattning USA (icke-UltiPro) från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="37114-124">tooconfigure hello integration of Perception United States (Non-UltiPro) into Azure AD, you need tooadd Perception United States (Non-UltiPro) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="37114-125">**tooadd uppfattning USA (icke-UltiPro) från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="37114-125">**tooadd Perception United States (Non-UltiPro) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="37114-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="37114-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="37114-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="37114-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="37114-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="37114-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="37114-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37114-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="37114-133">Skriv i sökrutan hello **uppfattning USA (icke-UltiPro)**väljer **uppfattning USA (icke-UltiPro)** resultatet-panelen klickar **Lägg till** knappen tooadd hello-program.</span><span class="sxs-lookup"><span data-stu-id="37114-133">In hello search box, type **Perception United States (Non-UltiPro)**, select **Perception United States (Non-UltiPro)** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Uppfattning USA (icke-UltiPro) i hello resultatlistan](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="37114-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37114-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="37114-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med uppfattning USA (icke-UltiPro) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="37114-136">In this section, you configure and test Azure AD single sign-on with Perception United States (Non-UltiPro) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="37114-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i uppfattning USA (icke-UltiPro) är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="37114-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Perception United States (Non-UltiPro) is tooa user in Azure AD.</span></span> <span data-ttu-id="37114-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i uppfattning USA (icke-UltiPro) toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="37114-138">In other words, a link relationship between an Azure AD user and hello related user in Perception United States (Non-UltiPro) needs toobe established.</span></span>

<span data-ttu-id="37114-139">I uppfattning USA (icke-UltiPro), tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="37114-139">In Perception United States (Non-UltiPro), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="37114-140">tooconfigure och testa Azure AD enkel inloggning med uppfattning USA (icke-UltiPro), behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="37114-140">tooconfigure and test Azure AD single sign-on with Perception United States (Non-UltiPro), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="37114-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="37114-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="37114-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37114-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="37114-143">**[Skapa en testanvändare uppfattning USA (icke-UltiPro)](#create-a-perception-united-states-non-ultipro-test-user)**  -toohave en motsvarighet för Britta Simon i uppfattning USA (icke-UltiPro) som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="37114-143">**[Create a Perception United States (Non-UltiPro) test user](#create-a-perception-united-states-non-ultipro-test-user)** - toohave a counterpart of Britta Simon in Perception United States (Non-UltiPro) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="37114-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="37114-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="37114-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="37114-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="37114-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37114-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="37114-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet uppfattning USA (icke-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="37114-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Perception United States (Non-UltiPro) application.</span></span>

<span data-ttu-id="37114-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med uppfattning USA (icke-UltiPro):**</span><span class="sxs-lookup"><span data-stu-id="37114-148">**tooconfigure Azure AD single sign-on with Perception United States (Non-UltiPro), perform hello following steps:**</span></span>

1. <span data-ttu-id="37114-149">I hello Azure-portalen på hello **uppfattning USA (icke-UltiPro)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="37114-149">In hello Azure portal, on hello **Perception United States (Non-UltiPro)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="37114-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="37114-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. <span data-ttu-id="37114-153">På hello **domänen uppfattning USA (icke-UltiPro) och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="37114-153">On hello **Perception United States (Non-UltiPro) Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och uppfattning USA (icke-UltiPro)-domän med enkel inloggning information](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    <span data-ttu-id="37114-155">a.</span><span class="sxs-lookup"><span data-stu-id="37114-155">a.</span></span> <span data-ttu-id="37114-156">I hello **identifierare** textruta typen hello URL:`https://perception.kanjoya.com/sp`</span><span class="sxs-lookup"><span data-stu-id="37114-156">In hello **Identifier** textbox, type hello URL: `https://perception.kanjoya.com/sp`</span></span>

    <span data-ttu-id="37114-157">b.</span><span class="sxs-lookup"><span data-stu-id="37114-157">b.</span></span> <span data-ttu-id="37114-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://perception.kanjoya.com/sso?idp=<entity_id>`</span><span class="sxs-lookup"><span data-stu-id="37114-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://perception.kanjoya.com/sso?idp=<entity_id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="37114-159">hello-värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="37114-159">hello value is not real.</span></span> <span data-ttu-id="37114-160">Hello värdet uppdateras med hello faktiska Reply URL, vilket beskrivs senare i hello kursen.</span><span class="sxs-lookup"><span data-stu-id="37114-160">You will update hello value with hello actual Reply URL, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="37114-161">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="37114-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. <span data-ttu-id="37114-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="37114-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="37114-165">På hello **uppfattning USA (icke-UltiPro) konfiguration** klickar du på **konfigurera uppfattning USA (icke-UltiPro)** tooopen **konfigurera inloggning** fönstret.</span><span class="sxs-lookup"><span data-stu-id="37114-165">On hello **Perception United States (Non-UltiPro) Configuration** section, click **Configure Perception United States (Non-UltiPro)** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="37114-166">Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="37114-166">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="37114-167">a.</span><span class="sxs-lookup"><span data-stu-id="37114-167">a.</span></span> <span data-ttu-id="37114-168">Hej **uppfattning USA (icke-UltiPro)** programmet kräver hello **SAML enhets-ID** -värde som du har kopierat toobe uri-kodad.</span><span class="sxs-lookup"><span data-stu-id="37114-168">hello **Perception United States (Non-UltiPro)** application requires hello **SAML Entity ID** value, which you have copied, toobe uri encoded.</span></span> <span data-ttu-id="37114-169">tooget hello uri-kodad värde, Använd hello följande länk:**http://www.url-encode-decode.com/**.</span><span class="sxs-lookup"><span data-stu-id="37114-169">tooget hello uri encoded value, use hello following link:**http://www.url-encode-decode.com/**.</span></span>

    <span data-ttu-id="37114-170">b.</span><span class="sxs-lookup"><span data-stu-id="37114-170">b.</span></span> <span data-ttu-id="37114-171">När du har skaffat hello uri kombinera det procentkodade värdet med hello **Reply URL** som anges nedan -</span><span class="sxs-lookup"><span data-stu-id="37114-171">After getting hello uri encoded value combine it with hello **Reply URL** as mentioned below-</span></span>

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    <span data-ttu-id="37114-172">c.</span><span class="sxs-lookup"><span data-stu-id="37114-172">c.</span></span> <span data-ttu-id="37114-173">Klistra in hello större än värdet i hello **Reply URL** TextBox-kontroll i **domänen uppfattning USA (icke-UltiPro) och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="37114-173">Paste hello above value in hello **Reply URL** textbox in **Perception United States (Non-UltiPro) Domain and URLs** section.</span></span>

    ![Uppfattning USA (icke-UltiPro) konfiguration](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. <span data-ttu-id="37114-175">Logga in tooyour uppfattning USA (icke-UltiPro) företagets webbplats som en administratör i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="37114-175">In another browser window, sign on tooyour Perception United States (Non-UltiPro) company site as an administrator.</span></span>

8. <span data-ttu-id="37114-176">I hello verktygsfältet klickar du på **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="37114-176">In hello main toolbar, click **Account Settings**.</span></span>

    ![Uppfattning USA (icke-UltiPro) användare](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. <span data-ttu-id="37114-178">På hello **kontoinställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="37114-178">On hello **Account Settings** page, perform hello following steps:</span></span>

    ![Uppfattning USA (icke-UltiPro) användare](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    <span data-ttu-id="37114-180">a.</span><span class="sxs-lookup"><span data-stu-id="37114-180">a.</span></span> <span data-ttu-id="37114-181">I hello **företagsnamn** textruta hello-typnamn för hello **företagets**.</span><span class="sxs-lookup"><span data-stu-id="37114-181">In hello **Company Name** textbox, type hello name of hello **Company**.</span></span>
    
    <span data-ttu-id="37114-182">b.</span><span class="sxs-lookup"><span data-stu-id="37114-182">b.</span></span> <span data-ttu-id="37114-183">I hello **kontonamn** textruta hello-typnamn för hello **konto**.</span><span class="sxs-lookup"><span data-stu-id="37114-183">In hello **Account Name** textbox, type hello name of hello **Account**.</span></span>

    <span data-ttu-id="37114-184">c.</span><span class="sxs-lookup"><span data-stu-id="37114-184">c.</span></span> <span data-ttu-id="37114-185">I **standard Reply-tooEmail** textruta, typen hello giltig **e-post**.</span><span class="sxs-lookup"><span data-stu-id="37114-185">In **Default Reply-tooEmail** text box, type hello valid **Email**.</span></span>

    <span data-ttu-id="37114-186">d.</span><span class="sxs-lookup"><span data-stu-id="37114-186">d.</span></span> <span data-ttu-id="37114-187">Välj **SSO identitetsleverantör** som **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="37114-187">Select **SSO Identity Provider** as **SAML 2.0**.</span></span>

10. <span data-ttu-id="37114-188">På hello **SSO Configuration** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="37114-188">On hello **SSO Configuration** page, perform hello following steps:</span></span>

    ![Uppfattning USA (icke-UltiPro) SSOConfig](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    <span data-ttu-id="37114-190">a.</span><span class="sxs-lookup"><span data-stu-id="37114-190">a.</span></span> <span data-ttu-id="37114-191">Välj **SAML NameID typen** som **e-post**.</span><span class="sxs-lookup"><span data-stu-id="37114-191">Select **SAML NameID Type** as **EMAIL**.</span></span>

    <span data-ttu-id="37114-192">b.</span><span class="sxs-lookup"><span data-stu-id="37114-192">b.</span></span> <span data-ttu-id="37114-193">I hello **SSO Konfigurationsnamnet** textruta hello-typnamn för din **Configuration**.</span><span class="sxs-lookup"><span data-stu-id="37114-193">In hello **SSO Configuration Name** textbox, type hello name of your **Configuration**.</span></span>
    
    <span data-ttu-id="37114-194">c.</span><span class="sxs-lookup"><span data-stu-id="37114-194">c.</span></span> <span data-ttu-id="37114-195">I **identitet providernamn** textruta klistra in hello värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="37114-195">In **Identity Provider Name** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="37114-196">d.</span><span class="sxs-lookup"><span data-stu-id="37114-196">d.</span></span> <span data-ttu-id="37114-197">I **SAML domän textruta**, ange hello-domän som  **@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="37114-197">In **SAML Domain textbox**, enter hello domain like **@contoso.com**.</span></span>

    <span data-ttu-id="37114-198">e.</span><span class="sxs-lookup"><span data-stu-id="37114-198">e.</span></span> <span data-ttu-id="37114-199">Klicka på **överför igen** tooupload hello **XML-Metadata för** fil.</span><span class="sxs-lookup"><span data-stu-id="37114-199">Click on **Upload Again** tooupload hello **Metadata XML** file.</span></span>

    <span data-ttu-id="37114-200">f.</span><span class="sxs-lookup"><span data-stu-id="37114-200">f.</span></span> <span data-ttu-id="37114-201">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="37114-201">Click **Update**.</span></span>


> [!TIP]
> <span data-ttu-id="37114-202">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="37114-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="37114-203">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="37114-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="37114-204">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="37114-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="37114-205">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="37114-205">Create an Azure AD test user</span></span>

<span data-ttu-id="37114-206">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37114-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="37114-208">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="37114-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="37114-209">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="37114-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="37114-211">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="37114-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="37114-213">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37114-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="37114-215">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="37114-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    <span data-ttu-id="37114-217">a.</span><span class="sxs-lookup"><span data-stu-id="37114-217">a.</span></span> <span data-ttu-id="37114-218">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="37114-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="37114-219">b.</span><span class="sxs-lookup"><span data-stu-id="37114-219">b.</span></span> <span data-ttu-id="37114-220">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="37114-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="37114-221">c.</span><span class="sxs-lookup"><span data-stu-id="37114-221">c.</span></span> <span data-ttu-id="37114-222">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="37114-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="37114-223">d.</span><span class="sxs-lookup"><span data-stu-id="37114-223">d.</span></span> <span data-ttu-id="37114-224">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="37114-224">Click **Create**.</span></span>
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a><span data-ttu-id="37114-225">Skapa en testanvändare uppfattning USA (icke-UltiPro)</span><span class="sxs-lookup"><span data-stu-id="37114-225">Create a Perception United States (Non-UltiPro) test user</span></span>

<span data-ttu-id="37114-226">I det här avsnittet skapar du en användare som kallas Britta Simon uppfattning United States (icke-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="37114-226">In this section, you create a user called Britta Simon in Perception United States (Non-UltiPro).</span></span> <span data-ttu-id="37114-227">Arbeta med [uppfattning USA (icke-UltiPro) supportteamet](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello användare i hello uppfattning USA (icke-UltiPro)-plattformen.</span><span class="sxs-lookup"><span data-stu-id="37114-227">Work with [Perception United States (Non-UltiPro) support team](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello users in hello Perception United States (Non-UltiPro) platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="37114-228">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="37114-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="37114-229">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooPerception USA (icke-UltiPro).</span><span class="sxs-lookup"><span data-stu-id="37114-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPerception United States (Non-UltiPro).</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="37114-231">**tooassign Britta Simon tooPerception USA (icke-UltiPro), utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="37114-231">**tooassign Britta Simon tooPerception United States (Non-UltiPro), perform hello following steps:**</span></span>

1. <span data-ttu-id="37114-232">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="37114-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="37114-234">Välj i listan med program hello **uppfattning USA (icke-UltiPro)**.</span><span class="sxs-lookup"><span data-stu-id="37114-234">In hello applications list, select **Perception United States (Non-UltiPro)**.</span></span>

    ![hello uppfattning USA (icke-UltiPro) länken i listan med program hello](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. <span data-ttu-id="37114-236">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="37114-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="37114-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="37114-238">Click **Add** button.</span></span> <span data-ttu-id="37114-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37114-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="37114-241">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="37114-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="37114-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37114-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="37114-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="37114-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="37114-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="37114-244">Test single sign-on</span></span>

<span data-ttu-id="37114-245">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="37114-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="37114-246">När du klickar på hello uppfattning USA (icke-UltiPro)-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour uppfattning USA (icke-UltiPro) program.</span><span class="sxs-lookup"><span data-stu-id="37114-246">When you click hello Perception United States (Non-UltiPro) tile in hello Access Panel, you should get automatically signed-on tooyour Perception United States (Non-UltiPro) application.</span></span>
<span data-ttu-id="37114-247">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="37114-247">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="37114-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="37114-248">Additional resources</span></span>

* [<span data-ttu-id="37114-249">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="37114-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="37114-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="37114-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

