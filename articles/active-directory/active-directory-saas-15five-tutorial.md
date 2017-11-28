---
title: "Självstudier: Azure Active Directory-integrering med 15Five | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och 15Five."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2fb301c2-7d7a-4046-8ee1-7dc9e7684806
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: ea36774747a0fcfa4ace1aefb2d46dba815d92c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-15five"></a><span data-ttu-id="1a2d5-103">Självstudier: Azure Active Directory-integrering med 15Five</span><span class="sxs-lookup"><span data-stu-id="1a2d5-103">Tutorial: Azure Active Directory integration with 15Five</span></span>

<span data-ttu-id="1a2d5-104">I kursen får lära du att integrera 15Five med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1a2d5-104">In this tutorial, you learn how to integrate 15Five with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1a2d5-105">Integrera 15Five med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-105">Integrating 15Five with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1a2d5-106">Du kan styra i Azure AD som har åtkomst till 15Five</span><span class="sxs-lookup"><span data-stu-id="1a2d5-106">You can control in Azure AD who has access to 15Five</span></span>
- <span data-ttu-id="1a2d5-107">Du kan aktivera användarna att automatiskt hämta loggat in på 15Five (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1a2d5-107">You can enable your users to automatically get signed-on to 15Five (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1a2d5-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1a2d5-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1a2d5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1a2d5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a2d5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1a2d5-110">Prerequisites</span></span>

<span data-ttu-id="1a2d5-111">För att konfigurera Azure AD-integrering med 15Five, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-111">To configure Azure AD integration with 15Five, you need the following items:</span></span>

- <span data-ttu-id="1a2d5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1a2d5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="1a2d5-113">En 15Five enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="1a2d5-113">A 15Five single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1a2d5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1a2d5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1a2d5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1a2d5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1a2d5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1a2d5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="1a2d5-118">Scenario description</span></span>
<span data-ttu-id="1a2d5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1a2d5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1a2d5-121">Att lägga till 15Five från galleriet</span><span class="sxs-lookup"><span data-stu-id="1a2d5-121">Adding 15Five from the gallery</span></span>
2. <span data-ttu-id="1a2d5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a2d5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-15five-from-the-gallery"></a><span data-ttu-id="1a2d5-123">Att lägga till 15Five från galleriet</span><span class="sxs-lookup"><span data-stu-id="1a2d5-123">Adding 15Five from the gallery</span></span>
<span data-ttu-id="1a2d5-124">Du måste lägga till 15Five från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av 15Five i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-124">To configure the integration of 15Five into Azure AD, you need to add 15Five from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1a2d5-125">**Utför följande steg för att lägga till 15Five från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="1a2d5-125">**To add 15Five from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1a2d5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1a2d5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1a2d5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="1a2d5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="1a2d5-133">I sökrutan skriver **15Five**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-133">In the search box, type **15Five**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_search.png)

5. <span data-ttu-id="1a2d5-135">Välj i resultatpanelen **15Five**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-135">In the results panel, select **15Five**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/tutorial_15five_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1a2d5-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a2d5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1a2d5-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 15Five baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-138">In this section, you configure and test Azure AD single sign-on with 15Five based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1a2d5-139">Azure AD måste du känna till användaren i 15Five motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 15Five is to a user in Azure AD.</span></span> <span data-ttu-id="1a2d5-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i 15Five upprättas.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-140">In other words, a link relationship between an Azure AD user and the related user in 15Five needs to be established.</span></span>

<span data-ttu-id="1a2d5-141">I 15Five, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-141">In 15Five, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="1a2d5-142">Om du vill konfigurera och testa Azure AD enkel inloggning med 15Five, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-142">To configure and test Azure AD single sign-on with 15Five, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1a2d5-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1a2d5-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1a2d5-145">**[Skapa en testanvändare 15Five](#creating-a-15five-test-user)**  – du har en motsvarighet för Britta Simon i 15Five som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-145">**[Creating a 15Five test user](#creating-a-15five-test-user)** - to have a counterpart of Britta Simon in 15Five that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1a2d5-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1a2d5-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1a2d5-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a2d5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1a2d5-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt 15Five program.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 15Five application.</span></span>

<span data-ttu-id="1a2d5-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med 15Five:**</span><span class="sxs-lookup"><span data-stu-id="1a2d5-150">**To configure Azure AD single sign-on with 15Five, perform the following steps:**</span></span>

1. <span data-ttu-id="1a2d5-151">I Azure-portalen på den **15Five** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-151">In the Azure portal, on the **15Five** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="1a2d5-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_samlbase.png)

