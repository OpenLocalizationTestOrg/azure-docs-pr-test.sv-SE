---
title: "Självstudier: Azure Active Directory-integrering med Icertis kontraktet plattform | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Icertis plattform för kontraktet."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6627e6dd-f559-4cd4-a509-f6d9a4961b49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 9dd002f71b7a960338071db869f7c8cf88071342
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a><span data-ttu-id="4f29c-103">Självstudier: Azure Active Directory-integrering med Icertis kontraktet plattform</span><span class="sxs-lookup"><span data-stu-id="4f29c-103">Tutorial: Azure Active Directory integration with Icertis Contract Management Platform</span></span>

<span data-ttu-id="4f29c-104">I kursen får lära du att integrera Icertis plattform för hantering av kontrakt med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4f29c-104">In this tutorial, you learn how to integrate Icertis Contract Management Platform with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4f29c-105">Integrera Icertis plattform för hantering av kontrakt med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4f29c-105">Integrating Icertis Contract Management Platform with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4f29c-106">Du kan styra i Azure AD som har åtkomst till Icertis kontraktet plattform</span><span class="sxs-lookup"><span data-stu-id="4f29c-106">You can control in Azure AD who has access to Icertis Contract Management Platform</span></span>
- <span data-ttu-id="4f29c-107">Du kan aktivera användarna att automatiskt hämta loggat in på Icertis plattform för kontrakt (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4f29c-107">You can enable your users to automatically get signed-on to Icertis Contract Management Platform (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4f29c-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4f29c-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4f29c-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4f29c-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f29c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4f29c-110">Prerequisites</span></span>

<span data-ttu-id="4f29c-111">För att konfigurera Azure AD-integrering med Icertis plattform för hantering av kontrakt, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4f29c-111">To configure Azure AD integration with Icertis Contract Management Platform, you need the following items:</span></span>

- <span data-ttu-id="4f29c-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4f29c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4f29c-113">En plattform för hantering av Icertis kontraktet enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4f29c-113">An Icertis Contract Management Platform single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4f29c-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4f29c-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4f29c-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4f29c-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4f29c-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4f29c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4f29c-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4f29c-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4f29c-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4f29c-118">Scenario description</span></span>
<span data-ttu-id="4f29c-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4f29c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4f29c-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4f29c-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4f29c-121">Att lägga till Icertis plattform för hantering av kontrakt från galleriet</span><span class="sxs-lookup"><span data-stu-id="4f29c-121">Adding Icertis Contract Management Platform from the gallery</span></span>
2. <span data-ttu-id="4f29c-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f29c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icertis-contract-management-platform-from-the-gallery"></a><span data-ttu-id="4f29c-123">Att lägga till Icertis plattform för hantering av kontrakt från galleriet</span><span class="sxs-lookup"><span data-stu-id="4f29c-123">Adding Icertis Contract Management Platform from the gallery</span></span>
<span data-ttu-id="4f29c-124">Du måste lägga till Icertis plattform för hantering av kontrakt från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Icertis plattform för hantering av kontrakt i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4f29c-124">To configure the integration of Icertis Contract Management Platform into Azure AD, you need to add Icertis Contract Management Platform from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4f29c-125">**Utför följande steg för att lägga till Icertis plattform för hantering av kontrakt från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="4f29c-125">**To add Icertis Contract Management Platform from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4f29c-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4f29c-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4f29c-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4f29c-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4f29c-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f29c-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4f29c-133">I sökrutan skriver **Icertis plattform för hantering av kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-133">In the search box, type **Icertis Contract Management Platform**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_search.png)

5. <span data-ttu-id="4f29c-135">Välj i resultatpanelen **Icertis plattform för hantering av kontrakt**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="4f29c-135">In the results panel, select **Icertis Contract Management Platform**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4f29c-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f29c-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4f29c-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med Icertis kontraktet plattform för hantering av baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4f29c-138">In this section, you configure and test Azure AD single sign-on with Icertis Contract Management Platform based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4f29c-139">Azure AD måste du känna till motsvarande användaren i Icertis plattform för hantering av kontrakt till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="4f29c-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Icertis Contract Management Platform is to a user in Azure AD.</span></span> <span data-ttu-id="4f29c-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Icertis plattform för hantering av kontrakt upprättas.</span><span class="sxs-lookup"><span data-stu-id="4f29c-140">In other words, a link relationship between an Azure AD user and the related user in Icertis Contract Management Platform needs to be established.</span></span>

<span data-ttu-id="4f29c-141">I Icertis plattform för kontrakt, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4f29c-141">In Icertis Contract Management Platform, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4f29c-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Icertis plattform för hantering av kontrakt, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4f29c-142">To configure and test Azure AD single sign-on with Icertis Contract Management Platform, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4f29c-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4f29c-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4f29c-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f29c-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4f29c-145">**[Skapa en testanvändare Icertis plattform för hantering av kontrakt](#creating-an-icertis-contract-management-platform-test-user)**  – har en motsvarighet för Britta Simon Icertis kontraktet plattform som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4f29c-145">**[Creating an Icertis Contract Management Platform test user](#creating-an-icertis-contract-management-platform-test-user)** - to have a counterpart of Britta Simon in Icertis Contract Management Platform that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4f29c-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4f29c-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4f29c-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4f29c-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4f29c-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f29c-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4f29c-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Icertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="4f29c-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Icertis Contract Management Platform application.</span></span>

<span data-ttu-id="4f29c-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Icertis plattform för hantering av kontraktet:**</span><span class="sxs-lookup"><span data-stu-id="4f29c-150">**To configure Azure AD single sign-on with Icertis Contract Management Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="4f29c-151">I Azure-portalen på den **Icertis plattform för hantering av kontrakt** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-151">In the Azure portal, on the **Icertis Contract Management Platform** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4f29c-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4f29c-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_samlbase.png)

