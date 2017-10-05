---
title: "Självstudier: Azure Active Directory-integrering med Asana | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Asana."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: a2f0cecb93cc382bcfd710c44eb70f80ae67f9b6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a><span data-ttu-id="24eb3-103">Självstudier: Azure Active Directory-integrering med Asana</span><span class="sxs-lookup"><span data-stu-id="24eb3-103">Tutorial: Azure Active Directory integration with Asana</span></span>

<span data-ttu-id="24eb3-104">I kursen får lära du att integrera Asana med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="24eb3-104">In this tutorial, you learn how to integrate Asana with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="24eb3-105">Integrera Asana med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="24eb3-105">Integrating Asana with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="24eb3-106">Du kan styra i Azure AD som har åtkomst till Asana</span><span class="sxs-lookup"><span data-stu-id="24eb3-106">You can control in Azure AD who has access to Asana</span></span>
- <span data-ttu-id="24eb3-107">Du kan aktivera användarna att automatiskt hämta loggat in på Asana (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="24eb3-107">You can enable your users to automatically get signed-on to Asana (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="24eb3-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="24eb3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="24eb3-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="24eb3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24eb3-110">Krav</span><span class="sxs-lookup"><span data-stu-id="24eb3-110">Prerequisites</span></span>

<span data-ttu-id="24eb3-111">För att konfigurera Azure AD-integrering med Asana, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="24eb3-111">To configure Azure AD integration with Asana, you need the following items:</span></span>

- <span data-ttu-id="24eb3-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="24eb3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="24eb3-113">En Asana enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="24eb3-113">An Asana single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="24eb3-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="24eb3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="24eb3-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="24eb3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="24eb3-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="24eb3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="24eb3-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="24eb3-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="24eb3-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="24eb3-118">Scenario description</span></span>
<span data-ttu-id="24eb3-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="24eb3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="24eb3-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="24eb3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="24eb3-121">Att lägga till Asana från galleriet</span><span class="sxs-lookup"><span data-stu-id="24eb3-121">Adding Asana from the gallery</span></span>
2. <span data-ttu-id="24eb3-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24eb3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asana-from-the-gallery"></a><span data-ttu-id="24eb3-123">Att lägga till Asana från galleriet</span><span class="sxs-lookup"><span data-stu-id="24eb3-123">Adding Asana from the gallery</span></span>
<span data-ttu-id="24eb3-124">Du måste lägga till Asana från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Asana i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="24eb3-124">To configure the integration of Asana into Azure AD, you need to add Asana from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="24eb3-125">**Utför följande steg för att lägga till Asana från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="24eb3-125">**To add Asana from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="24eb3-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="24eb3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="24eb3-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="24eb3-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="24eb3-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24eb3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="24eb3-133">I sökrutan skriver **Asana**väljer **Asana** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="24eb3-133">In the search box, type **Asana**, select **Asana** from result panel then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="24eb3-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24eb3-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="24eb3-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Asana baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="24eb3-136">In this section, you configure and test Azure AD single sign-on with Asana based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="24eb3-137">Azure AD måste du känna till användaren i Asana motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="24eb3-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Asana is to a user in Azure AD.</span></span> <span data-ttu-id="24eb3-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Asana upprättas.</span><span class="sxs-lookup"><span data-stu-id="24eb3-138">In other words, a link relationship between an Azure AD user and the related user in Asana needs to be established.</span></span>

<span data-ttu-id="24eb3-139">I Asana, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="24eb3-139">In Asana, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="24eb3-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Asana, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="24eb3-140">To configure and test Azure AD single sign-on with Asana, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="24eb3-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="24eb3-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="24eb3-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="24eb3-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="24eb3-143">**[Skapa en testanvändare Asana](#create-an-asana-test-user)**  – du har en motsvarighet för Britta Simon i Asana som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="24eb3-143">**[Create an Asana test user](#create-an-asana-test-user)** - to have a counterpart of Britta Simon in Asana that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="24eb3-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="24eb3-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="24eb3-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="24eb3-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="24eb3-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24eb3-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="24eb3-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Asana program.</span><span class="sxs-lookup"><span data-stu-id="24eb3-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Asana application.</span></span>

<span data-ttu-id="24eb3-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Asana:**</span><span class="sxs-lookup"><span data-stu-id="24eb3-148">**To configure Azure AD single sign-on with Asana, perform the following steps:**</span></span>

1. <span data-ttu-id="24eb3-149">I Azure-portalen på den **Asana** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-149">In the Azure portal, on the **Asana** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="24eb3-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="24eb3-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. <span data-ttu-id="24eb3-153">På den **Asana domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="24eb3-153">On the **Asana Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och Asana domän med enkel inloggning information](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    <span data-ttu-id="24eb3-155">a.</span><span class="sxs-lookup"><span data-stu-id="24eb3-155">a.</span></span> <span data-ttu-id="24eb3-156">I den **inloggnings-URL** textruta typen URL:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="24eb3-156">In the **Sign-on URL** textbox, type URL: `https://app.asana.com/`</span></span>

    <span data-ttu-id="24eb3-157">b.</span><span class="sxs-lookup"><span data-stu-id="24eb3-157">b.</span></span> <span data-ttu-id="24eb3-158">I den **identifierare** textruta TYPVÄRDE:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="24eb3-158">In the **Identifier** textbox, type value: `https://app.asana.com/`</span></span>
 
4. <span data-ttu-id="24eb3-159">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="24eb3-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. <span data-ttu-id="24eb3-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="24eb3-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="24eb3-163">På den **Asana Configuration** klickar du på **konfigurera Asana** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="24eb3-163">On the **Asana Configuration** section, click **Configure Asana** to open **Configure sign-on** window.</span></span> <span data-ttu-id="24eb3-164">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="24eb3-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Asana konfiguration](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. <span data-ttu-id="24eb3-166">I ett annat webbläsarfönster inloggning i tillämpningsprogrammet Asana.</span><span class="sxs-lookup"><span data-stu-id="24eb3-166">In a different browser window, sign-on to your Asana application.</span></span> <span data-ttu-id="24eb3-167">Om du vill konfigurera enkel inloggning i Asana att komma åt arbetsytan inställningar genom att klicka på namnet på arbetsytan i det övre högra hörnet på skärmen.</span><span class="sxs-lookup"><span data-stu-id="24eb3-167">To configure SSO in Asana, access the workspace settings by clicking the workspace name on the top right corner of the screen.</span></span> <span data-ttu-id="24eb3-168">Klicka på  **\<arbetsytans namn\> inställningar**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-168">Then, click on **\<your workspace name\> Settings**.</span></span> 
   
    ![Asana sso-inställningar](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. <span data-ttu-id="24eb3-170">På den **organisationsinställningar** -fönstret klickar du på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-170">On the **Organization settings** window, click **Administration**.</span></span> <span data-ttu-id="24eb3-171">Klicka på **medlemmar måste logga in via SAML** att aktivera SSO-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="24eb3-171">Then, click **Members must log in via SAML** to enable the SSO configuration.</span></span> <span data-ttu-id="24eb3-172">Utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="24eb3-172">The perform the following steps:</span></span>
   
    ![Konfigurera inställningar för enkel inloggning organisation](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     <span data-ttu-id="24eb3-174">a.</span><span class="sxs-lookup"><span data-stu-id="24eb3-174">a.</span></span> <span data-ttu-id="24eb3-175">I den **inloggning Sidadress** textruta klistra in den **SAML inloggning tjänst-URL för enkel**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-175">In the **Sign-in page URL** textbox, paste the **SAML Single Sign-On Service URL**.</span></span>

     <span data-ttu-id="24eb3-176">b.</span><span class="sxs-lookup"><span data-stu-id="24eb3-176">b.</span></span> <span data-ttu-id="24eb3-177">Högerklicka på certifikatet hämtas från Azure-portalen och sedan öppna certifikatfilen i anteckningar eller din önskade textredigerare.</span><span class="sxs-lookup"><span data-stu-id="24eb3-177">Right click the certificate downloaded from Azure portal, then open the certificate file using Notepad or your preferred text editor.</span></span> <span data-ttu-id="24eb3-178">Kopiera innehåll mellan start- och slut-certifikat och klistra in den i den **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="24eb3-178">Copy the content between the begin and the end certificate title and paste it in the **X.509 Certificate** textbox.</span></span>

9. <span data-ttu-id="24eb3-179">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-179">Click **Save**.</span></span> <span data-ttu-id="24eb3-180">Gå till [Asana guide för att konfigurera enkel inloggning](https://asana.com/guide/help/premium/authentication#gl-saml) om du behöver ytterligare hjälp.</span><span class="sxs-lookup"><span data-stu-id="24eb3-180">Go to [Asana guide for setting up SSO](https://asana.com/guide/help/premium/authentication#gl-saml) if you need further assistance.</span></span>

> [!TIP]
> <span data-ttu-id="24eb3-181">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="24eb3-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="24eb3-182">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="24eb3-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="24eb3-183">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="24eb3-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="24eb3-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="24eb3-184">Create an Azure AD test user</span></span>

<span data-ttu-id="24eb3-185">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="24eb3-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="24eb3-187">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="24eb3-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="24eb3-188">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="24eb3-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="24eb3-190">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="24eb3-192">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24eb3-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="24eb3-194">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="24eb3-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="24eb3-196">a.</span><span class="sxs-lookup"><span data-stu-id="24eb3-196">a.</span></span> <span data-ttu-id="24eb3-197">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="24eb3-198">b.</span><span class="sxs-lookup"><span data-stu-id="24eb3-198">b.</span></span> <span data-ttu-id="24eb3-199">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="24eb3-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="24eb3-200">c.</span><span class="sxs-lookup"><span data-stu-id="24eb3-200">c.</span></span> <span data-ttu-id="24eb3-201">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="24eb3-202">d.</span><span class="sxs-lookup"><span data-stu-id="24eb3-202">d.</span></span> <span data-ttu-id="24eb3-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-203">Click **Create**.</span></span>
 
### <a name="create-an-asana-test-user"></a><span data-ttu-id="24eb3-204">Skapa en testanvändare Asana</span><span class="sxs-lookup"><span data-stu-id="24eb3-204">Create an Asana test user</span></span>

<span data-ttu-id="24eb3-205">I det här avsnittet skapar du en användare som kallas Britta Simon i Asana.</span><span class="sxs-lookup"><span data-stu-id="24eb3-205">In this section, you create a user called Britta Simon in Asana.</span></span>

1. <span data-ttu-id="24eb3-206">På **Asana**, gå till den **team** avsnitt i den vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="24eb3-206">On **Asana**, go to the **Teams** section on the left panel.</span></span> <span data-ttu-id="24eb3-207">Klicka på plustecknet.</span><span class="sxs-lookup"><span data-stu-id="24eb3-207">Click the plus sign button.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. <span data-ttu-id="24eb3-209">Skriv e-postmeddelandet britta.simon@contoso.com i textrutan och välj sedan **bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-209">Type the email britta.simon@contoso.com in the text box and then select **Invite**.</span></span>

3. <span data-ttu-id="24eb3-210">Klicka på **skicka inbjudan**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-210">Click **Send Invite**.</span></span> <span data-ttu-id="24eb3-211">Den nya användaren får ett e-postmeddelande till sin e-postkonto.</span><span class="sxs-lookup"><span data-stu-id="24eb3-211">The new user will receive an email into her email account.</span></span> <span data-ttu-id="24eb3-212">Hon behöver du skapa och verifiera kontot.</span><span class="sxs-lookup"><span data-stu-id="24eb3-212">She will need to create and validate the account.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="24eb3-213">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="24eb3-213">Assign the Azure AD test user</span></span>

<span data-ttu-id="24eb3-214">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Asana.</span><span class="sxs-lookup"><span data-stu-id="24eb3-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Asana.</span></span>

![Tilldela rollen][200]

<span data-ttu-id="24eb3-216">**Om du vill tilldela Asana Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="24eb3-216">**To assign Britta Simon to Asana, perform the following steps:**</span></span>

1. <span data-ttu-id="24eb3-217">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="24eb3-219">Välj i listan med program **Asana**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-219">In the applications list, select **Asana**.</span></span>

    ![Länken Asana i listan med program](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. <span data-ttu-id="24eb3-221">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-221">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="24eb3-223">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="24eb3-223">Click **Add** button.</span></span> <span data-ttu-id="24eb3-224">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24eb3-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="24eb3-226">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="24eb3-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="24eb3-227">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24eb3-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="24eb3-228">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="24eb3-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="24eb3-229">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="24eb3-229">Test single sign-on</span></span>

<span data-ttu-id="24eb3-230">Syftet med det här avsnittet är att testa din Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="24eb3-230">The objective of this section is to test your Azure AD single sign-on.</span></span>

<span data-ttu-id="24eb3-231">Gå till sidan för Asana inloggning.</span><span class="sxs-lookup"><span data-stu-id="24eb3-231">Go to Asana login page.</span></span> <span data-ttu-id="24eb3-232">Infoga den e-postadressen i textrutan e-postadress britta.simon@contoso.com. Lämna textrutan lösenord i tom och klickar sedan på **loggar In**.</span><span class="sxs-lookup"><span data-stu-id="24eb3-232">In the Email address textbox, insert the email address britta.simon@contoso.com. Leave the password textbox in blank and then click **Log In**.</span></span> <span data-ttu-id="24eb3-233">Du omdirigeras till inloggningssidan för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="24eb3-233">You will be redirected to Azure AD login page.</span></span> <span data-ttu-id="24eb3-234">Slutför Azure AD-autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="24eb3-234">Complete your Azure AD credentials.</span></span> <span data-ttu-id="24eb3-235">Nu kan är du inloggad Asana.</span><span class="sxs-lookup"><span data-stu-id="24eb3-235">Now, you are logged in on Asana.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24eb3-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="24eb3-236">Additional resources</span></span>

* [<span data-ttu-id="24eb3-237">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="24eb3-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="24eb3-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="24eb3-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
