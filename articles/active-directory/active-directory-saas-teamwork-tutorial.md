---
title: "Självstudier: Azure Active Directory-integrering med samarbete | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och samarbete."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03760032-3d76-4b47-ab84-241f72fbd561
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: edd2f9446515531f1147a8abf99295b618b89b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamwork"></a><span data-ttu-id="65ec4-103">Självstudier: Azure Active Directory-integrering med samarbete</span><span class="sxs-lookup"><span data-stu-id="65ec4-103">Tutorial: Azure Active Directory integration with Teamwork</span></span>

<span data-ttu-id="65ec4-104">I kursen får lära du att integrera samarbete med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="65ec4-104">In this tutorial, you learn how to integrate Teamwork with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="65ec4-105">Integrera samarbete med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="65ec4-105">Integrating Teamwork with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="65ec4-106">Du kan styra i Azure AD som har åtkomst till samarbete</span><span class="sxs-lookup"><span data-stu-id="65ec4-106">You can control in Azure AD who has access to Teamwork</span></span>
- <span data-ttu-id="65ec4-107">Du kan aktivera användarna att automatiskt hämta loggat in på samarbete (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="65ec4-107">You can enable your users to automatically get signed-on to Teamwork (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="65ec4-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="65ec4-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="65ec4-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="65ec4-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65ec4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="65ec4-110">Prerequisites</span></span>

<span data-ttu-id="65ec4-111">För att konfigurera Azure AD-integrering med samarbete, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="65ec4-111">To configure Azure AD integration with Teamwork, you need the following items:</span></span>

- <span data-ttu-id="65ec4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="65ec4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="65ec4-113">Ett samarbete enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="65ec4-113">A Teamwork single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="65ec4-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="65ec4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="65ec4-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="65ec4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="65ec4-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="65ec4-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="65ec4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="65ec4-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="65ec4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="65ec4-118">Scenario description</span></span>
<span data-ttu-id="65ec4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="65ec4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="65ec4-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="65ec4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="65ec4-121">Att lägga till samarbete från galleriet</span><span class="sxs-lookup"><span data-stu-id="65ec4-121">Adding Teamwork from the gallery</span></span>
2. <span data-ttu-id="65ec4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ec4-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-teamwork-from-the-gallery"></a><span data-ttu-id="65ec4-123">Att lägga till samarbete från galleriet</span><span class="sxs-lookup"><span data-stu-id="65ec4-123">Adding Teamwork from the gallery</span></span>
<span data-ttu-id="65ec4-124">Du måste lägga till samarbete från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av samarbete i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="65ec4-124">To configure the integration of Teamwork into Azure AD, you need to add Teamwork from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="65ec4-125">**Utför följande steg för att lägga till samarbete från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="65ec4-125">**To add Teamwork from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="65ec4-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="65ec4-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="65ec4-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="65ec4-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="65ec4-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ec4-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="65ec4-133">I sökrutan skriver **samarbete**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-133">In the search box, type **Teamwork**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_001.png)

5. <span data-ttu-id="65ec4-135">Välj i resultatpanelen **samarbete**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="65ec4-135">In the results panel, select **Teamwork**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="65ec4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ec4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="65ec4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med samarbete baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="65ec4-138">In this section, you configure and test Azure AD single sign-on with Teamwork based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="65ec4-139">Azure AD måste du känna till användaren i samarbete motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="65ec4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Teamwork is to a user in Azure AD.</span></span> <span data-ttu-id="65ec4-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i samarbete upprättas.</span><span class="sxs-lookup"><span data-stu-id="65ec4-140">In other words, a link relationship between an Azure AD user and the related user in Teamwork needs to be established.</span></span>

<span data-ttu-id="65ec4-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i samarbete.</span><span class="sxs-lookup"><span data-stu-id="65ec4-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Teamwork.</span></span>

<span data-ttu-id="65ec4-142">Om du vill konfigurera och testa Azure AD enkel inloggning med samarbete, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="65ec4-142">To configure and test Azure AD single sign-on with Teamwork, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="65ec4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="65ec4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="65ec4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65ec4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="65ec4-145">**[Skapa en testanvändare samarbete](#creating-a-teamwork-test-user)**  – du har en motsvarighet för Britta Simon i samarbete som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="65ec4-145">**[Creating a Teamwork test user](#creating-a-teamwork-test-user)** - to have a counterpart of Britta Simon in Teamwork that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="65ec4-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="65ec4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="65ec4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="65ec4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="65ec4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ec4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="65ec4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt program för samarbete.</span><span class="sxs-lookup"><span data-stu-id="65ec4-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Teamwork application.</span></span>

<span data-ttu-id="65ec4-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med samarbete:**</span><span class="sxs-lookup"><span data-stu-id="65ec4-150">**To configure Azure AD single sign-on with Teamwork, perform the following steps:**</span></span>

1. <span data-ttu-id="65ec4-151">I Azure-hanteringsportalen på den **samarbete** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-151">In the Azure Management portal, on the **Teamwork** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="65ec4-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="65ec4-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_01.png)

3. <span data-ttu-id="65ec4-155">På den **samarbete domän och URL: er** avsnitt i den **logga URL** textruta Skriv en URL med följande mönster:`https://<company name>.teamwork.com`</span><span class="sxs-lookup"><span data-stu-id="65ec4-155">On the **Teamwork Domain and URLs** section, in the **Sign On URL** textbox, type a URL using the following pattern: `https://<company name>.teamwork.com`</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_02.png)

    > [!NOTE] 
    > <span data-ttu-id="65ec4-157">Observera att detta inte är det verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="65ec4-157">Please note that this is not the real value.</span></span> <span data-ttu-id="65ec4-158">Du måste uppdatera det här värdet med faktiska logga på URL: en.</span><span class="sxs-lookup"><span data-stu-id="65ec4-158">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="65ec4-159">Kontakta [samarbete supportteamet](mailto:support@teamwork.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="65ec4-159">Contact [Teamwork support team](mailto:support@teamwork.com) to get this value.</span></span> 

4. <span data-ttu-id="65ec4-160">På den **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-160">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_03.png)   

5. <span data-ttu-id="65ec4-162">På den **skapa nya certifikat** dialogrutan, klicka på kalenderikonen och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-162">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="65ec4-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="65ec4-163">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="65ec4-165">På den **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="65ec4-165">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_04.png)

