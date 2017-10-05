---
title: "Självstudier: Azure Active Directory-integrering med Cherwell | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Cherwell."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ad891f99-179e-4487-834d-35f3bc01c1ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: jeedes
ms.openlocfilehash: 27fedb9caa1ef27693b2267df285d62aab78bc24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cherwell"></a><span data-ttu-id="4ce8e-103">Självstudier: Azure Active Directory-integrering med Cherwell</span><span class="sxs-lookup"><span data-stu-id="4ce8e-103">Tutorial: Azure Active Directory integration with Cherwell</span></span>

<span data-ttu-id="4ce8e-104">I kursen får lära du att integrera Cherwell med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4ce8e-104">In this tutorial, you learn how to integrate Cherwell with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4ce8e-105">Integrera Cherwell med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4ce8e-105">Integrating Cherwell with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="4ce8e-106">Du kan styra i Azure AD som har åtkomst till Cherwell</span><span class="sxs-lookup"><span data-stu-id="4ce8e-106">You can control in Azure AD who has access to Cherwell</span></span>
- <span data-ttu-id="4ce8e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Cherwell (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4ce8e-107">You can enable your users to automatically get signed-on to Cherwell (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4ce8e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4ce8e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="4ce8e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4ce8e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ce8e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4ce8e-110">Prerequisites</span></span>

<span data-ttu-id="4ce8e-111">För att konfigurera Azure AD-integrering med Cherwell, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="4ce8e-111">To configure Azure AD integration with Cherwell, you need the following items:</span></span>

- <span data-ttu-id="4ce8e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4ce8e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4ce8e-113">En Cherwell enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="4ce8e-113">A Cherwell single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4ce8e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4ce8e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4ce8e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4ce8e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4ce8e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4ce8e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4ce8e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4ce8e-118">Scenario description</span></span>
<span data-ttu-id="4ce8e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4ce8e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4ce8e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4ce8e-121">Att lägga till Cherwell från galleriet</span><span class="sxs-lookup"><span data-stu-id="4ce8e-121">Adding Cherwell from the gallery</span></span>
2. <span data-ttu-id="4ce8e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ce8e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cherwell-from-the-gallery"></a><span data-ttu-id="4ce8e-123">Att lägga till Cherwell från galleriet</span><span class="sxs-lookup"><span data-stu-id="4ce8e-123">Adding Cherwell from the gallery</span></span>
<span data-ttu-id="4ce8e-124">Du måste lägga till Cherwell från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Cherwell i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-124">To configure the integration of Cherwell into Azure AD, you need to add Cherwell from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="4ce8e-125">**Utför följande steg för att lägga till Cherwell från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="4ce8e-125">**To add Cherwell from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="4ce8e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4ce8e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="4ce8e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4ce8e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4ce8e-133">I sökrutan skriver **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-133">In the search box, type **Cherwell**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_search.png)

5. <span data-ttu-id="4ce8e-135">Välj i resultatpanelen **Cherwell**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-135">In the results panel, select **Cherwell**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4ce8e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ce8e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4ce8e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Cherwell baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-138">In this section, you configure and test Azure AD single sign-on with Cherwell based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="4ce8e-139">Azure AD måste du känna till användaren i Cherwell motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cherwell is to a user in Azure AD.</span></span> <span data-ttu-id="4ce8e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Cherwell upprättas.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-140">In other words, a link relationship between an Azure AD user and the related user in Cherwell needs to be established.</span></span>

<span data-ttu-id="4ce8e-141">I Cherwell, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-141">In Cherwell, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="4ce8e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Cherwell, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4ce8e-142">To configure and test Azure AD single sign-on with Cherwell, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="4ce8e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="4ce8e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4ce8e-145">**[Skapa en testanvändare Cherwell](#creating-a-cherwell-test-user)**  – du har en motsvarighet för Britta Simon i Cherwell som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-145">**[Creating a Cherwell test user](#creating-a-cherwell-test-user)** - to have a counterpart of Britta Simon in Cherwell that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="4ce8e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4ce8e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4ce8e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ce8e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4ce8e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Cherwell program.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cherwell application.</span></span>

<span data-ttu-id="4ce8e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Cherwell:**</span><span class="sxs-lookup"><span data-stu-id="4ce8e-150">**To configure Azure AD single sign-on with Cherwell, perform the following steps:**</span></span>

1. <span data-ttu-id="4ce8e-151">I Azure-portalen på den **Cherwell** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-151">In the Azure portal, on the **Cherwell** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4ce8e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_samlbase.png)

3. <span data-ttu-id="4ce8e-155">På den **Cherwell domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ce8e-155">On the **Cherwell Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_url.png)

    <span data-ttu-id="4ce8e-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.cherwellondemand.com/cherwellclient`</span><span class="sxs-lookup"><span data-stu-id="4ce8e-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.cherwellondemand.com/cherwellclient`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4ce8e-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-158">This value is not real.</span></span> <span data-ttu-id="4ce8e-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-159">Update this value with the actual Sign-on URL.</span></span> <span data-ttu-id="4ce8e-160">Kontakta [Cherwell supportteamet](https://csm.cherwell.com/contact) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-160">Contact [Cherwell support team](https://csm.cherwell.com/contact) to get this value.</span></span>
 
4. <span data-ttu-id="4ce8e-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_certificate.png) 

5. <span data-ttu-id="4ce8e-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cherwell-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4ce8e-165">På den **Cherwell Configuration** klickar du på **konfigurera Cherwell** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-165">On the **Cherwell Configuration** section, click **Configure Cherwell** to open **Configure sign-on** window.</span></span> <span data-ttu-id="4ce8e-166">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4ce8e-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_configure.png) 

7. <span data-ttu-id="4ce8e-168">Konfigurera enkel inloggning på **Cherwell** sida, måste du skicka den hämtade **certifikat (Base64)**, **SAML enkel inloggning Tjänstwebbadress**, och **SAML enhets-ID** till [Cherwell supportteamet](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="4ce8e-168">To configure single sign-on on **Cherwell** side, you need to send the downloaded **Certificate (Base64)**, **SAML Single Sign-On Service URL**, and **SAML Entity ID** to [Cherwell support team](https://csm.cherwell.com/contact).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="4ce8e-169">Supportteamet Cherwell har att göra den faktiska SSO-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-169">Your Cherwell support team has to do the actual SSO configuration.</span></span> <span data-ttu-id="4ce8e-170">Du får ett meddelande när enkel inloggning har aktiverats för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-170">You will get a notification when SSO has been enabled for your subscription.</span></span>
    > 
    
> [!TIP]
> <span data-ttu-id="4ce8e-171">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="4ce8e-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="4ce8e-172">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="4ce8e-173">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4ce8e-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4ce8e-174">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4ce8e-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="4ce8e-175">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4ce8e-177">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="4ce8e-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="4ce8e-178">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4ce8e-180">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4ce8e-182">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4ce8e-184">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="4ce8e-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-cherwell-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4ce8e-186">a.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-186">a.</span></span> <span data-ttu-id="4ce8e-187">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4ce8e-188">b.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-188">b.</span></span> <span data-ttu-id="4ce8e-189">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4ce8e-190">c.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-190">c.</span></span> <span data-ttu-id="4ce8e-191">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="4ce8e-192">d.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-192">d.</span></span> <span data-ttu-id="4ce8e-193">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-193">Click **Create**.</span></span>
 
### <a name="creating-a-cherwell-test-user"></a><span data-ttu-id="4ce8e-194">Skapa en testanvändare Cherwell</span><span class="sxs-lookup"><span data-stu-id="4ce8e-194">Creating a Cherwell test user</span></span>

<span data-ttu-id="4ce8e-195">Om du vill aktivera Azure AD-användare kan logga in på Cherwell etableras de i Cherwell.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-195">To enable Azure AD users to log in to Cherwell, they must be provisioned into Cherwell.</span></span>

<span data-ttu-id="4ce8e-196">När det gäller Cherwell, behöver användarkonton skapas av din [Cherwell supportteamet](https://csm.cherwell.com/contact).</span><span class="sxs-lookup"><span data-stu-id="4ce8e-196">In the case of Cherwell, the user accounts need to be created by your [Cherwell support team](https://csm.cherwell.com/contact).</span></span>

>[!NOTE]
><span data-ttu-id="4ce8e-197">Du kan använda något annat Cherwell användarens konto skapas verktyg eller API: er som tillhandahålls av Cherwell för att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-197">You can use any other Cherwell user account creation tools or APIs provided by Cherwell to provision Azure Active Directory user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="4ce8e-198">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="4ce8e-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="4ce8e-199">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Cherwell.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cherwell.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4ce8e-201">**Om du vill tilldela Cherwell Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4ce8e-201">**To assign Britta Simon to Cherwell, perform the following steps:**</span></span>

1. <span data-ttu-id="4ce8e-202">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4ce8e-204">Välj i listan med program **Cherwell**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-204">In the applications list, select **Cherwell**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-cherwell-tutorial/tutorial_cherwell_app.png) 

3. <span data-ttu-id="4ce8e-206">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4ce8e-208">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-208">Click **Add** button.</span></span> <span data-ttu-id="4ce8e-209">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4ce8e-211">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="4ce8e-212">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4ce8e-213">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4ce8e-214">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4ce8e-214">Testing single sign-on</span></span>

<span data-ttu-id="4ce8e-215">Om du vill testa dina inställningar för enkel inloggning, öppna åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4ce8e-215">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="4ce8e-216">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4ce8e-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4ce8e-217">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4ce8e-217">Additional resources</span></span>

* [<span data-ttu-id="4ce8e-218">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4ce8e-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4ce8e-219">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4ce8e-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cherwell-tutorial/tutorial_general_203.png

