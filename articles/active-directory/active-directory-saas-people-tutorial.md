---
title: "Självstudier: Azure Active Directory-integrering med personer | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och personer."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c9b6202-11dd-4bb6-a679-8fb0a7a0ef4e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: caa287a2ed8774965ef722685e4e950336e5e0ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-people"></a><span data-ttu-id="dbd83-103">Självstudier: Azure Active Directory-integrering med personer</span><span class="sxs-lookup"><span data-stu-id="dbd83-103">Tutorial: Azure Active Directory integration with People</span></span>

<span data-ttu-id="dbd83-104">I kursen får lära du att integrera personer med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="dbd83-104">In this tutorial, you learn how to integrate People with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dbd83-105">Integrera personer med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="dbd83-105">Integrating People with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dbd83-106">Du kan styra i Azure AD som har åtkomst till personer</span><span class="sxs-lookup"><span data-stu-id="dbd83-106">You can control in Azure AD who has access to People</span></span>
- <span data-ttu-id="dbd83-107">Du kan aktivera användarna att automatiskt hämta inloggade till personer (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="dbd83-107">You can enable your users to automatically get signed-on to People (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dbd83-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="dbd83-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dbd83-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dbd83-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dbd83-110">Krav</span><span class="sxs-lookup"><span data-stu-id="dbd83-110">Prerequisites</span></span>

<span data-ttu-id="dbd83-111">För att konfigurera Azure AD-integrering med personer, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="dbd83-111">To configure Azure AD integration with People, you need the following items:</span></span>

- <span data-ttu-id="dbd83-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dbd83-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dbd83-113">En användare enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="dbd83-113">A People single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dbd83-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="dbd83-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dbd83-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="dbd83-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dbd83-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="dbd83-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dbd83-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dbd83-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dbd83-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="dbd83-118">Scenario description</span></span>
<span data-ttu-id="dbd83-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="dbd83-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dbd83-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="dbd83-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dbd83-121">Att lägga till personer från galleriet</span><span class="sxs-lookup"><span data-stu-id="dbd83-121">Adding People from the gallery</span></span>
2. <span data-ttu-id="dbd83-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dbd83-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-people-from-the-gallery"></a><span data-ttu-id="dbd83-123">Att lägga till personer från galleriet</span><span class="sxs-lookup"><span data-stu-id="dbd83-123">Adding People from the gallery</span></span>
<span data-ttu-id="dbd83-124">För att konfigurera integrering av personer i Azure AD, måste du lägga till personer i din lista över hanterade SaaS-appar från galleriet.</span><span class="sxs-lookup"><span data-stu-id="dbd83-124">To configure the integration of People into Azure AD, you need to add People from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dbd83-125">**Utför följande steg för att lägga till personer från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="dbd83-125">**To add People from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dbd83-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dbd83-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dbd83-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dbd83-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="dbd83-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dbd83-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="dbd83-133">I sökrutan skriver **personer**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-133">In the search box, type **People**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_search.png)

5. <span data-ttu-id="dbd83-135">Välj i resultatpanelen **personer**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="dbd83-135">In the results panel, select **People**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-people-tutorial/tutorial_people_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dbd83-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dbd83-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dbd83-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med andra utifrån en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="dbd83-138">In this section, you configure and test Azure AD single sign-on with People based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dbd83-139">Azure AD måste du känna till användaren i personer motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="dbd83-139">For single sign-on to work, Azure AD needs to know what the counterpart user in People is to a user in Azure AD.</span></span> <span data-ttu-id="dbd83-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i personer upprättas.</span><span class="sxs-lookup"><span data-stu-id="dbd83-140">In other words, a link relationship between an Azure AD user and the related user in People needs to be established.</span></span>

<span data-ttu-id="dbd83-141">I personer, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="dbd83-141">In People, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dbd83-142">Om du vill konfigurera och testa Azure AD enkel inloggning med personer, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="dbd83-142">To configure and test Azure AD single sign-on with People, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dbd83-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="dbd83-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dbd83-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dbd83-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dbd83-145">**[Skapa en testanvändare personer](#creating-a-people-test-user)**  – har en motsvarighet för Britta Simon personer som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="dbd83-145">**[Creating a People test user](#creating-a-people-test-user)** - to have a counterpart of Britta Simon in People that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dbd83-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dbd83-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dbd83-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="dbd83-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dbd83-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dbd83-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dbd83-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet personer.</span><span class="sxs-lookup"><span data-stu-id="dbd83-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your People application.</span></span>

<span data-ttu-id="dbd83-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med personer:**</span><span class="sxs-lookup"><span data-stu-id="dbd83-150">**To configure Azure AD single sign-on with People, perform the following steps:**</span></span>

1. <span data-ttu-id="dbd83-151">I Azure-portalen på den **personer** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-151">In the Azure portal, on the **People** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="dbd83-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dbd83-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_people_samlbase.png)

3. <span data-ttu-id="dbd83-155">På den **personer domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="dbd83-155">On the **People Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_people_url.png)

    <span data-ttu-id="dbd83-157">a.</span><span class="sxs-lookup"><span data-stu-id="dbd83-157">a.</span></span> <span data-ttu-id="dbd83-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.peoplehr.com/`</span><span class="sxs-lookup"><span data-stu-id="dbd83-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.peoplehr.com/`</span></span>

    <span data-ttu-id="dbd83-159">b.</span><span class="sxs-lookup"><span data-stu-id="dbd83-159">b.</span></span> <span data-ttu-id="dbd83-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://www.peoplehr.com`</span><span class="sxs-lookup"><span data-stu-id="dbd83-160">In the **Identifier** textbox, type a URL using the following pattern: `https://www.peoplehr.com`</span></span>

    <span data-ttu-id="dbd83-161">c.</span><span class="sxs-lookup"><span data-stu-id="dbd83-161">c.</span></span> <span data-ttu-id="dbd83-162">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span><span class="sxs-lookup"><span data-stu-id="dbd83-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dbd83-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="dbd83-163">These values are not real.</span></span> <span data-ttu-id="dbd83-164">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="dbd83-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="dbd83-165">Kontakta [personer klienten supportteamet](mailto:customerservices@peoplehr.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="dbd83-165">Contact [People Client support team](mailto:customerservices@peoplehr.com) to get these values.</span></span>

5. <span data-ttu-id="dbd83-166">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="dbd83-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_people_certificate.png) 

6. <span data-ttu-id="dbd83-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="dbd83-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="dbd83-170">För att få SSO konfigurerats för ditt program måste logga in till din klient för personer som administratör.</span><span class="sxs-lookup"><span data-stu-id="dbd83-170">To get SSO configured for your application, you need to sign-on to your People tenant as an administrator.</span></span>
   
8. <span data-ttu-id="dbd83-171">Klicka på menyn till vänster **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-171">In the menu on the left side, click **Settings**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_people_001.png)

9. <span data-ttu-id="dbd83-173">Klicka på **företagets**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-173">Click **Company**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_people_002.png)

10. <span data-ttu-id="dbd83-175">På den **överför 'Single Sign On' SAML metadata filen**, klickar du på **Bläddra** att överföra metadatafilen hämtade.</span><span class="sxs-lookup"><span data-stu-id="dbd83-175">On the **Upload 'Single Sign On' SAML meta-data file**, click **Browse** to upload the downloaded metadata file.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)

