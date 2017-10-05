---
title: "Självstudier: Azure Active Directory-integrering med Convercent | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Convercent."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 7af33cae7448a212734d327570b5082d0278fa0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a><span data-ttu-id="f83a0-103">Självstudier: Azure Active Directory-integrering med Convercent</span><span class="sxs-lookup"><span data-stu-id="f83a0-103">Tutorial: Azure Active Directory integration with Convercent</span></span>

<span data-ttu-id="f83a0-104">I kursen får lära du att integrera Convercent med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f83a0-104">In this tutorial, you learn how to integrate Convercent with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f83a0-105">Integrera Convercent med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f83a0-105">Integrating Convercent with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f83a0-106">Du kan styra i Azure AD som har åtkomst till Convercent</span><span class="sxs-lookup"><span data-stu-id="f83a0-106">You can control in Azure AD who has access to Convercent</span></span>
- <span data-ttu-id="f83a0-107">Du kan aktivera användarna att automatiskt hämta loggat in på Convercent (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f83a0-107">You can enable your users to automatically get signed-on to Convercent (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f83a0-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f83a0-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f83a0-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f83a0-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f83a0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f83a0-110">Prerequisites</span></span>

<span data-ttu-id="f83a0-111">För att konfigurera Azure AD-integrering med Convercent, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f83a0-111">To configure Azure AD integration with Convercent, you need the following items:</span></span>

- <span data-ttu-id="f83a0-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f83a0-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f83a0-113">En Convercent enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f83a0-113">A Convercent single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f83a0-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f83a0-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f83a0-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f83a0-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f83a0-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f83a0-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f83a0-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f83a0-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f83a0-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f83a0-118">Scenario description</span></span>
<span data-ttu-id="f83a0-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f83a0-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f83a0-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f83a0-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f83a0-121">Att lägga till Convercent från galleriet</span><span class="sxs-lookup"><span data-stu-id="f83a0-121">Adding Convercent from the gallery</span></span>
2. <span data-ttu-id="f83a0-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f83a0-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-convercent-from-the-gallery"></a><span data-ttu-id="f83a0-123">Att lägga till Convercent från galleriet</span><span class="sxs-lookup"><span data-stu-id="f83a0-123">Adding Convercent from the gallery</span></span>
<span data-ttu-id="f83a0-124">Du måste lägga till Convercent från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Convercent i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f83a0-124">To configure the integration of Convercent into Azure AD, you need to add Convercent from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f83a0-125">**Utför följande steg för att lägga till Convercent från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f83a0-125">**To add Convercent from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f83a0-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f83a0-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f83a0-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f83a0-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f83a0-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f83a0-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f83a0-133">I sökrutan skriver **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-133">In the search box, type **Convercent**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_search.png)

5. <span data-ttu-id="f83a0-135">Välj i resultatpanelen **Convercent**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f83a0-135">In the results panel, select **Convercent**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f83a0-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f83a0-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f83a0-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Convercent baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f83a0-138">In this section, you configure and test Azure AD single sign-on with Convercent based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f83a0-139">Azure AD måste du känna till användaren i Convercent motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f83a0-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Convercent is to a user in Azure AD.</span></span> <span data-ttu-id="f83a0-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Convercent upprättas.</span><span class="sxs-lookup"><span data-stu-id="f83a0-140">In other words, a link relationship between an Azure AD user and the related user in Convercent needs to be established.</span></span>

<span data-ttu-id="f83a0-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Convercent.</span><span class="sxs-lookup"><span data-stu-id="f83a0-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Convercent.</span></span>

<span data-ttu-id="f83a0-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Convercent, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f83a0-142">To configure and test Azure AD single sign-on with Convercent, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f83a0-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f83a0-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f83a0-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f83a0-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f83a0-145">**[Skapa en testanvändare Convercent](#creating-a-convercent-test-user)**  – du har en motsvarighet för Britta Simon i Convercent som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f83a0-145">**[Creating a Convercent test user](#creating-a-convercent-test-user)** - to have a counterpart of Britta Simon in Convercent that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f83a0-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f83a0-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f83a0-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f83a0-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f83a0-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f83a0-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f83a0-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Convercent program.</span><span class="sxs-lookup"><span data-stu-id="f83a0-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Convercent application.</span></span>

<span data-ttu-id="f83a0-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Convercent:**</span><span class="sxs-lookup"><span data-stu-id="f83a0-150">**To configure Azure AD single sign-on with Convercent, perform the following steps:**</span></span>

1. <span data-ttu-id="f83a0-151">I Azure-portalen på den **Convercent** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-151">In the Azure portal, on the **Convercent** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f83a0-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f83a0-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_samlbase.png)

3. <span data-ttu-id="f83a0-155">På den **Convercent domän och URL: er** om du vill konfigurera programmet i **IDP initierade läge**, utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="f83a0-155">On the **Convercent Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url.png)

    <span data-ttu-id="f83a0-157">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="f83a0-157">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.convercent.com/`</span></span>
 
4. <span data-ttu-id="f83a0-158">Om du vill konfigurera programmet i **SP initierade läge**på den **Convercent domän och URL: er** avsnittet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f83a0-158">If you wish to configure the application in **SP initiated mode**, on the **Convercent Domain and URLs** section perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_url1.png)

     <span data-ttu-id="f83a0-160">a.</span><span class="sxs-lookup"><span data-stu-id="f83a0-160">a.</span></span> <span data-ttu-id="f83a0-161">Klicka på **”visa avancerade inställningar för URL”.**</span><span class="sxs-lookup"><span data-stu-id="f83a0-161">Click **"Show advanced URL settings."**</span></span> 

     <span data-ttu-id="f83a0-162">b.</span><span class="sxs-lookup"><span data-stu-id="f83a0-162">b.</span></span> <span data-ttu-id="f83a0-163">I den **logga URL** textruta Skriv det värde som använder följande mönster:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="f83a0-163">In the **Sign On URL** textbox, type the value using the following pattern: `https://<instancename>.convercent.com/`</span></span>

     <span data-ttu-id="f83a0-164">c.</span><span class="sxs-lookup"><span data-stu-id="f83a0-164">c.</span></span> <span data-ttu-id="f83a0-165">I den **Relay tillstånd** textruta Skriv det värde som använder följande mönster:`https://<instancename>.convercent.com/`</span><span class="sxs-lookup"><span data-stu-id="f83a0-165">In the **Relay State** textbox, type the value using the following pattern: `https://<instancename>.convercent.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f83a0-166">Dessa värden är inte de verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="f83a0-166">These values are not the real values.</span></span> <span data-ttu-id="f83a0-167">Uppdatera dessa värden med faktiska identifierare, logga URL och Relay tillstånd.</span><span class="sxs-lookup"><span data-stu-id="f83a0-167">Update these values with the actual Identifier, Sign On URL and Relay State.</span></span> <span data-ttu-id="f83a0-168">Kontakta [Convercent klienten supportteamet](http://support.convercent.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f83a0-168">Contact [Convercent Client support team](http://support.convercent.com) to get these values.</span></span>

5. <span data-ttu-id="f83a0-169">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f83a0-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_certificate.png) 

6. <span data-ttu-id="f83a0-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f83a0-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="f83a0-173">För att få SSO konfigurerats för ditt program, kontakta [Convercent supportteamet](mailto:support@convercent.com) och ger dem den hämtade **XML-Metadata för**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-173">To get SSO configured for your application, contact [Convercent support team](mailto:support@convercent.com) and provide them with the downloaded **Metadata XML**.</span></span>

> [!TIP]
> <span data-ttu-id="f83a0-174">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f83a0-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f83a0-175">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f83a0-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f83a0-176">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f83a0-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f83a0-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f83a0-177">Creating an Azure AD test user</span></span>
<span data-ttu-id="f83a0-178">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f83a0-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f83a0-180">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f83a0-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f83a0-181">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f83a0-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f83a0-183">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f83a0-185">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f83a0-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f83a0-187">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f83a0-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-convercent-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f83a0-189">a.</span><span class="sxs-lookup"><span data-stu-id="f83a0-189">a.</span></span> <span data-ttu-id="f83a0-190">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f83a0-191">b.</span><span class="sxs-lookup"><span data-stu-id="f83a0-191">b.</span></span> <span data-ttu-id="f83a0-192">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f83a0-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f83a0-193">c.</span><span class="sxs-lookup"><span data-stu-id="f83a0-193">c.</span></span> <span data-ttu-id="f83a0-194">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f83a0-195">d.</span><span class="sxs-lookup"><span data-stu-id="f83a0-195">d.</span></span> <span data-ttu-id="f83a0-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-196">Click **Create**.</span></span>
 
### <a name="creating-a-convercent-test-user"></a><span data-ttu-id="f83a0-197">Skapa en testanvändare Convercent</span><span class="sxs-lookup"><span data-stu-id="f83a0-197">Creating a Convercent test user</span></span>

<span data-ttu-id="f83a0-198">Arbeta med [Convercent supportteamet](mailto:support@convercent.com) att lägga till användare i Convercent-plattformen.</span><span class="sxs-lookup"><span data-stu-id="f83a0-198">Work with [Convercent support team](mailto:support@convercent.com) to add the users in the Convercent platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f83a0-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f83a0-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f83a0-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Convercent.</span><span class="sxs-lookup"><span data-stu-id="f83a0-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Convercent.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f83a0-202">**Om du vill tilldela Convercent Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f83a0-202">**To assign Britta Simon to Convercent, perform the following steps:**</span></span>

1. <span data-ttu-id="f83a0-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f83a0-205">Välj i listan med program **Convercent**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-205">In the applications list, select **Convercent**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_app.png) 

3. <span data-ttu-id="f83a0-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f83a0-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f83a0-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f83a0-209">Click **Add** button.</span></span> <span data-ttu-id="f83a0-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f83a0-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f83a0-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f83a0-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f83a0-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f83a0-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f83a0-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f83a0-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f83a0-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f83a0-215">Testing single sign-on</span></span>

<span data-ttu-id="f83a0-216">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f83a0-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f83a0-217">När du klickar på panelen Convercent på åtkomstpanelen du bör få automatiskt loggat in på ditt Convercent program.</span><span class="sxs-lookup"><span data-stu-id="f83a0-217">When you click the Convercent tile in the Access Panel, you should get automatically signed-on to your Convercent application.</span></span>
<span data-ttu-id="f83a0-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f83a0-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="f83a0-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f83a0-219">Additional resources</span></span>

* [<span data-ttu-id="f83a0-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f83a0-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f83a0-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f83a0-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_203.png

