---
title: "Självstudier: Azure Active Directory-integrering med FM:Systems | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och FM:Systems."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f78c58c5-6e98-458b-8991-78624a245665
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 3a597d228f6c9234ec2fd2644ec3ac50b98f3b6b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fmsystems"></a><span data-ttu-id="1cebe-103">Självstudier: Azure Active Directory-integrering med FM:Systems</span><span class="sxs-lookup"><span data-stu-id="1cebe-103">Tutorial: Azure Active Directory integration with FM:Systems</span></span>

<span data-ttu-id="1cebe-104">I kursen får lära du att integrera FM:Systems med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1cebe-104">In this tutorial, you learn how to integrate FM:Systems with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1cebe-105">Integrera FM:Systems med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1cebe-105">Integrating FM:Systems with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1cebe-106">Du kan styra i Azure AD som har åtkomst till FM:Systems</span><span class="sxs-lookup"><span data-stu-id="1cebe-106">You can control in Azure AD who has access to FM:Systems</span></span>
- <span data-ttu-id="1cebe-107">Du kan aktivera användarna att automatiskt hämta loggat in på FM:Systems (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1cebe-107">You can enable your users to automatically get signed-on to FM:Systems (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1cebe-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1cebe-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1cebe-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1cebe-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1cebe-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1cebe-110">Prerequisites</span></span>

<span data-ttu-id="1cebe-111">För att konfigurera Azure AD-integrering med FM:Systems, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="1cebe-111">To configure Azure AD integration with FM:Systems, you need the following items:</span></span>

- <span data-ttu-id="1cebe-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1cebe-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1cebe-113">En FM:Systems enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1cebe-113">An FM:Systems single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1cebe-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1cebe-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1cebe-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1cebe-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1cebe-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1cebe-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1cebe-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1cebe-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1cebe-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1cebe-118">Scenario description</span></span>
<span data-ttu-id="1cebe-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1cebe-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1cebe-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1cebe-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1cebe-121">Att lägga till FM:Systems från galleriet</span><span class="sxs-lookup"><span data-stu-id="1cebe-121">Adding FM:Systems from the gallery</span></span>
2. <span data-ttu-id="1cebe-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1cebe-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-fmsystems-from-the-gallery"></a><span data-ttu-id="1cebe-123">Att lägga till FM:Systems från galleriet</span><span class="sxs-lookup"><span data-stu-id="1cebe-123">Adding FM:Systems from the gallery</span></span>
<span data-ttu-id="1cebe-124">Du måste lägga till FM:Systems från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av FM:Systems i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1cebe-124">To configure the integration of FM:Systems into Azure AD, you need to add FM:Systems from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1cebe-125">**Utför följande steg för att lägga till FM:Systems från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="1cebe-125">**To add FM:Systems from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1cebe-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1cebe-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1cebe-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1cebe-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1cebe-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1cebe-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1cebe-133">I sökrutan skriver **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-133">In the search box, type **FM:Systems**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_search.png)

5. <span data-ttu-id="1cebe-135">Välj i resultatpanelen **FM:Systems**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="1cebe-135">In the results panel, select **FM:Systems**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1cebe-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1cebe-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1cebe-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FM:Systems baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1cebe-138">In this section, you configure and test Azure AD single sign-on with FM:Systems based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1cebe-139">Azure AD måste du känna till användaren i FM:Systems motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="1cebe-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FM:Systems is to a user in Azure AD.</span></span> <span data-ttu-id="1cebe-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i FM:Systems upprättas.</span><span class="sxs-lookup"><span data-stu-id="1cebe-140">In other words, a link relationship between an Azure AD user and the related user in FM:Systems needs to be established.</span></span>

