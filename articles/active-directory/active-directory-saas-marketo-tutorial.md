---
title: "Självstudier: Azure Active Directory-integrering med Marketo | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Marketo."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: e146fd5a8075bc9c7ba049b25e5f301fc2645ed9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a><span data-ttu-id="d1104-103">Självstudier: Azure Active Directory-integrering med Marketo</span><span class="sxs-lookup"><span data-stu-id="d1104-103">Tutorial: Azure Active Directory integration with Marketo</span></span>

<span data-ttu-id="d1104-104">I kursen får lära du att integrera Marketo med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d1104-104">In this tutorial, you learn how to integrate Marketo with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d1104-105">Integrera Marketo med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d1104-105">Integrating Marketo with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d1104-106">Du kan styra i Azure AD som har åtkomst till Marketo</span><span class="sxs-lookup"><span data-stu-id="d1104-106">You can control in Azure AD who has access to Marketo</span></span>
- <span data-ttu-id="d1104-107">Du kan aktivera användarna att automatiskt hämta loggat in på Marketo (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d1104-107">You can enable your users to automatically get signed-on to Marketo (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d1104-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d1104-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d1104-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d1104-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1104-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d1104-110">Prerequisites</span></span>

<span data-ttu-id="d1104-111">För att konfigurera Azure AD-integrering med Marketo, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d1104-111">To configure Azure AD integration with Marketo, you need the following items:</span></span>

- <span data-ttu-id="d1104-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d1104-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d1104-113">En Marketo enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d1104-113">A Marketo single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d1104-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d1104-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d1104-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d1104-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d1104-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d1104-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d1104-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d1104-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d1104-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d1104-118">Scenario description</span></span>
<span data-ttu-id="d1104-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d1104-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d1104-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d1104-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d1104-121">Att lägga till Marketo från galleriet</span><span class="sxs-lookup"><span data-stu-id="d1104-121">Adding Marketo from the gallery</span></span>
2. <span data-ttu-id="d1104-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1104-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-marketo-from-the-gallery"></a><span data-ttu-id="d1104-123">Att lägga till Marketo från galleriet</span><span class="sxs-lookup"><span data-stu-id="d1104-123">Adding Marketo from the gallery</span></span>
<span data-ttu-id="d1104-124">Du måste lägga till Marketo från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Marketo i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d1104-124">To configure the integration of Marketo into Azure AD, you need to add Marketo from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d1104-125">**Utför följande steg för att lägga till Marketo från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d1104-125">**To add Marketo from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d1104-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d1104-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d1104-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d1104-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d1104-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d1104-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d1104-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1104-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d1104-133">I sökrutan skriver **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="d1104-133">In the search box, type **Marketo**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_search.png)

5. <span data-ttu-id="d1104-135">Välj i resultatpanelen **Marketo**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d1104-135">In the results panel, select **Marketo**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d1104-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1104-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d1104-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Marketo baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d1104-138">In this section, you configure and test Azure AD single sign-on with Marketo based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d1104-139">Azure AD måste du känna till användaren i Marketo motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d1104-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Marketo is to a user in Azure AD.</span></span> <span data-ttu-id="d1104-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Marketo upprättas.</span><span class="sxs-lookup"><span data-stu-id="d1104-140">In other words, a link relationship between an Azure AD user and the related user in Marketo needs to be established.</span></span>

