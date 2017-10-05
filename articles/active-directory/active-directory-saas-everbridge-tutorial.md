---
title: "Självstudier: Azure Active Directory-integrering med EverBridge | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och EverBridge."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: f5a97fc8df978dd55a73ae53516a82f884c14bec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a><span data-ttu-id="f9603-103">Självstudier: Azure Active Directory-integrering med EverBridge</span><span class="sxs-lookup"><span data-stu-id="f9603-103">Tutorial: Azure Active Directory integration with EverBridge</span></span>

<span data-ttu-id="f9603-104">I kursen får lära du att integrera EverBridge med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="f9603-104">In this tutorial, you learn how to integrate EverBridge with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f9603-105">Integrera EverBridge med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="f9603-105">Integrating EverBridge with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f9603-106">Du kan styra i Azure AD som har åtkomst till EverBridge</span><span class="sxs-lookup"><span data-stu-id="f9603-106">You can control in Azure AD who has access to EverBridge</span></span>
- <span data-ttu-id="f9603-107">Du kan aktivera användarna att automatiskt hämta loggat in på EverBridge (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="f9603-107">You can enable your users to automatically get signed-on to EverBridge (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f9603-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="f9603-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f9603-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f9603-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f9603-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f9603-110">Prerequisites</span></span>

<span data-ttu-id="f9603-111">För att konfigurera Azure AD-integrering med EverBridge, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f9603-111">To configure Azure AD integration with EverBridge, you need the following items:</span></span>

- <span data-ttu-id="f9603-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="f9603-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f9603-113">En EverBridge enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="f9603-113">An EverBridge single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f9603-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f9603-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f9603-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="f9603-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f9603-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f9603-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f9603-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f9603-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f9603-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="f9603-118">Scenario description</span></span>
<span data-ttu-id="f9603-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="f9603-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f9603-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="f9603-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f9603-121">Att lägga till EverBridge från galleriet</span><span class="sxs-lookup"><span data-stu-id="f9603-121">Adding EverBridge from the gallery</span></span>
2. <span data-ttu-id="f9603-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9603-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-everbridge-from-the-gallery"></a><span data-ttu-id="f9603-123">Att lägga till EverBridge från galleriet</span><span class="sxs-lookup"><span data-stu-id="f9603-123">Adding EverBridge from the gallery</span></span>
<span data-ttu-id="f9603-124">Du måste lägga till EverBridge från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av EverBridge i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9603-124">To configure the integration of EverBridge into Azure AD, you need to add EverBridge from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f9603-125">**Utför följande steg för att lägga till EverBridge från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="f9603-125">**To add EverBridge from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f9603-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f9603-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f9603-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="f9603-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f9603-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f9603-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="f9603-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9603-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="f9603-133">I sökrutan skriver **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="f9603-133">In the search box, type **EverBridge**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_search.png)

5. <span data-ttu-id="f9603-135">Välj i resultatpanelen **EverBridge**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="f9603-135">In the results panel, select **EverBridge**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f9603-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9603-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f9603-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med EverBridge baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="f9603-138">In this section, you configure and test Azure AD single sign-on with EverBridge based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f9603-139">Azure AD måste du känna till användaren i EverBridge motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="f9603-139">For single sign-on to work, Azure AD needs to know what the counterpart user in EverBridge is to a user in Azure AD.</span></span> <span data-ttu-id="f9603-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i EverBridge upprättas.</span><span class="sxs-lookup"><span data-stu-id="f9603-140">In other words, a link relationship between an Azure AD user and the related user in EverBridge needs to be established.</span></span>

<span data-ttu-id="f9603-141">I EverBridge, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="f9603-141">In EverBridge, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f9603-142">Om du vill konfigurera och testa Azure AD enkel inloggning med EverBridge, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="f9603-142">To configure and test Azure AD single sign-on with EverBridge, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f9603-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="f9603-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f9603-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f9603-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f9603-145">**[Skapa en testanvändare EverBridge](#creating-an-everbridge-test-user)**  – du har en motsvarighet för Britta Simon i EverBridge som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="f9603-145">**[Creating an EverBridge test user](#creating-an-everbridge-test-user)** - to have a counterpart of Britta Simon in EverBridge that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f9603-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f9603-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f9603-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="f9603-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f9603-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9603-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f9603-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt EverBridge program.</span><span class="sxs-lookup"><span data-stu-id="f9603-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your EverBridge application.</span></span>

<span data-ttu-id="f9603-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med EverBridge:**</span><span class="sxs-lookup"><span data-stu-id="f9603-150">**To configure Azure AD single sign-on with EverBridge, perform the following steps:**</span></span>

1. <span data-ttu-id="f9603-151">I Azure-portalen på den **EverBridge** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="f9603-151">In the Azure portal, on the **EverBridge** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="f9603-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="f9603-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_samlbase.png)

