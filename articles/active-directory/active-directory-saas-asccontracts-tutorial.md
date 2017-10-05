---
title: "Självstudier: Azure Active Directory-integrering med kontrakt ASC | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ASC kontrakt."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7f54202-1581-4e55-a97e-02633ff9382d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: jeedes
ms.openlocfilehash: 87ea3cc55f9683e7d5b9912a87d675575cea0347
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asc-contracts"></a><span data-ttu-id="a715c-103">Självstudier: Azure Active Directory-integrering med ASC kontrakt</span><span class="sxs-lookup"><span data-stu-id="a715c-103">Tutorial: Azure Active Directory integration with ASC Contracts</span></span>

<span data-ttu-id="a715c-104">I kursen får lära du att integrera ASC kontrakt med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a715c-104">In this tutorial, you learn how to integrate ASC Contracts with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a715c-105">Integrera ASC kontrakt med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a715c-105">Integrating ASC Contracts with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a715c-106">Du kan styra i Azure AD som har åtkomst till ASC kontrakt</span><span class="sxs-lookup"><span data-stu-id="a715c-106">You can control in Azure AD who has access to ASC Contracts</span></span>
- <span data-ttu-id="a715c-107">Du kan aktivera användarna att automatiskt hämta loggat in på ASC kontrakt (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a715c-107">You can enable your users to automatically get signed-on to ASC Contracts (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a715c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a715c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a715c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a715c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a715c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a715c-110">Prerequisites</span></span>

<span data-ttu-id="a715c-111">För att konfigurera Azure AD-integrering med ASC kontrakt, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a715c-111">To configure Azure AD integration with ASC Contracts, you need the following items:</span></span>

- <span data-ttu-id="a715c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a715c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a715c-113">Ett kontrakt som ASC enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a715c-113">An ASC Contracts single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a715c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a715c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a715c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a715c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a715c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a715c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a715c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a715c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a715c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a715c-118">Scenario description</span></span>
<span data-ttu-id="a715c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a715c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a715c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a715c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a715c-121">Att lägga till ASC kontrakt från galleriet</span><span class="sxs-lookup"><span data-stu-id="a715c-121">Adding ASC Contracts from the gallery</span></span>
2. <span data-ttu-id="a715c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a715c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asc-contracts-from-the-gallery"></a><span data-ttu-id="a715c-123">Att lägga till ASC kontrakt från galleriet</span><span class="sxs-lookup"><span data-stu-id="a715c-123">Adding ASC Contracts from the gallery</span></span>
<span data-ttu-id="a715c-124">Du måste lägga till ASC kontrakt från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ASC kontrakt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a715c-124">To configure the integration of ASC Contracts into Azure AD, you need to add ASC Contracts from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a715c-125">**Utför följande steg för att lägga till ASC kontrakt från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a715c-125">**To add ASC Contracts from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a715c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a715c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a715c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a715c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a715c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a715c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a715c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a715c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a715c-133">I sökrutan skriver **ASC kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="a715c-133">In the search box, type **ASC Contracts**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_search.png)

5. <span data-ttu-id="a715c-135">Välj i resultatpanelen **ASC kontrakt**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a715c-135">In the results panel, select **ASC Contracts**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a715c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a715c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a715c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ASC kontrakt som bygger på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a715c-138">In this section, you configure and test Azure AD single sign-on with ASC Contracts based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a715c-139">Azure AD måste du känna till motsvarande användaren i ASC kontrakt till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a715c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ASC Contracts is to a user in Azure AD.</span></span> <span data-ttu-id="a715c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ASC kontrakt upprättas.</span><span class="sxs-lookup"><span data-stu-id="a715c-140">In other words, a link relationship between an Azure AD user and the related user in ASC Contracts needs to be established.</span></span>

<span data-ttu-id="a715c-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** ASC kontrakt.</span><span class="sxs-lookup"><span data-stu-id="a715c-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ASC Contracts.</span></span>

