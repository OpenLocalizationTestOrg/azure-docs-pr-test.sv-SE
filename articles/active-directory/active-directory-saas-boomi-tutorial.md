---
title: "Självstudier: Azure Active Directory-integrering med Boomi | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Boomi."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: ce64a4561697d311a8c7b1b244315bb552c5cfb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a><span data-ttu-id="62a15-103">Självstudier: Azure Active Directory-integrering med Boomi</span><span class="sxs-lookup"><span data-stu-id="62a15-103">Tutorial: Azure Active Directory integration with Boomi</span></span>

<span data-ttu-id="62a15-104">I kursen får du lära dig hur toointegrate Boomi med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="62a15-104">In this tutorial, you learn how toointegrate Boomi with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="62a15-105">Integrera Boomi med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="62a15-105">Integrating Boomi with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="62a15-106">Du kan styra i Azure AD som har åtkomst till tooBoomi</span><span class="sxs-lookup"><span data-stu-id="62a15-106">You can control in Azure AD who has access tooBoomi</span></span>
- <span data-ttu-id="62a15-107">Du kan aktivera din användare tooautomatically get inloggade tooBoomi (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="62a15-107">You can enable your users tooautomatically get signed-on tooBoomi (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="62a15-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="62a15-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="62a15-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="62a15-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62a15-110">Krav</span><span class="sxs-lookup"><span data-stu-id="62a15-110">Prerequisites</span></span>

<span data-ttu-id="62a15-111">tooconfigure Azure AD-integrering med Boomi, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="62a15-111">tooconfigure Azure AD integration with Boomi, you need hello following items:</span></span>

- <span data-ttu-id="62a15-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="62a15-112">An Azure AD subscription</span></span>
- <span data-ttu-id="62a15-113">En Boomi enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="62a15-113">A Boomi single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="62a15-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="62a15-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="62a15-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="62a15-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="62a15-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="62a15-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="62a15-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="62a15-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="62a15-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="62a15-118">Scenario description</span></span>
<span data-ttu-id="62a15-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="62a15-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="62a15-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="62a15-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="62a15-121">Att lägga till Boomi från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="62a15-121">Adding Boomi from hello gallery</span></span>
2. <span data-ttu-id="62a15-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a15-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-boomi-from-hello-gallery"></a><span data-ttu-id="62a15-123">Att lägga till Boomi från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="62a15-123">Adding Boomi from hello gallery</span></span>
<span data-ttu-id="62a15-124">tooconfigure hello integrering av Boomi i Azure AD, behöver du tooadd Boomi hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="62a15-124">tooconfigure hello integration of Boomi into Azure AD, you need tooadd Boomi from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="62a15-125">**tooadd Boomi från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="62a15-125">**tooadd Boomi from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a15-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="62a15-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="62a15-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="62a15-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="62a15-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="62a15-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="62a15-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a15-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="62a15-133">Skriv i sökrutan hello **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="62a15-133">In hello search box, type **Boomi**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. <span data-ttu-id="62a15-135">Markera hello resultat på panelen **Boomi**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="62a15-135">In hello results panel, select **Boomi**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="62a15-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a15-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="62a15-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Boomi baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="62a15-138">In this section, you configure and test Azure AD single sign-on with Boomi based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="62a15-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Boomi är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="62a15-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Boomi is tooa user in Azure AD.</span></span> <span data-ttu-id="62a15-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Boomi toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="62a15-140">In other words, a link relationship between an Azure AD user and hello related user in Boomi needs toobe established.</span></span>

<span data-ttu-id="62a15-141">I Boomi, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="62a15-141">In Boomi, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="62a15-142">tooconfigure och testa Azure AD enkel inloggning med Boomi, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="62a15-142">tooconfigure and test Azure AD single sign-on with Boomi, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="62a15-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="62a15-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="62a15-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="62a15-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="62a15-145">**[Skapa en testanvändare Boomi](#creating-a-boomi-test-user)**  -toohave en motsvarighet för Britta Simon i Boomi som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="62a15-145">**[Creating a Boomi test user](#creating-a-boomi-test-user)** - toohave a counterpart of Britta Simon in Boomi that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="62a15-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="62a15-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="62a15-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="62a15-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="62a15-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a15-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="62a15-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Boomi program.</span><span class="sxs-lookup"><span data-stu-id="62a15-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Boomi application.</span></span>

<span data-ttu-id="62a15-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Boomi:**</span><span class="sxs-lookup"><span data-stu-id="62a15-150">**tooconfigure Azure AD single sign-on with Boomi, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a15-151">I hello Azure-portalen på hello **Boomi** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="62a15-151">In hello Azure portal, on hello **Boomi** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="62a15-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="62a15-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. <span data-ttu-id="62a15-155">På hello **Boomi domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="62a15-155">On hello **Boomi Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    <span data-ttu-id="62a15-157">a.</span><span class="sxs-lookup"><span data-stu-id="62a15-157">a.</span></span> <span data-ttu-id="62a15-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="62a15-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    <span data-ttu-id="62a15-159">b.</span><span class="sxs-lookup"><span data-stu-id="62a15-159">b.</span></span> <span data-ttu-id="62a15-160">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://platform.boomi.com/sso/<accountname>/saml`</span><span class="sxs-lookup"><span data-stu-id="62a15-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://platform.boomi.com/sso/<accountname>/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="62a15-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="62a15-161">These values are not real.</span></span> <span data-ttu-id="62a15-162">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="62a15-162">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="62a15-163">Kontakta [Boomi supportteamet](https://boomi.com/company/contact/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="62a15-163">Contact [Boomi support team](https://boomi.com/company/contact/) tooget these values.</span></span>

4. <span data-ttu-id="62a15-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="62a15-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. <span data-ttu-id="62a15-166">Boomi program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="62a15-166">Boomi application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="62a15-167">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="62a15-167">Please configure hello following claims for this application.</span></span> <span data-ttu-id="62a15-168">Du kan hantera hello värden för attributen från hello ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="62a15-168">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="62a15-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="62a15-169">hello following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. <span data-ttu-id="62a15-171">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan för varje rad som visas i hello nedan, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="62a15-171">In hello **User Attributes** section on hello **Single sign-on** dialog, for each row shown in hello table below, perform hello following steps:</span></span>

    | <span data-ttu-id="62a15-172">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="62a15-172">Attribute Name</span></span> | <span data-ttu-id="62a15-173">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="62a15-173">Attribute Value</span></span> |
    | -------------- | --------------- |
    | <span data-ttu-id="62a15-174">FEDERATION_ID</span><span class="sxs-lookup"><span data-stu-id="62a15-174">FEDERATION_ID</span></span> | <span data-ttu-id="62a15-175">User.Mail</span><span class="sxs-lookup"><span data-stu-id="62a15-175">user.mail</span></span> |
    
    <span data-ttu-id="62a15-176">a.</span><span class="sxs-lookup"><span data-stu-id="62a15-176">a.</span></span> <span data-ttu-id="62a15-177">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a15-177">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="62a15-180">b.</span><span class="sxs-lookup"><span data-stu-id="62a15-180">b.</span></span> <span data-ttu-id="62a15-181">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="62a15-181">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="62a15-182">c.</span><span class="sxs-lookup"><span data-stu-id="62a15-182">c.</span></span> <span data-ttu-id="62a15-183">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="62a15-183">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="62a15-184">d.</span><span class="sxs-lookup"><span data-stu-id="62a15-184">d.</span></span> <span data-ttu-id="62a15-185">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="62a15-185">Click **Ok**.</span></span>

6. <span data-ttu-id="62a15-186">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="62a15-186">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="62a15-188">På hello **Boomi Configuration** klickar du på **konfigurera Boomi** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="62a15-188">On hello **Boomi Configuration** section, click **Configure Boomi** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="62a15-189">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="62a15-189">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. <span data-ttu-id="62a15-191">Logga in på webbplatsen Boomi företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="62a15-191">In a different web browser window, log into your Boomi company site as an administrator.</span></span> 

9. <span data-ttu-id="62a15-192">Navigera för**företagsnamn** och gå för**konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="62a15-192">Navigate too**Company Name** and go too**Set up**.</span></span>

10. <span data-ttu-id="62a15-193">Klicka på hello **SSO-alternativ** fliken och utföra nedanstående steg.</span><span class="sxs-lookup"><span data-stu-id="62a15-193">Click hello **SSO Options** tab and perform below steps.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    <span data-ttu-id="62a15-195">a.</span><span class="sxs-lookup"><span data-stu-id="62a15-195">a.</span></span> <span data-ttu-id="62a15-196">Kontrollera **aktivera SAML enkel inloggning** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="62a15-196">Check **Enable SAML Single Sign-On** checkbox.</span></span>

    <span data-ttu-id="62a15-197">b.</span><span class="sxs-lookup"><span data-stu-id="62a15-197">b.</span></span> <span data-ttu-id="62a15-198">Klicka på **importera** tooupload hello hämtat certifikat från Azure AD för**providern identitetscertifikat**.</span><span class="sxs-lookup"><span data-stu-id="62a15-198">Click **Import** tooupload hello downloaded certificate from Azure AD too**Identity Provider Certificate**.</span></span>
    
    <span data-ttu-id="62a15-199">c.</span><span class="sxs-lookup"><span data-stu-id="62a15-199">c.</span></span> <span data-ttu-id="62a15-200">I hello **identitet providern inloggnings-URL** textruta placera hello värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="62a15-200">In hello **Identity Provider Login URL** textbox, put hello value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="62a15-201">d.</span><span class="sxs-lookup"><span data-stu-id="62a15-201">d.</span></span> <span data-ttu-id="62a15-202">Som **Federation Id plats**väljer **Federation-Id är i FEDERATION_ID attributelementet** knappen.</span><span class="sxs-lookup"><span data-stu-id="62a15-202">As **Federation Id Location**, select **Federation Id is in FEDERATION_ID Attribute element** radio button.</span></span> 

    <span data-ttu-id="62a15-203">e.</span><span class="sxs-lookup"><span data-stu-id="62a15-203">e.</span></span> <span data-ttu-id="62a15-204">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="62a15-204">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="62a15-205">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="62a15-205">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="62a15-206">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="62a15-206">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="62a15-207">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="62a15-207">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="62a15-208">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="62a15-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="62a15-209">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="62a15-209">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="62a15-211">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="62a15-211">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a15-212">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="62a15-212">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="62a15-214">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="62a15-214">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="62a15-216">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a15-216">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="62a15-218">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="62a15-218">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="62a15-220">a.</span><span class="sxs-lookup"><span data-stu-id="62a15-220">a.</span></span> <span data-ttu-id="62a15-221">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="62a15-221">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="62a15-222">b.</span><span class="sxs-lookup"><span data-stu-id="62a15-222">b.</span></span> <span data-ttu-id="62a15-223">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="62a15-223">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="62a15-224">c.</span><span class="sxs-lookup"><span data-stu-id="62a15-224">c.</span></span> <span data-ttu-id="62a15-225">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="62a15-225">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="62a15-226">d.</span><span class="sxs-lookup"><span data-stu-id="62a15-226">d.</span></span> <span data-ttu-id="62a15-227">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="62a15-227">Click **Create**.</span></span>
 
### <a name="creating-a-boomi-test-user"></a><span data-ttu-id="62a15-228">Skapa en testanvändare Boomi</span><span class="sxs-lookup"><span data-stu-id="62a15-228">Creating a Boomi test user</span></span>

<span data-ttu-id="62a15-229">I ordning tooenable Azure AD-användare toolog i tooBoomi, måste de etableras i Boomi.</span><span class="sxs-lookup"><span data-stu-id="62a15-229">In order tooenable Azure AD users toolog in tooBoomi, they must be provisioned into Boomi.</span></span> <span data-ttu-id="62a15-230">Hello gäller Boomi är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="62a15-230">In hello case of Boomi, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="62a15-231">tooprovision ett användarkonto, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="62a15-231">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="62a15-232">Logga in tooyour Boomi företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="62a15-232">Log in tooyour Boomi company site as an administrator.</span></span>

2. <span data-ttu-id="62a15-233">När du loggar in, navigera för**Användarhantering** och gå för**användare**.</span><span class="sxs-lookup"><span data-stu-id="62a15-233">After logging in, navigate too**User Management** and go too**Users**.</span></span>

    <span data-ttu-id="62a15-234">![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "användare")</span><span class="sxs-lookup"><span data-stu-id="62a15-234">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")</span></span>

3. <span data-ttu-id="62a15-235">Klicka på  **+**  ikon och hello **Lägg till/Underhåll användarroller** öppnas.</span><span class="sxs-lookup"><span data-stu-id="62a15-235">Click **+**  icon and hello **Add/Maintain User Roles** dialog opens.</span></span>

    <span data-ttu-id="62a15-236">![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "användare")</span><span class="sxs-lookup"><span data-stu-id="62a15-236">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")</span></span>

    <span data-ttu-id="62a15-237">![Användare](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "användare")</span><span class="sxs-lookup"><span data-stu-id="62a15-237">![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")</span></span>

    <span data-ttu-id="62a15-238">a.</span><span class="sxs-lookup"><span data-stu-id="62a15-238">a.</span></span> <span data-ttu-id="62a15-239">I hello **användarens e-postadress** textruta hello e-post för användare som BrittaSimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="62a15-239">In hello **User e-mail address** textbox, type hello email of user like BrittaSimon@contoso.com.</span></span>
    
    <span data-ttu-id="62a15-240">b.</span><span class="sxs-lookup"><span data-stu-id="62a15-240">b.</span></span> <span data-ttu-id="62a15-241">I hello **Förnamn** textruta hello första-typnamn för användaren som Britta.</span><span class="sxs-lookup"><span data-stu-id="62a15-241">In hello **First name** textbox, type hello First name of user like Britta.</span></span>

    <span data-ttu-id="62a15-242">c.</span><span class="sxs-lookup"><span data-stu-id="62a15-242">c.</span></span> <span data-ttu-id="62a15-243">I hello **efternamn** textruta Skriv hello efternamn för användaren som Simon.</span><span class="sxs-lookup"><span data-stu-id="62a15-243">In hello **Last name** textbox, type hello Last name of user like Simon.</span></span>
    
    <span data-ttu-id="62a15-244">d.</span><span class="sxs-lookup"><span data-stu-id="62a15-244">d.</span></span> <span data-ttu-id="62a15-245">Ange hello användare **Federation ID**.</span><span class="sxs-lookup"><span data-stu-id="62a15-245">Enter hello user's **Federation ID**.</span></span> <span data-ttu-id="62a15-246">Varje användare måste ha ett ID för Federation som unikt identifierar hello användaren i hello-konto.</span><span class="sxs-lookup"><span data-stu-id="62a15-246">Each user must have a Federation ID that uniquely identifies hello user within hello account.</span></span>
    
    <span data-ttu-id="62a15-247">e.</span><span class="sxs-lookup"><span data-stu-id="62a15-247">e.</span></span> <span data-ttu-id="62a15-248">Tilldela hello **standardanvändare** rollen toohello användare.</span><span class="sxs-lookup"><span data-stu-id="62a15-248">Assign hello **Standard User** role toohello user.</span></span> <span data-ttu-id="62a15-249">Tilldela inte hello administratörsroll eftersom som skulle ge honom normal luften åtkomst samt åtkomst för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="62a15-249">Do not assign hello Administrator role because that would give him normal Atmosphere access as well as single sign-on access.</span></span>
    
    <span data-ttu-id="62a15-250">f.</span><span class="sxs-lookup"><span data-stu-id="62a15-250">f.</span></span> <span data-ttu-id="62a15-251">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="62a15-251">Click **OK**.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="62a15-252">hello användaren får inte en Välkommen e-postmeddelandet som innehåller ett lösenord som kan vara används toolog i toohello AtomSphere konto eftersom sitt lösenord hanteras via hello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="62a15-252">hello user will not receive a welcome notification email containing a password that can be used toolog in toohello AtomSphere account because his password is managed through hello identity provider.</span></span> <span data-ttu-id="62a15-253">Du kan använda andra Boomi användarens konto skapas verktyg eller API: er som tillhandahålls av Boomi tooprovision AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="62a15-253">You may use any other Boomi user account creation tools or APIs provided by Boomi tooprovision AAD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="62a15-254">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="62a15-254">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="62a15-255">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooBoomi.</span><span class="sxs-lookup"><span data-stu-id="62a15-255">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBoomi.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="62a15-257">**tooassign Britta Simon tooBoomi utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="62a15-257">**tooassign Britta Simon tooBoomi, perform hello following steps:**</span></span>

1. <span data-ttu-id="62a15-258">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="62a15-258">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="62a15-260">Välj i listan med program hello **Boomi**.</span><span class="sxs-lookup"><span data-stu-id="62a15-260">In hello applications list, select **Boomi**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. <span data-ttu-id="62a15-262">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="62a15-262">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="62a15-264">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="62a15-264">Click **Add** button.</span></span> <span data-ttu-id="62a15-265">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a15-265">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="62a15-267">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="62a15-267">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="62a15-268">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a15-268">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="62a15-269">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="62a15-269">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="62a15-270">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="62a15-270">Testing single sign-on</span></span>

<span data-ttu-id="62a15-271">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="62a15-271">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="62a15-272">Du bör få automatiskt inloggade tooyour Boomi programmet när du klickar på hello Boomi panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="62a15-272">When you click hello Boomi tile in hello Access Panel, you should get automatically signed-on tooyour Boomi application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62a15-273">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="62a15-273">Additional resources</span></span>

* [<span data-ttu-id="62a15-274">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="62a15-274">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="62a15-275">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="62a15-275">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png

