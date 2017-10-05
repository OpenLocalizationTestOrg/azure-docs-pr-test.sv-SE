---
title: "Självstudier: Azure Active Directory-integrering med RedVector | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och RedVector."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 99042f39-0ab2-475b-8df8-3016d7f875e9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: d7aedd360ba1d4c11da210f8fae7be6035007c22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-redvector"></a><span data-ttu-id="af223-103">Självstudier: Azure Active Directory-integrering med RedVector</span><span class="sxs-lookup"><span data-stu-id="af223-103">Tutorial: Azure Active Directory integration with RedVector</span></span>

<span data-ttu-id="af223-104">I kursen får lära du att integrera RedVector med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="af223-104">In this tutorial, you learn how to integrate RedVector with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="af223-105">Integrera RedVector med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="af223-105">Integrating RedVector with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="af223-106">Du kan styra i Azure AD som har åtkomst till RedVector</span><span class="sxs-lookup"><span data-stu-id="af223-106">You can control in Azure AD who has access to RedVector</span></span>
- <span data-ttu-id="af223-107">Du kan aktivera användarna att automatiskt hämta loggat in på RedVector (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="af223-107">You can enable your users to automatically get signed-on to RedVector (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="af223-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="af223-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="af223-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="af223-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af223-110">Krav</span><span class="sxs-lookup"><span data-stu-id="af223-110">Prerequisites</span></span>

<span data-ttu-id="af223-111">För att konfigurera Azure AD-integrering med RedVector, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="af223-111">To configure Azure AD integration with RedVector, you need the following items:</span></span>

- <span data-ttu-id="af223-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="af223-112">An Azure AD subscription</span></span>
- <span data-ttu-id="af223-113">En RedVector enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="af223-113">A RedVector single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="af223-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="af223-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="af223-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="af223-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="af223-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="af223-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="af223-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="af223-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="af223-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="af223-118">Scenario description</span></span>
<span data-ttu-id="af223-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="af223-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="af223-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="af223-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="af223-121">Att lägga till RedVector från galleriet</span><span class="sxs-lookup"><span data-stu-id="af223-121">Adding RedVector from the gallery</span></span>
2. <span data-ttu-id="af223-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="af223-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-redvector-from-the-gallery"></a><span data-ttu-id="af223-123">Att lägga till RedVector från galleriet</span><span class="sxs-lookup"><span data-stu-id="af223-123">Adding RedVector from the gallery</span></span>
<span data-ttu-id="af223-124">Du måste lägga till RedVector från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av RedVector i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="af223-124">To configure the integration of RedVector into Azure AD, you need to add RedVector from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="af223-125">**Utför följande steg för att lägga till RedVector från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="af223-125">**To add RedVector from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="af223-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="af223-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="af223-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="af223-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="af223-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="af223-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="af223-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="af223-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="af223-133">I sökrutan skriver **RedVector**.</span><span class="sxs-lookup"><span data-stu-id="af223-133">In the search box, type **RedVector**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_search.png)

5. <span data-ttu-id="af223-135">Välj i resultatpanelen **RedVector**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="af223-135">In the results panel, select **RedVector**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="af223-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="af223-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="af223-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med RedVector baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="af223-138">In this section, you configure and test Azure AD single sign-on with RedVector based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="af223-139">Azure AD måste du känna till användaren i RedVector motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="af223-139">For single sign-on to work, Azure AD needs to know what the counterpart user in RedVector is to a user in Azure AD.</span></span> <span data-ttu-id="af223-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i RedVector upprättas.</span><span class="sxs-lookup"><span data-stu-id="af223-140">In other words, a link relationship between an Azure AD user and the related user in RedVector needs to be established.</span></span>

<span data-ttu-id="af223-141">I RedVector, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="af223-141">In RedVector, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="af223-142">Om du vill konfigurera och testa Azure AD enkel inloggning med RedVector, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="af223-142">To configure and test Azure AD single sign-on with RedVector, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="af223-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="af223-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="af223-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af223-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="af223-145">**[Skapa en testanvändare RedVector](#creating-a-redvector-test-user)**  – du har en motsvarighet för Britta Simon i RedVector som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="af223-145">**[Creating a RedVector test user](#creating-a-redvector-test-user)** - to have a counterpart of Britta Simon in RedVector that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="af223-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="af223-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="af223-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="af223-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="af223-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="af223-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="af223-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt RedVector program.</span><span class="sxs-lookup"><span data-stu-id="af223-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your RedVector application.</span></span>

<span data-ttu-id="af223-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med RedVector:**</span><span class="sxs-lookup"><span data-stu-id="af223-150">**To configure Azure AD single sign-on with RedVector, perform the following steps:**</span></span>

1. <span data-ttu-id="af223-151">I Azure-portalen på den **RedVector** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="af223-151">In the Azure portal, on the **RedVector** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="af223-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="af223-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_samlbase.png)