<span data-ttu-id="1cebe-141">I FM:Systems, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1cebe-141">In FM:Systems, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1cebe-142">Om du vill konfigurera och testa Azure AD enkel inloggning med FM:Systems, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1cebe-142">To configure and test Azure AD single sign-on with FM:Systems, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1cebe-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1cebe-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1cebe-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1cebe-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1cebe-145">**[Skapa en testanvändare FM:Systems](#creating-an-fmsystems-test-user)**  – du har en motsvarighet för Britta Simon i FM:Systems som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1cebe-145">**[Creating an FM:Systems test user](#creating-an-fmsystems-test-user)** - to have a counterpart of Britta Simon in FM:Systems that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1cebe-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1cebe-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1cebe-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1cebe-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1cebe-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1cebe-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1cebe-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt FM:Systems program.</span><span class="sxs-lookup"><span data-stu-id="1cebe-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your FM:Systems application.</span></span>

<span data-ttu-id="1cebe-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med FM:Systems:**</span><span class="sxs-lookup"><span data-stu-id="1cebe-150">**To configure Azure AD single sign-on with FM:Systems, perform the following steps:**</span></span>

1. <span data-ttu-id="1cebe-151">I Azure-portalen på den **FM:Systems** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-151">In the Azure portal, on the **FM:Systems** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1cebe-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1cebe-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_samlbase.png)

3. <span data-ttu-id="1cebe-155">På den **FM:Systems domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1cebe-155">On the **FM:Systems Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_url.png)

    <span data-ttu-id="1cebe-157">I den **Reply URL** textruta Skriv din FM:Systems **Reply URL**, Skriv URL-Adressen med följande mönster:`https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span><span class="sxs-lookup"><span data-stu-id="1cebe-157">In the **Reply URL** textbox, type your FM:Systems **Reply URL**, type the URL using the following pattern: `https://<companyname>.fmshosted.com/fminteract/ConsumerService2.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1cebe-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1cebe-158">This value is not real.</span></span> <span data-ttu-id="1cebe-159">Uppdatera det här värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="1cebe-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="1cebe-160">Kontakta [FM:Systems supportteamet](https://fmsystems.com/ask-us/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="1cebe-160">Contact [FM:Systems support team](https://fmsystems.com/ask-us/) to get this value.</span></span>
 
4. <span data-ttu-id="1cebe-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="1cebe-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_certificate.png) 

