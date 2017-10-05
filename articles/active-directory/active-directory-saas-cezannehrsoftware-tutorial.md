---
title: "Självstudier: Azure Active Directory-integrering med Cezanne HR programvara | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Cezanne HR-programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a><span data-ttu-id="5b0c4-103">Självstudier: Integrera Azure Active Directory med Cezanne HR-programvara</span><span class="sxs-lookup"><span data-stu-id="5b0c4-103">Tutorial: Integrate Azure Active Directory with Cezanne HR software</span></span>

<span data-ttu-id="5b0c4-104">I kursen får lära du att integrera Cezanne HR-program med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5b0c4-104">In this tutorial, you learn how to integrate Cezanne HR software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5b0c4-105">Integrera Cezanne HR-program med Azure AD ger dig följande fördelar.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-105">Integrating Cezanne HR software with Azure AD provides you with the following benefits.</span></span> <span data-ttu-id="5b0c4-106">Du kan:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-106">You can:</span></span>

- <span data-ttu-id="5b0c4-107">Kontrollera i Azure AD som har åtkomst till Cezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-107">Control in Azure AD who has access to Cezanne HR software.</span></span>
- <span data-ttu-id="5b0c4-108">Ge användarna logga in automatiskt Cezanne HR-program med enkel inloggning (SSO) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-108">Enable your users to automatically sign in to Cezanne HR software with single sign-on (SSO) with their Azure AD accounts.</span></span>
- <span data-ttu-id="5b0c4-109">Hantera dina konton i en central plats: Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-109">Manage your accounts in one central location: the Azure portal.</span></span>

