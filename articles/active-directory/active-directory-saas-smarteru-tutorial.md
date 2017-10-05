---
title: "Självstudier: Azure Active Directory-integrering med SmarterU | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SmarterU."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 129d08c8a7b4228d4d5f1a3b7938ab649b2747a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a><span data-ttu-id="82b4c-103">Självstudier: Azure Active Directory-integrering med SmarterU</span><span class="sxs-lookup"><span data-stu-id="82b4c-103">Tutorial: Azure Active Directory integration with SmarterU</span></span>

<span data-ttu-id="82b4c-104">I kursen får lära du att integrera SmarterU med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="82b4c-104">In this tutorial, you learn how to integrate SmarterU with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="82b4c-105">Integrera SmarterU med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="82b4c-105">Integrating SmarterU with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="82b4c-106">Du kan styra i Azure AD som har åtkomst till SmarterU</span><span class="sxs-lookup"><span data-stu-id="82b4c-106">You can control in Azure AD who has access to SmarterU</span></span>
- <span data-ttu-id="82b4c-107">Du kan aktivera användarna att automatiskt hämta loggat in på SmarterU (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="82b4c-107">You can enable your users to automatically get signed-on to SmarterU (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="82b4c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="82b4c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="82b4c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="82b4c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82b4c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="82b4c-110">Prerequisites</span></span>

<span data-ttu-id="82b4c-111">För att konfigurera Azure AD-integrering med SmarterU, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="82b4c-111">To configure Azure AD integration with SmarterU, you need the following items:</span></span>

- <span data-ttu-id="82b4c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="82b4c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="82b4c-113">En SmarterU enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="82b4c-113">A SmarterU single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="82b4c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="82b4c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="82b4c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="82b4c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="82b4c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="82b4c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="82b4c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82b4c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="82b4c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="82b4c-118">Scenario description</span></span>
<span data-ttu-id="82b4c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="82b4c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="82b4c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="82b4c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="82b4c-121">Att lägga till SmarterU från galleriet</span><span class="sxs-lookup"><span data-stu-id="82b4c-121">Adding SmarterU from the gallery</span></span>
2. <span data-ttu-id="82b4c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82b4c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-smarteru-from-the-gallery"></a><span data-ttu-id="82b4c-123">Att lägga till SmarterU från galleriet</span><span class="sxs-lookup"><span data-stu-id="82b4c-123">Adding SmarterU from the gallery</span></span>
<span data-ttu-id="82b4c-124">Du måste lägga till SmarterU från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SmarterU i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="82b4c-124">To configure the integration of SmarterU into Azure AD, you need to add SmarterU from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="82b4c-125">**Utför följande steg för att lägga till SmarterU från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="82b4c-125">**To add SmarterU from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="82b4c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="82b4c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="82b4c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="82b4c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="82b4c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82b4c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="82b4c-133">I sökrutan skriver **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-133">In the search box, type **SmarterU**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_search.png)

5. <span data-ttu-id="82b4c-135">Välj i resultatpanelen **SmarterU**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="82b4c-135">In the results panel, select **SmarterU**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="82b4c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82b4c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="82b4c-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SmarterU baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="82b4c-138">In this section, you configure and test Azure AD single sign-on with SmarterU based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="82b4c-139">Azure AD måste du känna till användaren i SmarterU motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="82b4c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SmarterU is to a user in Azure AD.</span></span> <span data-ttu-id="82b4c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SmarterU upprättas.</span><span class="sxs-lookup"><span data-stu-id="82b4c-140">In other words, a link relationship between an Azure AD user and the related user in SmarterU needs to be established.</span></span>

<span data-ttu-id="82b4c-141">I SmarterU, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="82b4c-141">In SmarterU, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="82b4c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SmarterU, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="82b4c-142">To configure and test Azure AD single sign-on with SmarterU, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="82b4c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="82b4c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="82b4c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82b4c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="82b4c-145">**[Skapa en testanvändare SmarterU](#creating-a-smarteru-test-user)**  – du har en motsvarighet för Britta Simon i SmarterU som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="82b4c-145">**[Creating a SmarterU test user](#creating-a-smarteru-test-user)** - to have a counterpart of Britta Simon in SmarterU that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="82b4c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="82b4c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="82b4c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="82b4c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="82b4c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82b4c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="82b4c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt SmarterU program.</span><span class="sxs-lookup"><span data-stu-id="82b4c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SmarterU application.</span></span>

<span data-ttu-id="82b4c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SmarterU:**</span><span class="sxs-lookup"><span data-stu-id="82b4c-150">**To configure Azure AD single sign-on with SmarterU, perform the following steps:**</span></span>

1. <span data-ttu-id="82b4c-151">I Azure-portalen på den **SmarterU** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-151">In the Azure portal, on the **SmarterU** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="82b4c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="82b4c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_samlbase.png)

3. <span data-ttu-id="82b4c-155">På den **SmarterU domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="82b4c-155">On the **SmarterU Domain and URLs** section, perform the following steps:</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_url.png)

    <span data-ttu-id="82b4c-157">I den **identifierare** textruta anger du URL:`https://www.smarteru.com/`</span><span class="sxs-lookup"><span data-stu-id="82b4c-157">In the **Identifier** textbox, type the URL: `https://www.smarteru.com/`</span></span>

4. <span data-ttu-id="82b4c-158">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="82b4c-158">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_certificate.png) 

