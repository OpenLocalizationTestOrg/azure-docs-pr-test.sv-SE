---
title: "Självstudier: Azure Active Directory-integrering med Menlo säkerhet | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Menlo säkerhet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 75366abafa551d21630b0edddb65db23b9ea9d42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a><span data-ttu-id="ff058-103">Självstudier: Azure Active Directory-integrering med Menlo säkerhet</span><span class="sxs-lookup"><span data-stu-id="ff058-103">Tutorial: Azure Active Directory integration with Menlo Security</span></span>

<span data-ttu-id="ff058-104">I kursen får lära du att integrera Menlo säkerhet med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ff058-104">In this tutorial, you learn how to integrate Menlo Security with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ff058-105">Integrera Menlo säkerhet med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ff058-105">Integrating Menlo Security with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ff058-106">Du kan styra i Azure AD som har åtkomst till Menlo säkerhet</span><span class="sxs-lookup"><span data-stu-id="ff058-106">You can control in Azure AD who has access to Menlo Security</span></span>
- <span data-ttu-id="ff058-107">Du kan aktivera användarna att automatiskt hämta loggat in på Menlo säkerhet (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ff058-107">You can enable your users to automatically get signed-on to Menlo Security (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ff058-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ff058-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ff058-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns i.</span><span class="sxs-lookup"><span data-stu-id="ff058-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="ff058-110">[Vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ff058-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ff058-111">Krav</span><span class="sxs-lookup"><span data-stu-id="ff058-111">Prerequisites</span></span>

<span data-ttu-id="ff058-112">För att konfigurera Azure AD-integrering med Menlo säkerhet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ff058-112">To configure Azure AD integration with Menlo Security, you need the following items:</span></span>

- <span data-ttu-id="ff058-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ff058-113">An Azure AD subscription</span></span>
- <span data-ttu-id="ff058-114">En Menlo säkerhet enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="ff058-114">A Menlo Security single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ff058-115">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ff058-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ff058-116">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ff058-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ff058-117">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ff058-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ff058-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ff058-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ff058-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ff058-119">Scenario description</span></span>
<span data-ttu-id="ff058-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ff058-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ff058-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ff058-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ff058-122">Lägga till Menlo säkerhet från galleriet</span><span class="sxs-lookup"><span data-stu-id="ff058-122">Adding Menlo Security from the gallery</span></span>
2. <span data-ttu-id="ff058-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff058-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-menlo-security-from-the-gallery"></a><span data-ttu-id="ff058-124">Lägga till Menlo säkerhet från galleriet</span><span class="sxs-lookup"><span data-stu-id="ff058-124">Adding Menlo Security from the gallery</span></span>
<span data-ttu-id="ff058-125">Du måste lägga till Menlo säkerhet från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Menlo säkerhet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ff058-125">To configure the integration of Menlo Security into Azure AD, you need to add Menlo Security from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ff058-126">**Utför följande steg för att lägga till Menlo säkerhet från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ff058-126">**To add Menlo Security from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ff058-127">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ff058-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ff058-129">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ff058-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ff058-130">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ff058-130">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ff058-132">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff058-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ff058-134">I sökrutan skriver **Menlo säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="ff058-134">In the search box, type **Menlo Security**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. <span data-ttu-id="ff058-136">Välj i resultatpanelen **Menlo säkerhet**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ff058-136">In the results panel, select **Menlo Security**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ff058-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff058-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ff058-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Menlo säkerhet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ff058-139">In this section, you configure and test Azure AD single sign-on with Menlo Security based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ff058-140">Azure AD måste du känna till motsvarande användaren i Menlo säkerhet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ff058-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Menlo Security is to a user in Azure AD.</span></span> <span data-ttu-id="ff058-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Menlo säkerhet upprättas.</span><span class="sxs-lookup"><span data-stu-id="ff058-141">In other words, a link relationship between an Azure AD user and the related user in Menlo Security needs to be established.</span></span>

<span data-ttu-id="ff058-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Menlo säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ff058-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Menlo Security.</span></span>

<span data-ttu-id="ff058-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Menlo säkerhet, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ff058-143">To configure and test Azure AD single sign-on with Menlo Security, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ff058-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ff058-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ff058-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff058-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ff058-146">**[Skapa en testanvändare Menlo säkerhet](#creating-a-menlo-security-test-user)**  – har en motsvarighet för Britta Simon Menlo säkerhet som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ff058-146">**[Creating a Menlo Security test user](#creating-a-menlo-security-test-user)** - to have a counterpart of Britta Simon in Menlo Security that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ff058-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ff058-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ff058-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ff058-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ff058-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff058-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ff058-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Menlo säkerhetsprogram.</span><span class="sxs-lookup"><span data-stu-id="ff058-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Menlo Security application.</span></span>

<span data-ttu-id="ff058-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Menlo säkerhet:**</span><span class="sxs-lookup"><span data-stu-id="ff058-151">**To configure Azure AD single sign-on with Menlo Security, perform the following steps:**</span></span>

1. <span data-ttu-id="ff058-152">I Azure-portalen på den **Menlo säkerhet** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ff058-152">In the Azure portal, on the **Menlo Security** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ff058-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ff058-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. <span data-ttu-id="ff058-156">På den **Menlo säkerhetsdomän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ff058-156">On the **Menlo Security Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    <span data-ttu-id="ff058-158">a.</span><span class="sxs-lookup"><span data-stu-id="ff058-158">a.</span></span> <span data-ttu-id="ff058-159">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.menlosecurity.com/account/login`</span><span class="sxs-lookup"><span data-stu-id="ff058-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.menlosecurity.com/account/login`</span></span>

    <span data-ttu-id="ff058-160">b.</span><span class="sxs-lookup"><span data-stu-id="ff058-160">b.</span></span> <span data-ttu-id="ff058-161">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="ff058-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ff058-162">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="ff058-162">These values are not the real.</span></span> <span data-ttu-id="ff058-163">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="ff058-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="ff058-164">Kontakta [Menlo Säkerhetsklient supportteamet](https://www.menlosecurity.com/menlo-contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ff058-164">Contact [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) to get these values.</span></span> 
 
4. <span data-ttu-id="ff058-165">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ff058-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the Certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. <span data-ttu-id="ff058-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ff058-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="ff058-169">På den **Menlo säkerhetskonfiguration** klickar du på **konfigurera säkerheten för Menlo** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ff058-169">On the **Menlo Security Configuration** section, click **Configure Menlo Security** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ff058-170">Kopiera den **SAML enhets-ID**, och **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ff058-170">Copy the **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. <span data-ttu-id="ff058-172">Konfigurera enkel inloggning på **Menlo säkerhet** sida, logga in på den **Menlo säkerhet** webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="ff058-172">To configure single sign-on on **Menlo Security** side, login to the **Menlo Security** website as an administrator.</span></span>

8. <span data-ttu-id="ff058-173">Under **inställningar** gå till **autentisering** och utföra följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="ff058-173">Under **Settings** go to **Authentication** and perform following actions:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    <span data-ttu-id="ff058-175">a.</span><span class="sxs-lookup"><span data-stu-id="ff058-175">a.</span></span> <span data-ttu-id="ff058-176">Markera kryssrutan **aktivera användarautentisering med SAML**.</span><span class="sxs-lookup"><span data-stu-id="ff058-176">Tick the checkbox **Enable user authentication using SAML**.</span></span>

    <span data-ttu-id="ff058-177">b.</span><span class="sxs-lookup"><span data-stu-id="ff058-177">b.</span></span> <span data-ttu-id="ff058-178">Välj **ge extern åtkomst** till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ff058-178">Select **Allow External Access** to **Yes**.</span></span>

    <span data-ttu-id="ff058-179">c.</span><span class="sxs-lookup"><span data-stu-id="ff058-179">c.</span></span> <span data-ttu-id="ff058-180">Under **SAML-providern**väljer **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ff058-180">Under **SAML Provider**, select **Azure Active Directory**.</span></span>

    <span data-ttu-id="ff058-181">d.</span><span class="sxs-lookup"><span data-stu-id="ff058-181">d.</span></span> <span data-ttu-id="ff058-182">**SAML 2.0 Endpoint** : klistra in den **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ff058-182">**SAML 2.0 Endpoint** : Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="ff058-183">e.</span><span class="sxs-lookup"><span data-stu-id="ff058-183">e.</span></span> <span data-ttu-id="ff058-184">**Service-Identifier (utfärdaren)** : klistra in den **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ff058-184">**Service Identifier (Issuer)** : Paste the **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="ff058-185">f.</span><span class="sxs-lookup"><span data-stu-id="ff058-185">f.</span></span> <span data-ttu-id="ff058-186">**X.509-certifikat** : öppna den **certifikat (Base64)** hämtade från Azure-portalen i anteckningar och klistra in den i den här rutan.</span><span class="sxs-lookup"><span data-stu-id="ff058-186">**X.509 Certificate** : Open the **Certificate (Base64)** downloaded from the Azure Portal in notepad and paste it in this box.</span></span>

    <span data-ttu-id="ff058-187">g.</span><span class="sxs-lookup"><span data-stu-id="ff058-187">g.</span></span> <span data-ttu-id="ff058-188">Klicka på **spara** spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="ff058-188">Click **Save** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="ff058-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ff058-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ff058-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ff058-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ff058-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ff058-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ff058-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ff058-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="ff058-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ff058-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ff058-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ff058-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ff058-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ff058-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ff058-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ff058-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ff058-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff058-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ff058-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ff058-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ff058-204">a.</span><span class="sxs-lookup"><span data-stu-id="ff058-204">a.</span></span> <span data-ttu-id="ff058-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ff058-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ff058-206">b.</span><span class="sxs-lookup"><span data-stu-id="ff058-206">b.</span></span> <span data-ttu-id="ff058-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ff058-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ff058-208">c.</span><span class="sxs-lookup"><span data-stu-id="ff058-208">c.</span></span> <span data-ttu-id="ff058-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ff058-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ff058-210">d.</span><span class="sxs-lookup"><span data-stu-id="ff058-210">d.</span></span> <span data-ttu-id="ff058-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ff058-211">Click **Create**.</span></span>
 
### <a name="creating-a-menlo-security-test-user"></a><span data-ttu-id="ff058-212">Skapa en testanvändare Menlo säkerhet</span><span class="sxs-lookup"><span data-stu-id="ff058-212">Creating a Menlo Security test user</span></span>
 
<span data-ttu-id="ff058-213">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Menlo säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ff058-213">In this section, you create a user called Britta Simon in Menlo Security.</span></span> <span data-ttu-id="ff058-214">Arbeta med [Menlo Säkerhetsklient supportteamet](https://www.menlosecurity.com/menlo-contact) att lägga till användare i Menlo Security-plattformen.</span><span class="sxs-lookup"><span data-stu-id="ff058-214">Work with [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) to add the users in the Menlo Security platform.</span></span> <span data-ttu-id="ff058-215">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ff058-215">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ff058-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ff058-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ff058-217">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Menlo säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ff058-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Menlo Security.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ff058-219">**Om du vill tilldela Menlo säkerhet Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ff058-219">**To assign Britta Simon to Menlo Security, perform the following steps:**</span></span>

1. <span data-ttu-id="ff058-220">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ff058-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ff058-222">Välj i listan med program **Menlo säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="ff058-222">In the applications list, select **Menlo Security**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. <span data-ttu-id="ff058-224">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ff058-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ff058-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ff058-226">Click **Add** button.</span></span> <span data-ttu-id="ff058-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff058-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ff058-229">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ff058-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ff058-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff058-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ff058-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ff058-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ff058-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ff058-232">Testing single sign-on</span></span>

<span data-ttu-id="ff058-233">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="ff058-233">In this section, you test your Azure AD single sign-on configuration.</span></span>

<span data-ttu-id="ff058-234">Öppna ett webbläsarfönster i ett ”InPrivate” eller ”Incognito” läge för att utlösa en ny autentisering.</span><span class="sxs-lookup"><span data-stu-id="ff058-234">Open a browser window in an "InPrivate" or "Incognito" mode to trigger a new authentication.</span></span>  <span data-ttu-id="ff058-235">I Internet Explorer, använder du Ctrl + Skift + P.</span><span class="sxs-lookup"><span data-stu-id="ff058-235">In Internet Explorer, use Ctrl+Shift+P.</span></span>  <span data-ttu-id="ff058-236">I Chrome, använder du Ctrl + Shift + N.</span><span class="sxs-lookup"><span data-stu-id="ff058-236">In Chrome, use Ctrl+Shift+N.</span></span>  <span data-ttu-id="ff058-237">Bläddra till en skyddad resurs i privat webbläsarfönster och utföra en Azure AD-inloggning.</span><span class="sxs-lookup"><span data-stu-id="ff058-237">In the private browsing window, browse to a protected resource and perform an Azure AD login.</span></span>  <span data-ttu-id="ff058-238">Efter genomförd inloggning tas du till den begärda platsen i en session för isolering.</span><span class="sxs-lookup"><span data-stu-id="ff058-238">Upon successful login, you will be taken to the requested site in an isolation session.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff058-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ff058-239">Additional resources</span></span>

* [<span data-ttu-id="ff058-240">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ff058-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ff058-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ff058-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

