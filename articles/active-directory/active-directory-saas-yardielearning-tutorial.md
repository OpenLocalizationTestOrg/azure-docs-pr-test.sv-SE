---
title: "Självstudier: Azure Active Directory-integrering med Yardi e | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Yardi e."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7ea58b54-ec5b-4576-8586-814b11d0f4fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: e7b36b692ad2a8bc3a3f5203d93882af96fd2109
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-yardi-elearning"></a><span data-ttu-id="8f121-103">Självstudier: Azure Active Directory-integrering med Yardi e</span><span class="sxs-lookup"><span data-stu-id="8f121-103">Tutorial: Azure Active Directory integration with Yardi eLearning</span></span>

<span data-ttu-id="8f121-104">I kursen får lära du att integrera Yardi e med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8f121-104">In this tutorial, you learn how to integrate Yardi eLearning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8f121-105">Integrera Yardi ger e med Azure AD följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8f121-105">Integrating Yardi eLearning with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8f121-106">Du kan styra i Azure AD som har åtkomst till Yardi e</span><span class="sxs-lookup"><span data-stu-id="8f121-106">You can control in Azure AD who has access to Yardi eLearning</span></span>
- <span data-ttu-id="8f121-107">Du kan aktivera användarna att automatiskt hämta loggat in på Yardi e (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8f121-107">You can enable your users to automatically get signed-on to Yardi eLearning (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8f121-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8f121-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8f121-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8f121-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f121-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8f121-110">Prerequisites</span></span>

<span data-ttu-id="8f121-111">För att konfigurera Azure AD-integrering med Yardi e, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8f121-111">To configure Azure AD integration with Yardi eLearning, you need the following items:</span></span>

- <span data-ttu-id="8f121-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8f121-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8f121-113">En Yardi e enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="8f121-113">A Yardi eLearning single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8f121-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8f121-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8f121-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8f121-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8f121-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8f121-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8f121-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f121-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8f121-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8f121-118">Scenario description</span></span>
<span data-ttu-id="8f121-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8f121-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8f121-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8f121-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8f121-121">Att lägga till Yardi e från galleriet</span><span class="sxs-lookup"><span data-stu-id="8f121-121">Adding Yardi eLearning from the gallery</span></span>
2. <span data-ttu-id="8f121-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f121-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-yardi-elearning-from-the-gallery"></a><span data-ttu-id="8f121-123">Att lägga till Yardi e från galleriet</span><span class="sxs-lookup"><span data-stu-id="8f121-123">Adding Yardi eLearning from the gallery</span></span>
<span data-ttu-id="8f121-124">Du måste lägga till Yardi e från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Yardi e i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f121-124">To configure the integration of Yardi eLearning into Azure AD, you need to add Yardi eLearning from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8f121-125">**Utför följande steg för att lägga till Yardi e från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8f121-125">**To add Yardi eLearning from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8f121-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8f121-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8f121-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8f121-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8f121-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8f121-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8f121-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f121-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8f121-133">I sökrutan skriver **Yardi e**.</span><span class="sxs-lookup"><span data-stu-id="8f121-133">In the search box, type **Yardi eLearning**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_search.png)

5. <span data-ttu-id="8f121-135">Välj i resultatpanelen **Yardi e**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8f121-135">In the results panel, select **Yardi eLearning**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8f121-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f121-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8f121-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Yardi e baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8f121-138">In this section, you configure and test Azure AD single sign-on with Yardi eLearning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8f121-139">Azure AD måste du känna till användaren i Yardi e motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8f121-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Yardi eLearning is to a user in Azure AD.</span></span> <span data-ttu-id="8f121-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Yardi e upprättas.</span><span class="sxs-lookup"><span data-stu-id="8f121-140">In other words, a link relationship between an Azure AD user and the related user in Yardi eLearning needs to be established.</span></span>

