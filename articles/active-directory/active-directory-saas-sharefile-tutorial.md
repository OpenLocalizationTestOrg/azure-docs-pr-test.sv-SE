---
title: "Självstudier: Azure Active Directory-integrering med Citrix ShareFile | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Citrix ShareFile."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.openlocfilehash: d7eaf140e56c40f9f621062849dd8558588ffd1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a><span data-ttu-id="384c1-103">Självstudier: Azure Active Directory-integrering med Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="384c1-103">Tutorial: Azure Active Directory integration with Citrix ShareFile</span></span>

<span data-ttu-id="384c1-104">I kursen får du lära dig hur toointegrate Citrix ShareFile med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="384c1-104">In this tutorial, you learn how toointegrate Citrix ShareFile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="384c1-105">Integrera Citrix ShareFile med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="384c1-105">Integrating Citrix ShareFile with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="384c1-106">Du kan styra i Azure AD som har åtkomst till tooCitrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="384c1-106">You can control in Azure AD who has access tooCitrix ShareFile.</span></span>
- <span data-ttu-id="384c1-107">Du kan låta dina användare tooautomatically get inloggade tooCitrix ShareFile (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="384c1-107">You can enable your users tooautomatically get signed-on tooCitrix ShareFile (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="384c1-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="384c1-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="384c1-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="384c1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="384c1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="384c1-110">Prerequisites</span></span>

<span data-ttu-id="384c1-111">tooconfigure Azure AD-integrering med Citrix ShareFile måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="384c1-111">tooconfigure Azure AD integration with Citrix ShareFile, you need hello following items:</span></span>

- <span data-ttu-id="384c1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="384c1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="384c1-113">En Citrix ShareFile enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="384c1-113">A Citrix ShareFile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="384c1-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="384c1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="384c1-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="384c1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="384c1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="384c1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="384c1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="384c1-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="384c1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="384c1-118">Scenario description</span></span>
<span data-ttu-id="384c1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="384c1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="384c1-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="384c1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="384c1-121">Lägg till Citrix ShareFile från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="384c1-121">Add Citrix ShareFile from hello gallery</span></span>
2. <span data-ttu-id="384c1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="384c1-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-citrix-sharefile-from-hello-gallery"></a><span data-ttu-id="384c1-123">Lägg till Citrix ShareFile från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="384c1-123">Add Citrix ShareFile from hello gallery</span></span>
<span data-ttu-id="384c1-124">tooconfigure hello integrering av Citrix ShareFile i Azure AD, behöver du tooadd Citrix ShareFile hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="384c1-124">tooconfigure hello integration of Citrix ShareFile into Azure AD, you need tooadd Citrix ShareFile from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="384c1-125">**tooadd Citrix ShareFile från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="384c1-125">**tooadd Citrix ShareFile from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="384c1-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="384c1-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="384c1-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="384c1-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="384c1-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="384c1-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="384c1-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="384c1-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="384c1-133">Skriv i sökrutan hello **Citrix ShareFile**väljer **Citrix ShareFile** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="384c1-133">In hello search box, type **Citrix ShareFile**, select **Citrix ShareFile** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Citrix ShareFile i hello resulterar lista](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="384c1-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="384c1-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="384c1-136">Du konfigurera och testa Azure AD enkel inloggning med Citrix ShareFile baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="384c1-136">In this section, you configure and test Azure AD single sign-on with Citrix ShareFile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="384c1-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Citrix ShareFile är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="384c1-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Citrix ShareFile is tooa user in Azure AD.</span></span> <span data-ttu-id="384c1-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Citrix ShareFile toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="384c1-138">In other words, a link relationship between an Azure AD user and hello related user in Citrix ShareFile needs toobe established.</span></span>

<span data-ttu-id="384c1-139">Tilldela hello värdet för hello i Citrix ShareFile **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="384c1-139">In Citrix ShareFile, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="384c1-140">tooconfigure och testa Azure AD enkel inloggning med Citrix ShareFile, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="384c1-140">tooconfigure and test Azure AD single sign-on with Citrix ShareFile, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="384c1-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="384c1-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="384c1-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="384c1-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="384c1-143">**[Skapa en testanvändare Citrix ShareFile](#create-a-citrix-sharefile-test-user)**  -toohave en motsvarighet för Britta Simon i Citrix ShareFile som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="384c1-143">**[Create a Citrix ShareFile test user](#create-a-citrix-sharefile-test-user)** - toohave a counterpart of Britta Simon in Citrix ShareFile that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="384c1-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="384c1-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="384c1-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="384c1-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="384c1-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="384c1-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="384c1-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Citrix ShareFile-program.</span><span class="sxs-lookup"><span data-stu-id="384c1-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Citrix ShareFile application.</span></span>

<span data-ttu-id="384c1-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Citrix ShareFile:**</span><span class="sxs-lookup"><span data-stu-id="384c1-148">**tooconfigure Azure AD single sign-on with Citrix ShareFile, perform hello following steps:**</span></span>

1. <span data-ttu-id="384c1-149">I hello Azure-portalen på hello **Citrix ShareFile** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="384c1-149">In hello Azure portal, on hello **Citrix ShareFile** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="384c1-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="384c1-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_samlbase.png)

3. <span data-ttu-id="384c1-153">På hello **Citrix ShareFile domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="384c1-153">On hello **Citrix ShareFile Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Citrix ShareFile domän med enkel inloggning information](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_url.png)
    
    <span data-ttu-id="384c1-155">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenant-name>.sharefile.com/saml/login`</span><span class="sxs-lookup"><span data-stu-id="384c1-155">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.sharefile.com/saml/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="384c1-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="384c1-156">This value is not real.</span></span> <span data-ttu-id="384c1-157">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="384c1-157">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="384c1-158">Kontakta [Citrix ShareFile klienten supportteamet](https://www.citrix.co.in/products/sharefile/support.html) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="384c1-158">Contact [Citrix ShareFile Client support team](https://www.citrix.co.in/products/sharefile/support.html) tooget this value.</span></span> 

4. <span data-ttu-id="384c1-159">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="384c1-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_certificate.png) 

5. <span data-ttu-id="384c1-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="384c1-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-sharefile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="384c1-163">På hello **Citrix-konfiguration för ShareFile** klickar du på **konfigurera Citrix ShareFile** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="384c1-163">On hello **Citrix ShareFile Configuration** section, click **Configure Citrix ShareFile** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="384c1-164">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="384c1-164">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Citrix ShareFile konfiguration](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_configure.png) 

7. <span data-ttu-id="384c1-166">Logga in i ett annat webbläsarfönster din **Citrix ShareFile** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="384c1-166">In a different web browser window, log into your **Citrix ShareFile** company site as an administrator.</span></span>

8. <span data-ttu-id="384c1-167">Klicka i hello verktygsfältet hello längst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="384c1-167">In hello toolbar on hello top, click **Admin**.</span></span>

9. <span data-ttu-id="384c1-168">Hello vänstra navigeringsfönstret och välj **Konfigurera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="384c1-168">In hello left navigation pane, select **Configure Single Sign-On**.</span></span>
   
    <span data-ttu-id="384c1-169">![Kontot Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "konto Administration")</span><span class="sxs-lookup"><span data-stu-id="384c1-169">![Account Administration](./media/active-directory-saas-sharefile-tutorial/ic773627.png "Account Administration")</span></span>

10. <span data-ttu-id="384c1-170">På hello **enkel inloggning / SAML 2.0-konfiguration** dialogrutan sidan under **grundläggande inställningar**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="384c1-170">On hello **Single Sign-On/ SAML 2.0 Configuration** dialog page under **Basic Settings**, perform hello following steps:</span></span>
   
    <span data-ttu-id="384c1-171">![Enkel inloggning](./media/active-directory-saas-sharefile-tutorial/ic773628.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="384c1-171">![Single sign-on](./media/active-directory-saas-sharefile-tutorial/ic773628.png "Single sign-on")</span></span>
   
    <span data-ttu-id="384c1-172">a.</span><span class="sxs-lookup"><span data-stu-id="384c1-172">a.</span></span> <span data-ttu-id="384c1-173">Klicka på **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="384c1-173">Click **Enable SAML**.</span></span>
    
    <span data-ttu-id="384c1-174">b.</span><span class="sxs-lookup"><span data-stu-id="384c1-174">b.</span></span> <span data-ttu-id="384c1-175">I **din IDP utfärdaren / enhets-ID** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="384c1-175">In **Your IDP Issuer/ Entity ID** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="384c1-176">c.</span><span class="sxs-lookup"><span data-stu-id="384c1-176">c.</span></span> <span data-ttu-id="384c1-177">Klicka på **ändra** nästa toohello **X.509-certifikat** fält och sedan ladda upp hello certifikat du hämtade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="384c1-177">Click **Change** next toohello **X.509 Certificate** field and then upload hello certificate you downloaded from hello Azure portal.</span></span>
    
    <span data-ttu-id="384c1-178">d.</span><span class="sxs-lookup"><span data-stu-id="384c1-178">d.</span></span> <span data-ttu-id="384c1-179">I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="384c1-179">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="384c1-180">e.</span><span class="sxs-lookup"><span data-stu-id="384c1-180">e.</span></span> <span data-ttu-id="384c1-181">I **logga ut URL** textruta klistra in hello värdet för **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="384c1-181">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

11. <span data-ttu-id="384c1-182">Klicka på **spara** på hello Citrix ShareFile-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="384c1-182">Click **Save** on hello Citrix ShareFile management portal.</span></span>

> [!TIP]
> <span data-ttu-id="384c1-183">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="384c1-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="384c1-184">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="384c1-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="384c1-185">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="384c1-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="384c1-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="384c1-186">Create an Azure AD test user</span></span>

<span data-ttu-id="384c1-187">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="384c1-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="384c1-189">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="384c1-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="384c1-190">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="384c1-190">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="384c1-192">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="384c1-192">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="384c1-194">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="384c1-194">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="384c1-196">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="384c1-196">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png)

    <span data-ttu-id="384c1-198">a.</span><span class="sxs-lookup"><span data-stu-id="384c1-198">a.</span></span> <span data-ttu-id="384c1-199">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="384c1-199">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="384c1-200">b.</span><span class="sxs-lookup"><span data-stu-id="384c1-200">b.</span></span> <span data-ttu-id="384c1-201">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="384c1-201">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="384c1-202">c.</span><span class="sxs-lookup"><span data-stu-id="384c1-202">c.</span></span> <span data-ttu-id="384c1-203">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="384c1-203">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="384c1-204">d.</span><span class="sxs-lookup"><span data-stu-id="384c1-204">d.</span></span> <span data-ttu-id="384c1-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="384c1-205">Click **Create**.</span></span>
 
### <a name="create-a-citrix-sharefile-test-user"></a><span data-ttu-id="384c1-206">Skapa en testanvändare Citrix ShareFile</span><span class="sxs-lookup"><span data-stu-id="384c1-206">Create a Citrix ShareFile test user</span></span>

<span data-ttu-id="384c1-207">I ordning tooenable Azure AD-användare toolog till Citrix ShareFile, måste de etableras i Citrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="384c1-207">In order tooenable Azure AD users toolog into Citrix ShareFile, they must be provisioned into Citrix ShareFile.</span></span> <span data-ttu-id="384c1-208">Hello gäller Citrix ShareFile är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="384c1-208">In hello case of Citrix ShareFile, provisioning is a manual task.</span></span>

<span data-ttu-id="384c1-209">**tooprovision ett användarkonto, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="384c1-209">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="384c1-210">Logga in tooyour **Citrix ShareFile** klient.</span><span class="sxs-lookup"><span data-stu-id="384c1-210">Log in tooyour **Citrix ShareFile** tenant.</span></span>

2. <span data-ttu-id="384c1-211">Klicka på **hantera användare \> hantera användare Home \> + skapa medarbetare**.</span><span class="sxs-lookup"><span data-stu-id="384c1-211">Click **Manage Users \> Manage Users Home \> + Create Employee**.</span></span>
   
   <span data-ttu-id="384c1-212">![Skapa medarbetare](./media/active-directory-saas-sharefile-tutorial/IC781050.png "skapa medarbetare")</span><span class="sxs-lookup"><span data-stu-id="384c1-212">![Create Employee](./media/active-directory-saas-sharefile-tutorial/IC781050.png "Create Employee")</span></span>

3. <span data-ttu-id="384c1-213">På hello **grundläggande Information** avsnittet, utföra nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="384c1-213">On hello **Basic Information** section, perform below steps:</span></span>
   
   <span data-ttu-id="384c1-214">![Grundläggande Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "grundläggande Information")</span><span class="sxs-lookup"><span data-stu-id="384c1-214">![Basic Information](./media/active-directory-saas-sharefile-tutorial/IC799951.png "Basic Information")</span></span>
   
   <span data-ttu-id="384c1-215">a.</span><span class="sxs-lookup"><span data-stu-id="384c1-215">a.</span></span> <span data-ttu-id="384c1-216">I hello **e-postadress** textruta typen hello e-postadress Britta Simon som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="384c1-216">In hello **Email Address** textbox, type hello email address of Britta Simon as **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="384c1-217">b.</span><span class="sxs-lookup"><span data-stu-id="384c1-217">b.</span></span> <span data-ttu-id="384c1-218">I hello **Förnamn** textruta typen **Förnamn** för användare som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="384c1-218">In hello **First Name** textbox, type **first name** of user as **Britta**.</span></span>
   
   <span data-ttu-id="384c1-219">c.</span><span class="sxs-lookup"><span data-stu-id="384c1-219">c.</span></span> <span data-ttu-id="384c1-220">I hello **efternamn** textruta typen **efternamn** för användare som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="384c1-220">In hello **Last Name** textbox, type **last name** of user as **Simon**.</span></span>

4. <span data-ttu-id="384c1-221">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="384c1-221">Click **Add User**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="384c1-222">hello Azure AD-kontot innehavaren kommer ett e-postmeddelande och följ en länk tooconfirm sitt konto innan den aktiveras. Du kan använda något annat Citrix ShareFile användarens konto skapas verktyg eller API: er som tillhandahålls av Citrix ShareFile tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="384c1-222">hello Azure AD account holder will receive an email and follow a link tooconfirm their account before it becomes active.You can use any other Citrix ShareFile user account creation tools or APIs provided by Citrix ShareFile tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="384c1-223">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="384c1-223">Assign hello Azure AD test user</span></span>

<span data-ttu-id="384c1-224">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooCitrix ShareFile.</span><span class="sxs-lookup"><span data-stu-id="384c1-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCitrix ShareFile.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="384c1-226">**tooassign Britta Simon tooCitrix ShareFile, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="384c1-226">**tooassign Britta Simon tooCitrix ShareFile, perform hello following steps:**</span></span>

1. <span data-ttu-id="384c1-227">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="384c1-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="384c1-229">Välj i listan med program hello **Citrix ShareFile**.</span><span class="sxs-lookup"><span data-stu-id="384c1-229">In hello applications list, select **Citrix ShareFile**.</span></span>

    ![hello Citrix ShareFile länken i listan med program hello](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_app.png)  

3. <span data-ttu-id="384c1-231">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="384c1-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="384c1-233">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="384c1-233">Click **Add** button.</span></span> <span data-ttu-id="384c1-234">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="384c1-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="384c1-236">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="384c1-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="384c1-237">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="384c1-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="384c1-238">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="384c1-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="384c1-239">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="384c1-239">Test single sign-on</span></span>

<span data-ttu-id="384c1-240">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="384c1-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="384c1-241">Du bör få automatiskt inloggade tooyour Citrix ShareFile programmet när du klickar på hello Citrix ShareFile panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="384c1-241">When you click hello Citrix ShareFile tile in hello Access Panel, you should get automatically signed-on tooyour Citrix ShareFile application.</span></span>
<span data-ttu-id="384c1-242">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="384c1-242">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="384c1-243">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="384c1-243">Additional resources</span></span>

* [<span data-ttu-id="384c1-244">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="384c1-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="384c1-245">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="384c1-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png

