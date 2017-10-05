---
title: "Självstudier: Azure Active Directory-integrering med mark Gorilla klienten | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och mark Gorilla."
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
ms.date: 03/13/2017
ms.author: jeedes
ms.openlocfilehash: 744c420aa0298c59c44e645b95a716ad876752de
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-land-gorilla-client"></a><span data-ttu-id="cdd3f-103">Självstudier: Azure Active Directory-integrering med mark Gorilla klienten</span><span class="sxs-lookup"><span data-stu-id="cdd3f-103">Tutorial: Azure Active Directory integration with Land Gorilla Client</span></span>

<span data-ttu-id="cdd3f-104">I kursen får lära du att integrera mark Gorilla klienten med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cdd3f-104">In this tutorial, you learn how to integrate Land Gorilla Client with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cdd3f-105">Integrera mark Gorilla klienten med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-105">Integrating Land Gorilla Client with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cdd3f-106">Du kan styra i Azure AD som har åtkomst till mark Gorilla klienten</span><span class="sxs-lookup"><span data-stu-id="cdd3f-106">You can control in Azure AD who has access to Land Gorilla Client</span></span>
- <span data-ttu-id="cdd3f-107">Du kan aktivera användarna att automatiskt hämta loggat in på mark Gorilla klienten (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cdd3f-107">You can enable your users to automatically get signed-on to Land Gorilla Client (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cdd3f-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="cdd3f-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="cdd3f-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cdd3f-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="cdd3f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cdd3f-110">Prerequisites</span></span>

<span data-ttu-id="cdd3f-111">Om du vill konfigurera Azure AD-integrering med mark Gorilla klienten behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-111">To configure Azure AD integration with Land Gorilla Client, you need the following items:</span></span>

- <span data-ttu-id="cdd3f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cdd3f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cdd3f-113">En mark Gorilla klienten enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="cdd3f-113">A Land Gorilla Client single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="cdd3f-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="cdd3f-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cdd3f-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cdd3f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cdd3f-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="cdd3f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cdd3f-118">Scenario description</span></span>
<span data-ttu-id="cdd3f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cdd3f-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cdd3f-121">Lägger till mark Gorilla klient från galleriet</span><span class="sxs-lookup"><span data-stu-id="cdd3f-121">Adding Land Gorilla Client from the gallery</span></span>
2. <span data-ttu-id="cdd3f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cdd3f-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-land-gorilla-client-from-the-gallery"></a><span data-ttu-id="cdd3f-123">Lägger till mark Gorilla klient från galleriet</span><span class="sxs-lookup"><span data-stu-id="cdd3f-123">Adding Land Gorilla Client from the gallery</span></span>
<span data-ttu-id="cdd3f-124">Du måste lägga till mark Gorilla klienten från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av mark Gorilla klienten i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-124">To configure the integration of Land Gorilla Client into Azure AD, you need to add Land Gorilla Client from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cdd3f-125">**Utför följande steg för att lägga till mark Gorilla klienten från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cdd3f-125">**To add Land Gorilla Client from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cdd3f-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cdd3f-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cdd3f-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cdd3f-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cdd3f-133">I sökrutan skriver **mark Gorilla klienten**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-133">In the search box, type **Land Gorilla Client**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_search.png)

5. <span data-ttu-id="cdd3f-135">Välj i resultatpanelen **mark Gorilla klienten**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-135">In the results panel, select **Land Gorilla Client**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_addfromgallery.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cdd3f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cdd3f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cdd3f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med mark Gorilla klient med en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-138">In this section, you configure and test Azure AD single sign-on with Land Gorilla Client based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cdd3f-139">Azure AD måste du känna till motsvarande användaren i mark Gorilla klienten till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Land Gorilla Client is to a user in Azure AD.</span></span> <span data-ttu-id="cdd3f-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i mark Gorilla klienten upprättas.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-140">In other words, a link relationship between an Azure AD user and the related user in Land Gorilla Client needs to be established.</span></span>

<span data-ttu-id="cdd3f-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** mark Gorilla-klienten.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Land Gorilla Client.</span></span>

<span data-ttu-id="cdd3f-142">Om du vill konfigurera och testa Azure AD enkel inloggning med mark Gorilla klienten, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-142">To configure and test Azure AD single sign-on with Land Gorilla Client, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cdd3f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cdd3f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med en begränsad grupp.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with limited group.</span></span>
3. <span data-ttu-id="cdd3f-145">**[Skapa en testanvändare mark Gorilla](#creating-a-land-gorilla-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-145">**[Creating a Land Gorilla test user](#creating-a-land-gorilla-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="cdd3f-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cdd3f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cdd3f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cdd3f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cdd3f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt Land Gorilla klientprogram.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Land Gorilla Client application.</span></span>

<span data-ttu-id="cdd3f-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med mark Gorilla klienten:**</span><span class="sxs-lookup"><span data-stu-id="cdd3f-150">**To configure Azure AD single sign-on with Land Gorilla Client, perform the following steps:**</span></span>

1. <span data-ttu-id="cdd3f-151">I Azure-hanteringsportalen på den **mark Gorilla klienten** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-151">In the Azure Management portal, on the **Land Gorilla Client** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cdd3f-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_samlbase.png)

3. <span data-ttu-id="cdd3f-155">På den **mark Gorilla klienten domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-155">On the **Land Gorilla Client Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_url_02.png)

    <span data-ttu-id="cdd3f-157">a.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-157">a.</span></span> <span data-ttu-id="cdd3f-158">I den **identifierare** textruta Skriv värdet med någon av följande mönster:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-158">In the **Identifier** textbox, type the value using one of the following pattern:</span></span> 
    
    `https://<customer domain>.landgorilla.com/` 
    
    `https://www.<customer domain>.landgorilla.com`

    <span data-ttu-id="cdd3f-159">b.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-159">b.</span></span> <span data-ttu-id="cdd3f-160">I den **Reply URL** textruta, ange ett URL-Adressen med något av följande mönster:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-160">In the **Reply URL** textbox, type a URL using one of the following pattern:</span></span>

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/core/authenticate.php`

    `https://<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`
    
    `https://www.<customer domain>.landgorilla.com/simplesaml/module.php/saml/sp/saml2-acs.php/default-sp`

    > [!NOTE] 
    > <span data-ttu-id="cdd3f-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-161">Please note that these are not the real values.</span></span> <span data-ttu-id="cdd3f-162">Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="cdd3f-163">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="cdd3f-164">Kontakta [mark Gorilla klienten team](https://www.landgorilla.com/support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-164">Contact [Land Gorilla Client team](https://www.landgorilla.com/support/) to get these values.</span></span> 

4. <span data-ttu-id="cdd3f-165">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_certificate.png) 

5. <span data-ttu-id="cdd3f-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="cdd3f-169">För att få SSO konfigurationen klar för tillämpningsprogrammet mark Gorilla slutet, kontakta [mark Gorilla klienten supportteamet](https://www.landgorilla.com/support/) och ger dem den hämtade **”XML-Metadata för** fil.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-169">To get SSO configuration complete for your application at Land Gorilla end, Contact [Land Gorilla Client support team](https://www.landgorilla.com/support/) and provide them with the downloaded **“Metadata XML** file.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cdd3f-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cdd3f-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="cdd3f-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-171">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cdd3f-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cdd3f-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cdd3f-174">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-174">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cdd3f-176">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-176">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cdd3f-178">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-178">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cdd3f-180">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cdd3f-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-landgorilla-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cdd3f-182">a.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-182">a.</span></span> <span data-ttu-id="cdd3f-183">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cdd3f-184">b.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-184">b.</span></span> <span data-ttu-id="cdd3f-185">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cdd3f-186">c.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-186">c.</span></span> <span data-ttu-id="cdd3f-187">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cdd3f-188">d.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-188">d.</span></span> <span data-ttu-id="cdd3f-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-189">Click **Create**.</span></span> 

### <a name="creating-a-land-gorilla-test-user"></a><span data-ttu-id="cdd3f-190">Skapa en testanvändare mark Gorilla</span><span class="sxs-lookup"><span data-stu-id="cdd3f-190">Creating a Land Gorilla test user</span></span>

<span data-ttu-id="cdd3f-191">Se tillsammans med [mark Gorilla supportteamet](https://www.landgorilla.com/support/) att lägga till användare i mark Gorilla-plattformen.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-191">Please work with [Land Gorilla support team](https://www.landgorilla.com/support/) to add the users in the Land Gorilla platform.</span></span>
    
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cdd3f-192">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cdd3f-192">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cdd3f-193">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till mark Gorilla klienten.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-193">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Land Gorilla Client.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cdd3f-195">**Om du vill tilldela mark Gorilla klienten Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cdd3f-195">**To assign Britta Simon to Land Gorilla Client, perform the following steps:**</span></span>

1. <span data-ttu-id="cdd3f-196">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-196">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cdd3f-198">Välj i listan med program **mark Gorilla klienten**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-198">In the applications list, select **Land Gorilla Client**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-landgorilla-tutorial/tutorial_landgorilla_app.png) 

3. <span data-ttu-id="cdd3f-200">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-200">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cdd3f-202">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-202">Click **Add** button.</span></span> <span data-ttu-id="cdd3f-203">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cdd3f-205">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-205">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cdd3f-206">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cdd3f-207">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="cdd3f-208">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cdd3f-208">Testing single sign-on</span></span>

<span data-ttu-id="cdd3f-209">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-209">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cdd3f-210">När du klickar på panelen mark Gorilla klienten på åtkomstpanelen du bör få automatiskt loggat in på ditt Land Gorilla klientprogram.</span><span class="sxs-lookup"><span data-stu-id="cdd3f-210">When you click the Land Gorilla Client tile in the Access Panel, you should get automatically signed-on to your Land Gorilla Client application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cdd3f-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cdd3f-211">Additional resources</span></span>

* [<span data-ttu-id="cdd3f-212">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cdd3f-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cdd3f-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cdd3f-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-landgorilla-tutorial/tutorial_general_203.png
