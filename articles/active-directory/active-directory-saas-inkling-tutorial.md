---
title: "Självstudier: Azure Active Directory-integrering med Inkling | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Inkling."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 64c7ee45-ee8a-42f7-bf04-fd0e00833ea9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 7b0639c6515298731f88346c2e4ca82664653a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-inkling"></a><span data-ttu-id="8ec11-103">Självstudier: Azure Active Directory-integrering med Inkling</span><span class="sxs-lookup"><span data-stu-id="8ec11-103">Tutorial: Azure Active Directory integration with Inkling</span></span>

<span data-ttu-id="8ec11-104">I kursen får lära du att integrera Inkling med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8ec11-104">In this tutorial, you learn how to integrate Inkling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8ec11-105">Integrera Inkling med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8ec11-105">Integrating Inkling with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8ec11-106">Du kan styra i Azure AD som har åtkomst till Inkling</span><span class="sxs-lookup"><span data-stu-id="8ec11-106">You can control in Azure AD who has access to Inkling</span></span>
- <span data-ttu-id="8ec11-107">Du kan aktivera användarna att automatiskt hämta loggat in på Inkling (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8ec11-107">You can enable your users to automatically get signed-on to Inkling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8ec11-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="8ec11-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="8ec11-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8ec11-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ec11-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8ec11-110">Prerequisites</span></span>

<span data-ttu-id="8ec11-111">För att konfigurera Azure AD-integrering med Inkling, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8ec11-111">To configure Azure AD integration with Inkling, you need the following items:</span></span>

- <span data-ttu-id="8ec11-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8ec11-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8ec11-113">En Inkling enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="8ec11-113">An Inkling single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="8ec11-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8ec11-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="8ec11-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8ec11-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8ec11-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8ec11-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="8ec11-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8ec11-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="8ec11-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8ec11-118">Scenario description</span></span>
<span data-ttu-id="8ec11-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8ec11-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8ec11-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8ec11-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8ec11-121">Att lägga till Inkling från galleriet</span><span class="sxs-lookup"><span data-stu-id="8ec11-121">Adding Inkling from the gallery</span></span>
2. <span data-ttu-id="8ec11-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ec11-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-inkling-from-the-gallery"></a><span data-ttu-id="8ec11-123">Att lägga till Inkling från galleriet</span><span class="sxs-lookup"><span data-stu-id="8ec11-123">Adding Inkling from the gallery</span></span>
<span data-ttu-id="8ec11-124">Du måste lägga till Inkling från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Inkling i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8ec11-124">To configure the integration of Inkling into Azure AD, you need to add Inkling from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8ec11-125">**Utför följande steg för att lägga till Inkling från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8ec11-125">**To add Inkling from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8ec11-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8ec11-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8ec11-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8ec11-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8ec11-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ec11-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8ec11-133">I sökrutan skriver **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-133">In the search box, type **Inkling**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_001.png)

5. <span data-ttu-id="8ec11-135">Välj i resultatpanelen **Inkling**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8ec11-135">In the results panel, select **Inkling**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8ec11-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ec11-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8ec11-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Inkling baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8ec11-138">In this section, you configure and test Azure AD single sign-on with Inkling based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8ec11-139">Azure AD måste du känna till användaren i Inkling motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8ec11-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Inkling is to a user in Azure AD.</span></span> <span data-ttu-id="8ec11-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Inkling upprättas.</span><span class="sxs-lookup"><span data-stu-id="8ec11-140">In other words, a link relationship between an Azure AD user and the related user in Inkling needs to be established.</span></span>

<span data-ttu-id="8ec11-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Inkling.</span><span class="sxs-lookup"><span data-stu-id="8ec11-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Inkling.</span></span>

<span data-ttu-id="8ec11-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Inkling, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8ec11-142">To configure and test Azure AD single sign-on with Inkling, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8ec11-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8ec11-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8ec11-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ec11-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8ec11-145">**[Skapa en testanvändare Inkling](#creating-an-inkling-test-user)**  – du har en motsvarighet för Britta Simon i Inkling som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="8ec11-145">**[Creating an Inkling test user](#creating-an-inkling-test-user)** - to have a counterpart of Britta Simon in Inkling that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="8ec11-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8ec11-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8ec11-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8ec11-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8ec11-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ec11-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8ec11-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt Inkling program.</span><span class="sxs-lookup"><span data-stu-id="8ec11-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Inkling application.</span></span>

<span data-ttu-id="8ec11-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Inkling:**</span><span class="sxs-lookup"><span data-stu-id="8ec11-150">**To configure Azure AD single sign-on with Inkling, perform the following steps:**</span></span>

1. <span data-ttu-id="8ec11-151">I Azure-hanteringsportalen på den **Inkling** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-151">In the Azure Management portal, on the **Inkling** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8ec11-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="8ec11-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="8ec11-155">På den **Inkling domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8ec11-155">On the **Inkling Domain and URLs** section, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_01.png)

    <span data-ttu-id="8ec11-157">a.</span><span class="sxs-lookup"><span data-stu-id="8ec11-157">a.</span></span> <span data-ttu-id="8ec11-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://api.inkling.com/saml/v2/metadata/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="8ec11-158">In the **Identifier** textbox, type a URL using the following pattern: `https://api.inkling.com/saml/v2/metadata/<user-id>`</span></span>

    <span data-ttu-id="8ec11-159">b.</span><span class="sxs-lookup"><span data-stu-id="8ec11-159">b.</span></span> <span data-ttu-id="8ec11-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://api.inkling.com/saml/v2/acs/<user-id>`</span><span class="sxs-lookup"><span data-stu-id="8ec11-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://api.inkling.com/saml/v2/acs/<user-id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8ec11-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="8ec11-161">Please note that these are not the real values.</span></span> <span data-ttu-id="8ec11-162">Du måste uppdatera dessa värden med de faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="8ec11-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="8ec11-163">Kontakta [Inkling supportteamet](mailto:press@inkling.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8ec11-163">Contact [Inkling support team](mailto:press@inkling.com) to get these values.</span></span>

4. <span data-ttu-id="8ec11-164">På den **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-164">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_400.png)    

5. <span data-ttu-id="8ec11-166">På den **skapa nya certifikat** dialogrutan, klicka på kalenderikonen och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-166">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="8ec11-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8ec11-167">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="8ec11-169">På den **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8ec11-169">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_02.png)