<span data-ttu-id="5b0c4-110">Läs mer om programvara som en tjänst (SaaS) appintegrering med Azure AD i [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5b0c4-110">To learn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and SSO with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b0c4-111">Krav</span><span class="sxs-lookup"><span data-stu-id="5b0c4-111">Prerequisites</span></span>

<span data-ttu-id="5b0c4-112">För att konfigurera Azure AD-integrering med Cezanne HR programvara, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-112">To configure Azure AD integration with Cezanne HR software, you need the following items:</span></span>

- <span data-ttu-id="5b0c4-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5b0c4-113">An Azure AD subscription</span></span>
- <span data-ttu-id="5b0c4-114">Cezanne HR-programvara SSO-aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="5b0c4-114">A Cezanne HR software SSO-enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5b0c4-115">Om du vill testa stegen i den här självstudiekursen, rekommenderar vi att du inte använder en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-115">To test the steps in this tutorial, we recommend that you do not use a production environment.</span></span>

<span data-ttu-id="5b0c4-116">Följ dessa rekommendationer för att testa stegen i den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-116">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="5b0c4-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-117">Don't use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5b0c4-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5b0c4-118">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5b0c4-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5b0c4-119">Scenario description</span></span>
<span data-ttu-id="5b0c4-120">I kursen får testa du Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-120">In this tutorial, you test Azure AD SSO in a test environment.</span></span> 

<span data-ttu-id="5b0c4-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

* <span data-ttu-id="5b0c4-122">Lägga till Cezanne HR programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="5b0c4-122">Adding Cezanne HR software from the gallery</span></span>
* <span data-ttu-id="5b0c4-123">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="5b0c4-123">Configuring and testing Azure AD SSO</span></span>

## <a name="add-cezanne-hr-software-from-the-gallery"></a><span data-ttu-id="5b0c4-124">Lägg till Cezanne HR programvara från galleriet</span><span class="sxs-lookup"><span data-stu-id="5b0c4-124">Add Cezanne HR software from the gallery</span></span>
<span data-ttu-id="5b0c4-125">Lägg till Cezanne HR programvara från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Cezanne HR-program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-125">To configure the integration of Cezanne HR software into Azure AD, add Cezanne HR software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5b0c4-126">Lägg till Cezanne HR programvara från galleriet genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-126">To add Cezanne HR software from the gallery, do the following:</span></span>

1. <span data-ttu-id="5b0c4-127">I den  **[Azure-portalen](https://portal.azure.com)**, i den vänstra rutan, Välj den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-127">In the **[Azure portal](https://portal.azure.com)**, in the left pane, select the **Azure Active Directory** button.</span></span> 

    ![Knappen ”Azure Active Directory”][1]

2. <span data-ttu-id="5b0c4-129">Välj **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-129">Select **Enterprise applications** > **All applications**.</span></span>

    ![Länken ”alla program”][2]
    
3. <span data-ttu-id="5b0c4-131">Att lägga till ett nytt program överst i den **alla program** dialogrutan **nytt program**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-131">To add a new application, at the top of the **All applications** dialog box, select **New application**.</span></span>

    ![”Det nya programmet” knappen][3]

4. <span data-ttu-id="5b0c4-133">I sökrutan skriver **Cezanne HR programvara**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-133">In the search box, type **Cezanne HR Software**.</span></span>

    ![Sökrutan](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. <span data-ttu-id="5b0c4-135">Välj i resultatlistan **Cezanne HR programvara** och välj sedan den **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-135">In the results list, select **Cezanne HR Software** and then select the **Add** button to add the application.</span></span>

    ![Resultatlistan](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="5b0c4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5b0c4-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="5b0c4-138">I det här avsnittet kan du konfigurera och testa Azure AD SSO med Cezanne HR programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-138">In this section, you configure and test Azure AD SSO with Cezanne HR software based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5b0c4-139">För enkel inloggning ska fungera, Azure AD som behöver veta Cezanne HR programvarumotsvarighet till Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-139">For SSO to work, Azure AD needs to know the Cezanne HR software counterpart to the Azure AD user.</span></span> <span data-ttu-id="5b0c4-140">Med andra ord måste du upprätta en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Cezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-140">In other words, you must establish a link relationship between an Azure AD user and the related user in the Cezanne HR software.</span></span>

<span data-ttu-id="5b0c4-141">Tilldela Cezanne HR-programvara för att upprätta länken relationen, **användarnamn** värde som Azure AD **användarnamn** värde.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-141">To establish the link relationship, assign the Cezanne HR software **user name** value as the Azure AD **Username** value.</span></span>

<span data-ttu-id="5b0c4-142">Slutför följande byggblock för att konfigurera och testa Azure AD SSO med Cezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-142">To configure and test Azure AD SSO by using Cezanne HR software, complete the following building blocks.</span></span>

### <a name="configure-azure-ad-sso"></a><span data-ttu-id="5b0c4-143">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5b0c4-143">Configure Azure AD SSO</span></span>

<span data-ttu-id="5b0c4-144">I det här avsnittet kan du aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i Cezanne HR programmet genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-144">In this section, you can enable Azure AD SSO in the Azure portal and configure SSO in your Cezanne HR software application by doing the following:</span></span>

1. <span data-ttu-id="5b0c4-145">I Azure-portalen på den **Cezanne HR programvara** programmet integration anger **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-145">In the Azure portal, on the **Cezanne HR Software** application integration page, select **Single sign-on**.</span></span>

    ![Kommandot ”enkel inloggning”][4]

2. <span data-ttu-id="5b0c4-147">Att aktivera enkel inloggning, i den **enkel inloggning** dialogrutan markerar du den **läge** som **SAML-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-147">To enable SSO, in the **Single sign-on** dialog box, select the **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Rutan ”läge”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. <span data-ttu-id="5b0c4-149">Under **Cezanne HR programvara domän och URL: er**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-149">Under **Cezanne HR Software Domain and URLs**, do the following:</span></span>

    ![Avsnittet ”Cezanne HR programvara domän och URL: er”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    <span data-ttu-id="5b0c4-151">a.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-151">a.</span></span> <span data-ttu-id="5b0c4-152">I den **inloggnings-URL** skriver en URL som har följande syntax:`https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="5b0c4-152">In the **Sign-on URL** box, type a URL that has the following syntax: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`</span></span>

    <span data-ttu-id="5b0c4-153">b.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-153">b.</span></span> <span data-ttu-id="5b0c4-154">I den **Reply URL** skriver en URL som har följande syntax:`https://w3.cezanneondemand.com:443/<tenantid>`</span><span class="sxs-lookup"><span data-stu-id="5b0c4-154">In the **Reply URL** box, type a URL that has the following syntax: `https://w3.cezanneondemand.com:443/<tenantid>`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="5b0c4-155">Föregående värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-155">The preceding values are not real.</span></span> <span data-ttu-id="5b0c4-156">Uppdatera dem med faktiska reply-Webbadressen och Webbadressen för inloggning.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-156">Update them with the actual reply URL and the sign-on URL.</span></span> <span data-ttu-id="5b0c4-157">För att få värdena kan kontakta den [Cezanne HR programvara klienten supportteamet](mailto:info@cezannehr.com).</span><span class="sxs-lookup"><span data-stu-id="5b0c4-157">To obtain the values, contact the [Cezanne HR software client support team](mailto:info@cezannehr.com).</span></span>

4. <span data-ttu-id="5b0c4-158">Under **SAML-signeringscertifikat**väljer **certifikat (Base64)**, och sedan spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-158">Under **SAML Signing Certificate**, select **Certificate (Base64)**, and then save the certificate file on your computer.</span></span>

    ![Avsnittet ”SAML signeringscertifikat”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. <span data-ttu-id="5b0c4-160">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-160">Select **Save**.</span></span>

    ![Knappen ”Spara”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="5b0c4-162">Under **Cezanne HR programvarukonfiguration**väljer **konfigurera Cezanne HR programvara** att öppna den **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-162">Under **Cezanne HR Software Configuration**, select **Configure Cezanne HR Software** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="5b0c4-163">Kopiera den **SAML enhets-ID** och **SAML enkel inloggning** URL från den **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-163">Copy the **SAML Entity ID** and **SAML Single Sign-On Service** URL from the **Quick Reference** section.</span></span>

    ![I avsnittet ”Cezanne HR programvarukonfiguration”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. <span data-ttu-id="5b0c4-165">Logga in på din Cezanne HR-klient för programvara som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-165">In a different web browser window, sign on to your Cezanne HR software tenant as an administrator.</span></span>

8. <span data-ttu-id="5b0c4-166">I den vänstra rutan, Välj **systeminställningarna**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-166">In the left pane, select **System Setup**.</span></span> <span data-ttu-id="5b0c4-167">Välj **säkerhetsinställningar** > **enkel inloggning Configuration**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-167">Select **Security Settings** > **Single Sign-On Configuration**.</span></span>

    ![Länken ”enkel inloggning-konfiguration”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. <span data-ttu-id="5b0c4-169">I den **Tillåt användare att logga in med följande tjänster för enkel inloggning (SSO)** väljer den **SAML 2.0** kryssrutan och välj den **Advanced Configuration** alternativet.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-169">In the **Allow users to log in using the following Single Sign-On (SSO) services** pane, select the **SAML 2.0** check box and select the **Advanced Configuration** option.</span></span>

    ![Enkel inloggning services-alternativ](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. <span data-ttu-id="5b0c4-171">Välj **lägga till nya**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-171">Select **Add New**.</span></span>

    ![Knappen ”Lägg till ny”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. <span data-ttu-id="5b0c4-173">Under **SAML 2.0 identitetsleverantörer**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-173">Under **SAML 2.0 Identity Providers**, do the following:</span></span>

    ![Avsnittet ”identitetsleverantörer SAML 2.0”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    <span data-ttu-id="5b0c4-175">a.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-175">a.</span></span> <span data-ttu-id="5b0c4-176">I den **visningsnamn** anger du namnet på din identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-176">In the **Display Name** box, enter the name of your identity provider.</span></span>

    <span data-ttu-id="5b0c4-177">b.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-177">b.</span></span> <span data-ttu-id="5b0c4-178">I den **identifiering** klistra in den **SAML enhets-ID** som du kopierade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-178">In the **Entity Identifier** box, paste the **SAML Entity ID** that you copied from the Azure portal.</span></span> 

    <span data-ttu-id="5b0c4-179">c.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-179">c.</span></span> <span data-ttu-id="5b0c4-180">I den **SAML bindning** väljer **efter**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-180">In the **SAML Binding** list box, select **POST**.</span></span>

    <span data-ttu-id="5b0c4-181">d.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-181">d.</span></span> <span data-ttu-id="5b0c4-182">I den **säkerhetstoken tjänstslutpunkten** klistra in den **SAML enkel inloggning** URL som du kopierade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-182">In the **Security Token Service Endpoint** box, paste the **SAML Single Sign-On Service** URL that you copied from the Azure portal.</span></span> 
    
    <span data-ttu-id="5b0c4-183">e.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-183">e.</span></span> <span data-ttu-id="5b0c4-184">I den **användarnamn ID-attributet** ange `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-184">In the **User ID Attribute Name** box, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="5b0c4-185">f.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-185">f.</span></span> <span data-ttu-id="5b0c4-186">Ladda upp det hämta certifikatet från Azure AD, Välj den **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-186">To upload the downloaded certificate from Azure AD, select the **Upload** button.</span></span>
    
    <span data-ttu-id="5b0c4-187">g.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-187">g.</span></span> <span data-ttu-id="5b0c4-188">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-188">Select **OK**.</span></span> 

12. <span data-ttu-id="5b0c4-189">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-189">Select **Save**.</span></span>

    ![Knappen ”Spara”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> <span data-ttu-id="5b0c4-191">När du skapar appen kan du läsa en kortare version av föregående anvisningarna i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5b0c4-191">As you set up the app, you can read a concise version of the preceding instructions in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="5b0c4-192">När du har lagt till appen från den **Active Directory** > **företagsprogram** väljer den **enkel inloggning** fliken. Komma åt inbäddade dokumentationen från den **Configuration** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-192">After you add the app from the **Active Directory** > **Enterprise applications** section, select the **Single sign-on** tab. Then access the embedded documentation from the **Configuration** section.</span></span> 

<span data-ttu-id="5b0c4-193">Mer information om funktionen inbäddade dokumentation finns [Azure AD inbäddade dokumentationen]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="5b0c4-193">To learn more about the embedded documentation feature, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5b0c4-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5b0c4-194">Create an Azure AD test user</span></span>
<span data-ttu-id="5b0c4-195">I det här avsnittet skapar du testanvändare Britta Simon i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-195">In this section, you create test user Britta Simon in the Azure portal.</span></span>

![Testanvändare Britta Simon][100]

<span data-ttu-id="5b0c4-197">Om du vill skapa en testanvändare i Azure AD, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-197">To create a test user in Azure AD, do the following:</span></span>

1. <span data-ttu-id="5b0c4-198">I den **Azure-portalen**, i den vänstra rutan, Välj den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-198">In the **Azure portal**, in the left pane, select the **Azure Active Directory** button.</span></span>

    ![Knappen ”Azure Active Directory”](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5b0c4-200">Om du vill visa en lista över användare, Välj **användare och grupper** > **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-200">To display the list of users, select **Users and groups** > **All users**.</span></span>
    
    ![Länken ”alla användare”](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    <span data-ttu-id="5b0c4-202">Den **alla användare** öppnas.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-202">The **All users** dialog box opens.</span></span>

3. <span data-ttu-id="5b0c4-203">Öppna den **användare** dialogrutan **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-203">To open the **User** dialog box, select **Add**.</span></span>
 
    ![Knappen ”Lägg till”](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5b0c4-205">I den **användaren** dialogrutan Gör följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-205">In the **User** dialog box, do the following:</span></span>
 
    ![Dialogrutan ”användare”](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5b0c4-207">a.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-207">a.</span></span> <span data-ttu-id="5b0c4-208">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-208">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5b0c4-209">b.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-209">b.</span></span> <span data-ttu-id="5b0c4-210">I den **användarnamn** Skriv användaren Britta Simon **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-210">In the **User name** box, type user Britta Simon's **email address**.</span></span>

    <span data-ttu-id="5b0c4-211">c.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-211">c.</span></span> <span data-ttu-id="5b0c4-212">Välj den **visa lösenordet** och anteckna värdet som har genererats i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-212">Select the **Show Password** check box, and then note the value that was generated in the **Password** box.</span></span>

    <span data-ttu-id="5b0c4-213">d.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-213">d.</span></span> <span data-ttu-id="5b0c4-214">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-214">Select **Create**.</span></span>
 
### <a name="create-a-cezanne-hr-software-test-user"></a><span data-ttu-id="5b0c4-215">Skapa en testanvändare för Cezanne HR-programvara</span><span class="sxs-lookup"><span data-stu-id="5b0c4-215">Create a Cezanne HR software test user</span></span>

<span data-ttu-id="5b0c4-216">Om du vill aktivera Azure AD-användare att logga in på Cezanne HR programvara, måste de etableras i Cezanne HR programvara.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-216">To enable Azure AD users to sign in to Cezanne HR software, they must be provisioned into Cezanne HR software.</span></span> <span data-ttu-id="5b0c4-217">När det gäller Cezanne HR-program är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-217">In the case of Cezanne HR software, provisioning is a manual task.</span></span>

<span data-ttu-id="5b0c4-218">Konfigurera ett användarkonto genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-218">Provision a user account by doing the following:</span></span>

1.  <span data-ttu-id="5b0c4-219">Logga in på webbplatsen Cezanne HR programvara företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-219">Sign in to your Cezanne HR software company site as an administrator.</span></span>

2.  <span data-ttu-id="5b0c4-220">I den vänstra rutan, Välj **systeminställningarna** > **hantera användare** > **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-220">In the left pane, select **System Setup** > **Manage Users** > **Add New User**.</span></span>

    <span data-ttu-id="5b0c4-221">![Länken ”Lägg till nya användare”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="5b0c4-221">![The "Add New User" link](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")</span></span>

3.  <span data-ttu-id="5b0c4-222">Under **detaljer**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-222">Under **Person Details**, do the following:</span></span>

    <span data-ttu-id="5b0c4-223">![Avsnittet ”Person information”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="5b0c4-223">![The "Person Details" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")</span></span>
    
    <span data-ttu-id="5b0c4-224">a.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-224">a.</span></span> <span data-ttu-id="5b0c4-225">Ange **intern användare** som **OFF**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-225">Set **Internal User** as **OFF**.</span></span>
    
    <span data-ttu-id="5b0c4-226">b.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-226">b.</span></span> <span data-ttu-id="5b0c4-227">I den **Förnamn** Skriv användarens förnamn, till exempel **Britta**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-227">In the **First Name** box, type the user's first name, for example, **Britta**.</span></span>  
 
    <span data-ttu-id="5b0c4-228">c.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-228">c.</span></span> <span data-ttu-id="5b0c4-229">I den **efternamn** Skriv användarens efternamn, till exempel **Simon**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-229">In the **Last Name** box, type the user's last name, for example, **Simon**.</span></span>
    
    <span data-ttu-id="5b0c4-230">d.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-230">d.</span></span> <span data-ttu-id="5b0c4-231">I den **e-post** Skriv användarens e-postadress, till exempel Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-231">In the **E-mail** box, type the user's email address, for example, Brittasimon@contoso.com.</span></span>

4.  <span data-ttu-id="5b0c4-232">Under **kontoinformation**, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="5b0c4-232">Under **Account Information**, do the following:</span></span>

    <span data-ttu-id="5b0c4-233">![Avsnittet ”kontoinformation”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="5b0c4-233">![The "Account Information" section](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")</span></span>
    
    <span data-ttu-id="5b0c4-234">a.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-234">a.</span></span> <span data-ttu-id="5b0c4-235">I den **användarnamn** Skriv användarens e-postadress, till exempel Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-235">In the **Username** box, type the user's email address, for example, Brittasimon@contoso.com.</span></span>
    
    <span data-ttu-id="5b0c4-236">b.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-236">b.</span></span> <span data-ttu-id="5b0c4-237">I den **lösenord** Skriv användarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-237">In the **Password** box, type the user's password.</span></span>
    
    <span data-ttu-id="5b0c4-238">c.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-238">c.</span></span> <span data-ttu-id="5b0c4-239">I den **säkerhetsroll** väljer **HR Professional**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-239">In the **Security Role** box, select **HR Professional**.</span></span>
    
    <span data-ttu-id="5b0c4-240">d.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-240">d.</span></span> <span data-ttu-id="5b0c4-241">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-241">Select **OK**.</span></span>

5. <span data-ttu-id="5b0c4-242">På den **enkel inloggning** fliken den **SAML 2.0 identifierare** väljer **Lägg till ny**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-242">On the **Single sign-on** tab, in the **SAML 2.0 Identifiers** section, select **Add New**.</span></span>

    <span data-ttu-id="5b0c4-243">![Knappen ”Lägg till ny”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "användare")</span><span class="sxs-lookup"><span data-stu-id="5b0c4-243">![The "Add New" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "User")</span></span>

6. <span data-ttu-id="5b0c4-244">I den **identitetsleverantör** Välj identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-244">In the **Identity Provider** list box, select your identity provider.</span></span> <span data-ttu-id="5b0c4-245">I den **användar-ID** ange e-postadressen för test användaren Britta Simons konto.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-245">In the **User Identifier** box, enter the email address for test user Britta Simon's account.</span></span>

    <span data-ttu-id="5b0c4-246">![Rutorna ”identitetsleverantör” och ”User ID”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "användare")</span><span class="sxs-lookup"><span data-stu-id="5b0c4-246">![The "Identity Provider" and "User Identifier" boxes](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "User")</span></span>
    
7. <span data-ttu-id="5b0c4-247">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-247">Select **Save**.</span></span>

    <span data-ttu-id="5b0c4-248">![Knappen ”Spara”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "användare")</span><span class="sxs-lookup"><span data-stu-id="5b0c4-248">![The "Save" button](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "User")</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="5b0c4-249">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="5b0c4-249">Assign the Azure AD test user</span></span>

<span data-ttu-id="5b0c4-250">I det här avsnittet kan du aktivera testanvändare Britta Simon att använda Azure SSO genom att bevilja åtkomst till Cezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-250">In this section, you enable test user Britta Simon to use Azure SSO by granting access to Cezanne HR software.</span></span>

![Testa användaråtkomst][200] 

1. <span data-ttu-id="5b0c4-252">Öppna program och sedan gå till vyn katalog i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-252">In the Azure portal, open the applications view and then go to the directory view.</span></span> <span data-ttu-id="5b0c4-253">Välj **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-253">Select **Enterprise applications** > **All applications**.</span></span>

    ![Länken ”alla program”][201] 

2. <span data-ttu-id="5b0c4-255">Välj i listan med program **Cezanne HR programvara**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-255">In the applications list, select **Cezanne HR Software**.</span></span>

    ![Listan med ”program”](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. <span data-ttu-id="5b0c4-257">Välj på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-257">In the menu on the left, select **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5b0c4-259">Välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-259">Select **Add**.</span></span> <span data-ttu-id="5b0c4-260">I den **Lägg uppdrag** dialogrutan **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-260">Then in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![”Användare och grupper” länk][203]

5. <span data-ttu-id="5b0c4-262">I den **användare och grupper** i dialogrutan den **användare** väljer **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-262">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="5b0c4-263">I den **användare och grupper** dialogrutan **Välj**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-263">In the **Users and groups** dialog box, select **Select**.</span></span>

7. <span data-ttu-id="5b0c4-264">I den **Lägg uppdrag** dialogrutan **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-264">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-sso"></a><span data-ttu-id="5b0c4-265">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5b0c4-265">Test SSO</span></span>

<span data-ttu-id="5b0c4-266">I det här avsnittet testa du konfigurationen av Azure AD SSO med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-266">In this section, you test your Azure AD SSO configuration by using the Access Panel.</span></span>

<span data-ttu-id="5b0c4-267">När du väljer panelen Cezanne HR programvara på åtkomstpanelen kan logga du in automatiskt i tillämpningsprogrammet Cezanne HR-programvara.</span><span class="sxs-lookup"><span data-stu-id="5b0c4-267">When you select the Cezanne HR software tile in the Access Panel, you sign on automatically to your Cezanne HR software application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b0c4-268">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5b0c4-268">Next steps</span></span>

* [<span data-ttu-id="5b0c4-269">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5b0c4-269">List of tutorials on how to integrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5b0c4-270">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5b0c4-270">What is application access and SSO with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png

