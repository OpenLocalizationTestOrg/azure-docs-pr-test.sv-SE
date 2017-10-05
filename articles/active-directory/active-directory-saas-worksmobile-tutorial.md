---
title: "Självstudier: Azure Active Directory-integrering med fungerar MOBILE | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och fungerar MOBILE."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 139a1968a59424eae278de3e7fa227ad340a1eb8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-works-mobile"></a><span data-ttu-id="cb59b-103">Självstudier: Azure Active Directory-integrering med fungerar MOBILE</span><span class="sxs-lookup"><span data-stu-id="cb59b-103">Tutorial: Azure Active Directory integration with WORKS MOBILE</span></span>

<span data-ttu-id="cb59b-104">I kursen får du lära dig hur du integrerar MOBILE fungerar med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="cb59b-104">In this tutorial, you learn how to integrate WORKS MOBILE with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cb59b-105">Integrerar MOBILE fungerar med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="cb59b-105">Integrating WORKS MOBILE with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cb59b-106">Du kan styra i Azure AD som har åtkomst till fungerar MOBILE</span><span class="sxs-lookup"><span data-stu-id="cb59b-106">You can control in Azure AD who has access to WORKS MOBILE</span></span>
- <span data-ttu-id="cb59b-107">Du kan aktivera användarna att automatiskt hämta loggat in på fungerar MOBILE (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="cb59b-107">You can enable your users to automatically get signed-on to WORKS MOBILE (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cb59b-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="cb59b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cb59b-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cb59b-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cb59b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="cb59b-110">Prerequisites</span></span>

<span data-ttu-id="cb59b-111">För att konfigurera Azure AD-integrering med MOBILE fungerar, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="cb59b-111">To configure Azure AD integration with WORKS MOBILE, you need the following items:</span></span>

- <span data-ttu-id="cb59b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="cb59b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cb59b-113">En MOBILE fungerar enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="cb59b-113">A WORKS MOBILE single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cb59b-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="cb59b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cb59b-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="cb59b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cb59b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="cb59b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cb59b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cb59b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cb59b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="cb59b-118">Scenario description</span></span>
<span data-ttu-id="cb59b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="cb59b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cb59b-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="cb59b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cb59b-121">Att lägga till fungerar MOBILE från galleriet</span><span class="sxs-lookup"><span data-stu-id="cb59b-121">Adding WORKS MOBILE from the gallery</span></span>
2. <span data-ttu-id="cb59b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cb59b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-works-mobile-from-the-gallery"></a><span data-ttu-id="cb59b-123">Att lägga till fungerar MOBILE från galleriet</span><span class="sxs-lookup"><span data-stu-id="cb59b-123">Adding WORKS MOBILE from the gallery</span></span>
<span data-ttu-id="cb59b-124">Du måste lägga till MOBILE fungerar från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av MOBILE fungerar i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="cb59b-124">To configure the integration of WORKS MOBILE into Azure AD, you need to add WORKS MOBILE from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cb59b-125">**Utför följande steg för att lägga till MOBILE fungerar från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="cb59b-125">**To add WORKS MOBILE from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cb59b-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cb59b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cb59b-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cb59b-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="cb59b-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cb59b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="cb59b-133">I sökrutan skriver **fungerar MOBILE**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-133">In the search box, type **WORKS MOBILE**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_search.png)

5. <span data-ttu-id="cb59b-135">Välj i resultatpanelen **fungerar MOBILE**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="cb59b-135">In the results panel, select **WORKS MOBILE**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cb59b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cb59b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cb59b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med fungerar MOBILE baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="cb59b-138">In this section, you configure and test Azure AD single sign-on with WORKS MOBILE based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cb59b-139">Azure AD måste du känna till motsvarande användaren i fungerar MOBILE till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="cb59b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in WORKS MOBILE is to a user in Azure AD.</span></span> <span data-ttu-id="cb59b-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i fungerar MOBILE upprättas.</span><span class="sxs-lookup"><span data-stu-id="cb59b-140">In other words, a link relationship between an Azure AD user and the related user in WORKS MOBILE needs to be established.</span></span>

<span data-ttu-id="cb59b-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i MOBILE fungerar.</span><span class="sxs-lookup"><span data-stu-id="cb59b-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in WORKS MOBILE.</span></span>

<span data-ttu-id="cb59b-142">Om du vill konfigurera och testa Azure AD enkel inloggning med MOBILE fungerar, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="cb59b-142">To configure and test Azure AD single sign-on with WORKS MOBILE, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cb59b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="cb59b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cb59b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cb59b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cb59b-145">**[Skapa en testanvändare fungerar MOBILE](#creating-a-works-mobile-test-user)**  – du har en motsvarighet för Britta Simon i fungerar MOBILE som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="cb59b-145">**[Creating a WORKS MOBILE test user](#creating-a-works-mobile-test-user)** - to have a counterpart of Britta Simon in WORKS MOBILE that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cb59b-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cb59b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cb59b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="cb59b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cb59b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cb59b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cb59b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program fungerar MOBILE.</span><span class="sxs-lookup"><span data-stu-id="cb59b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your WORKS MOBILE application.</span></span>

<span data-ttu-id="cb59b-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med fungerar MOBILE:**</span><span class="sxs-lookup"><span data-stu-id="cb59b-150">**To configure Azure AD single sign-on with WORKS MOBILE, perform the following steps:**</span></span>

1. <span data-ttu-id="cb59b-151">I Azure-portalen på den **fungerar MOBILE** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-151">In the Azure portal, on the **WORKS MOBILE** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="cb59b-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="cb59b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_samlbase.png)

3. <span data-ttu-id="cb59b-155">På den **fungerar MOBILE domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cb59b-155">On the **WORKS MOBILE Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_url.png)

    <span data-ttu-id="cb59b-157">a.</span><span class="sxs-lookup"><span data-stu-id="cb59b-157">a.</span></span> <span data-ttu-id="cb59b-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.worksmobile.com/jp/myservice`</span><span class="sxs-lookup"><span data-stu-id="cb59b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.worksmobile.com/jp/myservice`</span></span>

    <span data-ttu-id="cb59b-159">b.</span><span class="sxs-lookup"><span data-stu-id="cb59b-159">b.</span></span> <span data-ttu-id="cb59b-160">I den **identifierare** textruta Skriv värdet som`worksmobile.com`</span><span class="sxs-lookup"><span data-stu-id="cb59b-160">In the **Identifier** textbox, type the value as `worksmobile.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cb59b-161">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="cb59b-161">This value is not real.</span></span> <span data-ttu-id="cb59b-162">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="cb59b-162">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="cb59b-163">Kontakta [fungerar mobila klienten supportteamet](mailto:dl_ssoinfo@worksmobile.com) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="cb59b-163">Contact [WORKS MOBILE Client support team](mailto:dl_ssoinfo@worksmobile.com) to get this value.</span></span> 
 
4. <span data-ttu-id="cb59b-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="cb59b-164">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_certificate.png) 

5. <span data-ttu-id="cb59b-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="cb59b-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-worksmobile-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cb59b-168">På den **fungerar MOBILE Configuration** klickar du på **konfigurera fungerar MOBILE** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="cb59b-168">On the **WORKS MOBILE Configuration** section, click **Configure WORKS MOBILE** to open **Configure sign-on** window.</span></span> <span data-ttu-id="cb59b-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="cb59b-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_configure.png) 

7. <span data-ttu-id="cb59b-171">För att få SSO konfigurerats för ditt program, kontakta [fungerar MOBILE supportteamet](mailto:dl_ssoinfo@worksmobile.com) och ge dem med följande information:</span><span class="sxs-lookup"><span data-stu-id="cb59b-171">To get SSO configured for your application, contact [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) and provide them with the following information:</span></span> 

    <span data-ttu-id="cb59b-172">• Den hämtade **certifikatfilen**</span><span class="sxs-lookup"><span data-stu-id="cb59b-172">• The downloaded **Certificate file**</span></span>

    <span data-ttu-id="cb59b-173">• Den **URL för SAML-tjänst för enkel inloggning**</span><span class="sxs-lookup"><span data-stu-id="cb59b-173">• The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="cb59b-174">• Den **SAML enhets-ID**</span><span class="sxs-lookup"><span data-stu-id="cb59b-174">• The **SAML Entity ID**</span></span>

    <span data-ttu-id="cb59b-175">• Den **URL för utloggning**</span><span class="sxs-lookup"><span data-stu-id="cb59b-175">• The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="cb59b-176">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="cb59b-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cb59b-177">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="cb59b-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cb59b-178">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cb59b-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cb59b-179">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="cb59b-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="cb59b-180">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cb59b-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="cb59b-182">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="cb59b-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cb59b-183">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="cb59b-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cb59b-185">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cb59b-187">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cb59b-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cb59b-189">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="cb59b-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-worksmobile-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cb59b-191">a.</span><span class="sxs-lookup"><span data-stu-id="cb59b-191">a.</span></span> <span data-ttu-id="cb59b-192">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cb59b-193">b.</span><span class="sxs-lookup"><span data-stu-id="cb59b-193">b.</span></span> <span data-ttu-id="cb59b-194">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cb59b-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cb59b-195">c.</span><span class="sxs-lookup"><span data-stu-id="cb59b-195">c.</span></span> <span data-ttu-id="cb59b-196">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cb59b-197">d.</span><span class="sxs-lookup"><span data-stu-id="cb59b-197">d.</span></span> <span data-ttu-id="cb59b-198">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-198">Click **Create**.</span></span>
 
### <a name="creating-a-works-mobile-test-user"></a><span data-ttu-id="cb59b-199">Skapa en testanvändare fungerar MOBILE</span><span class="sxs-lookup"><span data-stu-id="cb59b-199">Creating a WORKS MOBILE test user</span></span>

 <span data-ttu-id="cb59b-200">I det här avsnittet kan du skapa en användare som kallas Britta Simon i MOBILE fungerar.</span><span class="sxs-lookup"><span data-stu-id="cb59b-200">In this section, you create a user called Britta Simon in WORKS MOBILE.</span></span> <span data-ttu-id="cb59b-201">Se tillsammans med [fungerar MOBILE supportteamet](mailto:dl_ssoinfo@worksmobile.com) att lägga till användare i fungerar MOBILE-plattformen.</span><span class="sxs-lookup"><span data-stu-id="cb59b-201">Please work with [WORKS MOBILE support team](mailto:dl_ssoinfo@worksmobile.com) to add the users in the WORKS MOBILE platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cb59b-202">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="cb59b-202">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cb59b-203">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till MOBILE fungerar.</span><span class="sxs-lookup"><span data-stu-id="cb59b-203">In this section, you enable Britta Simon to use Azure single sign-on by granting access to WORKS MOBILE.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="cb59b-205">**Om du vill tilldela fungerar MOBILE Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="cb59b-205">**To assign Britta Simon to WORKS MOBILE, perform the following steps:**</span></span>

1. <span data-ttu-id="cb59b-206">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-206">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="cb59b-208">Välj i listan med program **fungerar MOBILE**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-208">In the applications list, select **WORKS MOBILE**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-worksmobile-tutorial/tutorial_worksmobile_app.png) 

3. <span data-ttu-id="cb59b-210">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="cb59b-210">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="cb59b-212">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="cb59b-212">Click **Add** button.</span></span> <span data-ttu-id="cb59b-213">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cb59b-213">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="cb59b-215">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="cb59b-215">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cb59b-216">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cb59b-216">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cb59b-217">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="cb59b-217">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cb59b-218">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="cb59b-218">Testing single sign-on</span></span>

<span data-ttu-id="cb59b-219">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="cb59b-219">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="cb59b-220">När du klickar på panelen fungerar MOBILE på åtkomstpanelen du ska hämta automatiskt loggat in på ditt fungerar mobila program.</span><span class="sxs-lookup"><span data-stu-id="cb59b-220">When you click the WORKS MOBILE tile in the Access Panel, you should get automatically signed-on to your WORKS MOBILE application.</span></span>
<span data-ttu-id="cb59b-221">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cb59b-221">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cb59b-222">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="cb59b-222">Additional resources</span></span>

* [<span data-ttu-id="cb59b-223">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cb59b-223">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cb59b-224">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="cb59b-224">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-worksmobile-tutorial/tutorial_general_203.png

