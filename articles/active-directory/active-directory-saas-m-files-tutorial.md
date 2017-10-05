---
title: "Självstudier: Azure Active Directory-integrering med M-filer | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och M-filer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 0f2682cf7cd3e11a5a7156938fbe9d4c7f541312
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a><span data-ttu-id="36bca-103">Självstudier: Azure Active Directory-integrering med M-filer</span><span class="sxs-lookup"><span data-stu-id="36bca-103">Tutorial: Azure Active Directory integration with M-Files</span></span>

<span data-ttu-id="36bca-104">I kursen får lära du att integrera M-filer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="36bca-104">In this tutorial, you learn how to integrate M-Files with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="36bca-105">Integrera M-filer med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="36bca-105">Integrating M-Files with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="36bca-106">Du kan styra i Azure AD som har åtkomst till M-filer</span><span class="sxs-lookup"><span data-stu-id="36bca-106">You can control in Azure AD who has access to M-Files</span></span>
- <span data-ttu-id="36bca-107">Du kan aktivera användarna att automatiskt hämta loggat in på M-filer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="36bca-107">You can enable your users to automatically get signed-on to M-Files (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="36bca-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="36bca-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="36bca-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="36bca-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36bca-110">Krav</span><span class="sxs-lookup"><span data-stu-id="36bca-110">Prerequisites</span></span>

<span data-ttu-id="36bca-111">För att konfigurera Azure AD-integrering med M-filer, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="36bca-111">To configure Azure AD integration with M-Files, you need the following items:</span></span>

- <span data-ttu-id="36bca-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="36bca-112">An Azure AD subscription</span></span>
- <span data-ttu-id="36bca-113">En M-filer enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="36bca-113">A M-Files single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="36bca-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="36bca-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="36bca-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="36bca-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="36bca-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="36bca-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="36bca-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="36bca-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="36bca-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="36bca-118">Scenario description</span></span>
<span data-ttu-id="36bca-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="36bca-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="36bca-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="36bca-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="36bca-121">Lägga till M-filer från galleriet</span><span class="sxs-lookup"><span data-stu-id="36bca-121">Adding M-Files from the gallery</span></span>
2. <span data-ttu-id="36bca-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bca-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-m-files-from-the-gallery"></a><span data-ttu-id="36bca-123">Lägga till M-filer från galleriet</span><span class="sxs-lookup"><span data-stu-id="36bca-123">Adding M-Files from the gallery</span></span>
<span data-ttu-id="36bca-124">Du måste lägga till M-filer från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av M-filer i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36bca-124">To configure the integration of M-Files into Azure AD, you need to add M-Files from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="36bca-125">**Utför följande steg för att lägga till M-filer från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="36bca-125">**To add M-Files from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="36bca-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="36bca-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="36bca-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="36bca-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="36bca-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="36bca-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="36bca-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bca-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="36bca-133">I sökrutan skriver **M-filer**.</span><span class="sxs-lookup"><span data-stu-id="36bca-133">In the search box, type **M-Files**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_search.png)

5. <span data-ttu-id="36bca-135">Välj i resultatpanelen **M filer**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="36bca-135">In the results panel, select **M-Files**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="36bca-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bca-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="36bca-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med M-filer baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="36bca-138">In this section, you configure and test Azure AD single sign-on with M-Files based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="36bca-139">Azure AD måste du känna till motsvarande användaren i M-filer till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="36bca-139">For single sign-on to work, Azure AD needs to know what the counterpart user in M-Files is to a user in Azure AD.</span></span> <span data-ttu-id="36bca-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i M-filer upprättas.</span><span class="sxs-lookup"><span data-stu-id="36bca-140">In other words, a link relationship between an Azure AD user and the related user in M-Files needs to be established.</span></span>

<span data-ttu-id="36bca-141">I M-filer, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="36bca-141">In M-Files, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="36bca-142">Om du vill konfigurera och testa Azure AD enkel inloggning med M-filer, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="36bca-142">To configure and test Azure AD single sign-on with M-Files, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="36bca-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="36bca-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="36bca-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36bca-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="36bca-145">**[Skapa en testanvändare M filer](#creating-a-m-files-test-user)**  – du har en motsvarighet för Britta Simon i M-filer som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="36bca-145">**[Creating a M-Files test user](#creating-a-m-files-test-user)** - to have a counterpart of Britta Simon in M-Files that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="36bca-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="36bca-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="36bca-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="36bca-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="36bca-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bca-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="36bca-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet M-filer.</span><span class="sxs-lookup"><span data-stu-id="36bca-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your M-Files application.</span></span>

<span data-ttu-id="36bca-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med M-filer:**</span><span class="sxs-lookup"><span data-stu-id="36bca-150">**To configure Azure AD single sign-on with M-Files, perform the following steps:**</span></span>

