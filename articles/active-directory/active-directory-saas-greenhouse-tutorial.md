---
title: "Självstudier: Azure Active Directory-integrering med växthusgaser | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och växthusgaser."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: d3aba4aab8ded8749db2bf8197f57a6763008c60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a><span data-ttu-id="100a9-103">Självstudier: Azure Active Directory-integrering med växthusgaser</span><span class="sxs-lookup"><span data-stu-id="100a9-103">Tutorial: Azure Active Directory integration with Greenhouse</span></span>

<span data-ttu-id="100a9-104">I kursen får lära du att integrera växthusgaser med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="100a9-104">In this tutorial, you learn how to integrate Greenhouse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="100a9-105">Integrera växthusgaser med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="100a9-105">Integrating Greenhouse with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="100a9-106">Du kan styra i Azure AD som har åtkomst till växthusgaser.</span><span class="sxs-lookup"><span data-stu-id="100a9-106">You can control in Azure AD who has access to Greenhouse.</span></span>
- <span data-ttu-id="100a9-107">Du kan aktivera användarna att automatiskt hämta loggat in på växthusgaser (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="100a9-107">You can enable your users to automatically get signed-on to Greenhouse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="100a9-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="100a9-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="100a9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="100a9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="100a9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="100a9-110">Prerequisites</span></span>

<span data-ttu-id="100a9-111">För att konfigurera Azure AD-integrering med växthusgaser, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="100a9-111">To configure Azure AD integration with Greenhouse, you need the following items:</span></span>

- <span data-ttu-id="100a9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="100a9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="100a9-113">En växthusgaser enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="100a9-113">A Greenhouse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="100a9-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="100a9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="100a9-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="100a9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="100a9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="100a9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="100a9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="100a9-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="100a9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="100a9-118">Scenario description</span></span>
<span data-ttu-id="100a9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="100a9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="100a9-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="100a9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="100a9-121">Att lägga till växthusgaser från galleriet</span><span class="sxs-lookup"><span data-stu-id="100a9-121">Adding Greenhouse from the gallery</span></span>
2. <span data-ttu-id="100a9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="100a9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-greenhouse-from-the-gallery"></a><span data-ttu-id="100a9-123">Att lägga till växthusgaser från galleriet</span><span class="sxs-lookup"><span data-stu-id="100a9-123">Adding Greenhouse from the gallery</span></span>
<span data-ttu-id="100a9-124">Du måste lägga till växthusgaser från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering världens i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="100a9-124">To configure the integration of Greenhouse into Azure AD, you need to add Greenhouse from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="100a9-125">**Utför följande steg för att lägga till växthusgaser från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="100a9-125">**To add Greenhouse from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="100a9-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="100a9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="100a9-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="100a9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="100a9-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="100a9-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="100a9-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="100a9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="100a9-133">I sökrutan skriver **växthusgaser**väljer **växthusgaser** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="100a9-133">In the search box, type **Greenhouse**, select **Greenhouse** from result panel then click **Add** button to add the application.</span></span>

    ![Växthusgaser i resultatlistan](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="100a9-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="100a9-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="100a9-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med växthusgaser baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="100a9-136">In this section, you configure and test Azure AD single sign-on with Greenhouse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="100a9-137">Azure AD måste du känna till användaren i växthusgaser motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="100a9-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Greenhouse is to a user in Azure AD.</span></span> <span data-ttu-id="100a9-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i växthusgaser upprättas.</span><span class="sxs-lookup"><span data-stu-id="100a9-138">In other words, a link relationship between an Azure AD user and the related user in Greenhouse needs to be established.</span></span>

<span data-ttu-id="100a9-139">I växthus, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="100a9-139">In Greenhouse, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="100a9-140">Om du vill konfigurera och testa Azure AD enkel inloggning med växthusgaser, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="100a9-140">To configure and test Azure AD single sign-on with Greenhouse, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="100a9-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="100a9-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="100a9-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="100a9-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="100a9-143">**[Skapa en testanvändare växthusgaser](#create-a-greenhouse-test-user)**  – har en motsvarighet för Britta Simon växthusgaser som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="100a9-143">**[Create a Greenhouse test user](#create-a-greenhouse-test-user)** - to have a counterpart of Britta Simon in Greenhouse that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="100a9-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="100a9-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="100a9-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="100a9-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="100a9-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="100a9-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="100a9-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet växthusgaser.</span><span class="sxs-lookup"><span data-stu-id="100a9-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Greenhouse application.</span></span>

<span data-ttu-id="100a9-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med växthusgaser:**</span><span class="sxs-lookup"><span data-stu-id="100a9-148">**To configure Azure AD single sign-on with Greenhouse, perform the following steps:**</span></span>

1. <span data-ttu-id="100a9-149">I Azure-portalen på den **växthusgaser** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="100a9-149">In the Azure portal, on the **Greenhouse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="100a9-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="100a9-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

3. <span data-ttu-id="100a9-153">På den **växthusgaser domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="100a9-153">On the **Greenhouse Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och växthusgaser domän med enkel inloggning information](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_url.png)

    <span data-ttu-id="100a9-155">a.</span><span class="sxs-lookup"><span data-stu-id="100a9-155">a.</span></span> <span data-ttu-id="100a9-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="100a9-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.greenhouse.io`</span></span>

    <span data-ttu-id="100a9-157">b.</span><span class="sxs-lookup"><span data-stu-id="100a9-157">b.</span></span> <span data-ttu-id="100a9-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="100a9-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.greenhouse.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="100a9-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="100a9-159">These values are not real.</span></span> <span data-ttu-id="100a9-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="100a9-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="100a9-161">Kontakta [växthusgaser klienten supportteamet](https://www.greenhouse.io/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="100a9-161">Contact [Greenhouse Client support team](https://www.greenhouse.io/contact) to get these values.</span></span> 
 


4. <span data-ttu-id="100a9-162">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="100a9-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

5. <span data-ttu-id="100a9-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="100a9-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-greenhouse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="100a9-166">Konfigurera enkel inloggning på **växthusgaser** sida, måste du skicka den hämtade **XML-Metadata för** till [växthusgaser supportteamet](http://www.greenhouse.io/contact).</span><span class="sxs-lookup"><span data-stu-id="100a9-166">To configure single sign-on on **Greenhouse** side, you need to send the downloaded **Metadata XML** to [Greenhouse support team](http://www.greenhouse.io/contact).</span></span>

> [!TIP]
> <span data-ttu-id="100a9-167">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="100a9-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="100a9-168">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="100a9-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="100a9-169">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="100a9-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="100a9-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="100a9-170">Create an Azure AD test user</span></span>

<span data-ttu-id="100a9-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="100a9-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="100a9-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="100a9-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="100a9-174">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="100a9-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="100a9-176">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="100a9-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="100a9-178">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="100a9-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="100a9-180">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="100a9-180">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="100a9-182">a.</span><span class="sxs-lookup"><span data-stu-id="100a9-182">a.</span></span> <span data-ttu-id="100a9-183">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="100a9-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="100a9-184">b.</span><span class="sxs-lookup"><span data-stu-id="100a9-184">b.</span></span> <span data-ttu-id="100a9-185">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="100a9-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="100a9-186">c.</span><span class="sxs-lookup"><span data-stu-id="100a9-186">c.</span></span> <span data-ttu-id="100a9-187">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="100a9-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="100a9-188">d.</span><span class="sxs-lookup"><span data-stu-id="100a9-188">d.</span></span> <span data-ttu-id="100a9-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="100a9-189">Click **Create**.</span></span>
 
### <a name="create-a-greenhouse-test-user"></a><span data-ttu-id="100a9-190">Skapa en testanvändare växthusgaser</span><span class="sxs-lookup"><span data-stu-id="100a9-190">Create a Greenhouse test user</span></span>

<span data-ttu-id="100a9-191">För att aktivera Azure AD-användare att logga in på växthusgaser etableras de i växthusgaser.</span><span class="sxs-lookup"><span data-stu-id="100a9-191">In order to enable Azure AD users to log into Greenhouse, they must be provisioned into Greenhouse.</span></span> <span data-ttu-id="100a9-192">När det gäller växthusgaser är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="100a9-192">In the case of Greenhouse, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="100a9-193">Du kan använda något annat växthusgaser användarens konto skapas verktyg eller API: er som tillhandahålls av växthusgaser etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="100a9-193">You can use any other Greenhouse user account creation tools or APIs provided by Greenhouse to provision AAD user accounts.</span></span> 

<span data-ttu-id="100a9-194">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="100a9-194">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="100a9-195">Logga in på ditt **växthusgaser** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="100a9-195">Log in to your **Greenhouse** company site as an administrator.</span></span>

2. <span data-ttu-id="100a9-196">Klicka på menyn högst upp **konfigurera**, och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="100a9-196">In the menu on the top, click **Configure**, and then click **Users**.</span></span>
   
   <span data-ttu-id="100a9-197">![Användare](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "användare")</span><span class="sxs-lookup"><span data-stu-id="100a9-197">![Users](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "Users")</span></span>

3. <span data-ttu-id="100a9-198">Klicka på **nya användare**.</span><span class="sxs-lookup"><span data-stu-id="100a9-198">Click **New Users**.</span></span>
   
   <span data-ttu-id="100a9-199">![Ny användare](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="100a9-199">![New User](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "New User")</span></span>

4. <span data-ttu-id="100a9-200">I den **Lägg till nya användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="100a9-200">In the **Add New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="100a9-201">![Lägga till nya användare](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "lägga till nya användare")</span><span class="sxs-lookup"><span data-stu-id="100a9-201">![Add New User](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "Add New User")</span></span>

   <span data-ttu-id="100a9-202">a.</span><span class="sxs-lookup"><span data-stu-id="100a9-202">a.</span></span> <span data-ttu-id="100a9-203">I den **ange användarens e-postmeddelanden** textruta, ange ett giltigt Azure Active Directory-konto som du vill etablera e-postadress.</span><span class="sxs-lookup"><span data-stu-id="100a9-203">In the **Enter user emails** textbox, type the email address of a valid Azure Active Directory account you want to provision.</span></span>

   <span data-ttu-id="100a9-204">b.</span><span class="sxs-lookup"><span data-stu-id="100a9-204">b.</span></span> <span data-ttu-id="100a9-205">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="100a9-205">Click **Save**.</span></span>    
   
      >[!NOTE]
      ><span data-ttu-id="100a9-206">Azure Active Directory-konto innehavare får ett e-postmeddelande med en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="100a9-206">The Azure Active Directory account holders will receive an email including a link to confirm the account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="100a9-207">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="100a9-207">Assign the Azure AD test user</span></span>

<span data-ttu-id="100a9-208">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till växthusgaser.</span><span class="sxs-lookup"><span data-stu-id="100a9-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Greenhouse.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="100a9-210">**Om du vill tilldela växthusgaser Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="100a9-210">**To assign Britta Simon to Greenhouse, perform the following steps:**</span></span>

1. <span data-ttu-id="100a9-211">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="100a9-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="100a9-213">Välj i listan med program **växthusgaser**.</span><span class="sxs-lookup"><span data-stu-id="100a9-213">In the applications list, select **Greenhouse**.</span></span>

    ![Länken växthusgaser i listan med program](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_app.png)  

3. <span data-ttu-id="100a9-215">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="100a9-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="100a9-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="100a9-217">Click **Add** button.</span></span> <span data-ttu-id="100a9-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="100a9-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="100a9-220">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="100a9-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="100a9-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="100a9-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="100a9-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="100a9-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="100a9-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="100a9-223">Test single sign-on</span></span>

<span data-ttu-id="100a9-224">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="100a9-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="100a9-225">När du klickar på panelen växthusgaser på åtkomstpanelen du bör få automatiskt loggat in på ditt växthusgaser program.</span><span class="sxs-lookup"><span data-stu-id="100a9-225">When you click the Greenhouse tile in the Access Panel, you should get automatically signed-on to your Greenhouse application.</span></span>
<span data-ttu-id="100a9-226">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="100a9-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="100a9-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="100a9-227">Additional resources</span></span>

* [<span data-ttu-id="100a9-228">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="100a9-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="100a9-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="100a9-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_203.png

