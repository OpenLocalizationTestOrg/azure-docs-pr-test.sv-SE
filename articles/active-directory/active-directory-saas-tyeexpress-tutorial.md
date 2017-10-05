---
title: "Självstudier: Azure Active Directory-integrering med T & E Express | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och T & E Express."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
ms.openlocfilehash: 869e5284c71904fcc817ceee0f39d94fab1bc6f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a><span data-ttu-id="a027b-103">Självstudier: Azure Active Directory-integrering med T & E Express</span><span class="sxs-lookup"><span data-stu-id="a027b-103">Tutorial: Azure Active Directory integration with T&E Express</span></span>

<span data-ttu-id="a027b-104">I kursen får lära du att integrera T & E Express med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a027b-104">In this tutorial, you learn how to integrate T&E Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a027b-105">Integrera T & E Express med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a027b-105">Integrating T&E Express with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a027b-106">Du kan styra i Azure AD som har åtkomst till d & E Express</span><span class="sxs-lookup"><span data-stu-id="a027b-106">You can control in Azure AD who has access to T&E Express</span></span>
- <span data-ttu-id="a027b-107">Du kan aktivera användarna att automatiskt hämta inloggade & E Express (Single Sign-On) till med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a027b-107">You can enable your users to automatically get signed-on to T&E Express (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a027b-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="a027b-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="a027b-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a027b-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a027b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a027b-110">Prerequisites</span></span>

<span data-ttu-id="a027b-111">För att konfigurera Azure AD-integrering med T & E Express, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a027b-111">To configure Azure AD integration with T&E Express, you need the following items:</span></span>

- <span data-ttu-id="a027b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a027b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a027b-113">En T & E Express enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a027b-113">A T&E Express single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a027b-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a027b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a027b-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a027b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a027b-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a027b-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="a027b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a027b-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a027b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a027b-118">Scenario description</span></span>
<span data-ttu-id="a027b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a027b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a027b-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a027b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a027b-121">Att lägga till d & E Express från galleriet</span><span class="sxs-lookup"><span data-stu-id="a027b-121">Adding T&E Express from the gallery</span></span>
2. <span data-ttu-id="a027b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a027b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-te-express-from-the-gallery"></a><span data-ttu-id="a027b-123">Att lägga till d & E Express från galleriet</span><span class="sxs-lookup"><span data-stu-id="a027b-123">Adding T&E Express from the gallery</span></span>
<span data-ttu-id="a027b-124">Du måste lägga till d & E Express från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av d & E Express i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a027b-124">To configure the integration of T&E Express into Azure AD, you need to add T&E Express from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a027b-125">**Utför följande steg för att lägga till d & E Express från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a027b-125">**To add T&E Express from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a027b-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a027b-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a027b-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a027b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a027b-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a027b-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a027b-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a027b-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a027b-133">I sökrutan skriver **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="a027b-133">In the search box, type **T&E Express**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. <span data-ttu-id="a027b-135">Välj i resultatpanelen **T & E Express**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a027b-135">In the results panel, select **T&E Express**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a027b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a027b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a027b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med T & E Express baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a027b-138">In this section, you configure and test Azure AD single sign-on with T&E Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a027b-139">Azure AD måste du känna till användaren i T & E Express motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a027b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in T&E Express is to a user in Azure AD.</span></span> <span data-ttu-id="a027b-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i T & E Express upprättas.</span><span class="sxs-lookup"><span data-stu-id="a027b-140">In other words, a link relationship between an Azure AD user and the related user in T&E Express needs to be established.</span></span>

<span data-ttu-id="a027b-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i T & E Express.</span><span class="sxs-lookup"><span data-stu-id="a027b-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in T&E Express.</span></span>

