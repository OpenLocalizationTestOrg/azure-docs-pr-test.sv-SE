---
title: "Självstudier: Azure Active Directory-integrering med Clever | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Clever."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 24430e1e6c750efa5787561aa151201b1fe7d428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a><span data-ttu-id="1c739-103">Självstudier: Azure Active Directory-integrering med Clever</span><span class="sxs-lookup"><span data-stu-id="1c739-103">Tutorial: Azure Active Directory integration with Clever</span></span>

<span data-ttu-id="1c739-104">I kursen får du lära dig hur toointegrate Clever med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1c739-104">In this tutorial, you learn how toointegrate Clever with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1c739-105">Integrera Clever med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1c739-105">Integrating Clever with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="1c739-106">Du kan styra i Azure AD som har åtkomst till tooClever.</span><span class="sxs-lookup"><span data-stu-id="1c739-106">You can control in Azure AD who has access tooClever.</span></span>
- <span data-ttu-id="1c739-107">Du kan låta dina användare tooautomatically get inloggade tooClever (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="1c739-107">You can enable your users tooautomatically get signed-on tooClever (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="1c739-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1c739-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="1c739-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1c739-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c739-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1c739-110">Prerequisites</span></span>

<span data-ttu-id="1c739-111">tooconfigure Azure AD-integrering med Clever, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1c739-111">tooconfigure Azure AD integration with Clever, you need hello following items:</span></span>

- <span data-ttu-id="1c739-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1c739-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1c739-113">En smarta enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1c739-113">A Clever single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1c739-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1c739-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1c739-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1c739-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1c739-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1c739-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1c739-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1c739-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1c739-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1c739-118">Scenario description</span></span>
<span data-ttu-id="1c739-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1c739-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1c739-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1c739-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1c739-121">Att lägga till Clever från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1c739-121">Adding Clever from hello gallery</span></span>
2. <span data-ttu-id="1c739-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c739-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clever-from-hello-gallery"></a><span data-ttu-id="1c739-123">Att lägga till Clever från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1c739-123">Adding Clever from hello gallery</span></span>
<span data-ttu-id="1c739-124">tooconfigure hello integrering av Clever i Azure AD, behöver du tooadd Clever hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="1c739-124">tooconfigure hello integration of Clever into Azure AD, you need tooadd Clever from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1c739-125">**tooadd Clever från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1c739-125">**tooadd Clever from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c739-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1c739-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="1c739-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1c739-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="1c739-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="1c739-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="1c739-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="1c739-133">Skriv i sökrutan hello **Clever**väljer **Clever** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="1c739-133">In hello search box, type **Clever**, select **Clever** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Smarta i hello resultatlistan](./media/active-directory-saas-clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="1c739-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c739-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="1c739-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Clever baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1c739-136">In this section, you configure and test Azure AD single sign-on with Clever based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1c739-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Clever är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1c739-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Clever is tooa user in Azure AD.</span></span> <span data-ttu-id="1c739-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Clever toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="1c739-138">In other words, a link relationship between an Azure AD user and hello related user in Clever needs toobe established.</span></span>

<span data-ttu-id="1c739-139">I Clever, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1c739-139">In Clever, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="1c739-140">tooconfigure och testa Azure AD enkel inloggning med Clever, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1c739-140">tooconfigure and test Azure AD single sign-on with Clever, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1c739-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1c739-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1c739-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1c739-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1c739-143">**[Skapa en smarta testanvändare](#create-a-clever-test-user)**  -toohave en motsvarighet för Britta Simon i Clever som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1c739-143">**[Create a Clever test user](#create-a-clever-test-user)** - toohave a counterpart of Britta Simon in Clever that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="1c739-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c739-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1c739-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1c739-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="1c739-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c739-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="1c739-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i smarta programmet.</span><span class="sxs-lookup"><span data-stu-id="1c739-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Clever application.</span></span>

<span data-ttu-id="1c739-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Clever:**</span><span class="sxs-lookup"><span data-stu-id="1c739-148">**tooconfigure Azure AD single sign-on with Clever, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c739-149">I hello Azure-portalen på hello **Clever** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1c739-149">In hello Azure portal, on hello **Clever** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="1c739-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c739-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-clever-tutorial/tutorial_clever_samlbase.png)

3. <span data-ttu-id="1c739-153">På hello **smarta domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1c739-153">On hello **Clever Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och smarta domän med enkel inloggning information](./media/active-directory-saas-clever-tutorial/tutorial_clever_url.png)

    <span data-ttu-id="1c739-155">a.</span><span class="sxs-lookup"><span data-stu-id="1c739-155">a.</span></span> <span data-ttu-id="1c739-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://clever.com/in/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="1c739-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://clever.com/in/<companyname>`</span></span>

    <span data-ttu-id="1c739-157">b.</span><span class="sxs-lookup"><span data-stu-id="1c739-157">b.</span></span> <span data-ttu-id="1c739-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://clever.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="1c739-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://clever.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1c739-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1c739-159">These values are not real.</span></span> <span data-ttu-id="1c739-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="1c739-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1c739-161">Kontakta [smarta klienten supportteamet](https://clever.com/about/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1c739-161">Contact [Clever Client support team](https://clever.com/about/contact/) tooget these values.</span></span>

4. <span data-ttu-id="1c739-162">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="1c739-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-clever-tutorial/tutorial_clever_certificate.png)

5. <span data-ttu-id="1c739-164">hello smarta programmet förväntar hello SAML intyg i ett specifikt format, vilket kräver tooadd attributet mappningar tooyour **SAML-Token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1c739-164">hello Clever application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **SAML Token Attributes** configuration.</span></span>

    <span data-ttu-id="1c739-165">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="1c739-165">hello following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_07.png) 

6. <span data-ttu-id="1c739-167">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1c739-167">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="1c739-168">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="1c739-168">Attribute Name</span></span>  | <span data-ttu-id="1c739-169">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="1c739-169">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="1c739-170">clever.student.Credentials.District\_användarnamn</span><span class="sxs-lookup"><span data-stu-id="1c739-170">clever.student.credentials.district\_username</span></span>  | <span data-ttu-id="1c739-171">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="1c739-171">user.userprincipalname</span></span> |
    | <span data-ttu-id="1c739-172">Förnamn</span><span class="sxs-lookup"><span data-stu-id="1c739-172">Firstname</span></span>  | <span data-ttu-id="1c739-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="1c739-173">user.givenname</span></span> |
    | <span data-ttu-id="1c739-174">Efternamn</span><span class="sxs-lookup"><span data-stu-id="1c739-174">Lastname</span></span>  | <span data-ttu-id="1c739-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="1c739-175">user.surname</span></span> |    

    <span data-ttu-id="1c739-176">a.</span><span class="sxs-lookup"><span data-stu-id="1c739-176">a.</span></span> <span data-ttu-id="1c739-177">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="1c739-180">b.</span><span class="sxs-lookup"><span data-stu-id="1c739-180">b.</span></span> <span data-ttu-id="1c739-181">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="1c739-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>

    <span data-ttu-id="1c739-182">c.</span><span class="sxs-lookup"><span data-stu-id="1c739-182">c.</span></span> <span data-ttu-id="1c739-183">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="1c739-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>

    <span data-ttu-id="1c739-184">d.</span><span class="sxs-lookup"><span data-stu-id="1c739-184">d.</span></span> <span data-ttu-id="1c739-185">Lämna hello **Namespace** textruta tomt.</span><span class="sxs-lookup"><span data-stu-id="1c739-185">Leave hello **Namespace** textbox blank.</span></span>
    
    <span data-ttu-id="1c739-186">d.</span><span class="sxs-lookup"><span data-stu-id="1c739-186">d.</span></span> <span data-ttu-id="1c739-187">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c739-187">Click **Ok**.</span></span>     

5. <span data-ttu-id="1c739-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1c739-188">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-clever-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="1c739-190">toogenerate hello **Metadata** url, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1c739-190">toogenerate hello **Metadata** url, perform hello following steps:</span></span>

    <span data-ttu-id="1c739-191">a.</span><span class="sxs-lookup"><span data-stu-id="1c739-191">a.</span></span> <span data-ttu-id="1c739-192">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="1c739-192">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_appregistrations.png)
   
    <span data-ttu-id="1c739-194">b.</span><span class="sxs-lookup"><span data-stu-id="1c739-194">b.</span></span> <span data-ttu-id="1c739-195">Klicka på **slutpunkter** tooopen **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-195">Click **Endpoints** tooopen **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpointicon.png)

    <span data-ttu-id="1c739-197">c.</span><span class="sxs-lookup"><span data-stu-id="1c739-197">c.</span></span> <span data-ttu-id="1c739-198">Klicka på hello Kopiera knappen toocopy **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="1c739-198">Click hello copy button toocopy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_endpoint.png)
     
    <span data-ttu-id="1c739-200">d.</span><span class="sxs-lookup"><span data-stu-id="1c739-200">d.</span></span> <span data-ttu-id="1c739-201">Gå nu toohello egenskapssida **Clever** och kopiera hello **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="1c739-201">Now go toohello property page of **Clever** and copy hello **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-clever-tutorial/tutorial_clever_appid.png)

    <span data-ttu-id="1c739-203">e.</span><span class="sxs-lookup"><span data-stu-id="1c739-203">e.</span></span> <span data-ttu-id="1c739-204">Generera hello **URL för tjänstmetadata** med hello följande mönster:`<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span><span class="sxs-lookup"><span data-stu-id="1c739-204">Generate hello **Metadata URL** using hello following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`</span></span>   

