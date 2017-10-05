---
title: "Självstudier: Azure Active Directory-integrering med JobScore | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och JobScore."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f51b32-e55c-4c66-96e8-50a2f9c2194a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: f6ed2d362f7b027bfdc38ba2fdaa03948ff5632c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-jobscore"></a><span data-ttu-id="eee0f-103">Självstudier: Azure Active Directory-integrering med JobScore</span><span class="sxs-lookup"><span data-stu-id="eee0f-103">Tutorial: Azure Active Directory integration with JobScore</span></span>

<span data-ttu-id="eee0f-104">I kursen får lära du att integrera JobScore med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="eee0f-104">In this tutorial, you learn how to integrate JobScore with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="eee0f-105">Integrera JobScore med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="eee0f-105">Integrating JobScore with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="eee0f-106">Du kan styra i Azure AD som har åtkomst till JobScore</span><span class="sxs-lookup"><span data-stu-id="eee0f-106">You can control in Azure AD who has access to JobScore</span></span>
- <span data-ttu-id="eee0f-107">Du kan aktivera användarna att automatiskt hämta loggat in på JobScore (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="eee0f-107">You can enable your users to automatically get signed-on to JobScore (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="eee0f-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="eee0f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="eee0f-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eee0f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eee0f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="eee0f-110">Prerequisites</span></span>

<span data-ttu-id="eee0f-111">För att konfigurera Azure AD-integrering med JobScore, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="eee0f-111">To configure Azure AD integration with JobScore, you need the following items:</span></span>

- <span data-ttu-id="eee0f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="eee0f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="eee0f-113">En JobScore enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="eee0f-113">A JobScore single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="eee0f-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="eee0f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="eee0f-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="eee0f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="eee0f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="eee0f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="eee0f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eee0f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="eee0f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="eee0f-118">Scenario description</span></span>
<span data-ttu-id="eee0f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="eee0f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="eee0f-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="eee0f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eee0f-121">Att lägga till JobScore från galleriet</span><span class="sxs-lookup"><span data-stu-id="eee0f-121">Adding JobScore from the gallery</span></span>
2. <span data-ttu-id="eee0f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eee0f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-jobscore-from-the-gallery"></a><span data-ttu-id="eee0f-123">Att lägga till JobScore från galleriet</span><span class="sxs-lookup"><span data-stu-id="eee0f-123">Adding JobScore from the gallery</span></span>
<span data-ttu-id="eee0f-124">Du måste lägga till JobScore från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av JobScore i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eee0f-124">To configure the integration of JobScore into Azure AD, you need to add JobScore from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eee0f-125">**Utför följande steg för att lägga till JobScore från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="eee0f-125">**To add JobScore from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="eee0f-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="eee0f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="eee0f-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="eee0f-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="eee0f-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eee0f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="eee0f-133">I sökrutan skriver **JobScore**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-133">In the search box, type **JobScore**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_search.png)

5. <span data-ttu-id="eee0f-135">Välj i resultatpanelen **JobScore**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="eee0f-135">In the results panel, select **JobScore**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="eee0f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eee0f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="eee0f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med JobScore baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="eee0f-138">In this section, you configure and test Azure AD single sign-on with JobScore based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="eee0f-139">Azure AD måste du känna till användaren i JobScore motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="eee0f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in JobScore is to a user in Azure AD.</span></span> <span data-ttu-id="eee0f-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i JobScore upprättas.</span><span class="sxs-lookup"><span data-stu-id="eee0f-140">In other words, a link relationship between an Azure AD user and the related user in JobScore needs to be established.</span></span>

<span data-ttu-id="eee0f-141">I JobScore, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="eee0f-141">In JobScore, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="eee0f-142">Om du vill konfigurera och testa Azure AD enkel inloggning med JobScore, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="eee0f-142">To configure and test Azure AD single sign-on with JobScore, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="eee0f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="eee0f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="eee0f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eee0f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eee0f-145">**[Skapa en testanvändare JobScore](#creating-a-jobscore-test-user)**  – du har en motsvarighet för Britta Simon i JobScore som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="eee0f-145">**[Creating a JobScore test user](#creating-a-jobscore-test-user)** - to have a counterpart of Britta Simon in JobScore that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="eee0f-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="eee0f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eee0f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="eee0f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="eee0f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eee0f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="eee0f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt JobScore program.</span><span class="sxs-lookup"><span data-stu-id="eee0f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your JobScore application.</span></span>

<span data-ttu-id="eee0f-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med JobScore:**</span><span class="sxs-lookup"><span data-stu-id="eee0f-150">**To configure Azure AD single sign-on with JobScore, perform the following steps:**</span></span>

1. <span data-ttu-id="eee0f-151">I Azure-portalen på den **JobScore** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-151">In the Azure portal, on the **JobScore** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="eee0f-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="eee0f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_samlbase.png)

3. <span data-ttu-id="eee0f-155">På den **JobScore domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="eee0f-155">On the **JobScore Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_url.png)

    <span data-ttu-id="eee0f-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://hire.jobscore.com/auth/adfs/<company name>`</span><span class="sxs-lookup"><span data-stu-id="eee0f-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://hire.jobscore.com/auth/adfs/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="eee0f-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="eee0f-158">This value is not real.</span></span> <span data-ttu-id="eee0f-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="eee0f-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="eee0f-160">Kontakta [JobScore klienten supportteamet](mailto:support@jobscore.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="eee0f-160">Contact [JobScore Client support team](mailto:support@jobscore.com) to get this value.</span></span> 
 
4. <span data-ttu-id="eee0f-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="eee0f-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_certificate.png) 

