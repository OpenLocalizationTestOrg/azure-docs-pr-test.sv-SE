---
title: "Självstudier: Azure Active Directory-integrering med 23 Video | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och 23 Video."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: ffcd665506c21e25c84367af5b6a3afb8e319af7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a><span data-ttu-id="ba7a1-103">Självstudier: Azure Active Directory-integrering med 23 Video</span><span class="sxs-lookup"><span data-stu-id="ba7a1-103">Tutorial: Azure Active Directory integration with 23 Video</span></span>

<span data-ttu-id="ba7a1-104">I kursen får lära du att integrera 23 Video med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ba7a1-104">In this tutorial, you learn how to integrate 23 Video with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ba7a1-105">Integrera 23 ger Video med Azure AD följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-105">Integrating 23 Video with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ba7a1-106">Du kan styra i Azure AD som har åtkomst till 23 Video</span><span class="sxs-lookup"><span data-stu-id="ba7a1-106">You can control in Azure AD who has access to 23 Video</span></span>
- <span data-ttu-id="ba7a1-107">Du kan aktivera användarna att automatiskt hämta loggat in på 23 Video (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ba7a1-107">You can enable your users to automatically get signed-on to 23 Video (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ba7a1-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ba7a1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ba7a1-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ba7a1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba7a1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ba7a1-110">Prerequisites</span></span>

<span data-ttu-id="ba7a1-111">För att konfigurera Azure AD-integrering med 23 Video, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-111">To configure Azure AD integration with 23 Video, you need the following items:</span></span>

- <span data-ttu-id="ba7a1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ba7a1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ba7a1-113">En 23 Video enkel inloggning på aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="ba7a1-113">A 23 Video single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ba7a1-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ba7a1-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ba7a1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ba7a1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ba7a1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ba7a1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ba7a1-118">Scenario description</span></span>
<span data-ttu-id="ba7a1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ba7a1-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ba7a1-121">Att lägga till 23 Video från galleriet</span><span class="sxs-lookup"><span data-stu-id="ba7a1-121">Adding 23 Video from the gallery</span></span>
2. <span data-ttu-id="ba7a1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba7a1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-23-video-from-the-gallery"></a><span data-ttu-id="ba7a1-123">Att lägga till 23 Video från galleriet</span><span class="sxs-lookup"><span data-stu-id="ba7a1-123">Adding 23 Video from the gallery</span></span>
<span data-ttu-id="ba7a1-124">Du måste lägga till 23 Video från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av 23 Video i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-124">To configure the integration of 23 Video into Azure AD, you need to add 23 Video from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ba7a1-125">**Utför följande steg för att lägga till 23 Video från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ba7a1-125">**To add 23 Video from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ba7a1-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ba7a1-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ba7a1-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ba7a1-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ba7a1-133">I sökrutan skriver **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-133">In the search box, type **23 Video**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_search.png)

5. <span data-ttu-id="ba7a1-135">Välj i resultatpanelen **23 Video**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-135">In the results panel, select **23 Video**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/tutorial_23video_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ba7a1-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba7a1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ba7a1-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 23 Video baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-138">In this section, you configure and test Azure AD single sign-on with 23 Video based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ba7a1-139">Azure AD måste du känna till användaren i 23 Video motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 23 Video is to a user in Azure AD.</span></span> <span data-ttu-id="ba7a1-140">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i 23 Video måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-140">In other words, a link relationship between an Azure AD user and the related user in 23 Video needs to be established.</span></span>

<span data-ttu-id="ba7a1-141">I 23 Video, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-141">In 23 Video, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ba7a1-142">Om du vill konfigurera och testa Azure AD enkel inloggning med 23 Video, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-142">To configure and test Azure AD single sign-on with 23 Video, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ba7a1-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ba7a1-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ba7a1-145">**[Skapa en 23 Video testanvändare](#creating-a-23-video-test-user)**  – du har en motsvarighet för Britta Simon i 23 videon som är länkad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-145">**[Creating a 23 Video test user](#creating-a-23-video-test-user)** - to have a counterpart of Britta Simon in 23 Video that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ba7a1-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ba7a1-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ba7a1-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba7a1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ba7a1-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet 23 Video.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 23 Video application.</span></span>

<span data-ttu-id="ba7a1-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med 23 Video:**</span><span class="sxs-lookup"><span data-stu-id="ba7a1-150">**To configure Azure AD single sign-on with 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="ba7a1-151">I Azure-portalen på den **23 Video** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-151">In the Azure portal, on the **23 Video** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ba7a1-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_samlbase.png)

3. <span data-ttu-id="ba7a1-155">På den **23 Video domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-155">On the **23 Video Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_url.png)

    <span data-ttu-id="ba7a1-157">a.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-157">a.</span></span> <span data-ttu-id="ba7a1-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.23video.com`</span><span class="sxs-lookup"><span data-stu-id="ba7a1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.23video.com`</span></span>

    <span data-ttu-id="ba7a1-159">b.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-159">b.</span></span> <span data-ttu-id="ba7a1-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://www.23video.com/saml/trust/<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="ba7a1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://www.23video.com/saml/trust/<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ba7a1-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-161">These values are not real.</span></span> <span data-ttu-id="ba7a1-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ba7a1-163">Kontakta [23 Video klienten supportteamet](mailto:support@23company.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-163">Contact [23 Video Client support team](mailto:support@23company.com) to get these values.</span></span> 
 
4. <span data-ttu-id="ba7a1-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_certificate.png) 

5. <span data-ttu-id="ba7a1-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ba7a1-168">På den **23 Video Configuration** klickar du på **konfigurera 23 Video** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-168">On the **23 Video Configuration** section, click **Configure 23 Video** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ba7a1-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ba7a1-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_configure.png) 

7. <span data-ttu-id="ba7a1-171">Konfigurera enkel inloggning på **23 Video** sida, måste du skicka den hämtade **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [23 Video supportteamet](mailto:support@23company.com).</span><span class="sxs-lookup"><span data-stu-id="ba7a1-171">To configure single sign-on on **23 Video** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [23 Video support team](mailto:support@23company.com).</span></span> 


> [!TIP]
> <span data-ttu-id="ba7a1-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ba7a1-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ba7a1-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ba7a1-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ba7a1-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ba7a1-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ba7a1-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="ba7a1-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ba7a1-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ba7a1-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ba7a1-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ba7a1-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ba7a1-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ba7a1-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ba7a1-187">a.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-187">a.</span></span> <span data-ttu-id="ba7a1-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ba7a1-189">b.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-189">b.</span></span> <span data-ttu-id="ba7a1-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ba7a1-191">c.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-191">c.</span></span> <span data-ttu-id="ba7a1-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ba7a1-193">d.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-193">d.</span></span> <span data-ttu-id="ba7a1-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-194">Click **Create**.</span></span>
 
### <a name="creating-a-23-video-test-user"></a><span data-ttu-id="ba7a1-195">Skapa en 23 Video testanvändare</span><span class="sxs-lookup"><span data-stu-id="ba7a1-195">Creating a 23 Video test user</span></span>

<span data-ttu-id="ba7a1-196">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i 23 videon.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-196">The objective of this section is to create a user called Britta Simon in 23 Video.</span></span>

<span data-ttu-id="ba7a1-197">**Utför följande steg för att skapa en användare som kallas Britta Simon i 23 videon:**</span><span class="sxs-lookup"><span data-stu-id="ba7a1-197">**To create a user called Britta Simon in 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="ba7a1-198">Logga in på webbplatsen företagets 23 Video som administratör.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-198">Sign on to your 23 Video company site as administrator.</span></span>

2. <span data-ttu-id="ba7a1-199">Gå till **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-199">Go to **Settings**.</span></span>
 
3. <span data-ttu-id="ba7a1-200">I **användare** klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-200">In **Users** section, click **Configure**.</span></span>
   
    ![Tilldela användare][400]

4. <span data-ttu-id="ba7a1-202">Klicka på **lägga till en ny användare**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-202">Click **Add a new user**.</span></span> 
   
    ![Tilldela användare][401]

5. <span data-ttu-id="ba7a1-204">I den **bjuda in någon att ansluta till den här platsen** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ba7a1-204">In the **Invite someone to join this site** section, perform the following steps:</span></span>
   
    ![Tilldela användare][402]

    <span data-ttu-id="ba7a1-206">a.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-206">a.</span></span> <span data-ttu-id="ba7a1-207">I den **e-postadresser** textruta Skriv Britta Simon e-postadress i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-207">In the **E-mail addresses** textbox, type Britta Simon's email address in Azure AD.</span></span>  
 
    <span data-ttu-id="ba7a1-208">b.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-208">b.</span></span> <span data-ttu-id="ba7a1-209">Klicka på **lägga till användaren**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-209">Click **Add the user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ba7a1-210">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ba7a1-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ba7a1-211">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till 23 Video.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 23 Video.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ba7a1-213">**Om du vill tilldela 23 Video Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ba7a1-213">**To assign Britta Simon to 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="ba7a1-214">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ba7a1-216">Välj i listan med program **23 Video**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-216">In the applications list, select **23 Video**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-23video-tutorial/tutorial_23video_app.png) 

3. <span data-ttu-id="ba7a1-218">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ba7a1-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-220">Click **Add** button.</span></span> <span data-ttu-id="ba7a1-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ba7a1-223">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ba7a1-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ba7a1-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ba7a1-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ba7a1-226">Testing single sign-on</span></span>

<span data-ttu-id="ba7a1-227">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="ba7a1-228">När du klickar på panelen 23 videon på åtkomstpanelen du bör få automatiskt loggat in på ditt 23 Video-program.</span><span class="sxs-lookup"><span data-stu-id="ba7a1-228">When you click the 23 Video tile in the Access Panel, you should get automatically signed-on to your 23 Video application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ba7a1-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ba7a1-229">Additional resources</span></span>

* [<span data-ttu-id="ba7a1-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ba7a1-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ba7a1-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ba7a1-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png
