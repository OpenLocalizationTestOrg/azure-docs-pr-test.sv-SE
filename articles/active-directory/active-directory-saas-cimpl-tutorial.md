---
title: "Självstudier: Azure Active Directory-integrering med Cimpl | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Cimpl."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 58ee5481-ae40-4e4a-a3c9-86343851fc9a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: jeedes
ms.openlocfilehash: 24a0c89966c83e1b32367d4519ead98d76f5ac6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cimpl"></a><span data-ttu-id="b6a89-103">Självstudier: Azure Active Directory-integrering med Cimpl</span><span class="sxs-lookup"><span data-stu-id="b6a89-103">Tutorial: Azure Active Directory integration with Cimpl</span></span>

<span data-ttu-id="b6a89-104">I kursen får lära du att integrera Cimpl med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b6a89-104">In this tutorial, you learn how to integrate Cimpl with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b6a89-105">Integrera Cimpl med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b6a89-105">Integrating Cimpl with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b6a89-106">Du kan styra i Azure AD som har åtkomst till Cimpl</span><span class="sxs-lookup"><span data-stu-id="b6a89-106">You can control in Azure AD who has access to Cimpl</span></span>
- <span data-ttu-id="b6a89-107">Du kan aktivera användarna att automatiskt hämta loggat in på Cimpl (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b6a89-107">You can enable your users to automatically get signed-on to Cimpl (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b6a89-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b6a89-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b6a89-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b6a89-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6a89-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b6a89-110">Prerequisites</span></span>

<span data-ttu-id="b6a89-111">För att konfigurera Azure AD-integrering med Cimpl, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b6a89-111">To configure Azure AD integration with Cimpl, you need the following items:</span></span>

- <span data-ttu-id="b6a89-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b6a89-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b6a89-113">En Cimpl enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="b6a89-113">A Cimpl single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b6a89-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b6a89-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b6a89-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b6a89-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b6a89-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b6a89-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b6a89-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b6a89-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b6a89-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b6a89-118">Scenario description</span></span>
<span data-ttu-id="b6a89-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b6a89-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b6a89-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b6a89-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b6a89-121">Att lägga till Cimpl från galleriet</span><span class="sxs-lookup"><span data-stu-id="b6a89-121">Adding Cimpl from the gallery</span></span>
2. <span data-ttu-id="b6a89-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6a89-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cimpl-from-the-gallery"></a><span data-ttu-id="b6a89-123">Att lägga till Cimpl från galleriet</span><span class="sxs-lookup"><span data-stu-id="b6a89-123">Adding Cimpl from the gallery</span></span>
<span data-ttu-id="b6a89-124">Du måste lägga till Cimpl från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Cimpl i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b6a89-124">To configure the integration of Cimpl into Azure AD, you need to add Cimpl from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b6a89-125">**Utför följande steg för att lägga till Cimpl från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b6a89-125">**To add Cimpl from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b6a89-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b6a89-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b6a89-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b6a89-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b6a89-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6a89-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b6a89-133">I sökrutan skriver **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-133">In the search box, type **Cimpl**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_search.png)

5. <span data-ttu-id="b6a89-135">Välj i resultatpanelen **Cimpl**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b6a89-135">In the results panel, select **Cimpl**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b6a89-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6a89-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b6a89-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Cimpl baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b6a89-138">In this section, you configure and test Azure AD single sign-on with Cimpl based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b6a89-139">Azure AD måste du känna till användaren i Cimpl motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b6a89-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cimpl is to a user in Azure AD.</span></span> <span data-ttu-id="b6a89-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Cimpl upprättas.</span><span class="sxs-lookup"><span data-stu-id="b6a89-140">In other words, a link relationship between an Azure AD user and the related user in Cimpl needs to be established.</span></span>

<span data-ttu-id="b6a89-141">I Cimpl, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b6a89-141">In Cimpl, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b6a89-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Cimpl, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b6a89-142">To configure and test Azure AD single sign-on with Cimpl, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b6a89-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b6a89-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b6a89-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6a89-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b6a89-145">**[Skapa en testanvändare Cimpl](#creating-a-cimpl-test-user)**  – du har en motsvarighet för Britta Simon i Cimpl som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b6a89-145">**[Creating a Cimpl test user](#creating-a-cimpl-test-user)** - to have a counterpart of Britta Simon in Cimpl that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b6a89-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6a89-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b6a89-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b6a89-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b6a89-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6a89-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b6a89-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Cimpl program.</span><span class="sxs-lookup"><span data-stu-id="b6a89-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cimpl application.</span></span>

<span data-ttu-id="b6a89-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Cimpl:**</span><span class="sxs-lookup"><span data-stu-id="b6a89-150">**To configure Azure AD single sign-on with Cimpl, perform the following steps:**</span></span>

1. <span data-ttu-id="b6a89-151">I Azure-portalen på den **Cimpl** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-151">In the Azure portal, on the **Cimpl** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b6a89-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b6a89-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_samlbase.png)

3. <span data-ttu-id="b6a89-155">På den **Cimpl domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b6a89-155">On the **Cimpl Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_url.png)

    <span data-ttu-id="b6a89-157">a.</span><span class="sxs-lookup"><span data-stu-id="b6a89-157">a.</span></span> <span data-ttu-id="b6a89-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="b6a89-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    <span data-ttu-id="b6a89-159">b.</span><span class="sxs-lookup"><span data-stu-id="b6a89-159">b.</span></span> <span data-ttu-id="b6a89-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://sso.etelesolv.com/<TENANTNAME>`</span><span class="sxs-lookup"><span data-stu-id="b6a89-160">In the **Identifier** textbox, type a URL using the following pattern: `https://sso.etelesolv.com/<TENANTNAME>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b6a89-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="b6a89-161">These values are not real.</span></span> <span data-ttu-id="b6a89-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="b6a89-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b6a89-163">Kontakta Cimpl-teamet på **+1 866-982-8250** att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b6a89-163">Contact Cimpl team at **+1 866-982-8250** to get these values.</span></span> 
 
4. <span data-ttu-id="b6a89-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b6a89-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_certificate.png) 

5. <span data-ttu-id="b6a89-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b6a89-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cimpl-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b6a89-168">På den **Cimpl Configuration** klickar du på **konfigurera Cimpl** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b6a89-168">On the **Cimpl Configuration** section, click **Configure Cimpl** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b6a89-169">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b6a89-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_configure.png) 

7. <span data-ttu-id="b6a89-171">Konfigurera enkel inloggning på **Cimpl** sida, måste du skicka den hämtade **certifikat (Base64)**, **SAML enhets-ID och SAML inloggning tjänst-URL för enkel** till Cimpl stöd för på **+1 866-982-8250**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-171">To configure single sign-on on **Cimpl** side, you need to send the downloaded **Certificate (Base64)**, **SAML Entity ID, and SAML Single Sign-On Service URL** to Cimpl support at **+1 866-982-8250**.</span></span>


> [!TIP]
> <span data-ttu-id="b6a89-172">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b6a89-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b6a89-173">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="b6a89-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b6a89-174">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b6a89-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b6a89-175">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b6a89-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="b6a89-176">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b6a89-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b6a89-178">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b6a89-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b6a89-179">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b6a89-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b6a89-181">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b6a89-183">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6a89-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b6a89-185">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b6a89-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cimpl-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b6a89-187">a.</span><span class="sxs-lookup"><span data-stu-id="b6a89-187">a.</span></span> <span data-ttu-id="b6a89-188">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b6a89-189">b.</span><span class="sxs-lookup"><span data-stu-id="b6a89-189">b.</span></span> <span data-ttu-id="b6a89-190">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b6a89-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b6a89-191">c.</span><span class="sxs-lookup"><span data-stu-id="b6a89-191">c.</span></span> <span data-ttu-id="b6a89-192">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b6a89-193">d.</span><span class="sxs-lookup"><span data-stu-id="b6a89-193">d.</span></span> <span data-ttu-id="b6a89-194">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-194">Click **Create**.</span></span>
 
### <a name="creating-a-cimpl-test-user"></a><span data-ttu-id="b6a89-195">Skapa en testanvändare Cimpl</span><span class="sxs-lookup"><span data-stu-id="b6a89-195">Creating a Cimpl test user</span></span>

<span data-ttu-id="b6a89-196">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Cimpl.</span><span class="sxs-lookup"><span data-stu-id="b6a89-196">The objective of this section is to create a user called Britta Simon in Cimpl.</span></span> <span data-ttu-id="b6a89-197">Arbeta med Cimpl support på **+1 866-982-8250** att lägga till användare i Cimpl-konto.</span><span class="sxs-lookup"><span data-stu-id="b6a89-197">Work with Cimpl support at **+1 866-982-8250** to add the users in the Cimpl account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b6a89-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b6a89-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b6a89-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Cimpl.</span><span class="sxs-lookup"><span data-stu-id="b6a89-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cimpl.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b6a89-201">**Om du vill tilldela Cimpl Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b6a89-201">**To assign Britta Simon to Cimpl, perform the following steps:**</span></span>

1. <span data-ttu-id="b6a89-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b6a89-204">Välj i listan med program **Cimpl**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-204">In the applications list, select **Cimpl**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cimpl-tutorial/tutorial_cimpl_app.png) 

3. <span data-ttu-id="b6a89-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b6a89-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b6a89-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b6a89-208">Click **Add** button.</span></span> <span data-ttu-id="b6a89-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6a89-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b6a89-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b6a89-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b6a89-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6a89-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b6a89-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b6a89-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b6a89-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b6a89-214">Testing single sign-on</span></span>

<span data-ttu-id="b6a89-215">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b6a89-215">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  <span data-ttu-id="b6a89-216">När du klickar på panelen Cimpl på åtkomstpanelen du bör få automatiskt loggat in på ditt Cimpl program.</span><span class="sxs-lookup"><span data-stu-id="b6a89-216">When you click the Cimpl tile in the Access Panel, you should get automatically signed-on to your Cimpl application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="b6a89-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b6a89-217">Additional resources</span></span>

* [<span data-ttu-id="b6a89-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b6a89-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b6a89-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b6a89-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cimpl-tutorial/tutorial_general_203.png

