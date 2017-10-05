---
title: "Självstudier: Azure Active Directory-integrering med Trakstar | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Trakstar."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411cb8c3-95c6-4138-acf2-ffc7f663e89a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 757429aa187e6536489b6636a0a11d122c7f9378
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-trakstar"></a><span data-ttu-id="326ab-103">Självstudier: Azure Active Directory-integrering med Trakstar</span><span class="sxs-lookup"><span data-stu-id="326ab-103">Tutorial: Azure Active Directory integration with Trakstar</span></span>

<span data-ttu-id="326ab-104">I kursen får lära du att integrera Trakstar med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="326ab-104">In this tutorial, you learn how to integrate Trakstar with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="326ab-105">Integrera Trakstar med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="326ab-105">Integrating Trakstar with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="326ab-106">Du kan styra i Azure AD som har åtkomst till Trakstar</span><span class="sxs-lookup"><span data-stu-id="326ab-106">You can control in Azure AD who has access to Trakstar</span></span>
- <span data-ttu-id="326ab-107">Du kan aktivera användarna att automatiskt hämta loggat in på Trakstar (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="326ab-107">You can enable your users to automatically get signed-on to Trakstar (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="326ab-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="326ab-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="326ab-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="326ab-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="326ab-110">Krav</span><span class="sxs-lookup"><span data-stu-id="326ab-110">Prerequisites</span></span>

<span data-ttu-id="326ab-111">För att konfigurera Azure AD-integrering med Trakstar, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="326ab-111">To configure Azure AD integration with Trakstar, you need the following items:</span></span>

- <span data-ttu-id="326ab-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="326ab-112">An Azure AD subscription</span></span>
- <span data-ttu-id="326ab-113">En Trakstar enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="326ab-113">A Trakstar single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="326ab-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="326ab-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="326ab-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="326ab-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="326ab-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="326ab-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="326ab-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="326ab-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="326ab-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="326ab-118">Scenario description</span></span>
<span data-ttu-id="326ab-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="326ab-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="326ab-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="326ab-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="326ab-121">Att lägga till Trakstar från galleriet</span><span class="sxs-lookup"><span data-stu-id="326ab-121">Adding Trakstar from the gallery</span></span>
2. <span data-ttu-id="326ab-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="326ab-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-trakstar-from-the-gallery"></a><span data-ttu-id="326ab-123">Att lägga till Trakstar från galleriet</span><span class="sxs-lookup"><span data-stu-id="326ab-123">Adding Trakstar from the gallery</span></span>
<span data-ttu-id="326ab-124">Du måste lägga till Trakstar från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Trakstar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="326ab-124">To configure the integration of Trakstar into Azure AD, you need to add Trakstar from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="326ab-125">**Utför följande steg för att lägga till Trakstar från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="326ab-125">**To add Trakstar from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="326ab-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="326ab-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="326ab-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="326ab-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="326ab-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="326ab-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="326ab-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="326ab-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="326ab-133">I sökrutan skriver **Trakstar**.</span><span class="sxs-lookup"><span data-stu-id="326ab-133">In the search box, type **Trakstar**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_search.png)

5. <span data-ttu-id="326ab-135">Välj i resultatpanelen **Trakstar**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="326ab-135">In the results panel, select **Trakstar**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="326ab-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="326ab-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="326ab-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Trakstar baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="326ab-138">In this section, you configure and test Azure AD single sign-on with Trakstar based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="326ab-139">Azure AD måste du känna till användaren i Trakstar motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="326ab-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Trakstar is to a user in Azure AD.</span></span> <span data-ttu-id="326ab-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Trakstar upprättas.</span><span class="sxs-lookup"><span data-stu-id="326ab-140">In other words, a link relationship between an Azure AD user and the related user in Trakstar needs to be established.</span></span>

<span data-ttu-id="326ab-141">I Trakstar, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="326ab-141">In Trakstar, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="326ab-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Trakstar, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="326ab-142">To configure and test Azure AD single sign-on with Trakstar, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="326ab-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="326ab-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="326ab-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="326ab-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="326ab-145">**[Skapa en testanvändare Trakstar](#creating-a-trakstar-test-user)**  – du har en motsvarighet för Britta Simon i Trakstar som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="326ab-145">**[Creating a Trakstar test user](#creating-a-trakstar-test-user)** - to have a counterpart of Britta Simon in Trakstar that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="326ab-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="326ab-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="326ab-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="326ab-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="326ab-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="326ab-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="326ab-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Trakstar program.</span><span class="sxs-lookup"><span data-stu-id="326ab-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Trakstar application.</span></span>

<span data-ttu-id="326ab-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Trakstar:**</span><span class="sxs-lookup"><span data-stu-id="326ab-150">**To configure Azure AD single sign-on with Trakstar, perform the following steps:**</span></span>

1. <span data-ttu-id="326ab-151">I Azure-portalen på den **Trakstar** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="326ab-151">In the Azure portal, on the **Trakstar** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="326ab-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="326ab-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_samlbase.png)

