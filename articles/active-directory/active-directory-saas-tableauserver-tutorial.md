---
title: "Självstudier: Azure Active Directory-integrering med Tableau Server | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Tableau Server."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: feb2087bd6ae6ddcb920901e6719688fc95ae287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a><span data-ttu-id="22825-103">Självstudier: Azure Active Directory-integrering med Tableau Server</span><span class="sxs-lookup"><span data-stu-id="22825-103">Tutorial: Azure Active Directory integration with Tableau Server</span></span>

<span data-ttu-id="22825-104">I kursen får du lära dig hur toointegrate Tableau Server med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="22825-104">In this tutorial, you learn how toointegrate Tableau Server with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="22825-105">Integrera Tableau Server med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="22825-105">Integrating Tableau Server with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="22825-106">Du kan styra i Azure AD som har åtkomst tooTableau Server</span><span class="sxs-lookup"><span data-stu-id="22825-106">You can control in Azure AD who has access tooTableau Server</span></span>
- <span data-ttu-id="22825-107">Du kan aktivera din användare tooautomatically get inloggade tooTableau Server (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="22825-107">You can enable your users tooautomatically get signed-on tooTableau Server (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="22825-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="22825-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="22825-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="22825-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22825-110">Krav</span><span class="sxs-lookup"><span data-stu-id="22825-110">Prerequisites</span></span>

<span data-ttu-id="22825-111">tooconfigure Azure AD-integrering med Tableau Server behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="22825-111">tooconfigure Azure AD integration with Tableau Server, you need hello following items:</span></span>

- <span data-ttu-id="22825-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="22825-112">An Azure AD subscription</span></span>
- <span data-ttu-id="22825-113">En Tableau Server enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="22825-113">A Tableau Server single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="22825-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="22825-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="22825-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="22825-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="22825-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="22825-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="22825-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="22825-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="22825-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="22825-118">Scenario description</span></span>
<span data-ttu-id="22825-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="22825-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="22825-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="22825-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="22825-121">Att lägga till Tableau Server från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="22825-121">Adding Tableau Server from hello gallery</span></span>
2. <span data-ttu-id="22825-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22825-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-server-from-hello-gallery"></a><span data-ttu-id="22825-123">Att lägga till Tableau Server från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="22825-123">Adding Tableau Server from hello gallery</span></span>
<span data-ttu-id="22825-124">tooconfigure hello integrering av Tableau Server i Azure AD, behöver du tooadd Tableau Server hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="22825-124">tooconfigure hello integration of Tableau Server into Azure AD, you need tooadd Tableau Server from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="22825-125">**tooadd Tableau Server från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="22825-125">**tooadd Tableau Server from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="22825-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="22825-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="22825-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="22825-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="22825-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="22825-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="22825-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22825-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="22825-133">Skriv i sökrutan hello **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="22825-133">In hello search box, type **Tableau Server**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. <span data-ttu-id="22825-135">Markera hello resultat på panelen **Tableau Server**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="22825-135">In hello results panel, select **Tableau Server**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="22825-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22825-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="22825-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Tableau Server baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="22825-138">In this section, you configure and test Azure AD single sign-on with Tableau Server based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="22825-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Tableau Server är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22825-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Tableau Server is tooa user in Azure AD.</span></span> <span data-ttu-id="22825-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Tableau servern toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="22825-140">In other words, a link relationship between an Azure AD user and hello related user in Tableau Server needs toobe established.</span></span>

<span data-ttu-id="22825-141">Tilldela hello värdet hello i Tableau Server **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="22825-141">In Tableau Server, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="22825-142">tooconfigure och testa Azure AD enkel inloggning med Tableau Server behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="22825-142">tooconfigure and test Azure AD single sign-on with Tableau Server, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="22825-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="22825-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="22825-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22825-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="22825-145">**[Skapa en testanvändare Tableau Server](#creating-a-tableau-server-test-user)**  -toohave en motsvarighet för Britta Simon i Tableau servern som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="22825-145">**[Creating a Tableau Server test user](#creating-a-tableau-server-test-user)** - toohave a counterpart of Britta Simon in Tableau Server that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="22825-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="22825-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="22825-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="22825-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="22825-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22825-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="22825-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i serverprogrammet Tableau.</span><span class="sxs-lookup"><span data-stu-id="22825-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Tableau Server application.</span></span>

<span data-ttu-id="22825-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Tableau Server:**</span><span class="sxs-lookup"><span data-stu-id="22825-150">**tooconfigure Azure AD single sign-on with Tableau Server, perform hello following steps:**</span></span>

1. <span data-ttu-id="22825-151">I hello Azure-portalen på hello **Tableau Server** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="22825-151">In hello Azure portal, on hello **Tableau Server** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="22825-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="22825-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. <span data-ttu-id="22825-155">På hello **URL: er och Tableau serverdomänen** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="22825-155">On hello **Tableau Server Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    <span data-ttu-id="22825-157">a.</span><span class="sxs-lookup"><span data-stu-id="22825-157">a.</span></span> <span data-ttu-id="22825-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="22825-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link`</span></span>
    
    <span data-ttu-id="22825-159">b.</span><span class="sxs-lookup"><span data-stu-id="22825-159">b.</span></span> <span data-ttu-id="22825-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="22825-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link`</span></span>

    <span data-ttu-id="22825-161">c.</span><span class="sxs-lookup"><span data-stu-id="22825-161">c.</span></span> <span data-ttu-id="22825-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://azure.<domain name>.link/wg/saml/SSO/index.html`</span><span class="sxs-lookup"><span data-stu-id="22825-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://azure.<domain name>.link/wg/saml/SSO/index.html`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="22825-163">hello föregående värden är inte verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="22825-163">hello preceding values are not real values.</span></span> <span data-ttu-id="22825-164">Senare kan uppdatera du hello värden med hello faktiska URL och identifierare från konfigurationssidan för hello Tableau Server.</span><span class="sxs-lookup"><span data-stu-id="22825-164">Later, you update hello values with hello actual URL and identifier from hello Tableau Server configuration page.</span></span> 

4. <span data-ttu-id="22825-165">Tableau serverprogrammet förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="22825-165">Tableau Server application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="22825-166">Konfigurera hello följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="22825-166">Configure hello following claims for this application.</span></span> <span data-ttu-id="22825-167">Du kan hantera hello värden för attributen från hello **”användarattribut”** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="22825-167">You can manage hello values of these attributes from hello **"User Attributes"** section on application integration page.</span></span> <span data-ttu-id="22825-168">hello följande skärmbild visar ett exempel på hello samma.</span><span class="sxs-lookup"><span data-stu-id="22825-168">hello following screenshot shows an example for hello same.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. <span data-ttu-id="22825-170">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan Konfigurera SAML-token attribut som visas i hello bilden ovan och utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="22825-170">In hello **User Attributes** section on hello **Single sign-on** dialog, configure SAML token attribute as shown in hello image above and perform hello following steps:</span></span>
    
    | <span data-ttu-id="22825-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="22825-171">Attribute Name</span></span> | <span data-ttu-id="22825-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="22825-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="22825-173">användarnamn</span><span class="sxs-lookup"><span data-stu-id="22825-173">username</span></span> | <span data-ttu-id="22825-174">*User.DisplayName*</span><span class="sxs-lookup"><span data-stu-id="22825-174">*user.displayname*</span></span> |

    <span data-ttu-id="22825-175">a.</span><span class="sxs-lookup"><span data-stu-id="22825-175">a.</span></span> <span data-ttu-id="22825-176">Klicka på **Lägg till attributet** tooopen hello **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22825-176">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    <span data-ttu-id="22825-179">b.</span><span class="sxs-lookup"><span data-stu-id="22825-179">b.</span></span> <span data-ttu-id="22825-180">I hello **namn** textruta hello attributnamn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="22825-180">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="22825-181">c.</span><span class="sxs-lookup"><span data-stu-id="22825-181">c.</span></span> <span data-ttu-id="22825-182">Från hello **värdet** listan attributvärde för typ hello visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="22825-182">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="22825-183">d.</span><span class="sxs-lookup"><span data-stu-id="22825-183">d.</span></span> <span data-ttu-id="22825-184">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="22825-184">Click **Ok**</span></span>


6. <span data-ttu-id="22825-185">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="22825-185">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. <span data-ttu-id="22825-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="22825-187">Click **Save** button.</span></span>

    <span data-ttu-id="22825-188">![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="22825-188">![Configure Single Sign-On](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span></span>
8. <span data-ttu-id="22825-189">tooget SSO konfigurerats för ditt program måste toosign på tooyour Tableau Server-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="22825-189">tooget SSO configured for your application, you need toosign-on tooyour Tableau Server tenant as an administrator.</span></span>
   
   <span data-ttu-id="22825-190">a.</span><span class="sxs-lookup"><span data-stu-id="22825-190">a.</span></span> <span data-ttu-id="22825-191">Klicka på hello i hello Tableau serverkonfiguration **SAML** fliken.</span><span class="sxs-lookup"><span data-stu-id="22825-191">In hello Tableau Server configuration, click hello **SAML** tab.</span></span>
  
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   <span data-ttu-id="22825-193">b.</span><span class="sxs-lookup"><span data-stu-id="22825-193">b.</span></span> <span data-ttu-id="22825-194">Markera kryssrutan för hello av **Använd SAML för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="22825-194">Select hello checkbox of **Use SAML for single sign-on**.</span></span>
   
   <span data-ttu-id="22825-195">c.</span><span class="sxs-lookup"><span data-stu-id="22825-195">c.</span></span> <span data-ttu-id="22825-196">Tableau Server Retur-URL – hello URL som Tableau Server-användare kommer åt, till exempel http://tableau_server.</span><span class="sxs-lookup"><span data-stu-id="22825-196">Tableau Server return URL—hello URL that Tableau Server users will be accessing, such as http://tableau_server.</span></span> <span data-ttu-id="22825-197">Du bör inte använda http://localhost.</span><span class="sxs-lookup"><span data-stu-id="22825-197">Using http://localhost is not recommended.</span></span> <span data-ttu-id="22825-198">Med en URL som avslutande snedstreck (till exempel http://tableau_server/) stöds inte.</span><span class="sxs-lookup"><span data-stu-id="22825-198">Using a URL with a trailing slash (for example, http://tableau_server/) is not supported.</span></span> <span data-ttu-id="22825-199">Kopiera **Tableau Server Retur-URL** och klistra in den tooAzure AD **logga URL** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="22825-199">Copy **Tableau Server return URL** and paste it tooAzure AD **Sign On URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="22825-200">d.</span><span class="sxs-lookup"><span data-stu-id="22825-200">d.</span></span> <span data-ttu-id="22825-201">SAML enhets-ID – hello enhets-ID som unikt identifierar din Tableau Server installation toohello IdP.</span><span class="sxs-lookup"><span data-stu-id="22825-201">SAML entity ID—hello entity ID uniquely identifies your Tableau Server installation toohello IdP.</span></span> <span data-ttu-id="22825-202">Du kan ange Tableau Server URL: en igen här, om du vill, men den har inte toobe Tableau-Serveradress.</span><span class="sxs-lookup"><span data-stu-id="22825-202">You can enter your Tableau Server URL again here, if you like, but it does not have toobe your Tableau Server URL.</span></span> <span data-ttu-id="22825-203">Kopiera **SAML enhets-ID** och klistra in den tooAzure AD **identifierare** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="22825-203">Copy **SAML entity ID** and paste it tooAzure AD **Identifier** textbox in **Tableau Server Domain and URLs** section.</span></span>
     
   <span data-ttu-id="22825-204">e.</span><span class="sxs-lookup"><span data-stu-id="22825-204">e.</span></span> <span data-ttu-id="22825-205">Klicka på hello **exportera metadatafil** och öppna den i hello text redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="22825-205">Click hello **Export Metadata File** and open it in hello text editor application.</span></span> <span data-ttu-id="22825-206">Leta upp Assertion konsumenten tjänst-URL med Http Post och Index 0 och kopiera hello-URL.</span><span class="sxs-lookup"><span data-stu-id="22825-206">Locate Assertion Consumer Service URL with Http Post and Index 0 and copy hello URL.</span></span> <span data-ttu-id="22825-207">Klistra in den tooAzure AD **Reply URL** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="22825-207">Now paste it tooAzure AD **Reply URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="22825-208">f.</span><span class="sxs-lookup"><span data-stu-id="22825-208">f.</span></span> <span data-ttu-id="22825-209">Leta upp din Federationsmetadata-fil som hämtas från Azure-portalen och överföra den i hello **SAML Idp metadatafil**.</span><span class="sxs-lookup"><span data-stu-id="22825-209">Locate your Federation Metadata file downloaded from Azure portal, and then upload it in hello **SAML Idp metadata file**.</span></span>
   
   <span data-ttu-id="22825-210">g.</span><span class="sxs-lookup"><span data-stu-id="22825-210">g.</span></span> <span data-ttu-id="22825-211">Klicka på hello **OK** knappen hello Tableau serverkonfiguration på sidan.</span><span class="sxs-lookup"><span data-stu-id="22825-211">Click hello **OK** button in hello Tableau Server Configuration page.</span></span>
   
    >[!NOTE] 
    ><span data-ttu-id="22825-212">Kunden har tooupload alla certifikat i hello Tableau Server SAML SSO-konfigurationen och kommer hämta ignoreras i hello flödet för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="22825-212">Customer have tooupload any certificate in hello Tableau Server SAML SSO configuration and it will get ignored in hello SSO flow.</span></span>
    ><span data-ttu-id="22825-213">Om du behöver hjälp konfigurerar SAML på Tableau Server sedan se toothis artikel [konfigurera SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span><span class="sxs-lookup"><span data-stu-id="22825-213">If you need help configuring SAML on Tableau Server then please refer toothis article [Configure SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span></span>
    >
<CE>

> [!TIP]
> <span data-ttu-id="22825-214">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="22825-214">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="22825-215">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="22825-215">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="22825-216">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="22825-216">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="22825-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="22825-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="22825-218">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22825-218">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="22825-220">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="22825-220">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="22825-221">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="22825-221">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="22825-223">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="22825-223">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="22825-225">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22825-225">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="22825-227">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="22825-227">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="22825-229">a.</span><span class="sxs-lookup"><span data-stu-id="22825-229">a.</span></span> <span data-ttu-id="22825-230">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="22825-230">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="22825-231">b.</span><span class="sxs-lookup"><span data-stu-id="22825-231">b.</span></span> <span data-ttu-id="22825-232">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="22825-232">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="22825-233">c.</span><span class="sxs-lookup"><span data-stu-id="22825-233">c.</span></span> <span data-ttu-id="22825-234">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="22825-234">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="22825-235">d.</span><span class="sxs-lookup"><span data-stu-id="22825-235">d.</span></span> <span data-ttu-id="22825-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="22825-236">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-server-test-user"></a><span data-ttu-id="22825-237">Skapa en testanvändare Tableau Server</span><span class="sxs-lookup"><span data-stu-id="22825-237">Creating a Tableau Server test user</span></span>

<span data-ttu-id="22825-238">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Tableau Server.</span><span class="sxs-lookup"><span data-stu-id="22825-238">hello objective of this section is toocreate a user called Britta Simon in Tableau Server.</span></span> <span data-ttu-id="22825-239">Du måste tooprovision alla hello användare i hello Tableau server.</span><span class="sxs-lookup"><span data-stu-id="22825-239">You need tooprovision all hello users in hello Tableau server.</span></span> 

<span data-ttu-id="22825-240">Att användarnamnet för användaren hello bör matcha hello-värde som du har konfigurerat i hello Azure AD-attributet för **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="22825-240">That username of hello user should match hello value which you have configured in hello Azure AD custom attribute of **username**.</span></span> <span data-ttu-id="22825-241">Med hello rätt mappning hello integrering ska fungera [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="22825-241">With hello correct mapping hello integration should work [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="22825-242">Om du behöver toocreate en användare manuellt, måste toocontact hello Tableau Server-administratören i din organisation.</span><span class="sxs-lookup"><span data-stu-id="22825-242">If you need toocreate a user manually, you need toocontact hello Tableau Server administrator in your organization.</span></span>
> 
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="22825-243">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="22825-243">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="22825-244">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTableau Server.</span><span class="sxs-lookup"><span data-stu-id="22825-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTableau Server.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="22825-246">**tooassign Britta Simon tooTableau Server, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="22825-246">**tooassign Britta Simon tooTableau Server, perform hello following steps:**</span></span>

1. <span data-ttu-id="22825-247">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="22825-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="22825-249">Välj i listan med program hello **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="22825-249">In hello applications list, select **Tableau Server**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. <span data-ttu-id="22825-251">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="22825-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="22825-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="22825-253">Click **Add** button.</span></span> <span data-ttu-id="22825-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22825-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="22825-256">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="22825-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="22825-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22825-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="22825-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22825-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="22825-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22825-259">Testing single sign-on</span></span>

<span data-ttu-id="22825-260">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="22825-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="22825-261">När du klickar på panelen för hello Tableau Server i hello åtkomstpanelen får automatiskt inloggade tooyour Tableau serverprogram.</span><span class="sxs-lookup"><span data-stu-id="22825-261">When you click hello Tableau Server tile in hello Access Panel, you should get automatically signed-on tooyour Tableau Server application.</span></span>
<span data-ttu-id="22825-262">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="22825-262">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="22825-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="22825-263">Additional resources</span></span>

* [<span data-ttu-id="22825-264">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="22825-264">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="22825-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="22825-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png