> [!TIP]
> <span data-ttu-id="dbd83-177">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="dbd83-177">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dbd83-178">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="dbd83-178">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dbd83-179">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dbd83-179">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dbd83-180">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="dbd83-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="dbd83-181">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dbd83-181">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="dbd83-183">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="dbd83-183">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dbd83-184">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="dbd83-184">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dbd83-186">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-186">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dbd83-188">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dbd83-188">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dbd83-190">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="dbd83-190">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dbd83-192">a.</span><span class="sxs-lookup"><span data-stu-id="dbd83-192">a.</span></span> <span data-ttu-id="dbd83-193">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-193">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dbd83-194">b.</span><span class="sxs-lookup"><span data-stu-id="dbd83-194">b.</span></span> <span data-ttu-id="dbd83-195">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dbd83-195">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dbd83-196">c.</span><span class="sxs-lookup"><span data-stu-id="dbd83-196">c.</span></span> <span data-ttu-id="dbd83-197">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-197">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dbd83-198">d.</span><span class="sxs-lookup"><span data-stu-id="dbd83-198">d.</span></span> <span data-ttu-id="dbd83-199">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-199">Click **Create**.</span></span>
 
### <a name="creating-a-people-test-user"></a><span data-ttu-id="dbd83-200">Skapa en testanvändare personer</span><span class="sxs-lookup"><span data-stu-id="dbd83-200">Creating a People test user</span></span>

<span data-ttu-id="dbd83-201">I det här avsnittet skapar du en användare som kallas Britta Simon i personer.</span><span class="sxs-lookup"><span data-stu-id="dbd83-201">In this section, you create a user called Britta Simon in People.</span></span> <span data-ttu-id="dbd83-202">Arbeta med [personer klienten supportteamet](mailto:customerservices@peoplehr.com) att lägga till användare i personer-plattformen.</span><span class="sxs-lookup"><span data-stu-id="dbd83-202">Work with [People Client support team](mailto:customerservices@peoplehr.com) to add the users in the People platform.</span></span> <span data-ttu-id="dbd83-203">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="dbd83-203">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dbd83-204">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="dbd83-204">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dbd83-205">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till personer.</span><span class="sxs-lookup"><span data-stu-id="dbd83-205">In this section, you enable Britta Simon to use Azure single sign-on by granting access to People.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="dbd83-207">**Om du vill tilldela personer Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="dbd83-207">**To assign Britta Simon to People, perform the following steps:**</span></span>

1. <span data-ttu-id="dbd83-208">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-208">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="dbd83-210">Välj i listan med program **personer**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-210">In the applications list, select **People**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-people-tutorial/tutorial_people_app.png) 

3. <span data-ttu-id="dbd83-212">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="dbd83-212">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="dbd83-214">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="dbd83-214">Click **Add** button.</span></span> <span data-ttu-id="dbd83-215">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dbd83-215">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="dbd83-217">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="dbd83-217">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dbd83-218">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dbd83-218">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dbd83-219">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="dbd83-219">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dbd83-220">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="dbd83-220">Testing single sign-on</span></span>

<span data-ttu-id="dbd83-221">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="dbd83-221">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="dbd83-222">När du klickar på panelen personer på åtkomstpanelen du bör få automatiskt loggat in i tillämpningsprogrammet personer.</span><span class="sxs-lookup"><span data-stu-id="dbd83-222">When you click the People tile in the Access Panel, you should get automatically signed-on to your People application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dbd83-223">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="dbd83-223">Additional resources</span></span>

* [<span data-ttu-id="dbd83-224">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dbd83-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dbd83-225">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dbd83-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-people-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png

