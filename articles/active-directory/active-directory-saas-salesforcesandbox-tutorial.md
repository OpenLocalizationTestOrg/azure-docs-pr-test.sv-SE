---
title: "Självstudier: Azure Active Directory-integrering med Salesforce Sandbox | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Salesforce Sandbox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 90e08b9cf2feb93de4877bec9734352949896dca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="86aba-103">Självstudier: Azure Active Directory-integrering med Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="86aba-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="86aba-104">I kursen får lära du att integrera Salesforce Sandbox med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="86aba-104">In this tutorial, you learn how to integrate Salesforce Sandbox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="86aba-105">Integrera Salesforce Sandbox med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="86aba-105">Integrating Salesforce Sandbox with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="86aba-106">Du kan styra i Azure AD som har åtkomst till Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="86aba-106">You can control in Azure AD who has access to Salesforce Sandbox</span></span>
- <span data-ttu-id="86aba-107">Du kan aktivera användarna att automatiskt hämta loggat in på Salesforce begränsat läge (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="86aba-107">You can enable your users to automatically get signed-on to Salesforce Sandbox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="86aba-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="86aba-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="86aba-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="86aba-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86aba-110">Krav</span><span class="sxs-lookup"><span data-stu-id="86aba-110">Prerequisites</span></span>

<span data-ttu-id="86aba-111">Om du vill konfigurera Azure AD-integrering med Salesforce Sandbox behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="86aba-111">To configure Azure AD integration with Salesforce Sandbox, you need the following items:</span></span>

- <span data-ttu-id="86aba-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="86aba-112">An Azure AD subscription</span></span>
- <span data-ttu-id="86aba-113">En Salesforce Sandbox enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="86aba-113">A Salesforce Sandbox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="86aba-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="86aba-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="86aba-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="86aba-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="86aba-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="86aba-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="86aba-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="86aba-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="86aba-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="86aba-118">Scenario description</span></span>
<span data-ttu-id="86aba-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="86aba-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="86aba-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="86aba-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="86aba-121">Att lägga till Salesforce Sandbox från galleriet</span><span class="sxs-lookup"><span data-stu-id="86aba-121">Adding Salesforce Sandbox from the gallery</span></span>
2. <span data-ttu-id="86aba-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86aba-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-sandbox-from-the-gallery"></a><span data-ttu-id="86aba-123">Att lägga till Salesforce Sandbox från galleriet</span><span class="sxs-lookup"><span data-stu-id="86aba-123">Adding Salesforce Sandbox from the gallery</span></span>
<span data-ttu-id="86aba-124">Du måste lägga till Salesforce Sandbox från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Salesforce Sandbox i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="86aba-124">To configure the integration of Salesforce Sandbox into Azure AD, you need to add Salesforce Sandbox from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="86aba-125">**Utför följande steg för att lägga till Salesforce Sandbox från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="86aba-125">**To add Salesforce Sandbox from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="86aba-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="86aba-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="86aba-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="86aba-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="86aba-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="86aba-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="86aba-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86aba-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="86aba-133">I sökrutan skriver **Salesforce Sandbox**.</span><span class="sxs-lookup"><span data-stu-id="86aba-133">In the search box, type **Salesforce Sandbox**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. <span data-ttu-id="86aba-135">Välj i resultatpanelen **Salesforce Sandbox**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="86aba-135">In the results panel, select **Salesforce Sandbox**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="86aba-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86aba-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="86aba-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Salesforce Sandbox baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="86aba-138">In this section, you configure and test Azure AD single sign-on with Salesforce Sandbox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="86aba-139">Azure AD måste du känna till användaren i Salesforce Sandbox motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="86aba-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Salesforce Sandbox is to a user in Azure AD.</span></span> <span data-ttu-id="86aba-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Salesforce Sandbox upprättas.</span><span class="sxs-lookup"><span data-stu-id="86aba-140">In other words, a link relationship between an Azure AD user and the related user in Salesforce Sandbox needs to be established.</span></span>

<span data-ttu-id="86aba-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="86aba-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Salesforce Sandbox.</span></span>

<span data-ttu-id="86aba-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Salesforce Sandbox, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="86aba-142">To configure and test Azure AD single sign-on with Salesforce Sandbox, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="86aba-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="86aba-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="86aba-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86aba-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="86aba-145">**[Skapa en testanvändare Salesforce Sandbox](#creating-a-salesforce-sandbox-test-user)**  – du har en motsvarighet för Britta Simon i Salesforce Sandbox som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="86aba-145">**[Creating a Salesforce Sandbox test user](#creating-a-salesforce-sandbox-test-user)** - to have a counterpart of Britta Simon in Salesforce Sandbox that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="86aba-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="86aba-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="86aba-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="86aba-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="86aba-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86aba-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="86aba-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Salesforce-Sandbox-program.</span><span class="sxs-lookup"><span data-stu-id="86aba-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Salesforce Sandbox application.</span></span>

<span data-ttu-id="86aba-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Salesforce Sandbox:**</span><span class="sxs-lookup"><span data-stu-id="86aba-150">**To configure Azure AD single sign-on with Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="86aba-151">I Azure-portalen på den **Salesforce Sandbox** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="86aba-151">In the Azure portal, on the **Salesforce Sandbox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="86aba-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="86aba-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. <span data-ttu-id="86aba-155">På den **Salesforce Sandbox domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="86aba-155">On the **Salesforce Sandbox Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     <span data-ttu-id="86aba-157">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="86aba-157">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<subdomain>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="86aba-158">Det här värdet är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="86aba-158">This value is not the real.</span></span> <span data-ttu-id="86aba-159">Uppdatera det här värdet med det faktiska inloggnings-URL. Kontakta [Salesforce Sandbox klienten supportteamet](https://help.salesforce.com/support) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="86aba-159">Update this value with the actual Sign-on URL.Contact [Salesforce Sandbox Client support team](https://help.salesforce.com/support) to get this value.</span></span>


4. <span data-ttu-id="86aba-160">Om du redan har konfigurerat enkel inloggning för en annan Salesforce Sandbox-instans i katalogen, så måste du även konfigurera den **identifierare** ska ha samma värde som den **inloggning URL**.</span><span class="sxs-lookup"><span data-stu-id="86aba-160">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure the **Identifier** to have the same value as the **Sign on URL**.</span></span> 
    
    >[!Note]
    ><span data-ttu-id="86aba-161">Den **identifierare** fältet finns genom att kontrollera den **visa avancerade inställningar** kryssruta på den **konfigurera App-URL** sidan av dialogrutan</span><span class="sxs-lookup"><span data-stu-id="86aba-161">The **Identifier** field can be found by checking the **Show advanced settings** checkbox on the **Configure App URL** page of the dialog</span></span> 


5. <span data-ttu-id="86aba-162">På den **SAML-signeringscertifikat** klickar du på **certifikat** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="86aba-162">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. <span data-ttu-id="86aba-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="86aba-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="86aba-166">På den **Salesforce Sandbox Configuration** klickar du på **konfigurera Salesforce Sandbox** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="86aba-166">On the **Salesforce Sandbox Configuration** section, click **Configure Salesforce Sandbox** to open **Configure sign-on** window.</span></span> <span data-ttu-id="86aba-167">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="86aba-167">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    <span data-ttu-id="86aba-168">![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="86aba-168">![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span></span>
8. <span data-ttu-id="86aba-169">Öppna en ny flik i webbläsaren och logga in på ditt Salesforce-Sandbox-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="86aba-169">Open a new tab in your browser and log in to your Salesforce Sandbox administrator account.</span></span>

9. <span data-ttu-id="86aba-170">Klicka på menyn högst upp **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="86aba-170">In the menu on the top, click **Setup**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. <span data-ttu-id="86aba-172">I navigeringsfönstret till vänster klickar du på **säkerhetsåtgärder**, och klicka sedan på **inställningar för enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="86aba-172">In the navigation pane on the left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. <span data-ttu-id="86aba-174">Utför följande steg i avsnittet Inställningar för enkel inloggning: ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span><span class="sxs-lookup"><span data-stu-id="86aba-174">On the Single Sign-On Settings section, perform the following steps:  ![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span></span>
     
     <span data-ttu-id="86aba-175">a.</span><span class="sxs-lookup"><span data-stu-id="86aba-175">a.</span></span>  <span data-ttu-id="86aba-176">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="86aba-176">Select **SAML Enabled**.</span></span> 

     <span data-ttu-id="86aba-177">b.</span><span class="sxs-lookup"><span data-stu-id="86aba-177">b.</span></span>  <span data-ttu-id="86aba-178">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="86aba-178">Click **New**.</span></span>

12. <span data-ttu-id="86aba-179">Utför följande steg på avsnittet SAML enkel inloggning inställningar:</span><span class="sxs-lookup"><span data-stu-id="86aba-179">On the SAML Single Sign-On Settings section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    <span data-ttu-id="86aba-181">a.In textrutan namn skriver du namnet på konfigurationen (t.ex.: *SPSSOWAAD\_Test*).</span><span class="sxs-lookup"><span data-stu-id="86aba-181">a.In the Name textbox, type the name of the configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 

    <span data-ttu-id="86aba-182">b.</span><span class="sxs-lookup"><span data-stu-id="86aba-182">b.</span></span> <span data-ttu-id="86aba-183">Klistra in **SMAL enhets-ID** värde i den **utfärdaren** textruta.</span><span class="sxs-lookup"><span data-stu-id="86aba-183">Paste **SMAL Entity ID** value into the **Issuer** textbox.</span></span>

    <span data-ttu-id="86aba-184">c.</span><span class="sxs-lookup"><span data-stu-id="86aba-184">c.</span></span> <span data-ttu-id="86aba-185">I den **enhets-Id** textruta typen **https://test.salesforce.com** om det är den första Salesforce Sandbox-instans som du lägger till din katalog.</span><span class="sxs-lookup"><span data-stu-id="86aba-185">In the **Entity Id** textbox, type **https://test.salesforce.com** if it is the first Salesforce Sandbox instance that you are adding to your directory.</span></span> <span data-ttu-id="86aba-186">Om du redan har lagt till en instans av Salesforce Sandbox sedan för den **enhets-ID** Skriv i den **logga URL**, som ska vara i formatet:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="86aba-186">If you have already added an instance of Salesforce Sandbox, then for the **Entity ID** type in the **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>  
 
    <span data-ttu-id="86aba-187">d.</span><span class="sxs-lookup"><span data-stu-id="86aba-187">d.</span></span> <span data-ttu-id="86aba-188">Klicka på **Bläddra** att överföra hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="86aba-188">Click **Browse** to upload the downloaded certificate.</span></span>  

    <span data-ttu-id="86aba-189">e.</span><span class="sxs-lookup"><span data-stu-id="86aba-189">e.</span></span> <span data-ttu-id="86aba-190">Som **SAML identitetstyp**väljer **Assertion innehåller Federation-ID från användarobjektet**.</span><span class="sxs-lookup"><span data-stu-id="86aba-190">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span>
 
    <span data-ttu-id="86aba-191">f.</span><span class="sxs-lookup"><span data-stu-id="86aba-191">f.</span></span> <span data-ttu-id="86aba-192">Som **SAML identitet plats**väljer **identitet är i elementet NameIdentifier i instruktionen ämne**.</span><span class="sxs-lookup"><span data-stu-id="86aba-192">As **SAML Identity Location**, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>

    <span data-ttu-id="86aba-193">g.</span><span class="sxs-lookup"><span data-stu-id="86aba-193">g.</span></span> <span data-ttu-id="86aba-194">Klistra in **inloggning tjänst-URL för enkel** till den **identitet providern inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="86aba-194">Paste **Single Sign-On Service URL** into the **Identity Provider Login URL** textbox.</span></span> 

    <span data-ttu-id="86aba-195">h.</span><span class="sxs-lookup"><span data-stu-id="86aba-195">h.</span></span> <span data-ttu-id="86aba-196">SFDC har inte stöd för SAML logga ut.</span><span class="sxs-lookup"><span data-stu-id="86aba-196">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="86aba-197">Som en tillfällig lösning kan du klistra in 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' till den **identitet providern logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="86aba-197">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into the **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="86aba-198">Jag.</span><span class="sxs-lookup"><span data-stu-id="86aba-198">i.</span></span> <span data-ttu-id="86aba-199">Som **providern initierade begära Tjänstbindning**väljer **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="86aba-199">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 

    <span data-ttu-id="86aba-200">j.</span><span class="sxs-lookup"><span data-stu-id="86aba-200">j.</span></span> <span data-ttu-id="86aba-201">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="86aba-201">Click **Save**.</span></span>

### <a name="enable-your-domain"></a><span data-ttu-id="86aba-202">Aktivera din domän</span><span class="sxs-lookup"><span data-stu-id="86aba-202">Enable your domain</span></span>
<span data-ttu-id="86aba-203">Det här avsnittet förutsätter att du redan har skapat en domän.</span><span class="sxs-lookup"><span data-stu-id="86aba-203">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="86aba-204">Mer information finns i [definierar domännamn](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="86aba-204">For more information, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="86aba-205">**Utför följande steg om du vill aktivera din domän:**</span><span class="sxs-lookup"><span data-stu-id="86aba-205">**To enable your domain, perform the following steps:**</span></span>

1. <span data-ttu-id="86aba-206">I det vänstra navigeringsfönstret klickar du på **domänhantering**, och klicka sedan på **min domän.**</span><span class="sxs-lookup"><span data-stu-id="86aba-206">In the left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
     ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   ><span data-ttu-id="86aba-208">Kontrollera att din domän har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="86aba-208">Please make sure that your domain has been configured correctly.</span></span> 

2. <span data-ttu-id="86aba-209">I den **sidan inloggningsinställningar** klickar du på **redigera**, sedan som **Autentiseringstjänsten**, Välj namnet på SAML enkel inloggning inställningen från föregående avsnitt och klickar slutligen **spara**.</span><span class="sxs-lookup"><span data-stu-id="86aba-209">In the **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select the name of the SAML Single Sign-On Setting from the previous section, and finally click **Save**.</span></span>
   
   ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

<span data-ttu-id="86aba-211">Så snart du har en domän som har konfigurerats, bör användarna använda domän-URL för att logga in till Salesforce sandbox.</span><span class="sxs-lookup"><span data-stu-id="86aba-211">As soon as you have a domain configured, your users should use the domain URL to login to the Salesforce sandbox.</span></span>  

<span data-ttu-id="86aba-212">Om du vill hämta värdet för URL: en, klickar du på SSO-profil som du har skapat i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="86aba-212">To get the value of the URL, click the SSO profile you have created in the previous section.</span></span>    

> [!TIP]
> <span data-ttu-id="86aba-213">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="86aba-213">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="86aba-214">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="86aba-214">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="86aba-215">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="86aba-215">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="86aba-216">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="86aba-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="86aba-217">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="86aba-217">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="86aba-219">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="86aba-219">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="86aba-220">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="86aba-220">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="86aba-222">Om du vill visa en lista över användare går du till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="86aba-222">to display the list of users go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="86aba-224">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86aba-224">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="86aba-226">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="86aba-226">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="86aba-228">a.</span><span class="sxs-lookup"><span data-stu-id="86aba-228">a.</span></span> <span data-ttu-id="86aba-229">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="86aba-229">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="86aba-230">b.</span><span class="sxs-lookup"><span data-stu-id="86aba-230">b.</span></span> <span data-ttu-id="86aba-231">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="86aba-231">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="86aba-232">c.</span><span class="sxs-lookup"><span data-stu-id="86aba-232">c.</span></span> <span data-ttu-id="86aba-233">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="86aba-233">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="86aba-234">d.</span><span class="sxs-lookup"><span data-stu-id="86aba-234">d.</span></span> <span data-ttu-id="86aba-235">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="86aba-235">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-sandbox-test-user"></a><span data-ttu-id="86aba-236">Skapa en testanvändare Salesforce Sandbox</span><span class="sxs-lookup"><span data-stu-id="86aba-236">Creating a Salesforce Sandbox test user</span></span>

<span data-ttu-id="86aba-237">I det här avsnittet skapas en användare som kallas Britta Simon i Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="86aba-237">In this section, a user called Britta Simon is created in Salesforce Sandbox.</span></span> <span data-ttu-id="86aba-238">Salesforce Sandbox stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="86aba-238">Salesforce Sandbox supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="86aba-239">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="86aba-239">There is no action item for you in this section.</span></span> <span data-ttu-id="86aba-240">Om en användare inte redan finns i Salesforce Sandbox, skapas en ny när du försöker komma åt Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="86aba-240">If a user doesn't already exist in Salesforce Sandbox, a new one is created when you attempt to access Salesforce Sandbox.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="86aba-241">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="86aba-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="86aba-242">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Salesforce Sandbox.</span><span class="sxs-lookup"><span data-stu-id="86aba-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Salesforce Sandbox.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="86aba-244">**Om du vill tilldela Salesforce Sandbox Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="86aba-244">**To assign Britta Simon to Salesforce Sandbox, perform the following steps:**</span></span>

1. <span data-ttu-id="86aba-245">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="86aba-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="86aba-247">Välj i listan med program **Salesforce Sandbox**.</span><span class="sxs-lookup"><span data-stu-id="86aba-247">In the applications list, select **Salesforce Sandbox**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. <span data-ttu-id="86aba-249">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="86aba-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="86aba-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="86aba-251">Click **Add** button.</span></span> <span data-ttu-id="86aba-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86aba-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="86aba-254">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="86aba-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="86aba-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86aba-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="86aba-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="86aba-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="86aba-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="86aba-257">Testing single sign-on</span></span>

<span data-ttu-id="86aba-258">Om du vill testa inställningarna för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="86aba-258">If you want to test your SSO settings, open the Access Panel.</span></span> <span data-ttu-id="86aba-259">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="86aba-259">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="86aba-260">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="86aba-260">Additional resources</span></span>

* [<span data-ttu-id="86aba-261">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="86aba-261">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="86aba-262">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="86aba-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="86aba-263">Konfigurera Användaretablering</span><span class="sxs-lookup"><span data-stu-id="86aba-263">Configure User Provisioning</span></span>](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