<span data-ttu-id="d1104-141">I Marketo, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d1104-141">In Marketo, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d1104-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Marketo, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d1104-142">To configure and test Azure AD single sign-on with Marketo, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d1104-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d1104-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d1104-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1104-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d1104-145">**[Skapa en testanvändare Marketo](#creating-a-marketo-test-user)**  – du har en motsvarighet för Britta Simon i Marketo som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d1104-145">**[Creating a Marketo test user](#creating-a-marketo-test-user)** - to have a counterpart of Britta Simon in Marketo that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d1104-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d1104-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d1104-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d1104-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d1104-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1104-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d1104-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Marketo-program.</span><span class="sxs-lookup"><span data-stu-id="d1104-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Marketo application.</span></span>

<span data-ttu-id="d1104-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Marketo:**</span><span class="sxs-lookup"><span data-stu-id="d1104-150">**To configure Azure AD single sign-on with Marketo, perform the following steps:**</span></span>

1. <span data-ttu-id="d1104-151">I Azure-portalen på den **Marketo** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d1104-151">In the Azure portal, on the **Marketo** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d1104-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d1104-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_samlbase.png)

3. <span data-ttu-id="d1104-155">På den **Marketo domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1104-155">On the **Marketo Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_url.png)

    <span data-ttu-id="d1104-157">a.</span><span class="sxs-lookup"><span data-stu-id="d1104-157">a.</span></span> <span data-ttu-id="d1104-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://saml.marketo.com/sp`</span><span class="sxs-lookup"><span data-stu-id="d1104-158">In the **Identifier** textbox, type a URL using the following pattern: `https://saml.marketo.com/sp`</span></span>

    <span data-ttu-id="d1104-159">b.</span><span class="sxs-lookup"><span data-stu-id="d1104-159">b.</span></span> <span data-ttu-id="d1104-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://login.marketo.com/saml/assertion/\<munchkinid\>`</span><span class="sxs-lookup"><span data-stu-id="d1104-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://login.marketo.com/saml/assertion/\<munchkinid\>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d1104-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d1104-161">These values are not real.</span></span> <span data-ttu-id="d1104-162">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="d1104-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="d1104-163">Kontakta [Marketo supportteamet](http://investors.marketo.com/contactus.cfm) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d1104-163">Contact [Marketo support team](http://investors.marketo.com/contactus.cfm) to get these values.</span></span>
 
4. <span data-ttu-id="d1104-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d1104-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_certificate.png) 

5. <span data-ttu-id="d1104-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1104-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d1104-168">På den **Marketo Configuration** klickar du på **konfigurera Marketo** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d1104-168">On the **Marketo Configuration** section, click **Configure Marketo** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d1104-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d1104-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_configure.png) 

7. <span data-ttu-id="d1104-171">Logga in på Marketo med administratörsautentiseringsuppgifter för att få Munchkin Id för ditt program, och utför följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="d1104-171">To get Munchkin Id of your application, log in to Marketo using admin credentials and perform following actions:</span></span>
   
    <span data-ttu-id="d1104-172">a.</span><span class="sxs-lookup"><span data-stu-id="d1104-172">a.</span></span> <span data-ttu-id="d1104-173">Logga in på Marketo-app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d1104-173">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="d1104-174">b.</span><span class="sxs-lookup"><span data-stu-id="d1104-174">b.</span></span> <span data-ttu-id="d1104-175">Klicka på den **Admin** knappen i det övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="d1104-175">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="d1104-177">c.</span><span class="sxs-lookup"><span data-stu-id="d1104-177">c.</span></span> <span data-ttu-id="d1104-178">Gå till menyn integrering och på den **Munchkin länk**.</span><span class="sxs-lookup"><span data-stu-id="d1104-178">Navigate to the Integration menu and click the **Munchkin link**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)
   
    <span data-ttu-id="d1104-180">d.</span><span class="sxs-lookup"><span data-stu-id="d1104-180">d.</span></span> <span data-ttu-id="d1104-181">Kopiera Munchkin-Id som visas på skärmen och slutför Reply-URL i guiden för konfiguration av Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d1104-181">Copy the Munchkin Id shown on the screen and complete your Reply URL in the Azure AD configuration wizard.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png) 

8. <span data-ttu-id="d1104-183">Så här konfigurerar du SSO i programmet i nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="d1104-183">To configure the SSO in the application, follow the below steps:</span></span>
   
    <span data-ttu-id="d1104-184">a.</span><span class="sxs-lookup"><span data-stu-id="d1104-184">a.</span></span> <span data-ttu-id="d1104-185">Logga in på Marketo-app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d1104-185">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="d1104-186">b.</span><span class="sxs-lookup"><span data-stu-id="d1104-186">b.</span></span> <span data-ttu-id="d1104-187">Klicka på den **Admin** knappen i det övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="d1104-187">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="d1104-189">c.</span><span class="sxs-lookup"><span data-stu-id="d1104-189">c.</span></span> <span data-ttu-id="d1104-190">Gå till menyn integrering och på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d1104-190">Navigate to the Integration menu and click **Single Sign On**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png) 
   
    <span data-ttu-id="d1104-192">d.</span><span class="sxs-lookup"><span data-stu-id="d1104-192">d.</span></span> <span data-ttu-id="d1104-193">Om du vill aktivera SAML-inställningar, klickar du på **redigera** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1104-193">To enable the SAML Settings, click **Edit** button.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png) 
   
    <span data-ttu-id="d1104-195">e.</span><span class="sxs-lookup"><span data-stu-id="d1104-195">e.</span></span> <span data-ttu-id="d1104-196">**Aktiverad** inställningar för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d1104-196">**Enabled** Single Sign-On settings.</span></span>
   
    <span data-ttu-id="d1104-197">f.</span><span class="sxs-lookup"><span data-stu-id="d1104-197">f.</span></span> <span data-ttu-id="d1104-198">Klistra in den **SAML enhets-ID**i den **utfärdaren ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="d1104-198">Paste the **SAML Entity ID**, in the **Issuer ID** textbox.</span></span>
   
    <span data-ttu-id="d1104-199">g.</span><span class="sxs-lookup"><span data-stu-id="d1104-199">g.</span></span> <span data-ttu-id="d1104-200">I den **enhets-ID** textruta ange URL: en som `http://saml.marketo.com/sp`.</span><span class="sxs-lookup"><span data-stu-id="d1104-200">In the **Entity ID** textbox, enter the URL as `http://saml.marketo.com/sp`.</span></span>
   
    <span data-ttu-id="d1104-201">h.</span><span class="sxs-lookup"><span data-stu-id="d1104-201">h.</span></span> <span data-ttu-id="d1104-202">Välj den plats som ID **namnidentifierare elementet**.</span><span class="sxs-lookup"><span data-stu-id="d1104-202">Select the User ID Location as **Name Identifier element**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > <span data-ttu-id="d1104-204">Om ditt användar-ID inte UPN-värde kan ändra värdet i fliken attribut.</span><span class="sxs-lookup"><span data-stu-id="d1104-204">If your User Identifier is not UPN value then change the value in the Attribute tab.</span></span>
   
    <span data-ttu-id="d1104-205">Jag.</span><span class="sxs-lookup"><span data-stu-id="d1104-205">i.</span></span> <span data-ttu-id="d1104-206">Ladda upp certifikatet som du har hämtat från Azure AD-konfigurationsguiden.</span><span class="sxs-lookup"><span data-stu-id="d1104-206">Upload the certificate, which you have downloaded from Azure AD configuration wizard.</span></span> <span data-ttu-id="d1104-207">**Spara** inställningarna.</span><span class="sxs-lookup"><span data-stu-id="d1104-207">**Save** the settings.</span></span>
   
    <span data-ttu-id="d1104-208">j.</span><span class="sxs-lookup"><span data-stu-id="d1104-208">j.</span></span> <span data-ttu-id="d1104-209">Redigera inställningar för omdirigera felsidor.</span><span class="sxs-lookup"><span data-stu-id="d1104-209">Edit the Redirect Pages settings.</span></span>
   
    <span data-ttu-id="d1104-210">k.</span><span class="sxs-lookup"><span data-stu-id="d1104-210">k.</span></span> <span data-ttu-id="d1104-211">Klistra in den **SAML enkel inloggning Tjänstwebbadress** i den **inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="d1104-211">Paste the **SAML Single Sign-On Service URL** in the **Login URL** textbox.</span></span>
   
    <span data-ttu-id="d1104-212">l.</span><span class="sxs-lookup"><span data-stu-id="d1104-212">l.</span></span> <span data-ttu-id="d1104-213">Klistra in den **Sign-Out URL** i den **logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="d1104-213">Paste the **Sign-Out URL** in the **Logout URL** textbox.</span></span>
   
    <span data-ttu-id="d1104-214">m.</span><span class="sxs-lookup"><span data-stu-id="d1104-214">m.</span></span> <span data-ttu-id="d1104-215">I den **fel URL**, kopiera ditt **Marketo instans-URL** och på **spara** för att spara inställningar.</span><span class="sxs-lookup"><span data-stu-id="d1104-215">In the **Error URL**, copy your **Marketo instance URL** and click **Save** button to save settings.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