3. <span data-ttu-id="f9603-155">På den **EverBridge domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f9603-155">On the **EverBridge Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_url.png)

    <span data-ttu-id="f9603-157">a.</span><span class="sxs-lookup"><span data-stu-id="f9603-157">a.</span></span> <span data-ttu-id="f9603-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://sso.everbridge.net/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="f9603-158">In the **Identifier** textbox, type a URL using the following pattern: `https://sso.everbridge.net/<companyname>`</span></span>

    <span data-ttu-id="f9603-159">b.</span><span class="sxs-lookup"><span data-stu-id="f9603-159">b.</span></span> <span data-ttu-id="f9603-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span><span class="sxs-lookup"><span data-stu-id="f9603-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://manager.everbridge.net/saml/SSO/<companyname>/alias/defaultAlias`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f9603-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="f9603-161">These values are not real.</span></span> <span data-ttu-id="f9603-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="f9603-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="f9603-163">Kontakta [EverBridge supportteamet](mailto:support@everbridge.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="f9603-163">Contact [EverBridge support team](mailto:support@everbridge.com) to get these values.</span></span>
 
4. <span data-ttu-id="f9603-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="f9603-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_certificate.png) 

5. <span data-ttu-id="f9603-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="f9603-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f9603-168">På den **EverBridge Configuration** klickar du på **konfigurera EverBridge** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="f9603-168">On the **EverBridge Configuration** section, click **Configure EverBridge** to open **Configure sign-on** window.</span></span> <span data-ttu-id="f9603-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="f9603-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_configure.png) 

6. <span data-ttu-id="f9603-171">För att få SSO konfigurerats för ditt program måste logga in till din Everbridge-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="f9603-171">To get SSO configured for your application, you need to sign-on to your Everbridge tenant as an administrator.</span></span>

7. <span data-ttu-id="f9603-172">Klicka på menyn högst upp i **inställningar** fliken och markera **enkel inloggning** under **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="f9603-172">In the menu on the top, click the **Settings** tab and select **Single Sign-On** under **Security**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)
   
    <span data-ttu-id="f9603-174">a.</span><span class="sxs-lookup"><span data-stu-id="f9603-174">a.</span></span> <span data-ttu-id="f9603-175">I den **namn** textruta skriver du namnet på ID-providern (till exempel: företagets namn).</span><span class="sxs-lookup"><span data-stu-id="f9603-175">In the **Name** textbox, type the name of Identifier Provider (for example: your company name).</span></span>
   
    <span data-ttu-id="f9603-176">b.</span><span class="sxs-lookup"><span data-stu-id="f9603-176">b.</span></span> <span data-ttu-id="f9603-177">I den **API-namnet** textruta skriver du namnet på API.</span><span class="sxs-lookup"><span data-stu-id="f9603-177">In the **API Name** textbox, type the name of API.</span></span>
   
    <span data-ttu-id="f9603-178">c.</span><span class="sxs-lookup"><span data-stu-id="f9603-178">c.</span></span> <span data-ttu-id="f9603-179">Klicka på **Välj fil** för att överföra metadatafilen som du laddade ned från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f9603-179">Click **Choose File** button to upload the metadata file which you downloaded from Azure portal.</span></span>
   
    <span data-ttu-id="f9603-180">d.</span><span class="sxs-lookup"><span data-stu-id="f9603-180">d.</span></span> <span data-ttu-id="f9603-181">Markera i platsen för SAML-identitet **identitet är i elementet NameIdentifier i instruktionen ämne**.</span><span class="sxs-lookup"><span data-stu-id="f9603-181">In the SAML Identity Location, select **Identity is in the NameIdentifier element of the Subject statement**.</span></span>
   
    <span data-ttu-id="f9603-182">e.</span><span class="sxs-lookup"><span data-stu-id="f9603-182">e.</span></span> <span data-ttu-id="f9603-183">I den **identitet providern inloggnings-URL** textruta klistra in värdet för SAML SSO-URL från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="f9603-183">In the **Identity Provider Login URL** textbox, paste the value of SAML SSO URL from Azure AD.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)
   
    <span data-ttu-id="f9603-185">f.</span><span class="sxs-lookup"><span data-stu-id="f9603-185">f.</span></span> <span data-ttu-id="f9603-186">I den Service Provider initierade begära bindning, väljer **HTTP-omdirigering**.</span><span class="sxs-lookup"><span data-stu-id="f9603-186">In the Service Provider Initiated Request Binding, select **HTTP Redirect**.</span></span>

    <span data-ttu-id="f9603-187">g.</span><span class="sxs-lookup"><span data-stu-id="f9603-187">g.</span></span> <span data-ttu-id="f9603-188">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="f9603-188">Click **Save**</span></span>

