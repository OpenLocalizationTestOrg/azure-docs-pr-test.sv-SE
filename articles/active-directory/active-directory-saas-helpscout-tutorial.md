---
title: "Självstudier: Azure Active Directory-integrering med hjälp Scout | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och hjälpa Scout."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 84cee39c28a0f7e6b9878441e504131795673020
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a><span data-ttu-id="b566f-103">Självstudier: Azure Active Directory-integrering med hjälp Scout</span><span class="sxs-lookup"><span data-stu-id="b566f-103">Tutorial: Azure Active Directory integration with Help Scout</span></span>

<span data-ttu-id="b566f-104">I kursen får lära du att integrera hjälpa Scout med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b566f-104">In this tutorial, you learn how to integrate Help Scout with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b566f-105">Du kan få följande fördelar från hjälp Scout integrera med Azure AD:</span><span class="sxs-lookup"><span data-stu-id="b566f-105">You get the following benefits from integrating Help Scout with Azure AD:</span></span>

- <span data-ttu-id="b566f-106">Du kan styra vem som har åtkomst till hjälpen Scout i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b566f-106">In Azure AD, you can control who has access to Help Scout.</span></span>
- <span data-ttu-id="b566f-107">Du kan logga in automatiskt Scout hjälpa användarna med enkel inloggning och användarens Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="b566f-107">You can automatically sign in your users to Help Scout by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="b566f-108">Du kan hantera dina konton i en enda central plats och Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b566f-108">You can manage your accounts in one, central location, the Azure portal.</span></span>