5. <span data-ttu-id="1cebe-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1cebe-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1cebe-165">Konfigurera enkel inloggning på **FM:Systems** sida, måste du skicka den hämtade **XML-Metadata för** till [FM:Systems supportteamet](https://fmsystems.com/ask-us/).</span><span class="sxs-lookup"><span data-stu-id="1cebe-165">To configure single sign-on on **FM:Systems** side, you need to send the downloaded **Metadata XML** to [FM:Systems support team](https://fmsystems.com/ask-us/).</span></span> <span data-ttu-id="1cebe-166">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="1cebe-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span> <span data-ttu-id="1cebe-167">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1cebe-167">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="1cebe-168">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="1cebe-168">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1cebe-169">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="1cebe-169">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1cebe-170">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1cebe-170">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1cebe-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1cebe-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="1cebe-172">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1cebe-172">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1cebe-174">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="1cebe-174">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1cebe-175">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1cebe-175">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1cebe-177">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-177">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1cebe-179">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1cebe-179">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1cebe-181">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1cebe-181">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fm-systems-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1cebe-183">a.</span><span class="sxs-lookup"><span data-stu-id="1cebe-183">a.</span></span> <span data-ttu-id="1cebe-184">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-184">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1cebe-185">b.</span><span class="sxs-lookup"><span data-stu-id="1cebe-185">b.</span></span> <span data-ttu-id="1cebe-186">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1cebe-186">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1cebe-187">c.</span><span class="sxs-lookup"><span data-stu-id="1cebe-187">c.</span></span> <span data-ttu-id="1cebe-188">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-188">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1cebe-189">d.</span><span class="sxs-lookup"><span data-stu-id="1cebe-189">d.</span></span> <span data-ttu-id="1cebe-190">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-190">Click **Create**.</span></span>
 
### <a name="creating-an-fmsystems-test-user"></a><span data-ttu-id="1cebe-191">Skapa en testanvändare FM:Systems</span><span class="sxs-lookup"><span data-stu-id="1cebe-191">Creating an FM:Systems test user</span></span>

1. <span data-ttu-id="1cebe-192">Logga in på webbplatsen FM:Systems företag som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="1cebe-192">In a web browser window, log into your FM:Systems company site as an administrator.</span></span>

2. <span data-ttu-id="1cebe-193">Gå till **systemadministration \> Hantera säkerhet \> användare \> användarlistan**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-193">Go to **System Administration \> Manage Security \> Users \> User list**.</span></span>
   
    <span data-ttu-id="1cebe-194">![Systemadministration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "systemadministration")</span><span class="sxs-lookup"><span data-stu-id="1cebe-194">![System Administration](./media/active-directory-saas-fm-systems-tutorial/ic795905.png "System Administration")</span></span>

3. <span data-ttu-id="1cebe-195">Klicka på **skapa nya användare**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-195">Click **Create new user**.</span></span>
   
    <span data-ttu-id="1cebe-196">![Skapa ny användare](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Skapa ny användare")</span><span class="sxs-lookup"><span data-stu-id="1cebe-196">![Create New User](./media/active-directory-saas-fm-systems-tutorial/ic795906.png "Create New User")</span></span>

4. <span data-ttu-id="1cebe-197">I den **skapa användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1cebe-197">In the **Create User** section, perform the following steps:</span></span>
   
    <span data-ttu-id="1cebe-198">![Skapa användare](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "skapa användare")</span><span class="sxs-lookup"><span data-stu-id="1cebe-198">![Create User](./media/active-directory-saas-fm-systems-tutorial/ic795907.png "Create User")</span></span>
   
    <span data-ttu-id="1cebe-199">a.</span><span class="sxs-lookup"><span data-stu-id="1cebe-199">a.</span></span> <span data-ttu-id="1cebe-200">Typ av **användarnamn**, **lösenord**, **Bekräfta lösenord**, **e-post** och **anställnings-ID** av en giltigt Azure Active Directory-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="1cebe-200">Type the **UserName**, the **Password**, **Confirm Password**, **E-mail** and the **Employee ID** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="1cebe-201">b.</span><span class="sxs-lookup"><span data-stu-id="1cebe-201">b.</span></span> <span data-ttu-id="1cebe-202">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-202">Click **Next**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1cebe-203">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="1cebe-203">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1cebe-204">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till FM:Systems.</span><span class="sxs-lookup"><span data-stu-id="1cebe-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to FM:Systems.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1cebe-206">**Om du vill tilldela FM:Systems Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1cebe-206">**To assign Britta Simon to FM:Systems, perform the following steps:**</span></span>

1. <span data-ttu-id="1cebe-207">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1cebe-209">Välj i listan med program **FM:Systems**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-209">In the applications list, select **FM:Systems**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fm-systems-tutorial/tutorial_fmsystems_app.png) 

3. <span data-ttu-id="1cebe-211">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1cebe-211">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1cebe-213">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1cebe-213">Click **Add** button.</span></span> <span data-ttu-id="1cebe-214">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1cebe-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1cebe-216">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="1cebe-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1cebe-217">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1cebe-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1cebe-218">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1cebe-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1cebe-219">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1cebe-219">Testing single sign-on</span></span>

<span data-ttu-id="1cebe-220">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="1cebe-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1cebe-221">När du klickar på panelen FM:Systems på åtkomstpanelen du bör få automatiskt loggat in på ditt FM:Systems program.</span><span class="sxs-lookup"><span data-stu-id="1cebe-221">When you click the FM:Systems tile in the Access Panel, you should get automatically signed-on to your FM:Systems application.</span></span>
<span data-ttu-id="1cebe-222">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1cebe-222">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1cebe-223">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1cebe-223">Additional resources</span></span>

* [<span data-ttu-id="1cebe-224">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1cebe-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1cebe-225">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1cebe-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fm-systems-tutorial/tutorial_general_203.png