7. <span data-ttu-id="65ec4-167">På popup-fönstret **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-167">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="65ec4-169">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="65ec4-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_05.png) 

9. <span data-ttu-id="65ec4-171">För att få SSO konfigurerats för ditt program, kontakta [samarbete supportteamet](mailto:support@teamwork.com) och ger dem den hämtade **metadata**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-171">To get SSO configured for your application, contact [Teamwork support team](mailto:support@teamwork.com) and provide them with the downloaded **metadata**.</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="65ec4-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="65ec4-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="65ec4-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="65ec4-173">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="65ec4-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="65ec4-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="65ec4-176">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="65ec4-176">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="65ec4-178">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="65ec4-178">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="65ec4-180">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ec4-180">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="65ec4-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="65ec4-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-teamwork-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="65ec4-184">a.</span><span class="sxs-lookup"><span data-stu-id="65ec4-184">a.</span></span> <span data-ttu-id="65ec4-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="65ec4-186">b.</span><span class="sxs-lookup"><span data-stu-id="65ec4-186">b.</span></span> <span data-ttu-id="65ec4-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="65ec4-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="65ec4-188">c.</span><span class="sxs-lookup"><span data-stu-id="65ec4-188">c.</span></span> <span data-ttu-id="65ec4-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="65ec4-190">d.</span><span class="sxs-lookup"><span data-stu-id="65ec4-190">d.</span></span> <span data-ttu-id="65ec4-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-191">Click **Create**.</span></span> 



### <a name="creating-a-teamwork-test-user"></a><span data-ttu-id="65ec4-192">Skapa en testanvändare samarbete</span><span class="sxs-lookup"><span data-stu-id="65ec4-192">Creating a Teamwork test user</span></span>

<span data-ttu-id="65ec4-193">I det här avsnittet skapar du en användare som kallas Britta Simon i samarbete.</span><span class="sxs-lookup"><span data-stu-id="65ec4-193">In this section, you create a user called Britta Simon in Teamwork.</span></span> <span data-ttu-id="65ec4-194">Se tillsammans med [samarbete supportteamet](mailto:support@teamwork.com) att lägga till användare i samarbete-plattformen.</span><span class="sxs-lookup"><span data-stu-id="65ec4-194">Please work with [Teamwork support team](mailto:support@teamwork.com) to add the users in the Teamwork platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="65ec4-195">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="65ec4-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="65ec4-196">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till samarbete.</span><span class="sxs-lookup"><span data-stu-id="65ec4-196">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Teamwork.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="65ec4-198">**Om du vill tilldela samarbete Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="65ec4-198">**To assign Britta Simon to Teamwork, perform the following steps:**</span></span>

1. <span data-ttu-id="65ec4-199">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-199">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="65ec4-201">Välj i listan med program **samarbete**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-201">In the applications list, select **Teamwork**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_50.png) 

3. <span data-ttu-id="65ec4-203">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="65ec4-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="65ec4-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="65ec4-205">Click **Add** button.</span></span> <span data-ttu-id="65ec4-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ec4-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="65ec4-208">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="65ec4-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="65ec4-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ec4-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="65ec4-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="65ec4-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="65ec4-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="65ec4-211">Testing single sign-on</span></span>

<span data-ttu-id="65ec4-212">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="65ec4-212">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="65ec4-213">När du klickar på panelen samarbete på åtkomstpanelen du bör få automatiskt loggat in på ditt program för samarbete.</span><span class="sxs-lookup"><span data-stu-id="65ec4-213">When you click the Teamwork tile in the Access Panel, you should get automatically signed-on to your Teamwork application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="65ec4-214">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="65ec4-214">Additional resources</span></span>

* [<span data-ttu-id="65ec4-215">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="65ec4-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="65ec4-216">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="65ec4-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_203.png