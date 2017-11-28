---
title: "Självstudier: Azure Active Directory-integrering med Druva | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Druva."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: a1c36c06d6d005e0aa363fbf34efe630e4cca1ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a><span data-ttu-id="4b397-103">Självstudier: Azure Active Directory-integrering med Druva</span><span class="sxs-lookup"><span data-stu-id="4b397-103">Tutorial: Azure Active Directory integration with Druva</span></span>

<span data-ttu-id="4b397-104">I kursen får du lära dig hur toointegrate Druva med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4b397-104">In this tutorial, you learn how toointegrate Druva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4b397-105">Integrera Druva med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4b397-105">Integrating Druva with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4b397-106">Du kan styra i Azure AD som har åtkomst till tooDruva.</span><span class="sxs-lookup"><span data-stu-id="4b397-106">You can control in Azure AD who has access tooDruva.</span></span>
- <span data-ttu-id="4b397-107">Du kan låta dina användare tooautomatically get inloggade tooDruva (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="4b397-107">You can enable your users tooautomatically get signed-on tooDruva (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="4b397-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b397-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="4b397-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4b397-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b397-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4b397-110">Prerequisites</span></span>

<span data-ttu-id="4b397-111">tooconfigure Azure AD-integrering med Druva, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4b397-111">tooconfigure Azure AD integration with Druva, you need hello following items:</span></span>

- <span data-ttu-id="4b397-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4b397-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4b397-113">En Druva enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4b397-113">A Druva single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4b397-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4b397-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4b397-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4b397-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4b397-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4b397-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4b397-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4b397-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4b397-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4b397-118">Scenario description</span></span>
<span data-ttu-id="4b397-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4b397-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4b397-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4b397-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4b397-121">Att lägga till Druva från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4b397-121">Adding Druva from hello gallery</span></span>
2. <span data-ttu-id="4b397-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4b397-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-druva-from-hello-gallery"></a><span data-ttu-id="4b397-123">Att lägga till Druva från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4b397-123">Adding Druva from hello gallery</span></span>
<span data-ttu-id="4b397-124">tooconfigure hello integrering av Druva i Azure AD, behöver du tooadd Druva hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4b397-124">tooconfigure hello integration of Druva into Azure AD, you need tooadd Druva from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4b397-125">**tooadd Druva från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4b397-125">**tooadd Druva from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b397-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4b397-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="4b397-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4b397-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4b397-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4b397-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="4b397-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4b397-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="4b397-133">Skriv i sökrutan hello **Druva**väljer **Druva** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4b397-133">In hello search box, type **Druva**, select **Druva** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Druva i hello resultatlistan](./media/active-directory-saas-druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4b397-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4b397-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="4b397-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Druva baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4b397-136">In this section, you configure and test Azure AD single sign-on with Druva based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4b397-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Druva är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4b397-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Druva is tooa user in Azure AD.</span></span> <span data-ttu-id="4b397-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Druva toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4b397-138">In other words, a link relationship between an Azure AD user and hello related user in Druva needs toobe established.</span></span>

<span data-ttu-id="4b397-139">I Druva, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4b397-139">In Druva, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4b397-140">tooconfigure och testa Azure AD enkel inloggning med Druva, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4b397-140">tooconfigure and test Azure AD single sign-on with Druva, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4b397-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4b397-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4b397-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4b397-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4b397-143">**[Skapa en testanvändare Druva](#create-a-druva-test-user)**  -toohave en motsvarighet för Britta Simon i Druva som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4b397-143">**[Create a Druva test user](#create-a-druva-test-user)** - toohave a counterpart of Britta Simon in Druva that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4b397-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4b397-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4b397-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4b397-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="4b397-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4b397-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="4b397-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Druva program.</span><span class="sxs-lookup"><span data-stu-id="4b397-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Druva application.</span></span>

<span data-ttu-id="4b397-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Druva:**</span><span class="sxs-lookup"><span data-stu-id="4b397-148">**tooconfigure Azure AD single sign-on with Druva, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b397-149">I hello Azure-portalen på hello **Druva** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4b397-149">In hello Azure portal, on hello **Druva** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="4b397-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4b397-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-druva-tutorial/tutorial_druva_samlbase.png)

3. <span data-ttu-id="4b397-153">På hello **Druva domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4b397-153">On hello **Druva Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_url.png)

    <span data-ttu-id="4b397-155">I hello **inloggnings-URL** textruta typen hello URL:`https://cloud.druva.com/home`</span><span class="sxs-lookup"><span data-stu-id="4b397-155">In hello **Sign-on URL** textbox, type hello URL: `https://cloud.druva.com/home`</span></span>

4. <span data-ttu-id="4b397-156">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="4b397-156">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-druva-tutorial/tutorial_druva_certificate.png) 

5. <span data-ttu-id="4b397-158">Tillämpningsprogrammet Druva förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **SAML-Token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4b397-158">Your Druva application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_attribute.png)

6. <span data-ttu-id="4b397-160">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i föregående bild hello och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4b397-160">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello preceding image and perform hello following steps:</span></span>

    | <span data-ttu-id="4b397-161">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="4b397-161">Attribute Name</span></span>      | <span data-ttu-id="4b397-162">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="4b397-162">Attribute Value</span></span>      |
    | ------------------- | -------------------- |
    | <span data-ttu-id="4b397-163">synkroniserad\_auth\_token</span><span class="sxs-lookup"><span data-stu-id="4b397-163">insync\_auth\_token</span></span> |<span data-ttu-id="4b397-164">Ange hello token genererade värde</span><span class="sxs-lookup"><span data-stu-id="4b397-164">Enter hello token generated value</span></span> |
    
    <span data-ttu-id="4b397-165">a.</span><span class="sxs-lookup"><span data-stu-id="4b397-165">a.</span></span> <span data-ttu-id="4b397-166">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4b397-166">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="4b397-169">b.</span><span class="sxs-lookup"><span data-stu-id="4b397-169">b.</span></span> <span data-ttu-id="4b397-170">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="4b397-170">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="4b397-171">c.</span><span class="sxs-lookup"><span data-stu-id="4b397-171">c.</span></span> <span data-ttu-id="4b397-172">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="4b397-172">From hello **Value** list, type hello attribute value shown for that row.</span></span> <span data-ttu-id="4b397-173">hello token genererat värde beskrivs senare i självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="4b397-173">hello token generated value is explained later in tutorial.</span></span>
    
    <span data-ttu-id="4b397-174">d.</span><span class="sxs-lookup"><span data-stu-id="4b397-174">d.</span></span> <span data-ttu-id="4b397-175">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b397-175">Click **Ok**.</span></span>    

7. <span data-ttu-id="4b397-176">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4b397-176">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="4b397-178">På hello **Druva Configuration** klickar du på **konfigurera Druva** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4b397-178">On hello **Druva Configuration** section, click **Configure Druva** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4b397-179">Kopiera hello **Sign-Out Webbadressen och SAML enkel inloggning Service** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4b397-179">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-druva-tutorial/tutorial_druva_configure.png) 