3. <span data-ttu-id="4f29c-155">På den **URL: er och domänen för plattform för hantering av kontrakt Icertis** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4f29c-155">On the **Icertis Contract Management Platform Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_url.png)

    <span data-ttu-id="4f29c-157">a.</span><span class="sxs-lookup"><span data-stu-id="4f29c-157">a.</span></span> <span data-ttu-id="4f29c-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="4f29c-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.icertis.com`</span></span>

    <span data-ttu-id="4f29c-159">b.</span><span class="sxs-lookup"><span data-stu-id="4f29c-159">b.</span></span> <span data-ttu-id="4f29c-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="4f29c-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.icertis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4f29c-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4f29c-161">These values are not real.</span></span> <span data-ttu-id="4f29c-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="4f29c-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="4f29c-163">Kontakta [Icertis kontraktet Hanteringsklient plattform supportteamet](https://www.icertis.com/company/contact/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="4f29c-163">Contact [Icertis Contract Management Platform Client support team](https://www.icertis.com/company/contact/) to get these values.</span></span> 

4. <span data-ttu-id="4f29c-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4f29c-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_certificate.png) 

5. <span data-ttu-id="4f29c-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4f29c-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4f29c-168">På den **Icertis kontraktet Management plattformskonfigurationen** klickar du på **konfigurera Icertis kontraktet plattform för hantering av** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4f29c-168">On the **Icertis Contract Management Platform Configuration** section, click **Configure Icertis Contract Management Platform** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4f29c-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4f29c-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_configure.png) 

7. <span data-ttu-id="4f29c-171">Konfigurera enkel inloggning på **Icertis plattform för hantering av kontrakt** sida, måste du skicka den hämtade **XML-Metadata för** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel** till [Icertis plattform för hantering av kontrakt supportteamet](https://www.icertis.com/company/contact/).</span><span class="sxs-lookup"><span data-stu-id="4f29c-171">To configure single sign-on on **Icertis Contract Management Platform** side, you need to send the downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="4f29c-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="4f29c-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4f29c-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="4f29c-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4f29c-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4f29c-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4f29c-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4f29c-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="4f29c-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4f29c-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4f29c-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="4f29c-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4f29c-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4f29c-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4f29c-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4f29c-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f29c-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4f29c-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4f29c-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4f29c-187">a.</span><span class="sxs-lookup"><span data-stu-id="4f29c-187">a.</span></span> <span data-ttu-id="4f29c-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4f29c-189">b.</span><span class="sxs-lookup"><span data-stu-id="4f29c-189">b.</span></span> <span data-ttu-id="4f29c-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4f29c-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4f29c-191">c.</span><span class="sxs-lookup"><span data-stu-id="4f29c-191">c.</span></span> <span data-ttu-id="4f29c-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4f29c-193">d.</span><span class="sxs-lookup"><span data-stu-id="4f29c-193">d.</span></span> <span data-ttu-id="4f29c-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-194">Click **Create**.</span></span>
 
### <a name="creating-an-icertis-contract-management-platform-test-user"></a><span data-ttu-id="4f29c-195">Skapa en testanvändare Icertis plattform för hantering av kontrakt</span><span class="sxs-lookup"><span data-stu-id="4f29c-195">Creating an Icertis Contract Management Platform test user</span></span>

<span data-ttu-id="4f29c-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Icertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="4f29c-196">In this section, you create a user called Britta Simon in Icertis Contract Management Platform.</span></span> <span data-ttu-id="4f29c-197">Se tillsammans med [Icertis plattform för hantering av kontrakt supportteamet](https://www.icertis.com/company/contact/) att lägga till användare i Hanteringsplattformen Icertis kontraktet.</span><span class="sxs-lookup"><span data-stu-id="4f29c-197">Please work with [Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/) to add the users in the Icertis Contract Management Platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4f29c-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4f29c-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4f29c-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Icertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="4f29c-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Icertis Contract Management Platform.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4f29c-201">**Om du vill tilldela Britta Simon Icertis plattform för hantering av kontrakt, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4f29c-201">**To assign Britta Simon to Icertis Contract Management Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="4f29c-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4f29c-204">Välj i listan med program **Icertis plattform för hantering av kontrakt**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-204">In the applications list, select **Icertis Contract Management Platform**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_app.png) 

3. <span data-ttu-id="4f29c-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4f29c-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4f29c-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4f29c-208">Click **Add** button.</span></span> <span data-ttu-id="4f29c-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f29c-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4f29c-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="4f29c-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4f29c-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f29c-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4f29c-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4f29c-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4f29c-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4f29c-214">Testing single sign-on</span></span>

<span data-ttu-id="4f29c-215">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="4f29c-215">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="4f29c-216">När du klickar på panelen Icertis kontraktet plattform på åtkomstpanelen du ska hämta automatiskt inloggade i tillämpningsprogrammet Icertis plattform för kontraktet.</span><span class="sxs-lookup"><span data-stu-id="4f29c-216">When you click the Icertis Contract Management Platform tile in the Access Panel, you should get automatically signed-on to your Icertis Contract Management Platform application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f29c-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4f29c-217">Additional resources</span></span>

* [<span data-ttu-id="4f29c-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4f29c-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4f29c-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4f29c-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_203.png

