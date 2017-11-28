---
title: "Självstudier: Azure Active Directory-integrering med Onit | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Onit."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bc479a28-8fcd-493f-ac53-681975a5149c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jeedes
ms.openlocfilehash: 9e12449e5bf7f169b3cadfaa12438ac5d52ed8f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-onit"></a><span data-ttu-id="aff54-103">Självstudier: Azure Active Directory-integrering med Onit</span><span class="sxs-lookup"><span data-stu-id="aff54-103">Tutorial: Azure Active Directory integration with Onit</span></span>

<span data-ttu-id="aff54-104">I kursen får du lära dig hur toointegrate Onit med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="aff54-104">In this tutorial, you learn how toointegrate Onit with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="aff54-105">Integrera Onit med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="aff54-105">Integrating Onit with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="aff54-106">Du kan styra i Azure AD som har åtkomst till tooOnit.</span><span class="sxs-lookup"><span data-stu-id="aff54-106">You can control in Azure AD who has access tooOnit.</span></span>
- <span data-ttu-id="aff54-107">Du kan låta dina användare tooautomatically get inloggade tooOnit (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="aff54-107">You can enable your users tooautomatically get signed-on tooOnit (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="aff54-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aff54-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="aff54-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="aff54-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aff54-110">Krav</span><span class="sxs-lookup"><span data-stu-id="aff54-110">Prerequisites</span></span>

<span data-ttu-id="aff54-111">tooconfigure Azure AD-integrering med Onit, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="aff54-111">tooconfigure Azure AD integration with Onit, you need hello following items:</span></span>

- <span data-ttu-id="aff54-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="aff54-112">An Azure AD subscription</span></span>
- <span data-ttu-id="aff54-113">En Onit enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="aff54-113">An Onit single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="aff54-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="aff54-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="aff54-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="aff54-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="aff54-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="aff54-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="aff54-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="aff54-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="aff54-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="aff54-118">Scenario description</span></span>

<span data-ttu-id="aff54-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="aff54-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="aff54-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="aff54-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="aff54-121">Att lägga till Onit från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="aff54-121">Adding Onit from hello gallery</span></span>
2. <span data-ttu-id="aff54-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aff54-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-onit-from-hello-gallery"></a><span data-ttu-id="aff54-123">Att lägga till Onit från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="aff54-123">Adding Onit from hello gallery</span></span>
<span data-ttu-id="aff54-124">tooconfigure hello integrering av Onit i Azure AD, behöver du tooadd Onit hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="aff54-124">tooconfigure hello integration of Onit into Azure AD, you need tooadd Onit from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="aff54-125">**tooadd Onit från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aff54-125">**tooadd Onit from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="aff54-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="aff54-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="aff54-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="aff54-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="aff54-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="aff54-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="aff54-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aff54-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="aff54-133">Skriv i sökrutan hello **Onit**väljer **Onit** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="aff54-133">In hello search box, type **Onit**, select **Onit** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Onit i hello resultatlistan](./media/active-directory-saas-onit-tutorial/tutorial_onit_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="aff54-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aff54-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="aff54-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Onit baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="aff54-136">In this section, you configure and test Azure AD single sign-on with Onit based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="aff54-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Onit är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aff54-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Onit is tooa user in Azure AD.</span></span> <span data-ttu-id="aff54-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Onit toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="aff54-138">In other words, a link relationship between an Azure AD user and hello related user in Onit needs toobe established.</span></span>

<span data-ttu-id="aff54-139">I Onit, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="aff54-139">In Onit, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="aff54-140">tooconfigure och testa Azure AD enkel inloggning med Onit, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="aff54-140">tooconfigure and test Azure AD single sign-on with Onit, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="aff54-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="aff54-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="aff54-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aff54-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="aff54-143">**[Skapa en testanvändare Onit](#create-an-onit-test-user)**  -toohave en motsvarighet för Britta Simon i Onit som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="aff54-143">**[Create an Onit test user](#create-an-onit-test-user)** - toohave a counterpart of Britta Simon in Onit that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="aff54-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aff54-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="aff54-145">**[Testa enkel inloggning](#test-single-sign-on)**  tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="aff54-145">**[Test single sign-on](#test-single-sign-on)** tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="aff54-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aff54-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="aff54-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Onit program.</span><span class="sxs-lookup"><span data-stu-id="aff54-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Onit application.</span></span>

<span data-ttu-id="aff54-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Onit:**</span><span class="sxs-lookup"><span data-stu-id="aff54-148">**tooconfigure Azure AD single sign-on with Onit, perform hello following steps:**</span></span>

1. <span data-ttu-id="aff54-149">I hello Azure-portalen på hello **Onit** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="aff54-149">In hello Azure portal, on hello **Onit** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="aff54-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="aff54-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-onit-tutorial/tutorial_onit_samlbase.png)

3. <span data-ttu-id="aff54-153">På hello **Onit domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aff54-153">On hello **Onit Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och Onit domän med enkel inloggning information](./media/active-directory-saas-onit-tutorial/tutorial_onit_url.png)

    <span data-ttu-id="aff54-155">a.</span><span class="sxs-lookup"><span data-stu-id="aff54-155">a.</span></span> <span data-ttu-id="aff54-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="aff54-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<sub-domain>.onit.com`</span></span>

    <span data-ttu-id="aff54-157">b.</span><span class="sxs-lookup"><span data-stu-id="aff54-157">b.</span></span> <span data-ttu-id="aff54-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<sub-domain>.onit.com`</span><span class="sxs-lookup"><span data-stu-id="aff54-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<sub-domain>.onit.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="aff54-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="aff54-159">These values are not real.</span></span> <span data-ttu-id="aff54-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="aff54-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="aff54-161">Kontakta [Onit klienten supportteamet](https://www.onit.com/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="aff54-161">Contact [Onit Client support team](https://www.onit.com/support) tooget these values.</span></span> 
 
4. <span data-ttu-id="aff54-162">På hello **SAML-signeringscertifikat** avsnitt, kopiera hello **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="aff54-162">On hello **SAML Signing Certificate** section, copy hello **THUMBPRINT** value of certificate.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-onit-tutorial/tutorial_onit_certificate.png) 

5. <span data-ttu-id="aff54-164">Onit program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="aff54-164">Onit application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="aff54-165">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="aff54-165">Please configure hello following claims for this application.</span></span> <span data-ttu-id="aff54-166">Du kan hantera hello värden för attributen från hello **”Atrribute”** fliken av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="aff54-166">You can manage hello values of these attributes from hello **"Atrribute"** tab of hello application.</span></span> <span data-ttu-id="aff54-167">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="aff54-167">hello following screenshot shows an example for this.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-onit-tutorial/tutorial_onit_attribute.png) 

6. <span data-ttu-id="aff54-169">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attributet enligt hello bild och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aff54-169">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image and perform hello following steps:</span></span>
    
    | <span data-ttu-id="aff54-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="aff54-170">Attribute Name</span></span> | <span data-ttu-id="aff54-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="aff54-171">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="aff54-172">E-post</span><span class="sxs-lookup"><span data-stu-id="aff54-172">email</span></span> | <span data-ttu-id="aff54-173">User.Mail</span><span class="sxs-lookup"><span data-stu-id="aff54-173">user.mail</span></span> |
    
    <span data-ttu-id="aff54-174">a.</span><span class="sxs-lookup"><span data-stu-id="aff54-174">a.</span></span> <span data-ttu-id="aff54-175">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aff54-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-onit-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-onit-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="aff54-178">b.</span><span class="sxs-lookup"><span data-stu-id="aff54-178">b.</span></span> <span data-ttu-id="aff54-179">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="aff54-179">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="aff54-180">c.</span><span class="sxs-lookup"><span data-stu-id="aff54-180">c.</span></span> <span data-ttu-id="aff54-181">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="aff54-181">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="aff54-182">d.</span><span class="sxs-lookup"><span data-stu-id="aff54-182">d.</span></span> <span data-ttu-id="aff54-183">Lämna hello **Namespace** tomt.</span><span class="sxs-lookup"><span data-stu-id="aff54-183">Leave hello **Namespace** blank.</span></span>
    
    <span data-ttu-id="aff54-184">e.</span><span class="sxs-lookup"><span data-stu-id="aff54-184">e.</span></span> <span data-ttu-id="aff54-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="aff54-185">Click **Ok**.</span></span>

7. <span data-ttu-id="aff54-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="aff54-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-onit-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="aff54-188">På hello **Onit Configuration** klickar du på **konfigurera Onit** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="aff54-188">On hello **Onit Configuration** section, click **Configure Onit** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="aff54-189">Kopiera hello **Sign-Out URL, SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="aff54-189">Copy hello **Sign-Out URL, SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Onit konfiguration](./media/active-directory-saas-onit-tutorial/tutorial_onit_configure.png)