3. <span data-ttu-id="af223-155">På den **RedVector domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="af223-155">On the **RedVector Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_url.png)

    <span data-ttu-id="af223-157">a.</span><span class="sxs-lookup"><span data-stu-id="af223-157">a.</span></span> <span data-ttu-id="af223-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://sso2.redvector.com/adfs/<Companyname>`</span><span class="sxs-lookup"><span data-stu-id="af223-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso2.redvector.com/adfs/<Companyname>`</span></span>

    <span data-ttu-id="af223-159">b.</span><span class="sxs-lookup"><span data-stu-id="af223-159">b.</span></span> <span data-ttu-id="af223-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<Companyname>.redvector.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="af223-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<Companyname>.redvector.com/saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="af223-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="af223-161">These values are not real.</span></span> <span data-ttu-id="af223-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="af223-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="af223-163">Kontakta [RedVector klienten supportteamet](mailto:sso@redvector.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="af223-163">Contact [RedVector Client support team](mailto:sso@redvector.com) to get these values.</span></span> 
 
4. <span data-ttu-id="af223-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="af223-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_certificate.png) 

5. <span data-ttu-id="af223-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="af223-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="af223-168">På den **RedVector Configuration** klickar du på **konfigurera RedVector** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="af223-168">On the **RedVector Configuration** section, click **Configure RedVector** to open **Configure sign-on** window.</span></span> <span data-ttu-id="af223-169">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="af223-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_configure.png) 

7. <span data-ttu-id="af223-171">Konfigurera enkel inloggning på **RedVector** sida, måste du skicka den hämtade **certifikat (Base64)** och **SAML enkel inloggning Tjänstwebbadress** till [ RedVector supportteamet](mailto:sso@redvector.com).</span><span class="sxs-lookup"><span data-stu-id="af223-171">To configure single sign-on on **RedVector** side, you need to send the downloaded **Certificate (Base64)** and **SAML Single Sign-On Service URL** to [RedVector support team](mailto:sso@redvector.com).</span></span> <span data-ttu-id="af223-172">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="af223-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="af223-173">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="af223-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="af223-174">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="af223-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="af223-175">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="af223-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="af223-176">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="af223-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="af223-177">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="af223-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="af223-179">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="af223-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="af223-180">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="af223-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="af223-182">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="af223-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="af223-184">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="af223-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="af223-186">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="af223-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-redvector-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="af223-188">a.</span><span class="sxs-lookup"><span data-stu-id="af223-188">a.</span></span> <span data-ttu-id="af223-189">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="af223-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="af223-190">b.</span><span class="sxs-lookup"><span data-stu-id="af223-190">b.</span></span> <span data-ttu-id="af223-191">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="af223-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="af223-192">c.</span><span class="sxs-lookup"><span data-stu-id="af223-192">c.</span></span> <span data-ttu-id="af223-193">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="af223-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="af223-194">d.</span><span class="sxs-lookup"><span data-stu-id="af223-194">d.</span></span> <span data-ttu-id="af223-195">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="af223-195">Click **Create**.</span></span>
 
### <a name="creating-a-redvector-test-user"></a><span data-ttu-id="af223-196">Skapa en testanvändare RedVector</span><span class="sxs-lookup"><span data-stu-id="af223-196">Creating a RedVector test user</span></span>

<span data-ttu-id="af223-197">I det här avsnittet skapar du en användare som kallas Britta Simon i RedVector.</span><span class="sxs-lookup"><span data-stu-id="af223-197">In this section, you create a user called Britta Simon in RedVector.</span></span> <span data-ttu-id="af223-198">Om du inte vet hur du lägger till Britta Simon i RedVector, arbeta med [RedVector supportteamet](mailto:sso@redvector.com) att lägga till användaren och aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="af223-198">If you don't know how to add Britta Simon in RedVector, Work with [RedVector support team](mailto:sso@redvector.com) to add the test user and enable SSO.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="af223-199">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="af223-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="af223-200">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till RedVector.</span><span class="sxs-lookup"><span data-stu-id="af223-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to RedVector.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="af223-202">**Om du vill tilldela RedVector Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="af223-202">**To assign Britta Simon to RedVector, perform the following steps:**</span></span>

1. <span data-ttu-id="af223-203">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="af223-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="af223-205">Välj i listan med program **RedVector**.</span><span class="sxs-lookup"><span data-stu-id="af223-205">In the applications list, select **RedVector**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-redvector-tutorial/tutorial_redvector_app.png) 

3. <span data-ttu-id="af223-207">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="af223-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="af223-209">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="af223-209">Click **Add** button.</span></span> <span data-ttu-id="af223-210">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="af223-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="af223-212">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="af223-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="af223-213">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="af223-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="af223-214">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="af223-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="af223-215">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="af223-215">Testing single sign-on</span></span>

<span data-ttu-id="af223-216">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="af223-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="af223-217">När du klickar på panelen RedVector på åtkomstpanelen du bör få automatiskt loggat in på ditt RedVector program...</span><span class="sxs-lookup"><span data-stu-id="af223-217">When you click the RedVector tile in the Access Panel, you should get automatically signed-on to your RedVector application..</span></span>

## <a name="additional-resources"></a><span data-ttu-id="af223-218">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="af223-218">Additional resources</span></span>

* [<span data-ttu-id="af223-219">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="af223-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="af223-220">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="af223-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-redvector-tutorial/tutorial_general_203.png

