---
title: "Självstudier: Azure Active Directory-integrering med Jostle | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Jostle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ca4ca1f-8f68-4225-81a6-1666b486d6a8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 0ca8aca1446a38643ce9f6751b6fe9cae1eaa5b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jostle"></a><span data-ttu-id="cd100-103">Självstudier: Azure Active Directory-integrering med Jostle</span><span class="sxs-lookup"><span data-stu-id="cd100-103">Tutorial: Azure Active Directory integration with Jostle</span></span>

<span data-ttu-id="cd100-104">I kursen får lära du att integrera Jostle med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cd100-104">In this tutorial, you learn how to integrate Jostle with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cd100-105">Integrera Jostle med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cd100-105">Integrating Jostle with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cd100-106">Du kan styra i Azure AD som har åtkomst till Jostle</span><span class="sxs-lookup"><span data-stu-id="cd100-106">You can control in Azure AD who has access to Jostle</span></span>
- <span data-ttu-id="cd100-107">Du kan aktivera användarna att automatiskt hämta loggat in på Jostle (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cd100-107">You can enable your users to automatically get signed-on to Jostle (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cd100-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cd100-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cd100-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cd100-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd100-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cd100-110">Prerequisites</span></span>

<span data-ttu-id="cd100-111">För att konfigurera Azure AD-integrering med Jostle, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cd100-111">To configure Azure AD integration with Jostle, you need the following items:</span></span>

- <span data-ttu-id="cd100-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cd100-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cd100-113">En Jostle enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="cd100-113">A Jostle single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cd100-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cd100-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cd100-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cd100-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cd100-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cd100-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cd100-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd100-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cd100-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cd100-118">Scenario description</span></span>
<span data-ttu-id="cd100-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cd100-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cd100-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cd100-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cd100-121">Att lägga till Jostle från galleriet</span><span class="sxs-lookup"><span data-stu-id="cd100-121">Adding Jostle from the gallery</span></span>
2. <span data-ttu-id="cd100-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd100-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jostle-from-the-gallery"></a><span data-ttu-id="cd100-123">Att lägga till Jostle från galleriet</span><span class="sxs-lookup"><span data-stu-id="cd100-123">Adding Jostle from the gallery</span></span>
<span data-ttu-id="cd100-124">Du måste lägga till Jostle från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Jostle i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cd100-124">To configure the integration of Jostle into Azure AD, you need to add Jostle from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cd100-125">**Utför följande steg för att lägga till Jostle från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cd100-125">**To add Jostle from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cd100-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cd100-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cd100-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cd100-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cd100-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cd100-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cd100-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd100-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cd100-133">I sökrutan skriver **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="cd100-133">In the search box, type **Jostle**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_search.png)

5. <span data-ttu-id="cd100-135">Välj i resultatpanelen **Jostle**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cd100-135">In the results panel, select **Jostle**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cd100-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd100-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cd100-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Jostle baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cd100-138">In this section, you configure and test Azure AD single sign-on with Jostle based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cd100-139">Azure AD måste du känna till användaren i Jostle motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cd100-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Jostle is to a user in Azure AD.</span></span> <span data-ttu-id="cd100-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Jostle upprättas.</span><span class="sxs-lookup"><span data-stu-id="cd100-140">In other words, a link relationship between an Azure AD user and the related user in Jostle needs to be established.</span></span>

