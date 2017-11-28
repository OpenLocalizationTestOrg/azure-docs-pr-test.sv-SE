---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Salesforce Sandbox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7539f08356568a17ebfcee2764bbbefa129b0553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="5649c-103">Självstudier: Azure Active Directory-integrering med Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="5649c-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="5649c-104">I kursen får du lära dig hur toointegrate Salesforce Sandbox med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5649c-104">In this tutorial, you learn how toointegrate Salesforce Sandbox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5649c-105">Integrera Salesforce Sandbox med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5649c-105">Integrating Salesforce Sandbox with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5649c-106">Du kan styra i Azure AD som har åtkomst tooSalesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="5649c-106">You can control in Azure AD who has access tooSalesforce Sandbox</span></span>
- <span data-ttu-id="5649c-107">Du kan aktivera din användare tooautomatically get inloggade tooSalesforce begränsat läge (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5649c-107">You can enable your users tooautomatically get signed-on tooSalesforce Sandbox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5649c-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5649c-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="5649c-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5649c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5649c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5649c-110">Prerequisites</span></span>

<span data-ttu-id="5649c-111">tooconfigure Azure AD-integrering med Salesforce Sandbox måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="5649c-111">tooconfigure Azure AD integration with Salesforce Sandbox, you need hello following items:</span></span>

- <span data-ttu-id="5649c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5649c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5649c-113">En Salesforce Sandbox enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5649c-113">A Salesforce Sandbox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5649c-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5649c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5649c-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5649c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5649c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5649c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5649c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5649c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5649c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5649c-118">Scenario description</span></span>
<span data-ttu-id="5649c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5649c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5649c-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5649c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5649c-121">Att lägga till Salesforce Sandbox från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5649c-121">Adding Salesforce Sandbox from hello gallery</span></span>
2. <span data-ttu-id="5649c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5649c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-sandbox-from-hello-gallery"></a><span data-ttu-id="5649c-123">Att lägga till Salesforce Sandbox från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="5649c-123">Adding Salesforce Sandbox from hello gallery</span></span>
<span data-ttu-id="5649c-124">tooconfigure hello integrering av Salesforce Sandbox i Azure AD, behöver du tooadd Salesforce Sandbox hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="5649c-124">tooconfigure hello integration of Salesforce Sandbox into Azure AD, you need tooadd Salesforce Sandbox from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5649c-125">**tooadd Salesforce Sandbox från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5649c-125">**tooadd Salesforce Sandbox from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5649c-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5649c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5649c-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5649c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5649c-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="5649c-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5649c-131">Klicka på **nytt program** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5649c-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5649c-133">Skriv i sökrutan hello **Salesforce Sandbox**.</span><span class="sxs-lookup"><span data-stu-id="5649c-133">In hello search box, type **Salesforce Sandbox**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. <span data-ttu-id="5649c-135">Markera hello resultat på panelen **Salesforce Sandbox**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="5649c-135">In hello results panel, select **Salesforce Sandbox**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5649c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5649c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5649c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Salesforce Sandbox baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5649c-138">In this section, you configure and test Azure AD single sign-on with Salesforce Sandbox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5649c-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Salesforce Sandbox är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5649c-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Salesforce Sandbox is tooa user in Azure AD.</span></span> <span data-ttu-id="5649c-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Salesforce Sandbox toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="5649c-140">In other words, a link relationship between an Azure AD user and hello related user in Salesforce Sandbox needs toobe established.</span></span>

<span data-ttu-id="5649c-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="5649c-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Salesforce Sandbox.</span></span>

<span data-ttu-id="5649c-142">tooconfigure och testa Azure AD enkel inloggning med Salesforce Sandbox, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5649c-142">tooconfigure and test Azure AD single sign-on with Salesforce Sandbox, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5649c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5649c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5649c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5649c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5649c-145">**[Skapa en testanvändare Salesforce Sandbox](#creating-a-salesforce-sandbox-test-user)**  -toohave en motsvarighet för Britta Simon i Salesforce Sandbox som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5649c-145">**[Creating a Salesforce Sandbox test user](#creating-a-salesforce-sandbox-test-user)** - toohave a counterpart of Britta Simon in Salesforce Sandbox that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="5649c-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5649c-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5649c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5649c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5649c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5649c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5649c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Salesforce-Sandbox-program.</span><span class="sxs-lookup"><span data-stu-id="5649c-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Salesforce Sandbox application.</span></span>

<span data-ttu-id="5649c-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Salesforce Sandbox:**</span><span class="sxs-lookup"><span data-stu-id="5649c-150">**tooconfigure Azure AD single sign-on with Salesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="5649c-151">I hello Azure-portalen på hello **Salesforce Sandbox** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5649c-151">In hello Azure portal, on hello **Salesforce Sandbox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5649c-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5649c-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. <span data-ttu-id="5649c-155">På hello **Salesforce Sandbox domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5649c-155">On hello **Salesforce Sandbox Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     <span data-ttu-id="5649c-157">I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="5649c-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<subdomain>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5649c-158">Det här värdet är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="5649c-158">This value is not hello real.</span></span> <span data-ttu-id="5649c-159">Uppdatera det här värdet med hello faktiska inloggnings-URL. Kontakta [Salesforce Sandbox klienten supportteamet](https://help.salesforce.com/support) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="5649c-159">Update this value with hello actual Sign-on URL.Contact [Salesforce Sandbox Client support team](https://help.salesforce.com/support) tooget this value.</span></span>


4. <span data-ttu-id="5649c-160">Om du redan har konfigurerat enkel inloggning för en annan Salesforce Sandbox-instans i katalogen, så du måste också konfigurera hello **identifierare** toohave hello samma värde som hello **inloggning URL**.</span><span class="sxs-lookup"><span data-stu-id="5649c-160">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure hello **Identifier** toohave hello same value as hello **Sign on URL**.</span></span> 
    
    >[!Note]
    ><span data-ttu-id="5649c-161">Hej **identifierare** fältet finns genom att kontrollera hello **visa avancerade inställningar** kryssruta på hello **konfigurera App-URL** sidan för hello dialog</span><span class="sxs-lookup"><span data-stu-id="5649c-161">hello **Identifier** field can be found by checking hello **Show advanced settings** checkbox on hello **Configure App URL** page of hello dialog</span></span> 


5. <span data-ttu-id="5649c-162">På hello **SAML-signeringscertifikat** klickar du på **certifikat** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="5649c-162">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. <span data-ttu-id="5649c-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5649c-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="5649c-166">På hello **Salesforce Sandbox Configuration** klickar du på **konfigurera Salesforce Sandbox** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5649c-166">On hello **Salesforce Sandbox Configuration** section, click **Configure Salesforce Sandbox** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="5649c-167">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5649c-167">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="5649c-168">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="5649c-168">![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span></span>
8. <span data-ttu-id="5649c-169">Öppna en ny flik i webbläsaren och logga in tooyour Salesforce Sandbox-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="5649c-169">Open a new tab in your browser and log in tooyour Salesforce Sandbox administrator account.</span></span>

9. <span data-ttu-id="5649c-170">Hello-menyn överst hello **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="5649c-170">In hello menu on hello top, click **Setup**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. <span data-ttu-id="5649c-172">Hello navigeringsfönstret hello vänster, klicka på **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5649c-172">In hello navigation pane on hello left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. <span data-ttu-id="5649c-174">På hello inställningar för enkel inloggning, utför du följande hello: ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span><span class="sxs-lookup"><span data-stu-id="5649c-174">On hello Single Sign-On Settings section, perform hello following steps:  ![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span></span>
     
     <span data-ttu-id="5649c-175">a.</span><span class="sxs-lookup"><span data-stu-id="5649c-175">a.</span></span>  <span data-ttu-id="5649c-176">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="5649c-176">Select **SAML Enabled**.</span></span> 

     <span data-ttu-id="5649c-177">b.</span><span class="sxs-lookup"><span data-stu-id="5649c-177">b.</span></span>  <span data-ttu-id="5649c-178">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="5649c-178">Click **New**.</span></span>

12. <span data-ttu-id="5649c-179">Utför följande hello på hello SAML enkel inloggning inställningar:</span><span class="sxs-lookup"><span data-stu-id="5649c-179">On hello SAML Single Sign-On Settings section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    <span data-ttu-id="5649c-181">a.In hello textrutan, hello-typnamn för hello-konfiguration (t.ex.: *SPSSOWAAD\_Test*).</span><span class="sxs-lookup"><span data-stu-id="5649c-181">a.In hello Name textbox, type hello name of hello configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 

    <span data-ttu-id="5649c-182">b.</span><span class="sxs-lookup"><span data-stu-id="5649c-182">b.</span></span> <span data-ttu-id="5649c-183">Klistra in **SMAL enhets-ID** värdet till hello **utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="5649c-183">Paste **SMAL Entity ID** value into hello **Issuer** textbox.</span></span>

    <span data-ttu-id="5649c-184">c.</span><span class="sxs-lookup"><span data-stu-id="5649c-184">c.</span></span> <span data-ttu-id="5649c-185">I hello **enhets-Id** textruta typen **https://test.salesforce.com** om det är hello första Salesforce Sandbox-instans som du lägger till tooyour directory.</span><span class="sxs-lookup"><span data-stu-id="5649c-185">In hello **Entity Id** textbox, type **https://test.salesforce.com** if it is hello first Salesforce Sandbox instance that you are adding tooyour directory.</span></span> <span data-ttu-id="5649c-186">Om du redan har lagt till en instans av Salesforce begränsat, och därefter för hello **enhets-ID** typen i hello **logga URL**, som ska vara i formatet:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="5649c-186">If you have already added an instance of Salesforce Sandbox, then for hello **Entity ID** type in hello **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>  
 
    <span data-ttu-id="5649c-187">d.</span><span class="sxs-lookup"><span data-stu-id="5649c-187">d.</span></span> <span data-ttu-id="5649c-188">Klicka på **Bläddra** tooupload hello hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="5649c-188">Click **Browse** tooupload hello downloaded certificate.</span></span>  

    <span data-ttu-id="5649c-189">e.</span><span class="sxs-lookup"><span data-stu-id="5649c-189">e.</span></span> <span data-ttu-id="5649c-190">Som **SAML identitetstyp**väljer **Assertion innehåller hello Federation ID från hello användarobjektet**.</span><span class="sxs-lookup"><span data-stu-id="5649c-190">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>
 
    <span data-ttu-id="5649c-191">f.</span><span class="sxs-lookup"><span data-stu-id="5649c-191">f.</span></span> <span data-ttu-id="5649c-192">Som **SAML identitet plats**väljer **identitet är i hello NameIdentifier elementet i hello ämne instruktionen**.</span><span class="sxs-lookup"><span data-stu-id="5649c-192">As **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>

    <span data-ttu-id="5649c-193">g.</span><span class="sxs-lookup"><span data-stu-id="5649c-193">g.</span></span> <span data-ttu-id="5649c-194">Klistra in **inloggning tjänst-URL för enkel** till hello **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="5649c-194">Paste **Single Sign-On Service URL** into hello **Identity Provider Login URL** textbox.</span></span> 

    <span data-ttu-id="5649c-195">h.</span><span class="sxs-lookup"><span data-stu-id="5649c-195">h.</span></span> <span data-ttu-id="5649c-196">SFDC har inte stöd för SAML logga ut.</span><span class="sxs-lookup"><span data-stu-id="5649c-196">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="5649c-197">Som en tillfällig lösning kan du klistra in 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' till hello **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="5649c-197">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="5649c-198">Jag.</span><span class="sxs-lookup"><span data-stu-id="5649c-198">i.</span></span> <span data-ttu-id="5649c-199">Som **providern initierade begära Tjänstbindning**väljer **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="5649c-199">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 

    <span data-ttu-id="5649c-200">j.</span><span class="sxs-lookup"><span data-stu-id="5649c-200">j.</span></span> <span data-ttu-id="5649c-201">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5649c-201">Click **Save**.</span></span>

### <a name="enable-your-domain"></a><span data-ttu-id="5649c-202">Aktivera din domän</span><span class="sxs-lookup"><span data-stu-id="5649c-202">Enable your domain</span></span>
<span data-ttu-id="5649c-203">Det här avsnittet förutsätter att du redan har skapat en domän.</span><span class="sxs-lookup"><span data-stu-id="5649c-203">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="5649c-204">Mer information finns i [definierar domännamn](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="5649c-204">For more information, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="5649c-205">**tooenable din domän, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5649c-205">**tooenable your domain, perform hello following steps:**</span></span>

1. <span data-ttu-id="5649c-206">Hello vänstra navigeringsfönstret, klicka på **domänhantering**, och klicka sedan på **min domän.**</span><span class="sxs-lookup"><span data-stu-id="5649c-206">In hello left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
     ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   ><span data-ttu-id="5649c-208">Kontrollera att din domän har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="5649c-208">Please make sure that your domain has been configured correctly.</span></span> 

2. <span data-ttu-id="5649c-209">I hello **sidan inloggningsinställningar** klickar du på **redigera**, sedan som **Autentiseringstjänsten**väljer hello namnet på hello SAML enkel inloggning inställningen från hello tidigare avsnittet och klicka slutligen på **spara**.</span><span class="sxs-lookup"><span data-stu-id="5649c-209">In hello **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select hello name of hello SAML Single Sign-On Setting from hello previous section, and finally click **Save**.</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

<span data-ttu-id="5649c-211">Så snart du har en domän som har konfigurerats, bör användarna använda hello domän URL toologin toohello Salesforce sandbox.</span><span class="sxs-lookup"><span data-stu-id="5649c-211">As soon as you have a domain configured, your users should use hello domain URL toologin toohello Salesforce sandbox.</span></span>  

<span data-ttu-id="5649c-212">tooget hello värdet för hello URL på hello SSO profil du har skapat i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="5649c-212">tooget hello value of hello URL, click hello SSO profile you have created in hello previous section.</span></span>    

> [!TIP]
> <span data-ttu-id="5649c-213">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="5649c-213">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="5649c-214">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="5649c-214">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="5649c-215">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5649c-215">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5649c-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5649c-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="5649c-217">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5649c-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5649c-219">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5649c-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5649c-220">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5649c-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5649c-222">toodisplay hello lista över användare gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5649c-222">toodisplay hello list of users go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5649c-224">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5649c-224">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5649c-226">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="5649c-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5649c-228">a.</span><span class="sxs-lookup"><span data-stu-id="5649c-228">a.</span></span> <span data-ttu-id="5649c-229">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5649c-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5649c-230">b.</span><span class="sxs-lookup"><span data-stu-id="5649c-230">b.</span></span> <span data-ttu-id="5649c-231">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5649c-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5649c-232">c.</span><span class="sxs-lookup"><span data-stu-id="5649c-232">c.</span></span> <span data-ttu-id="5649c-233">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5649c-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5649c-234">d.</span><span class="sxs-lookup"><span data-stu-id="5649c-234">d.</span></span> <span data-ttu-id="5649c-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5649c-235">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-sandbox-test-user"></a><span data-ttu-id="5649c-236">Skapa en testanvändare Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="5649c-236">Creating a Salesforce Sandbox test user</span></span>

<span data-ttu-id="5649c-237">I det här avsnittet skapas en användare som kallas Britta Simon i Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="5649c-237">In this section, a user called Britta Simon is created in Salesforce Sandbox.</span></span> <span data-ttu-id="5649c-238">Salesforce Sandbox stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="5649c-238">Salesforce Sandbox supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="5649c-239">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="5649c-239">There is no action item for you in this section.</span></span> <span data-ttu-id="5649c-240">Om en användare inte redan finns i Salesforce Sandbox, skapas en ny när du försöker tooaccess Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="5649c-240">If a user doesn't already exist in Salesforce Sandbox, a new one is created when you attempt tooaccess Salesforce Sandbox.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5649c-241">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="5649c-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5649c-242">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSalesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="5649c-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSalesforce Sandbox.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5649c-244">**tooassign Britta Simon tooSalesforce Sandbox, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5649c-244">**tooassign Britta Simon tooSalesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="5649c-245">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5649c-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5649c-247">Välj i listan med program hello **Salesforce Sandbox**.</span><span class="sxs-lookup"><span data-stu-id="5649c-247">In hello applications list, select **Salesforce Sandbox**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. <span data-ttu-id="5649c-249">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5649c-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5649c-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5649c-251">Click **Add** button.</span></span> <span data-ttu-id="5649c-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5649c-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5649c-254">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="5649c-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5649c-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5649c-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5649c-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5649c-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5649c-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5649c-257">Testing single sign-on</span></span>

<span data-ttu-id="5649c-258">Om du vill tootest SSO-inställningarna, öppna hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5649c-258">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="5649c-259">Mer information om hello åtkomstpanelen finns [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="5649c-259">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5649c-260">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5649c-260">Additional resources</span></span>

* [<span data-ttu-id="5649c-261">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5649c-261">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5649c-262">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5649c-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="5649c-263">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="5649c-263">Configure User Provisioning</span></span>](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

