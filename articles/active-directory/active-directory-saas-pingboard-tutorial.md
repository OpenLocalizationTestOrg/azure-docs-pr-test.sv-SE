---
title: "Självstudier: Azure Active Directory-integrering med Pingboard | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Pingboard."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 008c670a8043da0c67ccefde48d5ef721c75d97c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a><span data-ttu-id="79607-103">Självstudier: Azure Active Directory-integrering med Pingboard</span><span class="sxs-lookup"><span data-stu-id="79607-103">Tutorial: Azure Active Directory integration with Pingboard</span></span>

<span data-ttu-id="79607-104">I kursen får lära du att integrera Pingboard med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="79607-104">In this tutorial, you learn how to integrate Pingboard with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="79607-105">Integrera Pingboard med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="79607-105">Integrating Pingboard with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="79607-106">Du kan styra i Azure AD som har åtkomst till Pingboard</span><span class="sxs-lookup"><span data-stu-id="79607-106">You can control in Azure AD who has access to Pingboard</span></span>
- <span data-ttu-id="79607-107">Du kan aktivera användarna att automatiskt hämta loggat in på Pingboard (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="79607-107">You can enable your users to automatically get signed-on to Pingboard (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="79607-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="79607-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="79607-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="79607-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79607-110">Krav</span><span class="sxs-lookup"><span data-stu-id="79607-110">Prerequisites</span></span>

<span data-ttu-id="79607-111">För att konfigurera Azure AD-integrering med Pingboard, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="79607-111">To configure Azure AD integration with Pingboard, you need the following items:</span></span>

- <span data-ttu-id="79607-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="79607-112">An Azure AD subscription</span></span>
- <span data-ttu-id="79607-113">En Pingboard enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="79607-113">A Pingboard single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="79607-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="79607-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="79607-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="79607-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="79607-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="79607-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="79607-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="79607-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="79607-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="79607-118">Scenario description</span></span>
<span data-ttu-id="79607-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="79607-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="79607-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="79607-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="79607-121">Att lägga till Pingboard från galleriet</span><span class="sxs-lookup"><span data-stu-id="79607-121">Adding Pingboard from the gallery</span></span>
2. <span data-ttu-id="79607-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79607-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pingboard-from-the-gallery"></a><span data-ttu-id="79607-123">Att lägga till Pingboard från galleriet</span><span class="sxs-lookup"><span data-stu-id="79607-123">Adding Pingboard from the gallery</span></span>
<span data-ttu-id="79607-124">Du måste lägga till Pingboard från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Pingboard i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="79607-124">To configure the integration of Pingboard into Azure AD, you need to add Pingboard from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="79607-125">**Utför följande steg för att lägga till Pingboard från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="79607-125">**To add Pingboard from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="79607-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="79607-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="79607-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="79607-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="79607-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="79607-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="79607-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79607-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="79607-133">I sökrutan skriver **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="79607-133">In the search box, type **Pingboard**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_search.png)

5. <span data-ttu-id="79607-135">Välj i resultatpanelen **Pingboard**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="79607-135">In the results panel, select **Pingboard**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="79607-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79607-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="79607-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Pingboard baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="79607-138">In this section, you configure and test Azure AD single sign-on with Pingboard based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="79607-139">Azure AD måste du känna till användaren i Pingboard motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="79607-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pingboard is to a user in Azure AD.</span></span> <span data-ttu-id="79607-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Pingboard upprättas.</span><span class="sxs-lookup"><span data-stu-id="79607-140">In other words, a link relationship between an Azure AD user and the related user in Pingboard needs to be established.</span></span>

<span data-ttu-id="79607-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Pingboard.</span><span class="sxs-lookup"><span data-stu-id="79607-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Pingboard.</span></span>

<span data-ttu-id="79607-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Pingboard, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="79607-142">To configure and test Azure AD single sign-on with Pingboard, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="79607-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="79607-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="79607-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79607-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="79607-145">**[Skapa en testanvändare Pingboard](#creating-a-pingboard-test-user)**  – du har en motsvarighet för Britta Simon i Pingboard som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="79607-145">**[Creating a Pingboard test user](#creating-a-pingboard-test-user)** - to have a counterpart of Britta Simon in Pingboard that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="79607-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="79607-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="79607-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="79607-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="79607-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79607-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="79607-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt Pingboard program.</span><span class="sxs-lookup"><span data-stu-id="79607-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Pingboard application.</span></span>

<span data-ttu-id="79607-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Pingboard:**</span><span class="sxs-lookup"><span data-stu-id="79607-150">**To configure Azure AD single sign-on with Pingboard, perform the following steps:**</span></span>

1. <span data-ttu-id="79607-151">I Azure-hanteringsportalen på den **Pingboard** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="79607-151">In the Azure Management portal, on the **Pingboard** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="79607-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="79607-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_samlbase.png)

3. <span data-ttu-id="79607-155">På den **Pingboard domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="79607-155">On the **Pingboard Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_url.png)

    <span data-ttu-id="79607-157">a.</span><span class="sxs-lookup"><span data-stu-id="79607-157">a.</span></span> <span data-ttu-id="79607-158">I den **identifierare** textruta Skriv värdet som:`http://<entity-id>.pingboard.com/sp`</span><span class="sxs-lookup"><span data-stu-id="79607-158">In the **Identifier** textbox, type the value as: `http://<entity-id>.pingboard.com/sp`</span></span>

    <span data-ttu-id="79607-159">b.</span><span class="sxs-lookup"><span data-stu-id="79607-159">b.</span></span> <span data-ttu-id="79607-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<entity-id>.pingboard.com/auth/saml/consume`</span><span class="sxs-lookup"><span data-stu-id="79607-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<entity-id>.pingboard.com/auth/saml/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="79607-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="79607-161">Please note that these are not the real values.</span></span> <span data-ttu-id="79607-162">Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="79607-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="79607-163">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="79607-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="79607-164">Kontakta [Pingboard klienten supportteamet](https://support.pingboard.com/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="79607-164">Contact [Pingboard Client support team](https://support.pingboard.com/) to get these values.</span></span> 

4. <span data-ttu-id="79607-165">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="79607-165">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_sp_initiated01.png)

    <span data-ttu-id="79607-167">a.</span><span class="sxs-lookup"><span data-stu-id="79607-167">a.</span></span> <span data-ttu-id="79607-168">I den **inloggnings-URL** textruta Skriv värdet som:`http://<sub-domain>.pingboard.com/sign_in`</span><span class="sxs-lookup"><span data-stu-id="79607-168">In the **Sign-on URL** textbox, type the value as: `http://<sub-domain>.pingboard.com/sign_in`</span></span>
     
