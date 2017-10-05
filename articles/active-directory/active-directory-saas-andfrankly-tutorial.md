---
title: "Självstudier: Azure Active Directory-integrering med & även | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och & även."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d702060-1b89-4e9d-9f01-ede4f1171c73
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: ea18a9f9bff258337a3de6d7703b4c548efa37df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-frankly"></a><span data-ttu-id="d0767-103">Självstudier: Azure Active Directory-integrering med & även</span><span class="sxs-lookup"><span data-stu-id="d0767-103">Tutorial: Azure Active Directory integration with &frankly</span></span>

<span data-ttu-id="d0767-104">I kursen får du lära dig hur du integrerar & även med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d0767-104">In this tutorial, you learn how to integrate &frankly with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d0767-105">Integrera & även med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d0767-105">Integrating &frankly with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d0767-106">Du kan styra i Azure AD som har åtkomst till & även</span><span class="sxs-lookup"><span data-stu-id="d0767-106">You can control in Azure AD who has access to &frankly</span></span>
- <span data-ttu-id="d0767-107">Du kan ge användarna automatiskt får inloggade till & även (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d0767-107">You can enable your users to automatically get signed-on to &frankly (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d0767-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d0767-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d0767-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d0767-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0767-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d0767-110">Prerequisites</span></span>

<span data-ttu-id="d0767-111">Om du vill konfigurera Azure AD behöver-integrering med & även, du du följande:</span><span class="sxs-lookup"><span data-stu-id="d0767-111">To configure Azure AD integration with &frankly, you need the following items:</span></span>

- <span data-ttu-id="d0767-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d0767-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d0767-113">A & även enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d0767-113">A &frankly single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d0767-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d0767-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d0767-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d0767-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d0767-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d0767-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d0767-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0767-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d0767-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d0767-118">Scenario description</span></span>
<span data-ttu-id="d0767-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d0767-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d0767-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d0767-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d0767-121">Lägga till & även från galleriet</span><span class="sxs-lookup"><span data-stu-id="d0767-121">Adding &frankly from the gallery</span></span>
2. <span data-ttu-id="d0767-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0767-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-frankly-from-the-gallery"></a><span data-ttu-id="d0767-123">Lägga till & även från galleriet</span><span class="sxs-lookup"><span data-stu-id="d0767-123">Adding &frankly from the gallery</span></span>
<span data-ttu-id="d0767-124">För att konfigurera integrering av & även till Azure AD, måste du lägga till & även från galleriet så att din lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d0767-124">To configure the integration of &frankly into Azure AD, you need to add &frankly from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d0767-125">**Lägg till & även från galleriet, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d0767-125">**To add &frankly from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d0767-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d0767-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d0767-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d0767-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d0767-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d0767-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d0767-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0767-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d0767-133">I sökrutan skriver **& även**.</span><span class="sxs-lookup"><span data-stu-id="d0767-133">In the search box, type **&frankly**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_search.png)

5. <span data-ttu-id="d0767-135">Välj i resultatpanelen **& även**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d0767-135">In the results panel, select **&frankly**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d0767-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0767-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d0767-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med & även baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d0767-138">In this section, you configure and test Azure AD single sign-on with &frankly based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d0767-139">För enkel inloggning ska fungera, Azure AD måste veta vilka motsvarande användaren i & även är att en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d0767-139">For single sign-on to work, Azure AD needs to know what the counterpart user in &frankly is to a user in Azure AD.</span></span> <span data-ttu-id="d0767-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i & även upprättas.</span><span class="sxs-lookup"><span data-stu-id="d0767-140">In other words, a link relationship between an Azure AD user and the related user in &frankly needs to be established.</span></span>

