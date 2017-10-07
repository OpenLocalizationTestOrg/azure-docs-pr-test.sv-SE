---
title: "Självstudier: Azure Active Directory-integrering med MOVEit överföring - integrering med Azure AD | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och MOVEit överföringen - integrering med Azure AD."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 5bbe4f2d952bd45c4d58d55ffc3467b4eb871fd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a><span data-ttu-id="9c920-103">Självstudier: Azure Active Directory-integrering med MOVEit överföring - Azure AD-integrering</span><span class="sxs-lookup"><span data-stu-id="9c920-103">Tutorial: Azure Active Directory integration with MOVEit Transfer - Azure AD integration</span></span>

<span data-ttu-id="9c920-104">I kursen får du lära dig hur toointegrate MOVEit överföring - Azure AD-integrering med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9c920-104">In this tutorial, you learn how toointegrate MOVEit Transfer - Azure AD integration with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c920-105">Integrera MOVEit överföring - Azure AD-integrering med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9c920-105">Integrating MOVEit Transfer - Azure AD integration with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="9c920-106">Du kan styra i Azure AD som har åtkomst tooMOVEit överföring - integrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c920-106">You can control in Azure AD who has access tooMOVEit Transfer - Azure AD integration.</span></span>
- <span data-ttu-id="9c920-107">Du kan låta dina användare tooautomatically get inloggade tooMOVEit överföring - Azure AD-integrering (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="9c920-107">You can enable your users tooautomatically get signed-on tooMOVEit Transfer - Azure AD integration (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9c920-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9c920-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="9c920-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c920-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c920-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9c920-110">Prerequisites</span></span>

<span data-ttu-id="9c920-111">tooconfigure Azure AD-integrering med MOVEit överföring - Azure AD-integrering måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="9c920-111">tooconfigure Azure AD integration with MOVEit Transfer - Azure AD integration, you need hello following items:</span></span>

- <span data-ttu-id="9c920-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9c920-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c920-113">En MOVEit överföring - Azure AD-integrering enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="9c920-113">A MOVEit Transfer - Azure AD integration single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c920-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9c920-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c920-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9c920-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c920-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9c920-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c920-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c920-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c920-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9c920-118">Scenario description</span></span>
<span data-ttu-id="9c920-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9c920-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c920-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9c920-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c920-121">Lägga till MOVEit överföring - integrering med Azure AD från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9c920-121">Adding MOVEit Transfer - Azure AD integration from hello gallery</span></span>
2. <span data-ttu-id="9c920-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c920-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moveit-transfer---azure-ad-integration-from-hello-gallery"></a><span data-ttu-id="9c920-123">Lägga till MOVEit överföring - integrering med Azure AD från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="9c920-123">Adding MOVEit Transfer - Azure AD integration from hello gallery</span></span>
<span data-ttu-id="9c920-124">tooconfigure hello integrering av MOVEit överföring - Azure AD-integrering i Azure AD, behöver du tooadd MOVEit överföring - Azure AD-integrering hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="9c920-124">tooconfigure hello integration of MOVEit Transfer - Azure AD integration into Azure AD, you need tooadd MOVEit Transfer - Azure AD integration from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="9c920-125">**tooadd MOVEit överföring - integrering med Azure AD från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9c920-125">**tooadd MOVEit Transfer - Azure AD integration from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c920-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9c920-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="9c920-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9c920-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="9c920-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="9c920-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="9c920-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c920-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="9c920-133">Skriv i sökrutan hello **MOVEit överföring - integrering med Azure AD**väljer **MOVEit överföring - integrering med Azure AD** resultatet-panelen klickar **Lägg till** knappen tooadd hello programmet.</span><span class="sxs-lookup"><span data-stu-id="9c920-133">In hello search box, type **MOVEit Transfer - Azure AD integration**, select **MOVEit Transfer - Azure AD integration** from result panel then click **Add** button tooadd hello application.</span></span>

    ![MOVEit överföring - Azure AD-integrering i hello resultatlistan](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9c920-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c920-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="9c920-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med MOVEit överföring - integrering med Azure AD utifrån en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9c920-136">In this section, you configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9c920-137">Azure AD måste tooknow vilken hello motsvarighet användare i MOVEit överföring för enkel inloggning toowork - Azure AD-integrering är tooa användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c920-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in MOVEit Transfer - Azure AD integration is tooa user in Azure AD.</span></span> <span data-ttu-id="9c920-138">Med andra ord en länk relationen mellan en Azure AD-användare och hello relaterade användare i MOVEit överföring - Azure AD-integrering måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="9c920-138">In other words, a link relationship between an Azure AD user and hello related user in MOVEit Transfer - Azure AD integration needs toobe established.</span></span>

<span data-ttu-id="9c920-139">Tilldela hello värdet för hello i MOVEit överföring - Azure AD-integrering **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9c920-139">In MOVEit Transfer - Azure AD integration, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="9c920-140">tooconfigure och testa Azure AD enkel inloggning med MOVEit överföring - Azure AD-integrering måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9c920-140">tooconfigure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="9c920-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9c920-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="9c920-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c920-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c920-143">**[Skapa en MOVEit överföring - Azure AD-integrering testanvändare](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - toohave en motsvarighet för Britta Simon i MOVEit överföring - Azure AD-integrering som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9c920-143">**[Create a MOVEit Transfer - Azure AD integration test user](#create-a-moveit-transfer---azure-ad-integration-test-user)** - toohave a counterpart of Britta Simon in MOVEit Transfer - Azure AD integration that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c920-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9c920-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c920-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9c920-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9c920-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c920-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9c920-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din MOVEit överföring - program för Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="9c920-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your MOVEit Transfer - Azure AD integration application.</span></span>

<span data-ttu-id="9c920-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med MOVEit överföring - Azure AD-integrering:**</span><span class="sxs-lookup"><span data-stu-id="9c920-148">**tooconfigure Azure AD single sign-on with MOVEit Transfer - Azure AD integration, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c920-149">I hello Azure-portalen på hello **MOVEit överföring - integrering med Azure AD** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9c920-149">In hello Azure portal, on hello **MOVEit Transfer - Azure AD integration** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="9c920-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9c920-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. <span data-ttu-id="9c920-153">På hello **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c920-153">On hello **MOVEit Transfer - Azure AD integration Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    <span data-ttu-id="9c920-155">a.</span><span class="sxs-lookup"><span data-stu-id="9c920-155">a.</span></span> <span data-ttu-id="9c920-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://contoso.com`</span><span class="sxs-lookup"><span data-stu-id="9c920-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://contoso.com`</span></span>

    <span data-ttu-id="9c920-157">b.</span><span class="sxs-lookup"><span data-stu-id="9c920-157">b.</span></span> <span data-ttu-id="9c920-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://contoso.com/<tenatid>`</span><span class="sxs-lookup"><span data-stu-id="9c920-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://contoso.com/<tenatid>`</span></span>

    <span data-ttu-id="9c920-159">c.</span><span class="sxs-lookup"><span data-stu-id="9c920-159">c.</span></span> <span data-ttu-id="9c920-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span><span class="sxs-lookup"><span data-stu-id="9c920-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="9c920-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9c920-161">These values are not real.</span></span> <span data-ttu-id="9c920-162">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="9c920-162">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="9c920-163">Du kan se dessa värden senare i **URL för tjänstmetadata providern** avsnitt eller kontakta [MOVEit överföring - supportteamet för Azure AD-integrering klienten](https://community.ipswitch.com/s/support) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9c920-163">You can refer these values later in **Service Provider Metadata URL** section or contact [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support) tooget these values.</span></span>

4. <span data-ttu-id="9c920-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="9c920-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. <span data-ttu-id="9c920-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9c920-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="9c920-168">Logga in tooyour MOVEit överföring klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="9c920-168">Sign on tooyour MOVEit Transfer tenant as an administrator.</span></span>

7. <span data-ttu-id="9c920-169">Klicka på hello vänstra navigeringsfönstret **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="9c920-169">On hello left navigation pane, click **Settings**.</span></span>

    ![Inställningar för avsnittet på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. <span data-ttu-id="9c920-171">Klicka på **enkel inloggning** länk som är under **principer -> användaren Auth**.</span><span class="sxs-lookup"><span data-stu-id="9c920-171">Click **Single Signon** link, which is under **Security Policies -> User Auth**.</span></span>

    ![Säkerhet principer på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. <span data-ttu-id="9c920-173">Klicka på hello URL för tjänstmetadata länken toodownload hello metadata dokument.</span><span class="sxs-lookup"><span data-stu-id="9c920-173">Click hello Metadata URL link toodownload hello metadata document.</span></span>

    ![URL för tjänstmetadata Provider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * <span data-ttu-id="9c920-175">Kontrollera **ID för entiteterna** matchar **identifierare** i hello **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9c920-175">Verify **entityID** matches **Identifier** in hello **MOVEit Transfer - Azure AD integration Domain and URLs** section .</span></span>
    * <span data-ttu-id="9c920-176">Kontrollera **AssertionConsumerService** -URL: en matchar **REPLY URL** i hello **MOVEit överföring - URL: er och integrering med Azure AD Domain** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9c920-176">Verify **AssertionConsumerService** Location URL matches **REPLY URL** in hello **MOVEit Transfer - Azure AD integration Domain and URLs** section.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. <span data-ttu-id="9c920-178">Klicka på **lägga till identitetsleverantör** knappen tooadd en ny Provider för federerad identitet.</span><span class="sxs-lookup"><span data-stu-id="9c920-178">Click **Add Identity Provider** button tooadd a new Federated Identity Provider.</span></span>

    ![Lägg till identitetsleverantören.](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. <span data-ttu-id="9c920-180">Klicka på **Bläddra...**  tooselect hello metadatafil som du laddade ned från Azure-portalen och klicka sedan på **lägga till identitetsleverantör** tooupload hello ned filen.</span><span class="sxs-lookup"><span data-stu-id="9c920-180">Click **Browse...** tooselect hello metadata file which you downloaded from Azure portal, then click **Add Identity Provider** tooupload hello downloaded file.</span></span>

    ![SAML-identitetsprovider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. <span data-ttu-id="9c920-182">Välj ”**Ja**” som **aktiverad** i hello **redigera federerad identitet providerinställningar...**  och klickar på **spara**.</span><span class="sxs-lookup"><span data-stu-id="9c920-182">Select "**Yes**" as **Enabled** in hello **Edit Federated Identity Provider Settings...** page and click **Save**.</span></span>

    ![Federerade identiteter providerinställningar](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. <span data-ttu-id="9c920-184">I hello **redigera federerad identitet providern användarinställningar** utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="9c920-184">In hello **Edit Federated Identity Provider User Settings** page, perform hello following actions:</span></span>
    
    ![Redigera inställningar för federerad identitet-Provider](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    <span data-ttu-id="9c920-186">a.</span><span class="sxs-lookup"><span data-stu-id="9c920-186">a.</span></span> <span data-ttu-id="9c920-187">Välj **SAML NameID** som **inloggningsnamnet**.</span><span class="sxs-lookup"><span data-stu-id="9c920-187">Select **SAML NameID** as **Login name**.</span></span>
    
    <span data-ttu-id="9c920-188">b.</span><span class="sxs-lookup"><span data-stu-id="9c920-188">b.</span></span> <span data-ttu-id="9c920-189">Välj **andra** som **fullständiga namn** och i hello **attributnamn** textruta placera hello värde: `http://schemas.microsoft.com/identity/claims/displayname`.</span><span class="sxs-lookup"><span data-stu-id="9c920-189">Select **Other** as **Full name** and in hello **Attribute name** textbox put hello value: `http://schemas.microsoft.com/identity/claims/displayname`.</span></span>
    
    <span data-ttu-id="9c920-190">c.</span><span class="sxs-lookup"><span data-stu-id="9c920-190">c.</span></span> <span data-ttu-id="9c920-191">Välj **andra** som **e-post** och i hello **attributnamn** textruta placera hello värde: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="9c920-191">Select **Other** as **Email** and in hello **Attribute name** textbox put hello value: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="9c920-192">d.</span><span class="sxs-lookup"><span data-stu-id="9c920-192">d.</span></span> <span data-ttu-id="9c920-193">Välj **Ja** som **skapa automatiskt konto på inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9c920-193">Select **Yes** as **Auto-create account on signon**.</span></span>
    
    <span data-ttu-id="9c920-194">e.</span><span class="sxs-lookup"><span data-stu-id="9c920-194">e.</span></span> <span data-ttu-id="9c920-195">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9c920-195">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="9c920-196">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="9c920-196">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="9c920-197">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="9c920-197">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="9c920-198">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c920-198">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9c920-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c920-199">Create an Azure AD test user</span></span>

<span data-ttu-id="9c920-200">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c920-200">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="9c920-202">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9c920-202">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c920-203">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="9c920-203">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9c920-205">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9c920-205">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9c920-207">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c920-207">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9c920-209">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c920-209">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9c920-211">a.</span><span class="sxs-lookup"><span data-stu-id="9c920-211">a.</span></span> <span data-ttu-id="9c920-212">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c920-212">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c920-213">b.</span><span class="sxs-lookup"><span data-stu-id="9c920-213">b.</span></span> <span data-ttu-id="9c920-214">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c920-214">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="9c920-215">c.</span><span class="sxs-lookup"><span data-stu-id="9c920-215">c.</span></span> <span data-ttu-id="9c920-216">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="9c920-216">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="9c920-217">d.</span><span class="sxs-lookup"><span data-stu-id="9c920-217">d.</span></span> <span data-ttu-id="9c920-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9c920-218">Click **Create**.</span></span>
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a><span data-ttu-id="9c920-219">Skapa en MOVEit överföring - testanvändare i Azure AD-integrering</span><span class="sxs-lookup"><span data-stu-id="9c920-219">Create a MOVEit Transfer - Azure AD integration test user</span></span>

<span data-ttu-id="9c920-220">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i MOVEit överföring - integrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c920-220">hello objective of this section is toocreate a user called Britta Simon in MOVEit Transfer - Azure AD integration.</span></span> <span data-ttu-id="9c920-221">MOVEit överföring - integrering med Azure AD stöder just-in-time-allokering som du har aktiverat.</span><span class="sxs-lookup"><span data-stu-id="9c920-221">MOVEit Transfer - Azure AD integration supports just-in-time provisioning, which you have enabled.</span></span> <span data-ttu-id="9c920-222">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9c920-222">There is no action item for you in this section.</span></span> <span data-ttu-id="9c920-223">En ny användare skapas under ett försök tooaccess MOVEit överföring - Azure AD-integrering om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="9c920-223">A new user is created during an attempt tooaccess MOVEit Transfer - Azure AD integration if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="9c920-224">Om du behöver toocreate en användare manuellt, måste toocontact hello [MOVEit överföring - supportteamet för Azure AD-integrering klienten](https://community.ipswitch.com/s/support).</span><span class="sxs-lookup"><span data-stu-id="9c920-224">If you need toocreate a user manually, you need toocontact hello [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="9c920-225">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c920-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="9c920-226">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMOVEit överföring - integrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c920-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMOVEit Transfer - Azure AD integration.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="9c920-228">**tooassign Britta Simon tooMOVEit överföring - Azure AD-integrering utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9c920-228">**tooassign Britta Simon tooMOVEit Transfer - Azure AD integration, perform hello following steps:**</span></span>

1. <span data-ttu-id="9c920-229">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9c920-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9c920-231">Välj i listan med program hello **MOVEit överföring - integrering med Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="9c920-231">In hello applications list, select **MOVEit Transfer - Azure AD integration**.</span></span>

    ![Hej MOVEit överföring - Azure AD-integrering länken i listan med program hello](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. <span data-ttu-id="9c920-233">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9c920-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="9c920-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9c920-235">Click **Add** button.</span></span> <span data-ttu-id="9c920-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c920-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="9c920-238">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="9c920-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="9c920-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c920-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c920-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c920-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9c920-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c920-241">Test single sign-on</span></span>

<span data-ttu-id="9c920-242">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="9c920-242">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="9c920-243">När du klickar på hello MOVEit överföring - panelen i Azure AD-integrering hello åtkomstpanelen får automatiskt inloggade tooyour MOVEit överföring - program för Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="9c920-243">When you click hello MOVEit Transfer - Azure AD integration tile in hello Access Panel, you should get automatically signed-on tooyour MOVEit Transfer - Azure AD integration application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9c920-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9c920-244">Additional resources</span></span>

* [<span data-ttu-id="9c920-245">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c920-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c920-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9c920-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