3. <span data-ttu-id="326ab-155">På den **Trakstar domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="326ab-155">On the **Trakstar Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_url.png)

    <span data-ttu-id="326ab-157">a.</span><span class="sxs-lookup"><span data-stu-id="326ab-157">a.</span></span> <span data-ttu-id="326ab-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://app.trakstar.com/auth/saml/callback?namespace=<NAMESPACE>`</span><span class="sxs-lookup"><span data-stu-id="326ab-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.trakstar.com/auth/saml/callback?namespace=<NAMESPACE>`</span></span>

    <span data-ttu-id="326ab-159">b.</span><span class="sxs-lookup"><span data-stu-id="326ab-159">b.</span></span> <span data-ttu-id="326ab-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.trakstar.com`</span><span class="sxs-lookup"><span data-stu-id="326ab-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.trakstar.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="326ab-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="326ab-161">These values are not real.</span></span> <span data-ttu-id="326ab-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="326ab-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="326ab-163">Kontakta [Trakstar klienten supportteamet](mailto:integrations@trakstar.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="326ab-163">Contact [Trakstar Client support team](mailto:integrations@trakstar.com) to get these values.</span></span> 
 
4. <span data-ttu-id="326ab-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="326ab-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_certificate.png) 

5. <span data-ttu-id="326ab-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="326ab-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakstar-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="326ab-168">På den **Trakstar Configuration** klickar du på **konfigurera Trakstar** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="326ab-168">On the **Trakstar Configuration** section, click **Configure Trakstar** to open **Configure sign-on** window.</span></span> <span data-ttu-id="326ab-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="326ab-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_configure.png) 

7. <span data-ttu-id="326ab-171">Konfigurera enkel inloggning på **Trakstar** sida, måste du skicka den hämtade **certifikat (Base64)**, **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**till [Trakstar supportteamet](mailto:integrations@trakstar.com).</span><span class="sxs-lookup"><span data-stu-id="326ab-171">To configure single sign-on on **Trakstar** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Trakstar support team](mailto:integrations@trakstar.com).</span></span> 

> [!TIP]
> <span data-ttu-id="326ab-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="326ab-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="326ab-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="326ab-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="326ab-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="326ab-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="326ab-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="326ab-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="326ab-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="326ab-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="326ab-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="326ab-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="326ab-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="326ab-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakstar-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="326ab-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="326ab-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakstar-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="326ab-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="326ab-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakstar-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="326ab-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="326ab-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-trakstar-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="326ab-187">a.</span><span class="sxs-lookup"><span data-stu-id="326ab-187">a.</span></span> <span data-ttu-id="326ab-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="326ab-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="326ab-189">b.</span><span class="sxs-lookup"><span data-stu-id="326ab-189">b.</span></span> <span data-ttu-id="326ab-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="326ab-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="326ab-191">c.</span><span class="sxs-lookup"><span data-stu-id="326ab-191">c.</span></span> <span data-ttu-id="326ab-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="326ab-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="326ab-193">d.</span><span class="sxs-lookup"><span data-stu-id="326ab-193">d.</span></span> <span data-ttu-id="326ab-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="326ab-194">Click **Create**.</span></span>
 
### <a name="creating-a-trakstar-test-user"></a><span data-ttu-id="326ab-195">Skapa en testanvändare Trakstar</span><span class="sxs-lookup"><span data-stu-id="326ab-195">Creating a Trakstar test user</span></span>

<span data-ttu-id="326ab-196">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Trakstar.</span><span class="sxs-lookup"><span data-stu-id="326ab-196">The objective of this section is to create a user called Britta Simon in Trakstar.</span></span> <span data-ttu-id="326ab-197">Arbeta med [Trakstar supportteamet](mailto:integrations@trakstar.com) att lägga till användare i Trakstar-konto.</span><span class="sxs-lookup"><span data-stu-id="326ab-197">Work with [Trakstar support team](mailto:integrations@trakstar.com) to add the users in the Trakstar account.</span></span> 


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="326ab-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="326ab-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="326ab-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Trakstar.</span><span class="sxs-lookup"><span data-stu-id="326ab-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Trakstar.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="326ab-201">**Om du vill tilldela Trakstar Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="326ab-201">**To assign Britta Simon to Trakstar, perform the following steps:**</span></span>

1. <span data-ttu-id="326ab-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="326ab-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="326ab-204">Välj i listan med program **Trakstar**.</span><span class="sxs-lookup"><span data-stu-id="326ab-204">In the applications list, select **Trakstar**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-trakstar-tutorial/tutorial_trakstar_app.png) 

3. <span data-ttu-id="326ab-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="326ab-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="326ab-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="326ab-208">Click **Add** button.</span></span> <span data-ttu-id="326ab-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="326ab-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="326ab-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="326ab-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="326ab-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="326ab-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="326ab-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="326ab-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="326ab-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="326ab-214">Testing single sign-on</span></span>

<span data-ttu-id="326ab-215">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="326ab-215">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="326ab-216">När du klickar på panelen Trakstar på åtkomstpanelen du bör få automatiskt loggat in på ditt Trakstar program.</span><span class="sxs-lookup"><span data-stu-id="326ab-216">When you click the Trakstar tile in the Access Panel, you should get automatically signed-on to your Trakstar application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="326ab-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="326ab-217">Additional resources</span></span>

* [<span data-ttu-id="326ab-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="326ab-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="326ab-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="326ab-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trakstar-tutorial/tutorial_general_203.png

