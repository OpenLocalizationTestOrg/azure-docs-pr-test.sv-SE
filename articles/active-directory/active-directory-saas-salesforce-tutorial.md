---
title: "Självstudier: Azure Active Directory-integrering med Salesforce | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Salesforce."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a><span data-ttu-id="1e56e-103">Självstudier: Azure Active Directory-integrering med Salesforce</span><span class="sxs-lookup"><span data-stu-id="1e56e-103">Tutorial: Azure Active Directory integration with Salesforce</span></span>

<span data-ttu-id="1e56e-104">I kursen får lära du att integrera Salesforce med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1e56e-104">In this tutorial, you learn how to integrate Salesforce with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1e56e-105">Integrera Salesforce med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1e56e-105">Integrating Salesforce with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1e56e-106">Du kan styra i Azure AD som har åtkomst till Salesforce</span><span class="sxs-lookup"><span data-stu-id="1e56e-106">You can control in Azure AD who has access to Salesforce</span></span>
- <span data-ttu-id="1e56e-107">Du kan aktivera användarna att automatiskt hämta loggat in till Salesforce (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1e56e-107">You can enable your users to automatically get signed-on to Salesforce (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1e56e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1e56e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1e56e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1e56e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1e56e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1e56e-110">Prerequisites</span></span>

<span data-ttu-id="1e56e-111">För att konfigurera Azure AD-integrering med Salesforce, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="1e56e-111">To configure Azure AD integration with Salesforce, you need the following items:</span></span>

- <span data-ttu-id="1e56e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1e56e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1e56e-113">En Salesforce enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="1e56e-113">A Salesforce single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1e56e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1e56e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1e56e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1e56e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1e56e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1e56e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1e56e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1e56e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1e56e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1e56e-118">Scenario description</span></span>
<span data-ttu-id="1e56e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1e56e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1e56e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1e56e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1e56e-121">Att lägga till Salesforce från galleriet</span><span class="sxs-lookup"><span data-stu-id="1e56e-121">Adding Salesforce from the gallery</span></span>
2. <span data-ttu-id="1e56e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1e56e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-from-the-gallery"></a><span data-ttu-id="1e56e-123">Att lägga till Salesforce från galleriet</span><span class="sxs-lookup"><span data-stu-id="1e56e-123">Adding Salesforce from the gallery</span></span>
<span data-ttu-id="1e56e-124">Du måste lägga till Salesforce från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Salesforce i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1e56e-124">To configure the integration of Salesforce into Azure AD, you need to add Salesforce from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1e56e-125">**Utför följande steg för att lägga till Salesforce från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="1e56e-125">**To add Salesforce from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1e56e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1e56e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1e56e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1e56e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1e56e-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1e56e-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1e56e-133">I sökrutan skriver **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-133">In the search box, type **Salesforce**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. <span data-ttu-id="1e56e-135">Välj i resultatpanelen **Salesforce**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="1e56e-135">In the results panel, select **Salesforce**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1e56e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1e56e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1e56e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Salesforce baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1e56e-138">In this section, you configure and test Azure AD single sign-on with Salesforce based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1e56e-139">Azure AD måste du känna till användaren i Salesforce motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="1e56e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Salesforce is to a user in Azure AD.</span></span> <span data-ttu-id="1e56e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Salesforce upprättas.</span><span class="sxs-lookup"><span data-stu-id="1e56e-140">In other words, a link relationship between an Azure AD user and the related user in Salesforce needs to be established.</span></span>

<span data-ttu-id="1e56e-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="1e56e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Salesforce.</span></span>

<span data-ttu-id="1e56e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Salesforce, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1e56e-142">To configure and test Azure AD single sign-on with Salesforce, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1e56e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1e56e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1e56e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1e56e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1e56e-145">**[Skapa en testanvändare Salesforce](#creating-a-salesforce-test-user)**  – du har en motsvarighet för Britta Simon i Salesforce som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1e56e-145">**[Creating a Salesforce test user](#creating-a-salesforce-test-user)** - to have a counterpart of Britta Simon in Salesforce that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1e56e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1e56e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1e56e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1e56e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1e56e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1e56e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1e56e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Salesforce-program.</span><span class="sxs-lookup"><span data-stu-id="1e56e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Salesforce application.</span></span>

<span data-ttu-id="1e56e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Salesforce:**</span><span class="sxs-lookup"><span data-stu-id="1e56e-150">**To configure Azure AD single sign-on with Salesforce, perform the following steps:**</span></span>

1. <span data-ttu-id="1e56e-151">I Azure-portalen på den **Salesforce** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-151">In the Azure portal, on the **Salesforce** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1e56e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1e56e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. <span data-ttu-id="1e56e-155">På den **Salesforce-domänen och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1e56e-155">On the **Salesforce Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    <span data-ttu-id="1e56e-157">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:</span><span class="sxs-lookup"><span data-stu-id="1e56e-157">In the **Sign-on URL** textbox, type the value using the following pattern:</span></span> 
   * <span data-ttu-id="1e56e-158">Enterprise-konto:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="1e56e-158">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
   * <span data-ttu-id="1e56e-159">Utvecklarkonto för:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="1e56e-159">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1e56e-160">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="1e56e-160">These values are not the real.</span></span> <span data-ttu-id="1e56e-161">Uppdatera dessa värden med den faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="1e56e-161">Update these values with the actual Sign-on URL.</span></span> <span data-ttu-id="1e56e-162">Kontakta [Salesforce klienten supportteamet](https://help.salesforce.com/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1e56e-162">Contact [Salesforce Client support team](https://help.salesforce.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="1e56e-163">På den **SAML-signeringscertifikat** klickar du på **certifikat** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="1e56e-163">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. <span data-ttu-id="1e56e-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1e56e-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1e56e-167">På den **Salesforce Configuration** klickar du på **konfigurera Salesforce** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="1e56e-167">On the **Salesforce Configuration** section, click **Configure Salesforce** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1e56e-168">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="1e56e-168">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span> 

    <span data-ttu-id="1e56e-169">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="1e56e-169">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS></span></span>
7.  <span data-ttu-id="1e56e-170">Öppna en ny flik i webbläsaren och logga in på ditt Salesforce-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="1e56e-170">Open a new tab in your browser and log in to your Salesforce administrator account.</span></span>

8.  <span data-ttu-id="1e56e-171">Under den **administratör** navigeringsfönstret klickar du på **säkerhetsåtgärder** att expandera avsnittet relaterade.</span><span class="sxs-lookup"><span data-stu-id="1e56e-171">Under the **Administrator** navigation pane, click **Security Controls** to expand the related section.</span></span> <span data-ttu-id="1e56e-172">Klicka på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-172">Then click **Single Sign-On Settings**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  <span data-ttu-id="1e56e-174">På den **inställningar för enkel inloggning** klickar du på den **redigera** knappen.</span><span class="sxs-lookup"><span data-stu-id="1e56e-174">On the **Single Sign-On Settings** page, click the **Edit** button.</span></span>
    <span data-ttu-id="1e56e-175">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span><span class="sxs-lookup"><span data-stu-id="1e56e-175">![Configure Single Sign-On](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)</span></span>

      > [!NOTE]
      > <span data-ttu-id="1e56e-176">Om det inte går att aktivera enkel inloggning inställningarna för ditt Salesforce-konto kan du behöva kontakta [Salesforce klienten supportteamet](https://help.salesforce.com/support).</span><span class="sxs-lookup"><span data-stu-id="1e56e-176">If you are unable to enable Single Sign-On settings for your Salesforce account, you may need to contact [Salesforce Client support team](https://help.salesforce.com/support).</span></span> 

10. <span data-ttu-id="1e56e-177">Välj **SAML aktiverat**, och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-177">Select **SAML Enabled**, and then click **Save**.</span></span>

      ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. <span data-ttu-id="1e56e-179">Om du vill konfigurera SAML enkel inloggning för, klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-179">To configure your SAML single sign-on settings, click **New**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. <span data-ttu-id="1e56e-181">På den **SAML enkel inloggning inställningen redigera** kontrollerar du följande konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="1e56e-181">On the **SAML Single Sign-On Setting Edit** page, make the following configurations:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    <span data-ttu-id="1e56e-183">a.</span><span class="sxs-lookup"><span data-stu-id="1e56e-183">a.</span></span> <span data-ttu-id="1e56e-184">För den **namnet** skriver du ett eget namn för den här konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1e56e-184">For the **Name** field, type in a friendly name for this configuration.</span></span> <span data-ttu-id="1e56e-185">Att tillhandahålla ett värde för **namn** automatiskt fylla i **API-namnet** textruta.</span><span class="sxs-lookup"><span data-stu-id="1e56e-185">Providing a value for **Name** automatically populate the **API Name** textbox.</span></span>

    <span data-ttu-id="1e56e-186">b.</span><span class="sxs-lookup"><span data-stu-id="1e56e-186">b.</span></span> <span data-ttu-id="1e56e-187">Klistra in **SMAL enhets-ID** värde i den **utfärdaren** i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="1e56e-187">Paste **SMAL Entity ID** value into the **Issuer** field in Salesforce.</span></span>

    <span data-ttu-id="1e56e-188">c.</span><span class="sxs-lookup"><span data-stu-id="1e56e-188">c.</span></span> <span data-ttu-id="1e56e-189">I den **enhets-Id textruta**, ange ditt Salesforce-domännamn med hjälp av följande mönster:</span><span class="sxs-lookup"><span data-stu-id="1e56e-189">In the **Entity Id textbox**, type your Salesforce domain name using the following pattern:</span></span>
      
      * <span data-ttu-id="1e56e-190">Enterprise-konto:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="1e56e-190">Enterprise account: `https://<subdomain>.my.salesforce.com`</span></span>
      * <span data-ttu-id="1e56e-191">Utvecklarkonto för:`https://<subdomain>-dev-ed.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="1e56e-191">Developer account: `https://<subdomain>-dev-ed.my.salesforce.com`</span></span>
      
    <span data-ttu-id="1e56e-192">d.</span><span class="sxs-lookup"><span data-stu-id="1e56e-192">d.</span></span> <span data-ttu-id="1e56e-193">Klicka på **Bläddra** eller **Välj fil** att öppna den **Välj fil för uppladdning** dialogrutan Välj ditt Salesforce-certifikat och klicka sedan på **öppna** att ladda upp certifikatet.</span><span class="sxs-lookup"><span data-stu-id="1e56e-193">Click **Browse** or **Choose File** to open the **Choose File to Upload** dialog, select your Salesforce certificate, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="1e56e-194">e.</span><span class="sxs-lookup"><span data-stu-id="1e56e-194">e.</span></span> <span data-ttu-id="1e56e-195">För **SAML identitetstyp**väljer **Assertion innehåller användarens salesforce.com användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-195">For **SAML Identity Type**, select **Assertion contains User's salesforce.com username**.</span></span>

    <span data-ttu-id="1e56e-196">f.</span><span class="sxs-lookup"><span data-stu-id="1e56e-196">f.</span></span> <span data-ttu-id="1e56e-197">För **SAML identitet plats**väljer **identitet är i elementet NameIdentifier i instruktionen ämne**</span><span class="sxs-lookup"><span data-stu-id="1e56e-197">For **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**</span></span>

    <span data-ttu-id="1e56e-198">g.</span><span class="sxs-lookup"><span data-stu-id="1e56e-198">g.</span></span> <span data-ttu-id="1e56e-199">Klistra in **inloggning tjänst-URL för enkel** till den **identitet providern inloggnings-URL** i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="1e56e-199">Paste **Single Sign-On Service URL** into the **Identity Provider Login URL** field in Salesforce.</span></span>
    
    <span data-ttu-id="1e56e-200">h.</span><span class="sxs-lookup"><span data-stu-id="1e56e-200">h.</span></span> <span data-ttu-id="1e56e-201">För **providern initierade begära Tjänstbindning**väljer **HTTP-omdirigering**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-201">For **Service Provider Initiated Request Binding**, select **HTTP Redirect**.</span></span>
    
    <span data-ttu-id="1e56e-202">Jag.</span><span class="sxs-lookup"><span data-stu-id="1e56e-202">i.</span></span> <span data-ttu-id="1e56e-203">Klicka slutligen på **spara** tillämpa SAML enkel inloggning inställningarna.</span><span class="sxs-lookup"><span data-stu-id="1e56e-203">Finally, click **Save** to apply your SAML single sign-on settings.</span></span>

13. <span data-ttu-id="1e56e-204">Klicka på det vänstra navigeringsfönstret i Salesforce **domänhantering** Expandera avsnittet relaterade och klicka sedan på **min domän**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-204">On the left navigation pane in Salesforce, click **Domain Management** to expand the related section, and then click **My Domain**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. <span data-ttu-id="1e56e-206">Rulla ned till den **Autentiseringskonfiguration** avsnittet och klicka på den **redigera** knappen.</span><span class="sxs-lookup"><span data-stu-id="1e56e-206">Scroll down to the **Authentication Configuration** section, and click the **Edit** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. <span data-ttu-id="1e56e-208">I den **Autentiseringstjänsten** avsnittet väljer du ett eget namn för konfigurationen av SAML SSO och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-208">In the **Authentication Service** section, select the friendly name of your SAML SSO configuration, and then click **Save**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > <span data-ttu-id="1e56e-210">Om mer än en autentiseringstjänst är markerat uppmanas användarna att välja vilka Autentiseringstjänsten som de vill logga in med vid initiering av enkel inloggning till Salesforce-miljön.</span><span class="sxs-lookup"><span data-stu-id="1e56e-210">If more than one authentication service is selected, users are prompted to select which authentication service they like to sign in with while initiating single sign-on to your Salesforce environment.</span></span> <span data-ttu-id="1e56e-211">Om du inte vill att det ska ske, så du bör **lämnar du alla andra autentiseringstjänster avmarkerat**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-211">If you don’t want it to happen, then you should **leave all other authentication services unchecked**.</span></span>
<CE>    
> [!TIP]
> <span data-ttu-id="1e56e-212">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="1e56e-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1e56e-213">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="1e56e-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1e56e-214">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1e56e-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1e56e-215">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1e56e-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="1e56e-216">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1e56e-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1e56e-218">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="1e56e-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1e56e-219">I det vänstra navigeringsfönstret i den **Azure-portalen**, klickar du på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1e56e-219">On the left navigation pane in the **Azure portal**, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1e56e-221">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-221">To display the list of users, Go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1e56e-223">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1e56e-223">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1e56e-225">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1e56e-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1e56e-227">a.</span><span class="sxs-lookup"><span data-stu-id="1e56e-227">a.</span></span> <span data-ttu-id="1e56e-228">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1e56e-229">b.</span><span class="sxs-lookup"><span data-stu-id="1e56e-229">b.</span></span> <span data-ttu-id="1e56e-230">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1e56e-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1e56e-231">c.</span><span class="sxs-lookup"><span data-stu-id="1e56e-231">c.</span></span> <span data-ttu-id="1e56e-232">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1e56e-233">d.</span><span class="sxs-lookup"><span data-stu-id="1e56e-233">d.</span></span> <span data-ttu-id="1e56e-234">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-234">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-test-user"></a><span data-ttu-id="1e56e-235">Skapa en testanvändare Salesforce</span><span class="sxs-lookup"><span data-stu-id="1e56e-235">Creating a Salesforce test user</span></span>

<span data-ttu-id="1e56e-236">I det här avsnittet skapas en användare som kallas Britta Simon i Salesforce.</span><span class="sxs-lookup"><span data-stu-id="1e56e-236">In this section, a user called Britta Simon is created in Salesforce.</span></span> <span data-ttu-id="1e56e-237">Salesforce stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="1e56e-237">Salesforce supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="1e56e-238">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1e56e-238">There is no action item for you in this section.</span></span> <span data-ttu-id="1e56e-239">Om en användare inte redan finns i Salesforce, skapas en ny när du försöker komma åt Salesforce.</span><span class="sxs-lookup"><span data-stu-id="1e56e-239">If a user doesn't already exist in Salesforce, a new one is created when you attempt to access Salesforce.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1e56e-240">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="1e56e-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1e56e-241">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Salesforce.</span><span class="sxs-lookup"><span data-stu-id="1e56e-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Salesforce.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1e56e-243">**Om du vill tilldela Salesforce Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1e56e-243">**To assign Britta Simon to Salesforce, perform the following steps:**</span></span>

1. <span data-ttu-id="1e56e-244">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1e56e-246">Välj i listan med program **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-246">In the applications list, select **Salesforce**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. <span data-ttu-id="1e56e-248">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1e56e-250">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1e56e-250">Click **Add** button.</span></span> <span data-ttu-id="1e56e-251">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1e56e-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1e56e-253">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="1e56e-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1e56e-254">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1e56e-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1e56e-255">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1e56e-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1e56e-256">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1e56e-256">Testing single sign-on</span></span>

<span data-ttu-id="1e56e-257">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen på [https://myapps.microsoft.com](https://myapps.microsoft.com/)sedan logga in i testkonto och klicka på **Salesforce**.</span><span class="sxs-lookup"><span data-stu-id="1e56e-257">To test your single sign-on settings, open the Access Panel at [https://myapps.microsoft.com](https://myapps.microsoft.com/), then sign into the test account, and click **Salesforce**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1e56e-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1e56e-258">Additional resources</span></span>

* [<span data-ttu-id="1e56e-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1e56e-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1e56e-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1e56e-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="1e56e-261">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="1e56e-261">Configure User Provisioning</span></span>](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png

