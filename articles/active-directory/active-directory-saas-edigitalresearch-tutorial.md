---
title: "Självstudier: Azure Active Directory-integrering med eDigitalResearch | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och eDigitalResearch."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c6b66ea0-16ba-45b4-b550-e81c56262b1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: f877a1dd844c40c913f3121e5288952653c312cd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-edigitalresearch"></a><span data-ttu-id="e15d1-103">Självstudier: Azure Active Directory-integrering med eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="e15d1-103">Tutorial: Azure Active Directory integration with eDigitalResearch</span></span>

<span data-ttu-id="e15d1-104">I kursen får lära du att integrera eDigitalResearch med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e15d1-104">In this tutorial, you learn how to integrate eDigitalResearch with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e15d1-105">Integrera eDigitalResearch med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e15d1-105">Integrating eDigitalResearch with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e15d1-106">Du kan styra i Azure AD som har åtkomst till eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="e15d1-106">You can control in Azure AD who has access to eDigitalResearch.</span></span>
- <span data-ttu-id="e15d1-107">Du kan aktivera användarna att automatiskt hämta loggat in på eDigitalResearch (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="e15d1-107">You can enable your users to automatically get signed-on to eDigitalResearch (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="e15d1-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e15d1-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="e15d1-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e15d1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e15d1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e15d1-110">Prerequisites</span></span>

<span data-ttu-id="e15d1-111">För att konfigurera Azure AD-integrering med eDigitalResearch, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e15d1-111">To configure Azure AD integration with eDigitalResearch, you need the following items:</span></span>

- <span data-ttu-id="e15d1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e15d1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e15d1-113">En eDigitalResearch enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e15d1-113">A eDigitalResearch single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e15d1-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e15d1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e15d1-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e15d1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e15d1-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e15d1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e15d1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e15d1-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e15d1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e15d1-118">Scenario description</span></span>
<span data-ttu-id="e15d1-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e15d1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e15d1-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e15d1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e15d1-121">Att lägga till eDigitalResearch från galleriet</span><span class="sxs-lookup"><span data-stu-id="e15d1-121">Adding eDigitalResearch from the gallery</span></span>
2. <span data-ttu-id="e15d1-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15d1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-edigitalresearch-from-the-gallery"></a><span data-ttu-id="e15d1-123">Att lägga till eDigitalResearch från galleriet</span><span class="sxs-lookup"><span data-stu-id="e15d1-123">Adding eDigitalResearch from the gallery</span></span>
<span data-ttu-id="e15d1-124">Du måste lägga till eDigitalResearch från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av eDigitalResearch i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e15d1-124">To configure the integration of eDigitalResearch into Azure AD, you need to add eDigitalResearch from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e15d1-125">**Utför följande steg för att lägga till eDigitalResearch från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e15d1-125">**To add eDigitalResearch from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e15d1-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e15d1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="e15d1-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e15d1-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="e15d1-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15d1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="e15d1-133">I sökrutan skriver **eDigitalResearch**väljer **eDigitalResearch** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e15d1-133">In the search box, type **eDigitalResearch**, select **eDigitalResearch** from result panel then click **Add** button to add the application.</span></span>

    ![eDigitalResearch i resultatlistan](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e15d1-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15d1-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="e15d1-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med eDigitalResearch baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e15d1-136">In this section, you configure and test Azure AD single sign-on with eDigitalResearch based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e15d1-137">Azure AD måste du känna till användaren i eDigitalResearch motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e15d1-137">For single sign-on to work, Azure AD needs to know what the counterpart user in eDigitalResearch is to a user in Azure AD.</span></span> <span data-ttu-id="e15d1-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i eDigitalResearch upprättas.</span><span class="sxs-lookup"><span data-stu-id="e15d1-138">In other words, a link relationship between an Azure AD user and the related user in eDigitalResearch needs to be established.</span></span>

<span data-ttu-id="e15d1-139">I eDigitalResearch, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e15d1-139">In eDigitalResearch, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e15d1-140">Om du vill konfigurera och testa Azure AD enkel inloggning med eDigitalResearch, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e15d1-140">To configure and test Azure AD single sign-on with eDigitalResearch, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e15d1-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e15d1-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e15d1-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e15d1-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e15d1-143">**[Skapa en testanvändare eDigitalResearch](#create-a-edigitalresearch-test-user)**  – du har en motsvarighet för Britta Simon i eDigitalResearch som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e15d1-143">**[Create a eDigitalResearch test user](#create-a-edigitalresearch-test-user)** - to have a counterpart of Britta Simon in eDigitalResearch that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e15d1-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e15d1-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e15d1-145">**[Testa enkel inloggning](#test-single-sign-on)**  att kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e15d1-145">**[Test single sign-on](#test-single-sign-on)**  to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e15d1-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15d1-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="e15d1-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt eDigitalResearch program.</span><span class="sxs-lookup"><span data-stu-id="e15d1-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your eDigitalResearch application.</span></span>

<span data-ttu-id="e15d1-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med eDigitalResearch:**</span><span class="sxs-lookup"><span data-stu-id="e15d1-148">**To configure Azure AD single sign-on with eDigitalResearch, perform the following steps:**</span></span>

1. <span data-ttu-id="e15d1-149">I Azure-portalen på den **eDigitalResearch** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-149">In the Azure portal, on the **eDigitalResearch** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="e15d1-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e15d1-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_samlbase.png)

3. <span data-ttu-id="e15d1-153">På den **eDigitalResearch domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e15d1-153">On the **eDigitalResearch Domain and URLs** section, perform the following steps:</span></span>

    ![eDigitalResearch domän URL: er och enkel inloggning information](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_url.png)

    <span data-ttu-id="e15d1-155">a.</span><span class="sxs-lookup"><span data-stu-id="e15d1-155">a.</span></span> <span data-ttu-id="e15d1-156">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<company-name>.edigitalresearch.com`</span><span class="sxs-lookup"><span data-stu-id="e15d1-156">In the **Identifier** textbox, type a URL using the following pattern: `https://<company-name>.edigitalresearch.com`</span></span>

    <span data-ttu-id="e15d1-157">b.</span><span class="sxs-lookup"><span data-stu-id="e15d1-157">b.</span></span> <span data-ttu-id="e15d1-158">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<company-name>.edigitalresearch.com/login/consume`</span><span class="sxs-lookup"><span data-stu-id="e15d1-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company-name>.edigitalresearch.com/login/consume`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e15d1-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e15d1-159">These values are not real.</span></span> <span data-ttu-id="e15d1-160">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="e15d1-160">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="e15d1-161">Kontakta [eDigitalResearch supportteamet](http://www.maruedr.com/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="e15d1-161">Contact [eDigitalResearch support team](http://www.maruedr.com/contact) to get these values.</span></span>
 


4. <span data-ttu-id="e15d1-162">På den **SAML-signeringscertifikat** klickar du på **certifikat Base(64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e15d1-162">On the **SAML Signing Certificate** section, click **Certificate Base(64)** and then save the certificate file on your computer.</span></span>

    <span data-ttu-id="e15d1-163">!</span><span class="sxs-lookup"><span data-stu-id="e15d1-163">!</span></span>![Länken hämta certifikatet](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_certificate.png) 

5. <span data-ttu-id="e15d1-165">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e15d1-165">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e15d1-167">På den **eDigitalResearch Configuration** klickar du på **konfigurera eDigitalResearch** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e15d1-167">On the **eDigitalResearch Configuration** section, click **Configure eDigitalResearch** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e15d1-168">Kopiera den **Sign-Out URL, SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e15d1-168">Copy the **Sign-Out URL, SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![eDigitalResearch konfiguration](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_configure.png) 

7. <span data-ttu-id="e15d1-170">Konfigurera enkel inloggning på **eDigitalResearch** sida, måste du skicka den hämtade **certifikatfil (Base64)**, **SAML enhets-ID**, och **utloggning URL: en** till [eDigitalResearch supportteamet](http://www.maruedr.com/contact).</span><span class="sxs-lookup"><span data-stu-id="e15d1-170">To configure single sign-on on **eDigitalResearch** side, you need to send the downloaded **Certificate (Base64) File**, **SAML Entity ID**, and **Sign-Out URL** to [eDigitalResearch support team](http://www.maruedr.com/contact).</span></span> <span data-ttu-id="e15d1-171">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="e15d1-171">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="e15d1-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e15d1-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e15d1-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e15d1-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e15d1-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e15d1-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e15d1-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e15d1-175">Create an Azure AD test user</span></span>

<span data-ttu-id="e15d1-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e15d1-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="e15d1-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e15d1-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e15d1-179">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="e15d1-179">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="e15d1-181">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-181">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="e15d1-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15d1-183">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="e15d1-185">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e15d1-185">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_04.png)

    <span data-ttu-id="e15d1-187">a.</span><span class="sxs-lookup"><span data-stu-id="e15d1-187">a.</span></span> <span data-ttu-id="e15d1-188">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-188">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e15d1-189">b.</span><span class="sxs-lookup"><span data-stu-id="e15d1-189">b.</span></span> <span data-ttu-id="e15d1-190">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="e15d1-190">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="e15d1-191">c.</span><span class="sxs-lookup"><span data-stu-id="e15d1-191">c.</span></span> <span data-ttu-id="e15d1-192">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="e15d1-192">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="e15d1-193">d.</span><span class="sxs-lookup"><span data-stu-id="e15d1-193">d.</span></span> <span data-ttu-id="e15d1-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-194">Click **Create**.</span></span>
  
### <a name="create-a-edigitalresearch-test-user"></a><span data-ttu-id="e15d1-195">Skapa en testanvändare eDigitalResearch</span><span class="sxs-lookup"><span data-stu-id="e15d1-195">Create a eDigitalResearch test user</span></span>

<span data-ttu-id="e15d1-196">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="e15d1-196">The objective of this section is to create a user called Britta Simon in eDigitalResearch.</span></span> 

<span data-ttu-id="e15d1-197">Arbeta med den [eDigitalResearch supportteamet](http://www.maruedr.com/contact) att hämta användare som skapas.</span><span class="sxs-lookup"><span data-stu-id="e15d1-197">Work with the [eDigitalResearch support team](http://www.maruedr.com/contact) to get users created.</span></span>     
    
 > [!NOTE]
 > <span data-ttu-id="e15d1-198">Azure Active Directory kontoinnehavaren får ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="e15d1-198">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="e15d1-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e15d1-199">Assign the Azure AD test user</span></span>

<span data-ttu-id="e15d1-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till eDigitalResearch.</span><span class="sxs-lookup"><span data-stu-id="e15d1-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to eDigitalResearch.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="e15d1-202">**Om du vill tilldela eDigitalResearch Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e15d1-202">**To assign Britta Simon to eDigitalResearch, perform the following steps:**</span></span>

1. <span data-ttu-id="e15d1-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e15d1-205">Välj i listan med program **eDigitalResearch**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-205">In the applications list, select **eDigitalResearch**.</span></span>

    ![Länken eDigitalResearch i listan med program](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_app.png)  

3. <span data-ttu-id="e15d1-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e15d1-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="e15d1-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e15d1-209">Click **Add** button.</span></span> <span data-ttu-id="e15d1-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15d1-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="e15d1-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e15d1-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e15d1-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15d1-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e15d1-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e15d1-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="e15d1-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e15d1-215">Test single sign-on</span></span>

<span data-ttu-id="e15d1-216">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e15d1-216">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e15d1-217">När du klickar på panelen eDigitalResearch på åtkomstpanelen du bör få automatiskt loggat in på ditt eDigitalResearch program.</span><span class="sxs-lookup"><span data-stu-id="e15d1-217">When you click the eDigitalResearch tile in the Access Panel, you should get automatically signed-on to your eDigitalResearch application.</span></span>
<span data-ttu-id="e15d1-218">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e15d1-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="e15d1-219">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e15d1-219">Additional resources</span></span>

* [<span data-ttu-id="e15d1-220">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e15d1-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e15d1-221">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e15d1-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_203.png