9. <span data-ttu-id="aff54-191">Logga in på webbplatsen Onit företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="aff54-191">In a different web browser window, log into your Onit company site as an administrator.</span></span>

10. <span data-ttu-id="aff54-192">Hello-menyn överst hello **Administration**.</span><span class="sxs-lookup"><span data-stu-id="aff54-192">In hello menu on hello top, click **Administration**.</span></span>
   
   <span data-ttu-id="aff54-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="aff54-193">![Administration](./media/active-directory-saas-onit-tutorial/IC791174.png "Administration")</span></span>
11. <span data-ttu-id="aff54-194">Klicka på **redigera Corporation**.</span><span class="sxs-lookup"><span data-stu-id="aff54-194">Click **Edit Corporation**.</span></span>
   
   <span data-ttu-id="aff54-195">![Redigera Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "redigera Corporation")</span><span class="sxs-lookup"><span data-stu-id="aff54-195">![Edit Corporation](./media/active-directory-saas-onit-tutorial/IC791175.png "Edit Corporation")</span></span>
   
12. <span data-ttu-id="aff54-196">Klicka på hello **säkerhet** fliken.</span><span class="sxs-lookup"><span data-stu-id="aff54-196">Click hello **Security** tab.</span></span>
    
    <span data-ttu-id="aff54-197">![Redigera företagsinformation](./media/active-directory-saas-onit-tutorial/IC791176.png "Redigera företagsinformation")</span><span class="sxs-lookup"><span data-stu-id="aff54-197">![Edit Company Information](./media/active-directory-saas-onit-tutorial/IC791176.png "Edit Company Information")</span></span>