5. <span data-ttu-id="eee0f-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="eee0f-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="eee0f-165">Konfigurera enkel inloggning på **JobScore** sida, måste du skicka den hämtade **XML-Metadata för** till [JobScore supportteamet](mailto:support@jobscore.com).</span><span class="sxs-lookup"><span data-stu-id="eee0f-165">To configure single sign-on on **JobScore** side, you need to send the downloaded **Metadata XML** to [JobScore support team](mailto:support@jobscore.com).</span></span> 

> [!TIP]
> <span data-ttu-id="eee0f-166">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="eee0f-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="eee0f-167">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="eee0f-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="eee0f-168">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="eee0f-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="eee0f-169">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="eee0f-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="eee0f-170">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="eee0f-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="eee0f-172">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="eee0f-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="eee0f-173">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="eee0f-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="eee0f-175">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="eee0f-177">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eee0f-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eee0f-179">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="eee0f-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="eee0f-181">a.</span><span class="sxs-lookup"><span data-stu-id="eee0f-181">a.</span></span> <span data-ttu-id="eee0f-182">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="eee0f-183">b.</span><span class="sxs-lookup"><span data-stu-id="eee0f-183">b.</span></span> <span data-ttu-id="eee0f-184">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="eee0f-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="eee0f-185">c.</span><span class="sxs-lookup"><span data-stu-id="eee0f-185">c.</span></span> <span data-ttu-id="eee0f-186">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="eee0f-187">d.</span><span class="sxs-lookup"><span data-stu-id="eee0f-187">d.</span></span> <span data-ttu-id="eee0f-188">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-188">Click **Create**.</span></span>
 
### <a name="creating-a-jobscore-test-user"></a><span data-ttu-id="eee0f-189">Skapa en testanvändare JobScore</span><span class="sxs-lookup"><span data-stu-id="eee0f-189">Creating a JobScore test user</span></span>

<span data-ttu-id="eee0f-190">I det här avsnittet skapar du en användare som kallas Britta Simon i JobScore.</span><span class="sxs-lookup"><span data-stu-id="eee0f-190">In this section, you create a user called Britta Simon in JobScore.</span></span> <span data-ttu-id="eee0f-191">Arbeta med [JobScore supportteamet](mailto:support@jobscore.com) att lägga till användare i JobScore-plattformen.</span><span class="sxs-lookup"><span data-stu-id="eee0f-191">Work with [JobScore support team](mailto:support@jobscore.com) to add the users in the JobScore platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="eee0f-192">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="eee0f-192">Assigning the Azure AD test user</span></span>

<span data-ttu-id="eee0f-193">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till JobScore.</span><span class="sxs-lookup"><span data-stu-id="eee0f-193">In this section, you enable Britta Simon to use Azure single sign-on by granting access to JobScore.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="eee0f-195">**Om du vill tilldela JobScore Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="eee0f-195">**To assign Britta Simon to JobScore, perform the following steps:**</span></span>

1. <span data-ttu-id="eee0f-196">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-196">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="eee0f-198">Välj i listan med program **JobScore**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-198">In the applications list, select **JobScore**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_app.png) 

3. <span data-ttu-id="eee0f-200">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="eee0f-200">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="eee0f-202">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="eee0f-202">Click **Add** button.</span></span> <span data-ttu-id="eee0f-203">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eee0f-203">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="eee0f-205">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="eee0f-205">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="eee0f-206">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eee0f-206">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="eee0f-207">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="eee0f-207">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="eee0f-208">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="eee0f-208">Testing single sign-on</span></span>

<span data-ttu-id="eee0f-209">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="eee0f-209">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="eee0f-210">När du klickar på panelen JobScore på åtkomstpanelen du bör få automatiskt loggat in på ditt JobScore program.</span><span class="sxs-lookup"><span data-stu-id="eee0f-210">When you click the JobScore tile in the Access Panel, you should get automatically signed-on to your JobScore application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eee0f-211">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="eee0f-211">Additional resources</span></span>

* [<span data-ttu-id="eee0f-212">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eee0f-212">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eee0f-213">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="eee0f-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_203.png