1. <span data-ttu-id="36bca-151">I Azure-portalen på den **M filer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="36bca-151">In the Azure portal, on the **M-Files** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="36bca-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="36bca-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_samlbase.png)

3. <span data-ttu-id="36bca-155">På den **M filer domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="36bca-155">On the **M-Files Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_url.png)

    <span data-ttu-id="36bca-157">a.</span><span class="sxs-lookup"><span data-stu-id="36bca-157">a.</span></span> <span data-ttu-id="36bca-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span><span class="sxs-lookup"><span data-stu-id="36bca-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`</span></span>

    <span data-ttu-id="36bca-159">b.</span><span class="sxs-lookup"><span data-stu-id="36bca-159">b.</span></span> <span data-ttu-id="36bca-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<tenantname>.cloudvault.m-files.com`</span><span class="sxs-lookup"><span data-stu-id="36bca-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenantname>.cloudvault.m-files.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="36bca-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="36bca-161">These values are not real.</span></span> <span data-ttu-id="36bca-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="36bca-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="36bca-163">Kontakta [M-filer som klienten supportteamet](mailto:support@m-files.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="36bca-163">Contact [M-Files Client support team](mailto:support@m-files.com) to get these values.</span></span> 
 
4. <span data-ttu-id="36bca-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="36bca-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_certificate.png) 

5. <span data-ttu-id="36bca-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="36bca-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="36bca-168">För att få SSO konfigurerats för ditt program, kontakta [M filer supportteam](mailto:support@m-files.com) och ge dem hämtade Metadata.</span><span class="sxs-lookup"><span data-stu-id="36bca-168">To get SSO configured for your application, contact [M-Files support team](mailto:support@m-files.com) and provide them the downloaded Metadata.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="36bca-169">Följ stegen nedan om du vill konfigurera enkel inloggning för dig miljon filen skrivbordsprogram.</span><span class="sxs-lookup"><span data-stu-id="36bca-169">Follow the next steps if you want to configure SSO for you M-File desktop application.</span></span> <span data-ttu-id="36bca-170">Inga ytterligare åtgärder krävs om du vill konfigurera enkel inloggning för webbversionen M-filer.</span><span class="sxs-lookup"><span data-stu-id="36bca-170">No extra steps are required if you only want to configure SSO for M-Files web version.</span></span>  

7. <span data-ttu-id="36bca-171">Följ stegen nedan om du vill konfigurera skrivbordsprogram M-filen för att aktivera enkel inloggning med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="36bca-171">Follow the next steps to configure the M-File desktop application to enable SSO with Azure AD.</span></span> <span data-ttu-id="36bca-172">Om du vill hämta M-filer, gå till [M filer hämta](https://www.m-files.com/en/download-latest-version) sidan.</span><span class="sxs-lookup"><span data-stu-id="36bca-172">To download M-Files, go to [M-Files download](https://www.m-files.com/en/download-latest-version) page.</span></span>

8. <span data-ttu-id="36bca-173">Öppna den **M filer Desktop inställningar** fönster.</span><span class="sxs-lookup"><span data-stu-id="36bca-173">Open the **M-Files Desktop Settings** window.</span></span> <span data-ttu-id="36bca-174">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="36bca-174">Then, click **Add**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)

9. <span data-ttu-id="36bca-176">På den **dokumentet valvet anslutningsegenskaper** fönster, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="36bca-176">On the **Document Vault Connection Properties** window, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  

    <span data-ttu-id="36bca-178">Under servern avsnittet typ, värdena på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="36bca-178">Under the Server section type, the values as follows:</span></span>  

    <span data-ttu-id="36bca-179">a.</span><span class="sxs-lookup"><span data-stu-id="36bca-179">a.</span></span> <span data-ttu-id="36bca-180">För **namn**, typen `<tenant-name>.cloudvault.m-files.com`.</span><span class="sxs-lookup"><span data-stu-id="36bca-180">For **Name**, type `<tenant-name>.cloudvault.m-files.com`.</span></span> 
 
    <span data-ttu-id="36bca-181">b.</span><span class="sxs-lookup"><span data-stu-id="36bca-181">b.</span></span> <span data-ttu-id="36bca-182">För **portnummer**, typen **4466**.</span><span class="sxs-lookup"><span data-stu-id="36bca-182">For **Port Number**, type **4466**.</span></span> 

    <span data-ttu-id="36bca-183">c.</span><span class="sxs-lookup"><span data-stu-id="36bca-183">c.</span></span> <span data-ttu-id="36bca-184">För **protokollet**väljer **HTTPS**.</span><span class="sxs-lookup"><span data-stu-id="36bca-184">For **Protocol**, select **HTTPS**.</span></span> 

    <span data-ttu-id="36bca-185">d.</span><span class="sxs-lookup"><span data-stu-id="36bca-185">d.</span></span> <span data-ttu-id="36bca-186">I den **autentisering** väljer **specifika Windows-användare**.</span><span class="sxs-lookup"><span data-stu-id="36bca-186">In the **Authentication** field, select **Specific Windows user**.</span></span> <span data-ttu-id="36bca-187">Du uppmanas sedan med en signering sida.</span><span class="sxs-lookup"><span data-stu-id="36bca-187">Then, you are prompted with a signing page.</span></span> <span data-ttu-id="36bca-188">Infoga Azure AD-autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="36bca-188">Insert your Azure AD credentials.</span></span> 

    <span data-ttu-id="36bca-189">e.</span><span class="sxs-lookup"><span data-stu-id="36bca-189">e.</span></span> <span data-ttu-id="36bca-190">För den **valvet på servern**, välj sedan motsvarande valv på servern.</span><span class="sxs-lookup"><span data-stu-id="36bca-190">For the **Vault on Server**,  select the corresponding vault on server.</span></span>
 
    <span data-ttu-id="36bca-191">f.</span><span class="sxs-lookup"><span data-stu-id="36bca-191">f.</span></span> <span data-ttu-id="36bca-192">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="36bca-192">Click **OK**.</span></span>

