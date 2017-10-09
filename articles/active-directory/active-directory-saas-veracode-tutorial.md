---
title: "Självstudier: Azure Active Directory-integrering med Veracode | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Veracode."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d17307b3864b7df8ee55f569d8f962e2e315b936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a><span data-ttu-id="829f8-103">Självstudier: Azure Active Directory-integrering med Veracode</span><span class="sxs-lookup"><span data-stu-id="829f8-103">Tutorial: Azure Active Directory integration with Veracode</span></span>

<span data-ttu-id="829f8-104">I kursen får du lära dig hur toointegrate Veracode med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="829f8-104">In this tutorial, you learn how toointegrate Veracode with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="829f8-105">Integrera Veracode med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="829f8-105">Integrating Veracode with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="829f8-106">Du kan styra i Azure AD som har åtkomst till tooVeracode.</span><span class="sxs-lookup"><span data-stu-id="829f8-106">You can control in Azure AD who has access tooVeracode.</span></span>
- <span data-ttu-id="829f8-107">Du kan låta dina användare tooautomatically get inloggade tooVeracode (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="829f8-107">You can enable your users tooautomatically get signed-on tooVeracode (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="829f8-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="829f8-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="829f8-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="829f8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="829f8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="829f8-110">Prerequisites</span></span>

<span data-ttu-id="829f8-111">tooconfigure Azure AD-integrering med Veracode, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="829f8-111">tooconfigure Azure AD integration with Veracode, you need hello following items:</span></span>

- <span data-ttu-id="829f8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="829f8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="829f8-113">En Veracode enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="829f8-113">A Veracode single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="829f8-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="829f8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="829f8-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="829f8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="829f8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="829f8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="829f8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="829f8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="829f8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="829f8-118">Scenario description</span></span>
<span data-ttu-id="829f8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="829f8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="829f8-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="829f8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="829f8-121">Lägg till Veracode från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="829f8-121">Add Veracode from hello gallery</span></span>
2. <span data-ttu-id="829f8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="829f8-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-veracode-from-hello-gallery"></a><span data-ttu-id="829f8-123">Lägg till Veracode från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="829f8-123">Add Veracode from hello gallery</span></span>
<span data-ttu-id="829f8-124">tooconfigure hello integrering av Veracode i Azure AD, behöver du tooadd Veracode hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="829f8-124">tooconfigure hello integration of Veracode into Azure AD, you need tooadd Veracode from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="829f8-125">**tooadd Veracode från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="829f8-125">**tooadd Veracode from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="829f8-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="829f8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="829f8-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="829f8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="829f8-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="829f8-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="829f8-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="829f8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="829f8-133">Skriv i sökrutan hello **Veracode**väljer **Veracode** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="829f8-133">In hello search box, type **Veracode**, select  **Veracode** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Veracode i hello resultatlistan](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="829f8-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="829f8-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="829f8-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Veracode baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="829f8-136">In this section, you configure and test Azure AD single sign-on with Veracode based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="829f8-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Veracode är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="829f8-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Veracode is tooa user in Azure AD.</span></span> <span data-ttu-id="829f8-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Veracode toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="829f8-138">In other words, a link relationship between an Azure AD user and hello related user in Veracode needs toobe established.</span></span>

<span data-ttu-id="829f8-139">I Veracode, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="829f8-139">In Veracode, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="829f8-140">tooconfigure och testa Azure AD enkel inloggning med Veracode, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="829f8-140">tooconfigure and test Azure AD single sign-on with Veracode, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="829f8-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="829f8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="829f8-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="829f8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="829f8-143">**[Skapa en testanvändare Veracode](#create-a-veracode-test-user)**  -toohave en motsvarighet för Britta Simon i Veracode som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="829f8-143">**[Create a Veracode test user](#create-a-veracode-test-user)** - toohave a counterpart of Britta Simon in Veracode that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="829f8-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="829f8-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="829f8-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="829f8-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="829f8-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="829f8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="829f8-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Veracode program.</span><span class="sxs-lookup"><span data-stu-id="829f8-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Veracode application.</span></span>

<span data-ttu-id="829f8-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Veracode:**</span><span class="sxs-lookup"><span data-stu-id="829f8-148">**tooconfigure Azure AD single sign-on with Veracode, perform hello following steps:**</span></span>

1. <span data-ttu-id="829f8-149">I hello Azure-portalen på hello **Veracode** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="829f8-149">In hello Azure portal, on hello **Veracode** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="829f8-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="829f8-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. <span data-ttu-id="829f8-153">På hello **Veracode domän och URL: er** avsnittet användaren har inte tooperform alla steg som hello app redan redan är integrerade med Azure.</span><span class="sxs-lookup"><span data-stu-id="829f8-153">On hello **Veracode Domain and URLs** section, the user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. <span data-ttu-id="829f8-155">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="829f8-155">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. <span data-ttu-id="829f8-157">hello syftet med det här avsnittet är toooutline hur tooenable användare tooauthenticate tooVeracode med sitt konto i Azure AD med hjälp av federation baserat på hello SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="829f8-157">hello objective of this section is toooutline how tooenable users tooauthenticate tooVeracode with their account in Azure AD using federation based on hello SAML protocol.</span></span>

    <span data-ttu-id="829f8-158">Tillämpningsprogrammet Veracode förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **saml-token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="829f8-158">Your Veracode application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **saml token attributes** configuration.</span></span> <span data-ttu-id="829f8-159">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="829f8-159">hello following screenshot shows an example for this.</span></span>
    
    <span data-ttu-id="829f8-160">![Attribut](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="829f8-160">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "Attributes")</span></span>

6. <span data-ttu-id="829f8-161">mappningar av tooadd hello krävs, utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="829f8-161">tooadd hello required attribute mappings, perform hello following steps:</span></span>

    | <span data-ttu-id="829f8-162">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="829f8-162">Attribute Name</span></span> | <span data-ttu-id="829f8-163">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="829f8-163">Attribute Value</span></span> |
    |--- |--- |
    | <span data-ttu-id="829f8-164">Förnamn</span><span class="sxs-lookup"><span data-stu-id="829f8-164">firstname</span></span> |<span data-ttu-id="829f8-165">User.givenName</span><span class="sxs-lookup"><span data-stu-id="829f8-165">User.givenname</span></span> |
    | <span data-ttu-id="829f8-166">Efternamn</span><span class="sxs-lookup"><span data-stu-id="829f8-166">lastname</span></span> |<span data-ttu-id="829f8-167">User.surname</span><span class="sxs-lookup"><span data-stu-id="829f8-167">User.surname</span></span> |
    | <span data-ttu-id="829f8-168">E-post</span><span class="sxs-lookup"><span data-stu-id="829f8-168">email</span></span> |<span data-ttu-id="829f8-169">User.Mail</span><span class="sxs-lookup"><span data-stu-id="829f8-169">User.mail</span></span> |
    
    <span data-ttu-id="829f8-170">a.</span><span class="sxs-lookup"><span data-stu-id="829f8-170">a.</span></span> <span data-ttu-id="829f8-171">För varje datarad i hello tabellen ovan klickar du på **lägga till användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="829f8-171">For each data row in hello table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="829f8-172">![Attribut](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="829f8-172">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "Attributes")</span></span>
    
    <span data-ttu-id="829f8-173">![Attribut](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="829f8-173">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "Attributes")</span></span>
    
    <span data-ttu-id="829f8-174">b.</span><span class="sxs-lookup"><span data-stu-id="829f8-174">b.</span></span> <span data-ttu-id="829f8-175">I hello **attributnamn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="829f8-175">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="829f8-176">c.</span><span class="sxs-lookup"><span data-stu-id="829f8-176">c.</span></span> <span data-ttu-id="829f8-177">I hello **attributvärdet** textruta väljer hello-attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="829f8-177">In hello **Attribute Value** textbox, select hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="829f8-178">d.</span><span class="sxs-lookup"><span data-stu-id="829f8-178">d.</span></span> <span data-ttu-id="829f8-179">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="829f8-179">Click **Ok**.</span></span>

7. <span data-ttu-id="829f8-180">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="829f8-180">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="829f8-182">På hello **Veracode Configuration** klickar du på **konfigurera Veracode** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="829f8-182">On hello **Veracode Configuration** section, click **Configure Veracode** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="829f8-183">Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="829f8-183">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Veracode konfiguration](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. <span data-ttu-id="829f8-185">Logga in på webbplatsen Veracode företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="829f8-185">In a different web browser window, log into your Veracode company site as an administrator.</span></span>

10. <span data-ttu-id="829f8-186">Hello-menyn överst hello **inställningar**, och klicka sedan på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="829f8-186">In hello menu on hello top, click **Settings**, and then click **Admin**.</span></span>
   
    <span data-ttu-id="829f8-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="829f8-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span></span>

11. <span data-ttu-id="829f8-188">Klicka på hello **SAML** fliken.</span><span class="sxs-lookup"><span data-stu-id="829f8-188">Click hello **SAML** tab.</span></span>

12. <span data-ttu-id="829f8-189">I hello **SAML organisationsinställningar** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="829f8-189">In hello **Organization SAML Settings** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="829f8-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="829f8-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span></span>
   
    <span data-ttu-id="829f8-191">a.</span><span class="sxs-lookup"><span data-stu-id="829f8-191">a.</span></span>  <span data-ttu-id="829f8-192">I **utfärdaren** textruta klistra in hello värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="829f8-192">In  **Issuer** textbox, paste hello value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="829f8-193">b.</span><span class="sxs-lookup"><span data-stu-id="829f8-193">b.</span></span> <span data-ttu-id="829f8-194">tooupload hämtade certifikatet från Azure-portalen klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="829f8-194">tooupload your downloaded certificate from Azure portal, click **Choose File**.</span></span>
   
    <span data-ttu-id="829f8-195">c.</span><span class="sxs-lookup"><span data-stu-id="829f8-195">c.</span></span> <span data-ttu-id="829f8-196">Välj **aktivera självregistrering**.</span><span class="sxs-lookup"><span data-stu-id="829f8-196">Select **Enable Self Registration**.</span></span>

13. <span data-ttu-id="829f8-197">I hello **Self Registreringsinställningar** avsnittet, utföra hello följande och klickar sedan på **spara**:</span><span class="sxs-lookup"><span data-stu-id="829f8-197">In hello **Self Registration Settings** section, perform hello following steps, and then click **Save**:</span></span>
   
    <span data-ttu-id="829f8-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="829f8-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span></span>
   
    <span data-ttu-id="829f8-199">a.</span><span class="sxs-lookup"><span data-stu-id="829f8-199">a.</span></span> <span data-ttu-id="829f8-200">Som **aktivera nya användare**väljer **Nej-aktivering krävs**.</span><span class="sxs-lookup"><span data-stu-id="829f8-200">As **New User Activation**, select **No Activation Required**.</span></span>
   
    <span data-ttu-id="829f8-201">b.</span><span class="sxs-lookup"><span data-stu-id="829f8-201">b.</span></span> <span data-ttu-id="829f8-202">Som **Data uppdateras**väljer **inställningar Veracode användardata**.</span><span class="sxs-lookup"><span data-stu-id="829f8-202">As **User Data Updates**, select **Preference Veracode User Data**.</span></span>
   
    <span data-ttu-id="829f8-203">c.</span><span class="sxs-lookup"><span data-stu-id="829f8-203">c.</span></span> <span data-ttu-id="829f8-204">För **SAML attributinformation**, Välj hello följande:</span><span class="sxs-lookup"><span data-stu-id="829f8-204">For **SAML Attribute Details**, select hello following:</span></span>
      * <span data-ttu-id="829f8-205">**Användarroller**</span><span class="sxs-lookup"><span data-stu-id="829f8-205">**User Roles**</span></span>
      * <span data-ttu-id="829f8-206">**Princip för administratör**</span><span class="sxs-lookup"><span data-stu-id="829f8-206">**Policy Administrator**</span></span>
      * <span data-ttu-id="829f8-207">**Granskare**</span><span class="sxs-lookup"><span data-stu-id="829f8-207">**Reviewer**</span></span>
      * <span data-ttu-id="829f8-208">**Säkerhet Lead**</span><span class="sxs-lookup"><span data-stu-id="829f8-208">**Security Lead**</span></span>
      * <span data-ttu-id="829f8-209">**Verkställande**</span><span class="sxs-lookup"><span data-stu-id="829f8-209">**Executive**</span></span>
      * <span data-ttu-id="829f8-210">**Avsändaren**</span><span class="sxs-lookup"><span data-stu-id="829f8-210">**Submitter**</span></span>
      * <span data-ttu-id="829f8-211">**Skapare**</span><span class="sxs-lookup"><span data-stu-id="829f8-211">**Creator**</span></span>
      * <span data-ttu-id="829f8-212">**Alla typer av genomgångar**</span><span class="sxs-lookup"><span data-stu-id="829f8-212">**All Scan Types**</span></span>
      * <span data-ttu-id="829f8-213">**Medlemskap i gruppen**</span><span class="sxs-lookup"><span data-stu-id="829f8-213">**Team Memberships**</span></span>
      * <span data-ttu-id="829f8-214">**Standard-teamet**</span><span class="sxs-lookup"><span data-stu-id="829f8-214">**Default Team**</span></span>

> [!TIP]
> <span data-ttu-id="829f8-215">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="829f8-215">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="829f8-216">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="829f8-216">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="829f8-217">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="829f8-217">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="829f8-218">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="829f8-218">Create an Azure AD test user</span></span>

<span data-ttu-id="829f8-219">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="829f8-219">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="829f8-221">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="829f8-221">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="829f8-222">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="829f8-222">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="829f8-224">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="829f8-224">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="829f8-226">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="829f8-226">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="829f8-228">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="829f8-228">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    <span data-ttu-id="829f8-230">a.</span><span class="sxs-lookup"><span data-stu-id="829f8-230">a.</span></span> <span data-ttu-id="829f8-231">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="829f8-231">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="829f8-232">b.</span><span class="sxs-lookup"><span data-stu-id="829f8-232">b.</span></span> <span data-ttu-id="829f8-233">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="829f8-233">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="829f8-234">c.</span><span class="sxs-lookup"><span data-stu-id="829f8-234">c.</span></span> <span data-ttu-id="829f8-235">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="829f8-235">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="829f8-236">d.</span><span class="sxs-lookup"><span data-stu-id="829f8-236">d.</span></span> <span data-ttu-id="829f8-237">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="829f8-237">Click **Create**.</span></span>
 
### <a name="create-a-veracode-test-user"></a><span data-ttu-id="829f8-238">Skapa en testanvändare Veracode</span><span class="sxs-lookup"><span data-stu-id="829f8-238">Create a Veracode test user</span></span>
<span data-ttu-id="829f8-239">I ordning tooenable Azure AD-användare toolog i Veracode, måste de etableras i Veracode.</span><span class="sxs-lookup"><span data-stu-id="829f8-239">In order tooenable Azure AD users toolog into Veracode, they must be provisioned into Veracode.</span></span> <span data-ttu-id="829f8-240">Hello gäller Veracode är etablering en automatisk uppgift.</span><span class="sxs-lookup"><span data-stu-id="829f8-240">In hello case of Veracode, provisioning is an automated task.</span></span> <span data-ttu-id="829f8-241">Det finns ingen åtgärd-objekt.</span><span class="sxs-lookup"><span data-stu-id="829f8-241">There is no action item for you.</span></span> <span data-ttu-id="829f8-242">Användare skapas automatiskt om det behövs under hello första enkel inloggning försöket.</span><span class="sxs-lookup"><span data-stu-id="829f8-242">Users are automatically created if necessary during hello first single sign-on attempt.</span></span>

> [!NOTE]
> <span data-ttu-id="829f8-243">Du kan använda något annat Veracode användarens konto skapas verktyg eller API: er som tillhandahålls av Veracode tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="829f8-243">You can use any other Veracode user account creation tools or APIs provided by Veracode tooprovision Azure AD user accounts.</span></span>
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="829f8-244">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="829f8-244">Assign hello Azure AD test user</span></span>

<span data-ttu-id="829f8-245">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooVeracode.</span><span class="sxs-lookup"><span data-stu-id="829f8-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVeracode.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="829f8-247">**tooassign Britta Simon tooVeracode utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="829f8-247">**tooassign Britta Simon tooVeracode, perform hello following steps:**</span></span>

1. <span data-ttu-id="829f8-248">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="829f8-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="829f8-250">Välj i listan med program hello **Veracode**.</span><span class="sxs-lookup"><span data-stu-id="829f8-250">In hello applications list, select **Veracode**.</span></span>

    ![Hej Veracode länken i listan med program hello](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. <span data-ttu-id="829f8-252">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="829f8-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="829f8-254">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="829f8-254">Click **Add** button.</span></span> <span data-ttu-id="829f8-255">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="829f8-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="829f8-257">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="829f8-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="829f8-258">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="829f8-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="829f8-259">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="829f8-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="829f8-260">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="829f8-260">Test single sign-on</span></span>

<span data-ttu-id="829f8-261">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="829f8-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="829f8-262">Du bör få automatiskt inloggade tooyour Veracode programmet när du klickar på hello Veracode panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="829f8-262">When you click hello Veracode tile in hello Access Panel, you should get automatically signed-on tooyour Veracode application.</span></span>
<span data-ttu-id="829f8-263">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="829f8-263">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="829f8-264">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="829f8-264">Additional resources</span></span>

* [<span data-ttu-id="829f8-265">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="829f8-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="829f8-266">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="829f8-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

