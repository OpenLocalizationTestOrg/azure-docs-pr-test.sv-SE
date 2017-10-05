---
title: "Självstudier: Azure Active Directory-integrering med Tableau Server | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Tableau Server."
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
ms.openlocfilehash: 6b35609d88fbbf649e15863901d521886db2a4d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a><span data-ttu-id="84c4b-103">Självstudier: Azure Active Directory-integrering med Tableau Server</span><span class="sxs-lookup"><span data-stu-id="84c4b-103">Tutorial: Azure Active Directory integration with Tableau Server</span></span>

<span data-ttu-id="84c4b-104">I kursen får lära du att integrera Tableau Server med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="84c4b-104">In this tutorial, you learn how to integrate Tableau Server with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="84c4b-105">Integrera Tableau Server med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="84c4b-105">Integrating Tableau Server with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="84c4b-106">Du kan styra i Azure AD som har åtkomst till Tableau Server</span><span class="sxs-lookup"><span data-stu-id="84c4b-106">You can control in Azure AD who has access to Tableau Server</span></span>
- <span data-ttu-id="84c4b-107">Du kan aktivera användarna att automatiskt hämta loggat in på Tableau Server (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="84c4b-107">You can enable your users to automatically get signed-on to Tableau Server (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="84c4b-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="84c4b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="84c4b-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="84c4b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84c4b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="84c4b-110">Prerequisites</span></span>

<span data-ttu-id="84c4b-111">Om du vill konfigurera Azure AD-integrering med Tableau Server behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="84c4b-111">To configure Azure AD integration with Tableau Server, you need the following items:</span></span>

- <span data-ttu-id="84c4b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="84c4b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="84c4b-113">En Tableau Server enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="84c4b-113">A Tableau Server single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="84c4b-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="84c4b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="84c4b-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="84c4b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="84c4b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="84c4b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="84c4b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="84c4b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="84c4b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="84c4b-118">Scenario description</span></span>
<span data-ttu-id="84c4b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="84c4b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="84c4b-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="84c4b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="84c4b-121">Att lägga till Tableau Server från galleriet</span><span class="sxs-lookup"><span data-stu-id="84c4b-121">Adding Tableau Server from the gallery</span></span>
2. <span data-ttu-id="84c4b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84c4b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-tableau-server-from-the-gallery"></a><span data-ttu-id="84c4b-123">Att lägga till Tableau Server från galleriet</span><span class="sxs-lookup"><span data-stu-id="84c4b-123">Adding Tableau Server from the gallery</span></span>
<span data-ttu-id="84c4b-124">Du måste lägga till Tableau Server från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Tableau Server i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="84c4b-124">To configure the integration of Tableau Server into Azure AD, you need to add Tableau Server from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="84c4b-125">**Om du vill lägga till Tableau Server från galleriet, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84c4b-125">**To add Tableau Server from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="84c4b-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84c4b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="84c4b-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="84c4b-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="84c4b-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84c4b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="84c4b-133">I sökrutan skriver **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-133">In the search box, type **Tableau Server**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. <span data-ttu-id="84c4b-135">Välj i resultatpanelen **Tableau Server**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="84c4b-135">In the results panel, select **Tableau Server**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="84c4b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84c4b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="84c4b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Tableau Server baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="84c4b-138">In this section, you configure and test Azure AD single sign-on with Tableau Server based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="84c4b-139">Azure AD måste du känna till motsvarande användaren i Tableau Server till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="84c4b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Tableau Server is to a user in Azure AD.</span></span> <span data-ttu-id="84c4b-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Tableau Server upprättas.</span><span class="sxs-lookup"><span data-stu-id="84c4b-140">In other words, a link relationship between an Azure AD user and the related user in Tableau Server needs to be established.</span></span>

<span data-ttu-id="84c4b-141">I Tableau Server, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="84c4b-141">In Tableau Server, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="84c4b-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Tableau Server, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="84c4b-142">To configure and test Azure AD single sign-on with Tableau Server, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="84c4b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="84c4b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="84c4b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84c4b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="84c4b-145">**[Skapa en testanvändare Tableau Server](#creating-a-tableau-server-test-user)**  – du har en motsvarighet för Britta Simon i Tableau-Server som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="84c4b-145">**[Creating a Tableau Server test user](#creating-a-tableau-server-test-user)** - to have a counterpart of Britta Simon in Tableau Server that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="84c4b-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84c4b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="84c4b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="84c4b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="84c4b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84c4b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="84c4b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i serverprogrammet Tableau.</span><span class="sxs-lookup"><span data-stu-id="84c4b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tableau Server application.</span></span>

<span data-ttu-id="84c4b-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Tableau Server:**</span><span class="sxs-lookup"><span data-stu-id="84c4b-150">**To configure Azure AD single sign-on with Tableau Server, perform the following steps:**</span></span>

1. <span data-ttu-id="84c4b-151">I Azure-portalen på den **Tableau Server** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-151">In the Azure portal, on the **Tableau Server** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="84c4b-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84c4b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. <span data-ttu-id="84c4b-155">På den **URL: er och Tableau serverdomänen** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="84c4b-155">On the **Tableau Server Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    <span data-ttu-id="84c4b-157">a.</span><span class="sxs-lookup"><span data-stu-id="84c4b-157">a.</span></span> <span data-ttu-id="84c4b-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="84c4b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://azure.<domain name>.link`</span></span>
    
    <span data-ttu-id="84c4b-159">b.</span><span class="sxs-lookup"><span data-stu-id="84c4b-159">b.</span></span> <span data-ttu-id="84c4b-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://azure.<domain name>.link`</span><span class="sxs-lookup"><span data-stu-id="84c4b-160">In the **Identifier** textbox, type a URL using the following pattern: `https://azure.<domain name>.link`</span></span>

    <span data-ttu-id="84c4b-161">c.</span><span class="sxs-lookup"><span data-stu-id="84c4b-161">c.</span></span> <span data-ttu-id="84c4b-162">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://azure.<domain name>.link/wg/saml/SSO/index.html`</span><span class="sxs-lookup"><span data-stu-id="84c4b-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://azure.<domain name>.link/wg/saml/SSO/index.html`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="84c4b-163">Föregående värden är inte verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="84c4b-163">The preceding values are not real values.</span></span> <span data-ttu-id="84c4b-164">Senare kan uppdatera du värdena med faktiska URL och identifierare från konfigurationssidan Tableau Server.</span><span class="sxs-lookup"><span data-stu-id="84c4b-164">Later, you update the values with the actual URL and identifier from the Tableau Server configuration page.</span></span> 

4. <span data-ttu-id="84c4b-165">Tableau serverprogrammet förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="84c4b-165">Tableau Server application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="84c4b-166">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="84c4b-166">Configure the following claims for this application.</span></span> <span data-ttu-id="84c4b-167">Du kan hantera värden för attributen från den **”användarattribut”** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="84c4b-167">You can manage the values of these attributes from the **"User Attributes"** section on application integration page.</span></span> <span data-ttu-id="84c4b-168">Följande skärmbild visar ett exempel för samma.</span><span class="sxs-lookup"><span data-stu-id="84c4b-168">The following screenshot shows an example for the same.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. <span data-ttu-id="84c4b-170">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="84c4b-170">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="84c4b-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="84c4b-171">Attribute Name</span></span> | <span data-ttu-id="84c4b-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="84c4b-172">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="84c4b-173">användarnamn</span><span class="sxs-lookup"><span data-stu-id="84c4b-173">username</span></span> | <span data-ttu-id="84c4b-174">*User.DisplayName*</span><span class="sxs-lookup"><span data-stu-id="84c4b-174">*user.displayname*</span></span> |

    <span data-ttu-id="84c4b-175">a.</span><span class="sxs-lookup"><span data-stu-id="84c4b-175">a.</span></span> <span data-ttu-id="84c4b-176">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84c4b-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    <span data-ttu-id="84c4b-179">b.</span><span class="sxs-lookup"><span data-stu-id="84c4b-179">b.</span></span> <span data-ttu-id="84c4b-180">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="84c4b-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="84c4b-181">c.</span><span class="sxs-lookup"><span data-stu-id="84c4b-181">c.</span></span> <span data-ttu-id="84c4b-182">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="84c4b-182">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="84c4b-183">d.</span><span class="sxs-lookup"><span data-stu-id="84c4b-183">d.</span></span> <span data-ttu-id="84c4b-184">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="84c4b-184">Click **Ok**</span></span>


6. <span data-ttu-id="84c4b-185">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="84c4b-185">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. <span data-ttu-id="84c4b-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="84c4b-187">Click **Save** button.</span></span>

    <span data-ttu-id="84c4b-188">![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="84c4b-188">![Configure Single Sign-On](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS></span></span>
8. <span data-ttu-id="84c4b-189">För att få SSO konfigurerats för ditt program måste logga in till din Tableau Server-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="84c4b-189">To get SSO configured for your application, you need to sign-on to your Tableau Server tenant as an administrator.</span></span>
   
   <span data-ttu-id="84c4b-190">a.</span><span class="sxs-lookup"><span data-stu-id="84c4b-190">a.</span></span> <span data-ttu-id="84c4b-191">I Tableau Server-konfiguration klickar du på den **SAML** fliken.</span><span class="sxs-lookup"><span data-stu-id="84c4b-191">In the Tableau Server configuration, click the **SAML** tab.</span></span>
  
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   <span data-ttu-id="84c4b-193">b.</span><span class="sxs-lookup"><span data-stu-id="84c4b-193">b.</span></span> <span data-ttu-id="84c4b-194">Markera kryssrutan för **Använd SAML för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-194">Select the checkbox of **Use SAML for single sign-on**.</span></span>
   
   <span data-ttu-id="84c4b-195">c.</span><span class="sxs-lookup"><span data-stu-id="84c4b-195">c.</span></span> <span data-ttu-id="84c4b-196">Tableau Server Retur-URL – URL: en som Tableau Server-användare kommer åt, till exempel http://tableau_server.</span><span class="sxs-lookup"><span data-stu-id="84c4b-196">Tableau Server return URL—The URL that Tableau Server users will be accessing, such as http://tableau_server.</span></span> <span data-ttu-id="84c4b-197">Du bör inte använda http://localhost.</span><span class="sxs-lookup"><span data-stu-id="84c4b-197">Using http://localhost is not recommended.</span></span> <span data-ttu-id="84c4b-198">Med en URL som avslutande snedstreck (till exempel http://tableau_server/) stöds inte.</span><span class="sxs-lookup"><span data-stu-id="84c4b-198">Using a URL with a trailing slash (for example, http://tableau_server/) is not supported.</span></span> <span data-ttu-id="84c4b-199">Kopiera **Tableau Server Retur-URL** och klistra in den till Azure AD **logga URL** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="84c4b-199">Copy **Tableau Server return URL** and paste it to Azure AD **Sign On URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="84c4b-200">d.</span><span class="sxs-lookup"><span data-stu-id="84c4b-200">d.</span></span> <span data-ttu-id="84c4b-201">SAML enhets-ID, enhets-ID som unikt identifierar din Tableau serverinstallation till IdP.</span><span class="sxs-lookup"><span data-stu-id="84c4b-201">SAML entity ID—The entity ID uniquely identifies your Tableau Server installation to the IdP.</span></span> <span data-ttu-id="84c4b-202">Du kan ange Tableau Server URL: en igen här, om du vill, men behöver inte vara Tableau-Serveradress.</span><span class="sxs-lookup"><span data-stu-id="84c4b-202">You can enter your Tableau Server URL again here, if you like, but it does not have to be your Tableau Server URL.</span></span> <span data-ttu-id="84c4b-203">Kopiera **SAML enhets-ID** och klistra in den till Azure AD **identifierare** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="84c4b-203">Copy **SAML entity ID** and paste it to Azure AD **Identifier** textbox in **Tableau Server Domain and URLs** section.</span></span>
     
   <span data-ttu-id="84c4b-204">e.</span><span class="sxs-lookup"><span data-stu-id="84c4b-204">e.</span></span> <span data-ttu-id="84c4b-205">Klicka på den **exportera metadatafil** och öppna den i redigeringsprogrammet text.</span><span class="sxs-lookup"><span data-stu-id="84c4b-205">Click the **Export Metadata File** and open it in the text editor application.</span></span> <span data-ttu-id="84c4b-206">Leta upp Assertion konsumenten tjänst-URL med Http Post och Index 0 och kopiera URL-Adressen.</span><span class="sxs-lookup"><span data-stu-id="84c4b-206">Locate Assertion Consumer Service URL with Http Post and Index 0 and copy the URL.</span></span> <span data-ttu-id="84c4b-207">Klistra in den till Azure AD **Reply URL** TextBox-kontroll i **URL: er och Tableau serverdomänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="84c4b-207">Now paste it to Azure AD **Reply URL** textbox in **Tableau Server Domain and URLs** section.</span></span>
   
   <span data-ttu-id="84c4b-208">f.</span><span class="sxs-lookup"><span data-stu-id="84c4b-208">f.</span></span> <span data-ttu-id="84c4b-209">Leta upp din Federationsmetadata-fil som hämtas från Azure-portalen och sedan ladda upp den i den **SAML Idp metadatafil**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-209">Locate your Federation Metadata file downloaded from Azure portal, and then upload it in the **SAML Idp metadata file**.</span></span>
   
   <span data-ttu-id="84c4b-210">g.</span><span class="sxs-lookup"><span data-stu-id="84c4b-210">g.</span></span> <span data-ttu-id="84c4b-211">Klicka på den **OK** knappen på sidan Tableau serverkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="84c4b-211">Click the **OK** button in the Tableau Server Configuration page.</span></span>
   
    >[!NOTE] 
    ><span data-ttu-id="84c4b-212">Kunden måste överföra alla certifikat i Tableau Server SAML SSO-konfigurationen och kommer hämta ignoreras i flödet för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="84c4b-212">Customer have to upload any certificate in the Tableau Server SAML SSO configuration and it will get ignored in the SSO flow.</span></span>
    ><span data-ttu-id="84c4b-213">Om du behöver hjälp konfigurerar SAML på Tableau Server finns i den här artikeln [konfigurera SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span><span class="sxs-lookup"><span data-stu-id="84c4b-213">If you need help configuring SAML on Tableau Server then please refer to this article [Configure SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).</span></span>
    >
<CE>

> [!TIP]
> <span data-ttu-id="84c4b-214">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="84c4b-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="84c4b-215">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="84c4b-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="84c4b-216">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="84c4b-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="84c4b-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="84c4b-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="84c4b-218">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="84c4b-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="84c4b-220">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="84c4b-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="84c4b-221">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="84c4b-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="84c4b-223">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="84c4b-225">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84c4b-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="84c4b-227">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="84c4b-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="84c4b-229">a.</span><span class="sxs-lookup"><span data-stu-id="84c4b-229">a.</span></span> <span data-ttu-id="84c4b-230">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="84c4b-231">b.</span><span class="sxs-lookup"><span data-stu-id="84c4b-231">b.</span></span> <span data-ttu-id="84c4b-232">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="84c4b-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="84c4b-233">c.</span><span class="sxs-lookup"><span data-stu-id="84c4b-233">c.</span></span> <span data-ttu-id="84c4b-234">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="84c4b-235">d.</span><span class="sxs-lookup"><span data-stu-id="84c4b-235">d.</span></span> <span data-ttu-id="84c4b-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-236">Click **Create**.</span></span>
 
### <a name="creating-a-tableau-server-test-user"></a><span data-ttu-id="84c4b-237">Skapa en testanvändare Tableau Server</span><span class="sxs-lookup"><span data-stu-id="84c4b-237">Creating a Tableau Server test user</span></span>

<span data-ttu-id="84c4b-238">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Tableau Server.</span><span class="sxs-lookup"><span data-stu-id="84c4b-238">The objective of this section is to create a user called Britta Simon in Tableau Server.</span></span> <span data-ttu-id="84c4b-239">Du måste etablera alla användare i Tableau-servern.</span><span class="sxs-lookup"><span data-stu-id="84c4b-239">You need to provision all the users in the Tableau server.</span></span> 

<span data-ttu-id="84c4b-240">Att användarnamnet för användaren som ska matcha värdet som du har konfigurerat i Azure AD-attributet för **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-240">That username of the user should match the value which you have configured in the Azure AD custom attribute of **username**.</span></span> <span data-ttu-id="84c4b-241">Integrationen ska fungera med korrekt mappning [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="84c4b-241">With the correct mapping the integration should work [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

>[!NOTE]
><span data-ttu-id="84c4b-242">Om du behöver skapa en användare manuellt, måste du kontakta serveradministratören Tableau i din organisation.</span><span class="sxs-lookup"><span data-stu-id="84c4b-242">If you need to create a user manually, you need to contact the Tableau Server administrator in your organization.</span></span>
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="84c4b-243">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="84c4b-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="84c4b-244">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Tableau Server.</span><span class="sxs-lookup"><span data-stu-id="84c4b-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tableau Server.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="84c4b-246">**Om du vill tilldela Britta Simon Tableau Server, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="84c4b-246">**To assign Britta Simon to Tableau Server, perform the following steps:**</span></span>

1. <span data-ttu-id="84c4b-247">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="84c4b-249">Välj i listan med program **Tableau Server**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-249">In the applications list, select **Tableau Server**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. <span data-ttu-id="84c4b-251">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="84c4b-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="84c4b-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="84c4b-253">Click **Add** button.</span></span> <span data-ttu-id="84c4b-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84c4b-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="84c4b-256">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="84c4b-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="84c4b-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84c4b-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="84c4b-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="84c4b-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="84c4b-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="84c4b-259">Testing single sign-on</span></span>

<span data-ttu-id="84c4b-260">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="84c4b-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="84c4b-261">När du klickar på panelen Tableau Server på åtkomstpanelen du bör få automatiskt loggat in på serverprogrammet Tableau.</span><span class="sxs-lookup"><span data-stu-id="84c4b-261">When you click the Tableau Server tile in the Access Panel, you should get automatically signed-on to your Tableau Server application.</span></span>
<span data-ttu-id="84c4b-262">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="84c4b-262">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="84c4b-263">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="84c4b-263">Additional resources</span></span>

* [<span data-ttu-id="84c4b-264">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="84c4b-264">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="84c4b-265">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="84c4b-265">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