<span data-ttu-id="a715c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med ASC kontrakt, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a715c-142">To configure and test Azure AD single sign-on with ASC Contracts, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a715c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a715c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a715c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a715c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a715c-145">**[Skapa en testanvändare ASC kontrakt](#creating-an-asc-contracts-test-user)**  – du har en motsvarighet för Britta Simon ASC kontrakt som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a715c-145">**[Creating an ASC Contracts test user](#creating-an-asc-contracts-test-user)** - to have a counterpart of Britta Simon in ASC Contracts that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a715c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a715c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a715c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a715c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a715c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a715c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a715c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet ASC kontrakt.</span><span class="sxs-lookup"><span data-stu-id="a715c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ASC Contracts application.</span></span>

<span data-ttu-id="a715c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ASC kontrakt:**</span><span class="sxs-lookup"><span data-stu-id="a715c-150">**To configure Azure AD single sign-on with ASC Contracts, perform the following steps:**</span></span>

1. <span data-ttu-id="a715c-151">I Azure-portalen på den **ASC kontrakt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a715c-151">In the Azure portal, on the **ASC Contracts** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a715c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a715c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_samlbase.png)

3. <span data-ttu-id="a715c-155">På den **ASC kontrakt domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a715c-155">On the **ASC Contracts Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_url.png)

    <span data-ttu-id="a715c-157">a.</span><span class="sxs-lookup"><span data-stu-id="a715c-157">a.</span></span> <span data-ttu-id="a715c-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.asccontracts.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="a715c-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.asccontracts.com/shibboleth`</span></span>

    <span data-ttu-id="a715c-159">b.</span><span class="sxs-lookup"><span data-stu-id="a715c-159">b.</span></span> <span data-ttu-id="a715c-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span><span class="sxs-lookup"><span data-stu-id="a715c-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.asccontracts.com/shibboleth.sso/login`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a715c-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="a715c-161">These values are not real.</span></span> <span data-ttu-id="a715c-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="a715c-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="a715c-163">Kontakta ASC nätverk Inc. (ASC)-teamet på **613.599.6178** att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="a715c-163">Contact ASC Networks Inc. (ASC) team at **613.599.6178** to get these values.</span></span>

4. <span data-ttu-id="a715c-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a715c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_certificate.png) 

5. <span data-ttu-id="a715c-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a715c-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a715c-168">Konfigurera enkel inloggning på **ASC kontrakt** sida, anropa ASC nätverk Inc. (ASC) support på **613.599.6178** och ger dem den hämtade **XML-Metadata för**.</span><span class="sxs-lookup"><span data-stu-id="a715c-168">To configure single sign-on on **ASC Contracts** side, call ASC Networks Inc. (ASC) support at **613.599.6178** and provide them with the downloaded **Metadata XML**.</span></span> <span data-ttu-id="a715c-169">De konfigurera programmet så att SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="a715c-169">They set this application up to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="a715c-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="a715c-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a715c-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="a715c-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a715c-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a715c-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a715c-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a715c-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="a715c-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a715c-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a715c-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a715c-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a715c-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a715c-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a715c-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a715c-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a715c-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a715c-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a715c-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a715c-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asccontracts-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a715c-185">a.</span><span class="sxs-lookup"><span data-stu-id="a715c-185">a.</span></span> <span data-ttu-id="a715c-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a715c-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a715c-187">b.</span><span class="sxs-lookup"><span data-stu-id="a715c-187">b.</span></span> <span data-ttu-id="a715c-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a715c-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a715c-189">c.</span><span class="sxs-lookup"><span data-stu-id="a715c-189">c.</span></span> <span data-ttu-id="a715c-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a715c-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a715c-191">d.</span><span class="sxs-lookup"><span data-stu-id="a715c-191">d.</span></span> <span data-ttu-id="a715c-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a715c-192">Click **Create**.</span></span>
 
### <a name="creating-an-asc-contracts-test-user"></a><span data-ttu-id="a715c-193">Skapa en testanvändare ASC kontrakt</span><span class="sxs-lookup"><span data-stu-id="a715c-193">Creating an ASC Contracts test user</span></span>

<span data-ttu-id="a715c-194">Arbeta med ASC nätverk Inc. (ASC) support på **613.599.6178** att hämta de användare som lagts till i ASC kontrakt-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a715c-194">Work with ASC Networks Inc. (ASC) support team at **613.599.6178** to get the users added in the ASC Contracts platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a715c-195">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a715c-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a715c-196">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ASC kontrakt.</span><span class="sxs-lookup"><span data-stu-id="a715c-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ASC Contracts.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a715c-198">**Om du vill tilldela ASC kontrakt Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a715c-198">**To assign Britta Simon to ASC Contracts, perform the following steps:**</span></span>

1. <span data-ttu-id="a715c-199">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a715c-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a715c-201">Välj i listan med program **ASC kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="a715c-201">In the applications list, select **ASC Contracts**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-asccontracts-tutorial/tutorial_asccontracts_app.png) 

3. <span data-ttu-id="a715c-203">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a715c-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a715c-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a715c-205">Click **Add** button.</span></span> <span data-ttu-id="a715c-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a715c-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a715c-208">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a715c-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a715c-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a715c-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a715c-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a715c-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a715c-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a715c-211">Testing single sign-on</span></span>

<span data-ttu-id="a715c-212">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a715c-212">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a715c-213">När du klickar på panelen ASC kontrakt på åtkomstpanelen du ska hämta automatiskt loggat in på ditt ASC kontrakt-program.</span><span class="sxs-lookup"><span data-stu-id="a715c-213">When you click the ASC Contracts tile in the Access Panel, you should get automatically signed-on to your ASC Contracts application.</span></span> <span data-ttu-id="a715c-214">Mer information om åtkomstpanelen finns i.</span><span class="sxs-lookup"><span data-stu-id="a715c-214">For more information about the Access Panel, see.</span></span> <span data-ttu-id="a715c-215">[Introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="a715c-215">[Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a715c-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a715c-216">Additional resources</span></span>

* [<span data-ttu-id="a715c-217">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a715c-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a715c-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a715c-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asccontracts-tutorial/tutorial_general_203.png

