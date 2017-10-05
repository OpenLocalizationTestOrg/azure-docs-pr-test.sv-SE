---
title: "Självstudier: Azure Active Directory-integrering med MCM | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och MCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7f00799d-e3e9-4ba9-ae4a-fbca843ac5db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: cbbb0f54b7954c0ec7326fb62bb427155527cc84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mcm"></a><span data-ttu-id="d5ffb-103">Självstudier: Azure Active Directory-integrering med MCM</span><span class="sxs-lookup"><span data-stu-id="d5ffb-103">Tutorial: Azure Active Directory integration with MCM</span></span>

<span data-ttu-id="d5ffb-104">I kursen får lära du att integrera MCM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d5ffb-104">In this tutorial, you learn how to integrate MCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d5ffb-105">Integrera MCM med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d5ffb-105">Integrating MCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d5ffb-106">Du kan styra i Azure AD som har åtkomst till MCM</span><span class="sxs-lookup"><span data-stu-id="d5ffb-106">You can control in Azure AD who has access to MCM</span></span>
- <span data-ttu-id="d5ffb-107">Du kan aktivera användarna att automatiskt hämta loggat in på MCM (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d5ffb-107">You can enable your users to automatically get signed-on to MCM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d5ffb-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d5ffb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d5ffb-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d5ffb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5ffb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d5ffb-110">Prerequisites</span></span>

<span data-ttu-id="d5ffb-111">Om du vill konfigurera Azure AD-integrering med MCM behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d5ffb-111">To configure Azure AD integration with MCM, you need the following items:</span></span>

- <span data-ttu-id="d5ffb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d5ffb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d5ffb-113">En MCM enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d5ffb-113">A MCM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d5ffb-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d5ffb-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d5ffb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d5ffb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d5ffb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5ffb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d5ffb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d5ffb-118">Scenario description</span></span>
<span data-ttu-id="d5ffb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d5ffb-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d5ffb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d5ffb-121">Att lägga till MCM från galleriet</span><span class="sxs-lookup"><span data-stu-id="d5ffb-121">Adding MCM from the gallery</span></span>
2. <span data-ttu-id="d5ffb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5ffb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mcm-from-the-gallery"></a><span data-ttu-id="d5ffb-123">Att lägga till MCM från galleriet</span><span class="sxs-lookup"><span data-stu-id="d5ffb-123">Adding MCM from the gallery</span></span>
<span data-ttu-id="d5ffb-124">Du måste lägga till MCM från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av MCM i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-124">To configure the integration of MCM into Azure AD, you need to add MCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d5ffb-125">**Utför följande steg för att lägga till MCM från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d5ffb-125">**To add MCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d5ffb-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d5ffb-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d5ffb-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d5ffb-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d5ffb-133">I sökrutan skriver **MCM**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-133">In the search box, type **MCM**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_search.png)

5. <span data-ttu-id="d5ffb-135">Välj i resultatpanelen **MCM**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-135">In the results panel, select **MCM**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d5ffb-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5ffb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d5ffb-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med MCM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-138">In this section, you configure and test Azure AD single sign-on with MCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d5ffb-139">Azure AD måste du känna till användaren i MCM motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in MCM is to a user in Azure AD.</span></span> <span data-ttu-id="d5ffb-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i MCM upprättas.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-140">In other words, a link relationship between an Azure AD user and the related user in MCM needs to be established.</span></span>

<span data-ttu-id="d5ffb-141">I MCM, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-141">In MCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d5ffb-142">Om du vill konfigurera och testa Azure AD enkel inloggning med MCM, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d5ffb-142">To configure and test Azure AD single sign-on with MCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d5ffb-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d5ffb-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d5ffb-145">**[Skapa en MCM testanvändare](#creating-a-mcm-test-user)**  – har en motsvarighet för Britta Simon MCM som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-145">**[Creating a MCM test user](#creating-a-mcm-test-user)** - to have a counterpart of Britta Simon in MCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d5ffb-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d5ffb-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d5ffb-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5ffb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d5ffb-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet MCM.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MCM application.</span></span>

<span data-ttu-id="d5ffb-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med MCM:**</span><span class="sxs-lookup"><span data-stu-id="d5ffb-150">**To configure Azure AD single sign-on with MCM, perform the following steps:**</span></span>

1. <span data-ttu-id="d5ffb-151">I Azure-portalen på den **MCM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-151">In the Azure portal, on the **MCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d5ffb-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_samlbase.png)

3. <span data-ttu-id="d5ffb-155">På den **MCM domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d5ffb-155">On the **MCM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_url.png)

    <span data-ttu-id="d5ffb-157">a.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-157">a.</span></span> <span data-ttu-id="d5ffb-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://myaba.co.uk/client-access/<companyname>/saml.php`</span><span class="sxs-lookup"><span data-stu-id="d5ffb-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://myaba.co.uk/client-access/<companyname>/saml.php`</span></span>

    <span data-ttu-id="d5ffb-159">b.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-159">b.</span></span> <span data-ttu-id="d5ffb-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://myaba.co.uk/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="d5ffb-160">In the **Identifier** textbox, type a URL using the following pattern: `https://myaba.co.uk/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d5ffb-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-161">These values are not real.</span></span> <span data-ttu-id="d5ffb-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d5ffb-163">Kontakta [MCM klienten supportteamet](http://mcmtechnology.com/support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-163">Contact [MCM Client support team](http://mcmtechnology.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="d5ffb-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_certificate.png) 

5. <span data-ttu-id="d5ffb-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mcm-tutorial/tutorial_general_400.png) 

6. <span data-ttu-id="d5ffb-168">Konfigurera enkel inloggning på **MCM** sida, måste du skicka den hämtade **XML-Metadata för** till [MCM supportteam](http://mcmtechnology.com/support/).</span><span class="sxs-lookup"><span data-stu-id="d5ffb-168">To configure single sign-on on **MCM** side, you need to send the downloaded **Metadata XML** to [MCM support team](http://mcmtechnology.com/support/).</span></span> <span data-ttu-id="d5ffb-169">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d5ffb-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d5ffb-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d5ffb-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d5ffb-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d5ffb-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d5ffb-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d5ffb-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="d5ffb-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d5ffb-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d5ffb-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d5ffb-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d5ffb-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d5ffb-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d5ffb-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d5ffb-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d5ffb-185">a.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-185">a.</span></span> <span data-ttu-id="d5ffb-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d5ffb-187">b.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-187">b.</span></span> <span data-ttu-id="d5ffb-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d5ffb-189">c.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-189">c.</span></span> <span data-ttu-id="d5ffb-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d5ffb-191">d.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-191">d.</span></span> <span data-ttu-id="d5ffb-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-192">Click **Create**.</span></span>
 
### <a name="creating-a-mcm-test-user"></a><span data-ttu-id="d5ffb-193">Skapa en testanvändare MCM</span><span class="sxs-lookup"><span data-stu-id="d5ffb-193">Creating a MCM test user</span></span>

<span data-ttu-id="d5ffb-194">I det här avsnittet skapar du en användare som kallas Britta Simon i MCM.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-194">In this section, you create a user called Britta Simon in MCM.</span></span> <span data-ttu-id="d5ffb-195">Arbeta med [MCM supportteam](http://mcmtechnology.com/support/) att lägga till användare i MCM-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-195">Work with [MCM support team](http://mcmtechnology.com/support/) to add the users in the MCM platform.</span></span>

> [!NOTE]
> <span data-ttu-id="d5ffb-196">Du kan använda något annat MCM användarens konto skapas verktyg eller API: er som tillhandahålls av MCM etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-196">You can use any other MCM user account creation tools or APIs provided by MCM to provision AAD user accounts.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d5ffb-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d5ffb-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d5ffb-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till MCM.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to MCM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d5ffb-200">**Om du vill tilldela MCM Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d5ffb-200">**To assign Britta Simon to MCM, perform the following steps:**</span></span>

1. <span data-ttu-id="d5ffb-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d5ffb-203">Välj i listan med program **MCM**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-203">In the applications list, select **MCM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_app.png) 

3. <span data-ttu-id="d5ffb-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d5ffb-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-207">Click **Add** button.</span></span> <span data-ttu-id="d5ffb-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d5ffb-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d5ffb-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d5ffb-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d5ffb-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d5ffb-213">Testing single sign-on</span></span>

<span data-ttu-id="d5ffb-214">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-214">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d5ffb-215">När du klickar på panelen MCM på åtkomstpanelen du bör få automatiskt loggat in på ditt MCM-program.</span><span class="sxs-lookup"><span data-stu-id="d5ffb-215">When you click the MCM tile in the Access Panel, you should get automatically signed-on to your MCM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5ffb-216">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d5ffb-216">Additional resources</span></span>

* [<span data-ttu-id="d5ffb-217">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d5ffb-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d5ffb-218">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d5ffb-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mcm-tutorial/tutorial_general_203.png