9. <span data-ttu-id="d1104-217">Om du vill aktivera enkel inloggning för användare att utföra följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="d1104-217">To enable the SSO for users, complete the following actions:</span></span>
   
    <span data-ttu-id="d1104-218">a.</span><span class="sxs-lookup"><span data-stu-id="d1104-218">a.</span></span> <span data-ttu-id="d1104-219">Logga in på Marketo-app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d1104-219">Log in to Marketo app using admin credentials.</span></span>
   
    <span data-ttu-id="d1104-220">b.</span><span class="sxs-lookup"><span data-stu-id="d1104-220">b.</span></span> <span data-ttu-id="d1104-221">Klicka på den **Admin** knappen i det övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="d1104-221">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 
   
    <span data-ttu-id="d1104-223">c.</span><span class="sxs-lookup"><span data-stu-id="d1104-223">c.</span></span> <span data-ttu-id="d1104-224">Navigera till den **säkerhet** -menyn och klicka på **inloggningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d1104-224">Navigate to the **Security** menu and click **Login Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)
   
    <span data-ttu-id="d1104-226">d.</span><span class="sxs-lookup"><span data-stu-id="d1104-226">d.</span></span> <span data-ttu-id="d1104-227">Kontrollera den **kräver SSO** alternativet och **spara** inställningarna.</span><span class="sxs-lookup"><span data-stu-id="d1104-227">Check the **Require SSO** option and **Save** the settings.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