5. <span data-ttu-id="82b4c-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="82b4c-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="82b4c-162">I en annan webbläsarfönster loggar du in på webbplatsen SmarterU företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="82b4c-162">In a different web browser window, log in to your SmarterU company site as an administrator.</span></span>

7. <span data-ttu-id="82b4c-163">Klicka på i verktygsfältet högst upp **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-163">In the toolbar on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="82b4c-164">![Kontoinställningar](./media/active-directory-saas-smarteru-tutorial/IC777326.png "kontoinställningar")</span><span class="sxs-lookup"><span data-stu-id="82b4c-164">![Account Settings](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Account Settings")</span></span>

8. <span data-ttu-id="82b4c-165">Utför följande steg på konfigurationssidan för kontot:</span><span class="sxs-lookup"><span data-stu-id="82b4c-165">On the account configuration page, perform the following steps:</span></span>
   
    <span data-ttu-id="82b4c-166">![Externa auktorisering](./media/active-directory-saas-smarteru-tutorial/IC777327.png "externa auktorisering")</span><span class="sxs-lookup"><span data-stu-id="82b4c-166">![External Authorization](./media/active-directory-saas-smarteru-tutorial/IC777327.png "External Authorization")</span></span> 
 
      <span data-ttu-id="82b4c-167">a.</span><span class="sxs-lookup"><span data-stu-id="82b4c-167">a.</span></span> <span data-ttu-id="82b4c-168">Välj **Aktivera externa auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-168">Select **Enable External Authorization**.</span></span>
  
      <span data-ttu-id="82b4c-169">b.</span><span class="sxs-lookup"><span data-stu-id="82b4c-169">b.</span></span> <span data-ttu-id="82b4c-170">I den **Master inloggningen kontrollen** väljer den **SmarterU** fliken.</span><span class="sxs-lookup"><span data-stu-id="82b4c-170">In the **Master Login Control** section, select the **SmarterU** tab.</span></span>
  
      <span data-ttu-id="82b4c-171">c.</span><span class="sxs-lookup"><span data-stu-id="82b4c-171">c.</span></span> <span data-ttu-id="82b4c-172">I den **standard användarinloggning** väljer den **SmarterU** fliken.</span><span class="sxs-lookup"><span data-stu-id="82b4c-172">In the **User Default Login** section, select the **SmarterU** tab.</span></span>
  
      <span data-ttu-id="82b4c-173">d.</span><span class="sxs-lookup"><span data-stu-id="82b4c-173">d.</span></span> <span data-ttu-id="82b4c-174">Välj **aktivera Okta**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-174">Select **Enable Okta**.</span></span>
  
      <span data-ttu-id="82b4c-175">e.</span><span class="sxs-lookup"><span data-stu-id="82b4c-175">e.</span></span> <span data-ttu-id="82b4c-176">Kopiera innehållet i den hämtade metadatafilen och klistrar in det i den **Okta Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="82b4c-176">Copy the content of the downloaded metadata file, and then paste it into the **Okta Metadata** textbox.</span></span>
  
      <span data-ttu-id="82b4c-177">f.</span><span class="sxs-lookup"><span data-stu-id="82b4c-177">f.</span></span> <span data-ttu-id="82b4c-178">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-178">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="82b4c-179">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="82b4c-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="82b4c-180">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="82b4c-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="82b4c-181">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="82b4c-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="82b4c-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="82b4c-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="82b4c-183">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="82b4c-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="82b4c-185">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="82b4c-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="82b4c-186">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="82b4c-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="82b4c-188">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="82b4c-190">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82b4c-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="82b4c-192">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="82b4c-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-smarteru-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="82b4c-194">a.</span><span class="sxs-lookup"><span data-stu-id="82b4c-194">a.</span></span> <span data-ttu-id="82b4c-195">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="82b4c-196">b.</span><span class="sxs-lookup"><span data-stu-id="82b4c-196">b.</span></span> <span data-ttu-id="82b4c-197">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="82b4c-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="82b4c-198">c.</span><span class="sxs-lookup"><span data-stu-id="82b4c-198">c.</span></span> <span data-ttu-id="82b4c-199">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="82b4c-200">d.</span><span class="sxs-lookup"><span data-stu-id="82b4c-200">d.</span></span> <span data-ttu-id="82b4c-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-201">Click **Create**.</span></span>
 
