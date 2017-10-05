---
title: "Självstudier: Azure Active Directory-integrering med 360 Online | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och 360 Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cda8eba6-843f-4a09-8c55-0aaf6e593d75
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 629c7db04b0f9c880da6dfa8eac7fe14ecd8a215
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-360-online"></a><span data-ttu-id="9e0a6-103">Självstudier: Azure Active Directory-integrering med 360 Online</span><span class="sxs-lookup"><span data-stu-id="9e0a6-103">Tutorial: Azure Active Directory integration with 360 Online</span></span>

<span data-ttu-id="9e0a6-104">I kursen får lära du att integrera 360 Online med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9e0a6-104">In this tutorial, you learn how to integrate 360 Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9e0a6-105">Integrera 360 Online med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9e0a6-105">Integrating 360 Online with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9e0a6-106">Du kan styra i Azure AD som har åtkomst till 360 Online</span><span class="sxs-lookup"><span data-stu-id="9e0a6-106">You can control in Azure AD who has access to 360 Online</span></span>
- <span data-ttu-id="9e0a6-107">Du kan aktivera användarna att automatiskt hämta loggat in på 360 Online (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9e0a6-107">You can enable your users to automatically get signed-on to 360 Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9e0a6-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9e0a6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9e0a6-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9e0a6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9e0a6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9e0a6-110">Prerequisites</span></span>

<span data-ttu-id="9e0a6-111">För att konfigurera Azure AD-integrering med 360 Online, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="9e0a6-111">To configure Azure AD integration with 360 Online, you need the following items:</span></span>

- <span data-ttu-id="9e0a6-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9e0a6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9e0a6-113">En 360 Online enkel inloggning på aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="9e0a6-113">A 360 Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9e0a6-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9e0a6-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9e0a6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9e0a6-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9e0a6-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9e0a6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9e0a6-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9e0a6-118">Scenario description</span></span>
<span data-ttu-id="9e0a6-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9e0a6-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9e0a6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9e0a6-121">Att lägga till 360 Online från galleriet</span><span class="sxs-lookup"><span data-stu-id="9e0a6-121">Adding 360 Online from the gallery</span></span>
2. <span data-ttu-id="9e0a6-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9e0a6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-360-online-from-the-gallery"></a><span data-ttu-id="9e0a6-123">Att lägga till 360 Online från galleriet</span><span class="sxs-lookup"><span data-stu-id="9e0a6-123">Adding 360 Online from the gallery</span></span>
<span data-ttu-id="9e0a6-124">Du måste lägga till 360 Online från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av 360 Online i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-124">To configure the integration of 360 Online into Azure AD, you need to add 360 Online from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9e0a6-125">**Utför följande steg för att lägga till 360 Online från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="9e0a6-125">**To add 360 Online from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9e0a6-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9e0a6-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9e0a6-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9e0a6-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9e0a6-133">I sökrutan skriver **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-133">In the search box, type **360 Online**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-360online-tutorial/tutorial_360online_search.png)

5. <span data-ttu-id="9e0a6-135">Välj i resultatpanelen **360 Online**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-135">In the results panel, select **360 Online**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-360online-tutorial/tutorial_360online_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9e0a6-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9e0a6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9e0a6-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 360 Online baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-138">In this section, you configure and test Azure AD single sign-on with 360 Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9e0a6-139">Azure AD måste du känna till motsvarande användaren i 360 Online till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 360 Online is to a user in Azure AD.</span></span> <span data-ttu-id="9e0a6-140">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i 360 Online måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-140">In other words, a link relationship between an Azure AD user and the related user in 360 Online needs to be established.</span></span>