> [!TIP]
> <span data-ttu-id="d1104-229">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d1104-229">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d1104-230">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d1104-230">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d1104-231">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d1104-231">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d1104-232">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d1104-232">Creating an Azure AD test user</span></span>
<span data-ttu-id="d1104-233">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d1104-233">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d1104-235">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d1104-235">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d1104-236">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d1104-236">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d1104-238">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d1104-238">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d1104-240">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1104-240">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d1104-242">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d1104-242">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d1104-244">a.</span><span class="sxs-lookup"><span data-stu-id="d1104-244">a.</span></span> <span data-ttu-id="d1104-245">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d1104-245">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d1104-246">b.</span><span class="sxs-lookup"><span data-stu-id="d1104-246">b.</span></span> <span data-ttu-id="d1104-247">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d1104-247">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d1104-248">c.</span><span class="sxs-lookup"><span data-stu-id="d1104-248">c.</span></span> <span data-ttu-id="d1104-249">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d1104-249">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d1104-250">d.</span><span class="sxs-lookup"><span data-stu-id="d1104-250">d.</span></span> <span data-ttu-id="d1104-251">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d1104-251">Click **Create**.</span></span>
 
### <a name="creating-a-marketo-test-user"></a><span data-ttu-id="d1104-252">Skapa en testanvändare Marketo</span><span class="sxs-lookup"><span data-stu-id="d1104-252">Creating a Marketo test user</span></span>

<span data-ttu-id="d1104-253">I det här avsnittet skapar du en användare som kallas Britta Simon i Marketo.</span><span class="sxs-lookup"><span data-stu-id="d1104-253">In this section, you create a user called Britta Simon in Marketo.</span></span> <span data-ttu-id="d1104-254">Följ dessa steg om du vill skapa en användare i Marketo-plattformen.</span><span class="sxs-lookup"><span data-stu-id="d1104-254">follow these steps to create a user in Marketo platform.</span></span>

1. <span data-ttu-id="d1104-255">Logga in på Marketo-app med administratörsautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d1104-255">Log in to Marketo app using admin credentials.</span></span>

2. <span data-ttu-id="d1104-256">Klicka på den **Admin** knappen i det övre navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="d1104-256">Click the **Admin** button on the top navigation pane.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png) 

3. <span data-ttu-id="d1104-258">Navigera till den **säkerhet** -menyn och klicka på **användare och roller**</span><span class="sxs-lookup"><span data-stu-id="d1104-258">Navigate to the **Security** menu and click **Users & Roles**</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)  

