---
title: "Självstudier: Azure Active Directory-integrering med Evernote | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Evernote."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: be94152a84bbbeacb623d7dd8b540e3981931a8e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a><span data-ttu-id="a0efb-103">Självstudier: Azure Active Directory-integrering med Evernote</span><span class="sxs-lookup"><span data-stu-id="a0efb-103">Tutorial: Azure Active Directory integration with Evernote</span></span>

<span data-ttu-id="a0efb-104">I kursen får lära du att integrera Evernote med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a0efb-104">In this tutorial, you learn how to integrate Evernote with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a0efb-105">Integrera Evernote med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a0efb-105">Integrating Evernote with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a0efb-106">Du kan styra i Azure AD som har åtkomst till Evernote.</span><span class="sxs-lookup"><span data-stu-id="a0efb-106">You can control in Azure AD who has access to Evernote.</span></span>
- <span data-ttu-id="a0efb-107">Du kan aktivera användarna att automatiskt hämta loggat in på Evernote (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="a0efb-107">You can enable your users to automatically get signed-on to Evernote (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a0efb-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0efb-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="a0efb-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a0efb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0efb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a0efb-110">Prerequisites</span></span>

<span data-ttu-id="a0efb-111">För att konfigurera Azure AD-integrering med Evernote, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a0efb-111">To configure Azure AD integration with Evernote, you need the following items:</span></span>

- <span data-ttu-id="a0efb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a0efb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a0efb-113">En Evernote enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a0efb-113">An Evernote single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a0efb-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0efb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a0efb-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a0efb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a0efb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a0efb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a0efb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a0efb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a0efb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a0efb-118">Scenario description</span></span>
<span data-ttu-id="a0efb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0efb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a0efb-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a0efb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a0efb-121">Att lägga till Evernote från galleriet</span><span class="sxs-lookup"><span data-stu-id="a0efb-121">Adding Evernote from the gallery</span></span>
2. <span data-ttu-id="a0efb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0efb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evernote-from-the-gallery"></a><span data-ttu-id="a0efb-123">Att lägga till Evernote från galleriet</span><span class="sxs-lookup"><span data-stu-id="a0efb-123">Adding Evernote from the gallery</span></span>
<span data-ttu-id="a0efb-124">Du måste lägga till Evernote från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Evernote i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a0efb-124">To configure the integration of Evernote into Azure AD, you need to add Evernote from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a0efb-125">**Utför följande steg för att lägga till Evernote från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a0efb-125">**To add Evernote from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a0efb-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a0efb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="a0efb-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a0efb-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="a0efb-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0efb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="a0efb-133">I sökrutan skriver **Evernote**väljer **Evernote** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a0efb-133">In the search box, type **Evernote**, select **Evernote** from result panel then click **Add** button to add the application.</span></span>

    ![Evernote i resultatlistan](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a0efb-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0efb-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a0efb-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Evernote baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a0efb-136">In this section, you configure and test Azure AD single sign-on with Evernote based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a0efb-137">Azure AD måste du känna till användaren i Evernote motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a0efb-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Evernote is to a user in Azure AD.</span></span> <span data-ttu-id="a0efb-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Evernote upprättas.</span><span class="sxs-lookup"><span data-stu-id="a0efb-138">In other words, a link relationship between an Azure AD user and the related user in Evernote needs to be established.</span></span>

<span data-ttu-id="a0efb-139">I Evernote, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a0efb-139">In Evernote, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a0efb-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Evernote, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a0efb-140">To configure and test Azure AD single sign-on with Evernote, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a0efb-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a0efb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a0efb-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a0efb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a0efb-143">**[Skapa en testanvändare Evernote](#create-an-evernote-test-user)**  – du har en motsvarighet för Britta Simon i Evernote som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a0efb-143">**[Create an Evernote test user](#create-an-evernote-test-user)** - to have a counterpart of Britta Simon in Evernote that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a0efb-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a0efb-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a0efb-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a0efb-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a0efb-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0efb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a0efb-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Evernote program.</span><span class="sxs-lookup"><span data-stu-id="a0efb-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Evernote application.</span></span>

<span data-ttu-id="a0efb-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Evernote:**</span><span class="sxs-lookup"><span data-stu-id="a0efb-148">**To configure Azure AD single sign-on with Evernote, perform the following steps:**</span></span>

1. <span data-ttu-id="a0efb-149">I Azure-portalen på den **Evernote** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-149">In the Azure portal, on the **Evernote** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="a0efb-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a0efb-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. <span data-ttu-id="a0efb-153">På den **Evernote domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i IDP initierade läge:</span><span class="sxs-lookup"><span data-stu-id="a0efb-153">On the **Evernote Domain and URLs** section, perform the following steps if you wish to configure the application in IDP initiated mode:</span></span>

    ![URL: er och Evernote domän med enkel inloggning information](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    <span data-ttu-id="a0efb-155">I den **identifierare** textruta anger du URL:`https://www.evernote.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="a0efb-155">In the **Identifier** textbox, type the URL: `https://www.evernote.com/saml2`</span></span>

4. <span data-ttu-id="a0efb-156">Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="a0efb-156">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![URL: er och Evernote domän med enkel inloggning information](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    <span data-ttu-id="a0efb-158">I den **inloggning URL** textruta anger du URL:`https://www.evernote.com/Login.action`</span><span class="sxs-lookup"><span data-stu-id="a0efb-158">In the **Sign on URL** textbox, type the URL: `https://www.evernote.com/Login.action`</span></span>   

5. <span data-ttu-id="a0efb-159">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a0efb-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. <span data-ttu-id="a0efb-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0efb-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="a0efb-163">På den **Evernote Configuration** klickar du på **konfigurera Evernote** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a0efb-163">On the **Evernote Configuration** section, click **Configure Evernote** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a0efb-164">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a0efb-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Evernote konfiguration](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. <span data-ttu-id="a0efb-166">Logga in på webbplatsen Evernote företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a0efb-166">In a different web browser window, log into your Evernote company site as an administrator.</span></span>

9. <span data-ttu-id="a0efb-167">Gå till **Admin Console**</span><span class="sxs-lookup"><span data-stu-id="a0efb-167">Go to **'Admin Console'**</span></span>

    ![Administrationskonsolen](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. <span data-ttu-id="a0efb-169">Från den **Admin Console**, gå till **”säkerhet”** och välj **' Single Sign-On-**</span><span class="sxs-lookup"><span data-stu-id="a0efb-169">From the **'Admin Console'**, go to **‘Security’** and select **‘Single Sign-On’**</span></span>

    ![SSO-inställning](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. <span data-ttu-id="a0efb-171">Konfigurera följande värden:</span><span class="sxs-lookup"><span data-stu-id="a0efb-171">Configure the following values:</span></span>

    ![Certifikat-inställning](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    <span data-ttu-id="a0efb-173">a.</span><span class="sxs-lookup"><span data-stu-id="a0efb-173">a.</span></span>  <span data-ttu-id="a0efb-174">**Aktivera enkel inloggning:** enkel inloggning är aktiverad som standard (klicka på **inaktivera enkel inloggning** att ta bort kravet SSO)</span><span class="sxs-lookup"><span data-stu-id="a0efb-174">**Enable SSO:** SSO is enabled by default (Click **Disable Single Sign-on** to remove the SSO requirement)</span></span>

    <span data-ttu-id="a0efb-175">b.</span><span class="sxs-lookup"><span data-stu-id="a0efb-175">b.</span></span> <span data-ttu-id="a0efb-176">Klistra in **SAML enkel inloggning Tjänstwebbadress** -värde som du har kopierat från Azure-portalen i den **SAML HTTP-begäran URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0efb-176">Paste **SAML Single sign-on Service URL** value, which you have copied from the Azure portal into the **SAML HTTP Request URL** textbox.</span></span>

    <span data-ttu-id="a0efb-177">c.</span><span class="sxs-lookup"><span data-stu-id="a0efb-177">c.</span></span> <span data-ttu-id="a0efb-178">Öppna hämtade certifikatet från Azure AD i en anteckningar och kopiera innehållet inklusive ”BEGIN CERTIFICATE” och ”END CERTIFICATE” och klistrar in det i den **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0efb-178">Open the downloaded certificate from Azure AD in a notepad and copy the content including "BEGIN CERTIFICATE" and "END CERTIFICATE" and paste it into the **X.509 Certificate** textbox.</span></span> 

    <span data-ttu-id="a0efb-179">d.Click **spara ändringar**</span><span class="sxs-lookup"><span data-stu-id="a0efb-179">d.Click **Save Changes**</span></span>

> [!TIP]
> <span data-ttu-id="a0efb-180">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="a0efb-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a0efb-181">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="a0efb-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a0efb-182">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a0efb-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a0efb-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a0efb-183">Create an Azure AD test user</span></span>

<span data-ttu-id="a0efb-184">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a0efb-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="a0efb-186">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a0efb-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a0efb-187">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0efb-187">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a0efb-189">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-189">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a0efb-191">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0efb-191">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a0efb-193">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a0efb-193">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    <span data-ttu-id="a0efb-195">a.</span><span class="sxs-lookup"><span data-stu-id="a0efb-195">a.</span></span> <span data-ttu-id="a0efb-196">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-196">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a0efb-197">b.</span><span class="sxs-lookup"><span data-stu-id="a0efb-197">b.</span></span> <span data-ttu-id="a0efb-198">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a0efb-198">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="a0efb-199">c.</span><span class="sxs-lookup"><span data-stu-id="a0efb-199">c.</span></span> <span data-ttu-id="a0efb-200">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="a0efb-200">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="a0efb-201">d.</span><span class="sxs-lookup"><span data-stu-id="a0efb-201">d.</span></span> <span data-ttu-id="a0efb-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-202">Click **Create**.</span></span>
 
### <a name="create-an-evernote-test-user"></a><span data-ttu-id="a0efb-203">Skapa en testanvändare Evernote</span><span class="sxs-lookup"><span data-stu-id="a0efb-203">Create an Evernote test user</span></span>

<span data-ttu-id="a0efb-204">För att aktivera Azure AD-användare att logga in på Evernote etableras de i Evernote.</span><span class="sxs-lookup"><span data-stu-id="a0efb-204">In order to enable Azure AD users to log into Evernote, they must be provisioned into Evernote.</span></span>  
<span data-ttu-id="a0efb-205">När det gäller Evernote är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a0efb-205">In the case of Evernote, provisioning is a manual task.</span></span>

<span data-ttu-id="a0efb-206">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="a0efb-206">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="a0efb-207">Logga in på webbplatsen Evernote företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="a0efb-207">Log in to your Evernote company site as an administrator.</span></span>

2. <span data-ttu-id="a0efb-208">Klicka på den **Admin Console**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-208">Click the **'Admin Console'**.</span></span>

    ![Administrationskonsolen](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. <span data-ttu-id="a0efb-210">Från den **Admin Console**, gå till **'Lägg till användare ”**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-210">From the **'Admin Console'**, go to **‘Add users’**.</span></span>

    ![Lägg till testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. <span data-ttu-id="a0efb-212">**Lägg till teammedlemmar** i den **e-post** textruta Skriv e-postadressen för användarkontot och klicka på **bjuda in.**</span><span class="sxs-lookup"><span data-stu-id="a0efb-212">**Add team members** in the **Email** textbox, type the email address of user account and click **Invite.**</span></span>

    ![Lägg till testUser](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. <span data-ttu-id="a0efb-214">När inbjudan har skickats får kontoinnehavaren Azure Active Directory ett e-postmeddelande för att tacka ja till inbjudan.</span><span class="sxs-lookup"><span data-stu-id="a0efb-214">After invitation is sent, the Azure Active Directory account holder will receive an email to accept the invitation.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a0efb-215">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a0efb-215">Assign the Azure AD test user</span></span>

<span data-ttu-id="a0efb-216">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Evernote.</span><span class="sxs-lookup"><span data-stu-id="a0efb-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Evernote.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="a0efb-218">**Om du vill tilldela Evernote Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a0efb-218">**To assign Britta Simon to Evernote, perform the following steps:**</span></span>

1. <span data-ttu-id="a0efb-219">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a0efb-221">Välj i listan med program **Evernote**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-221">In the applications list, select **Evernote**.</span></span>

    ![Länken Evernote i listan med program](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. <span data-ttu-id="a0efb-223">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a0efb-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="a0efb-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0efb-225">Click **Add** button.</span></span> <span data-ttu-id="a0efb-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0efb-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="a0efb-228">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a0efb-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a0efb-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0efb-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a0efb-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a0efb-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a0efb-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a0efb-231">Test single sign-on</span></span>

<span data-ttu-id="a0efb-232">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a0efb-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a0efb-233">När du klickar på panelen Evernote på åtkomstpanelen du ska hämta loggat in på ditt Evernote program.</span><span class="sxs-lookup"><span data-stu-id="a0efb-233">When you click the Evernote tile in the Access Panel, you should get signed-on to your Evernote application.</span></span> <span data-ttu-id="a0efb-234">Du måste logga in som en organisation konto men sedan måste du logga in med ditt eget konto.</span><span class="sxs-lookup"><span data-stu-id="a0efb-234">You'll be logging in as an Organization account but then need to log in with your personal account.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a0efb-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a0efb-235">Additional resources</span></span>

* [<span data-ttu-id="a0efb-236">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a0efb-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a0efb-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a0efb-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

