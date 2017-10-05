---
title: "Självstudier: Azure Active Directory-integrering med Amazon Web Services (AWS) | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 0fb9c8f428368271b548e3f174726fa01ea910c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a><span data-ttu-id="23900-103">Självstudier: Azure Active Directory-integrering med Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="23900-103">Tutorial: Azure Active Directory integration with Amazon Web Services (AWS)</span></span>

<span data-ttu-id="23900-104">I kursen får lära du att integrera Amazon Web Services (AWS) med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="23900-104">In this tutorial, you learn how to integrate Amazon Web Services (AWS) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="23900-105">Integrera Amazon Web Services (AWS) med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="23900-105">Integrating Amazon Web Services (AWS) with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="23900-106">Du kan styra i Azure AD som har åtkomst till Amazon Web Services (AWS)</span><span class="sxs-lookup"><span data-stu-id="23900-106">You can control in Azure AD who has access to Amazon Web Services (AWS)</span></span>
- <span data-ttu-id="23900-107">Du kan aktivera användarna att automatiskt hämta inloggade till Amazon Web Services (AWS) (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="23900-107">You can enable your users to automatically get signed-on to Amazon Web Services (AWS) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="23900-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="23900-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="23900-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="23900-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="23900-110">Krav</span><span class="sxs-lookup"><span data-stu-id="23900-110">Prerequisites</span></span>

<span data-ttu-id="23900-111">Om du vill konfigurera Azure AD-integrering med Amazon Web Services (AWS) behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="23900-111">To configure Azure AD integration with Amazon Web Services (AWS), you need the following items:</span></span>

- <span data-ttu-id="23900-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="23900-112">An Azure AD subscription</span></span>
- <span data-ttu-id="23900-113">Amazon Web Services (AWS) enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="23900-113">Amazon Web Services (AWS) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="23900-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="23900-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="23900-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="23900-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="23900-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="23900-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="23900-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="23900-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="23900-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="23900-118">Scenario description</span></span>
<span data-ttu-id="23900-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="23900-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="23900-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="23900-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="23900-121">Att lägga till Amazon Web Services (AWS) från galleriet</span><span class="sxs-lookup"><span data-stu-id="23900-121">Adding Amazon Web Services (AWS) from the gallery</span></span>
2. <span data-ttu-id="23900-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="23900-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a><span data-ttu-id="23900-123">Att lägga till Amazon Web Services (AWS) från galleriet</span><span class="sxs-lookup"><span data-stu-id="23900-123">Adding Amazon Web Services (AWS) from the gallery</span></span>
<span data-ttu-id="23900-124">Du måste lägga till Amazon Web Services (AWS) från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Amazon Web Services (AWS) i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23900-124">To configure the integration of Amazon Web Services (AWS) into Azure AD, you need to add Amazon Web Services (AWS) from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="23900-125">**Utför följande steg för att lägga till Amazon Web Services (AWS) från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="23900-125">**To add Amazon Web Services (AWS) from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="23900-126">I den  **[Azure Portal](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="23900-126">In the **[Azure Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="23900-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="23900-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="23900-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="23900-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="23900-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="23900-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="23900-133">I sökrutan skriver **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="23900-133">In the search box, type **Amazon Web Services (AWS)**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. <span data-ttu-id="23900-135">Välj i resultatpanelen **Amazon Web Services (AWS)**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="23900-135">In the results panel, select **Amazon Web Services (AWS)**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="23900-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="23900-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="23900-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Amazon Web Services (AWS) baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="23900-138">In this section, you configure and test Azure AD single sign-on with Amazon Web Services (AWS) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="23900-139">Azure AD måste du känna till motsvarande användaren i Amazon Web Services (AWS) till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="23900-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Amazon Web Services (AWS) is to a user in Azure AD.</span></span> <span data-ttu-id="23900-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Amazon Web Services (AWS) upprättas.</span><span class="sxs-lookup"><span data-stu-id="23900-140">In other words, a link relationship between an Azure AD user and the related user in Amazon Web Services (AWS) needs to be established.</span></span>

<span data-ttu-id="23900-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="23900-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Amazon Web Services (AWS).</span></span>

<span data-ttu-id="23900-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Amazon Web Services (AWS), måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="23900-142">To configure and test Azure AD single sign-on with Amazon Web Services (AWS), you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="23900-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="23900-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="23900-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23900-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="23900-145">**[Skapa en testanvändare Amazon Web Services](#creating-an-amazon-web-services-test-user)**  – du har en motsvarighet för Britta Simon i Amazon Web Services (AWS) och som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="23900-145">**[Creating an Amazon Web Services test user](#creating-an-amazon-web-services-test-user)** - to have a counterpart of Britta Simon in Amazon Web Services (AWS) that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="23900-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="23900-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="23900-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="23900-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="23900-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="23900-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="23900-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="23900-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Amazon Web Services (AWS) application.</span></span>

<span data-ttu-id="23900-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Amazon Web Services (AWS):**</span><span class="sxs-lookup"><span data-stu-id="23900-150">**To configure Azure AD single sign-on with Amazon Web Services (AWS), perform the following steps:**</span></span>

1. <span data-ttu-id="23900-151">I Azure-portalen på den **Amazon Web Services (AWS)** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="23900-151">In the Azure Portal, on the **Amazon Web Services (AWS)** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="23900-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="23900-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. <span data-ttu-id="23900-155">På den **Amazon Web Services (AWS)-domän och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="23900-155">On the **Amazon Web Services (AWS) Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. <span data-ttu-id="23900-157">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="23900-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. <span data-ttu-id="23900-159">Amazon Web Services (AWS) program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="23900-159">The Amazon Web Services (AWS) Software application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="23900-160">Konfigurera följande anspråk för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="23900-160">Please configure the following claims for this application.</span></span> <span data-ttu-id="23900-161">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="23900-161">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="23900-162">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="23900-162">The following screenshot shows an example for this.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. <span data-ttu-id="23900-164">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-164">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="23900-165">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="23900-165">Attribute Name</span></span>  | <span data-ttu-id="23900-166">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="23900-166">Attribute Value</span></span> | <span data-ttu-id="23900-167">namnområde</span><span class="sxs-lookup"><span data-stu-id="23900-167">Namespace</span></span> |
    | --------------- | --------------- | --------------- |
    | <span data-ttu-id="23900-168">RoleSessionName</span><span class="sxs-lookup"><span data-stu-id="23900-168">RoleSessionName</span></span> | <span data-ttu-id="23900-169">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="23900-169">user.userprincipalname</span></span> | <span data-ttu-id="23900-170">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="23900-170">https://aws.amazon.com/SAML/Attributes</span></span> |
    | <span data-ttu-id="23900-171">Roll</span><span class="sxs-lookup"><span data-stu-id="23900-171">Role</span></span>            | <span data-ttu-id="23900-172">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="23900-172">user.assignedroles</span></span> |  <span data-ttu-id="23900-173">https://AWS.Amazon.com/SAML/Attributes</span><span class="sxs-lookup"><span data-stu-id="23900-173">https://aws.amazon.com/SAML/Attributes</span></span> |
    
    >[!TIP]
    ><span data-ttu-id="23900-174">Du måste konfigurera användaretablering i Azure AD för att hämta alla roller från AWS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="23900-174">You need to configure the user provisioning in Azure AD to fetch all the roles from AWS Console.</span></span> <span data-ttu-id="23900-175">Se etablering stegen nedan.</span><span class="sxs-lookup"><span data-stu-id="23900-175">Please refer the provisioning steps below.</span></span>

    <span data-ttu-id="23900-176">a.</span><span class="sxs-lookup"><span data-stu-id="23900-176">a.</span></span> <span data-ttu-id="23900-177">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="23900-177">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    <span data-ttu-id="23900-179">b.</span><span class="sxs-lookup"><span data-stu-id="23900-179">b.</span></span> <span data-ttu-id="23900-180">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="23900-180">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="23900-182">c.</span><span class="sxs-lookup"><span data-stu-id="23900-182">c.</span></span> <span data-ttu-id="23900-183">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="23900-183">From the **Value** list, type the attribute value shown for that row.</span></span> <span data-ttu-id="23900-184">Lägg till Namespace-värde som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="23900-184">Add the Namespace value as given above.</span></span>
    
    <span data-ttu-id="23900-185">d.</span><span class="sxs-lookup"><span data-stu-id="23900-185">d.</span></span> <span data-ttu-id="23900-186">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="23900-186">Click **Ok**.</span></span>

7. <span data-ttu-id="23900-187">Klicka på **spara** för att spara inställningarna på Azure.</span><span class="sxs-lookup"><span data-stu-id="23900-187">Click **Save** button to save the settings on Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="23900-189">I ett annat webbläsarfönster inloggning till webbplatsen Amazon Web Services (AWS) företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="23900-189">In a different browser window, sign-on to your Amazon Web Services (AWS) company site as administrator.</span></span>

9. <span data-ttu-id="23900-190">Klicka på **konsolen Home**.</span><span class="sxs-lookup"><span data-stu-id="23900-190">Click **Console Home**.</span></span>
   
    ![Konfigurera enkel inloggning][11]

10. <span data-ttu-id="23900-192">Klicka på **IAM** från **säkerhet, identitet och efterlevnad** service.</span><span class="sxs-lookup"><span data-stu-id="23900-192">Click **IAM** from **Security, Identity & Compliance** service.</span></span>
   
    ![Konfigurera enkel inloggning][12]

11. <span data-ttu-id="23900-194">Klicka på **identitetsleverantörer**, och klicka sedan på **skapa providern**.</span><span class="sxs-lookup"><span data-stu-id="23900-194">Click **Identity Providers**, and then click **Create Provider**.</span></span>
   
    ![Konfigurera enkel inloggning][13]

12. <span data-ttu-id="23900-196">På den **konfigurera Provider** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-196">On the **Configure Provider** dialog page, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning][14]
 
    <span data-ttu-id="23900-198">a.</span><span class="sxs-lookup"><span data-stu-id="23900-198">a.</span></span> <span data-ttu-id="23900-199">Som **providertyp**väljer **SAML**.</span><span class="sxs-lookup"><span data-stu-id="23900-199">As **Provider Type**, select **SAML**.</span></span>

    <span data-ttu-id="23900-200">b.</span><span class="sxs-lookup"><span data-stu-id="23900-200">b.</span></span> <span data-ttu-id="23900-201">I den **providernamn** textruta skriver ett provider-namn (t.ex.: *trä*).</span><span class="sxs-lookup"><span data-stu-id="23900-201">In the **Provider Name** textbox, type a provider name (e.g.: *WAAD*).</span></span>

    <span data-ttu-id="23900-202">c.</span><span class="sxs-lookup"><span data-stu-id="23900-202">c.</span></span> <span data-ttu-id="23900-203">Om du vill överföra din hämtade metadatafil klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="23900-203">To upload your downloaded metadata file, click **Choose File**.</span></span>

    <span data-ttu-id="23900-204">d.</span><span class="sxs-lookup"><span data-stu-id="23900-204">d.</span></span> <span data-ttu-id="23900-205">Klicka på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="23900-205">Click **Next Step**.</span></span>

13. <span data-ttu-id="23900-206">På den **Kontrollera Providerinformationen** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="23900-206">On the **Verify Provider Information** dialog page, click **Create**.</span></span> 
    
    ![Konfigurera enkel inloggning][15]

14. <span data-ttu-id="23900-208">Klicka på **roller**, och klicka sedan på **Skapa ny roll**.</span><span class="sxs-lookup"><span data-stu-id="23900-208">Click **Roles**, and then click **Create New Role**.</span></span> 
    
    ![Konfigurera enkel inloggning][16]

15. <span data-ttu-id="23900-210">På den **ange rollnamn** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-210">On the **Set Role Name** dialog, perform the following steps:</span></span> 
    
    ![Konfigurera enkel inloggning][17] 

    <span data-ttu-id="23900-212">a.</span><span class="sxs-lookup"><span data-stu-id="23900-212">a.</span></span> <span data-ttu-id="23900-213">I den **rollnamn** textruta, ange ett rollnamn (t.ex.: *TestUser*).</span><span class="sxs-lookup"><span data-stu-id="23900-213">In the **Role Name** textbox, type a role name (e.g.: *TestUser*).</span></span> 

    <span data-ttu-id="23900-214">b.</span><span class="sxs-lookup"><span data-stu-id="23900-214">b.</span></span> <span data-ttu-id="23900-215">Klicka på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="23900-215">Click **Next Step**.</span></span>

16. <span data-ttu-id="23900-216">På den **Välj typ av roll** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-216">On the **Select Role Type** dialog, perform the following steps:</span></span> 
    
    ![Konfigurera enkel inloggning][18] 

    <span data-ttu-id="23900-218">a.</span><span class="sxs-lookup"><span data-stu-id="23900-218">a.</span></span> <span data-ttu-id="23900-219">Välj **roll för identitets-providerns åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="23900-219">Select **Role For Identity Provider Access**.</span></span> 

    <span data-ttu-id="23900-220">b.</span><span class="sxs-lookup"><span data-stu-id="23900-220">b.</span></span> <span data-ttu-id="23900-221">I den **bevilja Web Single Sign-On (WebSSO) åtkomst till SAML-providers** klickar du på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="23900-221">In the **Grant Web Single Sign-On (WebSSO) access to SAML providers** section, click **Select**.</span></span>

17. <span data-ttu-id="23900-222">På den **upprätta förtroende** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-222">On the **Establish Trust** dialog, perform the following steps:</span></span>  
    
    ![Konfigurera enkel inloggning][19] 

    <span data-ttu-id="23900-224">a.</span><span class="sxs-lookup"><span data-stu-id="23900-224">a.</span></span> <span data-ttu-id="23900-225">Välj SAML-provider som du har skapat tidigare som SAML-provider (t.ex.: *trä*)</span><span class="sxs-lookup"><span data-stu-id="23900-225">As SAML provider, select the SAML provider you have created previously (e.g.: *WAAD*)</span></span>
  
    <span data-ttu-id="23900-226">b.</span><span class="sxs-lookup"><span data-stu-id="23900-226">b.</span></span> <span data-ttu-id="23900-227">Klicka på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="23900-227">Click **Next Step**.</span></span>

18. <span data-ttu-id="23900-228">På den **Kontrollera rollen förtroende** dialogrutan klickar du på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="23900-228">On the **Verify Role Trust** dialog, click **Next Step**.</span></span>
    
    ![Konfigurera enkel inloggning][32]

19. <span data-ttu-id="23900-230">På den **koppla principen** dialogrutan klickar du på **nästa steg**.</span><span class="sxs-lookup"><span data-stu-id="23900-230">On the **Attach Policy** dialog, click **Next Step**.</span></span>
    
    ![Konfigurera enkel inloggning][33]

20. <span data-ttu-id="23900-232">På den **granska** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-232">On the **Review** dialog, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning][34]
 
    <span data-ttu-id="23900-234">a.</span><span class="sxs-lookup"><span data-stu-id="23900-234">a.</span></span> <span data-ttu-id="23900-235">Klicka på **skapa roll**.</span><span class="sxs-lookup"><span data-stu-id="23900-235">Click **Create Role**.</span></span>

    <span data-ttu-id="23900-236">b.</span><span class="sxs-lookup"><span data-stu-id="23900-236">b.</span></span> <span data-ttu-id="23900-237">Skapa så många roller vid behov och koppla dem till identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="23900-237">Create as many roles as needed and map them to the Identity Provider.</span></span>

21. <span data-ttu-id="23900-238">Konfigurera användaretablering för att hämta alla roller från AWS</span><span class="sxs-lookup"><span data-stu-id="23900-238">Now configure the user provisioning to fetch all the roles from AWS</span></span>

    <span data-ttu-id="23900-239">a.</span><span class="sxs-lookup"><span data-stu-id="23900-239">a.</span></span> <span data-ttu-id="23900-240">I AWS-konsolen logga in med ditt konto rot</span><span class="sxs-lookup"><span data-stu-id="23900-240">In the AWS Console login with your root account</span></span>

    <span data-ttu-id="23900-241">b.</span><span class="sxs-lookup"><span data-stu-id="23900-241">b.</span></span> <span data-ttu-id="23900-242">Klicka på ditt namn i övre högra hörnet och klicka sedan på den **Mina säkerhetsreferenser** alternativet.</span><span class="sxs-lookup"><span data-stu-id="23900-242">In the top right corner click your name and then click the **My Security Credentials** option.</span></span> <span data-ttu-id="23900-243">Då öppnas en skärm som en varning visas.</span><span class="sxs-lookup"><span data-stu-id="23900-243">This will open up a screen as a warning message.</span></span> <span data-ttu-id="23900-244">Klicka på knappen **säkerhetsreferenser** knapp för att ge skärmen.</span><span class="sxs-lookup"><span data-stu-id="23900-244">Click the button **Security Credentials** button to pass the screen.</span></span>
        
       ![Konfigurera enkel inloggning][36]

       ![Konfigurera enkel inloggning][37]

    <span data-ttu-id="23900-247">c.</span><span class="sxs-lookup"><span data-stu-id="23900-247">c.</span></span> <span data-ttu-id="23900-248">I avsnittet snabbtangenter klickar du på den **skapa nya åtkomstnyckel** knappen.</span><span class="sxs-lookup"><span data-stu-id="23900-248">In the Access Keys section click the **Create New Access Key** button.</span></span> <span data-ttu-id="23900-249">Detta genererar åtkomst nyckel-ID och ett token värde.</span><span class="sxs-lookup"><span data-stu-id="23900-249">This generates the Access Key ID and a token value.</span></span>
    
       ![Konfigurera enkel inloggning][38]

    <span data-ttu-id="23900-251">d.</span><span class="sxs-lookup"><span data-stu-id="23900-251">d.</span></span> <span data-ttu-id="23900-252">Kopiera båda dessa värden och även hämta den, så att du inte förlorar den.</span><span class="sxs-lookup"><span data-stu-id="23900-252">Copy both these values and also download it, so that you don't lose it.</span></span>

    <span data-ttu-id="23900-253">e.</span><span class="sxs-lookup"><span data-stu-id="23900-253">e.</span></span> <span data-ttu-id="23900-254">Klicka på Azure Portal på sidan Amazon Web Services (AWS) program integration **etablering**.</span><span class="sxs-lookup"><span data-stu-id="23900-254">In the Azure Portal, on the Amazon Web Services (AWS) application integration page, click **Provisioning**.</span></span>
        
       ![Konfigurera enkel inloggning][35]

    <span data-ttu-id="23900-256">f.</span><span class="sxs-lookup"><span data-stu-id="23900-256">f.</span></span> <span data-ttu-id="23900-257">Anger läget för etablering till **automatisk**</span><span class="sxs-lookup"><span data-stu-id="23900-257">Set the Provisioning mode to **Automatic**</span></span>
        
       ![Konfigurera enkel inloggning][39]

    <span data-ttu-id="23900-259">g.</span><span class="sxs-lookup"><span data-stu-id="23900-259">g.</span></span> <span data-ttu-id="23900-260">I den **clientsecret** och **hemlighet Token** klistra in motsvarande värden som du har kopierat från AWS-konsolen.</span><span class="sxs-lookup"><span data-stu-id="23900-260">Now in the **clientsecret** and **Secret Token** paste the corresponding values, which you have copied from AWS Console.</span></span>
    
    <span data-ttu-id="23900-261">h.</span><span class="sxs-lookup"><span data-stu-id="23900-261">h.</span></span> <span data-ttu-id="23900-262">Du kan klicka på den **Testa anslutning** knappen för att testa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="23900-262">You can click the **Test Connection** button to test the connectivity.</span></span> <span data-ttu-id="23900-263">När det går bra kan du starta etablering kopplingen.</span><span class="sxs-lookup"><span data-stu-id="23900-263">Once that is successful then you can start the provisioning connector.</span></span>
       
       ![Konfigurera enkel inloggning][40]

    <span data-ttu-id="23900-265">Jag.</span><span class="sxs-lookup"><span data-stu-id="23900-265">i.</span></span> <span data-ttu-id="23900-266">Nu aktivera etablering Status till **på**.</span><span class="sxs-lookup"><span data-stu-id="23900-266">Now enable the Provisioning Status to **On**.</span></span> <span data-ttu-id="23900-267">Detta startar hämtning av rollerna från programmet.</span><span class="sxs-lookup"><span data-stu-id="23900-267">This starts fetching the roles from the application.</span></span>

       ![Konfigurera enkel inloggning][41]

    > [!NOTE]
    > <span data-ttu-id="23900-269">Azure AD-etablering-tjänsten körs varje efter en stund att synkronisera roller från AWS.</span><span class="sxs-lookup"><span data-stu-id="23900-269">Azure AD Provisioning service runs every after some time to sync the roles from AWS.</span></span> <span data-ttu-id="23900-270">Du bör se alla identitetsleverantör ansluten AWS roller till Azure AD och du kan använda dem vid tilldela program till användare eller grupper.</span><span class="sxs-lookup"><span data-stu-id="23900-270">You should see all the Identity Provider attached AWS roles into Azure AD and you can use them while assigning the application to users or groups.</span></span>

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="23900-271">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="23900-271">Creating an Azure AD test user</span></span>
<span data-ttu-id="23900-272">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="23900-272">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="23900-274">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="23900-274">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="23900-275">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="23900-275">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="23900-277">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="23900-277">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="23900-279">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="23900-279">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="23900-281">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-281">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="23900-283">a.</span><span class="sxs-lookup"><span data-stu-id="23900-283">a.</span></span> <span data-ttu-id="23900-284">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="23900-284">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="23900-285">b.</span><span class="sxs-lookup"><span data-stu-id="23900-285">b.</span></span> <span data-ttu-id="23900-286">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="23900-286">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="23900-287">c.</span><span class="sxs-lookup"><span data-stu-id="23900-287">c.</span></span> <span data-ttu-id="23900-288">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="23900-288">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="23900-289">d.</span><span class="sxs-lookup"><span data-stu-id="23900-289">d.</span></span> <span data-ttu-id="23900-290">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="23900-290">Click **Create**.</span></span>
 
### <a name="creating-an-amazon-web-services-test-user"></a><span data-ttu-id="23900-291">Skapa en testanvändare Amazon Web Services</span><span class="sxs-lookup"><span data-stu-id="23900-291">Creating an Amazon Web Services test user</span></span>

<span data-ttu-id="23900-292">De måste etableras i Amazon Web Services (AWS) för att aktivera Azure AD-användare kan logga in till Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="23900-292">In order to enable Azure AD users to log in to Amazon Web Services (AWS), they must be provisioned into Amazon Web Services (AWS).</span></span> <span data-ttu-id="23900-293">När det gäller Amazon Web Services (AWS) är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="23900-293">In the case of Amazon Web Services (AWS), provisioning is a manual task.</span></span>

<span data-ttu-id="23900-294">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="23900-294">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="23900-295">Logga in på ditt **Amazon Web Services (AWS)** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="23900-295">Log in to your **Amazon Web Services (AWS)** company site as administrator.</span></span>

2. <span data-ttu-id="23900-296">Klicka på den **konsolen Home** ikon.</span><span class="sxs-lookup"><span data-stu-id="23900-296">Click the **Console Home** icon.</span></span> 
   
    ![Konfigurera enkel inloggning][11]

3. <span data-ttu-id="23900-298">Klicka på identitet och åtkomsthantering.</span><span class="sxs-lookup"><span data-stu-id="23900-298">Click Identity and Access Management.</span></span> 
   
    ![Konfigurera enkel inloggning][28]

4. <span data-ttu-id="23900-300">I instrumentpanelen, klickar du på **användare**, och klicka sedan på **Create New Users**.</span><span class="sxs-lookup"><span data-stu-id="23900-300">In the Dashboard, click **Users**, and then click **Create New Users**.</span></span> 
   
    ![Konfigurera enkel inloggning][29]

5. <span data-ttu-id="23900-302">I dialogrutan Skapa användare utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="23900-302">On the Create User dialog, perform the following steps:</span></span> 
   
    ![Konfigurera enkel inloggning][30]   
    
    <span data-ttu-id="23900-304">a.</span><span class="sxs-lookup"><span data-stu-id="23900-304">a.</span></span> <span data-ttu-id="23900-305">I den **ange användarnamn** textrutor, Skriv Brita Simon användarnamn (userprincipalname) i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="23900-305">In the **Enter User Names** textboxes, type Brita Simon's user name (userprincipalname) in Azure AD.</span></span>

    <span data-ttu-id="23900-306">b.</span><span class="sxs-lookup"><span data-stu-id="23900-306">b.</span></span> <span data-ttu-id="23900-307">Klicka på **skapa.**</span><span class="sxs-lookup"><span data-stu-id="23900-307">Click **Create.**</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="23900-308">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="23900-308">Assigning the Azure AD test user</span></span>

<span data-ttu-id="23900-309">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="23900-309">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Amazon Web Services (AWS).</span></span>

![Tilldela användare][200] 

<span data-ttu-id="23900-311">**Om du vill tilldela Britta Simon till Amazon Web Services (AWS), utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="23900-311">**To assign Britta Simon to Amazon Web Services (AWS), perform the following steps:**</span></span>

1. <span data-ttu-id="23900-312">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="23900-312">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="23900-314">Välj i listan med program **Amazon Web Services (AWS)**.</span><span class="sxs-lookup"><span data-stu-id="23900-314">In the applications list, select **Amazon Web Services (AWS)**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. <span data-ttu-id="23900-316">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="23900-316">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="23900-318">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="23900-318">Click **Add** button.</span></span> <span data-ttu-id="23900-319">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="23900-319">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="23900-321">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="23900-321">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="23900-322">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="23900-322">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="23900-323">På **Välj roll** väljer sedan rätt roll för användaren.</span><span class="sxs-lookup"><span data-stu-id="23900-323">On **Select Role** tab, select the appropriate role for the user.</span></span> <span data-ttu-id="23900-324">Dessa roller visas med rollnamnet och identitetsnamn för providern.</span><span class="sxs-lookup"><span data-stu-id="23900-324">All these roles are shown with the role name and identity provider name.</span></span> <span data-ttu-id="23900-325">Det här sättet kan du enkelt identifiera roller från AWS.</span><span class="sxs-lookup"><span data-stu-id="23900-325">This way you can easily identify the roles from AWS.</span></span>

8. <span data-ttu-id="23900-326">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="23900-326">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="23900-327">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="23900-327">Testing single sign-on</span></span>

<span data-ttu-id="23900-328">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="23900-328">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="23900-329">När du klickar på panelen Amazon Web Services (AWS) på åtkomstpanelen du bör få automatiskt loggat in på ditt program för Amazon Web Services (AWS).</span><span class="sxs-lookup"><span data-stu-id="23900-329">When you click the Amazon Web Services (AWS) tile in the Access Panel, you should get automatically signed-on to your Amazon Web Services (AWS) application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="23900-330">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="23900-330">Additional resources</span></span>

* [<span data-ttu-id="23900-331">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="23900-331">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="23900-332">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="23900-332">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
