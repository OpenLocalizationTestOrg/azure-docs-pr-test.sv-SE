---
title: "Självstudier: Azure Active Directory-integrering med Expensify | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Expensify."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: jeedes
ms.openlocfilehash: e45576fd92706881121469ccd82150b3d48059cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a><span data-ttu-id="595cb-103">Självstudier: Azure Active Directory-integrering med Expensify</span><span class="sxs-lookup"><span data-stu-id="595cb-103">Tutorial: Azure Active Directory integration with Expensify</span></span>

<span data-ttu-id="595cb-104">I kursen får lära du att integrera Expensify med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="595cb-104">In this tutorial, you learn how to integrate Expensify with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="595cb-105">Integrera Expensify med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="595cb-105">Integrating Expensify with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="595cb-106">Du kan styra i Azure AD som har åtkomst till Expensify</span><span class="sxs-lookup"><span data-stu-id="595cb-106">You can control in Azure AD who has access to Expensify</span></span>
- <span data-ttu-id="595cb-107">Du kan aktivera användarna att automatiskt hämta loggat in på Expensify (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="595cb-107">You can enable your users to automatically get signed-on to Expensify (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="595cb-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="595cb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="595cb-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="595cb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="595cb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="595cb-110">Prerequisites</span></span>

<span data-ttu-id="595cb-111">För att konfigurera Azure AD-integrering med Expensify, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="595cb-111">To configure Azure AD integration with Expensify, you need the following items:</span></span>

- <span data-ttu-id="595cb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="595cb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="595cb-113">En Expensify enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="595cb-113">An Expensify single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="595cb-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="595cb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="595cb-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="595cb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="595cb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="595cb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="595cb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="595cb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="595cb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="595cb-118">Scenario description</span></span>
<span data-ttu-id="595cb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="595cb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="595cb-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="595cb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="595cb-121">Att lägga till Expensify från galleriet</span><span class="sxs-lookup"><span data-stu-id="595cb-121">Adding Expensify from the gallery</span></span>
2. <span data-ttu-id="595cb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="595cb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-expensify-from-the-gallery"></a><span data-ttu-id="595cb-123">Att lägga till Expensify från galleriet</span><span class="sxs-lookup"><span data-stu-id="595cb-123">Adding Expensify from the gallery</span></span>
<span data-ttu-id="595cb-124">Du måste lägga till Expensify från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Expensify i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="595cb-124">To configure the integration of Expensify into Azure AD, you need to add Expensify from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="595cb-125">**Utför följande steg för att lägga till Expensify från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="595cb-125">**To add Expensify from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="595cb-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="595cb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="595cb-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="595cb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="595cb-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="595cb-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="595cb-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="595cb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="595cb-133">I sökrutan skriver **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="595cb-133">In the search box, type **Expensify**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_search.png)

5. <span data-ttu-id="595cb-135">Välj i resultatpanelen **Expensify**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="595cb-135">In the results panel, select **Expensify**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="595cb-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="595cb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="595cb-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Expensify baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="595cb-138">In this section, you configure and test Azure AD single sign-on with Expensify based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="595cb-139">Azure AD måste du känna till användaren i Expensify motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="595cb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Expensify is to a user in Azure AD.</span></span> <span data-ttu-id="595cb-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Expensify upprättas.</span><span class="sxs-lookup"><span data-stu-id="595cb-140">In other words, a link relationship between an Azure AD user and the related user in Expensify needs to be established.</span></span>

<span data-ttu-id="595cb-141">I Expensify, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="595cb-141">In Expensify, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="595cb-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Expensify, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="595cb-142">To configure and test Azure AD single sign-on with Expensify, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="595cb-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="595cb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="595cb-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="595cb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="595cb-145">**[Skapa en testanvändare Expensify](#creating-an-expensify-test-user)**  – du har en motsvarighet för Britta Simon i Expensify som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="595cb-145">**[Creating an Expensify test user](#creating-an-expensify-test-user)** - to have a counterpart of Britta Simon in Expensify that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="595cb-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="595cb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="595cb-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="595cb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="595cb-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="595cb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="595cb-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Expensify program.</span><span class="sxs-lookup"><span data-stu-id="595cb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Expensify application.</span></span>

<span data-ttu-id="595cb-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Expensify:**</span><span class="sxs-lookup"><span data-stu-id="595cb-150">**To configure Azure AD single sign-on with Expensify, perform the following steps:**</span></span>

1. <span data-ttu-id="595cb-151">I Azure-portalen på den **Expensify** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="595cb-151">In the Azure portal, on the **Expensify** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="595cb-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="595cb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_samlbase.png)