<span data-ttu-id="8f121-141">I Yardi e, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8f121-141">In Yardi eLearning, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8f121-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Yardi e, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8f121-142">To configure and test Azure AD single sign-on with Yardi eLearning, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8f121-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8f121-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8f121-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8f121-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8f121-145">**[Skapa en testanvändare för Yardi e](#creating-a-yardi-elearning-test-user)**  – du har en motsvarighet för Britta Simon i Yardi e som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8f121-145">**[Creating a Yardi eLearning test user](#creating-a-yardi-elearning-test-user)** - to have a counterpart of Britta Simon in Yardi eLearning that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8f121-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8f121-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8f121-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8f121-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8f121-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f121-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8f121-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Yardi e-program.</span><span class="sxs-lookup"><span data-stu-id="8f121-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Yardi eLearning application.</span></span>

<span data-ttu-id="8f121-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Yardi e:**</span><span class="sxs-lookup"><span data-stu-id="8f121-150">**To configure Azure AD single sign-on with Yardi eLearning, perform the following steps:**</span></span>

1. <span data-ttu-id="8f121-151">I Azure-portalen på den **Yardi e** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8f121-151">In the Azure portal, on the **Yardi eLearning** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8f121-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8f121-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_samlbase.png)

3. <span data-ttu-id="8f121-155">På den **Yardi e domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8f121-155">On the **Yardi eLearning Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_url.png)

    <span data-ttu-id="8f121-157">a.</span><span class="sxs-lookup"><span data-stu-id="8f121-157">a.</span></span> <span data-ttu-id="8f121-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.yardielearning.com/login`</span><span class="sxs-lookup"><span data-stu-id="8f121-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.yardielearning.com/login`</span></span>

    <span data-ttu-id="8f121-159">b.</span><span class="sxs-lookup"><span data-stu-id="8f121-159">b.</span></span> <span data-ttu-id="8f121-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.yardielearning.com/trust`</span><span class="sxs-lookup"><span data-stu-id="8f121-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.yardielearning.com/trust`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8f121-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="8f121-161">These values are not real.</span></span> <span data-ttu-id="8f121-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="8f121-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8f121-163">Kontakta [Yardi e klienten supportteamet](mailto:elearning@yardi.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8f121-163">Contact [Yardi eLearning Client support team](mailto:elearning@yardi.com) to get these values.</span></span> 
 
4. <span data-ttu-id="8f121-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8f121-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_certificate.png) 

5. <span data-ttu-id="8f121-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8f121-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-yardielearning-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8f121-168">Konfigurera enkel inloggning på **Yardi e** sida, måste du skicka den hämtade **XML-Metadata för** till [Yardi e-supportteamet](mailto:elearning@yardi.com).</span><span class="sxs-lookup"><span data-stu-id="8f121-168">To configure single sign-on on **Yardi eLearning** side, you need to send the downloaded **Metadata XML** to [Yardi eLearning support team](mailto:elearning@yardi.com).</span></span> 

> [!TIP]
> <span data-ttu-id="8f121-169">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8f121-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8f121-170">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8f121-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8f121-171">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8f121-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8f121-172">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8f121-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="8f121-173">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8f121-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8f121-175">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8f121-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8f121-176">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8f121-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8f121-178">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8f121-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8f121-180">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f121-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8f121-182">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8f121-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-yardielearning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8f121-184">a.</span><span class="sxs-lookup"><span data-stu-id="8f121-184">a.</span></span> <span data-ttu-id="8f121-185">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8f121-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8f121-186">b.</span><span class="sxs-lookup"><span data-stu-id="8f121-186">b.</span></span> <span data-ttu-id="8f121-187">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8f121-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8f121-188">c.</span><span class="sxs-lookup"><span data-stu-id="8f121-188">c.</span></span> <span data-ttu-id="8f121-189">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8f121-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8f121-190">d.</span><span class="sxs-lookup"><span data-stu-id="8f121-190">d.</span></span> <span data-ttu-id="8f121-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8f121-191">Click **Create**.</span></span>
 
### <a name="creating-a-yardi-elearning-test-user"></a><span data-ttu-id="8f121-192">Skapa en Yardi e testanvändare</span><span class="sxs-lookup"><span data-stu-id="8f121-192">Creating a Yardi eLearning test user</span></span>

<span data-ttu-id="8f121-193">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Yardi e.</span><span class="sxs-lookup"><span data-stu-id="8f121-193">The objective of this section is to create a user called Britta Simon in Yardi eLearning.</span></span> <span data-ttu-id="8f121-194">Yardi e stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="8f121-194">Yardi eLearning supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="8f121-195">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8f121-195">There is no action item for you in this section.</span></span> <span data-ttu-id="8f121-196">En ny användare skapas under ett försök att komma åt Yardi e om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="8f121-196">A new user is created during an attempt to access Yardi eLearning if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="8f121-197">Om du behöver skapa en användare manuellt, måste du kontakta den [Yardi e-supportteamet](mailto:elearning@yardi.com).</span><span class="sxs-lookup"><span data-stu-id="8f121-197">If you need to create a user manually, you need to contact the [Yardi eLearning support team](mailto:elearning@yardi.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8f121-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8f121-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8f121-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Yardi e.</span><span class="sxs-lookup"><span data-stu-id="8f121-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Yardi eLearning.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8f121-201">**Om du vill tilldela Yardi e Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8f121-201">**To assign Britta Simon to Yardi eLearning, perform the following steps:**</span></span>

1. <span data-ttu-id="8f121-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8f121-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8f121-204">Välj i listan med program **Yardi e**.</span><span class="sxs-lookup"><span data-stu-id="8f121-204">In the applications list, select **Yardi eLearning**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-yardielearning-tutorial/tutorial_yardielearning_app.png) 

3. <span data-ttu-id="8f121-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8f121-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8f121-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8f121-208">Click **Add** button.</span></span> <span data-ttu-id="8f121-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f121-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8f121-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8f121-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8f121-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f121-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8f121-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f121-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8f121-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f121-214">Testing single sign-on</span></span>

<span data-ttu-id="8f121-215">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8f121-215">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8f121-216">När du klickar på panelen Yardi e på åtkomstpanelen du bör få automatiskt loggat in på ditt Yardi e-program.</span><span class="sxs-lookup"><span data-stu-id="8f121-216">When you click the Yardi eLearning tile in the Access Panel, you should get automatically signed-on to your Yardi eLearning application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8f121-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8f121-217">Additional resources</span></span>

* [<span data-ttu-id="8f121-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8f121-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8f121-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8f121-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-yardielearning-tutorial/tutorial_general_203.png

