---
title: "Självstudier: Azure Active Directory-integrering med Heroku | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Heroku."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d7d72ec6-4a60-4524-8634-26d8fbbcc833
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: d30605e4757b484f327a784b73f939b62ef59373
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-heroku"></a><span data-ttu-id="99897-103">Självstudier: Azure Active Directory-integrering med Heroku</span><span class="sxs-lookup"><span data-stu-id="99897-103">Tutorial: Azure Active Directory integration with Heroku</span></span>

<span data-ttu-id="99897-104">I kursen får lära du att integrera Heroku med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="99897-104">In this tutorial, you learn how to integrate Heroku with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="99897-105">Integrera Heroku med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="99897-105">Integrating Heroku with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="99897-106">Du kan styra i Azure AD som har åtkomst till Heroku</span><span class="sxs-lookup"><span data-stu-id="99897-106">You can control in Azure AD who has access to Heroku</span></span>
- <span data-ttu-id="99897-107">Du kan aktivera användarna att automatiskt hämta loggat in på Heroku (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="99897-107">You can enable your users to automatically get signed-on to Heroku (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="99897-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="99897-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="99897-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="99897-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99897-110">Krav</span><span class="sxs-lookup"><span data-stu-id="99897-110">Prerequisites</span></span>

<span data-ttu-id="99897-111">För att konfigurera Azure AD-integrering med Heroku, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="99897-111">To configure Azure AD integration with Heroku, you need the following items:</span></span>

- <span data-ttu-id="99897-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="99897-112">An Azure AD subscription</span></span>
- <span data-ttu-id="99897-113">En Heroku enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="99897-113">A Heroku single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="99897-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="99897-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="99897-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="99897-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="99897-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="99897-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="99897-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="99897-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="99897-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="99897-118">Scenario description</span></span>
<span data-ttu-id="99897-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="99897-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="99897-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="99897-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="99897-121">Att lägga till Heroku från galleriet</span><span class="sxs-lookup"><span data-stu-id="99897-121">Adding Heroku from the gallery</span></span>
2. <span data-ttu-id="99897-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99897-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-heroku-from-the-gallery"></a><span data-ttu-id="99897-123">Att lägga till Heroku från galleriet</span><span class="sxs-lookup"><span data-stu-id="99897-123">Adding Heroku from the gallery</span></span>
<span data-ttu-id="99897-124">Du måste lägga till Heroku från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Heroku i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="99897-124">To configure the integration of Heroku into Azure AD, you need to add Heroku from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="99897-125">**Utför följande steg för att lägga till Heroku från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="99897-125">**To add Heroku from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="99897-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99897-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="99897-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="99897-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="99897-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="99897-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="99897-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99897-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="99897-133">I sökrutan skriver **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="99897-133">In the search box, type **Heroku**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_search.png)

5. <span data-ttu-id="99897-135">Välj i resultatpanelen **Heroku**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="99897-135">In the results panel, select **Heroku**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="99897-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99897-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="99897-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Heroku baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="99897-138">In this section, you configure and test Azure AD single sign-on with Heroku based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="99897-139">Azure AD måste du känna till användaren i Heroku motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="99897-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Heroku is to a user in Azure AD.</span></span> <span data-ttu-id="99897-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Heroku upprättas.</span><span class="sxs-lookup"><span data-stu-id="99897-140">In other words, a link relationship between an Azure AD user and the related user in Heroku needs to be established.</span></span>

<span data-ttu-id="99897-141">I Heroku, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="99897-141">In Heroku, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="99897-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Heroku, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="99897-142">To configure and test Azure AD single sign-on with Heroku, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="99897-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="99897-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="99897-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99897-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="99897-145">**[Skapa en testanvändare Heroku](#creating-a-heroku-test-user)**  – du har en motsvarighet för Britta Simon i Heroku som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="99897-145">**[Creating a Heroku test user](#creating-a-heroku-test-user)** - to have a counterpart of Britta Simon in Heroku that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="99897-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99897-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="99897-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="99897-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="99897-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99897-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="99897-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Heroku program.</span><span class="sxs-lookup"><span data-stu-id="99897-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Heroku application.</span></span>

<span data-ttu-id="99897-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Heroku:**</span><span class="sxs-lookup"><span data-stu-id="99897-150">**To configure Azure AD single sign-on with Heroku, perform the following steps:**</span></span>

1. <span data-ttu-id="99897-151">I Azure-portalen på den **Heroku** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="99897-151">In the Azure portal, on the **Heroku** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="99897-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="99897-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_samlbase.png)

3. <span data-ttu-id="99897-155">På den **Heroku domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="99897-155">On the **Heroku Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_url.png)

    <span data-ttu-id="99897-157">a.</span><span class="sxs-lookup"><span data-stu-id="99897-157">a.</span></span> <span data-ttu-id="99897-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="99897-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>    
    `https://sso.heroku.com/saml/<company-name>/init`

    <span data-ttu-id="99897-159">b.</span><span class="sxs-lookup"><span data-stu-id="99897-159">b.</span></span> <span data-ttu-id="99897-160">I den **Identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="99897-160">In the **Identifier URL** textbox, type a URL using the following pattern:</span></span>            
    `https://sso.heroku.com/saml/<company-name>`

    > [!NOTE]
    ><span data-ttu-id="99897-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="99897-161">These values are not real.</span></span> <span data-ttu-id="99897-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="99897-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="99897-163">Du kan få värdena från Heroku team, som beskrivs i avsnitten nedan i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="99897-163">You get these values from Heroku team, which is described in later sections of this article.</span></span> 
        