9. <span data-ttu-id="4b397-181">Logga in tooyour Druva företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="4b397-181">In a different web browser window, log in tooyour Druva company site as an administrator.</span></span>

10. <span data-ttu-id="4b397-182">Gå för**hantera \> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="4b397-182">Go too**Manage \> Settings**.</span></span>

    <span data-ttu-id="4b397-183">![Inställningar för](./media/active-directory-saas-druva-tutorial/ic795091.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="4b397-183">![Settings](./media/active-directory-saas-druva-tutorial/ic795091.png "Settings")</span></span>

11. <span data-ttu-id="4b397-184">Dialogrutan Inställningar för enkel inloggning hello utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4b397-184">On hello Single Sign-On Settings dialog, perform hello following steps:</span></span>

    <span data-ttu-id="4b397-185">![Enkel inloggning inställningar](./media/active-directory-saas-druva-tutorial/ic795092.png "enkel inloggning inställningar")</span><span class="sxs-lookup"><span data-stu-id="4b397-185">![Single Sign-On Settings](./media/active-directory-saas-druva-tutorial/ic795092.png "Single Sign-On Settings")</span></span>
    
    <span data-ttu-id="4b397-186">a.</span><span class="sxs-lookup"><span data-stu-id="4b397-186">a.</span></span> <span data-ttu-id="4b397-187">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från hello Azure-portalen i hello **ID providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="4b397-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from hello Azure portal into hello **ID Provider Login URL** textbox.</span></span>
    
    <span data-ttu-id="4b397-188">b.</span><span class="sxs-lookup"><span data-stu-id="4b397-188">b.</span></span> <span data-ttu-id="4b397-189">Klistra in **Sign-Out URL** -värde som du har kopierat från hello Azure-portalen i hello **ID providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="4b397-189">Paste **Sign-Out URL** value, which you have copied from hello Azure portal into hello **ID Provider Logout URL** textbox.</span></span>
    
     <span data-ttu-id="4b397-190">c.</span><span class="sxs-lookup"><span data-stu-id="4b397-190">c.</span></span> <span data-ttu-id="4b397-191">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **ID providern certifikat** textruta</span><span class="sxs-lookup"><span data-stu-id="4b397-191">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **ID Provider Certificate** textbox</span></span>
     
     <span data-ttu-id="4b397-192">d.</span><span class="sxs-lookup"><span data-stu-id="4b397-192">d.</span></span> <span data-ttu-id="4b397-193">tooopen hello **inställningar** klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="4b397-193">tooopen hello **Settings** page, click **Save**.</span></span>

12. <span data-ttu-id="4b397-194">På hello **inställningar** klickar du på **generera SSO Token**.</span><span class="sxs-lookup"><span data-stu-id="4b397-194">On hello **Settings** page, click **Generate SSO Token**.</span></span>

    <span data-ttu-id="4b397-195">![Inställningar för](./media/active-directory-saas-druva-tutorial/ic795093.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="4b397-195">![Settings](./media/active-directory-saas-druva-tutorial/ic795093.png "Settings")</span></span>

13. <span data-ttu-id="4b397-196">På hello **enkel inloggning autentiseringstoken** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4b397-196">On hello **Single Sign-on Authentication Token** dialog, perform hello following steps:</span></span>

    <span data-ttu-id="4b397-197">![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO-Token")</span><span class="sxs-lookup"><span data-stu-id="4b397-197">![SSO Token](./media/active-directory-saas-druva-tutorial/ic795094.png "SSO Token")</span></span>
    
    <span data-ttu-id="4b397-198">a.</span><span class="sxs-lookup"><span data-stu-id="4b397-198">a.</span></span> <span data-ttu-id="4b397-199">Klicka på **kopiera**, klistra in kopierade värdet i hello **värdet** TextBox-kontroll i hello **lägga till attributet** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4b397-199">Click **Copy**, Paste copied value in hello **Value** textbox in hello **Add Attribute** section.</span></span>
    
    <span data-ttu-id="4b397-200">b.</span><span class="sxs-lookup"><span data-stu-id="4b397-200">b.</span></span> <span data-ttu-id="4b397-201">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="4b397-201">Click **Close**.</span></span>

> [!TIP]
> <span data-ttu-id="4b397-202">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4b397-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4b397-203">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4b397-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4b397-204">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4b397-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4b397-205">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4b397-205">Create an Azure AD test user</span></span>

<span data-ttu-id="4b397-206">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4b397-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="4b397-208">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4b397-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b397-209">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="4b397-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-druva-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="4b397-211">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4b397-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-druva-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="4b397-213">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4b397-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-druva-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="4b397-215">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4b397-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-druva-tutorial/create_aaduser_04.png)

    <span data-ttu-id="4b397-217">a.</span><span class="sxs-lookup"><span data-stu-id="4b397-217">a.</span></span> <span data-ttu-id="4b397-218">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4b397-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4b397-219">b.</span><span class="sxs-lookup"><span data-stu-id="4b397-219">b.</span></span> <span data-ttu-id="4b397-220">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4b397-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="4b397-221">c.</span><span class="sxs-lookup"><span data-stu-id="4b397-221">c.</span></span> <span data-ttu-id="4b397-222">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="4b397-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="4b397-223">d.</span><span class="sxs-lookup"><span data-stu-id="4b397-223">d.</span></span> <span data-ttu-id="4b397-224">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4b397-224">Click **Create**.</span></span>
 
### <a name="create-a-druva-test-user"></a><span data-ttu-id="4b397-225">Skapa en testanvändare Druva</span><span class="sxs-lookup"><span data-stu-id="4b397-225">Create a Druva test user</span></span>

<span data-ttu-id="4b397-226">I ordning tooenable Azure AD-användare toolog i tooDruva, måste de etableras i Druva.</span><span class="sxs-lookup"><span data-stu-id="4b397-226">In order tooenable Azure AD users toolog in tooDruva, they must be provisioned into Druva.</span></span> <span data-ttu-id="4b397-227">Hello gäller Druva är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4b397-227">In hello case of Druva, provisioning is a manual task.</span></span>

<span data-ttu-id="4b397-228">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="4b397-228">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b397-229">Logga in tooyour **Druva** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="4b397-229">Log in tooyour **Druva** company site as administrator.</span></span>

2. <span data-ttu-id="4b397-230">Gå för**hantera \> användare**.</span><span class="sxs-lookup"><span data-stu-id="4b397-230">Go too**Manage \> Users**.</span></span>
   
   <span data-ttu-id="4b397-231">![Hantera användare](./media/active-directory-saas-druva-tutorial/ic795097.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="4b397-231">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795097.png "Manage Users")</span></span>

3. <span data-ttu-id="4b397-232">Klicka på **skapa nya**.</span><span class="sxs-lookup"><span data-stu-id="4b397-232">Click **Create New**.</span></span>
   
   <span data-ttu-id="4b397-233">![Hantera användare](./media/active-directory-saas-druva-tutorial/ic795098.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="4b397-233">![Manage Users](./media/active-directory-saas-druva-tutorial/ic795098.png "Manage Users")</span></span>

4. <span data-ttu-id="4b397-234">Dialogrutan Skapa ny användare hello utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4b397-234">On hello Create New User dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="4b397-235">![Skapa ny användare](./media/active-directory-saas-druva-tutorial/ic795099.png "Skapa ny användare")</span><span class="sxs-lookup"><span data-stu-id="4b397-235">![Create NewUser](./media/active-directory-saas-druva-tutorial/ic795099.png "Create NewUser")</span></span>
   
   <span data-ttu-id="4b397-236">a.</span><span class="sxs-lookup"><span data-stu-id="4b397-236">a.</span></span> <span data-ttu-id="4b397-237">I hello **e-postadress** textruta ange hello e-postadress för användaren som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="4b397-237">In hello **Email address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>
   
   <span data-ttu-id="4b397-238">b.</span><span class="sxs-lookup"><span data-stu-id="4b397-238">b.</span></span> <span data-ttu-id="4b397-239">I hello **namn** textruta anger hello namnet på användaren som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4b397-239">In hello **Name** textbox, enter hello name of user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="4b397-240">c.</span><span class="sxs-lookup"><span data-stu-id="4b397-240">c.</span></span> <span data-ttu-id="4b397-241">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="4b397-241">Click **Create User**.</span></span>

>[!NOTE]
><span data-ttu-id="4b397-242">Du kan använda något annat Druva användarens konto skapas verktyg eller API: er som tillhandahålls av Druva tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4b397-242">You can use any other Druva user account creation tools or APIs provided by Druva tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="4b397-243">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4b397-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="4b397-244">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooDruva.</span><span class="sxs-lookup"><span data-stu-id="4b397-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooDruva.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="4b397-246">**tooassign Britta Simon tooDruva utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4b397-246">**tooassign Britta Simon tooDruva, perform hello following steps:**</span></span>

1. <span data-ttu-id="4b397-247">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4b397-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4b397-249">Välj i listan med program hello **Druva**.</span><span class="sxs-lookup"><span data-stu-id="4b397-249">In hello applications list, select **Druva**.</span></span>

    ![Hej Druva länken i listan med program hello](./media/active-directory-saas-druva-tutorial/tutorial_druva_app.png)  

3. <span data-ttu-id="4b397-251">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4b397-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="4b397-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4b397-253">Click **Add** button.</span></span> <span data-ttu-id="4b397-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4b397-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="4b397-256">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4b397-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4b397-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4b397-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4b397-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4b397-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="4b397-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4b397-259">Test single sign-on</span></span>

<span data-ttu-id="4b397-260">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4b397-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4b397-261">Du bör få automatiskt inloggade tooyour Druva programmet när du klickar på hello Druva panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4b397-261">When you click hello Druva tile in hello Access Panel, you should get automatically signed-on tooyour Druva application.</span></span>
<span data-ttu-id="4b397-262">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4b397-262">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="4b397-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4b397-263">Additional resources</span></span>

* [<span data-ttu-id="4b397-264">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4b397-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4b397-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4b397-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-druva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-druva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-druva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-druva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-druva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-druva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-druva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-druva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-druva-tutorial/tutorial_general_203.png

