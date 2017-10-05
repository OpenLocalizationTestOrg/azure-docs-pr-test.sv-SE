---
title: "Självstudier: Azure Active Directory-integrering med Veracode | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Veracode."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4fe78050-cb6d-4db9-96ec-58cc0779167f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: d49349c5ae08e67d91e30967f3644623211823ce
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-veracode"></a><span data-ttu-id="6aed9-103">Självstudier: Azure Active Directory-integrering med Veracode</span><span class="sxs-lookup"><span data-stu-id="6aed9-103">Tutorial: Azure Active Directory integration with Veracode</span></span>

<span data-ttu-id="6aed9-104">I kursen får lära du att integrera Veracode med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6aed9-104">In this tutorial, you learn how to integrate Veracode with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6aed9-105">Integrera Veracode med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6aed9-105">Integrating Veracode with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6aed9-106">Du kan styra i Azure AD som har åtkomst till Veracode.</span><span class="sxs-lookup"><span data-stu-id="6aed9-106">You can control in Azure AD who has access to Veracode.</span></span>
- <span data-ttu-id="6aed9-107">Du kan aktivera användarna att automatiskt hämta loggat in på Veracode (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="6aed9-107">You can enable your users to automatically get signed-on to Veracode (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="6aed9-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6aed9-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="6aed9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6aed9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6aed9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6aed9-110">Prerequisites</span></span>

<span data-ttu-id="6aed9-111">För att konfigurera Azure AD-integrering med Veracode, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="6aed9-111">To configure Azure AD integration with Veracode, you need the following items:</span></span>

- <span data-ttu-id="6aed9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6aed9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6aed9-113">En Veracode enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6aed9-113">A Veracode single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6aed9-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6aed9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6aed9-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6aed9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6aed9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6aed9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6aed9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6aed9-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6aed9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6aed9-118">Scenario description</span></span>
<span data-ttu-id="6aed9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6aed9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6aed9-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6aed9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6aed9-121">Lägg till Veracode från galleriet</span><span class="sxs-lookup"><span data-stu-id="6aed9-121">Add Veracode from the gallery</span></span>
2. <span data-ttu-id="6aed9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aed9-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-veracode-from-the-gallery"></a><span data-ttu-id="6aed9-123">Lägg till Veracode från galleriet</span><span class="sxs-lookup"><span data-stu-id="6aed9-123">Add Veracode from the gallery</span></span>
<span data-ttu-id="6aed9-124">Du måste lägga till Veracode från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Veracode i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6aed9-124">To configure the integration of Veracode into Azure AD, you need to add Veracode from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6aed9-125">**Utför följande steg för att lägga till Veracode från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="6aed9-125">**To add Veracode from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6aed9-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6aed9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="6aed9-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6aed9-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="6aed9-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aed9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="6aed9-133">I sökrutan skriver **Veracode**väljer **Veracode** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6aed9-133">In the search box, type **Veracode**, select  **Veracode** from result panel then click **Add** button to add the application.</span></span>

    ![Veracode i resultatlistan](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6aed9-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aed9-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="6aed9-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Veracode baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6aed9-136">In this section, you configure and test Azure AD single sign-on with Veracode based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6aed9-137">Azure AD måste du känna till användaren i Veracode motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="6aed9-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Veracode is to a user in Azure AD.</span></span> <span data-ttu-id="6aed9-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Veracode upprättas.</span><span class="sxs-lookup"><span data-stu-id="6aed9-138">In other words, a link relationship between an Azure AD user and the related user in Veracode needs to be established.</span></span>

<span data-ttu-id="6aed9-139">I Veracode, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6aed9-139">In Veracode, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6aed9-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Veracode, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6aed9-140">To configure and test Azure AD single sign-on with Veracode, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6aed9-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6aed9-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6aed9-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6aed9-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6aed9-143">**[Skapa en testanvändare Veracode](#create-a-veracode-test-user)**  – du har en motsvarighet för Britta Simon i Veracode som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6aed9-143">**[Create a Veracode test user](#create-a-veracode-test-user)** - to have a counterpart of Britta Simon in Veracode that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6aed9-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6aed9-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6aed9-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6aed9-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="6aed9-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aed9-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="6aed9-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Veracode program.</span><span class="sxs-lookup"><span data-stu-id="6aed9-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Veracode application.</span></span>

<span data-ttu-id="6aed9-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Veracode:**</span><span class="sxs-lookup"><span data-stu-id="6aed9-148">**To configure Azure AD single sign-on with Veracode, perform the following steps:**</span></span>

1. <span data-ttu-id="6aed9-149">I Azure-portalen på den **Veracode** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-149">In the Azure portal, on the **Veracode** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="6aed9-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6aed9-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_samlbase.png)

3. <span data-ttu-id="6aed9-153">På den **Veracode domän och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="6aed9-153">On the **Veracode Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_url.png)

4. <span data-ttu-id="6aed9-155">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="6aed9-155">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_certificate.png) 

5. <span data-ttu-id="6aed9-157">Syftet med det här avsnittet är att beskriva hur användarna att autentisera till Veracode med sitt konto i Azure AD med hjälp av federation baserat på SAML-protokoll.</span><span class="sxs-lookup"><span data-stu-id="6aed9-157">The objective of this section is to outline how to enable users to authenticate to Veracode with their account in Azure AD using federation based on the SAML protocol.</span></span>

    <span data-ttu-id="6aed9-158">Tillämpningsprogrammet Veracode förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till attributmappningar till din **saml-token attribut** konfiguration.</span><span class="sxs-lookup"><span data-stu-id="6aed9-158">Your Veracode application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **saml token attributes** configuration.</span></span> <span data-ttu-id="6aed9-159">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="6aed9-159">The following screenshot shows an example for this.</span></span>
    
    <span data-ttu-id="6aed9-160">![Attribut](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="6aed9-160">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_attr.png "Attributes")</span></span>

6. <span data-ttu-id="6aed9-161">Utför följande steg för att lägga till nödvändiga attributmappning:</span><span class="sxs-lookup"><span data-stu-id="6aed9-161">To add the required attribute mappings, perform the following steps:</span></span>

    | <span data-ttu-id="6aed9-162">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="6aed9-162">Attribute Name</span></span> | <span data-ttu-id="6aed9-163">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="6aed9-163">Attribute Value</span></span> |
    |--- |--- |
    | <span data-ttu-id="6aed9-164">Förnamn</span><span class="sxs-lookup"><span data-stu-id="6aed9-164">firstname</span></span> |<span data-ttu-id="6aed9-165">User.givenName</span><span class="sxs-lookup"><span data-stu-id="6aed9-165">User.givenname</span></span> |
    | <span data-ttu-id="6aed9-166">Efternamn</span><span class="sxs-lookup"><span data-stu-id="6aed9-166">lastname</span></span> |<span data-ttu-id="6aed9-167">User.surname</span><span class="sxs-lookup"><span data-stu-id="6aed9-167">User.surname</span></span> |
    | <span data-ttu-id="6aed9-168">E-post</span><span class="sxs-lookup"><span data-stu-id="6aed9-168">email</span></span> |<span data-ttu-id="6aed9-169">User.Mail</span><span class="sxs-lookup"><span data-stu-id="6aed9-169">User.mail</span></span> |
    
    <span data-ttu-id="6aed9-170">a.</span><span class="sxs-lookup"><span data-stu-id="6aed9-170">a.</span></span> <span data-ttu-id="6aed9-171">För varje datarad i tabellen ovan, klickar du på **lägga till användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-171">For each data row in the table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="6aed9-172">![Attribut](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="6aed9-172">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr.png "Attributes")</span></span>
    
    <span data-ttu-id="6aed9-173">![Attribut](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="6aed9-173">![Attributes](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_addattr1.png "Attributes")</span></span>
    
    <span data-ttu-id="6aed9-174">b.</span><span class="sxs-lookup"><span data-stu-id="6aed9-174">b.</span></span> <span data-ttu-id="6aed9-175">I den **attributnamn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="6aed9-175">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="6aed9-176">c.</span><span class="sxs-lookup"><span data-stu-id="6aed9-176">c.</span></span> <span data-ttu-id="6aed9-177">I den **attributvärdet** textruta Välj attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="6aed9-177">In the **Attribute Value** textbox, select the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="6aed9-178">d.</span><span class="sxs-lookup"><span data-stu-id="6aed9-178">d.</span></span> <span data-ttu-id="6aed9-179">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-179">Click **Ok**.</span></span>

7. <span data-ttu-id="6aed9-180">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6aed9-180">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-veracode-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="6aed9-182">På den **Veracode Configuration** klickar du på **konfigurera Veracode** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6aed9-182">On the **Veracode Configuration** section, click **Configure Veracode** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6aed9-183">Kopiera den **SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6aed9-183">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Veracode konfiguration](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_configure.png) 

9. <span data-ttu-id="6aed9-185">Logga in på webbplatsen Veracode företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="6aed9-185">In a different web browser window, log into your Veracode company site as an administrator.</span></span>

10. <span data-ttu-id="6aed9-186">Klicka på menyn högst upp **inställningar**, och klicka sedan på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-186">In the menu on the top, click **Settings**, and then click **Admin**.</span></span>
   
    <span data-ttu-id="6aed9-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="6aed9-187">![Administration](./media/active-directory-saas-veracode-tutorial/ic802911.png "Administration")</span></span>

11. <span data-ttu-id="6aed9-188">Klicka på den **SAML** fliken.</span><span class="sxs-lookup"><span data-stu-id="6aed9-188">Click the **SAML** tab.</span></span>

12. <span data-ttu-id="6aed9-189">I den **SAML organisationsinställningar** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6aed9-189">In the **Organization SAML Settings** section, perform the following steps:</span></span>
   
    <span data-ttu-id="6aed9-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="6aed9-190">![Administration](./media/active-directory-saas-veracode-tutorial/ic802912.png "Administration")</span></span>
   
    <span data-ttu-id="6aed9-191">a.</span><span class="sxs-lookup"><span data-stu-id="6aed9-191">a.</span></span>  <span data-ttu-id="6aed9-192">I **utfärdaren** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6aed9-192">In  **Issuer** textbox, paste the value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="6aed9-193">b.</span><span class="sxs-lookup"><span data-stu-id="6aed9-193">b.</span></span> <span data-ttu-id="6aed9-194">Om du vill överföra din hämtat certifikat från Azure-portalen klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-194">To upload your downloaded certificate from Azure portal, click **Choose File**.</span></span>
   
    <span data-ttu-id="6aed9-195">c.</span><span class="sxs-lookup"><span data-stu-id="6aed9-195">c.</span></span> <span data-ttu-id="6aed9-196">Välj **aktivera självregistrering**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-196">Select **Enable Self Registration**.</span></span>

13. <span data-ttu-id="6aed9-197">I den **Self Registreringsinställningar** avsnittet, utför följande steg och klicka sedan på **spara**:</span><span class="sxs-lookup"><span data-stu-id="6aed9-197">In the **Self Registration Settings** section, perform the following steps, and then click **Save**:</span></span>
   
    <span data-ttu-id="6aed9-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="6aed9-198">![Administration](./media/active-directory-saas-veracode-tutorial/ic802913.png "Administration")</span></span>
   
    <span data-ttu-id="6aed9-199">a.</span><span class="sxs-lookup"><span data-stu-id="6aed9-199">a.</span></span> <span data-ttu-id="6aed9-200">Som **aktivera nya användare**väljer **Nej-aktivering krävs**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-200">As **New User Activation**, select **No Activation Required**.</span></span>
   
    <span data-ttu-id="6aed9-201">b.</span><span class="sxs-lookup"><span data-stu-id="6aed9-201">b.</span></span> <span data-ttu-id="6aed9-202">Som **Data uppdateras**väljer **inställningar Veracode användardata**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-202">As **User Data Updates**, select **Preference Veracode User Data**.</span></span>
   
    <span data-ttu-id="6aed9-203">c.</span><span class="sxs-lookup"><span data-stu-id="6aed9-203">c.</span></span> <span data-ttu-id="6aed9-204">För **SAML attributinformation**, Välj följande:</span><span class="sxs-lookup"><span data-stu-id="6aed9-204">For **SAML Attribute Details**, select the following:</span></span>
      * <span data-ttu-id="6aed9-205">**Användarroller**</span><span class="sxs-lookup"><span data-stu-id="6aed9-205">**User Roles**</span></span>
      * <span data-ttu-id="6aed9-206">**Princip för administratör**</span><span class="sxs-lookup"><span data-stu-id="6aed9-206">**Policy Administrator**</span></span>
      * <span data-ttu-id="6aed9-207">**Granskare**</span><span class="sxs-lookup"><span data-stu-id="6aed9-207">**Reviewer**</span></span>
      * <span data-ttu-id="6aed9-208">**Säkerhet Lead**</span><span class="sxs-lookup"><span data-stu-id="6aed9-208">**Security Lead**</span></span>
      * <span data-ttu-id="6aed9-209">**Verkställande**</span><span class="sxs-lookup"><span data-stu-id="6aed9-209">**Executive**</span></span>
      * <span data-ttu-id="6aed9-210">**Avsändaren**</span><span class="sxs-lookup"><span data-stu-id="6aed9-210">**Submitter**</span></span>
      * <span data-ttu-id="6aed9-211">**Skapare**</span><span class="sxs-lookup"><span data-stu-id="6aed9-211">**Creator**</span></span>
      * <span data-ttu-id="6aed9-212">**Alla typer av genomgångar**</span><span class="sxs-lookup"><span data-stu-id="6aed9-212">**All Scan Types**</span></span>
      * <span data-ttu-id="6aed9-213">**Medlemskap i gruppen**</span><span class="sxs-lookup"><span data-stu-id="6aed9-213">**Team Memberships**</span></span>
      * <span data-ttu-id="6aed9-214">**Standard-teamet**</span><span class="sxs-lookup"><span data-stu-id="6aed9-214">**Default Team**</span></span>

> [!TIP]
> <span data-ttu-id="6aed9-215">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="6aed9-215">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6aed9-216">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="6aed9-216">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6aed9-217">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6aed9-217">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6aed9-218">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6aed9-218">Create an Azure AD test user</span></span>

<span data-ttu-id="6aed9-219">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6aed9-219">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="6aed9-221">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="6aed9-221">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6aed9-222">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="6aed9-222">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-veracode-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="6aed9-224">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-224">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-veracode-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="6aed9-226">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aed9-226">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-veracode-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="6aed9-228">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6aed9-228">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-veracode-tutorial/create_aaduser_04.png)

    <span data-ttu-id="6aed9-230">a.</span><span class="sxs-lookup"><span data-stu-id="6aed9-230">a.</span></span> <span data-ttu-id="6aed9-231">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-231">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6aed9-232">b.</span><span class="sxs-lookup"><span data-stu-id="6aed9-232">b.</span></span> <span data-ttu-id="6aed9-233">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="6aed9-233">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="6aed9-234">c.</span><span class="sxs-lookup"><span data-stu-id="6aed9-234">c.</span></span> <span data-ttu-id="6aed9-235">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="6aed9-235">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="6aed9-236">d.</span><span class="sxs-lookup"><span data-stu-id="6aed9-236">d.</span></span> <span data-ttu-id="6aed9-237">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-237">Click **Create**.</span></span>
 
### <a name="create-a-veracode-test-user"></a><span data-ttu-id="6aed9-238">Skapa en testanvändare Veracode</span><span class="sxs-lookup"><span data-stu-id="6aed9-238">Create a Veracode test user</span></span>
<span data-ttu-id="6aed9-239">För att aktivera Azure AD-användare att logga in på Veracode etableras de i Veracode.</span><span class="sxs-lookup"><span data-stu-id="6aed9-239">In order to enable Azure AD users to log into Veracode, they must be provisioned into Veracode.</span></span> <span data-ttu-id="6aed9-240">När det gäller Veracode är etablering en automatisk uppgift.</span><span class="sxs-lookup"><span data-stu-id="6aed9-240">In the case of Veracode, provisioning is an automated task.</span></span> <span data-ttu-id="6aed9-241">Det finns ingen åtgärd-objekt.</span><span class="sxs-lookup"><span data-stu-id="6aed9-241">There is no action item for you.</span></span> <span data-ttu-id="6aed9-242">Användare skapas automatiskt om det behövs under det första enkla inloggning försöket.</span><span class="sxs-lookup"><span data-stu-id="6aed9-242">Users are automatically created if necessary during the first single sign-on attempt.</span></span>

> [!NOTE]
> <span data-ttu-id="6aed9-243">Du kan använda något annat Veracode användarens konto skapas verktyg eller API: er som tillhandahålls av Veracode att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="6aed9-243">You can use any other Veracode user account creation tools or APIs provided by Veracode to provision Azure AD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="6aed9-244">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="6aed9-244">Assign the Azure AD test user</span></span>

<span data-ttu-id="6aed9-245">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Veracode.</span><span class="sxs-lookup"><span data-stu-id="6aed9-245">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Veracode.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="6aed9-247">**Om du vill tilldela Veracode Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6aed9-247">**To assign Britta Simon to Veracode, perform the following steps:**</span></span>

1. <span data-ttu-id="6aed9-248">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-248">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6aed9-250">Välj i listan med program **Veracode**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-250">In the applications list, select **Veracode**.</span></span>

    ![Länken Veracode i listan med program](./media/active-directory-saas-veracode-tutorial/tutorial_veracode_app.png)  

3. <span data-ttu-id="6aed9-252">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6aed9-252">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="6aed9-254">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6aed9-254">Click **Add** button.</span></span> <span data-ttu-id="6aed9-255">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aed9-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="6aed9-257">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="6aed9-257">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6aed9-258">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aed9-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6aed9-259">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6aed9-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="6aed9-260">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6aed9-260">Test single sign-on</span></span>

<span data-ttu-id="6aed9-261">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6aed9-261">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="6aed9-262">När du klickar på panelen Veracode på åtkomstpanelen du bör få automatiskt loggat in på ditt Veracode program.</span><span class="sxs-lookup"><span data-stu-id="6aed9-262">When you click the Veracode tile in the Access Panel, you should get automatically signed-on to your Veracode application.</span></span>
<span data-ttu-id="6aed9-263">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6aed9-263">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6aed9-264">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6aed9-264">Additional resources</span></span>

* [<span data-ttu-id="6aed9-265">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6aed9-265">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6aed9-266">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6aed9-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-veracode-tutorial/tutorial_general_203.png

