---
title: "Självstudier: Azure Active Directory-integrering med Fuze | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Fuze."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9780b4bf-1fd1-48c1-9ceb-f750225ae08a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: c7f7b095aac6202a7ec5248ee2bbb109615287a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuze"></a><span data-ttu-id="db342-103">Självstudier: Azure Active Directory-integrering med Fuze</span><span class="sxs-lookup"><span data-stu-id="db342-103">Tutorial: Azure Active Directory integration with Fuze</span></span>

<span data-ttu-id="db342-104">I kursen får lära du att integrera Fuze med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="db342-104">In this tutorial, you learn how to integrate Fuze with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="db342-105">Integrera Fuze med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="db342-105">Integrating Fuze with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="db342-106">Du kan styra i Azure AD som har åtkomst till Fuze</span><span class="sxs-lookup"><span data-stu-id="db342-106">You can control in Azure AD who has access to Fuze</span></span>
- <span data-ttu-id="db342-107">Du kan aktivera användarna att automatiskt hämta loggat in på Fuze (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="db342-107">You can enable your users to automatically get signed-on to Fuze (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="db342-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="db342-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="db342-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="db342-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db342-110">Krav</span><span class="sxs-lookup"><span data-stu-id="db342-110">Prerequisites</span></span>

<span data-ttu-id="db342-111">För att konfigurera Azure AD-integrering med Fuze, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="db342-111">To configure Azure AD integration with Fuze, you need the following items:</span></span>

- <span data-ttu-id="db342-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="db342-112">An Azure AD subscription</span></span>
- <span data-ttu-id="db342-113">En Fuze enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="db342-113">A Fuze single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="db342-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="db342-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="db342-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="db342-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="db342-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="db342-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="db342-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="db342-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="db342-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="db342-118">Scenario description</span></span>
<span data-ttu-id="db342-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="db342-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="db342-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="db342-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="db342-121">Att lägga till Fuze från galleriet</span><span class="sxs-lookup"><span data-stu-id="db342-121">Adding Fuze from the gallery</span></span>
2. <span data-ttu-id="db342-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db342-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-fuze-from-the-gallery"></a><span data-ttu-id="db342-123">Att lägga till Fuze från galleriet</span><span class="sxs-lookup"><span data-stu-id="db342-123">Adding Fuze from the gallery</span></span>
<span data-ttu-id="db342-124">Du måste lägga till Fuze från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Fuze i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="db342-124">To configure the integration of Fuze into Azure AD, you need to add Fuze from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="db342-125">**Utför följande steg för att lägga till Fuze från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="db342-125">**To add Fuze from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="db342-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="db342-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="db342-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="db342-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="db342-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="db342-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="db342-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db342-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="db342-133">I sökrutan skriver **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="db342-133">In the search box, type **Fuze**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_000.png)

5. <span data-ttu-id="db342-135">Välj i resultatpanelen **Fuze**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="db342-135">In the results panel, select **Fuze**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="db342-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db342-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="db342-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Fuze baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="db342-138">In this section, you configure and test Azure AD single sign-on with Fuze based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="db342-139">Azure AD måste du känna till användaren i Fuze motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="db342-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Fuze is to a user in Azure AD.</span></span> <span data-ttu-id="db342-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Fuze upprättas.</span><span class="sxs-lookup"><span data-stu-id="db342-140">In other words, a link relationship between an Azure AD user and the related user in Fuze needs to be established.</span></span>

<span data-ttu-id="db342-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Fuze.</span><span class="sxs-lookup"><span data-stu-id="db342-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Fuze.</span></span>

<span data-ttu-id="db342-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Fuze, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="db342-142">To configure and test Azure AD single sign-on with Fuze, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="db342-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="db342-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="db342-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db342-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="db342-145">**[Skapa en testanvändare Fuze](#creating-a-fuze-test-user)**  – du har en motsvarighet för Britta Simon i Fuze som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="db342-145">**[Creating a Fuze test user](#creating-a-fuze-test-user)** - to have a counterpart of Britta Simon in Fuze that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="db342-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="db342-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="db342-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="db342-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="db342-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db342-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="db342-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt Fuze program.</span><span class="sxs-lookup"><span data-stu-id="db342-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Fuze application.</span></span>

<span data-ttu-id="db342-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Fuze:**</span><span class="sxs-lookup"><span data-stu-id="db342-150">**To configure Azure AD single sign-on with Fuze, perform the following steps:**</span></span>

1. <span data-ttu-id="db342-151">I Azure-hanteringsportalen på den **Fuze** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="db342-151">In the Azure Management portal, on the **Fuze** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="db342-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="db342-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_01.png)

3. <span data-ttu-id="db342-155">På den **Fuze domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="db342-155">On the **Fuze Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_020.png)
    
    <span data-ttu-id="db342-157">I den **inloggning URL** textruta typen Sign-on-URL som:`https://www.thinkingphones.com/jetspeed/portal/`</span><span class="sxs-lookup"><span data-stu-id="db342-157">In the **Sign on URL** textbox, type the Sign-on URL as: `https://www.thinkingphones.com/jetspeed/portal/`</span></span>

4.  <span data-ttu-id="db342-158">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="db342-158">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="db342-160">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara xml-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="db342-160">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the xml file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_05.png) 