<span data-ttu-id="b566f-109">Läs mer om programvara som en tjänst (SaaS) appintegrering med Azure AD i [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b566f-109">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b566f-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b566f-110">Prerequisites</span></span>

<span data-ttu-id="b566f-111">Om du vill konfigurera Azure AD-integrering med hjälp Scout behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b566f-111">To set up Azure AD integration with Help Scout, you need the following items:</span></span>

- <span data-ttu-id="b566f-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b566f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b566f-113">En prenumeration som hjälper Scout med enkel inloggning aktiverad</span><span class="sxs-lookup"><span data-stu-id="b566f-113">A Help Scout subscription, with single sign-on turned on</span></span> 

> [!NOTE]
> <span data-ttu-id="b566f-114">Om du testar stegen i den här kursen rekommenderar vi att du inte testa dem i en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b566f-114">If you test the steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="b566f-115">Rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="b566f-115">Recommendations for testing the steps in this tutorial:</span></span>

- <span data-ttu-id="b566f-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b566f-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="b566f-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [skaffa en kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b566f-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b566f-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b566f-118">Scenario description</span></span>
<span data-ttu-id="b566f-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b566f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="b566f-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b566f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b566f-121">Lägg till hjälp Scout från galleriet.</span><span class="sxs-lookup"><span data-stu-id="b566f-121">Add Help Scout from the gallery.</span></span>
2. <span data-ttu-id="b566f-122">Konfigurera och testa Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b566f-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-help-scout-from-the-gallery"></a><span data-ttu-id="b566f-123">Lägg till hjälp Scout från galleriet</span><span class="sxs-lookup"><span data-stu-id="b566f-123">Add Help Scout from the gallery</span></span>
<span data-ttu-id="b566f-124">Lägg till hjälp Scout i listan över hanterade SaaS-appar för att ställa in att Scout med Azure AD-integrering i galleriet.</span><span class="sxs-lookup"><span data-stu-id="b566f-124">To set up the integration of Help Scout with Azure AD, in the gallery, add Help Scout to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b566f-125">Lägg till hjälp Scout från galleriet:</span><span class="sxs-lookup"><span data-stu-id="b566f-125">To add Help Scout from the gallery:</span></span>

1. <span data-ttu-id="b566f-126">I den [Azure-portalen](https://portal.azure.com), i den vänstra menyn, Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b566f-126">In the [Azure portal](https://portal.azure.com), in the left menu, select **Azure Active Directory**.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="b566f-128">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b566f-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Sidan Enterprise program][2]
    
3. <span data-ttu-id="b566f-130">Om du vill lägga till ett nytt program, Välj **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="b566f-130">To add a new application, select **New application**.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="b566f-132">I sökrutan anger **hjälp Scout**.</span><span class="sxs-lookup"><span data-stu-id="b566f-132">In the search box, enter **Help Scout**.</span></span> <span data-ttu-id="b566f-133">I sökresultaten väljer **hjälp Scout**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b566f-133">In the search results, select **Help Scout**, and then select **Add**.</span></span>

    ![Hjälp Scout i resultatlistan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b566f-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b566f-135">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="b566f-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med hjälp Scout baserat på en användare med namnet *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="b566f-136">In this section, you set up and test Azure AD single sign-on with Help Scout based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="b566f-137">För enkel inloggning ska fungera, Azure AD som behöver veta Azure AD motsvarighet användaren i Hjälp Scout.</span><span class="sxs-lookup"><span data-stu-id="b566f-137">For single sign-on to work, Azure AD needs to know the Azure AD counterpart user in Help Scout.</span></span> <span data-ttu-id="b566f-138">En länk förhållandet mellan en Azure AD-användare och relaterade användaren i Hjälp Scout måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="b566f-138">A link relationship between an Azure AD user and the related user in Help Scout must be established.</span></span>

<span data-ttu-id="b566f-139">Etablera länk-relationen i Hjälp Scout för **användarnamn**, tilldela värdet för den **användarnamn** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b566f-139">To establish the link relationship, in Help Scout, for **Username**, assign the value of the **user name** in Azure AD.</span></span>

<span data-ttu-id="b566f-140">Om du vill konfigurera och testa Azure AD enkel inloggning med hjälp Scout, utför följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="b566f-140">To configure and test Azure AD single sign-on with Help Scout, complete the following tasks:</span></span>

1. <span data-ttu-id="b566f-141">[Konfigurera Azure AD enkel inloggning](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="b566f-141">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="b566f-142">Ställer in en användare att använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b566f-142">Sets up a user to use this feature.</span></span>
2. <span data-ttu-id="b566f-143">[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="b566f-143">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="b566f-144">Testerna Azure AD enkel inloggning med användaren Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b566f-144">Tests Azure AD single sign-on with the user Britta Simon.</span></span>
3. <span data-ttu-id="b566f-145">[Skapa en testanvändare hjälpa Scout](#create-a-help-scout-test-user).</span><span class="sxs-lookup"><span data-stu-id="b566f-145">[Create a Help Scout test user](#create-a-help-scout-test-user).</span></span> <span data-ttu-id="b566f-146">Skapar en motsvarighet för Britta Simon i Hjälp Scout som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b566f-146">Creates a counterpart of Britta Simon in Help Scout that is linked to the Azure AD representation of the user.</span></span>
4. <span data-ttu-id="b566f-147">[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="b566f-147">[Assign the Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="b566f-148">Ställer in Britta Simon använda Azure AD för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b566f-148">Sets up Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b566f-149">[Testa enkel inloggning](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="b566f-149">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="b566f-150">Kontrollerar att konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b566f-150">Verifies that the configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="b566f-151">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b566f-151">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="b566f-152">I det här avsnittet kan du ställa in Azure AD enkel inloggning i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b566f-152">In this section, you set up Azure AD single sign-on in the Azure portal.</span></span> <span data-ttu-id="b566f-153">Sedan kan konfigurera du enkel inloggning i tillämpningsprogrammet att Scout.</span><span class="sxs-lookup"><span data-stu-id="b566f-153">Then, you set up single sign-on in your Help Scout application.</span></span>

<span data-ttu-id="b566f-154">Konfigurera Azure AD enkel inloggning med hjälp Scout:</span><span class="sxs-lookup"><span data-stu-id="b566f-154">To set up Azure AD single sign-on with Help Scout:</span></span>

1. <span data-ttu-id="b566f-155">I Azure-portalen på den **hjälp Scout** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b566f-155">In the Azure portal, on the **Help Scout** application integration page, select **Single sign-on**.</span></span>
 
    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="b566f-157">På den **enkel inloggning** sidan för **läge**väljer **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b566f-157">On the **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. <span data-ttu-id="b566f-159">Under **hjälp Scout domän och URL: er**, om du vill installera programmet i IDP-initierad läge fullständig följande steg:</span><span class="sxs-lookup"><span data-stu-id="b566f-159">Under **Help Scout Domain and URLs**, if you want to set up the application in IDP-initiated mode, complete the following steps:</span></span>

    1. <span data-ttu-id="b566f-160">I den **identifierare** ange en URL som har följande mönster:`urn:auth0:helpscout:<instancename>`</span><span class="sxs-lookup"><span data-stu-id="b566f-160">In the **Identifier** box, enter a URL that has the following pattern: `urn:auth0:helpscout:<instancename>`</span></span>

    2. <span data-ttu-id="b566f-161">I den **Reply URL** ange en URL som har följande mönster:`https://helpscout.auth0.com/login/callback?connection=<instancename>`</span><span class="sxs-lookup"><span data-stu-id="b566f-161">In the **Reply URL** box, enter a URL that has the following pattern: `https://helpscout.auth0.com/login/callback?connection=<instancename>`</span></span>

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. <span data-ttu-id="b566f-163">Om du vill installera programmet i SP-initierad läge, väljer du den **visa avancerade inställningar för URL: en** och gör sedan följande:</span><span class="sxs-lookup"><span data-stu-id="b566f-163">If you want to set up the application in SP-initiated mode, select the **Show advanced URL settings** check box, and then do the following:</span></span>

    * <span data-ttu-id="b566f-164">I den **inloggning URL** ange en URL som har följande format:`https://secure.helpscout.net/members/login/`</span><span class="sxs-lookup"><span data-stu-id="b566f-164">In the **Sign on URL** box, enter a URL that has the following format: `https://secure.helpscout.net/members/login/`</span></span>

    ![Hjälpinformation Scout domän URL: er och enkel inloggning](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > <span data-ttu-id="b566f-166">Värdena i dessa URL: er är bara exempel.</span><span class="sxs-lookup"><span data-stu-id="b566f-166">The values in these URLs are for demonstration only.</span></span> <span data-ttu-id="b566f-167">Uppdatera värdena med faktiska Identifierare och reply-URL.</span><span class="sxs-lookup"><span data-stu-id="b566f-167">Update the values with the actual identifier URL and reply URL.</span></span> <span data-ttu-id="b566f-168">För att få dessa värden kan kontakta [hjälp Scout supportteamet](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="b566f-168">To get these values, contact [Help Scout support team](mailto:help@helpscout.com).</span></span> 

5. <span data-ttu-id="b566f-169">Under **SAML-signeringscertifikat**väljer **XML-Metadata för**, och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b566f-169">Under **SAML Signing Certificate**, select **Metadata XML**, and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. <span data-ttu-id="b566f-171">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b566f-171">Select **Save**.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="b566f-173">Om du vill konfigurera enkel inloggning på sidan hjälp Scout, skicka hämtade metadata XML-filen till den [hjälp Scout supportteamet](mailto:help@helpscout.com).</span><span class="sxs-lookup"><span data-stu-id="b566f-173">To set up single sign-on on the Help Scout side, send the downloaded metadata XML file to the [Help Scout support team](mailto:help@helpscout.com).</span></span> <span data-ttu-id="b566f-174">Support-teamet hjälper Scout gäller den här inställningen så att SAML enkel inloggning anslutningen är korrekt inställda på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="b566f-174">The Help Scout support team applies this setting so that the SAML single sign-on connection is set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="b566f-175">Du kan läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b566f-175">You can read a concise version of these instructions in the [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="b566f-176">När du har lagt till appen genom att välja **Active Directory** > **företagsprogram**, Välj den **enkel inloggning** fliken. Du kan komma åt inbäddade dokumentation i den **Configuration** avsnittet längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="b566f-176">After you add the app by selecting **Active Directory** > **Enterprise Applications**, select the **Single Sign-On** tab. You can access the embedded documentation in the **Configuration** section, at the bottom of the page.</span></span> <span data-ttu-id="b566f-177">Mer information finns i [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="b566f-177">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b566f-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b566f-178">Create an Azure AD test user</span></span>

<span data-ttu-id="b566f-179">I det här avsnittet i Azure-portalen kan du skapa en testanvändare med namnet Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b566f-179">In this section, in the Azure portal, you create a test user named Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="b566f-181">Skapa en testanvändare i Azure AD:</span><span class="sxs-lookup"><span data-stu-id="b566f-181">To create a test user in Azure AD:</span></span>

1. <span data-ttu-id="b566f-182">Välj i Azure-portalen på den vänstra menyn **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b566f-182">In the Azure portal, in the left menu, select **Azure Active Directory**.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="b566f-184">Om du vill visa en lista över användare, Välj **användare och grupper**, och välj sedan **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b566f-184">To display the list of users, select **Users and groups**, and then select **All users**.</span></span>

    ![Välj användare och grupper och välj sedan alla användare](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="b566f-186">Öppna den **användare** dialogrutan överst i den **alla användare** väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b566f-186">To open the **User** dialog box, at the top of the **All Users** page, select **Add**.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="b566f-188">I den **användaren** dialogrutan rutan, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="b566f-188">In the **User** dialog box, complete the following steps:</span></span>

    1. <span data-ttu-id="b566f-189">I den **namn** ange **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b566f-189">In the **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="b566f-190">I den **användarnamn** ange användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="b566f-190">In the **User name** box, enter the email address of user Britta Simon.</span></span>

    3. <span data-ttu-id="b566f-191">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="b566f-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    4. <span data-ttu-id="b566f-192">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b566f-192">Select **Create**.</span></span>

        ![Dialogrutan användare](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a><span data-ttu-id="b566f-194">Skapa en hjälp Scout testanvändare</span><span class="sxs-lookup"><span data-stu-id="b566f-194">Create a Help Scout test user</span></span>

<span data-ttu-id="b566f-195">Syftet med det här avsnittet är att skapa en användare med namnet Britta Simon i Hjälp Scout.</span><span class="sxs-lookup"><span data-stu-id="b566f-195">The object of this section is to create a user named Britta Simon in Help Scout.</span></span> <span data-ttu-id="b566f-196">Hjälp Scout stöder just-in-time (JIT) etablering, som är aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="b566f-196">Help Scout supports just-in-time (JIT) provisioning, which is turned on by default.</span></span>

<span data-ttu-id="b566f-197">I det här avsnittet finns det ingen åtgärd eller åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b566f-197">In this section, there's no action or task to complete.</span></span> <span data-ttu-id="b566f-198">Om en användare inte redan finns i Hjälp Scout, skapas en ny när du försöker komma åt Hjälp Scout.</span><span class="sxs-lookup"><span data-stu-id="b566f-198">If a user doesn't already exist in Help Scout, a new one is created when you attempt to access Help Scout.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="b566f-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b566f-199">Assign the Azure AD test user</span></span>

<span data-ttu-id="b566f-200">I det här avsnittet tillåter användaren Britta Simon att använda Azure AD enkel inloggning genom att bevilja användarkontoåtkomst till hjälpen Scout.</span><span class="sxs-lookup"><span data-stu-id="b566f-200">In this section, you allow the user Britta Simon to use Azure AD single sign-on by granting the user account access to Help Scout.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="b566f-202">Tilldela Britta Simon hjälpa Scout:</span><span class="sxs-lookup"><span data-stu-id="b566f-202">To assign Britta Simon to Help Scout:</span></span>

1. <span data-ttu-id="b566f-203">Öppna vyn program i Azure-portalen och sedan gå till vyn directory.</span><span class="sxs-lookup"><span data-stu-id="b566f-203">In the Azure portal, open the applications view, and then go to the directory view.</span></span> <span data-ttu-id="b566f-204">Välj **företagsprogram**, och välj sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b566f-204">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b566f-206">Välj i listan med program **hjälp Scout**.</span><span class="sxs-lookup"><span data-stu-id="b566f-206">In the applications list, select **Help Scout**.</span></span>

    ![Länken Hjälp Scout i listan med program](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. <span data-ttu-id="b566f-208">Välj i den vänstra menyn **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b566f-208">In the left menu, select **Users and groups**.</span></span>

    ![Länka användare och grupper][202]

4. <span data-ttu-id="b566f-210">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b566f-210">Select **Add**.</span></span> <span data-ttu-id="b566f-211">Klicka sedan på den **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b566f-211">Then, on the **Add Assignment** page, select **Users and groups**.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="b566f-213">På den **användare och grupper** sida i listan över användare, Välj **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b566f-213">On the **Users and groups** page, in the list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="b566f-214">På den **användare och grupper** väljer **Välj**.</span><span class="sxs-lookup"><span data-stu-id="b566f-214">On the **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="b566f-215">På den **Lägg uppdrag** väljer **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="b566f-215">On the **Add Assignment** page, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="b566f-216">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b566f-216">Test single sign-on</span></span>

<span data-ttu-id="b566f-217">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b566f-217">In this section, you test your Azure AD single sign-on configuration by using the access panel.</span></span>

<span data-ttu-id="b566f-218">När du väljer att Scout panelen på åtkomstpanelen bör du vara automatiskt inloggad i tillämpningsprogrammet att Scout.</span><span class="sxs-lookup"><span data-stu-id="b566f-218">When you select the Help Scout tile in the access panel, you should be automatically signed in to your Help Scout application.</span></span>

<span data-ttu-id="b566f-219">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b566f-219">For more information about the access panel, see [Introduction to the access panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b566f-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b566f-220">Additional resources</span></span>

* [<span data-ttu-id="b566f-221">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b566f-221">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b566f-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b566f-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png

