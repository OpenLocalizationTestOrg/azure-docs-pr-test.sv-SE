---
title: "Självstudier: Azure Active Directory-integrering med Skilljar | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Skilljar."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c572f556-98a3-48e6-8e4c-e634b7a2ba70
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: d5238a0471b6ae4b367ca5c1ed5e1f273a0c476b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skilljar"></a><span data-ttu-id="90e71-103">Självstudier: Azure Active Directory-integrering med Skilljar</span><span class="sxs-lookup"><span data-stu-id="90e71-103">Tutorial: Azure Active Directory integration with Skilljar</span></span>

<span data-ttu-id="90e71-104">I kursen får lära du att integrera Skilljar med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="90e71-104">In this tutorial, you learn how to integrate Skilljar with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="90e71-105">Integrera Skilljar med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="90e71-105">Integrating Skilljar with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="90e71-106">Du kan styra i Azure AD som har åtkomst till Skilljar</span><span class="sxs-lookup"><span data-stu-id="90e71-106">You can control in Azure AD who has access to Skilljar</span></span>
- <span data-ttu-id="90e71-107">Du kan aktivera användarna att automatiskt hämta loggat in på Skilljar (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="90e71-107">You can enable your users to automatically get signed-on to Skilljar (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="90e71-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="90e71-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="90e71-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="90e71-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90e71-110">Krav</span><span class="sxs-lookup"><span data-stu-id="90e71-110">Prerequisites</span></span>

<span data-ttu-id="90e71-111">För att konfigurera Azure AD-integrering med Skilljar, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="90e71-111">To configure Azure AD integration with Skilljar, you need the following items:</span></span>

- <span data-ttu-id="90e71-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="90e71-112">An Azure AD subscription</span></span>
- <span data-ttu-id="90e71-113">En Skilljar enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="90e71-113">A Skilljar single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="90e71-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="90e71-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="90e71-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="90e71-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="90e71-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="90e71-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="90e71-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="90e71-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="90e71-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="90e71-118">Scenario description</span></span>
<span data-ttu-id="90e71-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="90e71-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="90e71-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="90e71-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="90e71-121">Att lägga till Skilljar från galleriet</span><span class="sxs-lookup"><span data-stu-id="90e71-121">Adding Skilljar from the gallery</span></span>
2. <span data-ttu-id="90e71-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="90e71-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skilljar-from-the-gallery"></a><span data-ttu-id="90e71-123">Att lägga till Skilljar från galleriet</span><span class="sxs-lookup"><span data-stu-id="90e71-123">Adding Skilljar from the gallery</span></span>
<span data-ttu-id="90e71-124">Du måste lägga till Skilljar från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Skilljar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="90e71-124">To configure the integration of Skilljar into Azure AD, you need to add Skilljar from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="90e71-125">**Utför följande steg för att lägga till Skilljar från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="90e71-125">**To add Skilljar from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="90e71-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="90e71-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="90e71-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="90e71-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="90e71-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="90e71-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="90e71-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="90e71-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="90e71-133">I sökrutan skriver **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="90e71-133">In the search box, type **Skilljar**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_search.png)

5. <span data-ttu-id="90e71-135">Välj i resultatpanelen **Skilljar**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="90e71-135">In the results panel, select **Skilljar**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="90e71-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="90e71-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="90e71-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Skilljar baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="90e71-138">In this section, you configure and test Azure AD single sign-on with Skilljar based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="90e71-139">Azure AD måste du känna till användaren i Skilljar motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="90e71-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Skilljar is to a user in Azure AD.</span></span> <span data-ttu-id="90e71-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Skilljar upprättas.</span><span class="sxs-lookup"><span data-stu-id="90e71-140">In other words, a link relationship between an Azure AD user and the related user in Skilljar needs to be established.</span></span>

<span data-ttu-id="90e71-141">I Skilljar, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="90e71-141">In Skilljar, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="90e71-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Skilljar, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="90e71-142">To configure and test Azure AD single sign-on with Skilljar, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="90e71-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="90e71-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="90e71-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90e71-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="90e71-145">**[Skapa en testanvändare Skilljar](#creating-a-skilljar-test-user)**  – du har en motsvarighet för Britta Simon i Skilljar som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="90e71-145">**[Creating a Skilljar test user](#creating-a-skilljar-test-user)** - to have a counterpart of Britta Simon in Skilljar that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="90e71-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="90e71-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="90e71-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="90e71-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="90e71-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="90e71-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="90e71-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Skilljar program.</span><span class="sxs-lookup"><span data-stu-id="90e71-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Skilljar application.</span></span>

<span data-ttu-id="90e71-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Skilljar:**</span><span class="sxs-lookup"><span data-stu-id="90e71-150">**To configure Azure AD single sign-on with Skilljar, perform the following steps:**</span></span>

1. <span data-ttu-id="90e71-151">I Azure-portalen på den **Skilljar** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="90e71-151">In the Azure portal, on the **Skilljar** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="90e71-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="90e71-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_samlbase.png)

3. <span data-ttu-id="90e71-155">På den **Skilljar domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="90e71-155">On the **Skilljar Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_url.png)

    <span data-ttu-id="90e71-157">a.</span><span class="sxs-lookup"><span data-stu-id="90e71-157">a.</span></span> <span data-ttu-id="90e71-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="90e71-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.skilljar.com/`</span></span>

    <span data-ttu-id="90e71-159">b.</span><span class="sxs-lookup"><span data-stu-id="90e71-159">b.</span></span> <span data-ttu-id="90e71-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.skilljar.com/`</span><span class="sxs-lookup"><span data-stu-id="90e71-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.skilljar.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="90e71-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="90e71-161">These values are not real.</span></span> <span data-ttu-id="90e71-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="90e71-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="90e71-163">Kontakta [Skilljar klienten supportteamet](http://support.skilljar.com/hc/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="90e71-163">Contact [Skilljar Client support team](http://support.skilljar.com/hc/) to get these values.</span></span> 
 
4. <span data-ttu-id="90e71-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="90e71-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_certificate.png) 