7. <span data-ttu-id="8ec11-171">På popup-fönstret **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-171">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="8ec11-173">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8ec11-173">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_03.png) 

9. <span data-ttu-id="8ec11-175">För att få SSO konfigurerats för ditt program, kontakta [Inkling supportteamet](mailto:press@inkling.com) och ge dem med hämtade **metadata**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-175">To get SSO configured for your application, contact [Inkling support team](mailto:press@inkling.com) and provide them with downloaded **metadata**.</span></span> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8ec11-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8ec11-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="8ec11-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8ec11-177">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8ec11-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8ec11-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8ec11-180">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8ec11-180">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8ec11-182">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="8ec11-182">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8ec11-184">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ec11-184">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8ec11-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8ec11-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-inkling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8ec11-188">a.</span><span class="sxs-lookup"><span data-stu-id="8ec11-188">a.</span></span> <span data-ttu-id="8ec11-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8ec11-190">b.</span><span class="sxs-lookup"><span data-stu-id="8ec11-190">b.</span></span> <span data-ttu-id="8ec11-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8ec11-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8ec11-192">c.</span><span class="sxs-lookup"><span data-stu-id="8ec11-192">c.</span></span> <span data-ttu-id="8ec11-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8ec11-194">d.</span><span class="sxs-lookup"><span data-stu-id="8ec11-194">d.</span></span> <span data-ttu-id="8ec11-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-195">Click **Create**.</span></span> 



### <a name="creating-an-inkling-test-user"></a><span data-ttu-id="8ec11-196">Skapa en testanvändare Inkling</span><span class="sxs-lookup"><span data-stu-id="8ec11-196">Creating an Inkling test user</span></span>

<span data-ttu-id="8ec11-197">I det här avsnittet skapar du en användare som kallas Britta Simon i Inkling.</span><span class="sxs-lookup"><span data-stu-id="8ec11-197">In this section, you create a user called Britta Simon in Inkling.</span></span> <span data-ttu-id="8ec11-198">Se tillsammans med [Inkling supportteamet](mailto:press@inkling.com) att lägga till användare i Inkling-plattformen.</span><span class="sxs-lookup"><span data-stu-id="8ec11-198">Please work with [Inkling support team](mailto:press@inkling.com) to add the users in the Inkling platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8ec11-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8ec11-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8ec11-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Inkling.</span><span class="sxs-lookup"><span data-stu-id="8ec11-200">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Inkling.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8ec11-202">**Om du vill tilldela Inkling Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8ec11-202">**To assign Britta Simon to Inkling, perform the following steps:**</span></span>

1. <span data-ttu-id="8ec11-203">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-203">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8ec11-205">Välj i listan med program **Inkling**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-205">In the applications list, select **Inkling**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-inkling-tutorial/tutorial_inkling_50.png) 

3. <span data-ttu-id="8ec11-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8ec11-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8ec11-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8ec11-209">Click **Add** button.</span></span> <span data-ttu-id="8ec11-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ec11-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8ec11-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8ec11-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8ec11-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ec11-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8ec11-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8ec11-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="8ec11-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8ec11-215">Testing single sign-on</span></span>

<span data-ttu-id="8ec11-216">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8ec11-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8ec11-217">När du klickar på panelen Inkling på åtkomstpanelen du bör få automatiskt loggat in på ditt Inkling program.</span><span class="sxs-lookup"><span data-stu-id="8ec11-217">When you click the Inkling tile in the Access Panel, you should get automatically signed-on to your Inkling application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="8ec11-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8ec11-218">Additional resources</span></span>

* [<span data-ttu-id="8ec11-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8ec11-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8ec11-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8ec11-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-inkling-tutorial/tutorial_general_203.png