---
title: "Självstudier: Azure Active Directory-integrering med BC i molnet | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och BC i molnet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7dc40d2c-6349-40cb-b304-b098bd03a66c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/1/2017
ms.author: jeedes
ms.openlocfilehash: ebc95d600eca1027331cd92cfe481d0c3ee833a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bc-in-the-cloud"></a><span data-ttu-id="40246-103">Självstudier: Azure Active Directory-integrering med BC i molnet</span><span class="sxs-lookup"><span data-stu-id="40246-103">Tutorial: Azure Active Directory integration with BC in the Cloud</span></span>

<span data-ttu-id="40246-104">I kursen får lära du att integrera BC i molnet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="40246-104">In this tutorial, you learn how to integrate BC in the Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="40246-105">Integrera BC i molnet med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="40246-105">Integrating BC in the Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="40246-106">Du kan styra i Azure AD som har åtkomst till BC i molnet</span><span class="sxs-lookup"><span data-stu-id="40246-106">You can control in Azure AD who has access to BC in the Cloud</span></span>
- <span data-ttu-id="40246-107">Du kan aktivera användarna att automatiskt hämta loggat in på BC i molnet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="40246-107">You can enable your users to automatically get signed-on to BC in the Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="40246-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="40246-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="40246-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="40246-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40246-110">Krav</span><span class="sxs-lookup"><span data-stu-id="40246-110">Prerequisites</span></span>

<span data-ttu-id="40246-111">För att konfigurera Azure AD-integrering med BC i molnet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="40246-111">To configure Azure AD integration with BC in the Cloud, you need the following items:</span></span>

- <span data-ttu-id="40246-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="40246-112">An Azure AD subscription</span></span>
- <span data-ttu-id="40246-113">En BC i den enda-logga in på molnet aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="40246-113">A BC in the Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="40246-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="40246-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="40246-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="40246-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="40246-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="40246-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="40246-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="40246-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="40246-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="40246-118">Scenario description</span></span>
<span data-ttu-id="40246-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="40246-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="40246-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="40246-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="40246-121">Lägga till BC i molnet från galleriet</span><span class="sxs-lookup"><span data-stu-id="40246-121">Adding BC in the Cloud from the gallery</span></span>
2. <span data-ttu-id="40246-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="40246-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bc-in-the-cloud-from-the-gallery"></a><span data-ttu-id="40246-123">Lägga till BC i molnet från galleriet</span><span class="sxs-lookup"><span data-stu-id="40246-123">Adding BC in the Cloud from the gallery</span></span>
<span data-ttu-id="40246-124">Du måste lägga till BC i molnet från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av BC i molnet till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="40246-124">To configure the integration of BC in the Cloud into Azure AD, you need to add BC in the Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="40246-125">**Utför följande steg för att lägga till BC i molnet från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="40246-125">**To add BC in the Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="40246-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="40246-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="40246-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="40246-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="40246-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="40246-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="40246-131">Om du vill lägga till nya programmet, klickar du på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="40246-131">To add new application, click **Add** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="40246-133">I sökrutan skriver **BC i molnet**.</span><span class="sxs-lookup"><span data-stu-id="40246-133">In the search box, type **BC in the Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_search.png)

5. <span data-ttu-id="40246-135">Välj i resultatpanelen **BC i molnet**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="40246-135">In the results panel, select **BC in the Cloud**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="40246-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="40246-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="40246-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med BC i molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="40246-138">In this section, you configure and test Azure AD single sign-on with BC in the Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="40246-139">Azure AD måste du känna till motsvarande användaren i BC i molnet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="40246-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BC in the Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="40246-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i BC i molnet upprättas.</span><span class="sxs-lookup"><span data-stu-id="40246-140">In other words, a link relationship between an Azure AD user and the related user in BC in the Cloud needs to be established.</span></span>

<span data-ttu-id="40246-141">I BC i molnet, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="40246-141">In BC in the Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="40246-142">Om du vill konfigurera och testa Azure AD enkel inloggning med BC i molnet, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="40246-142">To configure and test Azure AD single sign-on with BC in the Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="40246-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="40246-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="40246-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40246-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="40246-145">**[Skapa en BC i molnet testanvändare](#creating-a-bc-in-the-cloud-test-user)**  – har en motsvarighet för Britta Simon BC i molnet som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="40246-145">**[Creating a BC in the Cloud test user](#creating-a-bc-in-the-cloud-test-user)** - to have a counterpart of Britta Simon in BC in the Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="40246-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="40246-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="40246-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="40246-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="40246-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="40246-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="40246-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din BC i molnet programmet.</span><span class="sxs-lookup"><span data-stu-id="40246-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BC in the Cloud application.</span></span>

<span data-ttu-id="40246-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med BC i molnet:**</span><span class="sxs-lookup"><span data-stu-id="40246-150">**To configure Azure AD single sign-on with BC in the Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="40246-151">I Azure-portalen på den **BC i molnet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="40246-151">In the Azure portal, on the **BC in the Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="40246-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="40246-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_samlbase.png)

