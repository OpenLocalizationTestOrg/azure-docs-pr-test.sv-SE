---
title: "Självstudier: Azure Active Directory-integrering med DocuSign | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och DocuSign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: 29c99fdf39d366df90abc070f7b836320935035c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a><span data-ttu-id="33b63-103">Självstudier: Azure Active Directory-integrering med DocuSign</span><span class="sxs-lookup"><span data-stu-id="33b63-103">Tutorial: Azure Active Directory integration with DocuSign</span></span>

<span data-ttu-id="33b63-104">I kursen får lära du att integrera DocuSign med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="33b63-104">In this tutorial, you learn how to integrate DocuSign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="33b63-105">Integrera DocuSign med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="33b63-105">Integrating DocuSign with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="33b63-106">Du kan styra i Azure AD som har åtkomst till DocuSign</span><span class="sxs-lookup"><span data-stu-id="33b63-106">You can control in Azure AD who has access to DocuSign</span></span>
- <span data-ttu-id="33b63-107">Du kan aktivera användarna att automatiskt hämta loggat in på DocuSign (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="33b63-107">You can enable your users to automatically get signed-on to DocuSign (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="33b63-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="33b63-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="33b63-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="33b63-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33b63-110">Krav</span><span class="sxs-lookup"><span data-stu-id="33b63-110">Prerequisites</span></span>

<span data-ttu-id="33b63-111">För att konfigurera Azure AD-integrering med DocuSign, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="33b63-111">To configure Azure AD integration with DocuSign, you need the following items:</span></span>

- <span data-ttu-id="33b63-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="33b63-112">An Azure AD subscription</span></span>
- <span data-ttu-id="33b63-113">En DocuSign enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="33b63-113">A DocuSign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="33b63-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="33b63-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="33b63-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="33b63-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="33b63-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="33b63-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="33b63-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="33b63-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="33b63-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="33b63-118">Scenario description</span></span>
<span data-ttu-id="33b63-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="33b63-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="33b63-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="33b63-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="33b63-121">Att lägga till DocuSign från galleriet</span><span class="sxs-lookup"><span data-stu-id="33b63-121">Adding DocuSign from the gallery</span></span>
2. <span data-ttu-id="33b63-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33b63-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-docusign-from-the-gallery"></a><span data-ttu-id="33b63-123">Att lägga till DocuSign från galleriet</span><span class="sxs-lookup"><span data-stu-id="33b63-123">Adding DocuSign from the gallery</span></span>
<span data-ttu-id="33b63-124">Du måste lägga till DocuSign från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av DocuSign i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="33b63-124">To configure the integration of DocuSign into Azure AD, you need to add DocuSign from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="33b63-125">**Utför följande steg för att lägga till DocuSign från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="33b63-125">**To add DocuSign from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="33b63-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="33b63-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="33b63-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="33b63-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="33b63-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="33b63-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="33b63-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33b63-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="33b63-133">I sökrutan skriver **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="33b63-133">In the search box, type **DocuSign**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. <span data-ttu-id="33b63-135">Välj i resultatpanelen **DocuSign**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="33b63-135">In the results panel, select **DocuSign**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="33b63-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33b63-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="33b63-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med DocuSign baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="33b63-138">In this section, you configure and test Azure AD single sign-on with DocuSign based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="33b63-139">Azure AD måste du känna till användaren i DocuSign motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="33b63-139">For single sign-on to work, Azure AD needs to know what the counterpart user in DocuSign is to a user in Azure AD.</span></span> <span data-ttu-id="33b63-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i DocuSign upprättas.</span><span class="sxs-lookup"><span data-stu-id="33b63-140">In other words, a link relationship between an Azure AD user and the related user in DocuSign needs to be established.</span></span>

<span data-ttu-id="33b63-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i DocuSign.</span><span class="sxs-lookup"><span data-stu-id="33b63-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in DocuSign.</span></span>

<span data-ttu-id="33b63-142">Om du vill konfigurera och testa Azure AD enkel inloggning med DocuSign, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="33b63-142">To configure and test Azure AD single sign-on with DocuSign, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="33b63-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="33b63-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="33b63-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="33b63-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="33b63-145">**[Skapa en testanvändare DocuSign](#creating-a-docusign-test-user)**  – du har en motsvarighet för Britta Simon i DocuSign som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="33b63-145">**[Creating a DocuSign test user](#creating-a-docusign-test-user)** - to have a counterpart of Britta Simon in DocuSign that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="33b63-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="33b63-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="33b63-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="33b63-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="33b63-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33b63-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="33b63-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt DocuSign program.</span><span class="sxs-lookup"><span data-stu-id="33b63-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your DocuSign application.</span></span>

<span data-ttu-id="33b63-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med DocuSign:**</span><span class="sxs-lookup"><span data-stu-id="33b63-150">**To configure Azure AD single sign-on with DocuSign, perform the following steps:**</span></span>

1. <span data-ttu-id="33b63-151">I Azure-portalen på den **DocuSign** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="33b63-151">In the Azure portal, on the **DocuSign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="33b63-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="33b63-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. <span data-ttu-id="33b63-155">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base 64)** och spara certifikatet på datorn.</span><span class="sxs-lookup"><span data-stu-id="33b63-155">On the **SAML Signing Certificate** section, click **Certificate(Base 64)** and then save certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. <span data-ttu-id="33b63-157">På den **DocuSign Configuration** på Azure portal, klicka på **konfigurera DocuSign** öppna Konfigurera inloggning fönster.</span><span class="sxs-lookup"><span data-stu-id="33b63-157">On the **DocuSign Configuration** section of Azure portal, Click **Configure DocuSign** to open Configure sign-on window.</span></span> <span data-ttu-id="33b63-158">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="33b63-158">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. <span data-ttu-id="33b63-160">I en annan webbläsarfönstret, logga in på ditt **DocuSign administrationsportalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="33b63-160">In a different web browser window, login to your **DocuSign admin portal** as an administrator.</span></span>

6. <span data-ttu-id="33b63-161">Klicka på navigeringsmenyn till vänster **domäner**.</span><span class="sxs-lookup"><span data-stu-id="33b63-161">In the navigation menu on the left, click **Domains**.</span></span>
   
    ![Konfigurera enkel inloggning][51]

7. <span data-ttu-id="33b63-163">I den högra rutan, klickar du på **anspråk domän**.</span><span class="sxs-lookup"><span data-stu-id="33b63-163">On the right pane, click **Claim Domain**.</span></span>
   
    ![Konfigurera enkel inloggning][52]

8. <span data-ttu-id="33b63-165">På den **anspråk en domän** dialogrutan i den **domännamn** textruta Skriv företagets domän och klicka sedan på **anspråk**.</span><span class="sxs-lookup"><span data-stu-id="33b63-165">On the **Claim a domain** dialog, in the **Domain Name** textbox, type your company domain, and then click **Claim**.</span></span> <span data-ttu-id="33b63-166">Se till att du verifiera domänen och att statusen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="33b63-166">Make sure that you verify the domain and the status is active.</span></span>
   
    ![Konfigurera enkel inloggning][53]

9. <span data-ttu-id="33b63-168">Klicka på menyn till vänster **identitetsleverantörer**</span><span class="sxs-lookup"><span data-stu-id="33b63-168">In menu on the left side, click **Identity Providers**</span></span>  
   
    ![Konfigurera enkel inloggning][54]
10. <span data-ttu-id="33b63-170">I den högra rutan, klickar du på **lägga till identitetsleverantör**.</span><span class="sxs-lookup"><span data-stu-id="33b63-170">In the right pane, click **Add Identity Provider**.</span></span> 
   
    ![Konfigurera enkel inloggning][55]

11. <span data-ttu-id="33b63-172">På den **identitet providerinställningar** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="33b63-172">On the **Identity Provider Settings** page, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning][56]

    <span data-ttu-id="33b63-174">a.</span><span class="sxs-lookup"><span data-stu-id="33b63-174">a.</span></span> <span data-ttu-id="33b63-175">I den **namn** textruta, ange ett unikt namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="33b63-175">In the **Name** textbox, type a unique name for your configuration.</span></span> <span data-ttu-id="33b63-176">Använd inte blanksteg.</span><span class="sxs-lookup"><span data-stu-id="33b63-176">Do not use spaces.</span></span>

    <span data-ttu-id="33b63-177">b.</span><span class="sxs-lookup"><span data-stu-id="33b63-177">b.</span></span> <span data-ttu-id="33b63-178">Klistra in **SAML enhets-ID** till den **identitet providern utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="33b63-178">Paste **SAML Entity ID** into the **Identity Provider Issuer** textbox.</span></span>

    <span data-ttu-id="33b63-179">c.</span><span class="sxs-lookup"><span data-stu-id="33b63-179">c.</span></span> <span data-ttu-id="33b63-180">Klistra in **SAML enkel inloggning Tjänstwebbadress** till den **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="33b63-180">Paste **SAML Single Sign-On Service URL** into the **Identity Provider Login URL** textbox.</span></span>

    <span data-ttu-id="33b63-181">d.</span><span class="sxs-lookup"><span data-stu-id="33b63-181">d.</span></span> <span data-ttu-id="33b63-182">Klistra in **Sign-Out URL** till den **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="33b63-182">Paste **Sign-Out URL** into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="33b63-183">e.</span><span class="sxs-lookup"><span data-stu-id="33b63-183">e.</span></span> <span data-ttu-id="33b63-184">Välj **logga AuthN begäran**.</span><span class="sxs-lookup"><span data-stu-id="33b63-184">Select **Sign AuthN Request**.</span></span>

    <span data-ttu-id="33b63-185">f.</span><span class="sxs-lookup"><span data-stu-id="33b63-185">f.</span></span> <span data-ttu-id="33b63-186">Som **skicka AuthN-begäran från**väljer **efter**.</span><span class="sxs-lookup"><span data-stu-id="33b63-186">As **Send AuthN request by**, select **POST**.</span></span>

    <span data-ttu-id="33b63-187">g.</span><span class="sxs-lookup"><span data-stu-id="33b63-187">g.</span></span> <span data-ttu-id="33b63-188">Som **skicka logga ut begäran från**väljer **hämta**.</span><span class="sxs-lookup"><span data-stu-id="33b63-188">As **Send logout request by**, select **GET**.</span></span>

12. <span data-ttu-id="33b63-189">I den **anpassad attributmappning** väljer du de fält som du vill mappa med Azure AD-anspråk.</span><span class="sxs-lookup"><span data-stu-id="33b63-189">In the **Custom Attribute Mapping** section, choose the field you want to map with Azure AD Claim.</span></span> <span data-ttu-id="33b63-190">I det här exemplet i **emailaddress** anspråk mappas med värdet för **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="33b63-190">In this example, the **emailaddress** claim is mapped with the value of **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span> <span data-ttu-id="33b63-191">Det är standardnamnet för anspråk från Azure AD för e-anspråk.</span><span class="sxs-lookup"><span data-stu-id="33b63-191">It is the default claim name from Azure AD for email claim.</span></span> 
   
    > [!NOTE]
    > <span data-ttu-id="33b63-192">Använd rätt **användar-ID** att mappa användaren från Azure AD att DocuSign mappning.</span><span class="sxs-lookup"><span data-stu-id="33b63-192">Use the appropriate **User identifier** to map the user from Azure AD to DocuSign user mapping.</span></span> <span data-ttu-id="33b63-193">Välj rätt fältet och ange rätt värde baserat på dina organisationsinställningar.</span><span class="sxs-lookup"><span data-stu-id="33b63-193">Select the proper Field and enter the appropriate value based on your organization settings.</span></span>
          
    ![Konfigurera enkel inloggning][57]

13. <span data-ttu-id="33b63-195">I den **providern identitetscertifikat** klickar du på **Lägg till certifikat**, och sedan ladda upp det certifikat som du har hämtat från Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="33b63-195">In the **Identity Provider Certificate** section, click **Add Certificate**, and then upload the certificate you have downloaded from Azure AD portal.</span></span>   
   
    ![Konfigurera enkel inloggning][58]

14. <span data-ttu-id="33b63-197">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="33b63-197">Click **Save**.</span></span>

15. <span data-ttu-id="33b63-198">I den **identitetsleverantörer** klickar du på **åtgärder**, och klicka sedan på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="33b63-198">In the **Identity Providers** section, click **Actions**, and then click **Endpoints**.</span></span>   
   
    ![Konfigurera enkel inloggning][59]
 
16. <span data-ttu-id="33b63-200">I den **visa SAML 2.0 slutpunkter** avsnittet på **DocuSign administrationsportal**, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="33b63-200">In the **View SAML 2.0 Endpoints** section on **DocuSign admin portal**, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning][60]
   
    <span data-ttu-id="33b63-202">a.</span><span class="sxs-lookup"><span data-stu-id="33b63-202">a.</span></span> <span data-ttu-id="33b63-203">Kopiera den **Service Provider utfärdar-URL**, och sedan klistra in i den **identifierare** textruta på **DocuSign domän och URL: er** på Azure portal följer mönstret: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span><span class="sxs-lookup"><span data-stu-id="33b63-203">Copy the **Service Provider Issuer URL**, and then paste into the **Identifier** textbox on **DocuSign Domain and URLs** section of the Azure portal following the pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`.</span></span>
   
    <span data-ttu-id="33b63-204">b.</span><span class="sxs-lookup"><span data-stu-id="33b63-204">b.</span></span> <span data-ttu-id="33b63-205">Kopiera den **Service Provider inloggnings-URL**, och sedan klistra in i den **logga URL** textruta på **DocuSign domän och URL: er** på Azure portal följer mönstret: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span><span class="sxs-lookup"><span data-stu-id="33b63-205">Copy the **Service Provider Login URL**, and then paste into the **Sign On URL** textbox on **DocuSign Domain and URLs** section of the Azure portal following the pattern: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    <span data-ttu-id="33b63-207">c.</span><span class="sxs-lookup"><span data-stu-id="33b63-207">c.</span></span>  <span data-ttu-id="33b63-208">Klicka på **Stäng**</span><span class="sxs-lookup"><span data-stu-id="33b63-208">Click **Close**</span></span>
    
17. <span data-ttu-id="33b63-209">Klicka på Azure-portalen **spara**.</span><span class="sxs-lookup"><span data-stu-id="33b63-209">On the Azure portal, click **Save**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="33b63-211">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="33b63-211">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="33b63-212">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="33b63-212">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="33b63-213">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="33b63-213">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="33b63-214">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="33b63-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="33b63-215">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="33b63-215">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="33b63-217">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="33b63-217">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="33b63-218">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="33b63-218">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="33b63-220">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="33b63-220">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="33b63-222">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33b63-222">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="33b63-224">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="33b63-224">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="33b63-226">a.</span><span class="sxs-lookup"><span data-stu-id="33b63-226">a.</span></span> <span data-ttu-id="33b63-227">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="33b63-227">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="33b63-228">b.</span><span class="sxs-lookup"><span data-stu-id="33b63-228">b.</span></span> <span data-ttu-id="33b63-229">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="33b63-229">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="33b63-230">c.</span><span class="sxs-lookup"><span data-stu-id="33b63-230">c.</span></span> <span data-ttu-id="33b63-231">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="33b63-231">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="33b63-232">d.</span><span class="sxs-lookup"><span data-stu-id="33b63-232">d.</span></span> <span data-ttu-id="33b63-233">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="33b63-233">Click **Create**.</span></span>
 
### <a name="creating-a-docusign-test-user"></a><span data-ttu-id="33b63-234">Skapa en testanvändare DocuSign</span><span class="sxs-lookup"><span data-stu-id="33b63-234">Creating a DocuSign test user</span></span>

<span data-ttu-id="33b63-235">Programmet stöder **precis i tid användaretablering** och efter autentisering användare skapas automatiskt i programmet.</span><span class="sxs-lookup"><span data-stu-id="33b63-235">Application supports **Just in time user provisioning** and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="33b63-236">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="33b63-236">Assigning the Azure AD test user</span></span>

<span data-ttu-id="33b63-237">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till DocuSign.</span><span class="sxs-lookup"><span data-stu-id="33b63-237">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to DocuSign.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="33b63-239">**Om du vill tilldela DocuSign Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="33b63-239">**To assign Britta Simon to DocuSign, perform the following steps:**</span></span>

1. <span data-ttu-id="33b63-240">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="33b63-240">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="33b63-242">Välj i listan med program **DocuSign**.</span><span class="sxs-lookup"><span data-stu-id="33b63-242">In the applications list, select **DocuSign**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. <span data-ttu-id="33b63-244">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="33b63-244">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="33b63-246">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="33b63-246">Click **Add** button.</span></span> <span data-ttu-id="33b63-247">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33b63-247">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="33b63-249">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="33b63-249">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="33b63-250">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33b63-250">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="33b63-251">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="33b63-251">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="33b63-252">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="33b63-252">Testing single sign-on</span></span>

<span data-ttu-id="33b63-253">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="33b63-253">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="33b63-254">När du klickar på panelen DocuSign på åtkomstpanelen du bör få automatiskt loggat in på ditt DocuSign program.</span><span class="sxs-lookup"><span data-stu-id="33b63-254">When you click the DocuSign tile in the Access Panel, you should get automatically signed-on to your DocuSign application.</span></span>
<span data-ttu-id="33b63-255">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="33b63-255">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="33b63-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="33b63-256">Additional resources</span></span>

* [<span data-ttu-id="33b63-257">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="33b63-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="33b63-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="33b63-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="33b63-259">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="33b63-259">Configure User Provisioning</span></span>](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png

