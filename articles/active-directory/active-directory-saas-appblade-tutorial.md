---
title: "Självstudier: Azure Active Directory-integrering med AppBlade | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och AppBlade."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3360d7aa-6518-4f99-88bd-b7f7258183e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 7820a70b34b6d25ba81b17c472159d08904335d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-appblade"></a><span data-ttu-id="f7985-103">Självstudier: Azure Active Directory-integrering med AppBlade</span><span class="sxs-lookup"><span data-stu-id="f7985-103">Tutorial: Azure Active Directory integration with AppBlade</span></span>

<span data-ttu-id="f7985-104">I kursen får lära du att integrera AppBlade med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f7985-104">In this tutorial, you learn how to integrate AppBlade with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f7985-105">Integrera AppBlade med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f7985-105">Integrating AppBlade with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f7985-106">Du kan styra i Azure AD som har åtkomst till AppBlade</span><span class="sxs-lookup"><span data-stu-id="f7985-106">You can control in Azure AD who has access to AppBlade</span></span>
- <span data-ttu-id="f7985-107">Du kan aktivera användarna att automatiskt hämta loggat in på AppBlade (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f7985-107">You can enable your users to automatically get signed-on to AppBlade (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f7985-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f7985-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f7985-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f7985-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7985-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f7985-110">Prerequisites</span></span>

<span data-ttu-id="f7985-111">För att konfigurera Azure AD-integrering med AppBlade, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f7985-111">To configure Azure AD integration with AppBlade, you need the following items:</span></span>

- <span data-ttu-id="f7985-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f7985-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f7985-113">En AppBlade enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f7985-113">An AppBlade single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f7985-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f7985-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f7985-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f7985-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f7985-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f7985-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f7985-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f7985-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f7985-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f7985-118">Scenario description</span></span>
<span data-ttu-id="f7985-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f7985-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f7985-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f7985-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f7985-121">Att lägga till AppBlade från galleriet</span><span class="sxs-lookup"><span data-stu-id="f7985-121">Adding AppBlade from the gallery</span></span>
2. <span data-ttu-id="f7985-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7985-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-appblade-from-the-gallery"></a><span data-ttu-id="f7985-123">Att lägga till AppBlade från galleriet</span><span class="sxs-lookup"><span data-stu-id="f7985-123">Adding AppBlade from the gallery</span></span>
<span data-ttu-id="f7985-124">Du måste lägga till AppBlade från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av AppBlade i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f7985-124">To configure the integration of AppBlade into Azure AD, you need to add AppBlade from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f7985-125">**Utför följande steg för att lägga till AppBlade från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f7985-125">**To add AppBlade from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f7985-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f7985-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f7985-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f7985-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f7985-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f7985-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f7985-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7985-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f7985-133">I sökrutan skriver **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="f7985-133">In the search box, type **AppBlade**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_search.png)

5. <span data-ttu-id="f7985-135">Välj i resultatpanelen **AppBlade**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f7985-135">In the results panel, select **AppBlade**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f7985-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7985-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f7985-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med AppBlade baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f7985-138">In this section, you configure and test Azure AD single sign-on with AppBlade based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f7985-139">Azure AD måste du känna till användaren i AppBlade motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f7985-139">For single sign-on to work, Azure AD needs to know what the counterpart user in AppBlade is to a user in Azure AD.</span></span> <span data-ttu-id="f7985-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i AppBlade upprättas.</span><span class="sxs-lookup"><span data-stu-id="f7985-140">In other words, a link relationship between an Azure AD user and the related user in AppBlade needs to be established.</span></span>

