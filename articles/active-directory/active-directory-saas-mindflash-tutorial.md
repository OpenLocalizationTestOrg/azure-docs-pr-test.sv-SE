---
title: "Självstudier: Azure Active Directory-integrering med Mindflash | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Mindflash."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdf91993-aaaa-4598-89b7-77ef8ca065d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 90de7b6a82d88f9407a35fbfebe8a652928d76cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mindflash"></a><span data-ttu-id="cd918-103">Självstudier: Azure Active Directory-integrering med Mindflash</span><span class="sxs-lookup"><span data-stu-id="cd918-103">Tutorial: Azure Active Directory integration with Mindflash</span></span>

<span data-ttu-id="cd918-104">I kursen får lära du att integrera Mindflash med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cd918-104">In this tutorial, you learn how to integrate Mindflash with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd918-105">Integrera Mindflash med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cd918-105">Integrating Mindflash with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cd918-106">Du kan styra i Azure AD som har åtkomst till Mindflash</span><span class="sxs-lookup"><span data-stu-id="cd918-106">You can control in Azure AD who has access to Mindflash</span></span>
- <span data-ttu-id="cd918-107">Du kan aktivera användarna att automatiskt hämta loggat in på Mindflash (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cd918-107">You can enable your users to automatically get signed-on to Mindflash (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cd918-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cd918-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cd918-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd918-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd918-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cd918-110">Prerequisites</span></span>

<span data-ttu-id="cd918-111">För att konfigurera Azure AD-integrering med Mindflash, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cd918-111">To configure Azure AD integration with Mindflash, you need the following items:</span></span>

- <span data-ttu-id="cd918-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cd918-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd918-113">En Mindflash enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="cd918-113">A Mindflash single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cd918-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cd918-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cd918-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cd918-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd918-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cd918-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cd918-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd918-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cd918-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cd918-118">Scenario description</span></span>
<span data-ttu-id="cd918-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cd918-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd918-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cd918-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd918-121">Att lägga till Mindflash från galleriet</span><span class="sxs-lookup"><span data-stu-id="cd918-121">Adding Mindflash from the gallery</span></span>
2. <span data-ttu-id="cd918-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd918-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mindflash-from-the-gallery"></a><span data-ttu-id="cd918-123">Att lägga till Mindflash från galleriet</span><span class="sxs-lookup"><span data-stu-id="cd918-123">Adding Mindflash from the gallery</span></span>
<span data-ttu-id="cd918-124">Du måste lägga till Mindflash från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Mindflash i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd918-124">To configure the integration of Mindflash into Azure AD, you need to add Mindflash from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cd918-125">**Utför följande steg för att lägga till Mindflash från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cd918-125">**To add Mindflash from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cd918-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cd918-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cd918-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cd918-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cd918-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cd918-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cd918-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd918-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cd918-133">I sökrutan skriver **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="cd918-133">In the search box, type **Mindflash**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_search.png)

5. <span data-ttu-id="cd918-135">Välj i resultatpanelen **Mindflash**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cd918-135">In the results panel, select **Mindflash**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cd918-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd918-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cd918-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Mindflash baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cd918-138">In this section, you configure and test Azure AD single sign-on with Mindflash based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cd918-139">Azure AD måste du känna till användaren i Mindflash motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cd918-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mindflash is to a user in Azure AD.</span></span> <span data-ttu-id="cd918-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Mindflash upprättas.</span><span class="sxs-lookup"><span data-stu-id="cd918-140">In other words, a link relationship between an Azure AD user and the related user in Mindflash needs to be established.</span></span>

