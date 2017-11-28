---
title: "Självstudier: Azure Active Directory-integrering med säkrad | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och säkrad."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 720ed8af0b5de1e3bee5a40353ca0ee661766864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a><span data-ttu-id="b0b6f-103">Självstudier: Azure Active Directory-integrering med säkrad</span><span class="sxs-lookup"><span data-stu-id="b0b6f-103">Tutorial: Azure Active Directory integration with Fuse</span></span>

<span data-ttu-id="b0b6f-104">I kursen får du lära dig hur toointegrate videoformatet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b0b6f-104">In this tutorial, you learn how toointegrate Fuse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b0b6f-105">Integrera säkrad med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-105">Integrating Fuse with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b0b6f-106">Du kan styra i Azure AD som har åtkomst till tooFuse.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-106">You can control in Azure AD who has access tooFuse.</span></span>
- <span data-ttu-id="b0b6f-107">Du kan låta dina användare tooautomatically get inloggade tooFuse (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-107">You can enable your users tooautomatically get signed-on tooFuse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="b0b6f-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="b0b6f-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b0b6f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0b6f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b0b6f-110">Prerequisites</span></span>

<span data-ttu-id="b0b6f-111">tooconfigure Azure AD-integrering med säkrad, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-111">tooconfigure Azure AD integration with Fuse, you need hello following items:</span></span>

- <span data-ttu-id="b0b6f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b0b6f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b0b6f-113">En säkrad enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b0b6f-113">A Fuse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b0b6f-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b0b6f-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b0b6f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b0b6f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b0b6f-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b0b6f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b0b6f-118">Scenario description</span></span>
<span data-ttu-id="b0b6f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b0b6f-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b0b6f-121">Lägg till säkrad från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b0b6f-121">Add Fuse from hello gallery</span></span>
2. <span data-ttu-id="b0b6f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0b6f-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-fuse-from-hello-gallery"></a><span data-ttu-id="b0b6f-123">Lägg till säkrad från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b0b6f-123">Add Fuse from hello gallery</span></span>
<span data-ttu-id="b0b6f-124">tooconfigure hello integrering av säkrad i Azure AD, behöver du tooadd säkrad hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-124">tooconfigure hello integration of Fuse into Azure AD, you need tooadd Fuse from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b0b6f-125">**tooadd säkrad från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-125">**tooadd Fuse from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0b6f-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="b0b6f-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b0b6f-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="b0b6f-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="b0b6f-133">Skriv i sökrutan hello **videoformatet**väljer **videoformatet** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-133">In hello search box, type **Fuse**, select **Fuse** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Videoformatet i hello resultatlistan](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b0b6f-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0b6f-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="b0b6f-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med säkrad baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-136">In this section, you configure and test Azure AD single sign-on with Fuse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b0b6f-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i säkrad är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Fuse is tooa user in Azure AD.</span></span> <span data-ttu-id="b0b6f-138">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i säkrad behov toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-138">In other words, a link relationship between an Azure AD user and hello related user in Fuse needs toobe established.</span></span>

<span data-ttu-id="b0b6f-139">I säkrad, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-139">In Fuse, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b0b6f-140">tooconfigure och testa Azure AD enkel inloggning med säkrad, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-140">tooconfigure and test Azure AD single sign-on with Fuse, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b0b6f-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b0b6f-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b0b6f-143">**[Skapa en testanvändare säkrad](#create-a-fuse-test-user)**  -toohave en motsvarighet för Britta Simon i säkrad som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-143">**[Create a Fuse test user](#create-a-fuse-test-user)** - toohave a counterpart of Britta Simon in Fuse that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b0b6f-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b0b6f-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b0b6f-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0b6f-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="b0b6f-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din säkrad applikation.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Fuse application.</span></span>

<span data-ttu-id="b0b6f-148">**tooconfigure Azure AD enkel inloggning med säkrad, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-148">**tooconfigure Azure AD single sign-on with Fuse, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0b6f-149">I hello Azure-portalen på hello **videoformatet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-149">In hello Azure portal, on hello **Fuse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="b0b6f-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_samlbase.png)

3. <span data-ttu-id="b0b6f-153">På hello **videoformatet domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-153">On hello **Fuse Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och säkrad domän enkel inloggning information](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_url.png)
    
    <span data-ttu-id="b0b6f-155">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant name>.fusion-universal.com/`</span><span class="sxs-lookup"><span data-stu-id="b0b6f-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant name>.fusion-universal.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b0b6f-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-156">This value is not real.</span></span> <span data-ttu-id="b0b6f-157">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="b0b6f-158">Kontakta [videoformatet klienten supportteamet](mailto:support@fusion-universal.com) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-158">Contact [Fuse Client support team](mailto:support@fusion-universal.com) tooget this value.</span></span> 
 
4. <span data-ttu-id="b0b6f-159">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-159">On hello **SAML Signing Certificate** section, click **Certificate (Raw)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_certificate.png) 

5. <span data-ttu-id="b0b6f-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-fuse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b0b6f-163">På hello **videoformatet Configuration** klickar du på **konfigurera videoformatet** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-163">On hello **Fuse Configuration** section, click **Configure Fuse** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b0b6f-164">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Säkrad konfiguration](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_configure.png) 

7. <span data-ttu-id="b0b6f-166">tooget SSO konfigurerats för ditt program bör du kontakta [säkrad supportteamet](mailto:support@fusion-universal.com) och ge dem hello följande:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-166">tooget SSO configured for your application, contact [Fuse support team](mailto:support@fusion-universal.com) and provide them with hello following:</span></span>

    * <span data-ttu-id="b0b6f-167">hello hämtas **certifikatfil (Raw)**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-167">hello downloaded **Certificate (Raw) file**</span></span>
    * <span data-ttu-id="b0b6f-168">Hej **SAML enkel inloggning tjänst-URL**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-168">hello **SAML Single Sign-On Service URL**</span></span>
    * <span data-ttu-id="b0b6f-169">Hej **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-169">hello **SAML Entity ID**</span></span>
    * <span data-ttu-id="b0b6f-170">Hej **Sign-Out URL**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-170">hello **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="b0b6f-171">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="b0b6f-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b0b6f-172">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b0b6f-173">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b0b6f-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b0b6f-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0b6f-174">Create an Azure AD test user</span></span>

<span data-ttu-id="b0b6f-175">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="b0b6f-177">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0b6f-178">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-fuse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="b0b6f-180">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-fuse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="b0b6f-182">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="b0b6f-184">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b0b6f-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="b0b6f-186">a.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-186">a.</span></span> <span data-ttu-id="b0b6f-187">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-187">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b0b6f-188">b.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-188">b.</span></span> <span data-ttu-id="b0b6f-189">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="b0b6f-190">c.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-190">c.</span></span> <span data-ttu-id="b0b6f-191">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="b0b6f-192">d.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-192">d.</span></span> <span data-ttu-id="b0b6f-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-193">Click **Create**.</span></span>
 
### <a name="create-a-fuse-test-user"></a><span data-ttu-id="b0b6f-194">Skapa en testanvändare säkrad</span><span class="sxs-lookup"><span data-stu-id="b0b6f-194">Create a Fuse test user</span></span>

<span data-ttu-id="b0b6f-195">I det här avsnittet skapar du en användare som kallas Britta Simon i säkrad.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-195">In this section, you create a user called Britta Simon in Fuse.</span></span> <span data-ttu-id="b0b6f-196">Se tillsammans med [säkrad supportteamet](mailto:support@fusion-universal.com) tooadd hello användare i hello säkrad plattform.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-196">Please work with [Fuse support team](mailto:support@fusion-universal.com) tooadd hello users in hello Fuse platform.</span></span>


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b0b6f-197">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b0b6f-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="b0b6f-198">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooFuse.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFuse.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="b0b6f-200">**tooassign Britta Simon tooFuse utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b0b6f-200">**tooassign Britta Simon tooFuse, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0b6f-201">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b0b6f-203">Välj i listan med program hello **videoformatet**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-203">In hello applications list, select **Fuse**.</span></span>

    ![hello säkrad länken i listan med program hello](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_app.png)  

3. <span data-ttu-id="b0b6f-205">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="b0b6f-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-207">Click **Add** button.</span></span> <span data-ttu-id="b0b6f-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="b0b6f-210">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b0b6f-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b0b6f-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b0b6f-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b0b6f-213">Test single sign-on</span></span>

<span data-ttu-id="b0b6f-214">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="b0b6f-215">Du bör få automatiskt inloggade tooyour säkrad programmet när du klickar på hello säkrad panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b0b6f-215">When you click hello Fuse tile in hello Access Panel, you should get automatically signed-on tooyour Fuse application.</span></span>
<span data-ttu-id="b0b6f-216">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b0b6f-216">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b0b6f-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b0b6f-217">Additional resources</span></span>

* [<span data-ttu-id="b0b6f-218">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b0b6f-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b0b6f-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b0b6f-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png