<span data-ttu-id="9e0a6-141">I 360 Online, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-141">In 360 Online, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9e0a6-142">Om du vill konfigurera och testa Azure AD enkel inloggning med 360 Online, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9e0a6-142">To configure and test Azure AD single sign-on with 360 Online, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9e0a6-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9e0a6-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9e0a6-145">**[Skapa en 360 Online testanvändare](#creating-a-360-online-test-user)**  – du har en motsvarighet för Britta Simon i 360 Online som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-145">**[Creating a 360 Online test user](#creating-a-360-online-test-user)** - to have a counterpart of Britta Simon in 360 Online that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9e0a6-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9e0a6-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9e0a6-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9e0a6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9e0a6-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet 360 Online.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 360 Online application.</span></span>

<span data-ttu-id="9e0a6-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med 360 Online:**</span><span class="sxs-lookup"><span data-stu-id="9e0a6-150">**To configure Azure AD single sign-on with 360 Online, perform the following steps:**</span></span>

1. <span data-ttu-id="9e0a6-151">I Azure-portalen på den **360 Online** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-151">In the Azure portal, on the **360 Online** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9e0a6-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-360online-tutorial/tutorial_360online_samlbase.png)

3. <span data-ttu-id="9e0a6-155">På den **360 Online domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e0a6-155">On the **360 Online Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-360online-tutorial/tutorial_360online_url.png)

    <span data-ttu-id="9e0a6-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.public360online.com`</span><span class="sxs-lookup"><span data-stu-id="9e0a6-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.public360online.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9e0a6-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-158">The value is not real.</span></span> <span data-ttu-id="9e0a6-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="9e0a6-160">Kontakta [360 Online klienten supportteamet](mailto:360online@software-innovation.com) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-160">Contact [360 Online Client support team](mailto:360online@software-innovation.com) to get the value.</span></span> 
 
4. <span data-ttu-id="9e0a6-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-360online-tutorial/tutorial_360online_certificate.png) 

5. <span data-ttu-id="9e0a6-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-360online-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9e0a6-165">Konfigurera enkel inloggning på **360 Online** sida, måste du skicka den hämtade **XML-Metadata för** till [360 Online supportteam](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="9e0a6-165">To configure single sign-on on **360 Online** side, you need to send the downloaded **Metadata XML** to [360 Online support team](mailto:360online@software-innovation.com).</span></span> 

> [!TIP]
> <span data-ttu-id="9e0a6-166">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="9e0a6-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9e0a6-167">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9e0a6-168">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9e0a6-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9e0a6-169">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9e0a6-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="9e0a6-170">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9e0a6-172">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="9e0a6-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9e0a6-173">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9e0a6-175">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9e0a6-177">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9e0a6-179">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9e0a6-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9e0a6-181">a.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-181">a.</span></span> <span data-ttu-id="9e0a6-182">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9e0a6-183">b.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-183">b.</span></span> <span data-ttu-id="9e0a6-184">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9e0a6-185">c.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-185">c.</span></span> <span data-ttu-id="9e0a6-186">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9e0a6-187">d.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-187">d.</span></span> <span data-ttu-id="9e0a6-188">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-188">Click **Create**.</span></span>
 
### <a name="creating-a-360-online-test-user"></a><span data-ttu-id="9e0a6-189">Skapa en 360 Online testanvändare</span><span class="sxs-lookup"><span data-stu-id="9e0a6-189">Creating a 360 Online test user</span></span>

<span data-ttu-id="9e0a6-190">I det här avsnittet skapar du en användare som kallas Britta Simon i 360 Online.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-190">In this section, you create a user called Britta Simon in 360 Online.</span></span> <span data-ttu-id="9e0a6-191">Du behöver kontakta [360 Online supportteam](mailto:360online@software-innovation.com).</span><span class="sxs-lookup"><span data-stu-id="9e0a6-191">you need to contact [360 Online support team](mailto:360online@software-innovation.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9e0a6-192">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="9e0a6-192">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9e0a6-193">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till 360 Online.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-193">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 360 Online.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9e0a6-195">**Om du vill tilldela 360 Online Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9e0a6-195">**To assign Britta Simon to 360 Online, perform the following steps:**</span></span>

1. <span data-ttu-id="9e0a6-196">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-196">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9e0a6-198">Välj i listan med program **360 Online**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-198">In the applications list, select **360 Online**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-360online-tutorial/tutorial_360online_app.png) 

3. <span data-ttu-id="9e0a6-200">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-200">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9e0a6-202">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-202">Click **Add** button.</span></span> <span data-ttu-id="9e0a6-203">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9e0a6-205">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-205">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9e0a6-206">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9e0a6-207">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9e0a6-208">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9e0a6-208">Testing single sign-on</span></span>

<span data-ttu-id="9e0a6-209">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-209">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9e0a6-210">När du klickar på panelen 360 Online på panelen åtkomst du ska hämta automatiskt loggat in i tillämpningsprogrammet 360 Online.</span><span class="sxs-lookup"><span data-stu-id="9e0a6-210">When you click the 360 Online tile in the Access Panel, you should get automatically signed-on to your 360 Online application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9e0a6-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9e0a6-211">Additional resources</span></span>

* [<span data-ttu-id="9e0a6-212">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9e0a6-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9e0a6-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9e0a6-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-360online-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png