13. <span data-ttu-id="aff54-198">På hello **säkerhet** fliken, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aff54-198">On hello **Security** tab, perform hello following steps:</span></span>

    <span data-ttu-id="aff54-199">![Enkel inloggning](./media/active-directory-saas-onit-tutorial/IC791177.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="aff54-199">![Single Sign-On](./media/active-directory-saas-onit-tutorial/IC791177.png "Single Sign-On")</span></span>

    <span data-ttu-id="aff54-200">a.</span><span class="sxs-lookup"><span data-stu-id="aff54-200">a.</span></span> <span data-ttu-id="aff54-201">Som **autentiseringsstrategi**väljer **enkel inloggning och lösenord**.</span><span class="sxs-lookup"><span data-stu-id="aff54-201">As **Authentication Strategy**, select **Single Sign On and Password**.</span></span>
    
    <span data-ttu-id="aff54-202">b.</span><span class="sxs-lookup"><span data-stu-id="aff54-202">b.</span></span> <span data-ttu-id="aff54-203">I **Idp mål-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aff54-203">In **Idp Target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="aff54-204">c.</span><span class="sxs-lookup"><span data-stu-id="aff54-204">c.</span></span> <span data-ttu-id="aff54-205">I **Idp logga ut URL** textruta klistra in hello värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aff54-205">In **Idp logout URL** textbox, paste hello value of  **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="aff54-206">d.</span><span class="sxs-lookup"><span data-stu-id="aff54-206">d.</span></span> <span data-ttu-id="aff54-207">I **Idp Cert fingeravtryck (SHA1)** textruta klistra in hello **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="aff54-207">In **Idp Cert Fingerprint (SHA1)** textbox, paste hello  **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="aff54-208">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="aff54-208">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="aff54-209">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="aff54-209">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="aff54-210">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="aff54-210">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="aff54-211">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="aff54-211">Create an Azure AD test user</span></span>

<span data-ttu-id="aff54-212">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aff54-212">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="aff54-214">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aff54-214">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="aff54-215">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="aff54-215">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-onit-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="aff54-217">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="aff54-217">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-onit-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="aff54-219">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aff54-219">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-onit-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="aff54-221">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aff54-221">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-onit-tutorial/create_aaduser_04.png)

    <span data-ttu-id="aff54-223">a.</span><span class="sxs-lookup"><span data-stu-id="aff54-223">a.</span></span> <span data-ttu-id="aff54-224">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="aff54-224">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="aff54-225">b.</span><span class="sxs-lookup"><span data-stu-id="aff54-225">b.</span></span> <span data-ttu-id="aff54-226">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="aff54-226">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="aff54-227">c.</span><span class="sxs-lookup"><span data-stu-id="aff54-227">c.</span></span> <span data-ttu-id="aff54-228">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="aff54-228">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="aff54-229">d.</span><span class="sxs-lookup"><span data-stu-id="aff54-229">d.</span></span> <span data-ttu-id="aff54-230">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="aff54-230">Click **Create**.</span></span>
 