<span data-ttu-id="f7985-141">I AppBlade, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f7985-141">In AppBlade, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f7985-142">Om du vill konfigurera och testa Azure AD enkel inloggning med AppBlade, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f7985-142">To configure and test Azure AD single sign-on with AppBlade, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f7985-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f7985-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f7985-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7985-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f7985-145">**[Skapa en testanvändare AppBlade](#creating-an-appblade-test-user)**  – du har en motsvarighet för Britta Simon i AppBlade som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f7985-145">**[Creating an AppBlade test user](#creating-an-appblade-test-user)** - to have a counterpart of Britta Simon in AppBlade that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f7985-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f7985-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f7985-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f7985-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f7985-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7985-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f7985-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt AppBlade program.</span><span class="sxs-lookup"><span data-stu-id="f7985-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your AppBlade application.</span></span>

<span data-ttu-id="f7985-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med AppBlade:**</span><span class="sxs-lookup"><span data-stu-id="f7985-150">**To configure Azure AD single sign-on with AppBlade, perform the following steps:**</span></span>

1. <span data-ttu-id="f7985-151">I Azure-portalen på den **AppBlade** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f7985-151">In the Azure portal, on the **AppBlade** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f7985-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f7985-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_samlbase.png)

3. <span data-ttu-id="f7985-155">På den **AppBlade domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f7985-155">On the **AppBlade Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_url.png)

    <span data-ttu-id="f7985-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.appblade.com/saml/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="f7985-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.appblade.com/saml/<tenantid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f7985-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f7985-158">This value is not real.</span></span> <span data-ttu-id="f7985-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="f7985-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="f7985-160">Kontakta [AppBlade klienten supportteamet](mailto:support@appblade.com) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="f7985-160">Contact [AppBlade Client support team](mailto:support@appblade.com) to get the value.</span></span> 
 
4. <span data-ttu-id="f7985-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f7985-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_certificate.png) 

5. <span data-ttu-id="f7985-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f7985-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f7985-165">Konfigurera enkel inloggning på **AppBlade** sida, måste du skicka den hämtade **XML-Metadata för** till [AppBlade supportteamet](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="f7985-165">To configure single sign-on on **AppBlade** side, you need to send the downloaded **Metadata XML** to [AppBlade support team](mailto:support@appblade.com).</span></span> <span data-ttu-id="f7985-166">Dessutom be dem att konfigurera den **utfärdar-URL för SSO** som `https://appblade.com/saml`.</span><span class="sxs-lookup"><span data-stu-id="f7985-166">Also, please ask them to configure the **SSO Issuer URL** as `https://appblade.com/saml`.</span></span> <span data-ttu-id="f7985-167">Den här inställningen krävs för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f7985-167">This setting is required for single sign-on to work.</span></span>


> [!TIP]
> <span data-ttu-id="f7985-168">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f7985-168">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f7985-169">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f7985-169">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f7985-170">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f7985-170">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f7985-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f7985-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="f7985-172">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f7985-172">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f7985-174">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f7985-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f7985-175">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f7985-175">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f7985-177">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f7985-177">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f7985-179">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7985-179">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f7985-181">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f7985-181">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-appblade-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f7985-183">a.</span><span class="sxs-lookup"><span data-stu-id="f7985-183">a.</span></span> <span data-ttu-id="f7985-184">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f7985-184">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f7985-185">b.</span><span class="sxs-lookup"><span data-stu-id="f7985-185">b.</span></span> <span data-ttu-id="f7985-186">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f7985-186">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f7985-187">c.</span><span class="sxs-lookup"><span data-stu-id="f7985-187">c.</span></span> <span data-ttu-id="f7985-188">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f7985-188">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f7985-189">d.</span><span class="sxs-lookup"><span data-stu-id="f7985-189">d.</span></span> <span data-ttu-id="f7985-190">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f7985-190">Click **Create**.</span></span>
 
### <a name="creating-an-appblade-test-user"></a><span data-ttu-id="f7985-191">Skapa en testanvändare AppBlade</span><span class="sxs-lookup"><span data-stu-id="f7985-191">Creating an AppBlade test user</span></span>

<span data-ttu-id="f7985-192">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i AppBlade.</span><span class="sxs-lookup"><span data-stu-id="f7985-192">The objective of this section is to create a user called Britta Simon in AppBlade.</span></span> <span data-ttu-id="f7985-193">AppBlade stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="f7985-193">AppBlade supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="f7985-194">**Kontrollera att domännamnet är konfigurerad med AppBlade för användaretablering. Efter att endast användare som just-in-time-etablering fungerar.**</span><span class="sxs-lookup"><span data-stu-id="f7985-194">**Make sure that your domain name is configured with AppBlade for user provisioning. After that only the just-in-time user provisioning works.**</span></span>

<span data-ttu-id="f7985-195">Om användaren har en e-postadress som slutar med den domän som konfigurerats av AppBlade för ditt konto och sedan användaren kommer automatiskt att ansluta till kontot som en medlem med behörigheten som du anger, som är ”Basic” (en grundläggande användare kan bara installera program), ”teammedlem” (en användare kan ladda upp nya versioner av appen och hantera projekt) eller ”administratör” (fullständiga administratörsrättigheter till kontot).</span><span class="sxs-lookup"><span data-stu-id="f7985-195">If the user has an email address ending with the domain configured by AppBlade for your account, then the user will automatically join the account as a member with the permission level you specify, which is one of "Basic" (a basic user who can only install applications), "Team Member" (a user who can upload new app versions and manage projects), or "Administrator" (full admin privileges to the account).</span></span> <span data-ttu-id="f7985-196">En skulle normalt väljer Basic och främja användare manuellt via en inloggning för serveradministratör (AppBlade måste konfigurera antingen en e-postbaserad administratörsinloggning i förväg eller befordra en användare för kundens räkning efter inloggningen).</span><span class="sxs-lookup"><span data-stu-id="f7985-196">Normally one would choose Basic and then promote users manually via an Admin login (AppBlade needs to configure either an email-based admin login in advance or promote a user on behalf of the customer after login).</span></span>

<span data-ttu-id="f7985-197">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f7985-197">There is no action item for you in this section.</span></span> <span data-ttu-id="f7985-198">En ny användare skapas under ett försök att komma åt AppBlade om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="f7985-198">A new user is created during an attempt to access AppBlade if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="f7985-199">Om du behöver skapa en användare manuellt, måste du kontakta den [AppBlade supportteamet](mailto:support@appblade.com).</span><span class="sxs-lookup"><span data-stu-id="f7985-199">If you need to create a user manually, you need to contact the [AppBlade support team](mailto:support@appblade.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f7985-200">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f7985-200">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f7985-201">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till AppBlade.</span><span class="sxs-lookup"><span data-stu-id="f7985-201">In this section, you enable Britta Simon to use Azure single sign-on by granting access to AppBlade.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f7985-203">**Om du vill tilldela AppBlade Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f7985-203">**To assign Britta Simon to AppBlade, perform the following steps:**</span></span>

1. <span data-ttu-id="f7985-204">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f7985-204">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f7985-206">Välj i listan med program **AppBlade**.</span><span class="sxs-lookup"><span data-stu-id="f7985-206">In the applications list, select **AppBlade**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-appblade-tutorial/tutorial_appblade_app.png) 

3. <span data-ttu-id="f7985-208">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f7985-208">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f7985-210">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f7985-210">Click **Add** button.</span></span> <span data-ttu-id="f7985-211">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7985-211">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f7985-213">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f7985-213">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f7985-214">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7985-214">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f7985-215">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f7985-215">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f7985-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f7985-216">Testing single sign-on</span></span>

<span data-ttu-id="f7985-217">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f7985-217">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="f7985-218">När du klickar på panelen AppBlade på åtkomstpanelen du bör få automatiskt loggat in på ditt AppBlade program.</span><span class="sxs-lookup"><span data-stu-id="f7985-218">When you click the AppBlade tile in the Access Panel, you should get automatically signed-on to your AppBlade application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f7985-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f7985-219">Additional resources</span></span>

* [<span data-ttu-id="f7985-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f7985-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f7985-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f7985-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-appblade-tutorial/tutorial_general_203.png