5. <span data-ttu-id="79607-169">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="79607-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_certificate.png) 

6. <span data-ttu-id="79607-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="79607-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="79607-173">Konfigurera enkel inloggning på Pingboard sida, öppna ett nytt webbläsarfönster och logga in på ditt konto för Pingboard.</span><span class="sxs-lookup"><span data-stu-id="79607-173">To configure SSO on Pingboard side, open a new browser window and log in to your Pingboard Account.</span></span> <span data-ttu-id="79607-174">Du måste vara en Pingboard-administratör för att konfigurera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="79607-174">You must be a Pingboard admin to set up single sign on.</span></span>

8. <span data-ttu-id="79607-175">Välj den översta menyn **appar > integreringar**</span><span class="sxs-lookup"><span data-stu-id="79607-175">From the top menu select **Apps > Integrations**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_integration.png)

9.  <span data-ttu-id="79607-177">På den **integreringar** kan hitta den **”Azure Active Directory”** panelen och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="79607-177">On the **Integrations** page, find the **"Azure Active Directory"** tile, and click it.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_aad.png)

10. <span data-ttu-id="79607-179">I modal som följer Klicka **”konfigurera”**</span><span class="sxs-lookup"><span data-stu-id="79607-179">In the modal that follows click **"Configure"**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_configure.png)

11. <span data-ttu-id="79607-181">På följande sida ser du att ”Azure SSO-integrering är aktiverad”..</span><span class="sxs-lookup"><span data-stu-id="79607-181">On the following page, you will notice that "Azure SSO Integration is enabled.".</span></span> <span data-ttu-id="79607-182">Öppna den hämta filen i XML-Metadata i en anteckningar och klistra in innehållet i **IDP Metadata**.</span><span class="sxs-lookup"><span data-stu-id="79607-182">Open the downloaded Metadata XML file in a notepad and paste the content in **IDP Metadata**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/Pingboard_sso_configure.png)

12. <span data-ttu-id="79607-184">Filen kommer att valideras och om allt är korrekt, enkel inloggning på nu aktiveras</span><span class="sxs-lookup"><span data-stu-id="79607-184">The file will be validated, and if everything is correct, single sign on will now be enabled</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="79607-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="79607-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="79607-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79607-186">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="79607-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="79607-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="79607-189">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="79607-189">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="79607-191">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="79607-191">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="79607-193">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79607-193">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="79607-195">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="79607-195">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-pingboard-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="79607-197">a.</span><span class="sxs-lookup"><span data-stu-id="79607-197">a.</span></span> <span data-ttu-id="79607-198">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="79607-198">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="79607-199">b.</span><span class="sxs-lookup"><span data-stu-id="79607-199">b.</span></span> <span data-ttu-id="79607-200">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="79607-200">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="79607-201">c.</span><span class="sxs-lookup"><span data-stu-id="79607-201">c.</span></span> <span data-ttu-id="79607-202">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="79607-202">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="79607-203">d.</span><span class="sxs-lookup"><span data-stu-id="79607-203">d.</span></span> <span data-ttu-id="79607-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="79607-204">Click **Create**.</span></span>
 
