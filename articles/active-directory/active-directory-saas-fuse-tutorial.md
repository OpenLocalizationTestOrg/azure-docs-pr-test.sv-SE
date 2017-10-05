---
title: "Självstudier: Azure Active Directory-integrering med säkrad | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och säkrad."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5ef34f58-863a-4b37-875c-e8efa3e18bb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 9a91e22faced9e126043bebefd85c307dbdf933d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuse"></a><span data-ttu-id="902f5-103">Självstudier: Azure Active Directory-integrering med säkrad</span><span class="sxs-lookup"><span data-stu-id="902f5-103">Tutorial: Azure Active Directory integration with Fuse</span></span>

<span data-ttu-id="902f5-104">I kursen får lära du att integrera säkrad med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="902f5-104">In this tutorial, you learn how to integrate Fuse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="902f5-105">Integrera säkrad med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="902f5-105">Integrating Fuse with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="902f5-106">Du kan styra i Azure AD som har åtkomst till säkrad.</span><span class="sxs-lookup"><span data-stu-id="902f5-106">You can control in Azure AD who has access to Fuse.</span></span>
- <span data-ttu-id="902f5-107">Du kan aktivera användarna att automatiskt hämta loggat in på säkrad (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="902f5-107">You can enable your users to automatically get signed-on to Fuse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="902f5-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="902f5-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="902f5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="902f5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="902f5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="902f5-110">Prerequisites</span></span>

<span data-ttu-id="902f5-111">Om du vill konfigurera Azure AD-integrering med säkrad behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="902f5-111">To configure Azure AD integration with Fuse, you need the following items:</span></span>

- <span data-ttu-id="902f5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="902f5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="902f5-113">En säkrad enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="902f5-113">A Fuse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="902f5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="902f5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="902f5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="902f5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="902f5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="902f5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="902f5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="902f5-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="902f5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="902f5-118">Scenario description</span></span>
<span data-ttu-id="902f5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="902f5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="902f5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="902f5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="902f5-121">Lägg till säkrad från galleriet</span><span class="sxs-lookup"><span data-stu-id="902f5-121">Add Fuse from the gallery</span></span>
2. <span data-ttu-id="902f5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="902f5-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-fuse-from-the-gallery"></a><span data-ttu-id="902f5-123">Lägg till säkrad från galleriet</span><span class="sxs-lookup"><span data-stu-id="902f5-123">Add Fuse from the gallery</span></span>
<span data-ttu-id="902f5-124">Du måste lägga till säkrad från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av säkrad i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="902f5-124">To configure the integration of Fuse into Azure AD, you need to add Fuse from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="902f5-125">**Utför följande steg för att lägga till säkrad från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="902f5-125">**To add Fuse from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="902f5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="902f5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="902f5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="902f5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="902f5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="902f5-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="902f5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="902f5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="902f5-133">I sökrutan skriver **videoformatet**väljer **videoformatet** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="902f5-133">In the search box, type **Fuse**, select **Fuse** from result panel then click **Add** button to add the application.</span></span>

    ![Videoformatet i resultatlistan](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="902f5-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="902f5-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="902f5-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med säkrad baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="902f5-136">In this section, you configure and test Azure AD single sign-on with Fuse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="902f5-137">Azure AD måste du känna till användaren i säkrad motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="902f5-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Fuse is to a user in Azure AD.</span></span> <span data-ttu-id="902f5-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i säkrad upprättas.</span><span class="sxs-lookup"><span data-stu-id="902f5-138">In other words, a link relationship between an Azure AD user and the related user in Fuse needs to be established.</span></span>

<span data-ttu-id="902f5-139">I säkrad, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="902f5-139">In Fuse, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="902f5-140">Om du vill konfigurera och testa Azure AD enkel inloggning med säkrad, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="902f5-140">To configure and test Azure AD single sign-on with Fuse, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="902f5-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="902f5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="902f5-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="902f5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="902f5-143">**[Skapa en testanvändare säkrad](#create-a-fuse-test-user)**  – har en motsvarighet för Britta Simon säkrad som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="902f5-143">**[Create a Fuse test user](#create-a-fuse-test-user)** - to have a counterpart of Britta Simon in Fuse that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="902f5-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="902f5-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="902f5-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="902f5-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="902f5-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="902f5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="902f5-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din säkrad applikation.</span><span class="sxs-lookup"><span data-stu-id="902f5-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Fuse application.</span></span>

<span data-ttu-id="902f5-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med säkrad:**</span><span class="sxs-lookup"><span data-stu-id="902f5-148">**To configure Azure AD single sign-on with Fuse, perform the following steps:**</span></span>

1. <span data-ttu-id="902f5-149">I Azure-portalen på den **videoformatet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="902f5-149">In the Azure portal, on the **Fuse** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="902f5-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="902f5-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_samlbase.png)

3. <span data-ttu-id="902f5-153">På den **videoformatet domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="902f5-153">On the **Fuse Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och säkrad domän enkel inloggning information](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_url.png)
    
    <span data-ttu-id="902f5-155">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant name>.fusion-universal.com/`</span><span class="sxs-lookup"><span data-stu-id="902f5-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant name>.fusion-universal.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="902f5-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="902f5-156">This value is not real.</span></span> <span data-ttu-id="902f5-157">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="902f5-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="902f5-158">Kontakta [videoformatet klienten supportteamet](mailto:support@fusion-universal.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="902f5-158">Contact [Fuse Client support team](mailto:support@fusion-universal.com) to get this value.</span></span> 
 
4. <span data-ttu-id="902f5-159">På den **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="902f5-159">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_certificate.png) 

5. <span data-ttu-id="902f5-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="902f5-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-fuse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="902f5-163">På den **videoformatet Configuration** klickar du på **konfigurera videoformatet** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="902f5-163">On the **Fuse Configuration** section, click **Configure Fuse** to open **Configure sign-on** window.</span></span> <span data-ttu-id="902f5-164">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="902f5-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Säkrad konfiguration](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_configure.png) 

7. <span data-ttu-id="902f5-166">För att få SSO konfigurerats för ditt program, kontakta [säkrad supportteamet](mailto:support@fusion-universal.com) och ge dem med följande:</span><span class="sxs-lookup"><span data-stu-id="902f5-166">To get SSO configured for your application, contact [Fuse support team](mailto:support@fusion-universal.com) and provide them with the following:</span></span>

    * <span data-ttu-id="902f5-167">Den hämtade **certifikatfil (Raw)**</span><span class="sxs-lookup"><span data-stu-id="902f5-167">The downloaded **Certificate (Raw) file**</span></span>
    * <span data-ttu-id="902f5-168">Den **URL för SAML-tjänst för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="902f5-168">The **SAML Single Sign-On Service URL**</span></span>
    * <span data-ttu-id="902f5-169">Den **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="902f5-169">The **SAML Entity ID**</span></span>
    * <span data-ttu-id="902f5-170">Den **URL för utloggning**</span><span class="sxs-lookup"><span data-stu-id="902f5-170">The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="902f5-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="902f5-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="902f5-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="902f5-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="902f5-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="902f5-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="902f5-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="902f5-174">Create an Azure AD test user</span></span>

<span data-ttu-id="902f5-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="902f5-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="902f5-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="902f5-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="902f5-178">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="902f5-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-fuse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="902f5-180">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="902f5-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-fuse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="902f5-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="902f5-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-fuse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="902f5-184">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="902f5-184">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-fuse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="902f5-186">a.</span><span class="sxs-lookup"><span data-stu-id="902f5-186">a.</span></span> <span data-ttu-id="902f5-187">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="902f5-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="902f5-188">b.</span><span class="sxs-lookup"><span data-stu-id="902f5-188">b.</span></span> <span data-ttu-id="902f5-189">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="902f5-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="902f5-190">c.</span><span class="sxs-lookup"><span data-stu-id="902f5-190">c.</span></span> <span data-ttu-id="902f5-191">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="902f5-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="902f5-192">d.</span><span class="sxs-lookup"><span data-stu-id="902f5-192">d.</span></span> <span data-ttu-id="902f5-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="902f5-193">Click **Create**.</span></span>
 
### <a name="create-a-fuse-test-user"></a><span data-ttu-id="902f5-194">Skapa en testanvändare säkrad</span><span class="sxs-lookup"><span data-stu-id="902f5-194">Create a Fuse test user</span></span>

<span data-ttu-id="902f5-195">I det här avsnittet skapar du en användare som kallas Britta Simon i säkrad.</span><span class="sxs-lookup"><span data-stu-id="902f5-195">In this section, you create a user called Britta Simon in Fuse.</span></span> <span data-ttu-id="902f5-196">Se tillsammans med [säkrad supportteamet](mailto:support@fusion-universal.com) att lägga till användare i säkrad-plattformen.</span><span class="sxs-lookup"><span data-stu-id="902f5-196">Please work with [Fuse support team](mailto:support@fusion-universal.com) to add the users in the Fuse platform.</span></span>


### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="902f5-197">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="902f5-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="902f5-198">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till säkrad.</span><span class="sxs-lookup"><span data-stu-id="902f5-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Fuse.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="902f5-200">**Om du vill tilldela säkrad Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="902f5-200">**To assign Britta Simon to Fuse, perform the following steps:**</span></span>

1. <span data-ttu-id="902f5-201">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="902f5-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="902f5-203">Välj i listan med program **videoformatet**.</span><span class="sxs-lookup"><span data-stu-id="902f5-203">In the applications list, select **Fuse**.</span></span>

    ![Länken säkrad i listan med program](./media/active-directory-saas-fuse-tutorial/tutorial_fuse_app.png)  

3. <span data-ttu-id="902f5-205">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="902f5-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="902f5-207">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="902f5-207">Click **Add** button.</span></span> <span data-ttu-id="902f5-208">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="902f5-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="902f5-210">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="902f5-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="902f5-211">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="902f5-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="902f5-212">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="902f5-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="902f5-213">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="902f5-213">Test single sign-on</span></span>

<span data-ttu-id="902f5-214">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="902f5-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="902f5-215">När du klickar på panelen säkrad på åtkomstpanelen du ska hämta automatiskt loggat in på ditt säkrad program.</span><span class="sxs-lookup"><span data-stu-id="902f5-215">When you click the Fuse tile in the Access Panel, you should get automatically signed-on to your Fuse application.</span></span>
<span data-ttu-id="902f5-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="902f5-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="902f5-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="902f5-217">Additional resources</span></span>

* [<span data-ttu-id="902f5-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="902f5-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="902f5-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="902f5-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuse-tutorial/tutorial_general_203.png