3. <span data-ttu-id="595cb-155">På den **Expensify domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="595cb-155">On the **Expensify Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_url.png)

    <span data-ttu-id="595cb-157">a.</span><span class="sxs-lookup"><span data-stu-id="595cb-157">a.</span></span> <span data-ttu-id="595cb-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://www.expensify.com/authentication/saml/login`</span><span class="sxs-lookup"><span data-stu-id="595cb-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://www.expensify.com/authentication/saml/login`</span></span>

    <span data-ttu-id="595cb-159">b.</span><span class="sxs-lookup"><span data-stu-id="595cb-159">b.</span></span> <span data-ttu-id="595cb-160">I den **Identifierare** textruta Skriv en URL med följande mönster:`https://www.<companyname>.expensify.com/`</span><span class="sxs-lookup"><span data-stu-id="595cb-160">In the **Identifier URL** textbox, type a URL using the following pattern: `https://www.<companyname>.expensify.com/`</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="595cb-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="595cb-161">These values are not real.</span></span> <span data-ttu-id="595cb-162">Uppdatera dessa värden med den faktiska inloggnings-URL och Identifierare.</span><span class="sxs-lookup"><span data-stu-id="595cb-162">Update these values with the actual Sign-On URL and Identifier URL.</span></span> <span data-ttu-id="595cb-163">Kontakta [Expensify klienten supportteamet](mailto:help@expensify.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="595cb-163">Contact [Expensify Client support team](mailto:help@expensify.com) to get these values.</span></span> 
 
4. <span data-ttu-id="595cb-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="595cb-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_certificate.png) 