<span data-ttu-id="cd100-141">I Jostle, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cd100-141">In Jostle, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cd100-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Jostle, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cd100-142">To configure and test Azure AD single sign-on with Jostle, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cd100-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cd100-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cd100-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd100-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cd100-145">**[Skapa en testanvändare Jostle](#creating-a-jostle-test-user)**  – du har en motsvarighet för Britta Simon i Jostle som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cd100-145">**[Creating a Jostle test user](#creating-a-jostle-test-user)** - to have a counterpart of Britta Simon in Jostle that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cd100-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cd100-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cd100-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cd100-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cd100-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd100-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cd100-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Jostle program.</span><span class="sxs-lookup"><span data-stu-id="cd100-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Jostle application.</span></span>

<span data-ttu-id="cd100-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Jostle:**</span><span class="sxs-lookup"><span data-stu-id="cd100-150">**To configure Azure AD single sign-on with Jostle, perform the following steps:**</span></span>

1. <span data-ttu-id="cd100-151">I Azure-portalen på den **Jostle** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cd100-151">In the Azure portal, on the **Jostle** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cd100-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cd100-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_samlbase.png)

3. <span data-ttu-id="cd100-155">På den **Jostle domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cd100-155">On the **Jostle Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_url.png)

    <span data-ttu-id="cd100-157">a.</span><span class="sxs-lookup"><span data-stu-id="cd100-157">a.</span></span> <span data-ttu-id="cd100-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tanent name>.jostle.us/jostle-prod/`</span><span class="sxs-lookup"><span data-stu-id="cd100-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tanent name>.jostle.us/jostle-prod/`</span></span>

    <span data-ttu-id="cd100-159">b.</span><span class="sxs-lookup"><span data-stu-id="cd100-159">b.</span></span> <span data-ttu-id="cd100-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<tanent name>.jostle.us`</span><span class="sxs-lookup"><span data-stu-id="cd100-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tanent name>.jostle.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cd100-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cd100-161">These values are not real.</span></span> <span data-ttu-id="cd100-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="cd100-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cd100-163">Kontakta [Jostle supportteamet](mailto:support@jostle.me) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cd100-163">Contact [Jostle support team](mailto:support@jostle.me) to get these values.</span></span> 
 


4. <span data-ttu-id="cd100-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cd100-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_certificate.png) 

5. <span data-ttu-id="cd100-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cd100-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jostle-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="cd100-168">Om du vill konfigurera enkel inloggning på Jostle sida, måste du skicka hämtade XML-metadata till [Jostle supportteamet](mailto:support@jostle.me).</span><span class="sxs-lookup"><span data-stu-id="cd100-168">To configure single sign-on on Jostle side, you need to send the downloaded metadata XML to [Jostle support team](mailto:support@jostle.me).</span></span> <span data-ttu-id="cd100-169">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="cd100-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span> 

> [!TIP]
> <span data-ttu-id="cd100-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cd100-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cd100-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cd100-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cd100-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cd100-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cd100-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cd100-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="cd100-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cd100-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cd100-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cd100-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cd100-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cd100-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cd100-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cd100-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cd100-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd100-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cd100-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cd100-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jostle-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cd100-185">a.</span><span class="sxs-lookup"><span data-stu-id="cd100-185">a.</span></span> <span data-ttu-id="cd100-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cd100-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cd100-187">b.</span><span class="sxs-lookup"><span data-stu-id="cd100-187">b.</span></span> <span data-ttu-id="cd100-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cd100-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cd100-189">c.</span><span class="sxs-lookup"><span data-stu-id="cd100-189">c.</span></span> <span data-ttu-id="cd100-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cd100-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cd100-191">d.</span><span class="sxs-lookup"><span data-stu-id="cd100-191">d.</span></span> <span data-ttu-id="cd100-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cd100-192">Click **Create**.</span></span>
 
### <a name="creating-a-jostle-test-user"></a><span data-ttu-id="cd100-193">Skapa en testanvändare Jostle</span><span class="sxs-lookup"><span data-stu-id="cd100-193">Creating a Jostle test user</span></span>

<span data-ttu-id="cd100-194">I det här avsnittet skapar du en användare som kallas Britta Simon i Jostle.</span><span class="sxs-lookup"><span data-stu-id="cd100-194">In this section, you create a user called Britta Simon in Jostle.</span></span> <span data-ttu-id="cd100-195">Om du inte vet hur du lägger till Britta Simon i Jostle kan du kontakta med [Jostle supportteamet](mailto:support@jostle.me) att lägga till användaren och aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cd100-195">If you don't know how to add Britta Simon in Jostle, please contact with [Jostle support team](mailto:support@jostle.me) to add the test user and enable SSO.</span></span>

> [!NOTE]
> <span data-ttu-id="cd100-196">Azure Active Directory kontoinnehavaren får ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="cd100-196">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cd100-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cd100-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cd100-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Jostle.</span><span class="sxs-lookup"><span data-stu-id="cd100-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Jostle.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cd100-200">**Om du vill tilldela Jostle Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cd100-200">**To assign Britta Simon to Jostle, perform the following steps:**</span></span>

1. <span data-ttu-id="cd100-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cd100-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cd100-203">Välj i listan med program **Jostle**.</span><span class="sxs-lookup"><span data-stu-id="cd100-203">In the applications list, select **Jostle**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jostle-tutorial/tutorial_jostle_app.png) 

3. <span data-ttu-id="cd100-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cd100-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cd100-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cd100-207">Click **Add** button.</span></span> <span data-ttu-id="cd100-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd100-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cd100-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cd100-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cd100-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd100-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cd100-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cd100-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cd100-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cd100-213">Testing single sign-on</span></span>

<span data-ttu-id="cd100-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cd100-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cd100-215">När du klickar på panelen Jostle på åtkomstpanelen ska du automatiskt hämta inloggningssidan för Jostle program.</span><span class="sxs-lookup"><span data-stu-id="cd100-215">When you click the Jostle tile in the Access Panel, you should get automatically login page of Jostle application.</span></span>
<span data-ttu-id="cd100-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cd100-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cd100-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cd100-217">Additional resources</span></span>

* [<span data-ttu-id="cd100-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cd100-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cd100-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cd100-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jostle-tutorial/tutorial_general_203.png

