---
title: "Självstudier: Azure Active Directory-integrering med Lecorpio | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Lecorpio."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 35c94e2d9d8a938971f85ea732a74a7e1655545e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lecorpio"></a><span data-ttu-id="5c039-103">Självstudier: Azure Active Directory-integrering med Lecorpio</span><span class="sxs-lookup"><span data-stu-id="5c039-103">Tutorial: Azure Active Directory integration with Lecorpio</span></span>

<span data-ttu-id="5c039-104">I kursen får lära du att integrera Lecorpio med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5c039-104">In this tutorial, you learn how to integrate Lecorpio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5c039-105">Integrera Lecorpio med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5c039-105">Integrating Lecorpio with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5c039-106">Du kan styra i Azure AD som har åtkomst till Lecorpio</span><span class="sxs-lookup"><span data-stu-id="5c039-106">You can control in Azure AD who has access to Lecorpio</span></span>
- <span data-ttu-id="5c039-107">Du kan aktivera användarna att automatiskt hämta loggat in på Lecorpio (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5c039-107">You can enable your users to automatically get signed-on to Lecorpio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5c039-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5c039-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5c039-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5c039-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c039-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5c039-110">Prerequisites</span></span>

<span data-ttu-id="5c039-111">För att konfigurera Azure AD-integrering med Lecorpio, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5c039-111">To configure Azure AD integration with Lecorpio, you need the following items:</span></span>

- <span data-ttu-id="5c039-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c039-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5c039-113">En Lecorpio enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5c039-113">A Lecorpio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5c039-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c039-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5c039-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5c039-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5c039-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5c039-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5c039-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5c039-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5c039-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5c039-118">Scenario description</span></span>
<span data-ttu-id="5c039-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5c039-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5c039-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c039-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5c039-121">Att lägga till Lecorpio från galleriet</span><span class="sxs-lookup"><span data-stu-id="5c039-121">Adding Lecorpio from the gallery</span></span>
2. <span data-ttu-id="5c039-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c039-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lecorpio-from-the-gallery"></a><span data-ttu-id="5c039-123">Att lägga till Lecorpio från galleriet</span><span class="sxs-lookup"><span data-stu-id="5c039-123">Adding Lecorpio from the gallery</span></span>
<span data-ttu-id="5c039-124">Du måste lägga till Lecorpio från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Lecorpio i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5c039-124">To configure the integration of Lecorpio into Azure AD, you need to add Lecorpio from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5c039-125">**Utför följande steg för att lägga till Lecorpio från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="5c039-125">**To add Lecorpio from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5c039-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c039-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5c039-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5c039-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5c039-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c039-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5c039-131">Klicka på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c039-131">Click **New application** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5c039-133">I sökrutan skriver **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="5c039-133">In the search box, type **Lecorpio**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_search.png)

5. <span data-ttu-id="5c039-135">Välj i resultatpanelen **Lecorpio**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="5c039-135">In the results panel, select **Lecorpio**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5c039-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c039-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5c039-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lecorpio baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5c039-138">In this section, you configure and test Azure AD single sign-on with Lecorpio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5c039-139">Azure AD måste du känna till användaren i Lecorpio motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="5c039-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lecorpio is to a user in Azure AD.</span></span> <span data-ttu-id="5c039-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Lecorpio upprättas.</span><span class="sxs-lookup"><span data-stu-id="5c039-140">In other words, a link relationship between an Azure AD user and the related user in Lecorpio needs to be established.</span></span>

<span data-ttu-id="5c039-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="5c039-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Lecorpio.</span></span>

<span data-ttu-id="5c039-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Lecorpio, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5c039-142">To configure and test Azure AD single sign-on with Lecorpio, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5c039-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5c039-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5c039-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c039-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5c039-145">**[Skapa en testanvändare Lecorpio](#creating-a-lecorpio-test-user)**  – du har en motsvarighet för Britta Simon i Lecorpio som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5c039-145">**[Creating a Lecorpio test user](#creating-a-lecorpio-test-user)** - to have a counterpart of Britta Simon in Lecorpio that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5c039-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c039-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5c039-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5c039-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5c039-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c039-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5c039-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Lecorpio program.</span><span class="sxs-lookup"><span data-stu-id="5c039-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lecorpio application.</span></span>

<span data-ttu-id="5c039-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Lecorpio:**</span><span class="sxs-lookup"><span data-stu-id="5c039-150">**To configure Azure AD single sign-on with Lecorpio, perform the following steps:**</span></span>

1. <span data-ttu-id="5c039-151">I Azure-portalen på den **Lecorpio** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5c039-151">In the Azure portal, on the **Lecorpio** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5c039-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5c039-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_samlbase.png)

3. <span data-ttu-id="5c039-155">På den **Lecorpio domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c039-155">On the **Lecorpio Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_url.png)

    <span data-ttu-id="5c039-157">a.</span><span class="sxs-lookup"><span data-stu-id="5c039-157">a.</span></span> <span data-ttu-id="5c039-158">I den **inloggnings-URL** textruta Skriv det värde som använder följande mönster:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="5c039-158">In the **Sign-on URL** textbox, type the value using the following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    <span data-ttu-id="5c039-159">b.</span><span class="sxs-lookup"><span data-stu-id="5c039-159">b.</span></span> <span data-ttu-id="5c039-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<instance name>.lecorpio.com/<customer name>`</span><span class="sxs-lookup"><span data-stu-id="5c039-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<instance name>.lecorpio.com/<customer name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5c039-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="5c039-161">These values are not the real.</span></span> <span data-ttu-id="5c039-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="5c039-162">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="5c039-163">Här rekommenderar vi att du om du vill använda det unika värdet på strängen i identifieraren.</span><span class="sxs-lookup"><span data-stu-id="5c039-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="5c039-164">Kontakta [Lecorpio klienten supportteamet](mailto:info@lecorpio.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5c039-164">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) to get these values.</span></span> 
 
4. <span data-ttu-id="5c039-165">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5c039-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_certificate.png) 

5. <span data-ttu-id="5c039-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c039-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="5c039-169">Konfigurera enkel inloggning på **Lecorpio** sida, måste du skicka den hämtade **XML-Metadata för** till [Lecorpio supportteamet](mailto:info@lecorpio.com).</span><span class="sxs-lookup"><span data-stu-id="5c039-169">To configure single sign-on on **Lecorpio** side, you need to send the downloaded **Metadata XML** to [Lecorpio support team](mailto:info@lecorpio.com).</span></span>

> [!TIP]
> <span data-ttu-id="5c039-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="5c039-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5c039-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="5c039-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5c039-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5c039-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5c039-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5c039-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="5c039-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5c039-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5c039-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="5c039-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5c039-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5c039-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5c039-179">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="5c039-179">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5c039-181">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c039-181">At the top of the dialog, click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5c039-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5c039-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lecorpio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5c039-185">a.</span><span class="sxs-lookup"><span data-stu-id="5c039-185">a.</span></span> <span data-ttu-id="5c039-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5c039-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5c039-187">b.</span><span class="sxs-lookup"><span data-stu-id="5c039-187">b.</span></span> <span data-ttu-id="5c039-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5c039-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5c039-189">c.</span><span class="sxs-lookup"><span data-stu-id="5c039-189">c.</span></span> <span data-ttu-id="5c039-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5c039-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5c039-191">d.</span><span class="sxs-lookup"><span data-stu-id="5c039-191">d.</span></span> <span data-ttu-id="5c039-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5c039-192">Click **Create**.</span></span>
 
### <a name="creating-a-lecorpio-test-user"></a><span data-ttu-id="5c039-193">Skapa en testanvändare Lecorpio</span><span class="sxs-lookup"><span data-stu-id="5c039-193">Creating a Lecorpio test user</span></span>

<span data-ttu-id="5c039-194">I det här avsnittet skapar du en användare som kallas Britta Simon i Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="5c039-194">In this section, you create a user called Britta Simon in Lecorpio.</span></span> 

<span data-ttu-id="5c039-195">Kontakta [Lecorpio klienten supportteamet](mailto:info@lecorpio.com) att lägga till användare i Lecorpio-programmet.</span><span class="sxs-lookup"><span data-stu-id="5c039-195">Contact [Lecorpio Client support team](mailto:info@lecorpio.com) to add the users in the Lecorpio application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5c039-196">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="5c039-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5c039-197">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Lecorpio.</span><span class="sxs-lookup"><span data-stu-id="5c039-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lecorpio.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5c039-199">**Om du vill tilldela Lecorpio Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5c039-199">**To assign Britta Simon to Lecorpio, perform the following steps:**</span></span>

1. <span data-ttu-id="5c039-200">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5c039-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5c039-202">Välj i listan med program **Lecorpio**.</span><span class="sxs-lookup"><span data-stu-id="5c039-202">In the applications list, select **Lecorpio**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lecorpio-tutorial/tutorial_lecorpio_app.png) 

3. <span data-ttu-id="5c039-204">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5c039-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5c039-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5c039-206">Click **Add** button.</span></span> <span data-ttu-id="5c039-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c039-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5c039-209">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="5c039-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5c039-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c039-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5c039-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5c039-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5c039-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5c039-212">Testing single sign-on</span></span>

<span data-ttu-id="5c039-213">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5c039-213">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="5c039-214">När du klickar på panelen Lecorpio på åtkomstpanelen du bör få automatiskt loggat in på ditt Lecorpio program.</span><span class="sxs-lookup"><span data-stu-id="5c039-214">When you click the Lecorpio tile in the Access Panel, you should get automatically signed-on to your Lecorpio application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c039-215">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5c039-215">Additional resources</span></span>

* [<span data-ttu-id="5c039-216">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5c039-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5c039-217">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5c039-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lecorpio-tutorial/tutorial_general_203.png