5. <span data-ttu-id="595cb-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="595cb-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="595cb-168">Om du vill aktivera enkel inloggning i Expensify, måste du först aktivera **domänens kontroll** i programmet.</span><span class="sxs-lookup"><span data-stu-id="595cb-168">To enable SSO in Expensify, you first need to enable **Domain Control** in the application.</span></span> <span data-ttu-id="595cb-169">Du kan aktivera domänens kontroll i programmet genom stegen [här](http://help.expensify.com/domain-control).</span><span class="sxs-lookup"><span data-stu-id="595cb-169">You can enable Domain Control in the application through the steps listed [here](http://help.expensify.com/domain-control).</span></span> <span data-ttu-id="595cb-170">Arbeta med ytterligare support [Expensify klienten supportteamet](mailto:help@expensify.com).</span><span class="sxs-lookup"><span data-stu-id="595cb-170">For additional support, work with [Expensify Client support team](mailto:help@expensify.com).</span></span> <span data-ttu-id="595cb-171">När du har domän kontroll är aktiverat, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="595cb-171">Once you have Domain Control enabled, follow these steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png)
    
    <span data-ttu-id="595cb-173">a.</span><span class="sxs-lookup"><span data-stu-id="595cb-173">a.</span></span> <span data-ttu-id="595cb-174">Logga in i tillämpningsprogrammet Expensify.</span><span class="sxs-lookup"><span data-stu-id="595cb-174">Sign on to your Expensify application.</span></span>
    
    <span data-ttu-id="595cb-175">b.</span><span class="sxs-lookup"><span data-stu-id="595cb-175">b.</span></span> <span data-ttu-id="595cb-176">Klicka på i verktygsfältet högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="595cb-176">In the toolbar on the top, click **Admin**.</span></span>
    
    <span data-ttu-id="595cb-177">c.</span><span class="sxs-lookup"><span data-stu-id="595cb-177">c.</span></span> <span data-ttu-id="595cb-178">I den vänstra rutan klickar du på **domän**.</span><span class="sxs-lookup"><span data-stu-id="595cb-178">In the left panel, click **Domain**.</span></span>
    
    <span data-ttu-id="595cb-179">d.</span><span class="sxs-lookup"><span data-stu-id="595cb-179">d.</span></span> <span data-ttu-id="595cb-180">Klicka på ett verifierat domännamn.</span><span class="sxs-lookup"><span data-stu-id="595cb-180">Click your verified domain name.</span></span>
    
    <span data-ttu-id="595cb-181">e.</span><span class="sxs-lookup"><span data-stu-id="595cb-181">e.</span></span> <span data-ttu-id="595cb-182">I den vänstra rutan klickar du på **SAML**, och välj sedan **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="595cb-182">In the left panel, click **SAML**, and then select **Enabled**.</span></span>
    
    <span data-ttu-id="595cb-183">f.</span><span class="sxs-lookup"><span data-stu-id="595cb-183">f.</span></span> <span data-ttu-id="595cb-184">Öppna hämtade Federationsmetadata från Azure AD i anteckningar, kopiera innehållet och klistrar in det i den **identitet providern Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="595cb-184">Open the downloaded Federation Metadata from Azure AD in notepad, copy the content, and then paste it into the **Identity Provider Metadata** textbox.</span></span>

> [!TIP]
> <span data-ttu-id="595cb-185">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="595cb-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="595cb-186">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="595cb-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="595cb-187">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="595cb-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="595cb-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="595cb-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="595cb-189">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="595cb-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="595cb-191">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="595cb-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="595cb-192">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="595cb-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="595cb-194">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="595cb-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="595cb-196">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="595cb-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="595cb-198">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="595cb-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="595cb-200">a.</span><span class="sxs-lookup"><span data-stu-id="595cb-200">a.</span></span> <span data-ttu-id="595cb-201">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="595cb-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="595cb-202">b.</span><span class="sxs-lookup"><span data-stu-id="595cb-202">b.</span></span> <span data-ttu-id="595cb-203">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="595cb-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="595cb-204">c.</span><span class="sxs-lookup"><span data-stu-id="595cb-204">c.</span></span> <span data-ttu-id="595cb-205">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="595cb-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="595cb-206">d.</span><span class="sxs-lookup"><span data-stu-id="595cb-206">d.</span></span> <span data-ttu-id="595cb-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="595cb-207">Click **Create**.</span></span>
 
### <a name="creating-an-expensify-test-user"></a><span data-ttu-id="595cb-208">Skapa en testanvändare Expensify</span><span class="sxs-lookup"><span data-stu-id="595cb-208">Creating an Expensify test user</span></span>

<span data-ttu-id="595cb-209">I det här avsnittet skapar du en användare som kallas Britta Simon i Expensify.</span><span class="sxs-lookup"><span data-stu-id="595cb-209">In this section, you create a user called Britta Simon in Expensify.</span></span> <span data-ttu-id="595cb-210">Arbeta med [Expensify klienten supportteamet](mailto:help@expensify.com) att lägga till användare i Expensify-plattformen.</span><span class="sxs-lookup"><span data-stu-id="595cb-210">Work with [Expensify Client support team](mailto:help@expensify.com) to add the users in the Expensify platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="595cb-211">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="595cb-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="595cb-212">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Expensify.</span><span class="sxs-lookup"><span data-stu-id="595cb-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Expensify.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="595cb-214">**Om du vill tilldela Expensify Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="595cb-214">**To assign Britta Simon to Expensify, perform the following steps:**</span></span>

1. <span data-ttu-id="595cb-215">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="595cb-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="595cb-217">Välj i listan med program **Expensify**.</span><span class="sxs-lookup"><span data-stu-id="595cb-217">In the applications list, select **Expensify**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_app.png) 

3. <span data-ttu-id="595cb-219">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="595cb-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="595cb-221">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="595cb-221">Click **Add** button.</span></span> <span data-ttu-id="595cb-222">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="595cb-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="595cb-224">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="595cb-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="595cb-225">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="595cb-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="595cb-226">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="595cb-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="595cb-227">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="595cb-227">Testing single sign-on</span></span>

<span data-ttu-id="595cb-228">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="595cb-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>  

<span data-ttu-id="595cb-229">När du klickar på panelen Expensify på åtkomstpanelen du bör få automatiskt loggat in på ditt Expensify program.</span><span class="sxs-lookup"><span data-stu-id="595cb-229">When you click the Expensify tile in the Access Panel, you should get automatically signed-on to your Expensify application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="595cb-230">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="595cb-230">Additional resources</span></span>

* [<span data-ttu-id="595cb-231">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="595cb-231">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="595cb-232">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="595cb-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png

