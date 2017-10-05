---
title: "Självstudier: Azure Active Directory-integrering med Chromeriver | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Chromeriver."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 445c5600-e340-4724-a9cb-3cfaf5770b70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: 6ee3316a8258ee27e4fa4ec22badbf4fe047a844
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-chromeriver"></a><span data-ttu-id="f4bf7-103">Självstudier: Azure Active Directory-integrering med Chromeriver</span><span class="sxs-lookup"><span data-stu-id="f4bf7-103">Tutorial: Azure Active Directory integration with Chromeriver</span></span>

<span data-ttu-id="f4bf7-104">I kursen får lära du att integrera Chromeriver med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f4bf7-104">In this tutorial, you learn how to integrate Chromeriver with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f4bf7-105">Integrera Chromeriver med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f4bf7-105">Integrating Chromeriver with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f4bf7-106">Du kan styra i Azure AD som har åtkomst till Chromeriver</span><span class="sxs-lookup"><span data-stu-id="f4bf7-106">You can control in Azure AD who has access to Chromeriver</span></span>
- <span data-ttu-id="f4bf7-107">Du kan aktivera användarna att automatiskt hämta loggat in på Chromeriver (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f4bf7-107">You can enable your users to automatically get signed-on to Chromeriver (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f4bf7-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f4bf7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f4bf7-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f4bf7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4bf7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f4bf7-110">Prerequisites</span></span>

<span data-ttu-id="f4bf7-111">För att konfigurera Azure AD-integrering med Chromeriver, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f4bf7-111">To configure Azure AD integration with Chromeriver, you need the following items:</span></span>

- <span data-ttu-id="f4bf7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f4bf7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f4bf7-113">En Chromeriver enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f4bf7-113">A Chromeriver single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f4bf7-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f4bf7-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f4bf7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f4bf7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f4bf7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f4bf7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f4bf7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f4bf7-118">Scenario description</span></span>
<span data-ttu-id="f4bf7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f4bf7-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f4bf7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f4bf7-121">Att lägga till Chromeriver från galleriet</span><span class="sxs-lookup"><span data-stu-id="f4bf7-121">Adding Chromeriver from the gallery</span></span>
2. <span data-ttu-id="f4bf7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4bf7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-chromeriver-from-the-gallery"></a><span data-ttu-id="f4bf7-123">Att lägga till Chromeriver från galleriet</span><span class="sxs-lookup"><span data-stu-id="f4bf7-123">Adding Chromeriver from the gallery</span></span>
<span data-ttu-id="f4bf7-124">Du måste lägga till Chromeriver från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Chromeriver i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-124">To configure the integration of Chromeriver into Azure AD, you need to add Chromeriver from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f4bf7-125">**Utför följande steg för att lägga till Chromeriver från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f4bf7-125">**To add Chromeriver from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f4bf7-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f4bf7-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f4bf7-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f4bf7-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f4bf7-133">I sökrutan skriver **Chromeriver**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-133">In the search box, type **Chromeriver**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_search.png)

5. <span data-ttu-id="f4bf7-135">Välj i resultatpanelen **Chromeriver**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-135">In the results panel, select **Chromeriver**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f4bf7-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4bf7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f4bf7-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Chromeriver baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-138">In this section, you configure and test Azure AD single sign-on with Chromeriver based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f4bf7-139">Azure AD måste du känna till användaren i Chromeriver motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Chromeriver is to a user in Azure AD.</span></span> <span data-ttu-id="f4bf7-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Chromeriver upprättas.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-140">In other words, a link relationship between an Azure AD user and the related user in Chromeriver needs to be established.</span></span>

<span data-ttu-id="f4bf7-141">I Chromeriver, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-141">In Chromeriver, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f4bf7-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Chromeriver, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f4bf7-142">To configure and test Azure AD single sign-on with Chromeriver, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f4bf7-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f4bf7-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f4bf7-145">**[Skapa en testanvändare Chromeriver](#creating-a-chromeriver-test-user)**  – du har en motsvarighet för Britta Simon i Chromeriver som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-145">**[Creating a Chromeriver test user](#creating-a-chromeriver-test-user)** - to have a counterpart of Britta Simon in Chromeriver that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f4bf7-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f4bf7-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f4bf7-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4bf7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f4bf7-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Chromeriver program.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Chromeriver application.</span></span>

<span data-ttu-id="f4bf7-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Chromeriver:**</span><span class="sxs-lookup"><span data-stu-id="f4bf7-150">**To configure Azure AD single sign-on with Chromeriver, perform the following steps:**</span></span>

1. <span data-ttu-id="f4bf7-151">I Azure-portalen på den **Chromeriver** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-151">In the Azure portal, on the **Chromeriver** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f4bf7-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_samlbase.png)

3. <span data-ttu-id="f4bf7-155">På den **Chromeriver domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f4bf7-155">On the **Chromeriver Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_url.png)

    <span data-ttu-id="f4bf7-157">a.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-157">a.</span></span> <span data-ttu-id="f4bf7-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.chromeriver.com`</span><span class="sxs-lookup"><span data-stu-id="f4bf7-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.chromeriver.com`</span></span>

    <span data-ttu-id="f4bf7-159">b.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-159">b.</span></span> <span data-ttu-id="f4bf7-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="f4bf7-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.chromeriver.com/login/sso/saml/consume?customerId=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f4bf7-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-161">These values are not real.</span></span> <span data-ttu-id="f4bf7-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="f4bf7-163">Kontakta [Chromeriver supportteamet](https://www.chromeriver.com/services/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-163">Contact [Chromeriver support team](https://www.chromeriver.com/services/support) to get these values.</span></span>
 


4. <span data-ttu-id="f4bf7-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_certificate.png) 

5. <span data-ttu-id="f4bf7-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f4bf7-168">Konfigurera enkel inloggning på **Chromeriver** sida, måste du skicka den hämtade **XML-Metadata för** till [Chromeriver supportteamet](https://www.chromeriver.com/services/support).</span><span class="sxs-lookup"><span data-stu-id="f4bf7-168">To configure single sign-on on **Chromeriver** side, you need to send the downloaded **Metadata XML** to [Chromeriver support team](https://www.chromeriver.com/services/support).</span></span> <span data-ttu-id="f4bf7-169">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-169">You will get a notification when SSO has been enabled for your subscription.</span></span>

> [!TIP]
> <span data-ttu-id="f4bf7-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f4bf7-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f4bf7-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f4bf7-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f4bf7-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f4bf7-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f4bf7-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="f4bf7-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f4bf7-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f4bf7-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f4bf7-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f4bf7-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f4bf7-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f4bf7-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f4bf7-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-chromeriver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f4bf7-185">a.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-185">a.</span></span> <span data-ttu-id="f4bf7-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f4bf7-187">b.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-187">b.</span></span> <span data-ttu-id="f4bf7-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f4bf7-189">c.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-189">c.</span></span> <span data-ttu-id="f4bf7-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f4bf7-191">d.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-191">d.</span></span> <span data-ttu-id="f4bf7-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-192">Click **Create**.</span></span>
 
### <a name="creating-a-chromeriver-test-user"></a><span data-ttu-id="f4bf7-193">Skapa en testanvändare Chromeriver</span><span class="sxs-lookup"><span data-stu-id="f4bf7-193">Creating a Chromeriver test user</span></span>

<span data-ttu-id="f4bf7-194">Om du vill aktivera Azure AD-användare kan logga in på Chromeriver etableras de i Chromeriver.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-194">To enable Azure AD users to log in to Chromeriver, they must be provisioned into Chromeriver.</span></span>  

<span data-ttu-id="f4bf7-195">När det gäller Chromeriver, behöver användarkonton skapas av din [Chromeriver supportteamet](https://www.chromeriver.com/services/support).</span><span class="sxs-lookup"><span data-stu-id="f4bf7-195">In the case of Chromeriver, the user accounts need to be created by your [Chromeriver support team](https://www.chromeriver.com/services/support).</span></span>

>[!NOTE]
><span data-ttu-id="f4bf7-196">Du kan använda något annat Chromeriver användarens konto skapas verktyg eller API: er som tillhandahålls av Chromeriver för att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-196">You can use any other Chromeriver user account creation tools or APIs provided by Chromeriver to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f4bf7-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f4bf7-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f4bf7-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Chromeriver.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Chromeriver.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f4bf7-200">**Om du vill tilldela Chromeriver Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f4bf7-200">**To assign Britta Simon to Chromeriver, perform the following steps:**</span></span>

1. <span data-ttu-id="f4bf7-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f4bf7-203">Välj i listan med program **Chromeriver**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-203">In the applications list, select **Chromeriver**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-chromeriver-tutorial/tutorial_chromeriver_app.png) 

3. <span data-ttu-id="f4bf7-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f4bf7-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-207">Click **Add** button.</span></span> <span data-ttu-id="f4bf7-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f4bf7-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f4bf7-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f4bf7-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f4bf7-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f4bf7-213">Testing single sign-on</span></span>

<span data-ttu-id="f4bf7-214">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-214">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f4bf7-215">När du klickar på panelen Chromeriver på åtkomstpanelen du bör få automatiskt loggat in på ditt Chromeriver program.</span><span class="sxs-lookup"><span data-stu-id="f4bf7-215">When you click the Chromeriver tile in the Access Panel, you should get automatically signed-on to your Chromeriver application.</span></span> <span data-ttu-id="f4bf7-216">Mer information om åtkomstpanelen finns [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f4bf7-216">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4bf7-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f4bf7-217">Additional resources</span></span>

* [<span data-ttu-id="f4bf7-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f4bf7-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f4bf7-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f4bf7-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-chromeriver-tutorial/tutorial_general_203.png