> [!TIP]
> <span data-ttu-id="f9603-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="f9603-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f9603-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="f9603-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f9603-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f9603-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f9603-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="f9603-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="f9603-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f9603-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="f9603-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="f9603-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f9603-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="f9603-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f9603-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="f9603-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f9603-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9603-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f9603-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="f9603-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f9603-204">a.</span><span class="sxs-lookup"><span data-stu-id="f9603-204">a.</span></span> <span data-ttu-id="f9603-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f9603-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f9603-206">b.</span><span class="sxs-lookup"><span data-stu-id="f9603-206">b.</span></span> <span data-ttu-id="f9603-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f9603-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f9603-208">c.</span><span class="sxs-lookup"><span data-stu-id="f9603-208">c.</span></span> <span data-ttu-id="f9603-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="f9603-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f9603-210">d.</span><span class="sxs-lookup"><span data-stu-id="f9603-210">d.</span></span> <span data-ttu-id="f9603-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f9603-211">Click **Create**.</span></span>
 
### <a name="creating-an-everbridge-test-user"></a><span data-ttu-id="f9603-212">Skapa en testanvändare EverBridge</span><span class="sxs-lookup"><span data-stu-id="f9603-212">Creating an EverBridge test user</span></span>

<span data-ttu-id="f9603-213">I det här avsnittet skapar du en användare som kallas Britta Simon i Everbridge.</span><span class="sxs-lookup"><span data-stu-id="f9603-213">In this section, you create a user called Britta Simon in Everbridge.</span></span> <span data-ttu-id="f9603-214">Arbeta med [EverBridge supportteamet](mailto:support@everbridge.com) att lägga till användare i Everbridge-plattformen.</span><span class="sxs-lookup"><span data-stu-id="f9603-214">Work with [EverBridge support team](mailto:support@everbridge.com) to add the users in the Everbridge platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f9603-215">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="f9603-215">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f9603-216">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till EverBridge.</span><span class="sxs-lookup"><span data-stu-id="f9603-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to EverBridge.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="f9603-218">**Om du vill tilldela EverBridge Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="f9603-218">**To assign Britta Simon to EverBridge, perform the following steps:**</span></span>

1. <span data-ttu-id="f9603-219">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="f9603-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="f9603-221">Välj i listan med program **EverBridge**.</span><span class="sxs-lookup"><span data-stu-id="f9603-221">In the applications list, select **EverBridge**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_app.png) 

3. <span data-ttu-id="f9603-223">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="f9603-223">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="f9603-225">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="f9603-225">Click **Add** button.</span></span> <span data-ttu-id="f9603-226">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9603-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="f9603-228">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="f9603-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f9603-229">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9603-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f9603-230">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="f9603-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f9603-231">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="f9603-231">Testing single sign-on</span></span>

<span data-ttu-id="f9603-232">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="f9603-232">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f9603-233">När du klickar på panelen Everbridge på åtkomstpanelen du bör få automatiskt loggat in på ditt Everbridge program.</span><span class="sxs-lookup"><span data-stu-id="f9603-233">When you click the Everbridge tile in the Access Panel, you should get automatically signed-on to your Everbridge application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9603-234">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="f9603-234">Additional resources</span></span>

* [<span data-ttu-id="f9603-235">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f9603-235">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f9603-236">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="f9603-236">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png

