---
title: "Självstudier: Integrera Azure Active Directory med vxMaintain | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och vxMaintain."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 841a1066-593c-4603-9abe-f48496d73d10
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: ad87534af448356b8cc80d8ddd278bfb8a9165e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-vxmaintain"></a><span data-ttu-id="6df64-103">Självstudier: Integrera Azure Active Directory med vxMaintain</span><span class="sxs-lookup"><span data-stu-id="6df64-103">Tutorial: Integrate Azure Active Directory with vxMaintain</span></span>

<span data-ttu-id="6df64-104">I kursen får lära du att integrera vxMaintain med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6df64-104">In this tutorial, you learn how to integrate vxMaintain with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6df64-105">Den här integreringen ger flera viktiga fördelar.</span><span class="sxs-lookup"><span data-stu-id="6df64-105">This integration provides several important benefits.</span></span> <span data-ttu-id="6df64-106">Du kan:</span><span class="sxs-lookup"><span data-stu-id="6df64-106">You can:</span></span>

- <span data-ttu-id="6df64-107">Kontrollera i Azure AD som har åtkomst till vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="6df64-107">Control in Azure AD who has access to vxMaintain.</span></span>
- <span data-ttu-id="6df64-108">Ge användarna logga in automatiskt vxMaintain med enkel inloggning (SSO) med hjälp av sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="6df64-108">Enable your users to automatically sign in to vxMaintain with single sign-on (SSO) by using their Azure AD accounts.</span></span>
- <span data-ttu-id="6df64-109">Hantera dina konton i en central plats: Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6df64-109">Manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="6df64-110">Mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6df64-110">To learn more about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6df64-111">Krav</span><span class="sxs-lookup"><span data-stu-id="6df64-111">Prerequisites</span></span>

<span data-ttu-id="6df64-112">För att konfigurera Azure AD-integrering med vxMaintain, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="6df64-112">To configure Azure AD integration with vxMaintain, you need the following items:</span></span>

- <span data-ttu-id="6df64-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6df64-113">An Azure AD subscription</span></span>
- <span data-ttu-id="6df64-114">VxMaintain SSO-aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="6df64-114">A vxMaintain SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6df64-115">När du testar stegen i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6df64-115">When you test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="6df64-116">Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="6df64-116">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="6df64-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6df64-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6df64-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6df64-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6df64-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6df64-119">Scenario description</span></span>
<span data-ttu-id="6df64-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6df64-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="6df64-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6df64-121">The scenario that this tutorial outlines consists of two main building blocks:</span></span>

* <span data-ttu-id="6df64-122">Att lägga till vxMaintain från galleriet</span><span class="sxs-lookup"><span data-stu-id="6df64-122">Adding vxMaintain from the gallery</span></span>
* <span data-ttu-id="6df64-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6df64-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="add-vxmaintain-from-the-gallery"></a><span data-ttu-id="6df64-124">Lägg till vxMaintain från galleriet</span><span class="sxs-lookup"><span data-stu-id="6df64-124">Add vxMaintain from the gallery</span></span>
<span data-ttu-id="6df64-125">Du måste lägga till vxMaintain från galleriet i listan över hanterade SaaS-appar för att konfigurera vxMaintain-integrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6df64-125">To configure the integration of vxMaintain with Azure AD, you need to add vxMaintain from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6df64-126">Lägg till vxMaintain från galleriet genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="6df64-126">To add vxMaintain from the gallery, do the following:</span></span>

