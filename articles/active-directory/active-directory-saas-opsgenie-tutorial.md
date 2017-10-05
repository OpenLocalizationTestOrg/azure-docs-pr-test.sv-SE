---
title: "Självstudier: Azure Active Directory-integrering med OpsGenie | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och OpsGenie."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: ce63726d2406d2f1415d29786f0ef92ca95b9b90
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a><span data-ttu-id="9c61d-103">Självstudier: Azure Active Directory-integrering med OpsGenie</span><span class="sxs-lookup"><span data-stu-id="9c61d-103">Tutorial: Azure Active Directory integration with OpsGenie</span></span>

<span data-ttu-id="9c61d-104">I kursen får lära du att integrera OpsGenie med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9c61d-104">In this tutorial, you learn how to integrate OpsGenie with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9c61d-105">Integrera OpsGenie med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9c61d-105">Integrating OpsGenie with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9c61d-106">Du kan styra i Azure AD som har åtkomst till OpsGenie</span><span class="sxs-lookup"><span data-stu-id="9c61d-106">You can control in Azure AD who has access to OpsGenie</span></span>
- <span data-ttu-id="9c61d-107">Du kan aktivera användarna att automatiskt hämta loggat in på OpsGenie (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9c61d-107">You can enable your users to automatically get signed-on to OpsGenie (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9c61d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9c61d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9c61d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9c61d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c61d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9c61d-110">Prerequisites</span></span>

<span data-ttu-id="9c61d-111">För att konfigurera Azure AD-integrering med OpsGenie, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="9c61d-111">To configure Azure AD integration with OpsGenie, you need the following items:</span></span>

- <span data-ttu-id="9c61d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9c61d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9c61d-113">En OpsGenie enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="9c61d-113">A OpsGenie single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9c61d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9c61d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9c61d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9c61d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9c61d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9c61d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9c61d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9c61d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9c61d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9c61d-118">Scenario description</span></span>
<span data-ttu-id="9c61d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9c61d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9c61d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9c61d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9c61d-121">Att lägga till OpsGenie från galleriet</span><span class="sxs-lookup"><span data-stu-id="9c61d-121">Adding OpsGenie from the gallery</span></span>
2. <span data-ttu-id="9c61d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c61d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-opsgenie-from-the-gallery"></a><span data-ttu-id="9c61d-123">Att lägga till OpsGenie från galleriet</span><span class="sxs-lookup"><span data-stu-id="9c61d-123">Adding OpsGenie from the gallery</span></span>
<span data-ttu-id="9c61d-124">Du måste lägga till OpsGenie från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av OpsGenie i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9c61d-124">To configure the integration of OpsGenie into Azure AD, you need to add OpsGenie from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9c61d-125">**Utför följande steg för att lägga till OpsGenie från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="9c61d-125">**To add OpsGenie from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9c61d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9c61d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9c61d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9c61d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9c61d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c61d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9c61d-133">I sökrutan skriver **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-133">In the search box, type **OpsGenie**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_search.png)

5. <span data-ttu-id="9c61d-135">Välj i resultatpanelen **OpsGenie**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="9c61d-135">In the results panel, select **OpsGenie**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9c61d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c61d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9c61d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med OpsGenie baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9c61d-138">In this section, you configure and test Azure AD single sign-on with OpsGenie based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9c61d-139">Azure AD måste du känna till användaren i OpsGenie motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="9c61d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in OpsGenie is to a user in Azure AD.</span></span> <span data-ttu-id="9c61d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i OpsGenie upprättas.</span><span class="sxs-lookup"><span data-stu-id="9c61d-140">In other words, a link relationship between an Azure AD user and the related user in OpsGenie needs to be established.</span></span>