### <a name="creating-a-smarteru-test-user"></a><span data-ttu-id="82b4c-202">Skapa en testanvändare SmarterU</span><span class="sxs-lookup"><span data-stu-id="82b4c-202">Creating a SmarterU test user</span></span>

<span data-ttu-id="82b4c-203">Om du vill aktivera Azure AD-användare kan logga in på SmarterU etableras de i SmarterU.</span><span class="sxs-lookup"><span data-stu-id="82b4c-203">To enable Azure AD users to log in to SmarterU, they must be provisioned into SmarterU.</span></span>

<span data-ttu-id="82b4c-204">När SmarterU, etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="82b4c-204">When SmarterU, provisioning is a manual task.</span></span>

<span data-ttu-id="82b4c-205">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="82b4c-205">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="82b4c-206">Logga in på ditt **SmarterU** klient.</span><span class="sxs-lookup"><span data-stu-id="82b4c-206">Log in to your **SmarterU** tenant.</span></span>

2. <span data-ttu-id="82b4c-207">Gå till **användare**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-207">Go to **Users**.</span></span>

3. <span data-ttu-id="82b4c-208">Utför följande steg i avsnittet för användare:</span><span class="sxs-lookup"><span data-stu-id="82b4c-208">In the user section, perform the following steps:</span></span>
   
    <span data-ttu-id="82b4c-209">![Ny användare](./media/active-directory-saas-smarteru-tutorial/IC777329.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="82b4c-209">![New User](./media/active-directory-saas-smarteru-tutorial/IC777329.png "New User")</span></span>  

    <span data-ttu-id="82b4c-210">a.</span><span class="sxs-lookup"><span data-stu-id="82b4c-210">a.</span></span> <span data-ttu-id="82b4c-211">Klicka på **+ användaren**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-211">Click **+User**.</span></span>
    
    <span data-ttu-id="82b4c-212">b.</span><span class="sxs-lookup"><span data-stu-id="82b4c-212">b.</span></span> <span data-ttu-id="82b4c-213">Ange relaterade attributvärden som Azure AD-användarkontot i följande textrutor: **primära e-post**, **anställnings-ID**, **lösenord**, **Kontrollera Lösenordet**, **Förnamn**, **efternamn**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-213">Type the related attribute values of the Azure AD user account into the following textboxes: **Primary Email**, **Employee ID**, **Password**, **Verify Password**, **Given Name**, **Surname**.</span></span>
    
    <span data-ttu-id="82b4c-214">c.</span><span class="sxs-lookup"><span data-stu-id="82b4c-214">c.</span></span> <span data-ttu-id="82b4c-215">Klicka på **Active**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-215">Click **Active**.</span></span> 
    
    <span data-ttu-id="82b4c-216">d.</span><span class="sxs-lookup"><span data-stu-id="82b4c-216">d.</span></span> <span data-ttu-id="82b4c-217">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-217">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="82b4c-218">Du kan använda något annat SmarterU användarens konto skapas verktyg eller API: er som tillhandahålls av SmarterU att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="82b4c-218">You can use any other SmarterU user account creation tools or APIs provided by SmarterU to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="82b4c-219">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="82b4c-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="82b4c-220">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SmarterU.</span><span class="sxs-lookup"><span data-stu-id="82b4c-220">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SmarterU.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="82b4c-222">**Om du vill tilldela SmarterU Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="82b4c-222">**To assign Britta Simon to SmarterU, perform the following steps:**</span></span>

1. <span data-ttu-id="82b4c-223">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-223">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="82b4c-225">Välj i listan med program **SmarterU**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-225">In the applications list, select **SmarterU**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-smarteru-tutorial/tutorial_smarteru_app.png) 

3. <span data-ttu-id="82b4c-227">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="82b4c-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="82b4c-229">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="82b4c-229">Click **Add** button.</span></span> <span data-ttu-id="82b4c-230">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82b4c-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="82b4c-232">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="82b4c-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="82b4c-233">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82b4c-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="82b4c-234">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="82b4c-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="82b4c-235">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="82b4c-235">Testing single sign-on</span></span>

<span data-ttu-id="82b4c-236">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="82b4c-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="82b4c-237">När du klickar på panelen SmarterU på åtkomstpanelen du bör få automatiskt loggat in på ditt SmarterU program.</span><span class="sxs-lookup"><span data-stu-id="82b4c-237">When you click the SmarterU tile in the Access Panel, you should get automatically signed-on to your SmarterU application.</span></span>
<span data-ttu-id="82b4c-238">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="82b4c-238">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 


## <a name="additional-resources"></a><span data-ttu-id="82b4c-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="82b4c-239">Additional resources</span></span>

* [<span data-ttu-id="82b4c-240">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="82b4c-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="82b4c-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="82b4c-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-smarteru-tutorial/tutorial_general_203.png