3. <span data-ttu-id="1a2d5-155">På den **15Five domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-155">On the **15Five Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_url.png)

    <span data-ttu-id="1a2d5-157">a.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-157">a.</span></span> <span data-ttu-id="1a2d5-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.15five.com`</span><span class="sxs-lookup"><span data-stu-id="1a2d5-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.15five.com`</span></span>

    <span data-ttu-id="1a2d5-159">b.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-159">b.</span></span> <span data-ttu-id="1a2d5-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.15five.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="1a2d5-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.15five.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="1a2d5-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-161">These values are not real.</span></span> <span data-ttu-id="1a2d5-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="1a2d5-163">Kontakta [15Five klienten supportteamet](https://www.15five.com/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-163">Contact [15Five Client support team](https://www.15five.com/contact/) to get these values.</span></span> 
 
4. <span data-ttu-id="1a2d5-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_certificate.png) 

5. <span data-ttu-id="1a2d5-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="1a2d5-168">Konfigurera enkel inloggning på **15Five** sida, måste du skicka den hämtade **XML-Metadata för** till [15Five supportteamet](https://www.15five.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="1a2d5-168">To configure single sign-on on **15Five** side, you need to send the downloaded **Metadata XML** to [15Five support team](https://www.15five.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="1a2d5-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="1a2d5-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1a2d5-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1a2d5-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1a2d5-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1a2d5-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1a2d5-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="1a2d5-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="1a2d5-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="1a2d5-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1a2d5-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1a2d5-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1a2d5-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1a2d5-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-15five-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1a2d5-184">a.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-184">a.</span></span> <span data-ttu-id="1a2d5-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1a2d5-186">b.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-186">b.</span></span> <span data-ttu-id="1a2d5-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1a2d5-188">c.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-188">c.</span></span> <span data-ttu-id="1a2d5-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1a2d5-190">d.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-190">d.</span></span> <span data-ttu-id="1a2d5-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-191">Click **Create**.</span></span>
 
### <a name="creating-a-15five-test-user"></a><span data-ttu-id="1a2d5-192">Skapa en testanvändare 15Five</span><span class="sxs-lookup"><span data-stu-id="1a2d5-192">Creating a 15Five test user</span></span>

<span data-ttu-id="1a2d5-193">Om du vill aktivera Azure AD-användare kan logga in på 15Five etableras de i 15Five.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-193">To enable Azure AD users to log in to 15Five, they must be provisioned into 15Five.</span></span> <span data-ttu-id="1a2d5-194">När 15Five, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-194">When 15Five, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="1a2d5-195">Utför följande steg för att konfigurera användaretablering:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-195">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="1a2d5-196">Logga in på ditt **15Five** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-196">Log in to your **15Five** company site as administrator.</span></span>

2. <span data-ttu-id="1a2d5-197">Gå till **hantera företagets**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-197">Go to **Manage Company**.</span></span>
   
    <span data-ttu-id="1a2d5-198">![Hantera företagets](./media/active-directory-saas-15five-tutorial/IC784675.png "hantera företagets")</span><span class="sxs-lookup"><span data-stu-id="1a2d5-198">![Manage Company](./media/active-directory-saas-15five-tutorial/IC784675.png "Manage Company")</span></span>

3. <span data-ttu-id="1a2d5-199">Gå till **personer \> lägga till personer**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-199">Go to **People \> Add People**.</span></span>
   
    <span data-ttu-id="1a2d5-200">![Personer](./media/active-directory-saas-15five-tutorial/IC784676.png "personer")</span><span class="sxs-lookup"><span data-stu-id="1a2d5-200">![People](./media/active-directory-saas-15five-tutorial/IC784676.png "People")</span></span>

4. <span data-ttu-id="1a2d5-201">Utför följande steg i avsnittet Lägg till ny Person:</span><span class="sxs-lookup"><span data-stu-id="1a2d5-201">In the Add New Person section, perform the following steps:</span></span>
   
    <span data-ttu-id="1a2d5-202">![Lägg till ny Person](./media/active-directory-saas-15five-tutorial/IC784677.png "Lägg till ny Person")</span><span class="sxs-lookup"><span data-stu-id="1a2d5-202">![Add New Person](./media/active-directory-saas-15five-tutorial/IC784677.png "Add New Person")</span></span>
   
    <span data-ttu-id="1a2d5-203">a.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-203">a.</span></span> <span data-ttu-id="1a2d5-204">Typ av **Förnamn**, **efternamn**, **rubrik**, **e-postadress** för ett giltigt Azure Active Directory-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-204">Type the **First Name**, **Last Name**, **Title**, **Email address** of a valid Azure Active Directory account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="1a2d5-205">b.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-205">b.</span></span> <span data-ttu-id="1a2d5-206">Klicka på **Klar**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-206">Click **Done**.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="1a2d5-207">Azure AD-kontoinnehavaren får ett e-postmeddelande med en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-207">The Azure AD account holder receives an email including a link to confirm the account before it becomes active.</span></span>
   
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1a2d5-208">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="1a2d5-208">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1a2d5-209">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till 15Five.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-209">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 15Five.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1a2d5-211">**Om du vill tilldela 15Five Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1a2d5-211">**To assign Britta Simon to 15Five, perform the following steps:**</span></span>

1. <span data-ttu-id="1a2d5-212">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-212">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="1a2d5-214">Välj i listan med program **15Five**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-214">In the applications list, select **15Five**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-15five-tutorial/tutorial_15five_app.png) 

3. <span data-ttu-id="1a2d5-216">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-216">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="1a2d5-218">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-218">Click **Add** button.</span></span> <span data-ttu-id="1a2d5-219">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="1a2d5-221">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-221">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1a2d5-222">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1a2d5-223">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1a2d5-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1a2d5-224">Testing single sign-on</span></span>

<span data-ttu-id="1a2d5-225">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-225">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="1a2d5-226">Du bör få inloggningssidan för 15Five program när du klickar på panelen 15Five på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1a2d5-226">When you click the 15Five tile in the Access Panel, you should get login page of 15Five application.</span></span>
<span data-ttu-id="1a2d5-227">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1a2d5-227">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1a2d5-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1a2d5-228">Additional resources</span></span>

* [<span data-ttu-id="1a2d5-229">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1a2d5-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1a2d5-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1a2d5-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-15five-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-15five-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-15five-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-15five-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-15five-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-15five-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-15five-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-15five-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-15five-tutorial/tutorial_general_203.png