<span data-ttu-id="9c61d-141">I OpsGenie, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9c61d-141">In OpsGenie, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9c61d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med OpsGenie, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9c61d-142">To configure and test Azure AD single sign-on with OpsGenie, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9c61d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9c61d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9c61d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c61d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9c61d-145">**[Skapa en testanvändare OpsGenie](#creating-a-opsgenie-test-user)**  – du har en motsvarighet för Britta Simon i OpsGenie som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9c61d-145">**[Creating a OpsGenie test user](#creating-a-opsgenie-test-user)** - to have a counterpart of Britta Simon in OpsGenie that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9c61d-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9c61d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9c61d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9c61d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9c61d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c61d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9c61d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt OpsGenie program.</span><span class="sxs-lookup"><span data-stu-id="9c61d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your OpsGenie application.</span></span>

<span data-ttu-id="9c61d-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med OpsGenie:**</span><span class="sxs-lookup"><span data-stu-id="9c61d-150">**To configure Azure AD single sign-on with OpsGenie, perform the following steps:**</span></span>

1. <span data-ttu-id="9c61d-151">I Azure-portalen på den **OpsGenie** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-151">In the Azure portal, on the **OpsGenie** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9c61d-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9c61d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

3. <span data-ttu-id="9c61d-155">På den **OpsGenie domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c61d-155">On the **OpsGenie Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_url.png)

    <span data-ttu-id="9c61d-157">I den **inloggnings-URL** textruta anger du URL:`https://app.opsgenie.com/auth/login`</span><span class="sxs-lookup"><span data-stu-id="9c61d-157">In the **Sign-on URL** textbox, type the URL: `https://app.opsgenie.com/auth/login`</span></span>

4. <span data-ttu-id="9c61d-158">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="9c61d-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_certificate.png) 

5. <span data-ttu-id="9c61d-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9c61d-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9c61d-162">På den **OpsGenie Configuration** klickar du på **konfigurera OpsGenie** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9c61d-162">On the **OpsGenie Configuration** section, click **Configure OpsGenie** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9c61d-163">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="9c61d-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_configure.png) 

7. <span data-ttu-id="9c61d-165">Öppna en annan instans för webbläsaren och sedan logga in på OpsGenie som administratör.</span><span class="sxs-lookup"><span data-stu-id="9c61d-165">Open another browser instance, and then log-in to OpsGenie as an administrator.</span></span>

8. <span data-ttu-id="9c61d-166">Klicka på **inställningar**, och klicka sedan på den **enkel inloggning** fliken.</span><span class="sxs-lookup"><span data-stu-id="9c61d-166">Click **Settings**, and then click the **Single Sign On** tab.</span></span>
   
    ![OpsGenie enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

9. <span data-ttu-id="9c61d-168">Välj för att aktivera enkel inloggning **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-168">To enable SSO, select **Enabled**.</span></span>
   
    ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 

10. <span data-ttu-id="9c61d-170">I den **Provider** klickar du på den **Azure Active Directory** fliken.</span><span class="sxs-lookup"><span data-stu-id="9c61d-170">In the **Provider** section, click the **Azure Active Directory** tab.</span></span>
   
    ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 

11. <span data-ttu-id="9c61d-172">På sidan för Azure Active Directory-dialogrutan utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c61d-172">On the Azure Active Directory dialog page, perform the following steps:</span></span>
   
    ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    <span data-ttu-id="9c61d-174">a.</span><span class="sxs-lookup"><span data-stu-id="9c61d-174">a.</span></span> <span data-ttu-id="9c61d-175">Klistra in **enkel inloggning på Tjänstwebbadress**, som du har kopierat från Azure-portalen i den **SAML 2.0 Endpoint** textruta.</span><span class="sxs-lookup"><span data-stu-id="9c61d-175">Paste **Single Sign On Service URL**, which you have copied from the Azure portal into the **SAML 2.0 Endpoint** textbox.</span></span>
    
    <span data-ttu-id="9c61d-176">b.</span><span class="sxs-lookup"><span data-stu-id="9c61d-176">b.</span></span> <span data-ttu-id="9c61d-177">Öppna din hämtade Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistrar in det i den **X.500 certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="9c61d-177">Open your downloaded base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it into the **X.500 Certificate** textbox.</span></span>
    
    <span data-ttu-id="9c61d-178">c.</span><span class="sxs-lookup"><span data-stu-id="9c61d-178">c.</span></span> <span data-ttu-id="9c61d-179">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-179">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="9c61d-180">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="9c61d-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9c61d-181">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="9c61d-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9c61d-182">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9c61d-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9c61d-183">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c61d-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="9c61d-184">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9c61d-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9c61d-186">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="9c61d-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9c61d-187">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9c61d-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9c61d-189">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9c61d-191">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c61d-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9c61d-193">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c61d-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9c61d-195">a.</span><span class="sxs-lookup"><span data-stu-id="9c61d-195">a.</span></span> <span data-ttu-id="9c61d-196">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9c61d-197">b.</span><span class="sxs-lookup"><span data-stu-id="9c61d-197">b.</span></span> <span data-ttu-id="9c61d-198">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9c61d-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9c61d-199">c.</span><span class="sxs-lookup"><span data-stu-id="9c61d-199">c.</span></span> <span data-ttu-id="9c61d-200">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9c61d-201">d.</span><span class="sxs-lookup"><span data-stu-id="9c61d-201">d.</span></span> <span data-ttu-id="9c61d-202">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-202">Click **Create**.</span></span>
 
### <a name="creating-a-opsgenie-test-user"></a><span data-ttu-id="9c61d-203">Skapa en testanvändare OpsGenie</span><span class="sxs-lookup"><span data-stu-id="9c61d-203">Creating a OpsGenie test user</span></span>

<span data-ttu-id="9c61d-204">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="9c61d-204">The objective of this section is to create a user called Britta Simon in OpsGenie.</span></span> 

1. <span data-ttu-id="9c61d-205">Logga in på din OpsGenie klient som en administratör i ett webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="9c61d-205">In a web browser window, log into your OpsGenie tenant as an administrator.</span></span>

2. <span data-ttu-id="9c61d-206">Gå till listan genom att klicka på **användaren** i den vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="9c61d-206">Navigate to Users list by clicking **User** in left panel.</span></span>
   
   ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. <span data-ttu-id="9c61d-208">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-208">Click **Add User**.</span></span>

4. <span data-ttu-id="9c61d-209">På den **Lägg till användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c61d-209">On the **Add User** dialog, perform the following steps:</span></span>
   
   ![OpsGenie inställningar](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   <span data-ttu-id="9c61d-211">a.</span><span class="sxs-lookup"><span data-stu-id="9c61d-211">a.</span></span> <span data-ttu-id="9c61d-212">I den **e-post** textruta e-postadressen sorts BrittaSimon åtgärdas i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9c61d-212">In the **Email** textbox, type the email address of BrittaSimon addressed in Azure Active Directory.</span></span>
   
   <span data-ttu-id="9c61d-213">b.</span><span class="sxs-lookup"><span data-stu-id="9c61d-213">b.</span></span> <span data-ttu-id="9c61d-214">I den **fullständiga namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-214">In the **Full Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="9c61d-215">c.</span><span class="sxs-lookup"><span data-stu-id="9c61d-215">c.</span></span> <span data-ttu-id="9c61d-216">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-216">Click **Save**.</span></span> 

>[!NOTE]
><span data-ttu-id="9c61d-217">Britta får ett e-postmeddelande med instruktioner för att konfigurera sin profil.</span><span class="sxs-lookup"><span data-stu-id="9c61d-217">Britta gets an email with instructions for setting up her profile.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9c61d-218">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="9c61d-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9c61d-219">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="9c61d-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to OpsGenie.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9c61d-221">**Om du vill tilldela OpsGenie Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9c61d-221">**To assign Britta Simon to OpsGenie, perform the following steps:**</span></span>

1. <span data-ttu-id="9c61d-222">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9c61d-224">Välj i listan med program **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-224">In the applications list, select **OpsGenie**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_app.png) 

3. <span data-ttu-id="9c61d-226">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9c61d-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9c61d-228">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9c61d-228">Click **Add** button.</span></span> <span data-ttu-id="9c61d-229">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c61d-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9c61d-231">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="9c61d-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9c61d-232">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c61d-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9c61d-233">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9c61d-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9c61d-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9c61d-234">Testing single sign-on</span></span>

<span data-ttu-id="9c61d-235">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9c61d-235">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="9c61d-236">När du klickar på panelen OpsGenie på åtkomstpanelen du bör få automatiskt loggat in på ditt OpsGenie program.</span><span class="sxs-lookup"><span data-stu-id="9c61d-236">When you click the OpsGenie tile in the Access Panel, you should get automatically signed-on to your OpsGenie application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c61d-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9c61d-237">Additional resources</span></span>

* [<span data-ttu-id="9c61d-238">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9c61d-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9c61d-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9c61d-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png