1. <span data-ttu-id="6df64-127">I den [Azure-portalen](https://portal.azure.com), i den vänstra rutan, Välj den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="6df64-127">In the [Azure portal](https://portal.azure.com), in the left pane, select the **Azure Active Directory** button.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="6df64-129">Välj **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6df64-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![Fönstret ”företagsprogram”][2]
    
3. <span data-ttu-id="6df64-131">Att lägga till ett program i den **alla program** dialogrutan **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="6df64-131">To add an application, in the **All applications** dialog box, select **New application**.</span></span>

    ![”Det nya programmet” knappen][3]

4. <span data-ttu-id="6df64-133">I sökrutan skriver **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="6df64-133">In the search box, type **vxMaintain**.</span></span>

    ![Listrutan ”enkel inloggning läge”](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_search.png)

5. <span data-ttu-id="6df64-135">Välj i resultatlistan **vxMaintain**, och välj sedan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6df64-135">In the results list, select **vxMaintain**, and then select **Add**.</span></span>

    ![Länken vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="6df64-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6df64-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="6df64-138">I det här avsnittet kan du konfigurera och testa Azure AD SSO med hjälp av vxMaintain, baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6df64-138">In this section, you configure and test Azure AD SSO by using vxMaintain, based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6df64-139">För enkel inloggning ska fungera, Azure AD som behöver veta vxMaintain motsvarighet till Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="6df64-139">For SSO to work, Azure AD needs to know the vxMaintain counterpart to the Azure AD user.</span></span> <span data-ttu-id="6df64-140">Det vill säga måste du upprätta en länk relation mellan Azure AD-användare och motsvarande vxMaintain användaren.</span><span class="sxs-lookup"><span data-stu-id="6df64-140">That is, you must establish a link relationship between the Azure AD user and the corresponding vxMaintain user.</span></span>

<span data-ttu-id="6df64-141">Om du vill upprätta en länk relation, tilldela vxMaintain **användarnamn** värde som Azure AD **användarnamn** värde.</span><span class="sxs-lookup"><span data-stu-id="6df64-141">To establish the link relationship, assign the vxMaintain **user name** value as the Azure AD **Username** value.</span></span>

<span data-ttu-id="6df64-142">Slutför följande byggblock för att konfigurera och testa Azure AD SSO med vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="6df64-142">To configure and test Azure AD SSO by using vxMaintain, complete the following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="6df64-143">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6df64-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="6df64-144">I det här avsnittet kan du både aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet vxMaintain genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="6df64-144">In this section, you can both enable Azure AD SSO in the Azure portal and configure SSO in your vxMaintain application by doing the following:</span></span>

1. <span data-ttu-id="6df64-145">I Azure-portalen på den **vxMaintain** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6df64-145">In the Azure portal, on the **vxMaintain** application integration page, select **Single sign-on**.</span></span>

    ![Kommandot ”enkel inloggning”][4]

2. <span data-ttu-id="6df64-147">Att aktivera enkel inloggning, i den **läget för enkel inloggning** listrutan, Välj **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6df64-147">To enable SSO, in the **Single Sign-on Mode** drop-down list, select **SAML-based Sign-on**.</span></span>
 
    ![Kommandot ”SAML-baserade inloggning”](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_samlbase.png)

3. <span data-ttu-id="6df64-149">Under **vxMaintain domän och URL: er**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="6df64-149">Under **vxMaintain Domain and URLs**, do the following:</span></span>

    ![VxMaintain domän och URL: er](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_url.png)

    <span data-ttu-id="6df64-151">a.</span><span class="sxs-lookup"><span data-stu-id="6df64-151">a.</span></span> <span data-ttu-id="6df64-152">I den **identifierare** skriver en URL som har följande syntax:`https://<company name>.verisae.com`</span><span class="sxs-lookup"><span data-stu-id="6df64-152">In the **Identifier** box, type a URL that has the following syntax: `https://<company name>.verisae.com`</span></span>

    <span data-ttu-id="6df64-153">b.</span><span class="sxs-lookup"><span data-stu-id="6df64-153">b.</span></span> <span data-ttu-id="6df64-154">I den **Reply URL** skriver en URL som har följande syntax:`https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span><span class="sxs-lookup"><span data-stu-id="6df64-154">In the **Reply URL** box, type a URL that has the following syntax: `https://<company name>.verisae.com/DataNett/action/ssoConsume/mobile?_log=true`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6df64-155">Föregående värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6df64-155">The preceding values are not real.</span></span> <span data-ttu-id="6df64-156">Uppdatera dem med den faktiska identifieraren och reply URL.</span><span class="sxs-lookup"><span data-stu-id="6df64-156">Update them with the actual identifier and reply URL.</span></span> <span data-ttu-id="6df64-157">För att få värdena kan kontakta den [vxMaintain supportteamet](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="6df64-157">To obtain the values, contact the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>
 
4. <span data-ttu-id="6df64-158">Under **SAML-signeringscertifikat**väljer **XML-Metadata för**, och spara sedan metadatafilen till datorn.</span><span class="sxs-lookup"><span data-stu-id="6df64-158">Under **SAML Signing Certificate**, select **Metadata XML**, and then save the metadata file to your computer.</span></span>

    ![Avsnittet ”SAML signeringscertifikat”](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_certificate.png) 

5. <span data-ttu-id="6df64-160">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6df64-160">Select **Save**.</span></span>

    ![Knappen Spara](./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6df64-162">Så här konfigurerar du **vxMaintain** SSO skicka den hämtade **XML-Metadata för** filen till den [vxMaintain supportteamet](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="6df64-162">To configure **vxMaintain** SSO, send the downloaded **Metadata XML** file to the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span>

> [!TIP]
> <span data-ttu-id="6df64-163">När du skapar appen kan du läsa en kortare version av föregående anvisningarna i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6df64-163">As you set up the app, you can read a concise version of the preceding instructions in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6df64-164">När du har lagt till appen från den **Active Directory** > **företagsprogram** väljer den **enkel inloggning** fliken och sedan använda den inbäddade dokumentation från den **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6df64-164">After you add the app from the **Active Directory** > **Enterprise Applications** section, select the **Single Sign-On** tab, and then access the embedded documentation from the **Configuration** section.</span></span> 
>
><span data-ttu-id="6df64-165">Mer information om funktionen inbäddade dokumentation finns [hantera enkel inloggning för företagsappar](https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="6df64-165">To learn more about the embedded documentation feature, see [Managing single sign-on for enterprise apps](https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="6df64-166">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6df64-166">Create an Azure AD test user</span></span>
<span data-ttu-id="6df64-167">I det här avsnittet skapar du testanvändare Britta Simon i Azure-portalen genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="6df64-167">In this section, you create test user Britta Simon in the Azure portal by doing the following:</span></span>

![Azure AD-testanvändare][100]

1. <span data-ttu-id="6df64-169">I den **Azure-portalen**, i den vänstra rutan, Välj den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="6df64-169">In the **Azure portal**, in the left pane, select the **Azure Active Directory** button.</span></span>

    ![Knappen ”Azure Active Directory”](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6df64-171">Om du vill visa en lista över användare, gå till **användare och grupper** > **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6df64-171">To display a list of users, go to **Users and groups** > **All users**.</span></span>
    
    <span data-ttu-id="6df64-172">![Länken ”alla användare”](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span><span class="sxs-lookup"><span data-stu-id="6df64-172">![The "All users" link](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_02.png)</span></span>  
    <span data-ttu-id="6df64-173">Den **alla användare** öppnas.</span><span class="sxs-lookup"><span data-stu-id="6df64-173">The **All users** dialog box opens.</span></span> 

3. <span data-ttu-id="6df64-174">Öppna den **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="6df64-174">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6df64-176">I den **användaren** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="6df64-176">In the **User** dialog box, do the following:</span></span>
 
    ![Dialogrutan användare](./media/active-directory-saas-vxmaintain-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6df64-178">a.</span><span class="sxs-lookup"><span data-stu-id="6df64-178">a.</span></span> <span data-ttu-id="6df64-179">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6df64-179">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6df64-180">b.</span><span class="sxs-lookup"><span data-stu-id="6df64-180">b.</span></span> <span data-ttu-id="6df64-181">I den **användarnamn** skriver testanvändare Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="6df64-181">In the **User name** box, type the email address of test user Britta Simon.</span></span>

    <span data-ttu-id="6df64-182">c.</span><span class="sxs-lookup"><span data-stu-id="6df64-182">c.</span></span> <span data-ttu-id="6df64-183">Välj den **visa lösenordet** och anteckna värdet som har genererats i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="6df64-183">Select the **Show Password** check box, and then note the value that was generated in the **Password** box.</span></span>

    <span data-ttu-id="6df64-184">d.</span><span class="sxs-lookup"><span data-stu-id="6df64-184">d.</span></span> <span data-ttu-id="6df64-185">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6df64-185">Select **Create**.</span></span>
 
### <a name="create-a-vxmaintain-test-user"></a><span data-ttu-id="6df64-186">Skapa en testanvändare vxMaintain</span><span class="sxs-lookup"><span data-stu-id="6df64-186">Create a vxMaintain test user</span></span>

<span data-ttu-id="6df64-187">I det här avsnittet skapar du testanvändare Britta Simon i vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="6df64-187">In this section, you create test user Britta Simon in vxMaintain.</span></span> <span data-ttu-id="6df64-188">Om du vill lägga till användare i plattformen vxMaintain arbeta med den [vxMaintain supportteamet](http://www.verisae.com/contact-us).</span><span class="sxs-lookup"><span data-stu-id="6df64-188">To add users in the vxMaintain platform, work with the [vxMaintain support team](http://www.verisae.com/contact-us).</span></span> <span data-ttu-id="6df64-189">Innan du använder SSO, skapa och aktivera användarna.</span><span class="sxs-lookup"><span data-stu-id="6df64-189">Before you use SSO, create and activate the users.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="6df64-190">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="6df64-190">Assign the Azure AD test user</span></span>

<span data-ttu-id="6df64-191">I det här avsnittet kan du aktivera testanvändare Britta Simon att använda Azure SSO genom att bevilja åtkomst till vxMaintain.</span><span class="sxs-lookup"><span data-stu-id="6df64-191">In this section, you enable test user Britta Simon to use Azure SSO by granting access to vxMaintain.</span></span> <span data-ttu-id="6df64-192">Det gör du genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="6df64-192">To do so, do the following:</span></span>

![Testanvändare i listan visningsnamn][200] 

1. <span data-ttu-id="6df64-194">I Azure portal **program** visa, gå till **Directory** Visa > **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6df64-194">In the Azure portal **Applications** view, go to **Directory** view > **Enterprise applications** > **All applications**.</span></span>

    ![Länken ”alla program”][201] 

2. <span data-ttu-id="6df64-196">I den **program** väljer **vxMaintain**.</span><span class="sxs-lookup"><span data-stu-id="6df64-196">In the **Applications** list, select **vxMaintain**.</span></span>

    ![Länken vxMaintain](./media/active-directory-saas-vxmaintain-tutorial/tutorial_vxmaintain_app.png) 

3. <span data-ttu-id="6df64-198">I den vänstra rutan, Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6df64-198">In the left pane, select **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202] 

4. <span data-ttu-id="6df64-200">Välj **Lägg till** och sedan, i den **Lägg uppdrag** väljer **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6df64-200">Select **Add** and then, in the **Add Assignment** pane, select **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][203]

5. <span data-ttu-id="6df64-202">I den **användare och grupper** i dialogrutan den **användare** väljer **Britta Simon**, och välj sedan den **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="6df64-202">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**, and then select the **Select** button.</span></span>

7. <span data-ttu-id="6df64-203">I den **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="6df64-203">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-your-azure-ad-single-sign-on"></a><span data-ttu-id="6df64-204">Testa din Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6df64-204">Test your Azure AD single sign-on</span></span>

<span data-ttu-id="6df64-205">I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6df64-205">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="6df64-206">Att välja den **vxMaintain** panelen på åtkomstpanelen ska logga in i tillämpningsprogrammet vxMaintain automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6df64-206">Selecting the **vxMaintain** tile in the Access Panel should sign you in to your vxMaintain application automatically.</span></span>

<span data-ttu-id="6df64-207">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6df64-207">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6df64-208">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6df64-208">Next steps</span></span>

* [<span data-ttu-id="6df64-209">Lista över självstudier om att integrera SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6df64-209">List of tutorials on integrating SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6df64-210">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6df64-210">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-vxmaintain-tutorial/tutorial_general_203.png