<span data-ttu-id="d0767-141">I & även, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d0767-141">In &frankly, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d0767-142">Om du vill konfigurera och testa Azure AD måste enkel inloggning med & även, du slutföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d0767-142">To configure and test Azure AD single sign-on with &frankly, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d0767-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d0767-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d0767-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d0767-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d0767-145">**[Skapa en & även testanvändare](#creating-a-frankly-test-user)**  – du har en motsvarighet för Britta Simon i & även som är länkad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d0767-145">**[Creating a &frankly test user](#creating-a-frankly-test-user)** - to have a counterpart of Britta Simon in &frankly that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d0767-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0767-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d0767-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d0767-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d0767-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0767-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d0767-149">I det här avsnittet kan du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din & även programmet.</span><span class="sxs-lookup"><span data-stu-id="d0767-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your &frankly application.</span></span>

<span data-ttu-id="d0767-150">**För att konfigurera Azure AD enkel inloggning med & även, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d0767-150">**To configure Azure AD single sign-on with &frankly, perform the following steps:**</span></span>

1. <span data-ttu-id="d0767-151">I Azure-portalen på den **& även** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d0767-151">In the Azure portal, on the **&frankly** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d0767-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d0767-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_samlbase.png)

3. <span data-ttu-id="d0767-155">På den **& även URL: er och domänen** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="d0767-155">On the **&frankly Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_url.png)

    <span data-ttu-id="d0767-157">a.</span><span class="sxs-lookup"><span data-stu-id="d0767-157">a.</span></span> <span data-ttu-id="d0767-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="d0767-158">In the **Identifier** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`</span></span>

    <span data-ttu-id="d0767-159">b.</span><span class="sxs-lookup"><span data-stu-id="d0767-159">b.</span></span> <span data-ttu-id="d0767-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="d0767-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`</span></span>

4. <span data-ttu-id="d0767-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="d0767-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="d0767-162">Om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="d0767-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_url1.png)

    <span data-ttu-id="d0767-164">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="d0767-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`</span></span>
    > [!NOTE] 
    > <span data-ttu-id="d0767-165">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d0767-165">These values are not real.</span></span> <span data-ttu-id="d0767-166">Uppdatera dessa värden med den faktiska identifieraren inloggning och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d0767-166">Update these values with the actual Identifier, Sign-on, and Reply URL.</span></span> <span data-ttu-id="d0767-167">Kontakta [andfrankly supportteamet](mailto:help@andfrankly.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d0767-167">Contact [andfrankly support team](mailto:help@andfrankly.com) to get these values.</span></span>

5. <span data-ttu-id="d0767-168">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d0767-168">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_certificate.png) 

6. <span data-ttu-id="d0767-170">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d0767-170">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-andfrankly-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="d0767-172">Konfigurera enkel inloggning på **& även** sida, måste du skicka den hämtade **XML-Metadata för** till [andfrankly supportteamet](mailto:help@andfrankly.com).</span><span class="sxs-lookup"><span data-stu-id="d0767-172">To configure single sign-on on **&frankly** side, you need to send the downloaded **Metadata XML** to [andfrankly support team](mailto:help@andfrankly.com).</span></span> 

> [!TIP]
> <span data-ttu-id="d0767-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d0767-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d0767-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d0767-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d0767-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d0767-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d0767-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d0767-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="d0767-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d0767-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d0767-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d0767-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d0767-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d0767-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d0767-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d0767-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d0767-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0767-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d0767-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d0767-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d0767-188">a.</span><span class="sxs-lookup"><span data-stu-id="d0767-188">a.</span></span> <span data-ttu-id="d0767-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d0767-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d0767-190">b.</span><span class="sxs-lookup"><span data-stu-id="d0767-190">b.</span></span> <span data-ttu-id="d0767-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d0767-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d0767-192">c.</span><span class="sxs-lookup"><span data-stu-id="d0767-192">c.</span></span> <span data-ttu-id="d0767-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d0767-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d0767-194">d.</span><span class="sxs-lookup"><span data-stu-id="d0767-194">d.</span></span> <span data-ttu-id="d0767-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d0767-195">Click **Create**.</span></span>
 
### <a name="creating-a-frankly-test-user"></a><span data-ttu-id="d0767-196">Skapa en & även testanvändare</span><span class="sxs-lookup"><span data-stu-id="d0767-196">Creating a &frankly test user</span></span>

<span data-ttu-id="d0767-197">I det här avsnittet skapar du en användare som kallas Britta Simon i & även.</span><span class="sxs-lookup"><span data-stu-id="d0767-197">In this section, you create a user called Britta Simon in &frankly.</span></span> <span data-ttu-id="d0767-198">Arbeta med [andfrankly supportteamet](mailto:help@andfrankly.com) att lägga till användare i & även plattform.</span><span class="sxs-lookup"><span data-stu-id="d0767-198">Work with  [andfrankly support team](mailto:help@andfrankly.com) to add the users in the &frankly platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d0767-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d0767-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d0767-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till & även.</span><span class="sxs-lookup"><span data-stu-id="d0767-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to &frankly.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d0767-202">**Om du vill tilldela Britta Simon till & även, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d0767-202">**To assign Britta Simon to &frankly, perform the following steps:**</span></span>

1. <span data-ttu-id="d0767-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d0767-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d0767-205">Välj i listan med program **& även**.</span><span class="sxs-lookup"><span data-stu-id="d0767-205">In the applications list, select **&frankly**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_app.png) 

3. <span data-ttu-id="d0767-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d0767-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d0767-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d0767-209">Click **Add** button.</span></span> <span data-ttu-id="d0767-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0767-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d0767-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d0767-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d0767-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0767-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d0767-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0767-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d0767-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d0767-215">Testing single sign-on</span></span>

<span data-ttu-id="d0767-216">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d0767-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="d0767-217">När du klickar på & även panelen i åtkomstpanelen, bör det automatiskt loggat in på ditt & även programmet</span><span class="sxs-lookup"><span data-stu-id="d0767-217">When you click the &frankly tile in the Access Panel, you should get automatically signed-on to your &frankly application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0767-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d0767-218">Additional resources</span></span>

* [<span data-ttu-id="d0767-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d0767-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d0767-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d0767-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png

