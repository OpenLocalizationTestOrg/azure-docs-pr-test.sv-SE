---
title: "Självstudier: Azure Active Directory-integrering med HireVue | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och HireVue."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: aadfc342-14db-4d74-a83d-f0c76f0cf63c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 682d88d3010f5781948a9adde0e1351471608a5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hirevue"></a><span data-ttu-id="75a0e-103">Självstudier: Azure Active Directory-integrering med HireVue</span><span class="sxs-lookup"><span data-stu-id="75a0e-103">Tutorial: Azure Active Directory integration with HireVue</span></span>

<span data-ttu-id="75a0e-104">I kursen får lära du att integrera HireVue med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="75a0e-104">In this tutorial, you learn how to integrate HireVue with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="75a0e-105">Integrera HireVue med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="75a0e-105">Integrating HireVue with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="75a0e-106">Du kan styra i Azure AD som har åtkomst till HireVue</span><span class="sxs-lookup"><span data-stu-id="75a0e-106">You can control in Azure AD who has access to HireVue</span></span>
- <span data-ttu-id="75a0e-107">Du kan aktivera användarna att automatiskt hämta loggat in på HireVue (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="75a0e-107">You can enable your users to automatically get signed-on to HireVue (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="75a0e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="75a0e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="75a0e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="75a0e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75a0e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="75a0e-110">Prerequisites</span></span>

<span data-ttu-id="75a0e-111">För att konfigurera Azure AD-integrering med HireVue, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="75a0e-111">To configure Azure AD integration with HireVue, you need the following items:</span></span>

- <span data-ttu-id="75a0e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="75a0e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="75a0e-113">En HireVue enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="75a0e-113">A HireVue single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="75a0e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="75a0e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="75a0e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="75a0e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="75a0e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="75a0e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="75a0e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="75a0e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="75a0e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="75a0e-118">Scenario description</span></span>
<span data-ttu-id="75a0e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="75a0e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="75a0e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="75a0e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="75a0e-121">Att lägga till HireVue från galleriet</span><span class="sxs-lookup"><span data-stu-id="75a0e-121">Adding HireVue from the gallery</span></span>
2. <span data-ttu-id="75a0e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75a0e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-hirevue-from-the-gallery"></a><span data-ttu-id="75a0e-123">Att lägga till HireVue från galleriet</span><span class="sxs-lookup"><span data-stu-id="75a0e-123">Adding HireVue from the gallery</span></span>
<span data-ttu-id="75a0e-124">Du måste lägga till HireVue från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av HireVue i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="75a0e-124">To configure the integration of HireVue into Azure AD, you need to add HireVue from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="75a0e-125">**Utför följande steg för att lägga till HireVue från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="75a0e-125">**To add HireVue from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="75a0e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75a0e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="75a0e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="75a0e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="75a0e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75a0e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="75a0e-133">I sökrutan skriver **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-133">In the search box, type **HireVue**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_search.png)

5. <span data-ttu-id="75a0e-135">Välj i resultatpanelen **HireVue**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="75a0e-135">In the results panel, select **HireVue**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="75a0e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75a0e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="75a0e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med HireVue baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="75a0e-138">In this section, you configure and test Azure AD single sign-on with HireVue based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="75a0e-139">Azure AD måste du känna till användaren i HireVue motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="75a0e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HireVue is to a user in Azure AD.</span></span> <span data-ttu-id="75a0e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i HireVue upprättas.</span><span class="sxs-lookup"><span data-stu-id="75a0e-140">In other words, a link relationship between an Azure AD user and the related user in HireVue needs to be established.</span></span>

<span data-ttu-id="75a0e-141">I HireVue, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="75a0e-141">In HireVue, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="75a0e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med HireVue, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="75a0e-142">To configure and test Azure AD single sign-on with HireVue, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="75a0e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="75a0e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="75a0e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75a0e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="75a0e-145">**[Skapa en testanvändare HireVue](#creating-a-hirevue-test-user)**  – du har en motsvarighet för Britta Simon i HireVue som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="75a0e-145">**[Creating a HireVue test user](#creating-a-hirevue-test-user)** - to have a counterpart of Britta Simon in HireVue that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="75a0e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75a0e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="75a0e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="75a0e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="75a0e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75a0e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="75a0e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt HireVue program.</span><span class="sxs-lookup"><span data-stu-id="75a0e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HireVue application.</span></span>

<span data-ttu-id="75a0e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med HireVue:**</span><span class="sxs-lookup"><span data-stu-id="75a0e-150">**To configure Azure AD single sign-on with HireVue, perform the following steps:**</span></span>

1. <span data-ttu-id="75a0e-151">I Azure-portalen på den **HireVue** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-151">In the Azure portal, on the **HireVue** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="75a0e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="75a0e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_samlbase.png)

3. <span data-ttu-id="75a0e-155">På den **HireVue domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="75a0e-155">On the **HireVue Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_url.png)

    <span data-ttu-id="75a0e-157">a.</span><span class="sxs-lookup"><span data-stu-id="75a0e-157">a.</span></span> <span data-ttu-id="75a0e-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="75a0e-158">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>

    | <span data-ttu-id="75a0e-159">Miljö</span><span class="sxs-lookup"><span data-stu-id="75a0e-159">Environment</span></span> | <span data-ttu-id="75a0e-160">URL: EN</span><span class="sxs-lookup"><span data-stu-id="75a0e-160">URL</span></span> |
    |-------------|---|
    | <span data-ttu-id="75a0e-161">Produktion</span><span class="sxs-lookup"><span data-stu-id="75a0e-161">Production</span></span> | `https://<companyname>.hirevue.com` |
    | <span data-ttu-id="75a0e-162">Mellanlagring</span><span class="sxs-lookup"><span data-stu-id="75a0e-162">Staging</span></span>    | `https://<companyname>.stghv.com` |
    
    <span data-ttu-id="75a0e-163">b.</span><span class="sxs-lookup"><span data-stu-id="75a0e-163">b.</span></span> <span data-ttu-id="75a0e-164">I den **identifierare** textruta Skriv en URL som:</span><span class="sxs-lookup"><span data-stu-id="75a0e-164">In the **Identifier** textbox, type a URL as:</span></span>
    
    | <span data-ttu-id="75a0e-165">Miljö</span><span class="sxs-lookup"><span data-stu-id="75a0e-165">Environment</span></span> | <span data-ttu-id="75a0e-166">URN</span><span class="sxs-lookup"><span data-stu-id="75a0e-166">URN</span></span> |
    |-------------|-----|
    | <span data-ttu-id="75a0e-167">Produktion</span><span class="sxs-lookup"><span data-stu-id="75a0e-167">Production</span></span> |`urn:federation:hirevue.com:saml:sp:prod` |
    | <span data-ttu-id="75a0e-168">Mellanlagring</span><span class="sxs-lookup"><span data-stu-id="75a0e-168">Staging</span></span>    | `urn:federation:hirevue.com:saml:sp:staging`|
    
    > [!NOTE] 
    > <span data-ttu-id="75a0e-169">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="75a0e-169">These values are not real.</span></span> <span data-ttu-id="75a0e-170">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="75a0e-170">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="75a0e-171">Kontakta [HireVue klienten supportteamet](mailto:samlsupport@hirevue.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="75a0e-171">Contact [HireVue Client support team](mailto:samlsupport@hirevue.com) to get these values.</span></span> 
 
4. <span data-ttu-id="75a0e-172">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="75a0e-172">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_certificate.png) 

5. <span data-ttu-id="75a0e-174">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="75a0e-174">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hirevue-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="75a0e-176">På den **HireVue Configuration** klickar du på **konfigurera HireVue** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="75a0e-176">On the **HireVue Configuration** section, click **Configure HireVue** to open **Configure sign-on** window.</span></span> <span data-ttu-id="75a0e-177">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="75a0e-177">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_configure.png) 

7. <span data-ttu-id="75a0e-179">Konfigurera enkel inloggning på **HireVue** sida, måste du skicka den hämtade **Certificate(Base64)** och **Sign-Out URL, SAML enhets-ID och SAML inloggning tjänst-URL för enkel**till [HireVue supportteamet](mailto:samlsupport@hirevue.com).</span><span class="sxs-lookup"><span data-stu-id="75a0e-179">To configure single sign-on on **HireVue** side, you need to send the downloaded **Certificate(Base64)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [HireVue support team](mailto:samlsupport@hirevue.com).</span></span> <span data-ttu-id="75a0e-180">De kan ange den här inställningen att ha SAML SSO anslutningen korrekt på båda sidor.</span><span class="sxs-lookup"><span data-stu-id="75a0e-180">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="75a0e-181">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="75a0e-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="75a0e-182">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="75a0e-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="75a0e-183">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="75a0e-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="75a0e-184">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="75a0e-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="75a0e-185">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="75a0e-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="75a0e-187">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="75a0e-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="75a0e-188">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="75a0e-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="75a0e-190">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="75a0e-192">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75a0e-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="75a0e-194">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="75a0e-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-hirevue-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="75a0e-196">a.</span><span class="sxs-lookup"><span data-stu-id="75a0e-196">a.</span></span> <span data-ttu-id="75a0e-197">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="75a0e-198">b.</span><span class="sxs-lookup"><span data-stu-id="75a0e-198">b.</span></span> <span data-ttu-id="75a0e-199">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="75a0e-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="75a0e-200">c.</span><span class="sxs-lookup"><span data-stu-id="75a0e-200">c.</span></span> <span data-ttu-id="75a0e-201">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="75a0e-202">d.</span><span class="sxs-lookup"><span data-stu-id="75a0e-202">d.</span></span> <span data-ttu-id="75a0e-203">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-203">Click **Create**.</span></span>
 
### <a name="creating-a-hirevue-test-user"></a><span data-ttu-id="75a0e-204">Skapa en testanvändare HireVue</span><span class="sxs-lookup"><span data-stu-id="75a0e-204">Creating a HireVue test user</span></span>

<span data-ttu-id="75a0e-205">I det här avsnittet skapar du en användare som kallas Britta Simon i HireVue.</span><span class="sxs-lookup"><span data-stu-id="75a0e-205">In this section, you create a user called Britta Simon in HireVue.</span></span> <span data-ttu-id="75a0e-206">Se tillsammans med [HireVue supportteamet](mailto:samlsupport@hirevue.com) att lägga till användare i HireVue-plattformen.</span><span class="sxs-lookup"><span data-stu-id="75a0e-206">Please work with [HireVue support team](mailto:samlsupport@hirevue.com) to add the users in the HireVue platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="75a0e-207">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="75a0e-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="75a0e-208">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till HireVue.</span><span class="sxs-lookup"><span data-stu-id="75a0e-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HireVue.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="75a0e-210">**Om du vill tilldela HireVue Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="75a0e-210">**To assign Britta Simon to HireVue, perform the following steps:**</span></span>

1. <span data-ttu-id="75a0e-211">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="75a0e-213">Välj i listan med program **HireVue**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-213">In the applications list, select **HireVue**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-hirevue-tutorial/tutorial_hirevue_app.png) 

3. <span data-ttu-id="75a0e-215">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="75a0e-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="75a0e-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="75a0e-217">Click **Add** button.</span></span> <span data-ttu-id="75a0e-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75a0e-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="75a0e-220">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="75a0e-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="75a0e-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75a0e-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="75a0e-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="75a0e-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="75a0e-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="75a0e-223">Testing single sign-on</span></span>

<span data-ttu-id="75a0e-224">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="75a0e-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="75a0e-225">När du klickar på panelen HireVue på åtkomstpanelen du bör få automatiskt loggat in på ditt HireVue program.</span><span class="sxs-lookup"><span data-stu-id="75a0e-225">When you click the HireVue tile in the Access Panel, you should get automatically signed-on to your HireVue application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75a0e-226">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="75a0e-226">Additional resources</span></span>

* [<span data-ttu-id="75a0e-227">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="75a0e-227">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="75a0e-228">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="75a0e-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hirevue-tutorial/tutorial_general_203.png

