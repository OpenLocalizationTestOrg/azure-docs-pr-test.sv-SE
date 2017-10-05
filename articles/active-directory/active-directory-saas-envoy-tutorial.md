---
title: "Självstudier: Azure Active Directory-integrering med Envoy | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Envoy."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 71f7afcc-1033-4098-9b7e-4f9f2b26f734
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 49211b35ab3e28e0df914061e7fa623907935638
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-envoy"></a><span data-ttu-id="8c3d9-103">Självstudier: Azure Active Directory-integrering med Envoy</span><span class="sxs-lookup"><span data-stu-id="8c3d9-103">Tutorial: Azure Active Directory integration with Envoy</span></span>

<span data-ttu-id="8c3d9-104">I kursen får lära du att integrera Envoy med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8c3d9-104">In this tutorial, you learn how to integrate Envoy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8c3d9-105">Integrera Envoy med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-105">Integrating Envoy with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8c3d9-106">Du kan styra i Azure AD som har åtkomst till Envoy.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-106">You can control in Azure AD who has access to Envoy.</span></span>
- <span data-ttu-id="8c3d9-107">Du kan aktivera användarna att automatiskt hämta loggat in på Envoy (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-107">You can enable your users to automatically get signed-on to Envoy (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="8c3d9-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="8c3d9-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8c3d9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c3d9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8c3d9-110">Prerequisites</span></span>

<span data-ttu-id="8c3d9-111">Om du vill konfigurera Azure AD-integrering med Envoy behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-111">To configure Azure AD integration with Envoy, you need the following items:</span></span>

- <span data-ttu-id="8c3d9-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8c3d9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8c3d9-113">En Envoy enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8c3d9-113">An Envoy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8c3d9-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8c3d9-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8c3d9-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8c3d9-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8c3d9-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8c3d9-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8c3d9-118">Scenario description</span></span>
<span data-ttu-id="8c3d9-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8c3d9-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8c3d9-121">Att lägga till Envoy från galleriet</span><span class="sxs-lookup"><span data-stu-id="8c3d9-121">Adding Envoy from the gallery</span></span>
2. <span data-ttu-id="8c3d9-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c3d9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-envoy-from-the-gallery"></a><span data-ttu-id="8c3d9-123">Att lägga till Envoy från galleriet</span><span class="sxs-lookup"><span data-stu-id="8c3d9-123">Adding Envoy from the gallery</span></span>
<span data-ttu-id="8c3d9-124">Du måste lägga till Envoy från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Envoy i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-124">To configure the integration of Envoy into Azure AD, you need to add Envoy from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8c3d9-125">**Utför följande steg för att lägga till Envoy från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8c3d9-125">**To add Envoy from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8c3d9-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="8c3d9-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8c3d9-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="8c3d9-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="8c3d9-133">I sökrutan skriver **Envoy**väljer **Envoy** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-133">In the search box, type **Envoy**, select **Envoy** from result panel then click **Add** button to add the application.</span></span>

    ![Envoy i resultatlistan](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8c3d9-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c3d9-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="8c3d9-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Envoy baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-136">In this section, you configure and test Azure AD single sign-on with Envoy based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8c3d9-137">Azure AD måste du känna till användaren i Envoy motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Envoy is to a user in Azure AD.</span></span> <span data-ttu-id="8c3d9-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Envoy upprättas.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-138">In other words, a link relationship between an Azure AD user and the related user in Envoy needs to be established.</span></span>

<span data-ttu-id="8c3d9-139">I Envoy, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-139">In Envoy, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8c3d9-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Envoy, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-140">To configure and test Azure AD single sign-on with Envoy, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8c3d9-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8c3d9-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8c3d9-143">**[Skapa en testanvändare Envoy](#create-an-envoy-test-user)**  – har en motsvarighet för Britta Simon Envoy som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-143">**[Create an Envoy test user](#create-an-envoy-test-user)** - to have a counterpart of Britta Simon in Envoy that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8c3d9-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8c3d9-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8c3d9-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c3d9-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8c3d9-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Envoy.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Envoy application.</span></span>

<span data-ttu-id="8c3d9-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Envoy:**</span><span class="sxs-lookup"><span data-stu-id="8c3d9-148">**To configure Azure AD single sign-on with Envoy, perform the following steps:**</span></span>

1. <span data-ttu-id="8c3d9-149">I Azure-portalen på den **Envoy** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-149">In the Azure portal, on the **Envoy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="8c3d9-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_samlbase.png)

3. <span data-ttu-id="8c3d9-153">På den **Envoy domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-153">On the **Envoy Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Envoy domän med enkel inloggning information](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_url.png)

    <span data-ttu-id="8c3d9-155">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.Envoy.com`</span><span class="sxs-lookup"><span data-stu-id="8c3d9-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.Envoy.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="8c3d9-156">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-156">This value is not real.</span></span> <span data-ttu-id="8c3d9-157">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="8c3d9-158">Kontakta [Envoy klienten supportteamet](https://envoy.com/contact/) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-158">Contact [Envoy Client support team](https://envoy.com/contact/) to get this value.</span></span>

4. <span data-ttu-id="8c3d9-159">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikat...</span><span class="sxs-lookup"><span data-stu-id="8c3d9-159">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate..</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_certificate.png) 

5. <span data-ttu-id="8c3d9-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-envoy-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8c3d9-163">På den **Envoy Configuration** klickar du på **konfigurera Envoy** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-163">On the **Envoy Configuration** section, click **Configure Envoy** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8c3d9-164">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="8c3d9-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Envoy konfiguration](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_configure.png)

7. <span data-ttu-id="8c3d9-166">Logga in på webbplatsen Envoy företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-166">In a different web browser window, log into your Envoy company site as an administrator.</span></span>

8. <span data-ttu-id="8c3d9-167">Klicka på i verktygsfältet högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-167">In the toolbar on the top, click **Settings**.</span></span>

    <span data-ttu-id="8c3d9-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span><span class="sxs-lookup"><span data-stu-id="8c3d9-168">![Envoy](./media/active-directory-saas-envoy-tutorial/ic776782.png "Envoy")</span></span>

9. <span data-ttu-id="8c3d9-169">Klicka på **företagets**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-169">Click **Company**.</span></span>

    <span data-ttu-id="8c3d9-170">![Företagets](./media/active-directory-saas-envoy-tutorial/ic776783.png "företag")</span><span class="sxs-lookup"><span data-stu-id="8c3d9-170">![Company](./media/active-directory-saas-envoy-tutorial/ic776783.png "Company")</span></span>

10. <span data-ttu-id="8c3d9-171">Klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-171">Click **SAML**.</span></span>

    <span data-ttu-id="8c3d9-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="8c3d9-172">![SAML](./media/active-directory-saas-envoy-tutorial/ic776784.png "SAML")</span></span>

11. <span data-ttu-id="8c3d9-173">I den **SAML-autentisering** konfiguration och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-173">In the **SAML Authentication** configuration section, perform the following steps:</span></span>

    <span data-ttu-id="8c3d9-174">![SAML-autentisering](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="8c3d9-174">![SAML authentication](./media/active-directory-saas-envoy-tutorial/ic776785.png "SAML authentication")</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="8c3d9-175">Värdet för HQ plats-ID är automatisk som genereras av programmet.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-175">The value for the HQ location ID is auto generated by the application.</span></span>
    
    <span data-ttu-id="8c3d9-176">a.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-176">a.</span></span> <span data-ttu-id="8c3d9-177">I **fingeravtryck** textruta klistra in den **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-177">In **Fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="8c3d9-178">b.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-178">b.</span></span> <span data-ttu-id="8c3d9-179">Klistra in **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat formuläret Azure-portalen i den **identitet HTTP-URL-PROVIDERN SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-179">Paste **SAML Single Sign-On Service URL** value, which you have copied form the Azure portal into the **IDENTITY PROVIDER HTTP SAML URL** textbox.</span></span>
    
    <span data-ttu-id="8c3d9-180">c.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-180">c.</span></span> <span data-ttu-id="8c3d9-181">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-181">Click **Save changes**.</span></span>

> [!TIP]
> <span data-ttu-id="8c3d9-182">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8c3d9-182">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8c3d9-183">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-183">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8c3d9-184">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8c3d9-184">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8c3d9-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8c3d9-185">Create an Azure AD test user</span></span>

<span data-ttu-id="8c3d9-186">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-186">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="8c3d9-188">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8c3d9-188">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8c3d9-189">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-189">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-envoy-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="8c3d9-191">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-191">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-envoy-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="8c3d9-193">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-193">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-envoy-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="8c3d9-195">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8c3d9-195">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-envoy-tutorial/create_aaduser_04.png)

    <span data-ttu-id="8c3d9-197">a.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-197">a.</span></span> <span data-ttu-id="8c3d9-198">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-198">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8c3d9-199">b.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-199">b.</span></span> <span data-ttu-id="8c3d9-200">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-200">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="8c3d9-201">c.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-201">c.</span></span> <span data-ttu-id="8c3d9-202">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-202">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="8c3d9-203">d.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-203">d.</span></span> <span data-ttu-id="8c3d9-204">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-204">Click **Create**.</span></span>
 
### <a name="create-an-envoy-test-user"></a><span data-ttu-id="8c3d9-205">Skapa en testanvändare Envoy</span><span class="sxs-lookup"><span data-stu-id="8c3d9-205">Create an Envoy test user</span></span>

<span data-ttu-id="8c3d9-206">Det finns inga uppgiften som du kan konfigurera användaretablering till Envoy.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-206">There is no action item for you to configure user provisioning to Envoy.</span></span> <span data-ttu-id="8c3d9-207">När en tilldelad användare försöker logga in på Envoy panelen åtkomst kontrollerar Envoy om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-207">When an assigned user tries to log into Envoy using the access panel, Envoy checks whether the user exists.</span></span> <span data-ttu-id="8c3d9-208">Om det finns inget användarkonto ännu, skapas den automatiskt av Envoy.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-208">If there is no user account available yet, it is automatically created by Envoy.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="8c3d9-209">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8c3d9-209">Assign the Azure AD test user</span></span>

<span data-ttu-id="8c3d9-210">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Envoy.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-210">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Envoy.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="8c3d9-212">**Om du vill tilldela Envoy Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8c3d9-212">**To assign Britta Simon to Envoy, perform the following steps:**</span></span>

1. <span data-ttu-id="8c3d9-213">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-213">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8c3d9-215">Välj i listan med program **Envoy**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-215">In the applications list, select **Envoy**.</span></span>

    ![Länken Envoy i listan med program](./media/active-directory-saas-envoy-tutorial/tutorial_envoy_app.png)  

3. <span data-ttu-id="8c3d9-217">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-217">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="8c3d9-219">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-219">Click **Add** button.</span></span> <span data-ttu-id="8c3d9-220">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="8c3d9-222">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-222">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8c3d9-223">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8c3d9-224">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="8c3d9-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8c3d9-225">Test single sign-on</span></span>

<span data-ttu-id="8c3d9-226">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-226">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8c3d9-227">När du klickar på panelen Envoy på åtkomstpanelen du bör få automatiskt loggat in på ditt Envoy program.</span><span class="sxs-lookup"><span data-stu-id="8c3d9-227">When you click the Envoy tile in the Access Panel, you should get automatically signed-on to your Envoy application.</span></span>
<span data-ttu-id="8c3d9-228">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8c3d9-228">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8c3d9-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8c3d9-229">Additional resources</span></span>

* [<span data-ttu-id="8c3d9-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8c3d9-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8c3d9-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8c3d9-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envoy-tutorial/tutorial_general_203.png

