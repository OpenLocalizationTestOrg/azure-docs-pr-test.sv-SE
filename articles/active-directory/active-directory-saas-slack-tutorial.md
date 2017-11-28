---
title: "Självstudier: Azure Active Directory-integrering med Slack | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Slack."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 5aca630b2077d3f7d4ce9273ee668ed6a5f9843d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a><span data-ttu-id="449dd-103">Självstudier: Azure Active Directory-integrering med Slack</span><span class="sxs-lookup"><span data-stu-id="449dd-103">Tutorial: Azure Active Directory integration with Slack</span></span>

<span data-ttu-id="449dd-104">I kursen får lära du att integrera Slack med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="449dd-104">In this tutorial, you learn how to integrate Slack with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="449dd-105">Integrera Slack med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="449dd-105">Integrating Slack with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="449dd-106">Du kan styra i Azure AD som har åtkomst till Slack</span><span class="sxs-lookup"><span data-stu-id="449dd-106">You can control in Azure AD who has access to Slack</span></span>
- <span data-ttu-id="449dd-107">Du kan aktivera användarna att automatiskt hämta loggat in på Slack (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="449dd-107">You can enable your users to automatically get signed-on to Slack (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="449dd-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="449dd-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="449dd-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="449dd-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="449dd-110">Krav</span><span class="sxs-lookup"><span data-stu-id="449dd-110">Prerequisites</span></span>

<span data-ttu-id="449dd-111">För att konfigurera Azure AD-integrering med Slack, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="449dd-111">To configure Azure AD integration with Slack, you need the following items:</span></span>

- <span data-ttu-id="449dd-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="449dd-112">An Azure AD subscription</span></span>
- <span data-ttu-id="449dd-113">En Slack enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="449dd-113">A Slack single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="449dd-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="449dd-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="449dd-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="449dd-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="449dd-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="449dd-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="449dd-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="449dd-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="449dd-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="449dd-118">Scenario description</span></span>
<span data-ttu-id="449dd-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="449dd-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="449dd-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="449dd-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="449dd-121">Att lägga till Slack från galleriet</span><span class="sxs-lookup"><span data-stu-id="449dd-121">Adding Slack from the gallery</span></span>
2. <span data-ttu-id="449dd-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="449dd-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-slack-from-the-gallery"></a><span data-ttu-id="449dd-123">Att lägga till Slack från galleriet</span><span class="sxs-lookup"><span data-stu-id="449dd-123">Adding Slack from the gallery</span></span>
<span data-ttu-id="449dd-124">Du måste lägga till Slack från galleriet i listan över hanterade SaaS-appar för att konfigurera Slack till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="449dd-124">To configure the integration of Slack into Azure AD, you need to add Slack from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="449dd-125">**Utför följande steg för att lägga till Slack från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="449dd-125">**To add Slack from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="449dd-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="449dd-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="449dd-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="449dd-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="449dd-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="449dd-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="449dd-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="449dd-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="449dd-133">I sökrutan skriver **Slack**.</span><span class="sxs-lookup"><span data-stu-id="449dd-133">In the search box, type **Slack**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. <span data-ttu-id="449dd-135">Välj i resultatpanelen **Slack**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="449dd-135">In the results panel, select **Slack**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="449dd-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="449dd-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="449dd-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Slack baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="449dd-138">In this section, you configure and test Azure AD single sign-on with Slack based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="449dd-139">Azure AD måste du känna till användaren i Slack motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="449dd-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Slack is to a user in Azure AD.</span></span> <span data-ttu-id="449dd-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Slack upprättas.</span><span class="sxs-lookup"><span data-stu-id="449dd-140">In other words, a link relationship between an Azure AD user and the related user in Slack needs to be established.</span></span>

<span data-ttu-id="449dd-141">I Slack, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="449dd-141">In Slack, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="449dd-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Slack, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="449dd-142">To configure and test Azure AD single sign-on with Slack, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="449dd-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="449dd-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="449dd-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="449dd-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="449dd-145">**[Skapa en Slack testanvändare](#creating-a-slack-test-user)**  – har en motsvarighet för Britta Simon Slack som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="449dd-145">**[Creating a Slack test user](#creating-a-slack-test-user)** - to have a counterpart of Britta Simon in Slack that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="449dd-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="449dd-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="449dd-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="449dd-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="449dd-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="449dd-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="449dd-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Slack.</span><span class="sxs-lookup"><span data-stu-id="449dd-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Slack application.</span></span>

<span data-ttu-id="449dd-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Slack:**</span><span class="sxs-lookup"><span data-stu-id="449dd-150">**To configure Azure AD single sign-on with Slack, perform the following steps:**</span></span>

1. <span data-ttu-id="449dd-151">I Azure-portalen på den **Slack** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="449dd-151">In the Azure portal, on the **Slack** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="449dd-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="449dd-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. <span data-ttu-id="449dd-155">På den **Slack domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="449dd-155">On the **Slack Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    <span data-ttu-id="449dd-157">a.</span><span class="sxs-lookup"><span data-stu-id="449dd-157">a.</span></span> <span data-ttu-id="449dd-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.slack.com`</span><span class="sxs-lookup"><span data-stu-id="449dd-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.slack.com`</span></span>

    <span data-ttu-id="449dd-159">b.</span><span class="sxs-lookup"><span data-stu-id="449dd-159">b.</span></span> <span data-ttu-id="449dd-160">I den **identifierare** textruta anger du URL:`https://slack.com`</span><span class="sxs-lookup"><span data-stu-id="449dd-160">In the **Identifier** textbox, type the URL: `https://slack.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="449dd-161">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="449dd-161">The value is not real.</span></span> <span data-ttu-id="449dd-162">Du måste uppdatera värdet med faktiska logga på URL: en.</span><span class="sxs-lookup"><span data-stu-id="449dd-162">You have to update the value with the actual Sign On URL.</span></span> <span data-ttu-id="449dd-163">Kontakta [Slack supportteamet](https://slack.com/help/contact) värdet hämtas</span><span class="sxs-lookup"><span data-stu-id="449dd-163">Contact [Slack support team](https://slack.com/help/contact) to get the value</span></span>
     
4. <span data-ttu-id="449dd-164">Slack program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="449dd-164">Slack application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="449dd-165">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="449dd-165">Configure the following claims for this application.</span></span> <span data-ttu-id="449dd-166">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="449dd-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="449dd-167">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="449dd-167">The following screenshot shows an example for this.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. <span data-ttu-id="449dd-169">I den **användarattribut** avsnitt på den **enkel inloggning** markerar **user.mail** som **användar-ID** och för varje rad som visas i i tabellen nedan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="449dd-169">In the **User Attributes** section on the **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in the table below, perform the following steps:</span></span>
    
    | <span data-ttu-id="449dd-170">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="449dd-170">Attribute Name</span></span> | <span data-ttu-id="449dd-171">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="449dd-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="449dd-172">Förnamn</span><span class="sxs-lookup"><span data-stu-id="449dd-172">first_name</span></span> | <span data-ttu-id="449dd-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="449dd-173">user.givenname</span></span> |
    | <span data-ttu-id="449dd-174">Efternamn</span><span class="sxs-lookup"><span data-stu-id="449dd-174">last_name</span></span> | <span data-ttu-id="449dd-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="449dd-175">user.surname</span></span> |
    | <span data-ttu-id="449dd-176">User.Email</span><span class="sxs-lookup"><span data-stu-id="449dd-176">User.Email</span></span> | <span data-ttu-id="449dd-177">User.Mail</span><span class="sxs-lookup"><span data-stu-id="449dd-177">user.mail</span></span> |  
    | <span data-ttu-id="449dd-178">User.Username</span><span class="sxs-lookup"><span data-stu-id="449dd-178">User.Username</span></span> | <span data-ttu-id="449dd-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="449dd-179">user.userprincipalname</span></span> |

    <span data-ttu-id="449dd-180">a.</span><span class="sxs-lookup"><span data-stu-id="449dd-180">a.</span></span> <span data-ttu-id="449dd-181">Klicka på **attributet** att öppna **Redigera attribut** dialogrutan rutan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="449dd-181">Click on **Attribute** to open **Edit Attribute** dialog box and perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    <span data-ttu-id="449dd-183">a.</span><span class="sxs-lookup"><span data-stu-id="449dd-183">a.</span></span> <span data-ttu-id="449dd-184">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="449dd-184">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="449dd-185">b.</span><span class="sxs-lookup"><span data-stu-id="449dd-185">b.</span></span> <span data-ttu-id="449dd-186">Från den **värdet** väljer du det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="449dd-186">From the **Value** list, select the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="449dd-187">c.</span><span class="sxs-lookup"><span data-stu-id="449dd-187">c.</span></span> <span data-ttu-id="449dd-188">Klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="449dd-188">Click **OK**</span></span>

6. <span data-ttu-id="449dd-189">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="449dd-189">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. <span data-ttu-id="449dd-191">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="449dd-191">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="449dd-193">På den **Slack Configuration** klickar du på **konfigurera Slack** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="449dd-193">On the **Slack Configuration** section, click **Configure Slack** to open **Configure sign-on** window.</span></span> <span data-ttu-id="449dd-194">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="449dd-194">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  <span data-ttu-id="449dd-196">I en annan webbläsarfönster loggar du in på webbplatsen Slack företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="449dd-196">In a different web browser window, log in to your Slack company site as an administrator.</span></span>

10.  <span data-ttu-id="449dd-197">Gå till **Microsoft Azure AD** och gå till **inställningar för Team**.</span><span class="sxs-lookup"><span data-stu-id="449dd-197">Navigate to **Microsoft Azure AD** then go to **Team Settings**.</span></span>

     ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  <span data-ttu-id="449dd-199">I den **inställningar för Team** klickar du på den **autentisering** fliken och klicka sedan på **ändra inställningar för**.</span><span class="sxs-lookup"><span data-stu-id="449dd-199">In the **Team Settings** section, click the **Authentication** tab, and then click **Change Settings**.</span></span>

     ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. <span data-ttu-id="449dd-201">På den **SAML autentiseringsinställningar** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="449dd-201">On the **SAML Authentication Settings** dialog, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    <span data-ttu-id="449dd-203">a.</span><span class="sxs-lookup"><span data-stu-id="449dd-203">a.</span></span>  <span data-ttu-id="449dd-204">I den **SAML 2.0 slutpunkt (HTTP)** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="449dd-204">In the **SAML 2.0 Endpoint (HTTP)** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="449dd-205">b.</span><span class="sxs-lookup"><span data-stu-id="449dd-205">b.</span></span>  <span data-ttu-id="449dd-206">I den **identitet providern utfärdaren** textruta klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="449dd-206">In the **Identity Provider Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="449dd-207">c.</span><span class="sxs-lookup"><span data-stu-id="449dd-207">c.</span></span>  <span data-ttu-id="449dd-208">Öppna filen hämtat certifikat i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **certifikat med offentlig** textruta.</span><span class="sxs-lookup"><span data-stu-id="449dd-208">Open your downloaded certificate file in notepad, copy the content of it into your clipboard, and then paste it to the **Public Certificate** textbox.</span></span>

    <span data-ttu-id="449dd-209">d.</span><span class="sxs-lookup"><span data-stu-id="449dd-209">d.</span></span> <span data-ttu-id="449dd-210">Konfigurera tre inställningarna ovan som passar din Slack-teamet.</span><span class="sxs-lookup"><span data-stu-id="449dd-210">Configure the above three settings as appropriate for your Slack team.</span></span> <span data-ttu-id="449dd-211">Mer information om inställningarna hittar den **Slacks guiden för konfiguration av SSO** här.</span><span class="sxs-lookup"><span data-stu-id="449dd-211">For more information about the settings, please find the **Slack's SSO configuration guide** here.</span></span> `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    <span data-ttu-id="449dd-212">e.</span><span class="sxs-lookup"><span data-stu-id="449dd-212">e.</span></span>  <span data-ttu-id="449dd-213">Klicka på **spara konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="449dd-213">Click **Save Configuration**.</span></span>
     
    <!-- Deselect **Allow users to change their email address**.

    e.  Select **Allow users to choose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> <span data-ttu-id="449dd-214">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="449dd-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="449dd-215">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="449dd-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="449dd-216">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="449dd-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="449dd-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="449dd-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="449dd-218">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="449dd-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="449dd-220">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="449dd-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="449dd-221">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="449dd-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="449dd-223">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="449dd-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="449dd-225">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="449dd-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="449dd-227">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="449dd-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="449dd-229">a.</span><span class="sxs-lookup"><span data-stu-id="449dd-229">a.</span></span> <span data-ttu-id="449dd-230">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="449dd-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="449dd-231">b.</span><span class="sxs-lookup"><span data-stu-id="449dd-231">b.</span></span> <span data-ttu-id="449dd-232">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="449dd-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="449dd-233">c.</span><span class="sxs-lookup"><span data-stu-id="449dd-233">c.</span></span> <span data-ttu-id="449dd-234">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="449dd-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="449dd-235">d.</span><span class="sxs-lookup"><span data-stu-id="449dd-235">d.</span></span> <span data-ttu-id="449dd-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="449dd-236">Click **Create**.</span></span>
 
### <a name="creating-a-slack-test-user"></a><span data-ttu-id="449dd-237">Skapa en Slack testanvändare</span><span class="sxs-lookup"><span data-stu-id="449dd-237">Creating a Slack test user</span></span>

<span data-ttu-id="449dd-238">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Slack.</span><span class="sxs-lookup"><span data-stu-id="449dd-238">The objective of this section is to create a user called Britta Simon in Slack.</span></span> <span data-ttu-id="449dd-239">Slack stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="449dd-239">Slack supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="449dd-240">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="449dd-240">There is no action item for you in this section.</span></span> <span data-ttu-id="449dd-241">En ny användare skapas under ett försök att komma åt Slack om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="449dd-241">A new user is created during an attempt to access Slack if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="449dd-242">Om du behöver skapa en användare manuellt, måste du kontakta [Slack supportteamet](https://slack.com/help/contact).</span><span class="sxs-lookup"><span data-stu-id="449dd-242">If you need to create a user manually, you need to Contact [Slack support team](https://slack.com/help/contact).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="449dd-243">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="449dd-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="449dd-244">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Slack.</span><span class="sxs-lookup"><span data-stu-id="449dd-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Slack.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="449dd-246">**Om du vill tilldela Slack Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="449dd-246">**To assign Britta Simon to Slack, perform the following steps:**</span></span>

1. <span data-ttu-id="449dd-247">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="449dd-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="449dd-249">Välj i listan med program **Slack**.</span><span class="sxs-lookup"><span data-stu-id="449dd-249">In the applications list, select **Slack**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. <span data-ttu-id="449dd-251">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="449dd-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="449dd-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="449dd-253">Click **Add** button.</span></span> <span data-ttu-id="449dd-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="449dd-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="449dd-256">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="449dd-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="449dd-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="449dd-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="449dd-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="449dd-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="449dd-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="449dd-259">Testing single sign-on</span></span>

<span data-ttu-id="449dd-260">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="449dd-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="449dd-261">När du klickar på panelen Slack åtkomst på panelen du ska hämta automatiskt loggat in på ditt Slack program.</span><span class="sxs-lookup"><span data-stu-id="449dd-261">When you click the Slack tile in the Access Panel, you should get automatically signed-on to your Slack application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="449dd-262">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="449dd-262">Additional resources</span></span>

* [<span data-ttu-id="449dd-263">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="449dd-263">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="449dd-264">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="449dd-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

