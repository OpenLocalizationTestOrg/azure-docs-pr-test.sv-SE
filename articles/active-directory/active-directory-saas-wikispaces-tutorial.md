---
title: "Självstudier: Azure Active Directory-integrering med Wikispaces | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Wikispaces."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 665b95aa-f7f5-4406-9e2a-6fc299a1599c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: d01543955bdf6a274571f67eafdff5f637863d5c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wikispaces"></a><span data-ttu-id="4977c-103">Självstudier: Azure Active Directory-integrering med Wikispaces</span><span class="sxs-lookup"><span data-stu-id="4977c-103">Tutorial: Azure Active Directory integration with Wikispaces</span></span>

<span data-ttu-id="4977c-104">I kursen får lära du att integrera Wikispaces med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4977c-104">In this tutorial, you learn how to integrate Wikispaces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4977c-105">Integrera Wikispaces med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4977c-105">Integrating Wikispaces with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4977c-106">Du kan styra i Azure AD som har åtkomst till Wikispaces</span><span class="sxs-lookup"><span data-stu-id="4977c-106">You can control in Azure AD who has access to Wikispaces</span></span>
- <span data-ttu-id="4977c-107">Du kan aktivera användarna att automatiskt hämta loggat in på Wikispaces (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4977c-107">You can enable your users to automatically get signed-on to Wikispaces (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4977c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4977c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4977c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4977c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4977c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4977c-110">Prerequisites</span></span>

<span data-ttu-id="4977c-111">För att konfigurera Azure AD-integrering med Wikispaces, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4977c-111">To configure Azure AD integration with Wikispaces, you need the following items:</span></span>

- <span data-ttu-id="4977c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4977c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4977c-113">En Wikispaces enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4977c-113">A Wikispaces single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4977c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4977c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4977c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4977c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4977c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4977c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4977c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4977c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4977c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4977c-118">Scenario description</span></span>
<span data-ttu-id="4977c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4977c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4977c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4977c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4977c-121">Att lägga till Wikispaces från galleriet</span><span class="sxs-lookup"><span data-stu-id="4977c-121">Adding Wikispaces from the gallery</span></span>
2. <span data-ttu-id="4977c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4977c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wikispaces-from-the-gallery"></a><span data-ttu-id="4977c-123">Att lägga till Wikispaces från galleriet</span><span class="sxs-lookup"><span data-stu-id="4977c-123">Adding Wikispaces from the gallery</span></span>
<span data-ttu-id="4977c-124">Du måste lägga till Wikispaces från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Wikispaces i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4977c-124">To configure the integration of Wikispaces into Azure AD, you need to add Wikispaces from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4977c-125">**Utför följande steg för att lägga till Wikispaces från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="4977c-125">**To add Wikispaces from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4977c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4977c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4977c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4977c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4977c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4977c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4977c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4977c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4977c-133">I sökrutan skriver **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="4977c-133">In the search box, type **Wikispaces**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_search.png)

5. <span data-ttu-id="4977c-135">Välj i resultatpanelen **Wikispaces**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="4977c-135">In the results panel, select **Wikispaces**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4977c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4977c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4977c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Wikispaces baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4977c-138">In this section, you configure and test Azure AD single sign-on with Wikispaces based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4977c-139">Azure AD måste du känna till användaren i Wikispaces motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="4977c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Wikispaces is to a user in Azure AD.</span></span> <span data-ttu-id="4977c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Wikispaces upprättas.</span><span class="sxs-lookup"><span data-stu-id="4977c-140">In other words, a link relationship between an Azure AD user and the related user in Wikispaces needs to be established.</span></span>

<span data-ttu-id="4977c-141">I Wikispaces, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4977c-141">In Wikispaces, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4977c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Wikispaces, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4977c-142">To configure and test Azure AD single sign-on with Wikispaces, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4977c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4977c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4977c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4977c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4977c-145">**[Skapa en testanvändare Wikispaces](#creating-a-wikispaces-test-user)**  – du har en motsvarighet för Britta Simon i Wikispaces som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4977c-145">**[Creating a Wikispaces test user](#creating-a-wikispaces-test-user)** - to have a counterpart of Britta Simon in Wikispaces that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4977c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4977c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4977c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4977c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4977c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4977c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4977c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Wikispaces program.</span><span class="sxs-lookup"><span data-stu-id="4977c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Wikispaces application.</span></span>

<span data-ttu-id="4977c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Wikispaces:**</span><span class="sxs-lookup"><span data-stu-id="4977c-150">**To configure Azure AD single sign-on with Wikispaces, perform the following steps:**</span></span>

1. <span data-ttu-id="4977c-151">I Azure-portalen på den **Wikispaces** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4977c-151">In the Azure portal, on the **Wikispaces** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4977c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4977c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_samlbase.png)

3. <span data-ttu-id="4977c-155">På den **Wikispaces domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4977c-155">On the **Wikispaces Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_url.png)

    <span data-ttu-id="4977c-157">a.</span><span class="sxs-lookup"><span data-stu-id="4977c-157">a.</span></span> <span data-ttu-id="4977c-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.wikispaces.net`</span><span class="sxs-lookup"><span data-stu-id="4977c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.wikispaces.net`</span></span>

    <span data-ttu-id="4977c-159">b.</span><span class="sxs-lookup"><span data-stu-id="4977c-159">b.</span></span> <span data-ttu-id="4977c-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://session.wikispaces.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="4977c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://session.wikispaces.net/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4977c-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4977c-161">These values are not real.</span></span> <span data-ttu-id="4977c-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="4977c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4977c-163">Kontakta [Wikispaces klienten supportteamet](https://www.wikispaces.com/site/help) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4977c-163">Contact [Wikispaces Client support team](https://www.wikispaces.com/site/help) to get these values.</span></span> 

4. <span data-ttu-id="4977c-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4977c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_certificate.png) 

5. <span data-ttu-id="4977c-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4977c-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4977c-168">Konfigurera enkel inloggning på **Wikispaces** sida, måste du skicka den hämtade **XML-Metadata för** till [Wikispaces supportteam](https://www.wikispaces.com/site/help).</span><span class="sxs-lookup"><span data-stu-id="4977c-168">To configure single sign-on on **Wikispaces** side, you need to send the downloaded **Metadata XML** to [Wikispaces support team](https://www.wikispaces.com/site/help).</span></span> <span data-ttu-id="4977c-169">Du får ett meddelande så snart som konfigurationen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="4977c-169">You will get a notification as soon as the configuration has been completed.</span></span>

> [!TIP]
> <span data-ttu-id="4977c-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="4977c-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4977c-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="4977c-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4977c-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4977c-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4977c-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4977c-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="4977c-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4977c-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4977c-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="4977c-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4977c-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4977c-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4977c-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4977c-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4977c-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4977c-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4977c-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4977c-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wikispaces-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4977c-185">a.</span><span class="sxs-lookup"><span data-stu-id="4977c-185">a.</span></span> <span data-ttu-id="4977c-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4977c-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4977c-187">b.</span><span class="sxs-lookup"><span data-stu-id="4977c-187">b.</span></span> <span data-ttu-id="4977c-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4977c-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4977c-189">c.</span><span class="sxs-lookup"><span data-stu-id="4977c-189">c.</span></span> <span data-ttu-id="4977c-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4977c-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4977c-191">d.</span><span class="sxs-lookup"><span data-stu-id="4977c-191">d.</span></span> <span data-ttu-id="4977c-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4977c-192">Click **Create**.</span></span>
 
### <a name="creating-a-wikispaces-test-user"></a><span data-ttu-id="4977c-193">Skapa en testanvändare Wikispaces</span><span class="sxs-lookup"><span data-stu-id="4977c-193">Creating a Wikispaces test user</span></span>

<span data-ttu-id="4977c-194">För att aktivera Azure AD-användare kan logga in på Wikispaces etableras de i Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="4977c-194">In order to enable Azure AD users to log in to Wikispaces, they must be provisioned into Wikispaces.</span></span> <span data-ttu-id="4977c-195">När det gäller Wikispaces är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4977c-195">In the case of Wikispaces, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="4977c-196">Utför följande steg för att etablera en användarkonton:</span><span class="sxs-lookup"><span data-stu-id="4977c-196">To provision a user accounts, perform the following steps:</span></span>
1. <span data-ttu-id="4977c-197">Logga in på ditt **Wikispaces** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="4977c-197">Log in to your **Wikispaces** company site as an administrator.</span></span>

2. <span data-ttu-id="4977c-198">Gå till **medlemmar**.</span><span class="sxs-lookup"><span data-stu-id="4977c-198">Go to **Members**.</span></span>
   
    <span data-ttu-id="4977c-199">![Medlemmar](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "medlemmar")</span><span class="sxs-lookup"><span data-stu-id="4977c-199">![Members](./media/active-directory-saas-wikispaces-tutorial/ic787193.png "Members")</span></span>

3. <span data-ttu-id="4977c-200">Klicka på den **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="4977c-200">Click the **Invite People**.</span></span>
   
    <span data-ttu-id="4977c-201">![Bjuda in](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="4977c-201">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787194.png "Invite People")</span></span>

4. <span data-ttu-id="4977c-202">I den **bjuda in personer** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4977c-202">In the **Invite People** section, perform the following steps:</span></span>
   
    <span data-ttu-id="4977c-203">![Bjuda in](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "bjuda in")</span><span class="sxs-lookup"><span data-stu-id="4977c-203">![Invite People](./media/active-directory-saas-wikispaces-tutorial/ic787208.png "Invite People")</span></span>
   
    <span data-ttu-id="4977c-204">a.</span><span class="sxs-lookup"><span data-stu-id="4977c-204">a.</span></span> <span data-ttu-id="4977c-205">Typ av **användarnamn eller e-postadress** av en giltig AAD-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="4977c-205">Type the **Usernames or Email Address** of a valid AAD account you want to provision into the related textboxes.</span></span>
   
    <span data-ttu-id="4977c-206">b.</span><span class="sxs-lookup"><span data-stu-id="4977c-206">b.</span></span> <span data-ttu-id="4977c-207">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="4977c-207">Click **Send**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="4977c-208">Azure Active Directory kontoinnehavaren får ett e-postmeddelande med en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="4977c-208">The Azure Active Directory account holder receives an email including a link to confirm the account before it becomes active.</span></span>
    
> [!NOTE]
> <span data-ttu-id="4977c-209">Du kan använda något annat Wikispaces användarens konto skapas verktyg eller API: er som tillhandahålls av Wikispaces att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="4977c-209">You can use any other Wikispaces user account creation tools or APIs provided by Wikispaces to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4977c-210">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4977c-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4977c-211">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Wikispaces.</span><span class="sxs-lookup"><span data-stu-id="4977c-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Wikispaces.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4977c-213">**Om du vill tilldela Wikispaces Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4977c-213">**To assign Britta Simon to Wikispaces, perform the following steps:**</span></span>

1. <span data-ttu-id="4977c-214">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4977c-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4977c-216">Välj i listan med program **Wikispaces**.</span><span class="sxs-lookup"><span data-stu-id="4977c-216">In the applications list, select **Wikispaces**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-wikispaces-tutorial/tutorial_wikispaces_app.png) 

3. <span data-ttu-id="4977c-218">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4977c-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4977c-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4977c-220">Click **Add** button.</span></span> <span data-ttu-id="4977c-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4977c-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4977c-223">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="4977c-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4977c-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4977c-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4977c-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4977c-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4977c-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4977c-226">Testing single sign-on</span></span>

<span data-ttu-id="4977c-227">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4977c-227">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="4977c-228">När du klickar på panelen Wikispaces på åtkomstpanelen du bör få automatiskt loggat in på ditt Wikispaces program.</span><span class="sxs-lookup"><span data-stu-id="4977c-228">When you click the Wikispaces tile in the Access Panel, you should get automatically signed-on to your Wikispaces application.</span></span>
<span data-ttu-id="4977c-229">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4977c-229">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4977c-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4977c-230">Additional resources</span></span>

* [<span data-ttu-id="4977c-231">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4977c-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4977c-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4977c-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wikispaces-tutorial/tutorial_general_203.png

