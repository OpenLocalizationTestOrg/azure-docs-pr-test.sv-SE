---
title: "Självstudier: Azure Active Directory-integrering med Ceridian Dayforce HCM | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Ceridian Dayforce HCM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: b2ea3d92f233dab5bd6814e4875f881117eac8e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a><span data-ttu-id="c1c71-103">Självstudier: Azure Active Directory-integrering med Ceridian Dayforce HCM</span><span class="sxs-lookup"><span data-stu-id="c1c71-103">Tutorial: Azure Active Directory integration with Ceridian Dayforce HCM</span></span>

<span data-ttu-id="c1c71-104">I kursen får lära du att integrera Ceridian Dayforce HCM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="c1c71-104">In this tutorial, you learn how to integrate Ceridian Dayforce HCM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c1c71-105">Integrera Ceridian Dayforce HCM med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="c1c71-105">Integrating Ceridian Dayforce HCM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c1c71-106">Du kan styra i Azure AD som har åtkomst till Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="c1c71-106">You can control in Azure AD who has access to Ceridian Dayforce HCM.</span></span>
- <span data-ttu-id="c1c71-107">Du kan aktivera användarna att automatiskt hämta loggat in på Ceridian Dayforce HCM (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="c1c71-107">You can enable your users to automatically get signed-on to Ceridian Dayforce HCM (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c1c71-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c1c71-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="c1c71-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c1c71-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c1c71-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c1c71-110">Prerequisites</span></span>

<span data-ttu-id="c1c71-111">Om du vill konfigurera Azure AD-integrering med Ceridian Dayforce HCM behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="c1c71-111">To configure Azure AD integration with Ceridian Dayforce HCM, you need the following items:</span></span>

- <span data-ttu-id="c1c71-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="c1c71-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c1c71-113">En Ceridian Dayforce HCM enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="c1c71-113">A Ceridian Dayforce HCM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c1c71-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="c1c71-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c1c71-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="c1c71-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c1c71-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="c1c71-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c1c71-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c1c71-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c1c71-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="c1c71-118">Scenario description</span></span>
<span data-ttu-id="c1c71-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="c1c71-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c1c71-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="c1c71-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c1c71-121">Att lägga till Ceridian Dayforce HCM från galleriet</span><span class="sxs-lookup"><span data-stu-id="c1c71-121">Adding Ceridian Dayforce HCM from the gallery</span></span>
2. <span data-ttu-id="c1c71-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c71-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a><span data-ttu-id="c1c71-123">Att lägga till Ceridian Dayforce HCM från galleriet</span><span class="sxs-lookup"><span data-stu-id="c1c71-123">Adding Ceridian Dayforce HCM from the gallery</span></span>
<span data-ttu-id="c1c71-124">Du måste lägga till Ceridian Dayforce HCM från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Ceridian Dayforce HCM i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1c71-124">To configure the integration of Ceridian Dayforce HCM into Azure AD, you need to add Ceridian Dayforce HCM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c1c71-125">**Utför följande steg för att lägga till Ceridian Dayforce HCM från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="c1c71-125">**To add Ceridian Dayforce HCM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c71-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="c1c71-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="c1c71-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c1c71-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="c1c71-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="c1c71-133">I sökrutan skriver **Ceridian Dayforce HCM**väljer **Ceridian Dayforce HCM** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="c1c71-133">In the search box, type **Ceridian Dayforce HCM**, select **Ceridian Dayforce HCM** from result panel then click **Add** button to add the application.</span></span>

    ![Ceridian Dayforce HCM i resultatlistan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c1c71-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c71-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c1c71-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Ceridian Dayforce HCM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="c1c71-136">In this section, you configure and test Azure AD single sign-on with Ceridian Dayforce HCM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c1c71-137">Azure AD måste du känna till användaren i Ceridian Dayforce HCM motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="c1c71-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Ceridian Dayforce HCM is to a user in Azure AD.</span></span> <span data-ttu-id="c1c71-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Ceridian Dayforce HCM upprättas.</span><span class="sxs-lookup"><span data-stu-id="c1c71-138">In other words, a link relationship between an Azure AD user and the related user in Ceridian Dayforce HCM needs to be established.</span></span>

<span data-ttu-id="c1c71-139">I Ceridian Dayforce HCM tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="c1c71-139">In Ceridian Dayforce HCM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c1c71-140">Om du vill konfigurera och testa Azure AD enkel inloggning med Ceridian Dayforce HCM, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="c1c71-140">To configure and test Azure AD single sign-on with Ceridian Dayforce HCM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c1c71-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="c1c71-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c1c71-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1c71-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c1c71-143">**[Skapa en testanvändare Ceridian Dayforce HCM](#create-a-ceridian-dayforce-hcm-test-user)**  – har en motsvarighet för Britta Simon Ceridian Dayforce HCM som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="c1c71-143">**[Create a Ceridian Dayforce HCM test user](#create-a-ceridian-dayforce-hcm-test-user)** - to have a counterpart of Britta Simon in Ceridian Dayforce HCM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c1c71-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c1c71-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c1c71-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="c1c71-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c1c71-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c71-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c1c71-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Ceridian Dayforce HCM-program.</span><span class="sxs-lookup"><span data-stu-id="c1c71-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Ceridian Dayforce HCM application.</span></span>

<span data-ttu-id="c1c71-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Ceridian Dayforce HCM:**</span><span class="sxs-lookup"><span data-stu-id="c1c71-148">**To configure Azure AD single sign-on with Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c71-149">I Azure-portalen på den **Ceridian Dayforce HCM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-149">In the Azure portal, on the **Ceridian Dayforce HCM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="c1c71-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="c1c71-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. <span data-ttu-id="c1c71-153">På den **Ceridian Dayforce HCM domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1c71-153">On the **Ceridian Dayforce HCM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    <span data-ttu-id="c1c71-155">a.</span><span class="sxs-lookup"><span data-stu-id="c1c71-155">a.</span></span> <span data-ttu-id="c1c71-156">I den **logga URL** textruta, Skriv URL: en som används av dina användare logga in i tillämpningsprogrammet Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="c1c71-156">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Ceridian Dayforce HCM application.</span></span>
    
    | <span data-ttu-id="c1c71-157">Miljö</span><span class="sxs-lookup"><span data-stu-id="c1c71-157">Environment</span></span> | <span data-ttu-id="c1c71-158">URL: EN</span><span class="sxs-lookup"><span data-stu-id="c1c71-158">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="c1c71-159">För produktion</span><span class="sxs-lookup"><span data-stu-id="c1c71-159">For production</span></span> | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | <span data-ttu-id="c1c71-160">För testet</span><span class="sxs-lookup"><span data-stu-id="c1c71-160">For test</span></span> | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    <span data-ttu-id="c1c71-161">b.</span><span class="sxs-lookup"><span data-stu-id="c1c71-161">b.</span></span> <span data-ttu-id="c1c71-162">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="c1c71-162">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    
    | <span data-ttu-id="c1c71-163">Miljö</span><span class="sxs-lookup"><span data-stu-id="c1c71-163">Environment</span></span> | <span data-ttu-id="c1c71-164">URL: EN</span><span class="sxs-lookup"><span data-stu-id="c1c71-164">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="c1c71-165">För produktion</span><span class="sxs-lookup"><span data-stu-id="c1c71-165">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp` |
    | <span data-ttu-id="c1c71-166">För testet</span><span class="sxs-lookup"><span data-stu-id="c1c71-166">For test</span></span> | `https://fs-test.dayforcehcm.com/sp` |
    
    <span data-ttu-id="c1c71-167">c.</span><span class="sxs-lookup"><span data-stu-id="c1c71-167">c.</span></span> <span data-ttu-id="c1c71-168">I den **Reply URL** textruta, Skriv URL-Adressen används av Azure AD publicera svaret.</span><span class="sxs-lookup"><span data-stu-id="c1c71-168">In the **Reply URL** textbox, type the URL used by Azure AD to post the response.</span></span>
    
    | <span data-ttu-id="c1c71-169">Miljö</span><span class="sxs-lookup"><span data-stu-id="c1c71-169">Environment</span></span> | <span data-ttu-id="c1c71-170">URL: EN</span><span class="sxs-lookup"><span data-stu-id="c1c71-170">URL</span></span> |
    | :-- | :-- |
    | <span data-ttu-id="c1c71-171">För produktion</span><span class="sxs-lookup"><span data-stu-id="c1c71-171">For production</span></span> | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | <span data-ttu-id="c1c71-172">För testet</span><span class="sxs-lookup"><span data-stu-id="c1c71-172">For test</span></span> | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > <span data-ttu-id="c1c71-173">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="c1c71-173">These values are not real.</span></span> <span data-ttu-id="c1c71-174">Uppdatera dessa värden med faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="c1c71-174">Update these values with the actual Identifier, Reply URL and Sign-On URL.</span></span> <span data-ttu-id="c1c71-175">Kontakta [Ceridian Dayforce HCM klienten supportteamet](https://www.ceridian.com/contact-us/index.html) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="c1c71-175">Contact [Ceridian Dayforce HCM Client support team](https://www.ceridian.com/contact-us/index.html) to get these values.</span></span>

4. <span data-ttu-id="c1c71-176">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="c1c71-176">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. <span data-ttu-id="c1c71-178">Tillämpningsprogrammet Ceridian Dayforce HCM förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="c1c71-178">Your Ceridian Dayforce HCM application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="c1c71-179">Arbeta med [Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html) först för att identifiera rätt användar-ID.</span><span class="sxs-lookup"><span data-stu-id="c1c71-179">Work with [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) first to identify the correct user identifier.</span></span> <span data-ttu-id="c1c71-180">Microsoft rekommenderar att du använder den **”name”** attribut som användaridentifierare.</span><span class="sxs-lookup"><span data-stu-id="c1c71-180">Microsoft recommends using the **"name"** attribute as user identifier.</span></span> <span data-ttu-id="c1c71-181">Du kan hantera värden för attributen från den **användarattribut** avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="c1c71-181">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="c1c71-182">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="c1c71-182">The following screenshot shows an example for this.</span></span>  

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. <span data-ttu-id="c1c71-184">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden ovan och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1c71-184">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="c1c71-185">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="c1c71-185">Attribute Name</span></span>  | <span data-ttu-id="c1c71-186">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="c1c71-186">Attribute Value</span></span> |
    | --------------- | -------------------- |    
    | <span data-ttu-id="c1c71-187">namn</span><span class="sxs-lookup"><span data-stu-id="c1c71-187">name</span></span>  | <span data-ttu-id="c1c71-188">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="c1c71-188">user.extensionattribute2</span></span> |    

    <span data-ttu-id="c1c71-189">a.</span><span class="sxs-lookup"><span data-stu-id="c1c71-189">a.</span></span> <span data-ttu-id="c1c71-190">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-190">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    <span data-ttu-id="c1c71-193">b.</span><span class="sxs-lookup"><span data-stu-id="c1c71-193">b.</span></span> <span data-ttu-id="c1c71-194">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="c1c71-194">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="c1c71-195">c.</span><span class="sxs-lookup"><span data-stu-id="c1c71-195">c.</span></span> <span data-ttu-id="c1c71-196">I den **värdet** väljer användarattribut som du vill använda för din implementering.</span><span class="sxs-lookup"><span data-stu-id="c1c71-196">In the **Value** list, select the user attribute you want to use for your implementation.</span></span>
    <span data-ttu-id="c1c71-197">Till exempel om du vill använda EmployeeID som unikt identifierare och du har lagrat attributvärdet i ExtensionAttribute2, välj sedan **user.extensionattribute2**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-197">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select **user.extensionattribute2**.</span></span>
    
    <span data-ttu-id="c1c71-198">d.</span><span class="sxs-lookup"><span data-stu-id="c1c71-198">d.</span></span> <span data-ttu-id="c1c71-199">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-199">Click **Ok**.</span></span>

7. <span data-ttu-id="c1c71-200">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c71-200">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="c1c71-202">På den **Ceridian Dayforce HCM Configuration** klickar du på **konfigurera Ceridian Dayforce HCM** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="c1c71-202">On the **Ceridian Dayforce HCM Configuration** section, click **Configure Ceridian Dayforce HCM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c1c71-203">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="c1c71-203">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Ceridian Dayforce HCM-konfiguration](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. <span data-ttu-id="c1c71-205">Konfigurera enkel inloggning på **Ceridian Dayforce HCM** sida, måste du skicka den hämtade **XML-Metadata för** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** till [Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html).</span><span class="sxs-lookup"><span data-stu-id="c1c71-205">To configure single sign-on on **Ceridian Dayforce HCM** side, you need to send the downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html).</span></span>

> [!TIP]
> <span data-ttu-id="c1c71-206">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="c1c71-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c1c71-207">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="c1c71-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c1c71-208">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c1c71-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c1c71-209">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="c1c71-209">Create an Azure AD test user</span></span>

<span data-ttu-id="c1c71-210">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c1c71-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="c1c71-212">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="c1c71-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c71-213">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c71-213">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c1c71-215">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-215">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c1c71-217">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-217">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c1c71-219">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="c1c71-219">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c1c71-221">a.</span><span class="sxs-lookup"><span data-stu-id="c1c71-221">a.</span></span> <span data-ttu-id="c1c71-222">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-222">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c1c71-223">b.</span><span class="sxs-lookup"><span data-stu-id="c1c71-223">b.</span></span> <span data-ttu-id="c1c71-224">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="c1c71-224">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="c1c71-225">c.</span><span class="sxs-lookup"><span data-stu-id="c1c71-225">c.</span></span> <span data-ttu-id="c1c71-226">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-226">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="c1c71-227">d.</span><span class="sxs-lookup"><span data-stu-id="c1c71-227">d.</span></span> <span data-ttu-id="c1c71-228">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-228">Click **Create**.</span></span>
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a><span data-ttu-id="c1c71-229">Skapa en Ceridian Dayforce HCM testanvändare</span><span class="sxs-lookup"><span data-stu-id="c1c71-229">Create a Ceridian Dayforce HCM test user</span></span>

<span data-ttu-id="c1c71-230">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="c1c71-230">The objective of this section is to create a user called Britta Simon in Ceridian Dayforce HCM.</span></span> <span data-ttu-id="c1c71-231">Arbeta med den [Ceridian Dayforce HCM supportteamet](https://www.ceridian.com/contact-us/index.html) att hämta användare som lagts till i Ceridian Dayforce HCM-programmet.</span><span class="sxs-lookup"><span data-stu-id="c1c71-231">Work with the [Ceridian Dayforce HCM support team](https://www.ceridian.com/contact-us/index.html) to get users added in the Ceridian Dayforce HCM application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c1c71-232">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c1c71-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c1c71-233">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="c1c71-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ceridian Dayforce HCM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="c1c71-235">**Om du vill tilldela Ceridian Dayforce HCM Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c1c71-235">**To assign Britta Simon to Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c71-236">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c1c71-238">Välj i listan med program **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-238">In the applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. <span data-ttu-id="c1c71-240">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="c1c71-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c71-242">Click **Add** button.</span></span> <span data-ttu-id="c1c71-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="c1c71-245">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c1c71-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c1c71-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1c71-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-247">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c1c71-248">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="c1c71-248">Assign the Azure AD test user</span></span>

<span data-ttu-id="c1c71-249">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Ceridian Dayforce HCM.</span><span class="sxs-lookup"><span data-stu-id="c1c71-249">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Ceridian Dayforce HCM.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="c1c71-251">**Om du vill tilldela Ceridian Dayforce HCM Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="c1c71-251">**To assign Britta Simon to Ceridian Dayforce HCM, perform the following steps:**</span></span>

1. <span data-ttu-id="c1c71-252">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-252">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="c1c71-254">Välj i listan med program **Ceridian Dayforce HCM**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-254">In the applications list, select **Ceridian Dayforce HCM**.</span></span>

    ![Länken Ceridian Dayforce HCM i listan med program](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. <span data-ttu-id="c1c71-256">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="c1c71-256">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="c1c71-258">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="c1c71-258">Click **Add** button.</span></span> <span data-ttu-id="c1c71-259">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-259">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="c1c71-261">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="c1c71-261">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c1c71-262">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-262">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c1c71-263">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c1c71-263">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c1c71-264">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="c1c71-264">Test single sign-on</span></span>

<span data-ttu-id="c1c71-265">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="c1c71-265">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="c1c71-266">När du klickar på panelen Ceridian Dayforce HCM på åtkomstpanelen du bör få automatiskt loggat in på ditt Ceridian Dayforce HCM-program.</span><span class="sxs-lookup"><span data-stu-id="c1c71-266">When you click the Ceridian Dayforce HCM tile in the Access Panel, you should get automatically signed-on to your Ceridian Dayforce HCM application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c1c71-267">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="c1c71-267">Additional resources</span></span>

* [<span data-ttu-id="c1c71-268">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c1c71-268">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c1c71-269">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="c1c71-269">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png