6. <span data-ttu-id="db342-162">Konfigurera enkel inloggning på **Fuze** sida, måste du skicka den hämtade **XML-Metadata för** till [Fuze supportteamet](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="db342-162">To configure single sign-on on **Fuze** side, you need to send the downloaded **Metadata XML** to [Fuze support team](https://www.fuze.com/support).</span></span> <span data-ttu-id="db342-163">De ska ange detta för att få SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="db342-163">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="db342-164">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="db342-164">Creating an Azure AD test user</span></span>
<span data-ttu-id="db342-165">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="db342-165">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="db342-167">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="db342-167">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="db342-168">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="db342-168">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="db342-170">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="db342-170">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="db342-172">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db342-172">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="db342-174">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="db342-174">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-fuze-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="db342-176">a.</span><span class="sxs-lookup"><span data-stu-id="db342-176">a.</span></span> <span data-ttu-id="db342-177">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="db342-177">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="db342-178">b.</span><span class="sxs-lookup"><span data-stu-id="db342-178">b.</span></span> <span data-ttu-id="db342-179">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="db342-179">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="db342-180">c.</span><span class="sxs-lookup"><span data-stu-id="db342-180">c.</span></span> <span data-ttu-id="db342-181">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="db342-181">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="db342-182">d.</span><span class="sxs-lookup"><span data-stu-id="db342-182">d.</span></span> <span data-ttu-id="db342-183">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="db342-183">Click **Create**.</span></span> 


### <a name="creating-a-fuze-test-user"></a><span data-ttu-id="db342-184">Skapa en testanvändare Fuze</span><span class="sxs-lookup"><span data-stu-id="db342-184">Creating a Fuze test user</span></span>

<span data-ttu-id="db342-185">Fuze program stöder fullständig precis i tid användaren etablera så att användarna ska skapas automatiskt när de loggar in.</span><span class="sxs-lookup"><span data-stu-id="db342-185">Fuze application supports full Just in time user provision, so users will get created automatically when they sign-in.</span></span> <span data-ttu-id="db342-186">För andra information kontakta Fuze [stöder](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="db342-186">For any other clarification, please contact Fuze [support](https://www.fuze.com/support).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="db342-187">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="db342-187">Assigning the Azure AD test user</span></span>

<span data-ttu-id="db342-188">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Fuze.</span><span class="sxs-lookup"><span data-stu-id="db342-188">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Fuze.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="db342-190">**Om du vill tilldela Fuze Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="db342-190">**To assign Britta Simon to Fuze, perform the following steps:**</span></span>

1. <span data-ttu-id="db342-191">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="db342-191">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="db342-193">Välj i listan med program **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="db342-193">In the applications list, select **Fuze**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_50.png) 

3. <span data-ttu-id="db342-195">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="db342-195">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="db342-197">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="db342-197">Click **Add** button.</span></span> <span data-ttu-id="db342-198">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db342-198">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="db342-200">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="db342-200">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="db342-201">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db342-201">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="db342-202">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="db342-202">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="testing-single-sign-on"></a><span data-ttu-id="db342-203">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="db342-203">Testing single sign-on</span></span>

<span data-ttu-id="db342-204">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="db342-204">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="db342-205">När du klickar på panelen Fuze på åtkomstpanelen du bör få automatiskt loggat in på ditt Fuze program.</span><span class="sxs-lookup"><span data-stu-id="db342-205">When you click the Fuze tile in the Access Panel, you should get automatically signed-on to your Fuze application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="db342-206">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="db342-206">Additional resources</span></span>

* [<span data-ttu-id="db342-207">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="db342-207">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="db342-208">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="db342-208">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_203.png