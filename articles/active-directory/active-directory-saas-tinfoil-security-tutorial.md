---
title: "Självstudier: Azure Active Directory-integrering med TINFOIL SECURITY | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och TINFOIL SECURITY."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 614e4de3335574f4b56c7d641af4fcfafdb17d12
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a><span data-ttu-id="a76c1-103">Självstudier: Azure Active Directory-integrering med TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="a76c1-103">Tutorial: Azure Active Directory integration with TINFOIL SECURITY</span></span>

<span data-ttu-id="a76c1-104">I kursen får lära du att integrera TINFOIL SECURITY med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a76c1-104">In this tutorial, you learn how to integrate TINFOIL SECURITY with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a76c1-105">Integrera TINFOIL SECURITY med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a76c1-105">Integrating TINFOIL SECURITY with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a76c1-106">Du kan styra i Azure AD som har åtkomst till TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="a76c1-106">You can control in Azure AD who has access to TINFOIL SECURITY</span></span>
- <span data-ttu-id="a76c1-107">Du kan aktivera användarna att automatiskt hämta loggat in på TINFOIL SECURITY (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a76c1-107">You can enable your users to automatically get signed-on to TINFOIL SECURITY (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a76c1-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a76c1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a76c1-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a76c1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a76c1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a76c1-110">Prerequisites</span></span>

<span data-ttu-id="a76c1-111">För att konfigurera Azure AD-integrering med TINFOIL SECURITY, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a76c1-111">To configure Azure AD integration with TINFOIL SECURITY, you need the following items:</span></span>

- <span data-ttu-id="a76c1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a76c1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a76c1-113">En TINFOIL SECURITY enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a76c1-113">A TINFOIL SECURITY single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a76c1-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a76c1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a76c1-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a76c1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a76c1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a76c1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a76c1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a76c1-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a76c1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a76c1-118">Scenario description</span></span>
<span data-ttu-id="a76c1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a76c1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a76c1-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a76c1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a76c1-121">Lägg till TINFOIL SECURITY från galleriet</span><span class="sxs-lookup"><span data-stu-id="a76c1-121">Add TINFOIL SECURITY from the gallery</span></span>
2. <span data-ttu-id="a76c1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a76c1-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tinfoil-security-from-the-gallery"></a><span data-ttu-id="a76c1-123">Lägg till TINFOIL SECURITY från galleriet</span><span class="sxs-lookup"><span data-stu-id="a76c1-123">Add TINFOIL SECURITY from the gallery</span></span>
<span data-ttu-id="a76c1-124">Du måste lägga till TINFOIL SECURITY från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av TINFOIL SECURITY i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a76c1-124">To configure the integration of TINFOIL SECURITY into Azure AD, you need to add TINFOIL SECURITY from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a76c1-125">**Utför följande steg för att lägga till TINFOIL SECURITY från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a76c1-125">**To add TINFOIL SECURITY from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a76c1-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a76c1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a76c1-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a76c1-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a76c1-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a76c1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a76c1-133">I sökrutan skriver **TINFOIL SECURITY**väljer **TINFOIL SECURITY** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a76c1-133">In the search box, type **TINFOIL SECURITY**, select  **TINFOIL SECURITY** from result panel then click **Add** button to add the application.</span></span>

    ![TINFOIL SECURITY från galleriet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a76c1-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a76c1-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="a76c1-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TINFOIL SECURITY baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a76c1-136">In this section, you configure and test Azure AD single sign-on with TINFOIL SECURITY based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a76c1-137">Azure AD måste du känna till användaren i TINFOIL SECURITY motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a76c1-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TINFOIL SECURITY is to a user in Azure AD.</span></span> <span data-ttu-id="a76c1-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i TINFOIL SECURITY upprättas.</span><span class="sxs-lookup"><span data-stu-id="a76c1-138">In other words, a link relationship between an Azure AD user and the related user in TINFOIL SECURITY needs to be established.</span></span>

<span data-ttu-id="a76c1-139">I TINFOIL SECURITY tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a76c1-139">In TINFOIL SECURITY, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a76c1-140">Om du vill konfigurera och testa Azure AD enkel inloggning med TINFOIL SECURITY, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a76c1-140">To configure and test Azure AD single sign-on with TINFOIL SECURITY, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a76c1-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a76c1-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a76c1-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a76c1-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a76c1-143">**[Skapa en testanvändare TINFOIL SECURITY](#create-a-tinfoil-security-test-user)**  – har en motsvarighet för Britta Simon TINFOIL SECURITY som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a76c1-143">**[Create a TINFOIL SECURITY test user](#create-a-tinfoil-security-test-user)** - to have a counterpart of Britta Simon in TINFOIL SECURITY that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a76c1-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a76c1-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a76c1-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a76c1-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a76c1-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a76c1-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a76c1-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt TINFOIL SECURITY-program.</span><span class="sxs-lookup"><span data-stu-id="a76c1-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TINFOIL SECURITY application.</span></span>

<span data-ttu-id="a76c1-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med TINFOIL SECURITY:**</span><span class="sxs-lookup"><span data-stu-id="a76c1-148">**To configure Azure AD single sign-on with TINFOIL SECURITY, perform the following steps:**</span></span>

1. <span data-ttu-id="a76c1-149">I Azure-portalen på den **TINFOIL SECURITY** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-149">In the Azure portal, on the **TINFOIL SECURITY** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a76c1-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a76c1-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_samlbase.png)

3. <span data-ttu-id="a76c1-153">På den **TINFOIL SECURITY domän och URL: er** avsnittet användaren behöver inte utföra några steg som appen före redan är integrerad med Azure.</span><span class="sxs-lookup"><span data-stu-id="a76c1-153">On the **TINFOIL SECURITY Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_url.png)


4. <span data-ttu-id="a76c1-155">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värde.</span><span class="sxs-lookup"><span data-stu-id="a76c1-155">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_certificate.png) 

5. <span data-ttu-id="a76c1-157">Utför följande steg för att lägga till nödvändiga attributmappning:</span><span class="sxs-lookup"><span data-stu-id="a76c1-157">To add the required attribute mappings, perform the following steps:</span></span>
    
    <span data-ttu-id="a76c1-158">![Attribut](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="a76c1-158">![Attributes](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute1.png "Attributes")</span></span>
    
    | <span data-ttu-id="a76c1-159">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="a76c1-159">Attribute Name</span></span>    |   <span data-ttu-id="a76c1-160">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="a76c1-160">Attribute Value</span></span> |
    | ------------------- | -------------------- |
    | <span data-ttu-id="a76c1-161">accountid</span><span class="sxs-lookup"><span data-stu-id="a76c1-161">accountid</span></span> | <span data-ttu-id="a76c1-162">UXXXXXXXXXXXXX</span><span class="sxs-lookup"><span data-stu-id="a76c1-162">UXXXXXXXXXXXXX</span></span> |
    
    <span data-ttu-id="a76c1-163">a.</span><span class="sxs-lookup"><span data-stu-id="a76c1-163">a.</span></span> <span data-ttu-id="a76c1-164">Klicka på **lägga till användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-164">Click **add user attribute**.</span></span>
    
    <span data-ttu-id="a76c1-165">![Lägg till attributet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="a76c1-165">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_attribute.png "Attributes")</span></span>
    
    <span data-ttu-id="a76c1-166">![Lägg till attributet](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "attribut")</span><span class="sxs-lookup"><span data-stu-id="a76c1-166">![ADD Attribute](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_addatt.png "Attributes")</span></span>
    
    <span data-ttu-id="a76c1-167">b.</span><span class="sxs-lookup"><span data-stu-id="a76c1-167">b.</span></span> <span data-ttu-id="a76c1-168">I den **attributnamn** textruta typen **accountid**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-168">In the **Attribute Name** textbox, type **accountid**.</span></span>
    
    <span data-ttu-id="a76c1-169">c.</span><span class="sxs-lookup"><span data-stu-id="a76c1-169">c.</span></span> <span data-ttu-id="a76c1-170">I den **attributvärdet** textruta klistra in konto-ID-värde som du får senare på kursen.</span><span class="sxs-lookup"><span data-stu-id="a76c1-170">In the **Attribute Value** textbox, paste the account ID value which you will get later on the tutorial.</span></span>
    
    <span data-ttu-id="a76c1-171">d.</span><span class="sxs-lookup"><span data-stu-id="a76c1-171">d.</span></span> <span data-ttu-id="a76c1-172">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-172">Click **Ok**.</span></span>    

6. <span data-ttu-id="a76c1-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a76c1-173">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a76c1-175">På den **TINFOIL SECURITY Configuration** klickar du på **konfigurera TINFOIL SECURITY** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a76c1-175">On the **TINFOIL SECURITY Configuration** section, click **Configure TINFOIL SECURITY** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a76c1-176">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a76c1-176">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TINFOIL SECURITY Configuration](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_configure.png) 

8. <span data-ttu-id="a76c1-178">Logga in på webbplatsen TINFOIL SECURITY företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a76c1-178">In a different web browser window, log into your TINFOIL SECURITY company site as an administrator.</span></span>

9. <span data-ttu-id="a76c1-179">Klicka på i verktygsfältet högst upp **mitt konto**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-179">In the toolbar on the top, click **My Account**.</span></span>
   
    <span data-ttu-id="a76c1-180">![Instrumentpanelen](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "instrumentpanelen")</span><span class="sxs-lookup"><span data-stu-id="a76c1-180">![Dashboard](./media/active-directory-saas-tinfoil-security-tutorial/ic798971.png "Dashboard")</span></span>

10. <span data-ttu-id="a76c1-181">Klicka på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-181">Click **Security**.</span></span>
   
    <span data-ttu-id="a76c1-182">![Säkerhet](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="a76c1-182">![Security](./media/active-directory-saas-tinfoil-security-tutorial/ic798972.png "Security")</span></span>

11. <span data-ttu-id="a76c1-183">På den **enkel inloggning** konfigurationen utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a76c1-183">On the **Single Sign-On** configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="a76c1-184">![Enkel inloggning](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a76c1-184">![Single Sign-On](./media/active-directory-saas-tinfoil-security-tutorial/ic798973.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="a76c1-185">a.</span><span class="sxs-lookup"><span data-stu-id="a76c1-185">a.</span></span> <span data-ttu-id="a76c1-186">Välj **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-186">Select **Enable SAML**.</span></span>
   
    <span data-ttu-id="a76c1-187">b.</span><span class="sxs-lookup"><span data-stu-id="a76c1-187">b.</span></span> <span data-ttu-id="a76c1-188">Klicka på **manuell konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-188">Click **Manual Configuration**.</span></span>
   
    <span data-ttu-id="a76c1-189">c.</span><span class="sxs-lookup"><span data-stu-id="a76c1-189">c.</span></span> <span data-ttu-id="a76c1-190">I **SAML Post URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a76c1-190">In **SAML Post URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal</span></span>
   
    <span data-ttu-id="a76c1-191">d.</span><span class="sxs-lookup"><span data-stu-id="a76c1-191">d.</span></span> <span data-ttu-id="a76c1-192">I **SAML certifikat fingeravtryck** textruta klistra in värdet för **tumavtrycket** som du har kopierat från **SAML-signeringscertifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a76c1-192">In **SAML Certificate Fingerprint** textbox, paste the value of **Thumbprint** which you have copied from **SAML Signing Certificate** section.</span></span>
  
    <span data-ttu-id="a76c1-193">e.</span><span class="sxs-lookup"><span data-stu-id="a76c1-193">e.</span></span> <span data-ttu-id="a76c1-194">Kopiera **ditt konto-ID** värdet och klistrar in värdet i **attributvärdet** textruta under **lägga till attributet** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a76c1-194">Copy **Your Account ID** value and paste the value in **Attribute Value** textbox under **Add Attribute** section in Azure portal.</span></span>
   
    <span data-ttu-id="a76c1-195">f.</span><span class="sxs-lookup"><span data-stu-id="a76c1-195">f.</span></span> <span data-ttu-id="a76c1-196">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-196">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="a76c1-197">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="a76c1-197">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a76c1-198">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="a76c1-198">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a76c1-199">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a76c1-199">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a76c1-200">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a76c1-200">Create an Azure AD test user</span></span>
<span data-ttu-id="a76c1-201">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a76c1-201">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a76c1-203">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a76c1-203">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a76c1-204">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a76c1-204">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a76c1-206">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-206">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![<span data-ttu-id="a76c1-207">Användare och grupper -> alla användare</span><span class="sxs-lookup"><span data-stu-id="a76c1-207">Users and groups -> All users</span></span> ](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a76c1-208">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a76c1-208">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Användare](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a76c1-210">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a76c1-210">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tinfoil-security-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a76c1-212">a.</span><span class="sxs-lookup"><span data-stu-id="a76c1-212">a.</span></span> <span data-ttu-id="a76c1-213">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-213">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a76c1-214">b.</span><span class="sxs-lookup"><span data-stu-id="a76c1-214">b.</span></span> <span data-ttu-id="a76c1-215">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a76c1-215">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a76c1-216">c.</span><span class="sxs-lookup"><span data-stu-id="a76c1-216">c.</span></span> <span data-ttu-id="a76c1-217">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-217">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a76c1-218">d.</span><span class="sxs-lookup"><span data-stu-id="a76c1-218">d.</span></span> <span data-ttu-id="a76c1-219">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-219">Click **Create**.</span></span>
 
### <a name="create-a-tinfoil-security-test-user"></a><span data-ttu-id="a76c1-220">Skapa en testanvändare TINFOIL SECURITY</span><span class="sxs-lookup"><span data-stu-id="a76c1-220">Create a TINFOIL SECURITY test user</span></span>

<span data-ttu-id="a76c1-221">För att aktivera Azure AD-användare att logga in på TINFOIL SECURITY etableras de i TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="a76c1-221">In order to enable Azure AD users to log into TINFOIL SECURITY, they must be provisioned into TINFOIL SECURITY.</span></span> <span data-ttu-id="a76c1-222">TINFOIL SECURITY är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a76c1-222">In the case of TINFOIL SECURITY, provisioning is a manual task.</span></span>

<span data-ttu-id="a76c1-223">**Utför följande steg för att få en användare som har etablerats:**</span><span class="sxs-lookup"><span data-stu-id="a76c1-223">**To get a user provisioned, perform the following steps:**</span></span>

1. <span data-ttu-id="a76c1-224">Om användaren är en del av ett Enterprise-konto, behöver du [Kontakta supportteamet TINFOIL SECURITY](https://www.tinfoilsecurity.com/contact) att hämta det användarkonto som har skapats.</span><span class="sxs-lookup"><span data-stu-id="a76c1-224">If the user is a part of an Enterprise account, you need to [contact the TINFOIL SECURITY support team](https://www.tinfoilsecurity.com/contact) to get the user account created.</span></span>

2. <span data-ttu-id="a76c1-225">Om användaren är en vanlig TINFOIL SECURITY SaaS-användare, kan användaren lägga till en deltagare för användarens platser.</span><span class="sxs-lookup"><span data-stu-id="a76c1-225">If the user is a regular TINFOIL SECURITY SaaS user, then the user can add a collaborator to any of the user’s sites.</span></span> <span data-ttu-id="a76c1-226">Detta utlöser en process för att skicka en inbjudan till den angivna e-postadress för att skapa ett nytt användarkonto TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="a76c1-226">This triggers a process to send an invitation to the specified email to create a new TINFOIL SECURITY user account.</span></span>

> [!NOTE]
> <span data-ttu-id="a76c1-227">Du kan använda andra TINFOIL SECURITY användare skapa verktyg eller API: er som tillhandahålls av TINFOIL SECURITY för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a76c1-227">You can use any other TINFOIL SECURITY user account creation tools or APIs provided by TINFOIL SECURITY to provision Azure AD user accounts.</span></span>
> 
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a76c1-228">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a76c1-228">Assign the Azure AD test user</span></span>

<span data-ttu-id="a76c1-229">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till TINFOIL SECURITY.</span><span class="sxs-lookup"><span data-stu-id="a76c1-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TINFOIL SECURITY.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a76c1-231">**Om du vill tilldela TINFOIL SECURITY Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a76c1-231">**To assign Britta Simon to TINFOIL SECURITY, perform the following steps:**</span></span>

1. <span data-ttu-id="a76c1-232">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a76c1-234">Välj i listan med program **TINFOIL SECURITY**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-234">In the applications list, select **TINFOIL SECURITY**.</span></span>

    ![Välj TINFOIL SECURITY](./media/active-directory-saas-tinfoil-security-tutorial/tutorial_tinfoil-security_app.png) 

3. <span data-ttu-id="a76c1-236">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a76c1-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a76c1-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a76c1-238">Click **Add** button.</span></span> <span data-ttu-id="a76c1-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a76c1-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a76c1-241">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a76c1-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a76c1-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a76c1-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a76c1-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a76c1-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a76c1-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a76c1-244">Test single sign-on</span></span>

<span data-ttu-id="a76c1-245">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a76c1-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a76c1-246">När du klickar på panelen TINFOIL SECURITY på åtkomstpanelen du bör få automatiskt loggat in på ditt TINFOIL SECURITY-program.</span><span class="sxs-lookup"><span data-stu-id="a76c1-246">When you click the TINFOIL SECURITY tile in the Access Panel, you should get automatically signed-on to your TINFOIL SECURITY application.</span></span> <span data-ttu-id="a76c1-247">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a76c1-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a76c1-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a76c1-248">Additional resources</span></span>

* [<span data-ttu-id="a76c1-249">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a76c1-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a76c1-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a76c1-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tinfoil-security-tutorial/tutorial_general_203.png

