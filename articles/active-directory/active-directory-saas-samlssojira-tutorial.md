---
title: "Självstudier: Azure Active Directory-integrering med SAML SSO för Jira resolution GmbH | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SAML SSO för Jira resolution GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: cde5983710185d1e46a5601b16bbfb1c0fcae382
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a><span data-ttu-id="cbe08-103">Självstudier: Azure Active Directory-integrering med SAML SSO för Jira resolution GmbH</span><span class="sxs-lookup"><span data-stu-id="cbe08-103">Tutorial: Azure Active Directory integration with SAML SSO for Jira by resolution GmbH</span></span>

<span data-ttu-id="cbe08-104">I kursen får lära du att integrera SAML SSO för Jira resolution GmbH med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cbe08-104">In this tutorial, you learn how to integrate SAML SSO for Jira by resolution GmbH with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cbe08-105">Integrera SAML SSO för Jira resolution GmbH med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cbe08-105">Integrating SAML SSO for Jira by resolution GmbH with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cbe08-106">Du kan styra i Azure AD som har åtkomst till SAML SSO för Jira resolution GmbH</span><span class="sxs-lookup"><span data-stu-id="cbe08-106">You can control in Azure AD who has access to SAML SSO for Jira by resolution GmbH</span></span>
- <span data-ttu-id="cbe08-107">Du kan aktivera användarna att automatiskt hämta loggat in på SAML SSO för Jira resolution GmbH (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cbe08-107">You can enable your users to automatically get signed-on to SAML SSO for Jira by resolution GmbH (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cbe08-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cbe08-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cbe08-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cbe08-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbe08-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cbe08-110">Prerequisites</span></span>

<span data-ttu-id="cbe08-111">För att konfigurera Azure AD-integrering med SAML SSO för Jira resolution GmbH, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cbe08-111">To configure Azure AD integration with SAML SSO for Jira by resolution GmbH, you need the following items:</span></span>

- <span data-ttu-id="cbe08-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cbe08-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cbe08-113">En SAML SSO för Jira av upplösning GmbH enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="cbe08-113">A SAML SSO for Jira by resolution GmbH single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cbe08-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cbe08-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cbe08-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cbe08-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cbe08-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cbe08-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cbe08-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cbe08-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cbe08-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cbe08-118">Scenario description</span></span>
<span data-ttu-id="cbe08-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cbe08-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cbe08-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cbe08-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cbe08-121">Att lägga till SAML SSO för Jira resolution GmbH från galleriet</span><span class="sxs-lookup"><span data-stu-id="cbe08-121">Adding SAML SSO for Jira by resolution GmbH from the gallery</span></span>
2. <span data-ttu-id="cbe08-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cbe08-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-the-gallery"></a><span data-ttu-id="cbe08-123">Att lägga till SAML SSO för Jira resolution GmbH från galleriet</span><span class="sxs-lookup"><span data-stu-id="cbe08-123">Adding SAML SSO for Jira by resolution GmbH from the gallery</span></span>
<span data-ttu-id="cbe08-124">Du måste lägga till SAML SSO för Jira resolution GmbH från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SAML SSO för Jira resolution GmbH i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cbe08-124">To configure the integration of SAML SSO for Jira by resolution GmbH into Azure AD, you need to add SAML SSO for Jira by resolution GmbH from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cbe08-125">**Utför följande steg för att lägga till SAML SSO för Jira resolution GmbH från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cbe08-125">**To add SAML SSO for Jira by resolution GmbH from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cbe08-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cbe08-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cbe08-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cbe08-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cbe08-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cbe08-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cbe08-133">I sökrutan skriver **SAML SSO för Jira resolution GmbH**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-133">In the search box, type **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_search.png)

5. <span data-ttu-id="cbe08-135">Välj i resultatpanelen **SAML SSO för Jira resolution GmbH**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cbe08-135">In the results panel, select **SAML SSO for Jira by resolution GmbH**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cbe08-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cbe08-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cbe08-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAML SSO för Jira resolution GmbH baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cbe08-138">In this section, you configure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cbe08-139">För enkel inloggning ska fungera, Azure AD som behöver veta vilka motsvarande användaren i SAML SSO för Jira resolution GmbH är att en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cbe08-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAML SSO for Jira by resolution GmbH is to a user in Azure AD.</span></span> <span data-ttu-id="cbe08-140">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SAML SSO för Jira resolution GmbH måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="cbe08-140">In other words, a link relationship between an Azure AD user and the related user in SAML SSO for Jira by resolution GmbH needs to be established.</span></span>

<span data-ttu-id="cbe08-141">SAML SSO för Jira resolution GmbH, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-141">In SAML SSO for Jira by resolution GmbH, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cbe08-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SAML SSO för Jira resolution GmbH, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cbe08-142">To configure and test Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cbe08-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cbe08-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cbe08-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cbe08-145">**[Skapa en SAML SSO för Jira av upplösning GmbH testanvändare](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  – du har en motsvarighet för Britta Simon SAML SSO för Jira resolution GmbH som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cbe08-145">**[Creating a SAML SSO for Jira by resolution GmbH test user](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)** - to have a counterpart of Britta Simon in SAML SSO for Jira by resolution GmbH that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cbe08-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cbe08-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cbe08-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cbe08-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cbe08-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cbe08-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cbe08-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din SAML SSO för Jira resolution GmbH program.</span><span class="sxs-lookup"><span data-stu-id="cbe08-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAML SSO for Jira by resolution GmbH application.</span></span>

<span data-ttu-id="cbe08-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SAML SSO för Jira resolution GmbH:**</span><span class="sxs-lookup"><span data-stu-id="cbe08-150">**To configure Azure AD single sign-on with SAML SSO for Jira by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="cbe08-151">I Azure-portalen på den **SAML SSO för Jira resolution GmbH** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-151">In the Azure portal, on the **SAML SSO for Jira by resolution GmbH** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cbe08-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cbe08-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

3. <span data-ttu-id="cbe08-155">På den **SAML SSO Jira resolution GmbH domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="cbe08-155">On the **SAML SSO for Jira by resolution GmbH Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    <span data-ttu-id="cbe08-157">a.</span><span class="sxs-lookup"><span data-stu-id="cbe08-157">a.</span></span> <span data-ttu-id="cbe08-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="cbe08-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

    <span data-ttu-id="cbe08-159">b.</span><span class="sxs-lookup"><span data-stu-id="cbe08-159">b.</span></span> <span data-ttu-id="cbe08-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="cbe08-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>

4. <span data-ttu-id="cbe08-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="cbe08-162">Om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="cbe08-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    <span data-ttu-id="cbe08-164">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<server-base-url>/plugins/servlet/samlsso`</span><span class="sxs-lookup"><span data-stu-id="cbe08-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server-base-url>/plugins/servlet/samlsso`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="cbe08-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cbe08-165">These values are not real.</span></span> <span data-ttu-id="cbe08-166">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="cbe08-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="cbe08-167">Kontakta [SAML SSO för Jira resolution GmbH klienten supportteam](https://www.resolution.de/go/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cbe08-167">Contact [SAML SSO for Jira by resolution GmbH Client support team](https://www.resolution.de/go/support) to get these values.</span></span> 

5. <span data-ttu-id="cbe08-168">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cbe08-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

6. <span data-ttu-id="cbe08-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="cbe08-172">I en annan webbläsarfönstret, logga in på ditt **SAML SSO för Jira upplösning GmbH admin Portal** som administratör.</span><span class="sxs-lookup"><span data-stu-id="cbe08-172">In a different web browser window, log in to your **SAML SSO for Jira by resolution GmbH admin portal** as an administrator.</span></span>

8. <span data-ttu-id="cbe08-173">Hovra över kugge och klicka på den **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-173">Hover on cog and click the **Add-ons**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon1.png)

9. <span data-ttu-id="cbe08-175">Du omdirigeras till sidan med administratörsåtkomst.</span><span class="sxs-lookup"><span data-stu-id="cbe08-175">You are redirected to Administrator Access page.</span></span> <span data-ttu-id="cbe08-176">Ange den **lösenord** och på **Bekräfta** knappen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-176">Enter the **Password** and click **Confirm** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon2.png)

10. <span data-ttu-id="cbe08-178">Klicka under tillägg fliken avsnitt **söka efter nya tillägg**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-178">Under Add-ons tab section, click **Find new add-ons**.</span></span> <span data-ttu-id="cbe08-179">Sök **SAML enkel inloggning (SSO) för JIRA** och på **installera** för att installera den nya SAML-plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="cbe08-179">Search **SAML Single Sign On (SSO) for JIRA** and click **Install** button to install the new SAML plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon7.png)

11. <span data-ttu-id="cbe08-181">Plugin-installationen startar.</span><span class="sxs-lookup"><span data-stu-id="cbe08-181">The plugin installation will start.</span></span> <span data-ttu-id="cbe08-182">Klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-182">Click **Close**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon8.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon9.png)

12. <span data-ttu-id="cbe08-185">Klicka på **Hantera**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-185">Click **Manage**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon10.png)
    
13. <span data-ttu-id="cbe08-187">Klicka på **konfigurera** att konfigurera nya plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="cbe08-187">Click **Configure** to configure the new plugin.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon11.png)

14. <span data-ttu-id="cbe08-189">På **SAML SingleSignOn Plugin Configuration** klickar du på **lägga till ytterligare identitetsleverantör** för att konfigurera inställningarna för identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="cbe08-189">On **SAML SingleSignOn Plugin Configuration** page, click **Add additional Identity Provider** button to configure the settings of Identity Provider.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon4.png)

