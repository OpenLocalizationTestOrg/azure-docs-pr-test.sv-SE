---
title: "Självstudier: Azure Active Directory-integrering med ClickTime | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ClickTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: 0e0123a40d52dfd7a2e29c29cb2239e979089ca9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a><span data-ttu-id="babba-103">Självstudier: Azure Active Directory-integrering med ClickTime</span><span class="sxs-lookup"><span data-stu-id="babba-103">Tutorial: Azure Active Directory integration with ClickTime</span></span>

<span data-ttu-id="babba-104">I kursen får lära du att integrera ClickTime med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="babba-104">In this tutorial, you learn how to integrate ClickTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="babba-105">Integrera ClickTime med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="babba-105">Integrating ClickTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="babba-106">Du kan styra i Azure AD som har åtkomst till ClickTime</span><span class="sxs-lookup"><span data-stu-id="babba-106">You can control in Azure AD who has access to ClickTime</span></span>
- <span data-ttu-id="babba-107">Du kan aktivera användarna att automatiskt hämta loggat in på ClickTime (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="babba-107">You can enable your users to automatically get signed-on to ClickTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="babba-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="babba-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="babba-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="babba-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="babba-110">Krav</span><span class="sxs-lookup"><span data-stu-id="babba-110">Prerequisites</span></span>

<span data-ttu-id="babba-111">För att konfigurera Azure AD-integrering med ClickTime, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="babba-111">To configure Azure AD integration with ClickTime, you need the following items:</span></span>

- <span data-ttu-id="babba-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="babba-112">An Azure AD subscription</span></span>
- <span data-ttu-id="babba-113">En ClickTime enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="babba-113">A ClickTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="babba-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="babba-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="babba-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="babba-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="babba-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="babba-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="babba-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="babba-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="babba-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="babba-118">Scenario description</span></span>
<span data-ttu-id="babba-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="babba-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="babba-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="babba-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="babba-121">Att lägga till ClickTime från galleriet</span><span class="sxs-lookup"><span data-stu-id="babba-121">Adding ClickTime from the gallery</span></span>
2. <span data-ttu-id="babba-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="babba-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-clicktime-from-the-gallery"></a><span data-ttu-id="babba-123">Att lägga till ClickTime från galleriet</span><span class="sxs-lookup"><span data-stu-id="babba-123">Adding ClickTime from the gallery</span></span>
<span data-ttu-id="babba-124">Du måste lägga till ClickTime från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ClickTime i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="babba-124">To configure the integration of ClickTime into Azure AD, you need to add ClickTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="babba-125">**Utför följande steg för att lägga till ClickTime från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="babba-125">**To add ClickTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="babba-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="babba-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="babba-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="babba-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="babba-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="babba-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="babba-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="babba-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="babba-133">I sökrutan skriver **ClickTime**väljer **ClickTime** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="babba-133">In the search box, type **ClickTime**, select **ClickTime** from result panel then click **Add** button to add the application.</span></span>

    ![ClickTime i resultatlistan](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="babba-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="babba-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="babba-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ClickTime baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="babba-136">In this section, you configure and test Azure AD single sign-on with ClickTime based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="babba-137">Azure AD måste du känna till användaren i ClickTime motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="babba-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ClickTime is to a user in Azure AD.</span></span> <span data-ttu-id="babba-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ClickTime upprättas.</span><span class="sxs-lookup"><span data-stu-id="babba-138">In other words, a link relationship between an Azure AD user and the related user in ClickTime needs to be established.</span></span>

<span data-ttu-id="babba-139">I ClickTime, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="babba-139">In ClickTime, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="babba-140">Om du vill konfigurera och testa Azure AD enkel inloggning med ClickTime, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="babba-140">To configure and test Azure AD single sign-on with ClickTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="babba-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="babba-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="babba-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="babba-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="babba-143">**[Skapa en testanvändare ClickTime](#create-a-clicktime-test-user)**  – du har en motsvarighet för Britta Simon i ClickTime som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="babba-143">**[Create a ClickTime test user](#create-a-clicktime-test-user)** - to have a counterpart of Britta Simon in ClickTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="babba-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="babba-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="babba-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="babba-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="babba-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="babba-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="babba-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt ClickTime program.</span><span class="sxs-lookup"><span data-stu-id="babba-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ClickTime application.</span></span>

<span data-ttu-id="babba-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ClickTime:**</span><span class="sxs-lookup"><span data-stu-id="babba-148">**To configure Azure AD single sign-on with ClickTime, perform the following steps:**</span></span>

1. <span data-ttu-id="babba-149">I Azure-portalen på den **ClickTime** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="babba-149">In the Azure portal, on the **ClickTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="babba-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="babba-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_samlbase.png)

3. <span data-ttu-id="babba-153">På den **ClickTime domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="babba-153">On the **ClickTime Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och ClickTime domän med enkel inloggning information](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_url.png)

    <span data-ttu-id="babba-155">a.</span><span class="sxs-lookup"><span data-stu-id="babba-155">a.</span></span> <span data-ttu-id="babba-156">I den **identifierare** textruta Skriv en URL som:`https://app.clicktime.com/sp/`</span><span class="sxs-lookup"><span data-stu-id="babba-156">In the **Identifier** textbox, type a URL as: `https://app.clicktime.com/sp/`</span></span>
    
    <span data-ttu-id="babba-157">b.</span><span class="sxs-lookup"><span data-stu-id="babba-157">b.</span></span> <span data-ttu-id="babba-158">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="babba-158">In the **Reply URL** textbox, type a URL using the following patterns:</span></span> 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

4. <span data-ttu-id="babba-159">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="babba-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_certificate.png) 

5. <span data-ttu-id="babba-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="babba-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-clicktime-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="babba-163">På den **ClickTime Configuration** klickar du på **konfigurera ClickTime** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="babba-163">On the **ClickTime Configuration** section, click **Configure ClickTime** to open **Configure sign-on** window.</span></span> <span data-ttu-id="babba-164">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="babba-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![ClickTime konfiguration](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_configure.png) 

7. <span data-ttu-id="babba-166">Logga in på webbplatsen ClickTime företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="babba-166">In a different web browser window, log into your ClickTime company site as an administrator.</span></span>

8. <span data-ttu-id="babba-167">Klicka på i verktygsfältet högst upp **inställningar**, och klicka sedan på **säkerhetsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="babba-167">In the toolbar on the top, click **Preferences**, and then click **Security Settings**.</span></span>

9. <span data-ttu-id="babba-168">I den **inställningar för enkel inloggning** konfiguration och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="babba-168">In the **Single Sign-On Preferences** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="babba-169">![Säkerhetsinställningar](./media/active-directory-saas-clicktime-tutorial/tic777280.png "säkerhetsinställningar")</span><span class="sxs-lookup"><span data-stu-id="babba-169">![Security Settings](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Security Settings")</span></span>
   
    <span data-ttu-id="babba-170">a.</span><span class="sxs-lookup"><span data-stu-id="babba-170">a.</span></span>  <span data-ttu-id="babba-171">Välj **Tillåt** logga in med enkel inloggning (SSO) med **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="babba-171">Select **Allow** sign-in using Single Sign-On (SSO) with **Azure AD**.</span></span>
   
    <span data-ttu-id="babba-172">b.</span><span class="sxs-lookup"><span data-stu-id="babba-172">b.</span></span> <span data-ttu-id="babba-173">I den **identitet Leverantörsslutpunkt** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="babba-173">In the **Identity Provider Endpoint** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="babba-174">c.</span><span class="sxs-lookup"><span data-stu-id="babba-174">c.</span></span>  <span data-ttu-id="babba-175">Öppna den **Base64-kodat certifikat** hämtas från Azure-portalen i **anteckningar**, kopiera innehållet och klistrar in det i den **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="babba-175">Open the **base-64 encoded certificate** downloaded from Azure portal in **Notepad**, copy the content, and then paste it into the **X.509 Certificate** textbox.</span></span>
   
    <span data-ttu-id="babba-176">d.</span><span class="sxs-lookup"><span data-stu-id="babba-176">d.</span></span>  <span data-ttu-id="babba-177">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="babba-177">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="babba-178">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="babba-178">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="babba-179">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="babba-179">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="babba-180">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="babba-180">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="babba-181">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="babba-181">Create an Azure AD test user</span></span>
<span data-ttu-id="babba-182">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="babba-182">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="babba-184">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="babba-184">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="babba-185">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="babba-185">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-clicktime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="babba-187">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="babba-187">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-clicktime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="babba-189">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="babba-189">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-clicktime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="babba-191">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="babba-191">In the **User** dialog box, perform the following steps:</span></span>
 
    ![Dialogrutan användare](./media/active-directory-saas-clicktime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="babba-193">a.</span><span class="sxs-lookup"><span data-stu-id="babba-193">a.</span></span> <span data-ttu-id="babba-194">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="babba-194">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="babba-195">b.</span><span class="sxs-lookup"><span data-stu-id="babba-195">b.</span></span> <span data-ttu-id="babba-196">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="babba-196">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="babba-197">c.</span><span class="sxs-lookup"><span data-stu-id="babba-197">c.</span></span> <span data-ttu-id="babba-198">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="babba-198">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="babba-199">d.</span><span class="sxs-lookup"><span data-stu-id="babba-199">d.</span></span> <span data-ttu-id="babba-200">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="babba-200">Click **Create**.</span></span>
 
### <a name="create-a-clicktime-test-user"></a><span data-ttu-id="babba-201">Skapa en testanvändare ClickTime</span><span class="sxs-lookup"><span data-stu-id="babba-201">Create a ClickTime test user</span></span>

<span data-ttu-id="babba-202">För att aktivera Azure AD-användare att logga in på ClickTime etableras de i ClickTime.</span><span class="sxs-lookup"><span data-stu-id="babba-202">In order to enable Azure AD users to log into ClickTime, they must be provisioned into ClickTime.</span></span>  
<span data-ttu-id="babba-203">När det gäller ClickTime är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="babba-203">In the case of ClickTime, provisioning is a manual task.</span></span>

> [!NOTE]
> <span data-ttu-id="babba-204">Du kan använda något annat ClickTime användarens konto skapas verktyg eller API: er som tillhandahålls av ClickTime att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="babba-204">You can use any other ClickTime user account creation tools or APIs provided by ClickTime to provision Azure AD user accounts.</span></span>

<span data-ttu-id="babba-205">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="babba-205">**To provision a user account, perform the following steps:**</span></span>
1. <span data-ttu-id="babba-206">Logga in på ditt **ClickTime** klient.</span><span class="sxs-lookup"><span data-stu-id="babba-206">Log in to your **ClickTime** tenant.</span></span>
2. <span data-ttu-id="babba-207">Klicka på i verktygsfältet högst upp **företagets**, och klicka sedan på **personer**.</span><span class="sxs-lookup"><span data-stu-id="babba-207">In the toolbar on the top, click **Company**, and then click **People**.</span></span>
   
    <span data-ttu-id="babba-208">![Personer](./media/active-directory-saas-clicktime-tutorial/tic777282.png "personer")</span><span class="sxs-lookup"><span data-stu-id="babba-208">![People](./media/active-directory-saas-clicktime-tutorial/tic777282.png "People")</span></span>
3. <span data-ttu-id="babba-209">Klicka på **lägga till personen**.</span><span class="sxs-lookup"><span data-stu-id="babba-209">Click **Add Person**.</span></span>
   
    <span data-ttu-id="babba-210">![Lägga till personen](./media/active-directory-saas-clicktime-tutorial/tic777283.png "lägga till Person")</span><span class="sxs-lookup"><span data-stu-id="babba-210">![Add Person](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Add Person")</span></span>
4. <span data-ttu-id="babba-211">I avsnittet ny Person utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="babba-211">In the New Person section, perform the following steps:</span></span>
   
    <span data-ttu-id="babba-212">![Personer](./media/active-directory-saas-clicktime-tutorial/tic777284.png "personer")</span><span class="sxs-lookup"><span data-stu-id="babba-212">![People](./media/active-directory-saas-clicktime-tutorial/tic777284.png "People")</span></span>
   
    <span data-ttu-id="babba-213">a.</span><span class="sxs-lookup"><span data-stu-id="babba-213">a.</span></span>  <span data-ttu-id="babba-214">I den **fullständigt namn** textruta typen fullständigt namn för användaren som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="babba-214">In the **full name** textbox, type full name of user like **Britta Simon**.</span></span> 
  
    <span data-ttu-id="babba-215">b.</span><span class="sxs-lookup"><span data-stu-id="babba-215">b.</span></span>  <span data-ttu-id="babba-216">I den **e-postadress** textruta, ange den e-posten för användare som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="babba-216">In the **email address** textbox, type the email of user like **brittasimon@contoso.com**.</span></span>
       
    > [!NOTE]
    > <span data-ttu-id="babba-217">Om du vill kan ange du ytterligare egenskaper i objektet person.</span><span class="sxs-lookup"><span data-stu-id="babba-217">If you want to, you can set additional properties of the new person object.</span></span>
   
    <span data-ttu-id="babba-218">c.</span><span class="sxs-lookup"><span data-stu-id="babba-218">c.</span></span>  <span data-ttu-id="babba-219">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="babba-219">Click **Save**.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="babba-220">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="babba-220">Assign the Azure AD test user</span></span>

<span data-ttu-id="babba-221">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ClickTime.</span><span class="sxs-lookup"><span data-stu-id="babba-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ClickTime.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="babba-223">**Om du vill tilldela ClickTime Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="babba-223">**To assign Britta Simon to ClickTime, perform the following steps:**</span></span>

1. <span data-ttu-id="babba-224">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="babba-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="babba-226">Välj i listan med program **ClickTime**.</span><span class="sxs-lookup"><span data-stu-id="babba-226">In the applications list, select **ClickTime**.</span></span>

    ![ClickTimne länken i listan med program](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_app.png) 

3. <span data-ttu-id="babba-228">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="babba-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202] 

4. <span data-ttu-id="babba-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="babba-230">Click **Add** button.</span></span> <span data-ttu-id="babba-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="babba-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="babba-233">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="babba-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="babba-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="babba-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="babba-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="babba-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="babba-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="babba-236">Test single sign-on</span></span>

<span data-ttu-id="babba-237">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="babba-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="babba-238">När du klickar på panelen ClickTime på åtkomstpanelen du bör få automatiskt loggat in på ditt ClickTime program.</span><span class="sxs-lookup"><span data-stu-id="babba-238">When you click the ClickTime tile in the Access Panel, you should get automatically signed-on to your ClickTime application.</span></span>
<span data-ttu-id="babba-239">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="babba-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="babba-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="babba-240">Additional resources</span></span>

* [<span data-ttu-id="babba-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="babba-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="babba-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="babba-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png