### <a name="create-an-onit-test-user"></a><span data-ttu-id="aff54-231">Skapa en testanvändare Onit</span><span class="sxs-lookup"><span data-stu-id="aff54-231">Create an Onit test user</span></span>

<span data-ttu-id="aff54-232">I ordning tooenable Azure AD-användare toolog i Onit, måste de etableras i Onit.</span><span class="sxs-lookup"><span data-stu-id="aff54-232">In order tooenable Azure AD users toolog into Onit, they must be provisioned into Onit.</span></span>  

<span data-ttu-id="aff54-233">Hello gäller Onit är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="aff54-233">In hello case of Onit, provisioning is a manual task.</span></span>

<span data-ttu-id="aff54-234">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="aff54-234">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="aff54-235">Logga in tooyour **Onit** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="aff54-235">Sign on tooyour **Onit** company site as an administrator.</span></span>
2. <span data-ttu-id="aff54-236">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="aff54-236">Click **Add User**.</span></span>
   
   <span data-ttu-id="aff54-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="aff54-237">![Administration](./media/active-directory-saas-onit-tutorial/IC791180.png "Administration")</span></span>
3. <span data-ttu-id="aff54-238">På hello **Lägg till användare** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="aff54-238">On hello **Add User** dialog page, perform hello following steps:</span></span>
   
   <span data-ttu-id="aff54-239">![Lägg till användare](./media/active-directory-saas-onit-tutorial/IC791181.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="aff54-239">![Add User](./media/active-directory-saas-onit-tutorial/IC791181.png "Add User")</span></span>
   
  1. <span data-ttu-id="aff54-240">Typen hello **namn** och hello **e-postadress** av ett giltigt Azure AD-kontot som du vill använda tooprovision hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="aff54-240">Type hello **Name** and hello **Email Address** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span>
  2. <span data-ttu-id="aff54-241">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="aff54-241">Click **Create**.</span></span>    
   
 > [!NOTE]
 > <span data-ttu-id="aff54-242">hello Azure Active Directory användare får ett e-postmeddelande och följer en länk tooconfirm sitt konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="aff54-242">hello Azure Active Directory account holder receives an email and follows a link tooconfirm their account before it becomes active.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="aff54-243">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="aff54-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="aff54-244">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooOnit.</span><span class="sxs-lookup"><span data-stu-id="aff54-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooOnit.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="aff54-246">**tooassign Britta Simon tooOnit utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="aff54-246">**tooassign Britta Simon tooOnit, perform hello following steps:**</span></span>

1. <span data-ttu-id="aff54-247">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="aff54-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="aff54-249">Välj i listan med program hello **Onit**.</span><span class="sxs-lookup"><span data-stu-id="aff54-249">In hello applications list, select **Onit**.</span></span>

    ![Hej Onit länken i listan med program hello](./media/active-directory-saas-onit-tutorial/tutorial_onit_app.png)  

3. <span data-ttu-id="aff54-251">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="aff54-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="aff54-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="aff54-253">Click **Add** button.</span></span> <span data-ttu-id="aff54-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aff54-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="aff54-256">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="aff54-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="aff54-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aff54-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="aff54-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aff54-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="aff54-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aff54-259">Test single sign-on</span></span>

<span data-ttu-id="aff54-260">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="aff54-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="aff54-261">Du bör få automatiskt inloggade tooyour Onit programmet när du klickar på hello Onit panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="aff54-261">When you click hello Onit tile in hello Access Panel, you should get automatically signed-on tooyour Onit application.</span></span>
<span data-ttu-id="aff54-262">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="aff54-262">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="aff54-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="aff54-263">Additional resources</span></span>

* [<span data-ttu-id="aff54-264">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aff54-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="aff54-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="aff54-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-onit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-onit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-onit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-onit-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-onit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-onit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-onit-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-onit-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-onit-tutorial/tutorial_general_203.png