### <a name="creating-a-pingboard-test-user"></a><span data-ttu-id="79607-205">Skapa en testanvändare Pingboard</span><span class="sxs-lookup"><span data-stu-id="79607-205">Creating a Pingboard test user</span></span>

<span data-ttu-id="79607-206">För att aktivera Azure AD-användare att logga in på Pingboard etableras de i Pingboard.</span><span class="sxs-lookup"><span data-stu-id="79607-206">In order to enable Azure AD users to log into Pingboard, they must be provisioned into Pingboard.</span></span>  
<span data-ttu-id="79607-207">När det gäller Pingboard är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="79607-207">In the case of Pingboard, provisioning is a manual task.</span></span>

<span data-ttu-id="79607-208">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="79607-208">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="79607-209">Logga in på webbplatsen Pingboard företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="79607-209">Log in to your Pingboard company site as an administrator.</span></span>

2. <span data-ttu-id="79607-210">Klicka på **”Lägg till medarbetare”** knappen på **Directory** sidan.</span><span class="sxs-lookup"><span data-stu-id="79607-210">Click **“Add Employee”** button on **Directory** page.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-pingboard-tutorial/create_testuser_add.png)

3. <span data-ttu-id="79607-212">På den **”Lägg till medarbetare”** dialogrutan utför följande steg.</span><span class="sxs-lookup"><span data-stu-id="79607-212">On the **“Add Employee”** dialog page, perform the following steps.</span></span>

    ![Bjud in personer](./media/active-directory-saas-pingboard-tutorial/create_testuser_name.png)

    <span data-ttu-id="79607-214">a.</span><span class="sxs-lookup"><span data-stu-id="79607-214">a.</span></span> <span data-ttu-id="79607-215">I den **fullständiga namn** textruta skriver du det fullständiga namnet på Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79607-215">In the **Full Name** textbox, type the full name of Britta Simon.</span></span>

    <span data-ttu-id="79607-216">b.</span><span class="sxs-lookup"><span data-stu-id="79607-216">b.</span></span> <span data-ttu-id="79607-217">I den **e-post** textruta skriver Britta Simon konto e-postadress.</span><span class="sxs-lookup"><span data-stu-id="79607-217">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="79607-218">c.</span><span class="sxs-lookup"><span data-stu-id="79607-218">c.</span></span> <span data-ttu-id="79607-219">I den **befattning** textruta skriver Britta Simon befattningen.</span><span class="sxs-lookup"><span data-stu-id="79607-219">In the **Job Title** textbox, type the job title of Britta Simon.</span></span>

    <span data-ttu-id="79607-220">d.</span><span class="sxs-lookup"><span data-stu-id="79607-220">d.</span></span> <span data-ttu-id="79607-221">I den **plats** listrutan, välj platsen för Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="79607-221">In the **Location** dropdown, select the location  of Britta Simon.</span></span>
    
    <span data-ttu-id="79607-222">e.</span><span class="sxs-lookup"><span data-stu-id="79607-222">e.</span></span> <span data-ttu-id="79607-223">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="79607-223">Click **Add**.</span></span>   

4. <span data-ttu-id="79607-224">En bekräftelseskärm kommer nu att bekräfta att lägga till användaren.</span><span class="sxs-lookup"><span data-stu-id="79607-224">A confirmation screen will come up to confirm the addition of user.</span></span>
    
    ![Bekräfta](./media/active-directory-saas-pingboard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > <span data-ttu-id="79607-226">Innehavaren för Azure Active Directory-konto ett e-postmeddelande och länken för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="79607-226">The Azure Active Directory account holder will receive an email and follow a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="79607-227">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="79607-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="79607-228">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Pingboard.</span><span class="sxs-lookup"><span data-stu-id="79607-228">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Pingboard.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="79607-230">**Om du vill tilldela Pingboard Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="79607-230">**To assign Britta Simon to Pingboard, perform the following steps:**</span></span>

1. <span data-ttu-id="79607-231">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="79607-231">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="79607-233">Välj i listan med program **Pingboard**.</span><span class="sxs-lookup"><span data-stu-id="79607-233">In the applications list, select **Pingboard**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-pingboard-tutorial/tutorial_pingboard_app.png) 

3. <span data-ttu-id="79607-235">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="79607-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="79607-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="79607-237">Click **Add** button.</span></span> <span data-ttu-id="79607-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79607-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="79607-240">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="79607-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="79607-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79607-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="79607-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79607-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="79607-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="79607-243">Testing single sign-on</span></span>

<span data-ttu-id="79607-244">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="79607-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="79607-245">När du klickar på panelen Pingboard på åtkomstpanelen du bör få automatiskt loggat in på ditt Pingboard program.</span><span class="sxs-lookup"><span data-stu-id="79607-245">When you click the Pingboard tile in the Access Panel, you should get automatically signed-on to your Pingboard application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79607-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="79607-246">Additional resources</span></span>

* [<span data-ttu-id="79607-247">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="79607-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="79607-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="79607-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pingboard-tutorial/tutorial_general_203.png