3. <span data-ttu-id="40246-155">På den **BC i molnet domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="40246-155">On the **BC in the Cloud Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_url.png)

    <span data-ttu-id="40246-157">a.</span><span class="sxs-lookup"><span data-stu-id="40246-157">a.</span></span> <span data-ttu-id="40246-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span><span class="sxs-lookup"><span data-stu-id="40246-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.bcinthecloud.com/router/loginSaml/<customerid>`</span></span>

    <span data-ttu-id="40246-159">b.</span><span class="sxs-lookup"><span data-stu-id="40246-159">b.</span></span> <span data-ttu-id="40246-160">I den **identifierare** textruta Skriv en URL som:`https://app.bcinthecloud.com`</span><span class="sxs-lookup"><span data-stu-id="40246-160">In the **Identifier** textbox, type a URL as: `https://app.bcinthecloud.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="40246-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="40246-161">This value is not real.</span></span> <span data-ttu-id="40246-162">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="40246-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="40246-163">Kontakta [BC i molnet klienten supportteam](https://www.bcinthecloud.com/supportcenter/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="40246-163">Contact [BC in the Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) to get this value.</span></span> 
 
4. <span data-ttu-id="40246-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="40246-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_certificate.png) 

5. <span data-ttu-id="40246-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="40246-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="40246-168">Konfigurera enkel inloggning på **BC i molnet** sida, måste du skicka den hämtade **XML-Metadata för** till [BC i molnet supportteam](https://www.bcinthecloud.com/supportcenter/).</span><span class="sxs-lookup"><span data-stu-id="40246-168">To configure single sign-on on **BC in the Cloud** side, you need to send the downloaded **Metadata XML** to [BC in the Cloud support team](https://www.bcinthecloud.com/supportcenter/).</span></span>

> [!TIP]
> <span data-ttu-id="40246-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="40246-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="40246-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="40246-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="40246-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="40246-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="40246-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="40246-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="40246-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="40246-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="40246-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="40246-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="40246-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="40246-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="40246-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="40246-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="40246-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="40246-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="40246-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="40246-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bcinthecloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="40246-184">a.</span><span class="sxs-lookup"><span data-stu-id="40246-184">a.</span></span> <span data-ttu-id="40246-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="40246-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="40246-186">b.</span><span class="sxs-lookup"><span data-stu-id="40246-186">b.</span></span> <span data-ttu-id="40246-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="40246-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="40246-188">c.</span><span class="sxs-lookup"><span data-stu-id="40246-188">c.</span></span> <span data-ttu-id="40246-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="40246-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="40246-190">d.</span><span class="sxs-lookup"><span data-stu-id="40246-190">d.</span></span> <span data-ttu-id="40246-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="40246-191">Click **Create**.</span></span>
 
### <a name="creating-a-bc-in-the-cloud-test-user"></a><span data-ttu-id="40246-192">Skapa en BC i molnet testanvändare</span><span class="sxs-lookup"><span data-stu-id="40246-192">Creating a BC in the Cloud test user</span></span>

<span data-ttu-id="40246-193">I det här avsnittet skapar du en användare som kallas Britta Simon i BC i molnet.</span><span class="sxs-lookup"><span data-stu-id="40246-193">In this section, you create a user called Britta Simon in BC in the Cloud.</span></span> <span data-ttu-id="40246-194">Arbeta med [BC i molnet klienten supportteam](https://www.bcinthecloud.com/supportcenter/) att lägga till användare i BC i molnet programmet.</span><span class="sxs-lookup"><span data-stu-id="40246-194">Work with [BC in the Cloud Client support team](https://www.bcinthecloud.com/supportcenter/) to add the users in the BC in the Cloud application.</span></span> <span data-ttu-id="40246-195">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="40246-195">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="40246-196">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="40246-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="40246-197">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till BC i molnet.</span><span class="sxs-lookup"><span data-stu-id="40246-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BC in the Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="40246-199">**Om du vill tilldela BC Britta Simon i molnet, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="40246-199">**To assign Britta Simon to BC in the Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="40246-200">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="40246-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="40246-202">Välj i listan med program **BC i molnet**.</span><span class="sxs-lookup"><span data-stu-id="40246-202">In the applications list, select **BC in the Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bcinthecloud-tutorial/tutorial_bcinthecloud_app.png) 

3. <span data-ttu-id="40246-204">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="40246-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="40246-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="40246-206">Click **Add** button.</span></span> <span data-ttu-id="40246-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="40246-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="40246-209">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="40246-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="40246-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="40246-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="40246-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="40246-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="40246-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="40246-212">Testing single sign-on</span></span>

<span data-ttu-id="40246-213">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="40246-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

 <span data-ttu-id="40246-214">När du klickar på BC i panelen molnet på åtkomstpanelen du bör få automatiskt loggat in på ditt BC i molnet programmet.</span><span class="sxs-lookup"><span data-stu-id="40246-214">When you click the BC in the Cloud tile in the Access Panel, you should get automatically signed-on to your BC in the Cloud application.</span></span> <span data-ttu-id="40246-215">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="40246-215">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="40246-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="40246-216">Additional resources</span></span>

* [<span data-ttu-id="40246-217">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="40246-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="40246-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="40246-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bcinthecloud-tutorial/tutorial_general_203.png

