---
title: "Självstudier: Azure Active Directory-integrering med Workrite | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Workrite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4358c4c621634c17cbbd7fa1c72f12746b8e4a2a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a><span data-ttu-id="7ddc8-103">Självstudier: Azure Active Directory-integrering med Workrite</span><span class="sxs-lookup"><span data-stu-id="7ddc8-103">Tutorial: Azure Active Directory integration with Workrite</span></span>

<span data-ttu-id="7ddc8-104">I kursen får lära du att integrera Workrite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7ddc8-104">In this tutorial, you learn how to integrate Workrite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7ddc8-105">Integrera Workrite med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-105">Integrating Workrite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7ddc8-106">Du kan styra i Azure AD som har åtkomst till Workrite.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-106">You can control in Azure AD who has access to Workrite.</span></span>
- <span data-ttu-id="7ddc8-107">Du kan aktivera användarna att automatiskt hämta loggat in på Workrite (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-107">You can enable your users to automatically get signed-on to Workrite (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="7ddc8-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="7ddc8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7ddc8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ddc8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7ddc8-110">Prerequisites</span></span>

<span data-ttu-id="7ddc8-111">För att konfigurera Azure AD-integrering med Workrite, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-111">To configure Azure AD integration with Workrite, you need the following items:</span></span>

- <span data-ttu-id="7ddc8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ddc8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7ddc8-113">En Workrite enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ddc8-113">A Workrite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7ddc8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7ddc8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7ddc8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7ddc8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7ddc8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7ddc8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7ddc8-118">Scenario description</span></span>
<span data-ttu-id="7ddc8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7ddc8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7ddc8-121">Att lägga till Workrite från galleriet</span><span class="sxs-lookup"><span data-stu-id="7ddc8-121">Adding Workrite from the gallery</span></span>
2. <span data-ttu-id="7ddc8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ddc8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workrite-from-the-gallery"></a><span data-ttu-id="7ddc8-123">Att lägga till Workrite från galleriet</span><span class="sxs-lookup"><span data-stu-id="7ddc8-123">Adding Workrite from the gallery</span></span>
<span data-ttu-id="7ddc8-124">Du måste lägga till Workrite från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Workrite i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-124">To configure the integration of Workrite into Azure AD, you need to add Workrite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7ddc8-125">**Utför följande steg för att lägga till Workrite från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7ddc8-125">**To add Workrite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7ddc8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="7ddc8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7ddc8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="7ddc8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="7ddc8-133">I sökrutan skriver **Workrite**väljer **Workrite** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-133">In the search box, type **Workrite**, select **Workrite** from result panel then click **Add** button to add the application.</span></span>

    ![Workrite i resultatlistan](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="7ddc8-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ddc8-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="7ddc8-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Workrite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-136">In this section, you configure and test Azure AD single sign-on with Workrite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7ddc8-137">Azure AD måste du känna till användaren i Workrite motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Workrite is to a user in Azure AD.</span></span> <span data-ttu-id="7ddc8-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Workrite upprättas.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-138">In other words, a link relationship between an Azure AD user and the related user in Workrite needs to be established.</span></span>

<span data-ttu-id="7ddc8-139">I Workrite, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-139">In Workrite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7ddc8-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Workrite, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-140">To configure and test Azure AD single sign-on with Workrite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7ddc8-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7ddc8-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7ddc8-143">**[Skapa en testanvändare Workrite](#create-a-workrite-test-user)**  – du har en motsvarighet för Britta Simon i Workrite som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-143">**[Create a Workrite test user](#create-a-workrite-test-user)** - to have a counterpart of Britta Simon in Workrite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7ddc8-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7ddc8-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="7ddc8-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ddc8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="7ddc8-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Workrite program.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workrite application.</span></span>

<span data-ttu-id="7ddc8-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Workrite:**</span><span class="sxs-lookup"><span data-stu-id="7ddc8-148">**To configure Azure AD single sign-on with Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="7ddc8-149">I Azure-portalen på den **Workrite** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-149">In the Azure portal, on the **Workrite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="7ddc8-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_samlbase.png)

3. <span data-ttu-id="7ddc8-153">På den **Workrite domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-153">On the **Workrite Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Workrite domän med enkel inloggning information](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_url.png)

    <span data-ttu-id="7ddc8-155">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="7ddc8-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7ddc8-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-156">This value is not real.</span></span> <span data-ttu-id="7ddc8-157">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="7ddc8-158">Kontakta [Workrite klienten supportteamet](mailto:support@workrite.co.uk) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-158">Contact [Workrite Client support team](mailto:support@workrite.co.uk) to get this value.</span></span>

4. <span data-ttu-id="7ddc8-159">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_certificate.png) 

5. <span data-ttu-id="7ddc8-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-workrite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7ddc8-163">På den **Workrite Configuration** klickar du på **konfigurera Workrite** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-163">On the **Workrite Configuration** section, click **Configure Workrite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7ddc8-164">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7ddc8-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Workrite konfiguration](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_configure.png) 

7. <span data-ttu-id="7ddc8-166">Konfigurera enkel inloggning på **Workrite** sida, måste du skicka den hämtade **Certificate(Base64), Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** till [Workrite supportteam](mailto:support@workrite.co.uk).</span><span class="sxs-lookup"><span data-stu-id="7ddc8-166">To configure single sign-on on **Workrite** side, you need to send the downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Workrite support team](mailto:support@workrite.co.uk).</span></span>

