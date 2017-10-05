---
title: "Självstudier: Azure Active Directory-integrering med Certifikatutfärdare PPM | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Certifikatutfärdaren PPM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ca9d5e71-e429-4891-8d10-3498e7210e89
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 4ca9268c26f681fcc96955b6161fe4a119b2dcf4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a><span data-ttu-id="7292d-103">Självstudier: Azure Active Directory-integrering med Certifikatutfärdare PPM</span><span class="sxs-lookup"><span data-stu-id="7292d-103">Tutorial: Azure Active Directory integration with CA PPM</span></span>

<span data-ttu-id="7292d-104">I kursen får lära du att integrera CA PPM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7292d-104">In this tutorial, you learn how to integrate CA PPM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7292d-105">Integrera CA PPM med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7292d-105">Integrating CA PPM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7292d-106">Du kan styra i Azure AD som har åtkomst till CA PPM</span><span class="sxs-lookup"><span data-stu-id="7292d-106">You can control in Azure AD who has access to CA PPM</span></span>
- <span data-ttu-id="7292d-107">Du kan aktivera användarna att automatiskt hämta loggat in på CA PPM (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7292d-107">You can enable your users to automatically get signed-on to CA PPM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7292d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7292d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7292d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7292d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7292d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7292d-110">Prerequisites</span></span>

<span data-ttu-id="7292d-111">För att konfigurera Azure AD-integrering med Certifikatutfärdare PPM, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7292d-111">To configure Azure AD integration with CA PPM, you need the following items:</span></span>

- <span data-ttu-id="7292d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7292d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7292d-113">En CA PPM enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="7292d-113">A CA PPM single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7292d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7292d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7292d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7292d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7292d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7292d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7292d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7292d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7292d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7292d-118">Scenario description</span></span>
<span data-ttu-id="7292d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7292d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7292d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7292d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7292d-121">Att lägga till CA PPM från galleriet</span><span class="sxs-lookup"><span data-stu-id="7292d-121">Adding CA PPM from the gallery</span></span>
2. <span data-ttu-id="7292d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7292d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ca-ppm-from-the-gallery"></a><span data-ttu-id="7292d-123">Att lägga till CA PPM från galleriet</span><span class="sxs-lookup"><span data-stu-id="7292d-123">Adding CA PPM from the gallery</span></span>
<span data-ttu-id="7292d-124">Du måste lägga till CA PPM från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Certifikatutfärdaren PPM i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7292d-124">To configure the integration of CA PPM into Azure AD, you need to add CA PPM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7292d-125">**Utför följande steg för att lägga till CA PPM från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7292d-125">**To add CA PPM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7292d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7292d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7292d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7292d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7292d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7292d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7292d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7292d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7292d-133">I sökrutan skriver **CA PPM**.</span><span class="sxs-lookup"><span data-stu-id="7292d-133">In the search box, type **CA PPM**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_search.png)

5. <span data-ttu-id="7292d-135">Välj i resultatpanelen **CA PPM**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7292d-135">In the results panel, select **CA PPM**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7292d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7292d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7292d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med CA PPM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7292d-138">In this section, you configure and test Azure AD single sign-on with CA PPM based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="7292d-139">Azure AD måste du känna till motsvarande användaren i Certifikatutfärdaren PPM till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7292d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in CA PPM is to a user in Azure AD.</span></span> <span data-ttu-id="7292d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Certifikatutfärdaren PPM upprättas.</span><span class="sxs-lookup"><span data-stu-id="7292d-140">In other words, a link relationship between an Azure AD user and the related user in CA PPM needs to be established.</span></span>