<span data-ttu-id="a027b-142">Om du vill konfigurera och testa Azure AD enkel inloggning med T & E Express, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a027b-142">To configure and test Azure AD single sign-on with T&E Express, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a027b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a027b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a027b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a027b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a027b-145">**[Skapa en testanvändare T & E Express](#creating-a-te-express-test-user)**  – du har en motsvarighet för Britta Simon i T & E Express som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="a027b-145">**[Creating a T&E Express test user](#creating-a-te-express-test-user)** - to have a counterpart of Britta Simon in T&E Express that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="a027b-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a027b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a027b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a027b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a027b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a027b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a027b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i T & E Express-program.</span><span class="sxs-lookup"><span data-stu-id="a027b-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your T&E Express application.</span></span>

<span data-ttu-id="a027b-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med T & E Express:**</span><span class="sxs-lookup"><span data-stu-id="a027b-150">**To configure Azure AD single sign-on with T&E Express, perform the following steps:**</span></span>

1. <span data-ttu-id="a027b-151">I Azure-hanteringsportalen på den **T & E Express** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a027b-151">In the Azure Management portal, on the **T&E Express** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a027b-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="a027b-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. <span data-ttu-id="a027b-155">På den **& E Express domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a027b-155">On the **T&E Express Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    <span data-ttu-id="a027b-157">a.</span><span class="sxs-lookup"><span data-stu-id="a027b-157">a.</span></span> <span data-ttu-id="a027b-158">I den **identifierare** textruta Skriv värdet som:`https://<domain>.tyeexpress.com`</span><span class="sxs-lookup"><span data-stu-id="a027b-158">In the **Identifier** textbox, type the value as: `https://<domain>.tyeexpress.com`</span></span>

    <span data-ttu-id="a027b-159">b.</span><span class="sxs-lookup"><span data-stu-id="a027b-159">b.</span></span> <span data-ttu-id="a027b-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span><span class="sxs-lookup"><span data-stu-id="a027b-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a027b-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="a027b-161">Please note that these are not the real values.</span></span> <span data-ttu-id="a027b-162">Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="a027b-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a027b-163">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="a027b-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="a027b-164">Kontakta [T & E Express supportteam](http://www.tyeexpress.com/contacto.aspx) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a027b-164">Contact [T&E Express support team](http://www.tyeexpress.com/contacto.aspx) to get these values.</span></span>

5. <span data-ttu-id="a027b-165">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a027b-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. <span data-ttu-id="a027b-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a027b-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="a027b-169">Konfigurera enkel inloggning på **T & E snabb** sida, inloggning till T & E express program utan SAML för enkel inloggning på med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a027b-169">To configure single sign-on on **T&E Express** side, login to the T&E express application without SAML single sign on using admin credentials.</span></span>

9. <span data-ttu-id="a027b-170">Under den **Admin** klickar du på **SAML domän** att öppna inställningssidan SAML.</span><span class="sxs-lookup"><span data-stu-id="a027b-170">Under the **Admin** Tab, Click on **SAML domain** to Open the SAML settings page.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. <span data-ttu-id="a027b-172">Välj den **Activar(Activate)** alternativet från **nr** till **SI(Yes)**.</span><span class="sxs-lookup"><span data-stu-id="a027b-172">Select the **Activar(Activate)** option from **No** to **SI(Yes)**.</span></span> <span data-ttu-id="a027b-173">I den **identitet providern Metadata** textruta klistra in metadata XML som du har donwloaded från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a027b-173">In the **Identity Provider Metadata** textbox, paste the metadata XML which you have donwloaded from Azure portal.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. <span data-ttu-id="a027b-175">Klicka på den **Guardar(Save)** för att spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="a027b-175">Click on the **Guardar(Save)** button to save the settings.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a027b-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a027b-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="a027b-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a027b-177">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a027b-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a027b-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a027b-180">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a027b-180">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a027b-182">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="a027b-182">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a027b-184">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a027b-184">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a027b-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a027b-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a027b-188">a.</span><span class="sxs-lookup"><span data-stu-id="a027b-188">a.</span></span> <span data-ttu-id="a027b-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a027b-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a027b-190">b.</span><span class="sxs-lookup"><span data-stu-id="a027b-190">b.</span></span> <span data-ttu-id="a027b-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a027b-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a027b-192">c.</span><span class="sxs-lookup"><span data-stu-id="a027b-192">c.</span></span> <span data-ttu-id="a027b-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a027b-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a027b-194">d.</span><span class="sxs-lookup"><span data-stu-id="a027b-194">d.</span></span> <span data-ttu-id="a027b-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a027b-195">Click **Create**.</span></span>
 
### <a name="creating-a-te-express-test-user"></a><span data-ttu-id="a027b-196">Skapa en testanvändare T & E Express</span><span class="sxs-lookup"><span data-stu-id="a027b-196">Creating a T&E Express test user</span></span>

<span data-ttu-id="a027b-197">För att aktivera Azure AD-användare loggar in i T & E Express, måste de etableras i T & E Express.</span><span class="sxs-lookup"><span data-stu-id="a027b-197">In order to enable Azure AD users to log into T&E Express, they must be provisioned into T&E Express.</span></span>  
<span data-ttu-id="a027b-198">Vid T & E Express är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a027b-198">In case of T&E Express, provisioning is a manual task.</span></span>

<span data-ttu-id="a027b-199">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="a027b-199">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="a027b-200">Logga in på webbplatsen T & E Express företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="a027b-200">Log in to your T&E Express company site as an administrator.</span></span>

2. <span data-ttu-id="a027b-201">Klicka på användare för att öppna sidan som användare under Admin-taggen.</span><span class="sxs-lookup"><span data-stu-id="a027b-201">Under Admin tag, click on Users to open the Users master page.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. <span data-ttu-id="a027b-203">På sidan, klickar du på  **+**  att lägga till användarna.</span><span class="sxs-lookup"><span data-stu-id="a027b-203">On the home page, click on **+** to add the users.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. <span data-ttu-id="a027b-205">Ange alla obligatoriska uppgifter som frågan i formuläret och klicka på Spara för att spara informationen.</span><span class="sxs-lookup"><span data-stu-id="a027b-205">Enter all the mandatory details as asked in the form and click the save button to save the details.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Lägga till medarbetare](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a027b-208">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a027b-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a027b-209">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till d & E Express.</span><span class="sxs-lookup"><span data-stu-id="a027b-209">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to T&E Express.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a027b-211">**Utför följande steg om du vill tilldela Britta Simon & E Express till:**</span><span class="sxs-lookup"><span data-stu-id="a027b-211">**To assign Britta Simon to T&E Express, perform the following steps:**</span></span>

1. <span data-ttu-id="a027b-212">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a027b-212">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a027b-214">Välj i listan med program **T & E Express**.</span><span class="sxs-lookup"><span data-stu-id="a027b-214">In the applications list, select **T&E Express**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. <span data-ttu-id="a027b-216">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a027b-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a027b-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a027b-218">Click **Add** button.</span></span> <span data-ttu-id="a027b-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a027b-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a027b-221">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a027b-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a027b-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a027b-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a027b-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a027b-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a027b-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a027b-224">Testing single sign-on</span></span>

<span data-ttu-id="a027b-225">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a027b-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a027b-226">När du klickar på panelen T & E Express på åtkomstpanelen du bör få automatiskt loggat in på ditt d & E Express-program.</span><span class="sxs-lookup"><span data-stu-id="a027b-226">When you click the T&E Express tile in the Access Panel, you should get automatically signed-on to your T&E Express application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a027b-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a027b-227">Additional resources</span></span>

* [<span data-ttu-id="a027b-228">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a027b-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a027b-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a027b-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png