9. <span data-ttu-id="1c739-205">Logga in tooyour smarta företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="1c739-205">In a different web browser window, log in tooyour Clever company site as an administrator.</span></span>

10. <span data-ttu-id="1c739-206">Klicka i verktygsfältet hello **Instant inloggningen**.</span><span class="sxs-lookup"><span data-stu-id="1c739-206">In hello toolbar, click **Instant Login**.</span></span>

    <span data-ttu-id="1c739-207">![Direkt inloggning](./media/active-directory-saas-clever-tutorial/ic798984.png "Instant inloggning")</span><span class="sxs-lookup"><span data-stu-id="1c739-207">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798984.png "Instant Login")</span></span>

11. <span data-ttu-id="1c739-208">På hello **Instant inloggningen** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1c739-208">On hello **Instant Login** page, perform hello following steps:</span></span>
      
      <span data-ttu-id="1c739-209">![Direkt inloggning](./media/active-directory-saas-clever-tutorial/ic798985.png "Instant inloggning")</span><span class="sxs-lookup"><span data-stu-id="1c739-209">![Instant Login](./media/active-directory-saas-clever-tutorial/ic798985.png "Instant Login")</span></span>
      
      <span data-ttu-id="1c739-210">a.</span><span class="sxs-lookup"><span data-stu-id="1c739-210">a.</span></span> <span data-ttu-id="1c739-211">Typen hello **Inloggningswebbadressen**.</span><span class="sxs-lookup"><span data-stu-id="1c739-211">Type hello **Login URL**.</span></span>
      
      >[!NOTE]
      ><span data-ttu-id="1c739-212">Hej **Inloggningswebbadressen** är ett anpassat värde.</span><span class="sxs-lookup"><span data-stu-id="1c739-212">hello **Login URL** is a custom value.</span></span> <span data-ttu-id="1c739-213">Kontakta [smarta klienten supportteamet](https://clever.com/about/contact/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="1c739-213">Contact [Clever Client support team](https://clever.com/about/contact/) tooget this value.</span></span>
      
      <span data-ttu-id="1c739-214">b.</span><span class="sxs-lookup"><span data-stu-id="1c739-214">b.</span></span> <span data-ttu-id="1c739-215">Som **identitetssystem**väljer **ADFS**.</span><span class="sxs-lookup"><span data-stu-id="1c739-215">As **Identity System**, select **ADFS**.</span></span>

      <span data-ttu-id="1c739-216">c.</span><span class="sxs-lookup"><span data-stu-id="1c739-216">c.</span></span> <span data-ttu-id="1c739-217">Typen hello **URL för tjänstmetadata** i hello **URL för tjänstmetadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="1c739-217">Type hello **Metadata URL** in hello **Metadata URL** textbox.</span></span>
      
      <span data-ttu-id="1c739-218">d.</span><span class="sxs-lookup"><span data-stu-id="1c739-218">d.</span></span> <span data-ttu-id="1c739-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1c739-219">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="1c739-220">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="1c739-220">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="1c739-221">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="1c739-221">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="1c739-222">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1c739-222">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="1c739-223">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1c739-223">Create an Azure AD test user</span></span>

<span data-ttu-id="1c739-224">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1c739-224">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="1c739-226">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1c739-226">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c739-227">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="1c739-227">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-clever-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="1c739-229">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1c739-229">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-clever-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="1c739-231">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-231">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-clever-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="1c739-233">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1c739-233">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-clever-tutorial/create_aaduser_04.png)

    <span data-ttu-id="1c739-235">a.</span><span class="sxs-lookup"><span data-stu-id="1c739-235">a.</span></span> <span data-ttu-id="1c739-236">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1c739-236">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1c739-237">b.</span><span class="sxs-lookup"><span data-stu-id="1c739-237">b.</span></span> <span data-ttu-id="1c739-238">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1c739-238">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="1c739-239">c.</span><span class="sxs-lookup"><span data-stu-id="1c739-239">c.</span></span> <span data-ttu-id="1c739-240">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-240">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="1c739-241">d.</span><span class="sxs-lookup"><span data-stu-id="1c739-241">d.</span></span> <span data-ttu-id="1c739-242">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1c739-242">Click **Create**.</span></span>
 