15. <span data-ttu-id="cbe08-191">Utför följande steg på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="cbe08-191">Perform following steps on this page:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon5.png)
 
    <span data-ttu-id="cbe08-193">a.</span><span class="sxs-lookup"><span data-stu-id="cbe08-193">a.</span></span> <span data-ttu-id="cbe08-194">Lägg till **namn** av identitetsleverantören (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cbe08-194">Add **Name** of the Identity Provider (e.g Azure AD).</span></span>
    
    <span data-ttu-id="cbe08-195">b.</span><span class="sxs-lookup"><span data-stu-id="cbe08-195">b.</span></span> <span data-ttu-id="cbe08-196">Lägg till **beskrivning** av identitetsleverantören (t.ex Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cbe08-196">Add **Description** of the Identity Provider (e.g Azure AD).</span></span>

    <span data-ttu-id="cbe08-197">c.</span><span class="sxs-lookup"><span data-stu-id="cbe08-197">c.</span></span> <span data-ttu-id="cbe08-198">Klicka på **XML** och välj den **Metadata** fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-198">Click **XML** and select the **Metadata** file, which you have downloaded from Azure portal.</span></span>

    <span data-ttu-id="cbe08-199">d.</span><span class="sxs-lookup"><span data-stu-id="cbe08-199">d.</span></span> <span data-ttu-id="cbe08-200">Klicka på **belastningen** knappen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-200">Click **Load** button.</span></span>

    <span data-ttu-id="cbe08-201">e.</span><span class="sxs-lookup"><span data-stu-id="cbe08-201">e.</span></span> <span data-ttu-id="cbe08-202">Den läser IdP-metadata och fylls fälten som är markerade i skärmbilden.</span><span class="sxs-lookup"><span data-stu-id="cbe08-202">It reads the IdP metadata and populates the fields as highlighted in the screenshot.</span></span> 

16. <span data-ttu-id="cbe08-203">Klicka på **Spara inställningar** för att spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="cbe08-203">Click **Save settings** button to save the settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/addon6.png)

> [!TIP]
> <span data-ttu-id="cbe08-205">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cbe08-205">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cbe08-206">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cbe08-206">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cbe08-207">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cbe08-207">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cbe08-208">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cbe08-208">Creating an Azure AD test user</span></span>
<span data-ttu-id="cbe08-209">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cbe08-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cbe08-211">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cbe08-211">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cbe08-212">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cbe08-212">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cbe08-214">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-214">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cbe08-216">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cbe08-216">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cbe08-218">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cbe08-218">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-samlssojira-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cbe08-220">a.</span><span class="sxs-lookup"><span data-stu-id="cbe08-220">a.</span></span> <span data-ttu-id="cbe08-221">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-221">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cbe08-222">b.</span><span class="sxs-lookup"><span data-stu-id="cbe08-222">b.</span></span> <span data-ttu-id="cbe08-223">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cbe08-223">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cbe08-224">c.</span><span class="sxs-lookup"><span data-stu-id="cbe08-224">c.</span></span> <span data-ttu-id="cbe08-225">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-225">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cbe08-226">d.</span><span class="sxs-lookup"><span data-stu-id="cbe08-226">d.</span></span> <span data-ttu-id="cbe08-227">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-227">Click **Create**.</span></span>
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a><span data-ttu-id="cbe08-228">Skapa en SAML SSO för Jira av upplösning GmbH testanvändare</span><span class="sxs-lookup"><span data-stu-id="cbe08-228">Creating a SAML SSO for Jira by resolution GmbH test user</span></span>

<span data-ttu-id="cbe08-229">Om du vill aktivera Azure AD-användare kan logga in på SAML SSO för Jira resolution GmbH etableras de i SAML SSO för Jira resolution GmbH.</span><span class="sxs-lookup"><span data-stu-id="cbe08-229">To enable Azure AD users to log in to SAML SSO for Jira by resolution GmbH, they must be provisioned into SAML SSO for Jira by resolution GmbH.</span></span>  
<span data-ttu-id="cbe08-230">SAML SSO för Jira resolution GmbH är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="cbe08-230">In SAML SSO for Jira by resolution GmbH, provisioning is a manual task.</span></span>

<span data-ttu-id="cbe08-231">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="cbe08-231">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="cbe08-232">Logga in på ditt SAML SSO för Jira av upplösning GmbH företagets plats som administratör.</span><span class="sxs-lookup"><span data-stu-id="cbe08-232">Log in to your SAML SSO for Jira by resolution GmbH company site as an administrator.</span></span>

2. <span data-ttu-id="cbe08-233">Hovra över kugge och klicka på den **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-233">Hover on cog and click the **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user1.png) 

3. <span data-ttu-id="cbe08-235">Du omdirigeras till administratörsåtkomst sidan om du vill ange **lösenord** och på **Bekräfta** knappen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-235">You are redirected to Administrator Access page to enter **Password** and click **Confirm** button.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user2.png) 

4. <span data-ttu-id="cbe08-237">Under **Användarhantering** avsnittet klickar du på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-237">Under **User management** tab section, click **create user**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user3.png) 

5. <span data-ttu-id="cbe08-239">På den **”skapa nya användare”** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cbe08-239">On the **“Create new user”** dialog page, perform the following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-samlssojira-tutorial/user4.png) 

    <span data-ttu-id="cbe08-241">a.</span><span class="sxs-lookup"><span data-stu-id="cbe08-241">a.</span></span> <span data-ttu-id="cbe08-242">I den **e-postadress** textruta typen e-postadressen för användaren som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="cbe08-242">In the **Email address** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="cbe08-243">b.</span><span class="sxs-lookup"><span data-stu-id="cbe08-243">b.</span></span> <span data-ttu-id="cbe08-244">I den **fullständiga namn** textruta fullständiga typnamnet för användaren som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cbe08-244">In the **Full Name** textbox, type full name of the user like Britta Simon.</span></span>

    <span data-ttu-id="cbe08-245">c.</span><span class="sxs-lookup"><span data-stu-id="cbe08-245">c.</span></span> <span data-ttu-id="cbe08-246">I den **användarnamn** textruta, ange den e-posten för användare som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="cbe08-246">In the **Username** textbox, type the email of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="cbe08-247">d.</span><span class="sxs-lookup"><span data-stu-id="cbe08-247">d.</span></span> <span data-ttu-id="cbe08-248">I den **lösenord** textruta skriver du lösenordet för användaren.</span><span class="sxs-lookup"><span data-stu-id="cbe08-248">In the **Password** textbox, type the password of user.</span></span>

    <span data-ttu-id="cbe08-249">e.</span><span class="sxs-lookup"><span data-stu-id="cbe08-249">e.</span></span> <span data-ttu-id="cbe08-250">Klicka på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-250">Click **Create user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cbe08-251">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cbe08-251">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cbe08-252">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SAML SSO för Jira resolution GmbH.</span><span class="sxs-lookup"><span data-stu-id="cbe08-252">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAML SSO for Jira by resolution GmbH.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cbe08-254">**Om du vill tilldela SAML SSO för Jira Britta Simon resolution GmbH, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cbe08-254">**To assign Britta Simon to SAML SSO for Jira by resolution GmbH, perform the following steps:**</span></span>

1. <span data-ttu-id="cbe08-255">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-255">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cbe08-257">Välj i listan med program **SAML SSO för Jira resolution GmbH**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-257">In the applications list, select **SAML SSO for Jira by resolution GmbH**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-samlssojira-tutorial/tutorial_samlssojira_app.png) 

3. <span data-ttu-id="cbe08-259">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cbe08-259">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cbe08-261">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cbe08-261">Click **Add** button.</span></span> <span data-ttu-id="cbe08-262">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cbe08-262">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cbe08-264">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cbe08-264">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cbe08-265">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cbe08-265">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cbe08-266">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cbe08-266">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cbe08-267">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cbe08-267">Testing single sign-on</span></span>

<span data-ttu-id="cbe08-268">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cbe08-268">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cbe08-269">När du klickar på SAML SSO för Jira av upplösning GmbH panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt SAML SSO för Jira resolution GmbH program.</span><span class="sxs-lookup"><span data-stu-id="cbe08-269">When you click the SAML SSO for Jira by resolution GmbH tile in the Access Panel, you should get automatically signed-on to your SAML SSO for Jira by resolution GmbH application.</span></span>
<span data-ttu-id="cbe08-270">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cbe08-270">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cbe08-271">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cbe08-271">Additional resources</span></span>

* [<span data-ttu-id="cbe08-272">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cbe08-272">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cbe08-273">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cbe08-273">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-samlssojira-tutorial/tutorial_general_203.png

