---
title: "Självstudier: Azure Active Directory-integrering med Oneteam | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Oneteam."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2e94916c-64ae-4e1a-a8b5-bc6ef7d28c29
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: c4381ca3166bd75bda1179b9a67b2224ba58ae68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-oneteam"></a><span data-ttu-id="9b938-103">Självstudier: Azure Active Directory-integrering med Oneteam</span><span class="sxs-lookup"><span data-stu-id="9b938-103">Tutorial: Azure Active Directory integration with Oneteam</span></span>

<span data-ttu-id="9b938-104">I kursen får lära du att integrera Oneteam med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9b938-104">In this tutorial, you learn how to integrate Oneteam with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9b938-105">Integrera Oneteam med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9b938-105">Integrating Oneteam with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9b938-106">Du kan styra i Azure AD som har åtkomst till Oneteam</span><span class="sxs-lookup"><span data-stu-id="9b938-106">You can control in Azure AD who has access to Oneteam</span></span>
- <span data-ttu-id="9b938-107">Du kan aktivera användarna att automatiskt hämta loggat in på Oneteam (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9b938-107">You can enable your users to automatically get signed-on to Oneteam (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9b938-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9b938-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9b938-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9b938-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b938-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9b938-110">Prerequisites</span></span>

<span data-ttu-id="9b938-111">För att konfigurera Azure AD-integrering med Oneteam, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="9b938-111">To configure Azure AD integration with Oneteam, you need the following items:</span></span>

- <span data-ttu-id="9b938-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9b938-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9b938-113">En Oneteam enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="9b938-113">A Oneteam single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9b938-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9b938-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9b938-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9b938-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9b938-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9b938-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9b938-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9b938-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9b938-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9b938-118">Scenario description</span></span>
<span data-ttu-id="9b938-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9b938-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9b938-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9b938-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9b938-121">Att lägga till Oneteam från galleriet</span><span class="sxs-lookup"><span data-stu-id="9b938-121">Adding Oneteam from the gallery</span></span>
2. <span data-ttu-id="9b938-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b938-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-oneteam-from-the-gallery"></a><span data-ttu-id="9b938-123">Att lägga till Oneteam från galleriet</span><span class="sxs-lookup"><span data-stu-id="9b938-123">Adding Oneteam from the gallery</span></span>
<span data-ttu-id="9b938-124">Du måste lägga till Oneteam från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Oneteam i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9b938-124">To configure the integration of Oneteam into Azure AD, you need to add Oneteam from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9b938-125">**Utför följande steg för att lägga till Oneteam från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="9b938-125">**To add Oneteam from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9b938-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9b938-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9b938-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9b938-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9b938-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9b938-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9b938-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b938-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9b938-133">I sökrutan skriver **Oneteam**.</span><span class="sxs-lookup"><span data-stu-id="9b938-133">In the search box, type **Oneteam**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_search.png)

5. <span data-ttu-id="9b938-135">Välj i resultatpanelen **Oneteam**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="9b938-135">In the results panel, select **Oneteam**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9b938-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b938-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9b938-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Oneteam baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9b938-138">In this section, you configure and test Azure AD single sign-on with Oneteam based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9b938-139">Azure AD måste du känna till användaren i Oneteam motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="9b938-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Oneteam is to a user in Azure AD.</span></span> <span data-ttu-id="9b938-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Oneteam upprättas.</span><span class="sxs-lookup"><span data-stu-id="9b938-140">In other words, a link relationship between an Azure AD user and the related user in Oneteam needs to be established.</span></span>

<span data-ttu-id="9b938-141">I Oneteam, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9b938-141">In Oneteam, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9b938-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Oneteam, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9b938-142">To configure and test Azure AD single sign-on with Oneteam, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9b938-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9b938-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9b938-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9b938-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9b938-145">**[Skapa en testanvändare Oneteam](#creating-a-oneteam-test-user)**  – du har en motsvarighet för Britta Simon i Oneteam som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9b938-145">**[Creating a Oneteam test user](#creating-a-oneteam-test-user)** - to have a counterpart of Britta Simon in Oneteam that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9b938-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9b938-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9b938-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9b938-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9b938-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b938-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9b938-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Oneteam program.</span><span class="sxs-lookup"><span data-stu-id="9b938-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Oneteam application.</span></span>

<span data-ttu-id="9b938-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Oneteam:**</span><span class="sxs-lookup"><span data-stu-id="9b938-150">**To configure Azure AD single sign-on with Oneteam, perform the following steps:**</span></span>

1. <span data-ttu-id="9b938-151">I Azure-portalen på den **Oneteam** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9b938-151">In the Azure portal, on the **Oneteam** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9b938-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9b938-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_samlbase.png)

3. <span data-ttu-id="9b938-155">På den **Oneteam domän och URL: er** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="9b938-155">On the **Oneteam Domain and URLs** section, if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_url.png)

    <span data-ttu-id="9b938-157">a.</span><span class="sxs-lookup"><span data-stu-id="9b938-157">a.</span></span> <span data-ttu-id="9b938-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://api.one-team.io/teams/<team name>`</span><span class="sxs-lookup"><span data-stu-id="9b938-158">In the **Identifier** textbox, type a URL using the following pattern: `https://api.one-team.io/teams/<team name>`</span></span>

    <span data-ttu-id="9b938-159">b.</span><span class="sxs-lookup"><span data-stu-id="9b938-159">b.</span></span> <span data-ttu-id="9b938-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://api.one-team.io/teams/<team name>/auth/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="9b938-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://api.one-team.io/teams/<team name>/auth/saml/callback`</span></span>

4. <span data-ttu-id="9b938-161">Kontrollera **visa avancerade inställningar för URL: en**, om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="9b938-161">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_url1.png)

    <span data-ttu-id="9b938-163">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<team name>.one-team.io/`</span><span class="sxs-lookup"><span data-stu-id="9b938-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<team name>.one-team.io/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="9b938-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9b938-164">These values are not real.</span></span> <span data-ttu-id="9b938-165">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="9b938-165">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="9b938-166">Kontakta [Oneteam klienten supportteamet](https://support.one-team.com/hc/requests/new) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9b938-166">Contact [Oneteam Client support team](https://support.one-team.com/hc/requests/new) to get these values.</span></span> 



5. <span data-ttu-id="9b938-167">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="9b938-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_certificate.png) 

6. <span data-ttu-id="9b938-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9b938-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="9b938-171">För att få SSO konfigurerats för ditt program måste du höja supportärende med [Oneteam supportteamet](https://support.one-team.com/hc/requests/new) och ger dem den hämtade **Metadata**.</span><span class="sxs-lookup"><span data-stu-id="9b938-171">To get SSO configured for your application, you can raise the support ticket with [Oneteam support team](https://support.one-team.com/hc/requests/new) and provide them the downloaded **Metadata**.</span></span> 

> [!TIP]
> <span data-ttu-id="9b938-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="9b938-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9b938-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="9b938-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9b938-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9b938-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9b938-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9b938-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="9b938-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9b938-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9b938-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="9b938-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9b938-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9b938-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9b938-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9b938-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9b938-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b938-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9b938-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9b938-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-oneteam-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9b938-187">a.</span><span class="sxs-lookup"><span data-stu-id="9b938-187">a.</span></span> <span data-ttu-id="9b938-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9b938-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9b938-189">b.</span><span class="sxs-lookup"><span data-stu-id="9b938-189">b.</span></span> <span data-ttu-id="9b938-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9b938-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9b938-191">c.</span><span class="sxs-lookup"><span data-stu-id="9b938-191">c.</span></span> <span data-ttu-id="9b938-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9b938-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9b938-193">d.</span><span class="sxs-lookup"><span data-stu-id="9b938-193">d.</span></span> <span data-ttu-id="9b938-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9b938-194">Click **Create**.</span></span>
 
### <a name="creating-a-oneteam-test-user"></a><span data-ttu-id="9b938-195">Skapa en testanvändare Oneteam</span><span class="sxs-lookup"><span data-stu-id="9b938-195">Creating a Oneteam test user</span></span>

<span data-ttu-id="9b938-196">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Oneteam.</span><span class="sxs-lookup"><span data-stu-id="9b938-196">The objective of this section is to create a user called Britta Simon in Oneteam.</span></span> <span data-ttu-id="9b938-197">Oneteam stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="9b938-197">Oneteam supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="9b938-198">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9b938-198">There is no action item for you in this section.</span></span> <span data-ttu-id="9b938-199">En ny användare skapas vid ett försök att komma åt Oneteam, om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="9b938-199">A new user will be created during an attempt to access Oneteam, if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="9b938-200">Om du behöver skapa en användare manuellt kan du öka supportärende med [Oneteam supportteamet](https://support.one-team.com/hc/requests/new).</span><span class="sxs-lookup"><span data-stu-id="9b938-200">If you need to create an user manually, you can raise the support ticket with [Oneteam support team](https://support.one-team.com/hc/requests/new).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9b938-201">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="9b938-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9b938-202">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Oneteam.</span><span class="sxs-lookup"><span data-stu-id="9b938-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Oneteam.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9b938-204">**Om du vill tilldela Oneteam Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9b938-204">**To assign Britta Simon to Oneteam, perform the following steps:**</span></span>

1. <span data-ttu-id="9b938-205">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9b938-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9b938-207">Välj i listan med program **Oneteam**.</span><span class="sxs-lookup"><span data-stu-id="9b938-207">In the applications list, select **Oneteam**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-oneteam-tutorial/tutorial_oneteam_app.png) 

3. <span data-ttu-id="9b938-209">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9b938-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9b938-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9b938-211">Click **Add** button.</span></span> <span data-ttu-id="9b938-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b938-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9b938-214">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="9b938-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9b938-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b938-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9b938-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9b938-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9b938-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9b938-217">Testing single sign-on</span></span>

<span data-ttu-id="9b938-218">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9b938-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9b938-219">När du klickar på panelen Oneteam på åtkomstpanelen du bör få automatiskt loggat in på ditt Oneteam program.</span><span class="sxs-lookup"><span data-stu-id="9b938-219">When you click the Oneteam tile in the Access Panel, you should get automatically signed-on to your Oneteam application.</span></span>
<span data-ttu-id="9b938-220">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9b938-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="9b938-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9b938-221">Additional resources</span></span>

* [<span data-ttu-id="9b938-222">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9b938-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9b938-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9b938-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-oneteam-tutorial/tutorial_general_203.png

