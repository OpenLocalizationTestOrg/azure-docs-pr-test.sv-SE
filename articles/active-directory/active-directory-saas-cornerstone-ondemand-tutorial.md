---
title: "Självstudier: Azure Active Directory-integrering med hörnstenarna OnDemand | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och hörnstenarna OnDemand."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 8bb32588579a0d40b9ae7e0f823c6daab21c856e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a><span data-ttu-id="95df9-103">Självstudier: Azure Active Directory-integrering med hörnstenarna OnDemand</span><span class="sxs-lookup"><span data-stu-id="95df9-103">Tutorial: Azure Active Directory integration with Cornerstone OnDemand</span></span>

<span data-ttu-id="95df9-104">I kursen får lära du att integrera hörnstenarna OnDemand med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="95df9-104">In this tutorial, you learn how to integrate Cornerstone OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="95df9-105">Integrera hörnstenarna OnDemand med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="95df9-105">Integrating Cornerstone OnDemand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="95df9-106">Du kan styra i Azure AD som har åtkomst till hörnstenarna OnDemand</span><span class="sxs-lookup"><span data-stu-id="95df9-106">You can control in Azure AD who has access to Cornerstone OnDemand</span></span>
- <span data-ttu-id="95df9-107">Du kan aktivera användarna att automatiskt hämta loggat in på hörnstenarna OnDemand (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="95df9-107">You can enable your users to automatically get signed-on to Cornerstone OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="95df9-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="95df9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="95df9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="95df9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95df9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="95df9-110">Prerequisites</span></span>

<span data-ttu-id="95df9-111">Om du vill konfigurera Azure AD-integrering med hörnstenarna OnDemand behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="95df9-111">To configure Azure AD integration with Cornerstone OnDemand, you need the following items:</span></span>

- <span data-ttu-id="95df9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="95df9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="95df9-113">En hörnsten OnDemand enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="95df9-113">A Cornerstone OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="95df9-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="95df9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="95df9-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="95df9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="95df9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="95df9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="95df9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="95df9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="95df9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="95df9-118">Scenario description</span></span>
<span data-ttu-id="95df9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="95df9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="95df9-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="95df9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="95df9-121">Att lägga till hörnstenarna OnDemand från galleriet</span><span class="sxs-lookup"><span data-stu-id="95df9-121">Adding Cornerstone OnDemand from the gallery</span></span>
2. <span data-ttu-id="95df9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95df9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a><span data-ttu-id="95df9-123">Att lägga till hörnstenarna OnDemand från galleriet</span><span class="sxs-lookup"><span data-stu-id="95df9-123">Adding Cornerstone OnDemand from the gallery</span></span>
<span data-ttu-id="95df9-124">Du måste lägga till hörnstenarna OnDemand från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av hörnstenarna OnDemand i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="95df9-124">To configure the integration of Cornerstone OnDemand into Azure AD, you need to add Cornerstone OnDemand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="95df9-125">**Utför följande steg för att lägga till hörnstenarna OnDemand från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="95df9-125">**To add Cornerstone OnDemand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="95df9-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="95df9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="95df9-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="95df9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="95df9-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="95df9-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="95df9-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95df9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="95df9-133">I sökrutan skriver **hörnstenarna OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="95df9-133">In the search box, type **Cornerstone OnDemand**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. <span data-ttu-id="95df9-135">Välj i resultatpanelen **hörnstenarna OnDemand**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="95df9-135">In the results panel, select **Cornerstone OnDemand**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="95df9-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95df9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="95df9-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med hörnstenarna OnDemand baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="95df9-138">In this section, you configure and test Azure AD single sign-on with Cornerstone OnDemand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="95df9-139">Azure AD måste du känna till användaren i hörnstenarna OnDemand motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="95df9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cornerstone OnDemand is to a user in Azure AD.</span></span> <span data-ttu-id="95df9-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i hörnstenarna OnDemand upprättas.</span><span class="sxs-lookup"><span data-stu-id="95df9-140">In other words, a link relationship between an Azure AD user and the related user in Cornerstone OnDemand needs to be established.</span></span>

<span data-ttu-id="95df9-141">I grunden OnDemand tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="95df9-141">In Cornerstone OnDemand, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="95df9-142">Om du vill konfigurera och testa Azure AD enkel inloggning med hörnstenarna OnDemand, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="95df9-142">To configure and test Azure AD single sign-on with Cornerstone OnDemand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="95df9-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="95df9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="95df9-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95df9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="95df9-145">**[Skapa en testanvändare hörnstenarna OnDemand](#creating-a-cornerstone-ondemand-test-user)**  – har en motsvarighet för Britta Simon hörnstenarna OnDemand som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="95df9-145">**[Creating a Cornerstone OnDemand test user](#creating-a-cornerstone-ondemand-test-user)** - to have a counterpart of Britta Simon in Cornerstone OnDemand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="95df9-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="95df9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="95df9-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="95df9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="95df9-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95df9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="95df9-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt hörnstenarna OnDemand-program.</span><span class="sxs-lookup"><span data-stu-id="95df9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cornerstone OnDemand application.</span></span>

<span data-ttu-id="95df9-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med hörnstenarna OnDemand:**</span><span class="sxs-lookup"><span data-stu-id="95df9-150">**To configure Azure AD single sign-on with Cornerstone OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="95df9-151">I Azure-portalen på den **hörnstenarna OnDemand** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="95df9-151">In the Azure portal, on the **Cornerstone OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="95df9-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="95df9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. <span data-ttu-id="95df9-155">På den **hörnstenarna OnDemand domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="95df9-155">On the **Cornerstone OnDemand Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    <span data-ttu-id="95df9-157">a.</span><span class="sxs-lookup"><span data-stu-id="95df9-157">a.</span></span> <span data-ttu-id="95df9-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="95df9-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.csod.com`</span></span>

    <span data-ttu-id="95df9-159">b.</span><span class="sxs-lookup"><span data-stu-id="95df9-159">b.</span></span> <span data-ttu-id="95df9-160">I **identifierare** textruta Skriv en URL med följande mönster:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="95df9-160">In **Identifier** textbox, type a URL using the following pattern: `https://<company>.csod.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="95df9-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="95df9-161">These values are not real.</span></span> <span data-ttu-id="95df9-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="95df9-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="95df9-163">Kontakta [hörnstenarna OnDemand klienten supportteamet](mailTo:moreinfo@csod.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="95df9-163">Contact [Cornerstone OnDemand Client support team](mailTo:moreinfo@csod.com) to get these values.</span></span> 
 
4. <span data-ttu-id="95df9-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="95df9-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. <span data-ttu-id="95df9-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="95df9-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="95df9-168">På den **hörnstenarna OnDemand Configuration** klickar du på **konfigurera hörnstenarna OnDemand** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="95df9-168">On the **Cornerstone OnDemand Configuration** section, click **Configure Cornerstone OnDemand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="95df9-169">Kopiera den **Sign-Out Webbadressen och SAML enkel inloggning Service** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="95df9-169">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. <span data-ttu-id="95df9-171">Konfigurera enkel inloggning på **hörnstenarna OnDemand** sida, måste du skicka den hämtade **certifikat**, **Sign-Out URL** och **SAML enkel inloggning Tjänstwebbadress** till [hörnstenarna OnDemand supportteamet](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="95df9-171">To configure single sign-on on **Cornerstone OnDemand** side, you need to send the downloaded **Certificate**, **Sign-Out URL** and **SAML Single Sign-On Service URL**  to [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span> <span data-ttu-id="95df9-172">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="95df9-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="95df9-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="95df9-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="95df9-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="95df9-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="95df9-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="95df9-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="95df9-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="95df9-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="95df9-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="95df9-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="95df9-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="95df9-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="95df9-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="95df9-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="95df9-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="95df9-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="95df9-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95df9-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="95df9-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="95df9-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="95df9-188">a.</span><span class="sxs-lookup"><span data-stu-id="95df9-188">a.</span></span> <span data-ttu-id="95df9-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="95df9-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="95df9-190">b.</span><span class="sxs-lookup"><span data-stu-id="95df9-190">b.</span></span> <span data-ttu-id="95df9-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="95df9-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="95df9-192">c.</span><span class="sxs-lookup"><span data-stu-id="95df9-192">c.</span></span> <span data-ttu-id="95df9-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="95df9-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="95df9-194">d.</span><span class="sxs-lookup"><span data-stu-id="95df9-194">d.</span></span> <span data-ttu-id="95df9-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="95df9-195">Click **Create**.</span></span>
 
### <a name="creating-a-cornerstone-ondemand-test-user"></a><span data-ttu-id="95df9-196">Skapa en OnDemand hörnstenarna testanvändare</span><span class="sxs-lookup"><span data-stu-id="95df9-196">Creating a Cornerstone OnDemand test user</span></span>

<span data-ttu-id="95df9-197">För att aktivera Azure AD-användare att logga in på hörnstenarna OnDemand etableras de i hörnstenarna OnDemand.</span><span class="sxs-lookup"><span data-stu-id="95df9-197">In order to enable Azure AD users to log into Cornerstone OnDemand, they must be provisioned into Cornerstone OnDemand.</span></span> <span data-ttu-id="95df9-198">När det gäller hörnstenarna OnDemand är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="95df9-198">In the case of Cornerstone OnDemand, provisioning is a manual task.</span></span>

<span data-ttu-id="95df9-199">För att konfigurera användaretablering, skicka information (t.ex.: namn, e-post) om Azure AD-användare du vill etablera till den [hörnstenarna OnDemand supportteamet](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="95df9-199">To configure user provisioning, send the information (e.g.: Name, Email) about the Azure AD user you want to provision to the [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span>

>[!NOTE]
><span data-ttu-id="95df9-200">Du kan använda något annat hörnstenarna OnDemand användarens konto skapas verktyg eller API: er som tillhandahålls av hörnstenarna OnDemand etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="95df9-200">You can use any other Cornerstone OnDemand user account creation tools or APIs provided by Cornerstone OnDemand to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="95df9-201">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="95df9-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="95df9-202">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till hörnstenarna OnDemand.</span><span class="sxs-lookup"><span data-stu-id="95df9-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cornerstone OnDemand.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="95df9-204">**Om du vill tilldela hörnstenarna OnDemand Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="95df9-204">**To assign Britta Simon to Cornerstone OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="95df9-205">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="95df9-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="95df9-207">Välj i listan med program **hörnstenarna OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="95df9-207">In the applications list, select **Cornerstone OnDemand**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. <span data-ttu-id="95df9-209">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="95df9-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="95df9-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="95df9-211">Click **Add** button.</span></span> <span data-ttu-id="95df9-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95df9-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="95df9-214">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="95df9-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="95df9-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95df9-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="95df9-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="95df9-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="95df9-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="95df9-217">Testing single sign-on</span></span>

<span data-ttu-id="95df9-218">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="95df9-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="95df9-219">När du klickar på panelen hörnstenarna OnDemand på åtkomstpanelen du bör få automatiskt loggat in på ditt hörnstenarna OnDemand-program.</span><span class="sxs-lookup"><span data-stu-id="95df9-219">When you click the Cornerstone OnDemand tile in the Access Panel, you should get automatically signed-on to your Cornerstone OnDemand application.</span></span>
<span data-ttu-id="95df9-220">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="95df9-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="95df9-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="95df9-221">Additional resources</span></span>

* [<span data-ttu-id="95df9-222">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="95df9-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="95df9-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="95df9-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_203.png