### <a name="create-a-clever-test-user"></a><span data-ttu-id="1c739-243">Skapa en smarta testanvändare</span><span class="sxs-lookup"><span data-stu-id="1c739-243">Create a Clever test user</span></span>

<span data-ttu-id="1c739-244">tooenable Azure AD-användare toolog i tooClever, måste de etableras i Clever.</span><span class="sxs-lookup"><span data-stu-id="1c739-244">tooenable Azure AD users toolog in tooClever, they must be provisioned into Clever.</span></span>

<span data-ttu-id="1c739-245">Arbeta med vid Clever, [smarta klienten supportteamet](https://clever.com/about/contact/) att lägga till hello användare i hello smarta plattform.</span><span class="sxs-lookup"><span data-stu-id="1c739-245">In case of Clever, Work with [Clever Client support team](https://clever.com/about/contact/) to add hello users in hello Clever platform.</span></span> <span data-ttu-id="1c739-246">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1c739-246">Users must be created and activated before you use single sign-on.</span></span> 

>[!NOTE]
><span data-ttu-id="1c739-247">Du kan använda något annat smarta användarens konto skapas verktyg eller API: er som tillhandahålls av smarta tooprovision användarkonton i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1c739-247">You can use any other Clever user account creation tools or APIs provided by Clever tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="1c739-248">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="1c739-248">Assign hello Azure AD test user</span></span>

<span data-ttu-id="1c739-249">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooClever.</span><span class="sxs-lookup"><span data-stu-id="1c739-249">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooClever.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="1c739-251">**tooassign Britta Simon tooClever utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1c739-251">**tooassign Britta Simon tooClever, perform hello following steps:**</span></span>

1. <span data-ttu-id="1c739-252">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1c739-252">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1c739-254">Välj i listan med program hello **Clever**.</span><span class="sxs-lookup"><span data-stu-id="1c739-254">In hello applications list, select **Clever**.</span></span>

    ![Hej Clever länken i listan med program hello](./media/active-directory-saas-clever-tutorial/tutorial_clever_app.png)  

3. <span data-ttu-id="1c739-256">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1c739-256">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="1c739-258">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1c739-258">Click **Add** button.</span></span> <span data-ttu-id="1c739-259">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="1c739-261">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="1c739-261">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="1c739-262">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1c739-263">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1c739-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="1c739-264">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1c739-264">Test single sign-on</span></span>

<span data-ttu-id="1c739-265">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1c739-265">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="1c739-266">När du klickar på hello smarta panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour smarta programmet.</span><span class="sxs-lookup"><span data-stu-id="1c739-266">When you click hello Clever tile in hello Access Panel, you should get automatically signed-on tooyour Clever application.</span></span>
<span data-ttu-id="1c739-267">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1c739-267">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1c739-268">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1c739-268">Additional resources</span></span>

* [<span data-ttu-id="1c739-269">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1c739-269">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1c739-270">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1c739-270">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clever-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clever-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clever-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clever-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clever-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clever-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clever-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clever-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clever-tutorial/tutorial_general_203.png