> [!TIP]
> <span data-ttu-id="7ddc8-167">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7ddc8-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7ddc8-168">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7ddc8-169">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7ddc8-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7ddc8-170">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ddc8-170">Create an Azure AD test user</span></span>

<span data-ttu-id="7ddc8-171">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="7ddc8-173">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7ddc8-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7ddc8-174">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-workrite-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="7ddc8-176">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-workrite-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="7ddc8-178">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="7ddc8-180">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-180">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png)

    <span data-ttu-id="7ddc8-182">a.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-182">a.</span></span> <span data-ttu-id="7ddc8-183">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7ddc8-184">b.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-184">b.</span></span> <span data-ttu-id="7ddc8-185">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="7ddc8-186">c.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-186">c.</span></span> <span data-ttu-id="7ddc8-187">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="7ddc8-188">d.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-188">d.</span></span> <span data-ttu-id="7ddc8-189">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-189">Click **Create**.</span></span>
 
### <a name="create-a-workrite-test-user"></a><span data-ttu-id="7ddc8-190">Skapa en testanvändare Workrite</span><span class="sxs-lookup"><span data-stu-id="7ddc8-190">Create a Workrite test user</span></span>

<span data-ttu-id="7ddc8-191">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Workrite.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-191">The objective of this section is to create a user called Britta Simon in Workrite.</span></span>

<span data-ttu-id="7ddc8-192">**Utför följande steg för att skapa en användare som kallas Britta Simon i Workrite:**</span><span class="sxs-lookup"><span data-stu-id="7ddc8-192">**To create a user called Britta Simon in Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="7ddc8-193">Logga in på webbplatsen workrite företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-193">Sign on to your workrite company site as administrator.</span></span>

2. <span data-ttu-id="7ddc8-194">I navigeringsfönstret klickar du på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-194">In the navigation pane, click **Admin**.</span></span>
   
    ![-Administratörer kontroll][400]

3. <span data-ttu-id="7ddc8-196">Gå till snabblänkar och klicka sedan på **skapar du en användare**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-196">Go to Quick Links, and then click **Create a User**.</span></span>
   
    ![Skapa användare][401]

4. <span data-ttu-id="7ddc8-198">På den **skapa användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ddc8-198">On the **Create User** dialog, perform the following steps:</span></span>
   
    ![Skapa användare Dailog][402]
    
    <span data-ttu-id="7ddc8-200">a.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-200">a.</span></span> <span data-ttu-id="7ddc8-201">I den **e-post** textruta typen e-postadressen för användaren som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-201">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="7ddc8-202">b.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-202">b.</span></span> <span data-ttu-id="7ddc8-203">I den **Förnamn** textruta skriver firstname för användare som Britta.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-203">In the **First Name** textbox, type the firstname of user like Britta.</span></span>

    <span data-ttu-id="7ddc8-204">c.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-204">c.</span></span> <span data-ttu-id="7ddc8-205">I den **efternamn** textruta Ange efternamn för användaren som Simon.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-205">In the **Surname** textbox, type the surname of user like Simon.</span></span>
    
    <span data-ttu-id="7ddc8-206">d.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-206">d.</span></span> <span data-ttu-id="7ddc8-207">Välj **administratör** som **Välj rollen**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-207">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="7ddc8-208">e.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-208">e.</span></span> <span data-ttu-id="7ddc8-209">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-209">Click **Save**.</span></span>   

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="7ddc8-210">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7ddc8-210">Assign the Azure AD test user</span></span>

<span data-ttu-id="7ddc8-211">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Workrite.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workrite.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="7ddc8-213">**Om du vill tilldela Workrite Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7ddc8-213">**To assign Britta Simon to Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="7ddc8-214">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7ddc8-216">Välj i listan med program **Workrite**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-216">In the applications list, select **Workrite**.</span></span>

    ![Länken Workrite i listan med program](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_app.png)  

3. <span data-ttu-id="7ddc8-218">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="7ddc8-220">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-220">Click **Add** button.</span></span> <span data-ttu-id="7ddc8-221">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="7ddc8-223">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7ddc8-224">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7ddc8-225">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="7ddc8-226">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ddc8-226">Test single sign-on</span></span>

<span data-ttu-id="7ddc8-227">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="7ddc8-228">När du klickar på panelen Workrite på åtkomstpanelen du bör få automatiskt loggat in på ditt Workrite program.</span><span class="sxs-lookup"><span data-stu-id="7ddc8-228">When you click the Workrite tile in the Access Panel, you should get automatically signed-on to your Workrite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ddc8-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7ddc8-229">Additional resources</span></span>

* [<span data-ttu-id="7ddc8-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ddc8-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7ddc8-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7ddc8-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png

