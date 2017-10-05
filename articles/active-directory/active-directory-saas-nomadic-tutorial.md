---
title: "Självstudier: Azure Active Directory-integrering med Nomadic | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Nomadic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13d02b1c-d98a-40b1-824f-afa45a2deb6a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 1817a1395c2ffa7268abfff268d9d041f7f21a57
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadic"></a><span data-ttu-id="8c823-103">Självstudier: Azure Active Directory-integrering med Nomadic</span><span class="sxs-lookup"><span data-stu-id="8c823-103">Tutorial: Azure Active Directory integration with Nomadic</span></span>

<span data-ttu-id="8c823-104">I kursen får lära du att integrera Nomadic med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8c823-104">In this tutorial, you learn how to integrate Nomadic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8c823-105">Integrera Nomadic med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8c823-105">Integrating Nomadic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8c823-106">Du kan styra i Azure AD som har åtkomst till Nomadic.</span><span class="sxs-lookup"><span data-stu-id="8c823-106">You can control in Azure AD who has access to Nomadic.</span></span>
- <span data-ttu-id="8c823-107">Du kan aktivera användarna att automatiskt hämta loggat in på Nomadic (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="8c823-107">You can enable your users to automatically get signed-on to Nomadic (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="8c823-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8c823-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="8c823-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8c823-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c823-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8c823-110">Prerequisites</span></span>

<span data-ttu-id="8c823-111">För att konfigurera Azure AD-integrering med Nomadic, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8c823-111">To configure Azure AD integration with Nomadic, you need the following items:</span></span>

- <span data-ttu-id="8c823-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8c823-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8c823-113">En Nomadic enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8c823-113">A Nomadic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8c823-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8c823-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8c823-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8c823-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8c823-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8c823-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8c823-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8c823-117">If you don't have an Azure AD trial environment, you can [get a one-month trial here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8c823-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8c823-118">Scenario description</span></span>
<span data-ttu-id="8c823-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8c823-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8c823-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8c823-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8c823-121">Lägg till Nomadic från galleriet</span><span class="sxs-lookup"><span data-stu-id="8c823-121">Add Nomadic from the gallery</span></span>
2. <span data-ttu-id="8c823-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c823-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-nomadic-from-the-gallery"></a><span data-ttu-id="8c823-123">Lägg till Nomadic från galleriet</span><span class="sxs-lookup"><span data-stu-id="8c823-123">Add Nomadic from the gallery</span></span>
<span data-ttu-id="8c823-124">Du måste lägga till Nomadic från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Nomadic i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c823-124">To configure the integration of Nomadic into Azure AD, you need to add Nomadic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8c823-125">**Utför följande steg för att lägga till Nomadic från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8c823-125">**To add Nomadic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8c823-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8c823-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="8c823-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8c823-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8c823-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8c823-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="8c823-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c823-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="8c823-133">I sökrutan skriver **Nomadic**väljer **Nomadic** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8c823-133">In the search box, type **Nomadic**, select **Nomadic** from result panel then click **Add** button to add the application.</span></span>

    ![Nomadic i resultatlistan](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8c823-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c823-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="8c823-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Nomadic baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8c823-136">In this section, you configure and test Azure AD single sign-on with Nomadic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8c823-137">Azure AD måste du känna till användaren i Nomadic motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8c823-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Nomadic is to a user in Azure AD.</span></span> <span data-ttu-id="8c823-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Nomadic upprättas.</span><span class="sxs-lookup"><span data-stu-id="8c823-138">In other words, a link relationship between an Azure AD user and the related user in Nomadic needs to be established.</span></span>

<span data-ttu-id="8c823-139">I Nomadic, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8c823-139">In Nomadic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8c823-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Nomadic, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8c823-140">To configure and test Azure AD single sign-on with Nomadic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8c823-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8c823-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8c823-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8c823-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8c823-143">**[Skapa en Nomadic testanvändare](#create-a-nomadic-test-user)**  – du har en motsvarighet för Britta Simon i Nomadic som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8c823-143">**[Create a Nomadic test user](#create-a-nomadic-test-user)** - to have a counterpart of Britta Simon in Nomadic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8c823-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8c823-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8c823-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8c823-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8c823-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c823-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8c823-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Nomadic program.</span><span class="sxs-lookup"><span data-stu-id="8c823-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Nomadic application.</span></span>

<span data-ttu-id="8c823-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Nomadic:**</span><span class="sxs-lookup"><span data-stu-id="8c823-148">**To configure Azure AD single sign-on with Nomadic, perform the following steps:**</span></span>

1. <span data-ttu-id="8c823-149">I Azure-portalen på den **Nomadic** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8c823-149">In the Azure portal, on the **Nomadic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="8c823-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8c823-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_samlbase.png)

3. <span data-ttu-id="8c823-153">På den **Nomadic domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8c823-153">On the **Nomadic Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och nomadic domän med enkel inloggning information](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_url.png)

    <span data-ttu-id="8c823-155">a.</span><span class="sxs-lookup"><span data-stu-id="8c823-155">a.</span></span> <span data-ttu-id="8c823-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.nomadic.fm/signin`</span><span class="sxs-lookup"><span data-stu-id="8c823-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.nomadic.fm/signin`</span></span>

    <span data-ttu-id="8c823-157">b.</span><span class="sxs-lookup"><span data-stu-id="8c823-157">b.</span></span> <span data-ttu-id="8c823-158">I den **identifierare** textruta Skriv en URL med följande mönster: `https://<company name>.nomadic.fm/auth/saml2/sp`,`https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span><span class="sxs-lookup"><span data-stu-id="8c823-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.nomadic.fm/auth/saml2/sp`, `https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8c823-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="8c823-159">These values are not real.</span></span> <span data-ttu-id="8c823-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="8c823-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8c823-161">Kontakta [Nomadic klienten supportteamet](mailto:help@nomadic.fm) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8c823-161">Contact [Nomadic Client support team](mailto:help@nomadic.fm) to get these values.</span></span> 
 


4. <span data-ttu-id="8c823-162">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8c823-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_certificate.png) 

5. <span data-ttu-id="8c823-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8c823-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-nomadic-tutorial/tutorial_general_400.png)

6.  <span data-ttu-id="8c823-166">För att få SSO konfigurerats för ditt program, kontakta [Nomadic supportteamet](mailto:help@nomadic.fm) och ger dem den hämtade **metadata**.</span><span class="sxs-lookup"><span data-stu-id="8c823-166">To get SSO configured for your application, contact [Nomadic support team](mailto:help@nomadic.fm) and provide them with the downloaded **metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="8c823-167">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8c823-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8c823-168">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8c823-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8c823-169">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8c823-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8c823-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8c823-170">Create an Azure AD test user</span></span>

<span data-ttu-id="8c823-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8c823-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="8c823-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8c823-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8c823-174">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="8c823-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-nomadic-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="8c823-176">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8c823-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-nomadic-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="8c823-178">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c823-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-nomadic-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="8c823-180">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8c823-180">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-nomadic-tutorial/create_aaduser_04.png)

    <span data-ttu-id="8c823-182">a.</span><span class="sxs-lookup"><span data-stu-id="8c823-182">a.</span></span> <span data-ttu-id="8c823-183">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8c823-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8c823-184">b.</span><span class="sxs-lookup"><span data-stu-id="8c823-184">b.</span></span> <span data-ttu-id="8c823-185">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="8c823-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="8c823-186">c.</span><span class="sxs-lookup"><span data-stu-id="8c823-186">c.</span></span> <span data-ttu-id="8c823-187">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="8c823-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="8c823-188">d.</span><span class="sxs-lookup"><span data-stu-id="8c823-188">d.</span></span> <span data-ttu-id="8c823-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8c823-189">Click **Create**.</span></span>
 
### <a name="create-a-nomadic-test-user"></a><span data-ttu-id="8c823-190">Skapa en Nomadic testanvändare</span><span class="sxs-lookup"><span data-stu-id="8c823-190">Create a Nomadic test user</span></span>

<span data-ttu-id="8c823-191">I det här avsnittet skapar du en användare som kallas Britta Simon i Nomadic.</span><span class="sxs-lookup"><span data-stu-id="8c823-191">In this section, you create a user called Britta Simon in Nomadic.</span></span> <span data-ttu-id="8c823-192">Se tillsammans med [Nomadic supportteamet](mailto:help@nomadic.fm) att lägga till användare i Nomadic plattformen.</span><span class="sxs-lookup"><span data-stu-id="8c823-192">Please work with [Nomadic support team](mailto:help@nomadic.fm) to add the users in the Nomadic platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="8c823-193">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8c823-193">Assign the Azure AD test user</span></span>

<span data-ttu-id="8c823-194">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Nomadic.</span><span class="sxs-lookup"><span data-stu-id="8c823-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Nomadic.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="8c823-196">**Om du vill tilldela Nomadic Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8c823-196">**To assign Britta Simon to Nomadic, perform the following steps:**</span></span>

1. <span data-ttu-id="8c823-197">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8c823-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8c823-199">Välj i listan med program **Nomadic**.</span><span class="sxs-lookup"><span data-stu-id="8c823-199">In the applications list, select **Nomadic**.</span></span>

    ![Nomadic länken i listan med program](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_app.png)  

3. <span data-ttu-id="8c823-201">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8c823-201">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="8c823-203">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8c823-203">Click **Add** button.</span></span> <span data-ttu-id="8c823-204">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c823-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="8c823-206">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8c823-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8c823-207">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c823-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8c823-208">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c823-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8c823-209">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c823-209">Test single sign-on</span></span>

<span data-ttu-id="8c823-210">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8c823-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8c823-211">När du klickar på panelen Nomadic på panelen åtkomst du ska hämta automatiskt loggat in på ditt Nomadic program.</span><span class="sxs-lookup"><span data-stu-id="8c823-211">When you click the Nomadic tile in the Access Panel, you should get automatically signed-on to your Nomadic application.</span></span>
<span data-ttu-id="8c823-212">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8c823-212">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8c823-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8c823-213">Additional resources</span></span>

* [<span data-ttu-id="8c823-214">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8c823-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8c823-215">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8c823-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_203.png