4. <span data-ttu-id="d1104-260">Klicka på den **bjuda in nya användare** länk på fliken användare</span><span class="sxs-lookup"><span data-stu-id="d1104-260">Click the **Invite New User** link on the Users tab</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png) 

5. <span data-ttu-id="d1104-262">Fyll i följande information i guiden bjuda in nya användare</span><span class="sxs-lookup"><span data-stu-id="d1104-262">In the Invite New User wizard fill the following information</span></span>
   
    <span data-ttu-id="d1104-263">a.</span><span class="sxs-lookup"><span data-stu-id="d1104-263">a.</span></span> <span data-ttu-id="d1104-264">Ange användaren **e-post** adressen i textrutan</span><span class="sxs-lookup"><span data-stu-id="d1104-264">Enter the user **Email** address in the textbox</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)
   
    <span data-ttu-id="d1104-266">b.</span><span class="sxs-lookup"><span data-stu-id="d1104-266">b.</span></span> <span data-ttu-id="d1104-267">Ange den **Förnamn** i textrutan</span><span class="sxs-lookup"><span data-stu-id="d1104-267">Enter the **First Name** in the textbox</span></span>
   
    <span data-ttu-id="d1104-268">c.</span><span class="sxs-lookup"><span data-stu-id="d1104-268">c.</span></span> <span data-ttu-id="d1104-269">Ange den **efternamn** i textrutan</span><span class="sxs-lookup"><span data-stu-id="d1104-269">Enter the **Last Name**  in the textbox</span></span>
   
    <span data-ttu-id="d1104-270">d.</span><span class="sxs-lookup"><span data-stu-id="d1104-270">d.</span></span> <span data-ttu-id="d1104-271">Klicka på **Nästa**</span><span class="sxs-lookup"><span data-stu-id="d1104-271">Click **Next**</span></span>

6. <span data-ttu-id="d1104-272">I den **behörigheter** väljer den **userRoles** och på **nästa**</span><span class="sxs-lookup"><span data-stu-id="d1104-272">In the **Permissions** tab, select the **userRoles** and click **Next**</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)
7. <span data-ttu-id="d1104-274">Klicka på den **skicka** för att skicka användarinbjudan</span><span class="sxs-lookup"><span data-stu-id="d1104-274">Click the **Send** button to send the user invitation</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. <span data-ttu-id="d1104-276">Användaren får e-postmeddelande och klicka på länken och ändra lösenord för att aktivera kontot.</span><span class="sxs-lookup"><span data-stu-id="d1104-276">User receives the email notification and has to click the link and change the password to activate the account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d1104-277">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d1104-277">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d1104-278">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Marketo.</span><span class="sxs-lookup"><span data-stu-id="d1104-278">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Marketo.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d1104-280">**Om du vill tilldela Marketo Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d1104-280">**To assign Britta Simon to Marketo, perform the following steps:**</span></span>

1. <span data-ttu-id="d1104-281">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d1104-281">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d1104-283">Välj i listan med program **Marketo**.</span><span class="sxs-lookup"><span data-stu-id="d1104-283">In the applications list, select **Marketo**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_app.png) 

3. <span data-ttu-id="d1104-285">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d1104-285">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d1104-287">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1104-287">Click **Add** button.</span></span> <span data-ttu-id="d1104-288">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1104-288">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d1104-290">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d1104-290">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d1104-291">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1104-291">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d1104-292">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d1104-292">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d1104-293">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d1104-293">Testing single sign-on</span></span>

<span data-ttu-id="d1104-294">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d1104-294">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d1104-295">När du klickar på panelen Marketo på åtkomstpanelen du bör få automatiskt loggat in på ditt Marketo-program.</span><span class="sxs-lookup"><span data-stu-id="d1104-295">When you click the Marketo tile in the Access Panel, you should get automatically signed-on to your Marketo application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d1104-296">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d1104-296">Additional resources</span></span>

* [<span data-ttu-id="d1104-297">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d1104-297">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d1104-298">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d1104-298">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png