4. <span data-ttu-id="99897-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="99897-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_certificate.png) 

5. <span data-ttu-id="99897-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="99897-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-heroku-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="99897-168">Om du vill aktivera enkel inloggning i Heroku, utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="99897-168">To enable SSO in Heroku, perform the following steps:</span></span>
   
    <span data-ttu-id="99897-169">a.</span><span class="sxs-lookup"><span data-stu-id="99897-169">a.</span></span> <span data-ttu-id="99897-170">Logga in på kontot Heroku som administratör.</span><span class="sxs-lookup"><span data-stu-id="99897-170">Log in to the Heroku account as an administrator.</span></span>

    <span data-ttu-id="99897-171">b.</span><span class="sxs-lookup"><span data-stu-id="99897-171">b.</span></span> <span data-ttu-id="99897-172">Klicka på fliken **Settings** (Inställningar).</span><span class="sxs-lookup"><span data-stu-id="99897-172">Click the **Settings** tab.</span></span>

    <span data-ttu-id="99897-173">c.</span><span class="sxs-lookup"><span data-stu-id="99897-173">c.</span></span> <span data-ttu-id="99897-174">På den **enkel inloggning på sidan**, klickar du på **överföra Metadata**.</span><span class="sxs-lookup"><span data-stu-id="99897-174">On the **Single Sign On Page**, click **Upload Metadata**.</span></span>

    <span data-ttu-id="99897-175">d.</span><span class="sxs-lookup"><span data-stu-id="99897-175">d.</span></span> <span data-ttu-id="99897-176">Ladda upp metadatafilen, som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="99897-176">Upload the metadata file, which you have downloaded from the Azure portal.</span></span>

    <span data-ttu-id="99897-177">e.</span><span class="sxs-lookup"><span data-stu-id="99897-177">e.</span></span> <span data-ttu-id="99897-178">När installationen lyckas administratörer ser en dialogruta och URL för SSO-inloggning för slutanvändare visas.</span><span class="sxs-lookup"><span data-stu-id="99897-178">When the setup is successful, administrators see a confirmation dialog and the URL of the SSO Login for end users is displayed.</span></span> 

    <span data-ttu-id="99897-179">f.</span><span class="sxs-lookup"><span data-stu-id="99897-179">f.</span></span> <span data-ttu-id="99897-180">Kopiera den **Heroku inloggnings-URL** och **Heroku enhets-ID** värden och gå tillbaka till **Heroku domän och URL: er** i Azure-portalen och klistra in dessa värden i den  **Inloggnings-Url** och **identifierare** textrutor respektive.</span><span class="sxs-lookup"><span data-stu-id="99897-180">Copy the **Heroku Login URL** and **Heroku Entity ID** values and go back to **Heroku Domain and URLs** section in Azure portal and paste these values into the **Sign-On Url** and **Identifier** textboxes respectively.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_52.png) 
    
8. <span data-ttu-id="99897-182">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="99897-182">Click **Next**.</span></span>