<span data-ttu-id="cd918-141">I Mindflash, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cd918-141">In Mindflash, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cd918-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Mindflash, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cd918-142">To configure and test Azure AD single sign-on with Mindflash, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cd918-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cd918-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cd918-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd918-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd918-145">**[Skapa en testanvändare Mindflash](#creating-a-mindflash-test-user)**  – du har en motsvarighet för Britta Simon i Mindflash som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cd918-145">**[Creating a Mindflash test user](#creating-a-mindflash-test-user)** - to have a counterpart of Britta Simon in Mindflash that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cd918-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cd918-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd918-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cd918-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cd918-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd918-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cd918-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Mindflash program.</span><span class="sxs-lookup"><span data-stu-id="cd918-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mindflash application.</span></span>

<span data-ttu-id="cd918-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Mindflash:**</span><span class="sxs-lookup"><span data-stu-id="cd918-150">**To configure Azure AD single sign-on with Mindflash, perform the following steps:**</span></span>

1. <span data-ttu-id="cd918-151">I Azure-portalen på den **Mindflash** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cd918-151">In the Azure portal, on the **Mindflash** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cd918-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cd918-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_samlbase.png)

3. <span data-ttu-id="cd918-155">På den **Mindflash domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cd918-155">On the **Mindflash Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_url.png)

    <span data-ttu-id="cd918-157">a.</span><span class="sxs-lookup"><span data-stu-id="cd918-157">a.</span></span> <span data-ttu-id="cd918-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="cd918-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`</span></span>

    <span data-ttu-id="cd918-159">b.</span><span class="sxs-lookup"><span data-stu-id="cd918-159">b.</span></span> <span data-ttu-id="cd918-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="cd918-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cd918-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cd918-161">These values are not real.</span></span> <span data-ttu-id="cd918-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="cd918-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cd918-163">Kontakta [Mindflash klienten supportteamet](https://www.mindflash.com/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cd918-163">Contact [Mindflash Client support team](https://www.mindflash.com/contact/) to get these values.</span></span> 
 


4. <span data-ttu-id="cd918-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cd918-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_certificate.png) 

5. <span data-ttu-id="cd918-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cd918-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cd918-168">Konfigurera enkel inloggning på **Mindflash** sida, måste du skicka den hämtade **XML-Metadata för** till [Mindflash supportteamet](https://www.mindflash.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="cd918-168">To configure single sign-on on **Mindflash** side, you need to send the downloaded **Metadata XML** to [Mindflash support team](https://www.mindflash.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="cd918-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cd918-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cd918-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cd918-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cd918-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cd918-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cd918-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd918-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="cd918-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd918-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cd918-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cd918-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cd918-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cd918-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cd918-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cd918-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cd918-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd918-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cd918-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cd918-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mindflash-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cd918-184">a.</span><span class="sxs-lookup"><span data-stu-id="cd918-184">a.</span></span> <span data-ttu-id="cd918-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd918-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd918-186">b.</span><span class="sxs-lookup"><span data-stu-id="cd918-186">b.</span></span> <span data-ttu-id="cd918-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cd918-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cd918-188">c.</span><span class="sxs-lookup"><span data-stu-id="cd918-188">c.</span></span> <span data-ttu-id="cd918-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cd918-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cd918-190">d.</span><span class="sxs-lookup"><span data-stu-id="cd918-190">d.</span></span> <span data-ttu-id="cd918-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cd918-191">Click **Create**.</span></span>
 
### <a name="creating-a-mindflash-test-user"></a><span data-ttu-id="cd918-192">Skapa en testanvändare Mindflash</span><span class="sxs-lookup"><span data-stu-id="cd918-192">Creating a Mindflash test user</span></span>

<span data-ttu-id="cd918-193">För att aktivera Azure AD-användare att logga in på Mindflash etableras de i Mindflash.</span><span class="sxs-lookup"><span data-stu-id="cd918-193">In order to enable Azure AD users to log into Mindflash, they must be provisioned into Mindflash.</span></span> <span data-ttu-id="cd918-194">När det gäller Mindflash är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="cd918-194">In the case of Mindflash, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="cd918-195">Utför följande steg för att etablera en användarkonton:</span><span class="sxs-lookup"><span data-stu-id="cd918-195">To provision a user accounts, perform the following steps:</span></span>

1. <span data-ttu-id="cd918-196">Logga in på ditt **Mindflash** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="cd918-196">Log in to your **Mindflash** company site as an administrator.</span></span>

2. <span data-ttu-id="cd918-197">Gå till **hantera användare**.</span><span class="sxs-lookup"><span data-stu-id="cd918-197">Go to **Manage Users**.</span></span>
   
    <span data-ttu-id="cd918-198">![Hantera användare](./media/active-directory-saas-mindflash-tutorial/ic787140.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="cd918-198">![Manage Users](./media/active-directory-saas-mindflash-tutorial/ic787140.png "Manage Users")</span></span>

3. <span data-ttu-id="cd918-199">Klicka på den **Lägg till användare**, och klicka sedan på **ny**.</span><span class="sxs-lookup"><span data-stu-id="cd918-199">Click the **Add Users**, and then click **New**.</span></span>

4. <span data-ttu-id="cd918-200">I den **Lägg till nya användare** avsnittet, utför följande steg i ett giltigt Azure AD-kontot som du vill etablera:</span><span class="sxs-lookup"><span data-stu-id="cd918-200">In the **Add New Users** section, perform the following steps of a valid Azure AD account you want to provision:</span></span>
   
    <span data-ttu-id="cd918-201">![Lägga till nya användare](./media/active-directory-saas-mindflash-tutorial/ic787141.png "lägga till nya användare")</span><span class="sxs-lookup"><span data-stu-id="cd918-201">![Add New Users](./media/active-directory-saas-mindflash-tutorial/ic787141.png "Add New Users")</span></span>
   
    <span data-ttu-id="cd918-202">a.</span><span class="sxs-lookup"><span data-stu-id="cd918-202">a.</span></span> <span data-ttu-id="cd918-203">I den **Förnamn** textruta typen **Förnamn** användaren **Britta**.</span><span class="sxs-lookup"><span data-stu-id="cd918-203">In the **First name** textbox, type **First name** of the user as **Britta**.</span></span>

    <span data-ttu-id="cd918-204">b.</span><span class="sxs-lookup"><span data-stu-id="cd918-204">b.</span></span> <span data-ttu-id="cd918-205">I den **efternamn** textruta typen **efternamn** användaren **Simon**.</span><span class="sxs-lookup"><span data-stu-id="cd918-205">In the **Last name** textbox, type **Last name** of the user as **Simon**.</span></span>
    
    <span data-ttu-id="cd918-206">c.</span><span class="sxs-lookup"><span data-stu-id="cd918-206">c.</span></span> <span data-ttu-id="cd918-207">I den **e-post** textruta typen **e-postadress** användaren  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="cd918-207">In the **Email** textbox, type **Email Address** of the user as **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="cd918-208">b.</span><span class="sxs-lookup"><span data-stu-id="cd918-208">b.</span></span> <span data-ttu-id="cd918-209">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="cd918-209">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="cd918-210">Du kan använda något annat Mindflash användarens konto skapas verktyg eller API: er som tillhandahålls av Mindflash att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="cd918-210">You can use any other Mindflash user account creation tools or APIs provided by Mindflash to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cd918-211">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cd918-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cd918-212">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Mindflash.</span><span class="sxs-lookup"><span data-stu-id="cd918-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mindflash.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cd918-214">**Om du vill tilldela Mindflash Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cd918-214">**To assign Britta Simon to Mindflash, perform the following steps:**</span></span>

1. <span data-ttu-id="cd918-215">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cd918-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cd918-217">Välj i listan med program **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="cd918-217">In the applications list, select **Mindflash**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_app.png) 

3. <span data-ttu-id="cd918-219">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cd918-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cd918-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cd918-221">Click **Add** button.</span></span> <span data-ttu-id="cd918-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd918-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cd918-224">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cd918-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cd918-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd918-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd918-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd918-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cd918-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd918-227">Testing single sign-on</span></span>

<span data-ttu-id="cd918-228">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cd918-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cd918-229">Du bör få inloggningssidan för Mindflash program när du klickar på panelen Mindflash på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cd918-229">When you click the Mindflash tile in the Access Panel, you should get login page of Mindflash application.</span></span>
<span data-ttu-id="cd918-230">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cd918-230">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd918-231">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cd918-231">Additional resources</span></span>

* [<span data-ttu-id="cd918-232">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cd918-232">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd918-233">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cd918-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_203.png

