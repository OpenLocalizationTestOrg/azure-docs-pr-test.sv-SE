---
title: "Självstudier: Azure Active Directory-integrering med PolicyStat | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och PolicyStat."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 704afd5515b02ce2a4fbf35da65fad74dc506271
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a><span data-ttu-id="2bad2-103">Självstudier: Azure Active Directory-integrering med PolicyStat</span><span class="sxs-lookup"><span data-stu-id="2bad2-103">Tutorial: Azure Active Directory integration with PolicyStat</span></span>

<span data-ttu-id="2bad2-104">I kursen får lära du att integrera PolicyStat med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2bad2-104">In this tutorial, you learn how to integrate PolicyStat with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2bad2-105">Integrera PolicyStat med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2bad2-105">Integrating PolicyStat with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2bad2-106">Du kan styra i Azure AD som har åtkomst till PolicyStat</span><span class="sxs-lookup"><span data-stu-id="2bad2-106">You can control in Azure AD who has access to PolicyStat</span></span>
- <span data-ttu-id="2bad2-107">Du kan aktivera användarna att automatiskt hämta loggat in på PolicyStat (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2bad2-107">You can enable your users to automatically get signed-on to PolicyStat (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2bad2-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2bad2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2bad2-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2bad2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2bad2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2bad2-110">Prerequisites</span></span>

<span data-ttu-id="2bad2-111">För att konfigurera Azure AD-integrering med PolicyStat, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2bad2-111">To configure Azure AD integration with PolicyStat, you need the following items:</span></span>

- <span data-ttu-id="2bad2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2bad2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2bad2-113">En PolicyStat enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2bad2-113">A PolicyStat single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2bad2-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2bad2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2bad2-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2bad2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2bad2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2bad2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2bad2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2bad2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2bad2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2bad2-118">Scenario description</span></span>
<span data-ttu-id="2bad2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2bad2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2bad2-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2bad2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2bad2-121">Att lägga till PolicyStat från galleriet</span><span class="sxs-lookup"><span data-stu-id="2bad2-121">Adding PolicyStat from the gallery</span></span>
2. <span data-ttu-id="2bad2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2bad2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-policystat-from-the-gallery"></a><span data-ttu-id="2bad2-123">Att lägga till PolicyStat från galleriet</span><span class="sxs-lookup"><span data-stu-id="2bad2-123">Adding PolicyStat from the gallery</span></span>
<span data-ttu-id="2bad2-124">Du måste lägga till PolicyStat från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av PolicyStat i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2bad2-124">To configure the integration of PolicyStat into Azure AD, you need to add PolicyStat from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2bad2-125">**Utför följande steg för att lägga till PolicyStat från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="2bad2-125">**To add PolicyStat from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2bad2-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2bad2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2bad2-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2bad2-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="2bad2-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2bad2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="2bad2-133">I sökrutan skriver **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-133">In the search box, type **PolicyStat**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. <span data-ttu-id="2bad2-135">Välj i resultatpanelen **PolicyStat**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="2bad2-135">In the results panel, select **PolicyStat**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2bad2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2bad2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2bad2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PolicyStat baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2bad2-138">In this section, you configure and test Azure AD single sign-on with PolicyStat based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2bad2-139">Azure AD måste du känna till användaren i PolicyStat motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="2bad2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PolicyStat is to a user in Azure AD.</span></span> <span data-ttu-id="2bad2-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i PolicyStat upprättas.</span><span class="sxs-lookup"><span data-stu-id="2bad2-140">In other words, a link relationship between an Azure AD user and the related user in PolicyStat needs to be established.</span></span>

<span data-ttu-id="2bad2-141">I PolicyStat, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2bad2-141">In PolicyStat, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2bad2-142">Om du vill konfigurera och testa Azure AD enkel inloggning med PolicyStat, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2bad2-142">To configure and test Azure AD single sign-on with PolicyStat, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2bad2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2bad2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2bad2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2bad2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2bad2-145">**[Skapa en testanvändare PolicyStat](#creating-a-policystat-test-user)**  – du har en motsvarighet för Britta Simon i PolicyStat som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2bad2-145">**[Creating a PolicyStat test user](#creating-a-policystat-test-user)** - to have a counterpart of Britta Simon in PolicyStat that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2bad2-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2bad2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2bad2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2bad2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2bad2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2bad2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2bad2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt PolicyStat program.</span><span class="sxs-lookup"><span data-stu-id="2bad2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PolicyStat application.</span></span>

<span data-ttu-id="2bad2-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med PolicyStat:**</span><span class="sxs-lookup"><span data-stu-id="2bad2-150">**To configure Azure AD single sign-on with PolicyStat, perform the following steps:**</span></span>

1. <span data-ttu-id="2bad2-151">I Azure-portalen på den **PolicyStat** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-151">In the Azure portal, on the **PolicyStat** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2bad2-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2bad2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. <span data-ttu-id="2bad2-155">På den **PolicyStat domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2bad2-155">On the **PolicyStat Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    <span data-ttu-id="2bad2-157">a.</span><span class="sxs-lookup"><span data-stu-id="2bad2-157">a.</span></span> <span data-ttu-id="2bad2-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.policystat.com`</span><span class="sxs-lookup"><span data-stu-id="2bad2-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.policystat.com`</span></span>

    <span data-ttu-id="2bad2-159">b.</span><span class="sxs-lookup"><span data-stu-id="2bad2-159">b.</span></span> <span data-ttu-id="2bad2-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.policystat.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="2bad2-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.policystat.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2bad2-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2bad2-161">These values are not real.</span></span> <span data-ttu-id="2bad2-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="2bad2-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2bad2-163">Kontakta [PolicyStat klienten supportteamet](http://www.policystat.com/support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2bad2-163">Contact [PolicyStat Client support team](http://www.policystat.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="2bad2-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="2bad2-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. <span data-ttu-id="2bad2-166">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till PolicyStat med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="2bad2-166">The objective of this section is to outline how to enable users to authenticate to PolicyStat with their account in Azure AD using federation based on the SAML protocol.</span></span>

    <span data-ttu-id="2bad2-167">Programmet PolicyStat förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till attributmappningar till din **SAML-Token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="2bad2-167">The PolicyStat application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span>  

     <span data-ttu-id="2bad2-168">Följande skärmbild visar ett exempel på detta.</span><span class="sxs-lookup"><span data-stu-id="2bad2-168">The following screenshot shows an example of this.</span></span>

     <span data-ttu-id="2bad2-169">![Attribut](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="2bad2-169">![Attributes](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "Attributes")</span></span>

6. <span data-ttu-id="2bad2-170">Utför följande steg för att lägga till nödvändiga attributmappning:</span><span class="sxs-lookup"><span data-stu-id="2bad2-170">To add the required attribute mappings, perform the following steps:</span></span>

    | <span data-ttu-id="2bad2-171">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="2bad2-171">Attribute Name</span></span>    |   <span data-ttu-id="2bad2-172">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="2bad2-172">Attribute Value</span></span> |
    |------------------- | -------------------- |
    | <span data-ttu-id="2bad2-173">UID</span><span class="sxs-lookup"><span data-stu-id="2bad2-173">uid</span></span> | <span data-ttu-id="2bad2-174">ExtractMailPrefix([mail])</span><span class="sxs-lookup"><span data-stu-id="2bad2-174">ExtractMailPrefix([mail])</span></span> |
    
    <span data-ttu-id="2bad2-175">a.</span><span class="sxs-lookup"><span data-stu-id="2bad2-175">a.</span></span> <span data-ttu-id="2bad2-176">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2bad2-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    <span data-ttu-id="2bad2-179">b.</span><span class="sxs-lookup"><span data-stu-id="2bad2-179">b.</span></span> <span data-ttu-id="2bad2-180">I den **attributnamn** textruta typen **uid**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-180">In the **Attribute Name** textbox, type **uid**.</span></span>

    <span data-ttu-id="2bad2-181">c.</span><span class="sxs-lookup"><span data-stu-id="2bad2-181">c.</span></span> <span data-ttu-id="2bad2-182">I den **attributvärdet** textruta väljer **ExtractMailPrefix()**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-182">In the **Attribute Value** textbox, select **ExtractMailPrefix()**.</span></span>    
   
    <span data-ttu-id="2bad2-183">d.</span><span class="sxs-lookup"><span data-stu-id="2bad2-183">d.</span></span> <span data-ttu-id="2bad2-184">Från den **e** väljer **User.mail**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-184">From the **Mail** list, select **User.mail**.</span></span>
    
    <span data-ttu-id="2bad2-185">e.</span><span class="sxs-lookup"><span data-stu-id="2bad2-185">e.</span></span> <span data-ttu-id="2bad2-186">Klicka på **Ok**</span><span class="sxs-lookup"><span data-stu-id="2bad2-186">Click **Ok**</span></span>

7. <span data-ttu-id="2bad2-187">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2bad2-187">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="2bad2-189">Logga in på webbplatsen PolicyStat företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="2bad2-189">In a different web browser window, log into your PolicyStat company site as an administrator.</span></span>

9. <span data-ttu-id="2bad2-190">Klicka på den **Admin** fliken och klicka sedan på **konfiguration för enkel inloggning** i vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="2bad2-190">Click the **Admin** tab, and then click **Single Sign-On Configuration** in left navigation pane.</span></span>
   
    <span data-ttu-id="2bad2-191">![Administratörsmenyn](./media/active-directory-saas-policystat-tutorial/ic808633.png "administratör-menyn")</span><span class="sxs-lookup"><span data-stu-id="2bad2-191">![Administrator Menu](./media/active-directory-saas-policystat-tutorial/ic808633.png "Administrator Menu")</span></span>

10. <span data-ttu-id="2bad2-192">I den **installationsprogrammet** väljer **aktivera enkel inloggning Integration**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-192">In the **Setup** section, select **Enable Single Sign-on Integration**.</span></span>
   
    <span data-ttu-id="2bad2-193">![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="2bad2-193">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "Single Sign-On Configuration")</span></span>

11. <span data-ttu-id="2bad2-194">Klicka på **Konfigurera attribut**, och sedan, i den **Konfigurera attribut** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2bad2-194">Click **Configure Attributes**, and then, in the **Configure Attributes** section, perform the following steps:</span></span>
   
    <span data-ttu-id="2bad2-195">![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="2bad2-195">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="2bad2-196">a.</span><span class="sxs-lookup"><span data-stu-id="2bad2-196">a.</span></span> <span data-ttu-id="2bad2-197">I den **Username-attributet** textruta typen **uid**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-197">In the **Username Attribute** textbox, type **uid**.</span></span>

    <span data-ttu-id="2bad2-198">b.</span><span class="sxs-lookup"><span data-stu-id="2bad2-198">b.</span></span> <span data-ttu-id="2bad2-199">I den **förnamn attributet** textruta typen **Förnamn** för användare **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-199">In the **First Name Attribute** textbox, type **firstname** of user **Britta**.</span></span>

    <span data-ttu-id="2bad2-200">c.</span><span class="sxs-lookup"><span data-stu-id="2bad2-200">c.</span></span> <span data-ttu-id="2bad2-201">I den **senaste namnattributet** textruta typen **efternamn** för användare **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-201">In the **Last Name Attribute** textbox, type **lastname** of user **Simon**.</span></span>

    <span data-ttu-id="2bad2-202">d.</span><span class="sxs-lookup"><span data-stu-id="2bad2-202">d.</span></span> <span data-ttu-id="2bad2-203">I den **e-attributet** textruta typen **emailaddress** för användare  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="2bad2-203">In the **Email Attribute** textbox, type **emailaddress** of user **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="2bad2-204">e.</span><span class="sxs-lookup"><span data-stu-id="2bad2-204">e.</span></span> <span data-ttu-id="2bad2-205">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-205">Click **Save Changes**.</span></span>

12. <span data-ttu-id="2bad2-206">Klicka på **din IDP Metadata**, och sedan, i den **din IDP Metadata** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2bad2-206">Click **Your IDP Metadata**, and then, in the **Your IDP Metadata** section, perform the following steps:</span></span>
   
    <span data-ttu-id="2bad2-207">![Enkel inloggning Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "enkel inloggning konfiguration")</span><span class="sxs-lookup"><span data-stu-id="2bad2-207">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="2bad2-208">a.</span><span class="sxs-lookup"><span data-stu-id="2bad2-208">a.</span></span> <span data-ttu-id="2bad2-209">Öppna metadatafilen hämtade, kopiera innehållet och klistrar in det i den **din identitet providern Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="2bad2-209">Open your downloaded metadata file, copy the content, and  then paste it into the **Your Identity Provider Metadata** textbox.</span></span>

    <span data-ttu-id="2bad2-210">b.</span><span class="sxs-lookup"><span data-stu-id="2bad2-210">b.</span></span> <span data-ttu-id="2bad2-211">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-211">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="2bad2-212">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="2bad2-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2bad2-213">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="2bad2-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2bad2-214">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2bad2-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2bad2-215">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2bad2-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="2bad2-216">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2bad2-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2bad2-218">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="2bad2-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2bad2-219">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2bad2-219">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2bad2-221">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-221">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2bad2-223">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2bad2-223">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2bad2-225">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="2bad2-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2bad2-227">a.</span><span class="sxs-lookup"><span data-stu-id="2bad2-227">a.</span></span> <span data-ttu-id="2bad2-228">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2bad2-229">b.</span><span class="sxs-lookup"><span data-stu-id="2bad2-229">b.</span></span> <span data-ttu-id="2bad2-230">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2bad2-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2bad2-231">c.</span><span class="sxs-lookup"><span data-stu-id="2bad2-231">c.</span></span> <span data-ttu-id="2bad2-232">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2bad2-233">d.</span><span class="sxs-lookup"><span data-stu-id="2bad2-233">d.</span></span> <span data-ttu-id="2bad2-234">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-234">Click **Create**.</span></span>
 
### <a name="creating-a-policystat-test-user"></a><span data-ttu-id="2bad2-235">Skapa en testanvändare PolicyStat</span><span class="sxs-lookup"><span data-stu-id="2bad2-235">Creating a PolicyStat test user</span></span>

<span data-ttu-id="2bad2-236">För att aktivera Azure AD-användare att logga in på PolicyStat etableras de i PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="2bad2-236">In order to enable Azure AD users to log into PolicyStat, they must be provisioned into PolicyStat.</span></span>  

<span data-ttu-id="2bad2-237">PolicyStat stöder bara i tid användaretablering.</span><span class="sxs-lookup"><span data-stu-id="2bad2-237">PolicyStat supports just in time user provisioning.</span></span> <span data-ttu-id="2bad2-238">Det innebär du behöver inte lägga till användare manuellt till PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="2bad2-238">This means, you do not need to add the users manually to PolicyStat.</span></span> <span data-ttu-id="2bad2-239">Användarna ska få automatiskt läggas till på deras första inloggning via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2bad2-239">The users will get added automatically on their first login through SSO.</span></span>

>[!NOTE]
><span data-ttu-id="2bad2-240">Du kan använda något annat PolicyStat användarens konto skapas verktyg eller API: er som tillhandahålls av PolicyStat att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="2bad2-240">You can use any other PolicyStat user account creation tools or APIs provided by PolicyStat to provision Azure AD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2bad2-241">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="2bad2-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2bad2-242">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="2bad2-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PolicyStat.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="2bad2-244">**Om du vill tilldela PolicyStat Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2bad2-244">**To assign Britta Simon to PolicyStat, perform the following steps:**</span></span>

1. <span data-ttu-id="2bad2-245">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2bad2-247">Välj i listan med program **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-247">In the applications list, select **PolicyStat**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. <span data-ttu-id="2bad2-249">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2bad2-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="2bad2-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2bad2-251">Click **Add** button.</span></span> <span data-ttu-id="2bad2-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2bad2-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="2bad2-254">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="2bad2-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2bad2-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2bad2-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2bad2-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2bad2-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2bad2-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2bad2-257">Testing single sign-on</span></span>

<span data-ttu-id="2bad2-258">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2bad2-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2bad2-259">När du klickar på panelen PolicyStat på åtkomstpanelen du bör få automatiskt loggat in på ditt PolicyStat program.</span><span class="sxs-lookup"><span data-stu-id="2bad2-259">When you click the PolicyStat tile in the Access Panel, you should get automatically signed-on to your PolicyStat application.</span></span>
<span data-ttu-id="2bad2-260">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2bad2-260">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bad2-261">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2bad2-261">Additional resources</span></span>

* [<span data-ttu-id="2bad2-262">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2bad2-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2bad2-263">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2bad2-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

