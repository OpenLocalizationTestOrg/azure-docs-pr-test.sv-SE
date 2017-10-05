---
title: "Självstudier: Azure Active Directory-integrering med Sciforma | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Sciforma."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: abbfb5ac-7687-4153-b263-8090102dae37
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: jeedes
ms.openlocfilehash: 4a04f5b2b5ccb33dddefc063a89118d87a11ebf5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciforma"></a><span data-ttu-id="22ee2-103">Självstudier: Azure Active Directory-integrering med Sciforma</span><span class="sxs-lookup"><span data-stu-id="22ee2-103">Tutorial: Azure Active Directory integration with Sciforma</span></span>

<span data-ttu-id="22ee2-104">I kursen får lära du att integrera Sciforma med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="22ee2-104">In this tutorial, you learn how to integrate Sciforma with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="22ee2-105">Integrera Sciforma med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="22ee2-105">Integrating Sciforma with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="22ee2-106">Du kan styra i Azure AD som har åtkomst till Sciforma</span><span class="sxs-lookup"><span data-stu-id="22ee2-106">You can control in Azure AD who has access to Sciforma</span></span>
- <span data-ttu-id="22ee2-107">Du kan aktivera användarna att automatiskt hämta loggat in på Sciforma (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="22ee2-107">You can enable your users to automatically get signed-on to Sciforma (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="22ee2-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="22ee2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="22ee2-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="22ee2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22ee2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="22ee2-110">Prerequisites</span></span>

<span data-ttu-id="22ee2-111">För att konfigurera Azure AD-integrering med Sciforma, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="22ee2-111">To configure Azure AD integration with Sciforma, you need the following items:</span></span>

- <span data-ttu-id="22ee2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="22ee2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="22ee2-113">En Sciforma enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="22ee2-113">A Sciforma single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="22ee2-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="22ee2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="22ee2-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="22ee2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="22ee2-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="22ee2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="22ee2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="22ee2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="22ee2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="22ee2-118">Scenario description</span></span>
<span data-ttu-id="22ee2-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="22ee2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="22ee2-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="22ee2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="22ee2-121">Att lägga till Sciforma från galleriet</span><span class="sxs-lookup"><span data-stu-id="22ee2-121">Adding Sciforma from the gallery</span></span>
2. <span data-ttu-id="22ee2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22ee2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciforma-from-the-gallery"></a><span data-ttu-id="22ee2-123">Att lägga till Sciforma från galleriet</span><span class="sxs-lookup"><span data-stu-id="22ee2-123">Adding Sciforma from the gallery</span></span>
<span data-ttu-id="22ee2-124">Du måste lägga till Sciforma från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Sciforma i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="22ee2-124">To configure the integration of Sciforma into Azure AD, you need to add Sciforma from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="22ee2-125">**Utför följande steg för att lägga till Sciforma från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="22ee2-125">**To add Sciforma from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="22ee2-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="22ee2-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="22ee2-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="22ee2-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="22ee2-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22ee2-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="22ee2-133">I sökrutan skriver **Sciforma**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-133">In the search box, type **Sciforma**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_search.png)

5. <span data-ttu-id="22ee2-135">Välj i resultatpanelen **Sciforma**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="22ee2-135">In the results panel, select **Sciforma**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="22ee2-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22ee2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="22ee2-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Sciforma baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="22ee2-138">In this section, you configure and test Azure AD single sign-on with Sciforma based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="22ee2-139">Azure AD måste du känna till användaren i Sciforma motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="22ee2-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sciforma is to a user in Azure AD.</span></span> <span data-ttu-id="22ee2-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Sciforma upprättas.</span><span class="sxs-lookup"><span data-stu-id="22ee2-140">In other words, a link relationship between an Azure AD user and the related user in Sciforma needs to be established.</span></span>

<span data-ttu-id="22ee2-141">I Sciforma, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="22ee2-141">In Sciforma, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="22ee2-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Sciforma, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="22ee2-142">To configure and test Azure AD single sign-on with Sciforma, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="22ee2-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="22ee2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="22ee2-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22ee2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="22ee2-145">**[Skapa en testanvändare Sciforma](#creating-a-sciforma-test-user)**  – du har en motsvarighet för Britta Simon i Sciforma som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="22ee2-145">**[Creating a Sciforma test user](#creating-a-sciforma-test-user)** - to have a counterpart of Britta Simon in Sciforma that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="22ee2-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="22ee2-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="22ee2-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="22ee2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="22ee2-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22ee2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="22ee2-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Sciforma program.</span><span class="sxs-lookup"><span data-stu-id="22ee2-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sciforma application.</span></span>

<span data-ttu-id="22ee2-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Sciforma:**</span><span class="sxs-lookup"><span data-stu-id="22ee2-150">**To configure Azure AD single sign-on with Sciforma, perform the following steps:**</span></span>

1. <span data-ttu-id="22ee2-151">I Azure-portalen på den **Sciforma** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-151">In the Azure portal, on the **Sciforma** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="22ee2-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="22ee2-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_samlbase.png)

3. <span data-ttu-id="22ee2-155">På den **Sciforma domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="22ee2-155">On the **Sciforma Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_url.png)

    <span data-ttu-id="22ee2-157">a.</span><span class="sxs-lookup"><span data-stu-id="22ee2-157">a.</span></span> <span data-ttu-id="22ee2-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.sciforma.net/sciforma/main.html`</span><span class="sxs-lookup"><span data-stu-id="22ee2-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sciforma.net/sciforma/main.html`</span></span>

    <span data-ttu-id="22ee2-159">b.</span><span class="sxs-lookup"><span data-stu-id="22ee2-159">b.</span></span> <span data-ttu-id="22ee2-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.sciforma.net/sciforma/saml`</span><span class="sxs-lookup"><span data-stu-id="22ee2-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.sciforma.net/sciforma/saml`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="22ee2-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="22ee2-161">These values are not real.</span></span> <span data-ttu-id="22ee2-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="22ee2-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="22ee2-163">Kontakta [Sciforma klienten supportteamet](http://www.sciforma.com/company/contact_us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="22ee2-163">Contact [Sciforma Client support team](http://www.sciforma.com/company/contact_us) to get these values.</span></span> 
 


4. <span data-ttu-id="22ee2-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="22ee2-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_certificate.png) 

5. <span data-ttu-id="22ee2-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="22ee2-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="22ee2-168">Konfigurera enkel inloggning på **Sciforma** sida, måste du skicka den hämtade **XML-Metadata för** till [Sciforma supportteamet](http://www.sciforma.com/company/contact_us).</span><span class="sxs-lookup"><span data-stu-id="22ee2-168">To configure single sign-on on **Sciforma** side, you need to send the downloaded **Metadata XML** to [Sciforma support team](http://www.sciforma.com/company/contact_us).</span></span>

> [!TIP]
> <span data-ttu-id="22ee2-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="22ee2-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="22ee2-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="22ee2-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="22ee2-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="22ee2-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="22ee2-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="22ee2-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="22ee2-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="22ee2-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="22ee2-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="22ee2-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="22ee2-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="22ee2-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="22ee2-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="22ee2-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22ee2-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="22ee2-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="22ee2-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sciforma-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="22ee2-184">a.</span><span class="sxs-lookup"><span data-stu-id="22ee2-184">a.</span></span> <span data-ttu-id="22ee2-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="22ee2-186">b.</span><span class="sxs-lookup"><span data-stu-id="22ee2-186">b.</span></span> <span data-ttu-id="22ee2-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="22ee2-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="22ee2-188">c.</span><span class="sxs-lookup"><span data-stu-id="22ee2-188">c.</span></span> <span data-ttu-id="22ee2-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="22ee2-190">d.</span><span class="sxs-lookup"><span data-stu-id="22ee2-190">d.</span></span> <span data-ttu-id="22ee2-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-191">Click **Create**.</span></span>
 
### <a name="creating-a-sciforma-test-user"></a><span data-ttu-id="22ee2-192">Skapa en testanvändare Sciforma</span><span class="sxs-lookup"><span data-stu-id="22ee2-192">Creating a Sciforma test user</span></span>

<span data-ttu-id="22ee2-193">Det finns inga uppgiften som du kan konfigurera användaretablering till Sciforma.</span><span class="sxs-lookup"><span data-stu-id="22ee2-193">There is no action item for you to configure user provisioning to Sciforma.</span></span> <span data-ttu-id="22ee2-194">När en tilldelad användare försöker logga in med hjälp av åtkomstpanelen Sciforma kontrollerar Sciforma om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="22ee2-194">When an assigned user tries to log in to Sciforma using the access panel, Sciforma checks whether the user exists.</span></span>  

* <span data-ttu-id="22ee2-195">Om det finns inget användarkonto ännu, skapas den automatiskt av Sciforma.</span><span class="sxs-lookup"><span data-stu-id="22ee2-195">If there is no user account available yet, it is automatically created by Sciforma.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="22ee2-196">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="22ee2-196">Assigning the Azure AD test user</span></span>

<span data-ttu-id="22ee2-197">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Sciforma.</span><span class="sxs-lookup"><span data-stu-id="22ee2-197">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sciforma.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="22ee2-199">**Om du vill tilldela Sciforma Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="22ee2-199">**To assign Britta Simon to Sciforma, perform the following steps:**</span></span>

1. <span data-ttu-id="22ee2-200">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-200">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="22ee2-202">Välj i listan med program **Sciforma**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-202">In the applications list, select **Sciforma**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sciforma-tutorial/tutorial_sciforma_app.png) 

3. <span data-ttu-id="22ee2-204">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="22ee2-204">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="22ee2-206">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="22ee2-206">Click **Add** button.</span></span> <span data-ttu-id="22ee2-207">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22ee2-207">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="22ee2-209">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="22ee2-209">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="22ee2-210">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22ee2-210">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="22ee2-211">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="22ee2-211">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="22ee2-212">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="22ee2-212">Testing single sign-on</span></span>

<span data-ttu-id="22ee2-213">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="22ee2-213">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="22ee2-214">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="22ee2-214">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22ee2-215">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="22ee2-215">Additional resources</span></span>

* [<span data-ttu-id="22ee2-216">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="22ee2-216">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="22ee2-217">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="22ee2-217">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sciforma-tutorial/tutorial_general_203.png