<span data-ttu-id="7292d-141">I Certifikatutfärdarens PPM tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7292d-141">In CA PPM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7292d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med CA PPM, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7292d-142">To configure and test Azure AD single sign-on with CA PPM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7292d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7292d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7292d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7292d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7292d-145">**[Skapa en testanvändare CA PPM](#creating-a-ca-ppm-test-user)**  – du har en motsvarighet för Britta Simon i CA-PPM som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7292d-145">**[Creating a CA PPM test user](#creating-a-ca-ppm-test-user)** - to have a counterpart of Britta Simon in CA PPM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7292d-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7292d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7292d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7292d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7292d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7292d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7292d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt CA PPM-program.</span><span class="sxs-lookup"><span data-stu-id="7292d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your CA PPM application.</span></span>

<span data-ttu-id="7292d-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med CA PPM:**</span><span class="sxs-lookup"><span data-stu-id="7292d-150">**To configure Azure AD single sign-on with CA PPM, perform the following steps:**</span></span>

1. <span data-ttu-id="7292d-151">I Azure-portalen på den **CA PPM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7292d-151">In the Azure portal, on the **CA PPM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7292d-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7292d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_samlbase.png)

3. <span data-ttu-id="7292d-155">På den **URL: er och CA PPM domän** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7292d-155">On the **CA PPM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_url.png)

    <span data-ttu-id="7292d-157">a.</span><span class="sxs-lookup"><span data-stu-id="7292d-157">a.</span></span> <span data-ttu-id="7292d-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://ca.ondemand.saml.20.post.<companyname>`</span><span class="sxs-lookup"><span data-stu-id="7292d-158">In the **Identifier** textbox, type a URL using the following pattern: `https://ca.ondemand.saml.20.post.<companyname>`</span></span>
    
    <span data-ttu-id="7292d-159">b.</span><span class="sxs-lookup"><span data-stu-id="7292d-159">b.</span></span> <span data-ttu-id="7292d-160">I den **Reply URL** textruta typ som:`https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="7292d-160">In the **Reply URL** textbox, type as: `https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7292d-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7292d-161">This value is not real.</span></span> <span data-ttu-id="7292d-162">Uppdatera det här värdet med den faktiska identifieraren.</span><span class="sxs-lookup"><span data-stu-id="7292d-162">Update this value with the actual Identifier.</span></span> <span data-ttu-id="7292d-163">Kontakta [CA PPM supportteamet](mailto:catechnicalsupport@ca.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="7292d-163">Contact [CA PPM support team](mailto:catechnicalsupport@ca.com) to get this value.</span></span>
 
4. <span data-ttu-id="7292d-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7292d-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_certificate.png) 

5. <span data-ttu-id="7292d-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7292d-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7292d-168">På den **PPM certifikatutfärdarkonfiguration** klickar du på **konfigurera Certifikatutfärdaren PPM** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7292d-168">On the **CA PPM Configuration** section, click **Configure CA PPM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7292d-169">Kopiera den **SAML enhets-ID** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7292d-169">Copy the **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_configure.png) 

7. <span data-ttu-id="7292d-171">Konfigurera enkel inloggning på **CA PPM** sida, måste du skicka den hämtade **Certificate(Base64)** och **SAML enhets-ID** till [CA PPM supportteamet](mailto:catechnicalsupport@ca.com).</span><span class="sxs-lookup"><span data-stu-id="7292d-171">To configure single sign-on on **CA PPM** side, you need to send the downloaded **Certificate(Base64)** and **SAML Entity ID** to [CA PPM support team](mailto:catechnicalsupport@ca.com).</span></span>

> [!TIP]
> <span data-ttu-id="7292d-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7292d-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7292d-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7292d-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7292d-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7292d-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7292d-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7292d-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="7292d-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7292d-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7292d-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7292d-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7292d-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7292d-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7292d-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7292d-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7292d-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7292d-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7292d-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7292d-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7292d-187">a.</span><span class="sxs-lookup"><span data-stu-id="7292d-187">a.</span></span> <span data-ttu-id="7292d-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7292d-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7292d-189">b.</span><span class="sxs-lookup"><span data-stu-id="7292d-189">b.</span></span> <span data-ttu-id="7292d-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7292d-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7292d-191">c.</span><span class="sxs-lookup"><span data-stu-id="7292d-191">c.</span></span> <span data-ttu-id="7292d-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7292d-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7292d-193">d.</span><span class="sxs-lookup"><span data-stu-id="7292d-193">d.</span></span> <span data-ttu-id="7292d-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7292d-194">Click **Create**.</span></span>
 
### <a name="creating-a-ca-ppm-test-user"></a><span data-ttu-id="7292d-195">Skapa en CA PPM testanvändare</span><span class="sxs-lookup"><span data-stu-id="7292d-195">Creating a CA PPM test user</span></span>

<span data-ttu-id="7292d-196">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Certifikatutfärdaren PPM.</span><span class="sxs-lookup"><span data-stu-id="7292d-196">In this section, you create a user called Britta Simon in CA PPM.</span></span> <span data-ttu-id="7292d-197">Arbeta med [CA PPM supportteamet](mailto:catechnicalsupport@ca.com) att lägga till användare i Certifikatutfärdaren PPM-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7292d-197">Work with [CA PPM support team](mailto:catechnicalsupport@ca.com) to add the users in the CA PPM platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7292d-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7292d-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7292d-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till CA PPM.</span><span class="sxs-lookup"><span data-stu-id="7292d-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to CA PPM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7292d-201">**Om du vill tilldela CA PPM Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7292d-201">**To assign Britta Simon to CA PPM, perform the following steps:**</span></span>

1. <span data-ttu-id="7292d-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7292d-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7292d-204">Välj i listan med program **CA PPM**.</span><span class="sxs-lookup"><span data-stu-id="7292d-204">In the applications list, select **CA PPM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_app.png) 

3. <span data-ttu-id="7292d-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7292d-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7292d-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7292d-208">Click **Add** button.</span></span> <span data-ttu-id="7292d-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7292d-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7292d-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7292d-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7292d-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7292d-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7292d-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7292d-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7292d-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7292d-214">Testing single sign-on</span></span>

<span data-ttu-id="7292d-215">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7292d-215">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="7292d-216">När du klickar på panelen CA PPM på åtkomstpanelen du bör få automatiskt loggat in på ditt CA PPM-program.</span><span class="sxs-lookup"><span data-stu-id="7292d-216">When you click the CA PPM tile in the Access Panel, you should get automatically signed-on to your CA PPM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7292d-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7292d-217">Additional resources</span></span>

* [<span data-ttu-id="7292d-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7292d-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7292d-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7292d-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png

