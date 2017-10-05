---
title: "Självstudier: Azure Active Directory-integrering med Lynda.com | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Lynda.com."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6c92789-8b64-4049-bac9-8cb928398433
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 84ed2adcc2d49ddbb6bd2e9cc3b93b967ebed063
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lyndacom"></a><span data-ttu-id="bbe97-103">Självstudier: Azure Active Directory-integrering med Lynda.com</span><span class="sxs-lookup"><span data-stu-id="bbe97-103">Tutorial: Azure Active Directory integration with Lynda.com</span></span>

<span data-ttu-id="bbe97-104">I kursen får lära du att integrera Lynda.com med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="bbe97-104">In this tutorial, you learn how to integrate Lynda.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bbe97-105">Integrera Lynda.com med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="bbe97-105">Integrating Lynda.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bbe97-106">Du kan styra i Azure AD som har åtkomst till Lynda.com</span><span class="sxs-lookup"><span data-stu-id="bbe97-106">You can control in Azure AD who has access to Lynda.com</span></span>
- <span data-ttu-id="bbe97-107">Du kan aktivera användarna att automatiskt hämta loggat in på Lynda.com (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="bbe97-107">You can enable your users to automatically get signed-on to Lynda.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bbe97-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bbe97-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bbe97-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bbe97-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbe97-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bbe97-110">Prerequisites</span></span>

<span data-ttu-id="bbe97-111">För att konfigurera Azure AD-integrering med Lynda.com, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="bbe97-111">To configure Azure AD integration with Lynda.com, you need the following items:</span></span>

- <span data-ttu-id="bbe97-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="bbe97-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bbe97-113">En Lynda.com enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="bbe97-113">A Lynda.com single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bbe97-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="bbe97-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bbe97-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="bbe97-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bbe97-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="bbe97-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bbe97-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bbe97-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bbe97-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="bbe97-118">Scenario description</span></span>
<span data-ttu-id="bbe97-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="bbe97-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bbe97-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="bbe97-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bbe97-121">Att lägga till Lynda.com från galleriet</span><span class="sxs-lookup"><span data-stu-id="bbe97-121">Adding Lynda.com from the gallery</span></span>
2. <span data-ttu-id="bbe97-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbe97-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lyndacom-from-the-gallery"></a><span data-ttu-id="bbe97-123">Att lägga till Lynda.com från galleriet</span><span class="sxs-lookup"><span data-stu-id="bbe97-123">Adding Lynda.com from the gallery</span></span>
<span data-ttu-id="bbe97-124">Du måste lägga till Lynda.com från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Lynda.com i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="bbe97-124">To configure the integration of Lynda.com into Azure AD, you need to add Lynda.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bbe97-125">**Utför följande steg för att lägga till Lynda.com från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="bbe97-125">**To add Lynda.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bbe97-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bbe97-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bbe97-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bbe97-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="bbe97-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbe97-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="bbe97-133">I sökrutan skriver **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-133">In the search box, type **Lynda.com**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_search.png)

5. <span data-ttu-id="bbe97-135">Välj i resultatpanelen **Lynda.com**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="bbe97-135">In the results panel, select **Lynda.com**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bbe97-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbe97-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bbe97-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Lynda.com baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="bbe97-138">In this section, you configure and test Azure AD single sign-on with Lynda.com based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="bbe97-139">Azure AD måste du känna till användaren i Lynda.com motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="bbe97-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lynda.com is to a user in Azure AD.</span></span> <span data-ttu-id="bbe97-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Lynda.com upprättas.</span><span class="sxs-lookup"><span data-stu-id="bbe97-140">In other words, a link relationship between an Azure AD user and the related user in Lynda.com needs to be established.</span></span>

<span data-ttu-id="bbe97-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="bbe97-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Lynda.com.</span></span>

<span data-ttu-id="bbe97-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Lynda.com, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="bbe97-142">To configure and test Azure AD single sign-on with Lynda.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bbe97-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="bbe97-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bbe97-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bbe97-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bbe97-145">**[Skapa en testanvändare Lynda.com](#creating-a-lyndacom-test-user)**  – du har en motsvarighet för Britta Simon i Lynda.com som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="bbe97-145">**[Creating a Lynda.com test user](#creating-a-lyndacom-test-user)** - to have a counterpart of Britta Simon in Lynda.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bbe97-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bbe97-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bbe97-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="bbe97-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bbe97-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbe97-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bbe97-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Lynda.com program.</span><span class="sxs-lookup"><span data-stu-id="bbe97-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lynda.com application.</span></span>

<span data-ttu-id="bbe97-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Lynda.com:**</span><span class="sxs-lookup"><span data-stu-id="bbe97-150">**To configure Azure AD single sign-on with Lynda.com, perform the following steps:**</span></span>

1. <span data-ttu-id="bbe97-151">I Azure-portalen på den **Lynda.com** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-151">In the Azure portal, on the **Lynda.com** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="bbe97-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="bbe97-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_samlbase.png)

3. <span data-ttu-id="bbe97-155">På den **Lynda.com domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bbe97-155">On the **Lynda.com Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_url.png)

    <span data-ttu-id="bbe97-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span><span class="sxs-lookup"><span data-stu-id="bbe97-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.lynda.com/Shibboleth.sso/InCommon?providerId=<url>&target=<url> `</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bbe97-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="bbe97-158">This value is not real.</span></span> <span data-ttu-id="bbe97-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="bbe97-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="bbe97-160">Kontakta [Lynda.com klienten supportteamet](https://www.linkedin.com/help/lynda/ask) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="bbe97-160">Contact [Lynda.com Client support team](https://www.linkedin.com/help/lynda/ask) to get these values.</span></span> 
 
4. <span data-ttu-id="bbe97-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="bbe97-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_certificate.png) 

5. <span data-ttu-id="bbe97-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="bbe97-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lynda-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bbe97-165">Konfigurera enkel inloggning på **Lynda.com** sida, måste du skicka den hämtade **XML-Metadata för** [Lynda.com support](https://www.linkedin.com/help/lynda/ask).</span><span class="sxs-lookup"><span data-stu-id="bbe97-165">To configure single sign-on on **Lynda.com** side, you need to send the downloaded **Metadata XML** [Lynda.com support](https://www.linkedin.com/help/lynda/ask).</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bbe97-166">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="bbe97-166">Creating an Azure AD test user</span></span>
<span data-ttu-id="bbe97-167">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="bbe97-167">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="bbe97-169">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="bbe97-169">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bbe97-170">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="bbe97-170">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bbe97-172">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-172">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bbe97-174">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbe97-174">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bbe97-176">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bbe97-176">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lynda-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bbe97-178">a.</span><span class="sxs-lookup"><span data-stu-id="bbe97-178">a.</span></span> <span data-ttu-id="bbe97-179">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-179">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bbe97-180">b.</span><span class="sxs-lookup"><span data-stu-id="bbe97-180">b.</span></span> <span data-ttu-id="bbe97-181">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bbe97-181">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bbe97-182">c.</span><span class="sxs-lookup"><span data-stu-id="bbe97-182">c.</span></span> <span data-ttu-id="bbe97-183">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-183">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bbe97-184">d.</span><span class="sxs-lookup"><span data-stu-id="bbe97-184">d.</span></span> <span data-ttu-id="bbe97-185">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-185">Click **Create**.</span></span>
 
### <a name="creating-a-lyndacom-test-user"></a><span data-ttu-id="bbe97-186">Skapa en testanvändare Lynda.com</span><span class="sxs-lookup"><span data-stu-id="bbe97-186">Creating a Lynda.com test user</span></span>

<span data-ttu-id="bbe97-187">Det finns inga uppgiften som du kan konfigurera användaretablering till Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="bbe97-187">There is no action item for you to configure user provisioning to Lynda.com.</span></span>  
<span data-ttu-id="bbe97-188">När en tilldelad användare försöker logga in med hjälp av åtkomstpanelen Lynda.com kontrollerar Lynda.com om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="bbe97-188">When an assigned user tries to log in to Lynda.com using the access panel, Lynda.com checks whether the user exists.</span></span>  

<span data-ttu-id="bbe97-189">Om det finns inget användarkonto ännu, skapas den automatiskt av Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="bbe97-189">If there is no user account available yet, it is automatically created by Lynda.com.</span></span>

>[!NOTE]
><span data-ttu-id="bbe97-190">Du kan använda något annat Lynda.com användarens konto skapas verktyg eller API: er som tillhandahålls av Lynda.com att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="bbe97-190">You can use any other Lynda.com user account creation tools or APIs provided by Lynda.com to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bbe97-191">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="bbe97-191">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bbe97-192">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Lynda.com.</span><span class="sxs-lookup"><span data-stu-id="bbe97-192">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lynda.com.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="bbe97-194">**Om du vill tilldela Lynda.com Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="bbe97-194">**To assign Britta Simon to Lynda.com, perform the following steps:**</span></span>

1. <span data-ttu-id="bbe97-195">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-195">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="bbe97-197">Välj i listan med program **Lynda.com**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-197">In the applications list, select **Lynda.com**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lynda-tutorial/tutorial_lynda.com_app.png) 

3. <span data-ttu-id="bbe97-199">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="bbe97-199">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="bbe97-201">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="bbe97-201">Click **Add** button.</span></span> <span data-ttu-id="bbe97-202">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbe97-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="bbe97-204">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="bbe97-204">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bbe97-205">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbe97-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bbe97-206">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="bbe97-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bbe97-207">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="bbe97-207">Testing single sign-on</span></span>

<span data-ttu-id="bbe97-208">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="bbe97-208">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="bbe97-209">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bbe97-209">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bbe97-210">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="bbe97-210">Additional resources</span></span>

* [<span data-ttu-id="bbe97-211">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bbe97-211">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bbe97-212">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bbe97-212">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lynda-tutorial/tutorial_general_203.png