> [!TIP]
> <span data-ttu-id="99897-183">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="99897-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="99897-184">När du lägger till den här appen från den **Active Directory-företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den  **Konfigurationen** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="99897-184">After adding this app from the **Active Directory Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="99897-185">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="99897-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="99897-186">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="99897-186">Creating an Azure AD test user</span></span>

<span data-ttu-id="99897-187">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="99897-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="99897-189">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="99897-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="99897-190">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="99897-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="99897-192">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="99897-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="99897-194">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99897-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="99897-196">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="99897-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-heroku-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="99897-198">a.</span><span class="sxs-lookup"><span data-stu-id="99897-198">a.</span></span> <span data-ttu-id="99897-199">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="99897-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="99897-200">b.</span><span class="sxs-lookup"><span data-stu-id="99897-200">b.</span></span> <span data-ttu-id="99897-201">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="99897-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="99897-202">c.</span><span class="sxs-lookup"><span data-stu-id="99897-202">c.</span></span> <span data-ttu-id="99897-203">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="99897-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="99897-204">d.</span><span class="sxs-lookup"><span data-stu-id="99897-204">d.</span></span> <span data-ttu-id="99897-205">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="99897-205">Click **Create**.</span></span>
 
### <a name="creating-a-heroku-test-user"></a><span data-ttu-id="99897-206">Skapa en testanvändare Heroku</span><span class="sxs-lookup"><span data-stu-id="99897-206">Creating a Heroku test user</span></span>

<span data-ttu-id="99897-207">I det här avsnittet skapar du en användare som kallas Britta Simon i Heroku.</span><span class="sxs-lookup"><span data-stu-id="99897-207">In this section, you create a user called Britta Simon in Heroku.</span></span> <span data-ttu-id="99897-208">Heroku stöder just-in-time-allokering som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="99897-208">Heroku supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="99897-209">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="99897-209">There is no action item for you in this section.</span></span> <span data-ttu-id="99897-210">En ny användare skapas vid åtkomst till Heroku om användaren inte finns ännu.</span><span class="sxs-lookup"><span data-stu-id="99897-210">A new user is created when accessing Heroku if the user doesn't exist yet.</span></span> <span data-ttu-id="99897-211">När kontot har etablerats användaren får ett e-postmeddelandet och behöver klickar du på länken bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="99897-211">After the account is provisioned, the end user receives a verification email and needs to click the acknowledgement link.</span></span>

>[!NOTE]
><span data-ttu-id="99897-212">Om du behöver skapa en användare manuellt, måste du kontakta den [Heroku klienten supportteamet](https://www.heroku.com/support).</span><span class="sxs-lookup"><span data-stu-id="99897-212">If you need to create a user manually, you need to contact the [Heroku Client support team](https://www.heroku.com/support).</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="99897-213">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="99897-213">Assigning the Azure AD test user</span></span>

<span data-ttu-id="99897-214">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Heroku.</span><span class="sxs-lookup"><span data-stu-id="99897-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Heroku.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="99897-216">**Om du vill tilldela Heroku Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="99897-216">**To assign Britta Simon to Heroku, perform the following steps:**</span></span>

1. <span data-ttu-id="99897-217">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="99897-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="99897-219">Välj i listan med program **Heroku**.</span><span class="sxs-lookup"><span data-stu-id="99897-219">In the applications list, select **Heroku**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-heroku-tutorial/tutorial_heroku_app.png) 

3. <span data-ttu-id="99897-221">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="99897-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="99897-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="99897-223">Click **Add** button.</span></span> <span data-ttu-id="99897-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99897-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="99897-226">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="99897-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="99897-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99897-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="99897-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="99897-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="99897-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="99897-229">Testing single sign-on</span></span>

<span data-ttu-id="99897-230">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="99897-230">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="99897-231">När du klickar på panelen Heroku på åtkomstpanelen du bör få automatiskt loggat in på ditt Heroku program.</span><span class="sxs-lookup"><span data-stu-id="99897-231">When you click the Heroku tile in the Access Panel, you should get automatically signed-on to your Heroku application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="99897-232">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="99897-232">Additional resources</span></span>

* [<span data-ttu-id="99897-233">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="99897-233">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="99897-234">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="99897-234">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-heroku-tutorial/tutorial_general_203.png