5. <span data-ttu-id="90e71-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="90e71-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="90e71-168">Konfigurera enkel inloggning på **Skilljar** sida, måste du skicka den hämtade **XML-Metadata för**, och **namn Format identifierarvärde - urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress** till [Skilljar supportteamet](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="90e71-168">To configure single sign-on on **Skilljar** side, you need to send the downloaded **Metadata XML**, and **Name Identifier Format Value - urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress** to [Skilljar support team](http://support.skilljar.com/hc/).</span></span> <span data-ttu-id="90e71-169">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="90e71-169">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="90e71-170">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="90e71-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="90e71-171">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="90e71-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="90e71-172">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="90e71-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="90e71-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="90e71-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="90e71-174">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="90e71-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="90e71-176">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="90e71-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="90e71-177">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="90e71-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="90e71-179">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="90e71-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="90e71-181">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="90e71-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="90e71-183">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="90e71-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-skilljar-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="90e71-185">a.</span><span class="sxs-lookup"><span data-stu-id="90e71-185">a.</span></span> <span data-ttu-id="90e71-186">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="90e71-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="90e71-187">b.</span><span class="sxs-lookup"><span data-stu-id="90e71-187">b.</span></span> <span data-ttu-id="90e71-188">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="90e71-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="90e71-189">c.</span><span class="sxs-lookup"><span data-stu-id="90e71-189">c.</span></span> <span data-ttu-id="90e71-190">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="90e71-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="90e71-191">d.</span><span class="sxs-lookup"><span data-stu-id="90e71-191">d.</span></span> <span data-ttu-id="90e71-192">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="90e71-192">Click **Create**.</span></span>
 
### <a name="creating-a-skilljar-test-user"></a><span data-ttu-id="90e71-193">Skapa en testanvändare Skilljar</span><span class="sxs-lookup"><span data-stu-id="90e71-193">Creating a Skilljar test user</span></span>

<span data-ttu-id="90e71-194">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Skilljar.</span><span class="sxs-lookup"><span data-stu-id="90e71-194">The objective of this section is to create a user called Britta Simon in Skilljar.</span></span> <span data-ttu-id="90e71-195">Skilljar stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="90e71-195">Skilljar supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="90e71-196">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="90e71-196">There is no action item for you in this section.</span></span> <span data-ttu-id="90e71-197">En ny användare skapas under ett försök att komma åt Skilljar om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="90e71-197">A new user is created during an attempt to access Skilljar if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="90e71-198">Om du behöver skapa en användare manuellt, måste du kontakta den [Skilljar supportteamet](http://support.skilljar.com/hc/).</span><span class="sxs-lookup"><span data-stu-id="90e71-198">If you need to create a user manually, you need to contact the [Skilljar support team](http://support.skilljar.com/hc/).</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="90e71-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="90e71-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="90e71-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Skilljar.</span><span class="sxs-lookup"><span data-stu-id="90e71-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Skilljar.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="90e71-202">**Om du vill tilldela Skilljar Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="90e71-202">**To assign Britta Simon to Skilljar, perform the following steps:**</span></span>

1. <span data-ttu-id="90e71-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="90e71-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="90e71-205">Välj i listan med program **Skilljar**.</span><span class="sxs-lookup"><span data-stu-id="90e71-205">In the applications list, select **Skilljar**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-skilljar-tutorial/tutorial_skilljar_app.png) 

3. <span data-ttu-id="90e71-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="90e71-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="90e71-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="90e71-209">Click **Add** button.</span></span> <span data-ttu-id="90e71-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="90e71-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="90e71-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="90e71-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="90e71-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="90e71-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="90e71-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="90e71-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="90e71-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="90e71-215">Testing single sign-on</span></span>

<span data-ttu-id="90e71-216">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="90e71-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="90e71-217">När du klickar på panelen Skilljar på åtkomstpanelen du bör få automatiskt loggat in på ditt Skilljar program.</span><span class="sxs-lookup"><span data-stu-id="90e71-217">When you click the Skilljar tile in the Access Panel, you should get automatically signed-on to your Skilljar application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="90e71-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="90e71-218">Additional resources</span></span>

* [<span data-ttu-id="90e71-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="90e71-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="90e71-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="90e71-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skilljar-tutorial/tutorial_general_203.png

