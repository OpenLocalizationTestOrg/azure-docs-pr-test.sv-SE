---
title: "Självstudier: Azure Active Directory-integrering med antal samverkande SAML SSO av Microsoft | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och växer samman SAML SSO av Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 56de86df9c915fa7c41e3bf0a545cc528cdcf959
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a><span data-ttu-id="08bf2-103">Självstudier: Azure Active Directory-integrering med antal samverkande SAML SSO av Microsoft</span><span class="sxs-lookup"><span data-stu-id="08bf2-103">Tutorial: Azure Active Directory integration with Confluence SAML SSO by Microsoft</span></span>

<span data-ttu-id="08bf2-104">I kursen får lära du att integrera växer samman SAML SSO av Microsoft Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="08bf2-104">In this tutorial, you learn how to integrate Confluence SAML SSO by Microsoft with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="08bf2-105">Integrera växer samman SAML SSO av Microsoft med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="08bf2-105">Integrating Confluence SAML SSO by Microsoft with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="08bf2-106">Du kan styra i Azure AD som har åtkomst till växer samman SAML SSO av Microsoft</span><span class="sxs-lookup"><span data-stu-id="08bf2-106">You can control in Azure AD who has access to Confluence SAML SSO by Microsoft</span></span>
- <span data-ttu-id="08bf2-107">Du kan aktivera användarna att automatiskt hämta loggat in på växer samman SAML SSO av Microsoft (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="08bf2-107">You can enable your users to automatically get signed-on to Confluence SAML SSO by Microsoft (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="08bf2-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="08bf2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="08bf2-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="08bf2-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08bf2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="08bf2-110">Prerequisites</span></span>

<span data-ttu-id="08bf2-111">För att konfigurera Azure AD-integrering med antal samverkande SAML SSO av Microsoft, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="08bf2-111">To configure Azure AD integration with Confluence SAML SSO by Microsoft, you need the following items:</span></span>

- <span data-ttu-id="08bf2-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="08bf2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="08bf2-113">Antal samverkande server installerat program på en Windows 64-bitars server (lokalt eller i molnet IaaS-infrastruktur)</span><span class="sxs-lookup"><span data-stu-id="08bf2-113">Confluence server application installed on a Windows 64-bit server (on premise or on the cloud IaaS infrastructure)</span></span>
- <span data-ttu-id="08bf2-114">Antal samverkande servern är HTTPS-aktiverade</span><span class="sxs-lookup"><span data-stu-id="08bf2-114">Confluence server is HTTPS enabled</span></span>
- <span data-ttu-id="08bf2-115">Observera versionerna som stöds för antal samverkande Plugin nämns i under avsnittet.</span><span class="sxs-lookup"><span data-stu-id="08bf2-115">Note the supported versions for Confluence Plugin are mentioned in below section.</span></span>
- <span data-ttu-id="08bf2-116">Antal samverkande servern kan nås på internet särskilt till Azure AD inloggningssidan för autentisering och ska kunna ta emot token från Azure AD</span><span class="sxs-lookup"><span data-stu-id="08bf2-116">Confluence server is reachable on internet particularly to Azure AD Login page for authentication and should able to receive the token from Azure AD</span></span>
- <span data-ttu-id="08bf2-117">Admin-autentiseringsuppgifter anges i antal samverkande</span><span class="sxs-lookup"><span data-stu-id="08bf2-117">Admin credentials are set up in Confluence</span></span>
- <span data-ttu-id="08bf2-118">WebSudo har inaktiverats i antal samverkande</span><span class="sxs-lookup"><span data-stu-id="08bf2-118">WebSudo is disabled in Confluence</span></span>
- <span data-ttu-id="08bf2-119">Testanvändare som skapats i serverprogrammet växer samman</span><span class="sxs-lookup"><span data-stu-id="08bf2-119">Test user created in the Confluence server application</span></span>

> [!NOTE]
> <span data-ttu-id="08bf2-120">Om du vill testa stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö för växer samman.</span><span class="sxs-lookup"><span data-stu-id="08bf2-120">To test the steps in this tutorial, we do not recommend using a production environment of Confluence.</span></span> <span data-ttu-id="08bf2-121">Testa integreringen först i utvecklings- eller mellanlagringsmiljön för programmet och sedan använda produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="08bf2-121">Test the integration first in development or staging environment of the application and then use the production environment.</span></span>

<span data-ttu-id="08bf2-122">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="08bf2-122">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="08bf2-123">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="08bf2-123">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="08bf2-124">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="08bf2-124">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="supported-versions-of-confluence"></a><span data-ttu-id="08bf2-125">Antal samverkande versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="08bf2-125">Supported versions of Confluence</span></span> 

<span data-ttu-id="08bf2-126">Från och med nu stöds följande antal samverkande-versioner:</span><span class="sxs-lookup"><span data-stu-id="08bf2-126">As of now, following versions of Confluence are supported:</span></span>

- <span data-ttu-id="08bf2-127">Antal samverkande: 5.0 till 5.10</span><span class="sxs-lookup"><span data-stu-id="08bf2-127">Confluence: 5.0 to 5.10</span></span>

## <a name="scenario-description"></a><span data-ttu-id="08bf2-128">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="08bf2-128">Scenario description</span></span>
<span data-ttu-id="08bf2-129">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="08bf2-129">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="08bf2-130">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="08bf2-130">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="08bf2-131">Att lägga till växer samman SAML SSO av Microsoft från galleriet</span><span class="sxs-lookup"><span data-stu-id="08bf2-131">Adding Confluence SAML SSO by Microsoft from the gallery</span></span>
2. <span data-ttu-id="08bf2-132">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08bf2-132">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a><span data-ttu-id="08bf2-133">Att lägga till växer samman SAML SSO av Microsoft från galleriet</span><span class="sxs-lookup"><span data-stu-id="08bf2-133">Adding Confluence SAML SSO by Microsoft from the gallery</span></span>
<span data-ttu-id="08bf2-134">Du måste lägga till växer samman SAML SSO av Microsoft från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av antal samverkande SAML SSO av Microsoft i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08bf2-134">To configure the integration of Confluence SAML SSO by Microsoft into Azure AD, you need to add Confluence SAML SSO by Microsoft from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="08bf2-135">**Utför följande steg för att lägga till växer samman SAML SSO av Microsoft från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="08bf2-135">**To add Confluence SAML SSO by Microsoft from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="08bf2-136">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-136">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="08bf2-138">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-138">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="08bf2-139">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-139">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="08bf2-141">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08bf2-141">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="08bf2-143">I sökrutan skriver **växer samman SAML SSO av Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-143">In the search box, type **Confluence SAML SSO by Microsoft**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. <span data-ttu-id="08bf2-145">Välj i resultatpanelen **växer samman SAML SSO av Microsoft**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="08bf2-145">In the results panel, select **Confluence SAML SSO by Microsoft**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="08bf2-147">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08bf2-147">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="08bf2-148">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="08bf2-148">In this section, you configure and test Azure AD single sign-on with Confluence SAML SSO by Microsoft based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="08bf2-149">Azure AD måste du känna till motsvarande användaren i antal samverkande SAML SSO av Microsoft till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="08bf2-149">For single sign-on to work, Azure AD needs to know what the counterpart user in Confluence SAML SSO by Microsoft is to a user in Azure AD.</span></span> <span data-ttu-id="08bf2-150">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren växer samman SAML SSO av Microsoft upprättas.</span><span class="sxs-lookup"><span data-stu-id="08bf2-150">In other words, a link relationship between an Azure AD user and the related user in Confluence SAML SSO by Microsoft needs to be established.</span></span>

<span data-ttu-id="08bf2-151">Antal samverkande SAML SSO av Microsoft, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-151">In Confluence SAML SSO by Microsoft, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="08bf2-152">Om du vill konfigurera och testa Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="08bf2-152">To configure and test Azure AD single sign-on with Confluence SAML SSO by Microsoft, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="08bf2-153">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-153">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="08bf2-154">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-154">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="08bf2-155">**[Skapa ett antal samverkande SAML SSO av Microsoft testanvändare](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  – du har en motsvarighet för Britta Simon växer samman SAML SSO från Microsoft som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="08bf2-155">**[Creating a Confluence SAML SSO by Microsoft test user](#creating-a-confluence-saml-sso-by-microsoft-test-user)** - to have a counterpart of Britta Simon in Confluence SAML SSO by Microsoft that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="08bf2-156">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="08bf2-156">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="08bf2-157">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="08bf2-157">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="08bf2-158">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08bf2-158">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="08bf2-159">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din växer samman SAML SSO av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="08bf2-159">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Confluence SAML SSO by Microsoft application.</span></span>

<span data-ttu-id="08bf2-160">**Utför följande steg för att konfigurera Azure AD enkel inloggning med antal samverkande SAML SSO av Microsoft:**</span><span class="sxs-lookup"><span data-stu-id="08bf2-160">**To configure Azure AD single sign-on with Confluence SAML SSO by Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="08bf2-161">I Azure-portalen på den **växer samman SAML SSO av Microsoft** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-161">In the Azure portal, on the **Confluence SAML SSO by Microsoft** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="08bf2-163">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="08bf2-163">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. <span data-ttu-id="08bf2-165">På den **växer samman SAML SSO av URL: er och Microsoft Domain** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="08bf2-165">On the **Confluence SAML SSO by Microsoft Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    <span data-ttu-id="08bf2-167">a.</span><span class="sxs-lookup"><span data-stu-id="08bf2-167">a.</span></span> <span data-ttu-id="08bf2-168">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="08bf2-168">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    <span data-ttu-id="08bf2-169">b.</span><span class="sxs-lookup"><span data-stu-id="08bf2-169">b.</span></span> <span data-ttu-id="08bf2-170">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<domain:port>/`</span><span class="sxs-lookup"><span data-stu-id="08bf2-170">In the **Identifier** textbox, type a URL using the following pattern: `https://<domain:port>/`</span></span>

    <span data-ttu-id="08bf2-171">c.</span><span class="sxs-lookup"><span data-stu-id="08bf2-171">c.</span></span> <span data-ttu-id="08bf2-172">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<domain:port>/plugins/servlet/saml/auth`</span><span class="sxs-lookup"><span data-stu-id="08bf2-172">In the **Reply URL** textbox, type a URL using the following pattern: `https://<domain:port>/plugins/servlet/saml/auth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="08bf2-173">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="08bf2-173">These values are not real.</span></span> <span data-ttu-id="08bf2-174">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="08bf2-174">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="08bf2-175">Porten är valfria om det är en namngiven URL.</span><span class="sxs-lookup"><span data-stu-id="08bf2-175">Port is optional in case it’s a named URL.</span></span> <span data-ttu-id="08bf2-176">Dessa värden tas emot under konfigurationen av antal samverkande plugin som beskrivs senare i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="08bf2-176">These values are received during the configuration of Confluence plugin, which is explained later in the tutorial.</span></span>
 

4. <span data-ttu-id="08bf2-177">Att generera den **Metadata** url, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="08bf2-177">To generate the **Metadata** url, perform the following steps:</span></span>

    <span data-ttu-id="08bf2-178">a.</span><span class="sxs-lookup"><span data-stu-id="08bf2-178">a.</span></span> <span data-ttu-id="08bf2-179">Klicka på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-179">Click **App registrations**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    <span data-ttu-id="08bf2-181">b.</span><span class="sxs-lookup"><span data-stu-id="08bf2-181">b.</span></span> <span data-ttu-id="08bf2-182">Klicka på **slutpunkter** att öppna **slutpunkter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08bf2-182">Click **Endpoints** to open **Endpoints** dialog box.</span></span>  
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    <span data-ttu-id="08bf2-184">c.</span><span class="sxs-lookup"><span data-stu-id="08bf2-184">c.</span></span> <span data-ttu-id="08bf2-185">Klicka på kopieringsknappen för att kopiera **FEDERATION METADATADOKUMENTET** url och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="08bf2-185">Click the copy button to copy **FEDERATION METADATA DOCUMENT** url and paste it into notepad.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    <span data-ttu-id="08bf2-187">d.</span><span class="sxs-lookup"><span data-stu-id="08bf2-187">d.</span></span> <span data-ttu-id="08bf2-188">Gå till egenskapssidan för **växer samman SAML SSO av Microsoft** och kopiera den **program-Id** med **kopiera** knappen och klistra in den i anteckningar.</span><span class="sxs-lookup"><span data-stu-id="08bf2-188">Now go to the property page of **Confluence SAML SSO by Microsoft** and copy the **Application Id** using **Copy** button and paste it into notepad.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    <span data-ttu-id="08bf2-190">e.</span><span class="sxs-lookup"><span data-stu-id="08bf2-190">e.</span></span> <span data-ttu-id="08bf2-191">Generera den **URL för tjänstmetadata** med hjälp av följande mönster: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` och kopiera det här värdet i anteckningar eftersom den används senare för konfiguration av plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="08bf2-191">Generate the **Metadata URL** using the following pattern: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` and copy this value in notepad as it is used later for the configuration of the plugin.</span></span>

5. <span data-ttu-id="08bf2-192">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-192">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="08bf2-194">Kontakta [Microsoft](mailto:waadpartners@microsoft.com) med följande information för antal samverkande plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="08bf2-194">Contact [Microsoft](mailto:waadpartners@microsoft.com) with the following information for the Confluence plugin.</span></span>
    
    *   <span data-ttu-id="08bf2-195">Kundnamn:</span><span class="sxs-lookup"><span data-stu-id="08bf2-195">Customer Name:</span></span>
    *   <span data-ttu-id="08bf2-196">Namn på primär domän:</span><span class="sxs-lookup"><span data-stu-id="08bf2-196">Primary domain name:</span></span>
    *   <span data-ttu-id="08bf2-197">Azure AD Premium: Ja/Nej (plugin-program är tillgängliga för alla kunder lediga, grundläggande och Premium-SKU)</span><span class="sxs-lookup"><span data-stu-id="08bf2-197">Azure AD Premium: Yes/No (Plugin will be available to all     the customer Free, Basic, and Premium SKU)</span></span>
    *   <span data-ttu-id="08bf2-198">Antalet användare som kommer att använda den här integreringen:</span><span class="sxs-lookup"><span data-stu-id="08bf2-198">Number of users who will be using this integration:</span></span>
    *   <span data-ttu-id="08bf2-199">Antal samverkande Version:</span><span class="sxs-lookup"><span data-stu-id="08bf2-199">Confluence Version:</span></span>
    *   <span data-ttu-id="08bf2-200">Kommentarer:</span><span class="sxs-lookup"><span data-stu-id="08bf2-200">Comments:</span></span>

7. <span data-ttu-id="08bf2-201">I en annan webbläsarfönster loggar du in till växer samman-instans som administratör.</span><span class="sxs-lookup"><span data-stu-id="08bf2-201">In a different web browser window, log in to your Confluence instance as an administrator.</span></span>

8. <span data-ttu-id="08bf2-202">Hovra över kugge och klicka på den **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-202">Hover on cog and click the **Add-ons**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

9. <span data-ttu-id="08bf2-204">Klicka under tillägg fliken avsnitt **Hantera tillägg**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-204">Under Add-ons tab section, click **Manage add-ons**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon72.png)

10. <span data-ttu-id="08bf2-206">Ladda upp plugin-programmet som tillhandahålls av Microsoft manuellt.</span><span class="sxs-lookup"><span data-stu-id="08bf2-206">Manually upload the plugin provided by Microsoft.</span></span> <span data-ttu-id="08bf2-207">När plugin-programmet installeras, visas det i **användaren installerat** tillägg avsnitt i **Hantera tillägg** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="08bf2-207">Once the plugin is installed, it appears in **User Installed** add-ons section of **Manage Add-on** section.</span></span>

11. <span data-ttu-id="08bf2-208">Klicka på **konfigurera** att konfigurera nya plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="08bf2-208">Click **Configure** to configure the new plugin.</span></span>

12. <span data-ttu-id="08bf2-209">Utför följande steg på konfigurationssidan:</span><span class="sxs-lookup"><span data-stu-id="08bf2-209">Perform following steps on configuration page:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon5.png)
 
    <span data-ttu-id="08bf2-211">a.</span><span class="sxs-lookup"><span data-stu-id="08bf2-211">a.</span></span> <span data-ttu-id="08bf2-212">I **URL för tjänstmetadata** klistra in den **URL för tjänstmetadata** genereras från Azure AD och klicka på den **lösa** knappen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-212">In **Metadata URL** paste the **Metadata URL** generated from Azure AD and click the **Resolve** button.</span></span> <span data-ttu-id="08bf2-213">Den läser IdP metadata-URL och fyller i informationen för fält.</span><span class="sxs-lookup"><span data-stu-id="08bf2-213">It reads the IdP metadata URL and populates all the fields information.</span></span>

    > [!Note]
    > <span data-ttu-id="08bf2-214">Standardplatsen för SAML användar-ID är namnidentifierare.</span><span class="sxs-lookup"><span data-stu-id="08bf2-214">Default SAML User ID location is Name Identifier.</span></span> <span data-ttu-id="08bf2-215">Du kan ändra det till ett attributalternativ och ange lämpliga attributets namn.</span><span class="sxs-lookup"><span data-stu-id="08bf2-215">You can change this to an attribute option and enter the appropriate attribute name.</span></span>

    > [!TIP]
    > <span data-ttu-id="08bf2-216">Kontrollera att det finns bara ett certifikat mappas mot appen så att det finns inga fel vid matchning av metadata.</span><span class="sxs-lookup"><span data-stu-id="08bf2-216">Ensure that there is only one certificate mapped against the app so that there is no error in resolving the metadata.</span></span> <span data-ttu-id="08bf2-217">Om det finns flera certifikat vid lösning av metadata, får administratör ett fel.</span><span class="sxs-lookup"><span data-stu-id="08bf2-217">If there are multiple certificates, upon resolving the metadata, admin gets an error.</span></span>
    
    <span data-ttu-id="08bf2-218">b.</span><span class="sxs-lookup"><span data-stu-id="08bf2-218">b.</span></span> <span data-ttu-id="08bf2-219">Kopiera den **identifierare, svars-URL: en och URL: en inloggning** värden och klistra in dem i **identifierare, svars-URL: en och URL: en inloggning** textrutor respektive i **växer samman SAML SSO av URL: er och Microsoft Domain** avsnitt på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-219">Copy the **Identifier, Reply URL and Sign on URL** values and paste them in **Identifier, Reply URL and Sign on URL** textboxes respectively in **Confluence SAML SSO by Microsoft Domain and URLs** section on Azure portal.</span></span>

    <span data-ttu-id="08bf2-220">c.</span><span class="sxs-lookup"><span data-stu-id="08bf2-220">c.</span></span> <span data-ttu-id="08bf2-221">I **knappen inloggningsnamnet** skriver du namnet på knappen organisationen vill att användarna ska se på inloggningsskärmen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-221">In **Login Button Name** type the name of button your organization wants the users to see on login screen.</span></span>

    <span data-ttu-id="08bf2-222">d.</span><span class="sxs-lookup"><span data-stu-id="08bf2-222">d.</span></span> <span data-ttu-id="08bf2-223">I **SAML användar-ID platser** väljer du antingen **användar-ID är i elementet NameIdentifier i instruktionen ämne** eller **användar-ID är i ett element med attributet**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-223">In **SAML User ID Locations** select either **User ID is in the NameIdentifier element of the Subject statement** or **User ID is in an Attribute element**.</span></span>  <span data-ttu-id="08bf2-224">Detta ID måste vara växer samman användar-id.</span><span class="sxs-lookup"><span data-stu-id="08bf2-224">This ID has to be the Confluence user id.</span></span> <span data-ttu-id="08bf2-225">Om det användar-id inte matchas sedan kan inte användare att logga in.</span><span class="sxs-lookup"><span data-stu-id="08bf2-225">If the user id is not matched, then system will not allow users to log in.</span></span> 
    
    <span data-ttu-id="08bf2-226">e.</span><span class="sxs-lookup"><span data-stu-id="08bf2-226">e.</span></span> <span data-ttu-id="08bf2-227">Om du väljer **användar-ID är i ett element med attributet** alternativet i **attributnamn** textrutan anger du namnet på attributet som där användar-Id förväntas.</span><span class="sxs-lookup"><span data-stu-id="08bf2-227">If you select **User ID is in an Attribute element** option, then in **Attribute name** textbox type the name of the attribute where User Id is expected.</span></span> 

    <span data-ttu-id="08bf2-228">f.</span><span class="sxs-lookup"><span data-stu-id="08bf2-228">f.</span></span> <span data-ttu-id="08bf2-229">Om du använder den federerade domänen (t.ex. AD FS etc.) med Azure AD, klicka på den **aktivera identifiering av startsfär** och konfigurera den **domännamn**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-229">If you are using the federated domain (like ADFS etc.) with Azure AD, then click on the **Enable Home Realm Discovery** option and configure the **Domain Name**.</span></span>
    
    <span data-ttu-id="08bf2-230">g.</span><span class="sxs-lookup"><span data-stu-id="08bf2-230">g.</span></span> <span data-ttu-id="08bf2-231">I **domännamn** anger du domännamnet här vid inloggningen ADFS-baserade.</span><span class="sxs-lookup"><span data-stu-id="08bf2-231">In **Domain Name** type the domain name here in case of the ADFS-based login.</span></span>

    <span data-ttu-id="08bf2-232">h.</span><span class="sxs-lookup"><span data-stu-id="08bf2-232">h.</span></span> <span data-ttu-id="08bf2-233">Kontrollera **aktivera enkel inloggning ut** om du vill logga ut från Azure AD när en användare loggar från växer samman.</span><span class="sxs-lookup"><span data-stu-id="08bf2-233">Check **Enable Single Sign out** if you wish to log out from Azure AD when a user logs out from Confluence.</span></span> 

    <span data-ttu-id="08bf2-234">Jag.</span><span class="sxs-lookup"><span data-stu-id="08bf2-234">i.</span></span> <span data-ttu-id="08bf2-235">Klicka på **spara** för att spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="08bf2-235">Click **Save** button to save the settings.</span></span>


> [!TIP]
> <span data-ttu-id="08bf2-236">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="08bf2-236">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="08bf2-237">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="08bf2-237">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="08bf2-238">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="08bf2-238">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="08bf2-239">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="08bf2-239">Creating an Azure AD test user</span></span>
<span data-ttu-id="08bf2-240">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-240">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="08bf2-242">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="08bf2-242">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="08bf2-243">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-243">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="08bf2-245">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-245">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="08bf2-247">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08bf2-247">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="08bf2-249">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="08bf2-249">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="08bf2-251">a.</span><span class="sxs-lookup"><span data-stu-id="08bf2-251">a.</span></span> <span data-ttu-id="08bf2-252">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-252">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="08bf2-253">b.</span><span class="sxs-lookup"><span data-stu-id="08bf2-253">b.</span></span> <span data-ttu-id="08bf2-254">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-254">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="08bf2-255">c.</span><span class="sxs-lookup"><span data-stu-id="08bf2-255">c.</span></span> <span data-ttu-id="08bf2-256">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-256">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="08bf2-257">d.</span><span class="sxs-lookup"><span data-stu-id="08bf2-257">d.</span></span> <span data-ttu-id="08bf2-258">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-258">Click **Create**.</span></span>
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a><span data-ttu-id="08bf2-259">Skapa ett antal samverkande SAML SSO av Microsoft testanvändare</span><span class="sxs-lookup"><span data-stu-id="08bf2-259">Creating a Confluence SAML SSO by Microsoft test user</span></span>

<span data-ttu-id="08bf2-260">Om du vill aktivera Azure AD-användare kan logga in på växer samman på lokal server, måste de etableras i antal samverkande SAML SSO av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="08bf2-260">To enable Azure AD users to log in to Confluence on premise server, they must be provisioned into Confluence SAML SSO by Microsoft.</span></span> <span data-ttu-id="08bf2-261">Antal samverkande SAML SSO av Microsoft är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="08bf2-261">For Confluence SAML SSO by Microsoft, provisioning is a manual task.</span></span>

<span data-ttu-id="08bf2-262">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="08bf2-262">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="08bf2-263">Logga in på ditt växer samman på lokal server som administratör.</span><span class="sxs-lookup"><span data-stu-id="08bf2-263">Log in to your Confluence on premise server as an administrator.</span></span>

2. <span data-ttu-id="08bf2-264">Hovra över kugge och klicka på den **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-264">Hover on cog and click the **User management**.</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. <span data-ttu-id="08bf2-266">Under avsnittet för användare, klickar du på **lägga till användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="08bf2-266">Under Users section, click **Add users** tab.</span></span> <span data-ttu-id="08bf2-267">På den **”Lägg till en användare”** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="08bf2-267">On the **“Add a User”** dialog page, perform the following steps:</span></span>

    ![Lägga till medarbetare](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    <span data-ttu-id="08bf2-269">a.</span><span class="sxs-lookup"><span data-stu-id="08bf2-269">a.</span></span> <span data-ttu-id="08bf2-270">I den **användarnamn** textruta Skriv e-postadressen för användaren som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-270">In the **Username** textbox, type the email of user like Britta Simon.</span></span>

    <span data-ttu-id="08bf2-271">b.</span><span class="sxs-lookup"><span data-stu-id="08bf2-271">b.</span></span> <span data-ttu-id="08bf2-272">I den **fullständiga namn** textruta skriver du det fullständiga namnet på användaren som Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-272">In the **Full Name** textbox, type the full name of user like Britta Simon.</span></span>

    <span data-ttu-id="08bf2-273">c.</span><span class="sxs-lookup"><span data-stu-id="08bf2-273">c.</span></span> <span data-ttu-id="08bf2-274">I den **e-post** textruta typen e-postadressen för användaren som Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="08bf2-274">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="08bf2-275">d.</span><span class="sxs-lookup"><span data-stu-id="08bf2-275">d.</span></span> <span data-ttu-id="08bf2-276">I den **lösenord** textruta, Skriv in lösenordet för Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08bf2-276">In the **Password** textbox, type the password for Britta Simon.</span></span>

    <span data-ttu-id="08bf2-277">e.</span><span class="sxs-lookup"><span data-stu-id="08bf2-277">e.</span></span> <span data-ttu-id="08bf2-278">Klicka på **Bekräfta lösenord** ange lösenordet igen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-278">Click **Confirm Password** reenter the password.</span></span>
    
    <span data-ttu-id="08bf2-279">f.</span><span class="sxs-lookup"><span data-stu-id="08bf2-279">f.</span></span> <span data-ttu-id="08bf2-280">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-280">Click **Add** button.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="08bf2-281">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="08bf2-281">Assigning the Azure AD test user</span></span>

<span data-ttu-id="08bf2-282">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till växer samman SAML SSO av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="08bf2-282">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Confluence SAML SSO by Microsoft.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="08bf2-284">**Om du vill tilldela växer samman SAML SSO av Microsoft Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="08bf2-284">**To assign Britta Simon to Confluence SAML SSO by Microsoft, perform the following steps:**</span></span>

1. <span data-ttu-id="08bf2-285">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-285">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="08bf2-287">Välj i listan med program **växer samman SAML SSO av Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-287">In the applications list, select **Confluence SAML SSO by Microsoft**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. <span data-ttu-id="08bf2-289">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="08bf2-289">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="08bf2-291">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="08bf2-291">Click **Add** button.</span></span> <span data-ttu-id="08bf2-292">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08bf2-292">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="08bf2-294">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="08bf2-294">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="08bf2-295">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08bf2-295">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="08bf2-296">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="08bf2-296">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="08bf2-297">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="08bf2-297">Testing single sign-on</span></span>

<span data-ttu-id="08bf2-298">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="08bf2-298">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="08bf2-299">När du klickar på växer samman SAML SSO av Microsoft-panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt växer samman SAML SSO av Microsoft-program.</span><span class="sxs-lookup"><span data-stu-id="08bf2-299">When you click the Confluence SAML SSO by Microsoft tile in the Access Panel, you should get automatically signed-on to your Confluence SAML SSO by Microsoft application.</span></span>
<span data-ttu-id="08bf2-300">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="08bf2-300">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08bf2-301">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="08bf2-301">Additional resources</span></span>

* [<span data-ttu-id="08bf2-302">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="08bf2-302">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="08bf2-303">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="08bf2-303">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

