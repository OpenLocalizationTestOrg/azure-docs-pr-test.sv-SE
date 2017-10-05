---
title: "Självstudier: Azure Active Directory-integrering med Alcumus Info Exchange | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Alcumus Info Exchange."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d26034b8-f0d5-4f65-aa56-0fc168ceec8c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: jeedes
ms.openlocfilehash: 1f67682111de0bea1b18fd97d739492661ebbfd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a><span data-ttu-id="98d9f-103">Självstudier: Azure Active Directory-integrering med Alcumus Info Exchange</span><span class="sxs-lookup"><span data-stu-id="98d9f-103">Tutorial: Azure Active Directory integration with Alcumus Info Exchange</span></span>

<span data-ttu-id="98d9f-104">I kursen får lära du att integrera Alcumus Info Exchange med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="98d9f-104">In this tutorial, you learn how to integrate Alcumus Info Exchange with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="98d9f-105">Integrera Alcumus Info Exchange med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="98d9f-105">Integrating Alcumus Info Exchange with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="98d9f-106">Du kan styra i Azure AD som har åtkomst till Alcumus Info Exchange</span><span class="sxs-lookup"><span data-stu-id="98d9f-106">You can control in Azure AD who has access to Alcumus Info Exchange</span></span>
- <span data-ttu-id="98d9f-107">Du kan aktivera användarna att automatiskt hämta loggat in på Alcumus Info Exchange (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="98d9f-107">You can enable your users to automatically get signed-on to Alcumus Info Exchange (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="98d9f-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="98d9f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="98d9f-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="98d9f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98d9f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="98d9f-110">Prerequisites</span></span>

<span data-ttu-id="98d9f-111">För att konfigurera Azure AD-integrering med Alcumus Info Exchange, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="98d9f-111">To configure Azure AD integration with Alcumus Info Exchange, you need the following items:</span></span>

- <span data-ttu-id="98d9f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="98d9f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="98d9f-113">En Alcumus Info Exchange enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="98d9f-113">An Alcumus Info Exchange single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="98d9f-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="98d9f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="98d9f-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="98d9f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="98d9f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="98d9f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="98d9f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="98d9f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="98d9f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="98d9f-118">Scenario description</span></span>
<span data-ttu-id="98d9f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="98d9f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="98d9f-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="98d9f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="98d9f-121">Att lägga till Alcumus Info Exchange från galleriet</span><span class="sxs-lookup"><span data-stu-id="98d9f-121">Adding Alcumus Info Exchange from the gallery</span></span>
2. <span data-ttu-id="98d9f-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98d9f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-alcumus-info-exchange-from-the-gallery"></a><span data-ttu-id="98d9f-123">Att lägga till Alcumus Info Exchange från galleriet</span><span class="sxs-lookup"><span data-stu-id="98d9f-123">Adding Alcumus Info Exchange from the gallery</span></span>
<span data-ttu-id="98d9f-124">Du måste lägga till Alcumus Info Exchange från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Alcumus Info Exchange i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="98d9f-124">To configure the integration of Alcumus Info Exchange into Azure AD, you need to add Alcumus Info Exchange from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="98d9f-125">**Utför följande steg för att lägga till Alcumus Info Exchange från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="98d9f-125">**To add Alcumus Info Exchange from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="98d9f-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="98d9f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="98d9f-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="98d9f-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="98d9f-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98d9f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="98d9f-133">I sökrutan skriver **Alcumus Info Exchange**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-133">In the search box, type **Alcumus Info Exchange**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_search.png)

5. <span data-ttu-id="98d9f-135">Välj i resultatpanelen **Alcumus Info Exchange**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="98d9f-135">In the results panel, select **Alcumus Info Exchange**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="98d9f-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98d9f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="98d9f-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Alcumus Info Exchange baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="98d9f-138">In this section, you configure and test Azure AD single sign-on with Alcumus Info Exchange based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="98d9f-139">Azure AD måste du känna till motsvarande användaren i Alcumus Info Exchange till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="98d9f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Alcumus Info Exchange is to a user in Azure AD.</span></span> <span data-ttu-id="98d9f-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Alcumus Info Exchange upprättas.</span><span class="sxs-lookup"><span data-stu-id="98d9f-140">In other words, a link relationship between an Azure AD user and the related user in Alcumus Info Exchange needs to be established.</span></span>

<span data-ttu-id="98d9f-141">I Alcumus Info Exchange, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="98d9f-141">In Alcumus Info Exchange, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="98d9f-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Alcumus Info Exchange måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="98d9f-142">To configure and test Azure AD single sign-on with Alcumus Info Exchange, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="98d9f-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="98d9f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="98d9f-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98d9f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="98d9f-145">**[Skapa en testanvändare Alcumus Info Exchange](#creating-an-alcumus-info-exchange-test-user)**  – har en motsvarighet för Britta Simon Alcumus Info Exchange som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="98d9f-145">**[Creating an Alcumus Info Exchange test user](#creating-an-alcumus-info-exchange-test-user)** - to have a counterpart of Britta Simon in Alcumus Info Exchange that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="98d9f-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="98d9f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="98d9f-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="98d9f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="98d9f-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98d9f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="98d9f-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Alcumus Info Exchange-program.</span><span class="sxs-lookup"><span data-stu-id="98d9f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Alcumus Info Exchange application.</span></span>

<span data-ttu-id="98d9f-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Alcumus Info Exchange:**</span><span class="sxs-lookup"><span data-stu-id="98d9f-150">**To configure Azure AD single sign-on with Alcumus Info Exchange, perform the following steps:**</span></span>

1. <span data-ttu-id="98d9f-151">I Azure-portalen på den **Alcumus Info Exchange** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-151">In the Azure portal, on the **Alcumus Info Exchange** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="98d9f-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="98d9f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_samlbase.png)

3. <span data-ttu-id="98d9f-155">På den **Alcumus information Exchange-domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="98d9f-155">On the **Alcumus Info Exchange Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_url.png)

    <span data-ttu-id="98d9f-157">a.</span><span class="sxs-lookup"><span data-stu-id="98d9f-157">a.</span></span> <span data-ttu-id="98d9f-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.info-exchange.com`</span><span class="sxs-lookup"><span data-stu-id="98d9f-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.info-exchange.com`</span></span>

    <span data-ttu-id="98d9f-159">b.</span><span class="sxs-lookup"><span data-stu-id="98d9f-159">b.</span></span> <span data-ttu-id="98d9f-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.info-exchange.com/Auth/`</span><span class="sxs-lookup"><span data-stu-id="98d9f-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.info-exchange.com/Auth/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="98d9f-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="98d9f-161">These values are not real.</span></span> <span data-ttu-id="98d9f-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="98d9f-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="98d9f-163">Kontakta [Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="98d9f-163">Contact [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com) to get these values.</span></span>
 
4. <span data-ttu-id="98d9f-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="98d9f-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_certificate.png) 

5. <span data-ttu-id="98d9f-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="98d9f-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="98d9f-168">Konfigurera enkel inloggning på **Alcumus Info Exchange** sida, måste du skicka den hämtade **XML-Metadata för** till [Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="98d9f-168">To configure single sign-on on **Alcumus Info Exchange** side, you need to send the downloaded **Metadata XML** to [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

> [!TIP]
> <span data-ttu-id="98d9f-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="98d9f-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="98d9f-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="98d9f-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="98d9f-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="98d9f-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="98d9f-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="98d9f-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="98d9f-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="98d9f-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="98d9f-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="98d9f-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="98d9f-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="98d9f-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="98d9f-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="98d9f-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98d9f-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="98d9f-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="98d9f-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="98d9f-184">a.</span><span class="sxs-lookup"><span data-stu-id="98d9f-184">a.</span></span> <span data-ttu-id="98d9f-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="98d9f-186">b.</span><span class="sxs-lookup"><span data-stu-id="98d9f-186">b.</span></span> <span data-ttu-id="98d9f-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="98d9f-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="98d9f-188">c.</span><span class="sxs-lookup"><span data-stu-id="98d9f-188">c.</span></span> <span data-ttu-id="98d9f-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="98d9f-190">d.</span><span class="sxs-lookup"><span data-stu-id="98d9f-190">d.</span></span> <span data-ttu-id="98d9f-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-191">Click **Create**.</span></span>
 
### <a name="creating-an-alcumus-info-exchange-test-user"></a><span data-ttu-id="98d9f-192">Skapa en testanvändare Alcumus Info Exchange</span><span class="sxs-lookup"><span data-stu-id="98d9f-192">Creating an Alcumus Info Exchange test user</span></span>

<span data-ttu-id="98d9f-193">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Alcumus Info Exchange.</span><span class="sxs-lookup"><span data-stu-id="98d9f-193">The objective of this section is to create a user called Britta Simon in Alcumus Info Exchange.</span></span>

<span data-ttu-id="98d9f-194">Om du vill skapa en användare som kallas Britta Simon i Alcumus Info Exchange kontakta den [Alcumus Info Exchange supportteamet](mailto:helpdesk@alcumusgroup.com).</span><span class="sxs-lookup"><span data-stu-id="98d9f-194">To create a user called Britta Simon in Alcumus Info Exchange, Contact the [Alcumus Info Exchange support team](mailto:helpdesk@alcumusgroup.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="98d9f-195">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="98d9f-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="98d9f-196">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Alcumus Info Exchange.</span><span class="sxs-lookup"><span data-stu-id="98d9f-196">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Alcumus Info Exchange.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="98d9f-198">**Om du vill tilldela Alcumus Info Exchange Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="98d9f-198">**To assign Britta Simon to Alcumus Info Exchange, perform the following steps:**</span></span>

1. <span data-ttu-id="98d9f-199">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-199">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="98d9f-201">Välj i listan med program **Alcumus Info Exchange**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-201">In the applications list, select **Alcumus Info Exchange**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumusinfoexchange_app.png) 

3. <span data-ttu-id="98d9f-203">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="98d9f-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="98d9f-205">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="98d9f-205">Click **Add** button.</span></span> <span data-ttu-id="98d9f-206">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98d9f-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="98d9f-208">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="98d9f-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="98d9f-209">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98d9f-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="98d9f-210">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="98d9f-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="98d9f-211">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="98d9f-211">Testing single sign-on</span></span>

<span data-ttu-id="98d9f-212">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="98d9f-212">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="98d9f-213">När du klickar på panelen Alcumus Info Exchange på åtkomstpanelen du bör få automatiskt loggat in på ditt Alcumus Info Exchange-program.</span><span class="sxs-lookup"><span data-stu-id="98d9f-213">When you click the Alcumus Info Exchange tile in the Access Panel, you should get automatically signed-on to your Alcumus Info Exchange application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="98d9f-214">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="98d9f-214">Additional resources</span></span>

* [<span data-ttu-id="98d9f-215">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="98d9f-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="98d9f-216">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="98d9f-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png

