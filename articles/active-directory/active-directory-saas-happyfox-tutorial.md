---
title: "Självstudier: Azure Active Directory-integrering med HappyFox | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och HappyFox."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 8b5ad750d7849e4037ed7ee93c48b5645c68e799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a><span data-ttu-id="cc6bb-103">Självstudier: Azure Active Directory-integrering med HappyFox</span><span class="sxs-lookup"><span data-stu-id="cc6bb-103">Tutorial: Azure Active Directory integration with HappyFox</span></span>

<span data-ttu-id="cc6bb-104">I kursen får lära du att integrera HappyFox med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cc6bb-104">In this tutorial, you learn how to integrate HappyFox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cc6bb-105">Integrera HappyFox med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cc6bb-105">Integrating HappyFox with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cc6bb-106">Du kan styra i Azure AD som har åtkomst till HappyFox</span><span class="sxs-lookup"><span data-stu-id="cc6bb-106">You can control in Azure AD who has access to HappyFox</span></span>
- <span data-ttu-id="cc6bb-107">Du kan aktivera användarna att automatiskt hämta loggat in på HappyFox (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cc6bb-107">You can enable your users to automatically get signed-on to HappyFox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cc6bb-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cc6bb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cc6bb-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cc6bb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc6bb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cc6bb-110">Prerequisites</span></span>

<span data-ttu-id="cc6bb-111">För att konfigurera Azure AD-integrering med HappyFox, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cc6bb-111">To configure Azure AD integration with HappyFox, you need the following items:</span></span>

- <span data-ttu-id="cc6bb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cc6bb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cc6bb-113">En HappyFox enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="cc6bb-113">A HappyFox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cc6bb-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cc6bb-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cc6bb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cc6bb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cc6bb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cc6bb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cc6bb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cc6bb-118">Scenario description</span></span>
<span data-ttu-id="cc6bb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cc6bb-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cc6bb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cc6bb-121">Att lägga till HappyFox från galleriet</span><span class="sxs-lookup"><span data-stu-id="cc6bb-121">Adding HappyFox from the gallery</span></span>
2. <span data-ttu-id="cc6bb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc6bb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-happyfox-from-the-gallery"></a><span data-ttu-id="cc6bb-123">Att lägga till HappyFox från galleriet</span><span class="sxs-lookup"><span data-stu-id="cc6bb-123">Adding HappyFox from the gallery</span></span>
<span data-ttu-id="cc6bb-124">Du måste lägga till HappyFox från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av HappyFox i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-124">To configure the integration of HappyFox into Azure AD, you need to add HappyFox from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cc6bb-125">**Utför följande steg för att lägga till HappyFox från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cc6bb-125">**To add HappyFox from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cc6bb-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cc6bb-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cc6bb-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cc6bb-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cc6bb-133">I sökrutan skriver **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-133">In the search box, type **HappyFox**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_search.png)

5. <span data-ttu-id="cc6bb-135">Välj i resultatpanelen **HappyFox**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-135">In the results panel, select **HappyFox**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cc6bb-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc6bb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cc6bb-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HappyFox baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-138">In this section, you configure and test Azure AD single sign-on with HappyFox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cc6bb-139">Azure AD måste du känna till användaren i HappyFox motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HappyFox is to a user in Azure AD.</span></span> <span data-ttu-id="cc6bb-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i HappyFox upprättas.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-140">In other words, a link relationship between an Azure AD user and the related user in HappyFox needs to be established.</span></span>

<span data-ttu-id="cc6bb-141">I HappyFox, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-141">In HappyFox, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="cc6bb-142">Om du vill konfigurera och testa Azure AD enkel inloggning med HappyFox, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cc6bb-142">To configure and test Azure AD single sign-on with HappyFox, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cc6bb-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cc6bb-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cc6bb-145">**[Skapa en testanvändare HappyFox](#creating-a-happyfox-test-user)**  – du har en motsvarighet för Britta Simon i HappyFox som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-145">**[Creating a HappyFox test user](#creating-a-happyfox-test-user)** - to have a counterpart of Britta Simon in HappyFox that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cc6bb-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cc6bb-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cc6bb-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc6bb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cc6bb-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt HappyFox program.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HappyFox application.</span></span>

<span data-ttu-id="cc6bb-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med HappyFox:**</span><span class="sxs-lookup"><span data-stu-id="cc6bb-150">**To configure Azure AD single sign-on with HappyFox, perform the following steps:**</span></span>

1. <span data-ttu-id="cc6bb-151">I Azure-portalen på den **HappyFox** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-151">In the Azure portal, on the **HappyFox** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cc6bb-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_samlbase.png)

3. <span data-ttu-id="cc6bb-155">På den **HappyFox domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cc6bb-155">On the **HappyFox Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_url.png)

    <span data-ttu-id="cc6bb-157">a.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-157">a.</span></span> <span data-ttu-id="cc6bb-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.happyfox.com/`</span><span class="sxs-lookup"><span data-stu-id="cc6bb-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.happyfox.com/`</span></span>

    <span data-ttu-id="cc6bb-159">b.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-159">b.</span></span> <span data-ttu-id="cc6bb-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.happyfox.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="cc6bb-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.happyfox.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cc6bb-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-161">These values are not real.</span></span> <span data-ttu-id="cc6bb-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="cc6bb-163">Kontakta [HappyFox klienten supportteamet](https://support.happyfox.com/home) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-163">Contact [HappyFox Client support team](https://support.happyfox.com/home) to get these values.</span></span> 
 
4. <span data-ttu-id="cc6bb-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the Certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_certificate.png) 

5. <span data-ttu-id="cc6bb-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cc6bb-168">På den **HappyFox Configuration** klickar du på **konfigurera HappyFox** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-168">On the **HappyFox Configuration** section, click **Configure HappyFox** to open **Configure sign-on** window.</span></span> <span data-ttu-id="cc6bb-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnittet**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_configure.png) 

7. <span data-ttu-id="cc6bb-171">Logga in på din HappyFox personal portal och gå till **hantera**, klicka på **integreringar** fliken.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-171">Sign on to your HappyFox staff portal and navigate to **Manage**, Click on **Integrations** tab.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/header.png) 

8. <span data-ttu-id="cc6bb-173">Klicka på fliken integreringar **konfigurera** under **SAML Integration** att öppna enkel inloggning på inställningarna.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-173">In the Integrations tab, Click **Configure** under **SAML Integration** to open the Single Sign On Settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/configure.png) 

9. <span data-ttu-id="cc6bb-175">I avsnittet för SAML-konfiguration, klistra in den **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen i **mål-URL för enkel inloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-175">Inside SAML configuration section, paste the **SAML Single Sign-On Service URL** that you have copied from Azure portal into **SSO Target URL** textbox.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/targeturl.png)

10. <span data-ttu-id="cc6bb-177">Öppna certifikatet hämtas från Azure-portalen i anteckningar och klistra in innehållet i **IdP signatur** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-177">Open the certificate downloaded from Azure portal in notepad and paste its content in **IdP Signature** section.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/cert.png)

11. <span data-ttu-id="cc6bb-179">Klicka på **Spara inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-179">Click **Save Settings** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/savesettings.png)

> [!TIP]
> <span data-ttu-id="cc6bb-181">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cc6bb-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cc6bb-182">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cc6bb-183">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cc6bb-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cc6bb-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cc6bb-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="cc6bb-185">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cc6bb-187">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cc6bb-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cc6bb-188">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cc6bb-190">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cc6bb-192">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cc6bb-194">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cc6bb-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-happyfox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cc6bb-196">a.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-196">a.</span></span> <span data-ttu-id="cc6bb-197">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cc6bb-198">b.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-198">b.</span></span> <span data-ttu-id="cc6bb-199">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cc6bb-200">c.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-200">c.</span></span> <span data-ttu-id="cc6bb-201">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cc6bb-202">d.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-202">d.</span></span> <span data-ttu-id="cc6bb-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-203">Click **Create**.</span></span>
 
### <a name="creating-a-happyfox-test-user"></a><span data-ttu-id="cc6bb-204">Skapa en testanvändare HappyFox</span><span class="sxs-lookup"><span data-stu-id="cc6bb-204">Creating a HappyFox test user</span></span>

<span data-ttu-id="cc6bb-205">Programmet stöder bara i tid användaretablering och authentication-användare skapas automatiskt i programmet.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-205">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cc6bb-206">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cc6bb-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cc6bb-207">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till HappyFox.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HappyFox.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cc6bb-209">**Om du vill tilldela HappyFox Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cc6bb-209">**To assign Britta Simon to HappyFox, perform the following steps:**</span></span>

1. <span data-ttu-id="cc6bb-210">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cc6bb-212">Välj i listan med program **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-212">In the applications list, select **HappyFox**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_app.png) 

3. <span data-ttu-id="cc6bb-214">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cc6bb-216">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-216">Click **Add** button.</span></span> <span data-ttu-id="cc6bb-217">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cc6bb-219">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cc6bb-220">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cc6bb-221">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cc6bb-222">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cc6bb-222">Testing single sign-on</span></span>

<span data-ttu-id="cc6bb-223">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-223">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

1. <span data-ttu-id="cc6bb-224">Du bör få inloggningssidan för HappyFox program när du klickar på panelen HappyFox på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-224">When you click the HappyFox tile in the Access Panel, you should get login page of HappyFox application.</span></span> <span data-ttu-id="cc6bb-225">Du bör se den **'SAML-** på sidan logga in.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-225">You should see the **‘SAML’** button on the sign-in page.</span></span>

    ![plugin-programmet](./media/active-directory-saas-happyfox-tutorial/saml.png) 

2. <span data-ttu-id="cc6bb-227">Klicka på den **'SAML-** knappen Logga in på HappyFox med hjälp av Azure AD-kontot.</span><span class="sxs-lookup"><span data-stu-id="cc6bb-227">Click the **‘SAML’** button to log in to HappyFox using your Azure AD account.</span></span>

<span data-ttu-id="cc6bb-228">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cc6bb-228">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cc6bb-229">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cc6bb-229">Additional resources</span></span>

* [<span data-ttu-id="cc6bb-230">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc6bb-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cc6bb-231">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cc6bb-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_203.png