> [!TIP]
> <span data-ttu-id="36bca-193">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="36bca-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="36bca-194">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="36bca-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="36bca-195">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="36bca-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="36bca-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="36bca-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="36bca-197">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="36bca-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="36bca-199">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="36bca-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="36bca-200">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="36bca-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="36bca-202">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="36bca-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="36bca-204">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bca-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="36bca-206">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="36bca-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="36bca-208">a.</span><span class="sxs-lookup"><span data-stu-id="36bca-208">a.</span></span> <span data-ttu-id="36bca-209">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="36bca-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="36bca-210">b.</span><span class="sxs-lookup"><span data-stu-id="36bca-210">b.</span></span> <span data-ttu-id="36bca-211">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="36bca-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="36bca-212">c.</span><span class="sxs-lookup"><span data-stu-id="36bca-212">c.</span></span> <span data-ttu-id="36bca-213">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="36bca-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="36bca-214">d.</span><span class="sxs-lookup"><span data-stu-id="36bca-214">d.</span></span> <span data-ttu-id="36bca-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="36bca-215">Click **Create**.</span></span>
 
### <a name="creating-a-m-files-test-user"></a><span data-ttu-id="36bca-216">Skapa en testanvändare M-filer</span><span class="sxs-lookup"><span data-stu-id="36bca-216">Creating a M-Files test user</span></span>

<span data-ttu-id="36bca-217">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i M-filer.</span><span class="sxs-lookup"><span data-stu-id="36bca-217">The objective of this section is to create a user called Britta Simon in M-Files.</span></span> <span data-ttu-id="36bca-218">Arbeta med [M filer supportteam](mailto:support@m-files.com) att lägga till användare i M-filer.</span><span class="sxs-lookup"><span data-stu-id="36bca-218">Work with  [M-Files support team](mailto:support@m-files.com) to add the users in the M-Files.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="36bca-219">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="36bca-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="36bca-220">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till M-filer.</span><span class="sxs-lookup"><span data-stu-id="36bca-220">In this section, you enable Britta Simon to use Azure single sign-on by granting access to M-Files.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="36bca-222">**Om du vill tilldela Britta Simon M-filer, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="36bca-222">**To assign Britta Simon to M-Files, perform the following steps:**</span></span>

1. <span data-ttu-id="36bca-223">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="36bca-223">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="36bca-225">Välj i listan med program **M-filer**.</span><span class="sxs-lookup"><span data-stu-id="36bca-225">In the applications list, select **M-Files**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-m-files-tutorial/tutorial_m-files_app.png) 

3. <span data-ttu-id="36bca-227">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="36bca-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="36bca-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="36bca-229">Click **Add** button.</span></span> <span data-ttu-id="36bca-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bca-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="36bca-232">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="36bca-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="36bca-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bca-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="36bca-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="36bca-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="36bca-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="36bca-235">Testing single sign-on</span></span>

<span data-ttu-id="36bca-236">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="36bca-236">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="36bca-237">När du klickar på panelen M-filer på åtkomstpanelen du bör få automatiskt loggat in i tillämpningsprogrammet M-filer.</span><span class="sxs-lookup"><span data-stu-id="36bca-237">When you click the M-Files tile in the Access Panel, you should get automatically signed-on to your M-Files application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="36bca-238">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="36bca-238">Additional resources</span></span>

* [<span data-ttu-id="36bca-239">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="36bca-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="36bca-240">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="36bca-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png

