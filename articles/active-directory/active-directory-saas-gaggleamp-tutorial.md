---
title: "Självstudier: Azure Active Directory-integrering med GaggleAMP | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och GaggleAMP."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: c591cf99aecc4153e378c42a530b80deeca63158
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a><span data-ttu-id="3f044-103">Självstudier: Azure Active Directory-integrering med GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="3f044-103">Tutorial: Azure Active Directory integration with GaggleAMP</span></span>

<span data-ttu-id="3f044-104">I kursen får lära du att integrera GaggleAMP med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3f044-104">In this tutorial, you learn how to integrate GaggleAMP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f044-105">Integrera GaggleAMP med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3f044-105">Integrating GaggleAMP with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f044-106">Du kan styra i Azure AD som har åtkomst till GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="3f044-106">You can control in Azure AD who has access to GaggleAMP</span></span>
- <span data-ttu-id="3f044-107">Du kan aktivera användarna att automatiskt hämta loggat in på GaggleAMP (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3f044-107">You can enable your users to automatically get signed-on to GaggleAMP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f044-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3f044-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f044-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f044-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f044-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3f044-110">Prerequisites</span></span>

<span data-ttu-id="3f044-111">För att konfigurera Azure AD-integrering med GaggleAMP, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3f044-111">To configure Azure AD integration with GaggleAMP, you need the following items:</span></span>

- <span data-ttu-id="3f044-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f044-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f044-113">En GaggleAMP enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f044-113">A GaggleAMP single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f044-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f044-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f044-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3f044-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f044-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3f044-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f044-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f044-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f044-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3f044-118">Scenario description</span></span>
<span data-ttu-id="3f044-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f044-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f044-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f044-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f044-121">Att lägga till GaggleAMP från galleriet</span><span class="sxs-lookup"><span data-stu-id="3f044-121">Adding GaggleAMP from the gallery</span></span>
2. <span data-ttu-id="3f044-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f044-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-gaggleamp-from-the-gallery"></a><span data-ttu-id="3f044-123">Att lägga till GaggleAMP från galleriet</span><span class="sxs-lookup"><span data-stu-id="3f044-123">Adding GaggleAMP from the gallery</span></span>
<span data-ttu-id="3f044-124">Du måste lägga till GaggleAMP från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av GaggleAMP i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f044-124">To configure the integration of GaggleAMP into Azure AD, you need to add GaggleAMP from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f044-125">**Utför följande steg för att lägga till GaggleAMP från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3f044-125">**To add GaggleAMP from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f044-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3f044-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f044-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3f044-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f044-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f044-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3f044-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f044-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3f044-133">I sökrutan skriver **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="3f044-133">In the search box, type **GaggleAMP**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_search.png)

5. <span data-ttu-id="3f044-135">Välj i resultatpanelen **GaggleAMP**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3f044-135">In the results panel, select **GaggleAMP**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f044-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f044-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f044-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med GaggleAMP baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3f044-138">In this section, you configure and test Azure AD single sign-on with GaggleAMP based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3f044-139">Azure AD måste du känna till användaren i GaggleAMP motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3f044-139">For single sign-on to work, Azure AD needs to know what the counterpart user in GaggleAMP is to a user in Azure AD.</span></span> <span data-ttu-id="3f044-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i GaggleAMP upprättas.</span><span class="sxs-lookup"><span data-stu-id="3f044-140">In other words, a link relationship between an Azure AD user and the related user in GaggleAMP needs to be established.</span></span>

<span data-ttu-id="3f044-141">I GaggleAMP, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3f044-141">In GaggleAMP, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3f044-142">Om du vill konfigurera och testa Azure AD enkel inloggning med GaggleAMP, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f044-142">To configure and test Azure AD single sign-on with GaggleAMP, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f044-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3f044-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f044-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f044-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f044-145">**[Skapa en testanvändare GaggleAMP](#creating-a-gaggleamp-test-user)**  – du har en motsvarighet för Britta Simon i GaggleAMP som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3f044-145">**[Creating a GaggleAMP test user](#creating-a-gaggleamp-test-user)** - to have a counterpart of Britta Simon in GaggleAMP that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f044-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f044-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f044-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3f044-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f044-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f044-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f044-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt GaggleAMP program.</span><span class="sxs-lookup"><span data-stu-id="3f044-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your GaggleAMP application.</span></span>

<span data-ttu-id="3f044-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med GaggleAMP:**</span><span class="sxs-lookup"><span data-stu-id="3f044-150">**To configure Azure AD single sign-on with GaggleAMP, perform the following steps:**</span></span>

1. <span data-ttu-id="3f044-151">I Azure-portalen på den **GaggleAMP** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3f044-151">In the Azure portal, on the **GaggleAMP** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3f044-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f044-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_samlbase.png)

3. <span data-ttu-id="3f044-155">På den **GaggleAMP domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f044-155">On the **GaggleAMP Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_url.png)

     <span data-ttu-id="3f044-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.gaggleamp.com`</span><span class="sxs-lookup"><span data-stu-id="3f044-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.gaggleamp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f044-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3f044-158">The value is not real.</span></span> <span data-ttu-id="3f044-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="3f044-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="3f044-160">Kontakta [GaggleAMP klienten supportteamet](mailto:sales@gaggleamp.com) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="3f044-160">Contact [GaggleAMP Client support team](mailto:sales@gaggleamp.com) to get the value.</span></span> 
 
4. <span data-ttu-id="3f044-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3f044-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_certificate.png) 

5. <span data-ttu-id="3f044-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f044-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f044-165">På den **GaggleAMP Configuration** klickar du på **konfigurera GaggleAMP** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3f044-165">On the **GaggleAMP Configuration** section, click **Configure GaggleAMP** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3f044-166">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3f044-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_configure.png) 

7. <span data-ttu-id="3f044-168">I en annan webbläsare instans, gå till sidan SAML SSO som skapats för dig av Gaggle supportteam (till exempel: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span><span class="sxs-lookup"><span data-stu-id="3f044-168">In another browser instance, navigate to the SAML SSO page created for you by the Gaggle support team (for example: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).</span></span>

8. <span data-ttu-id="3f044-169">På din **SAML SSO** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f044-169">On your **SAML SSO** page, perform the following steps:</span></span>  
   
    ![GaggleAMP enkel inloggning](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png) 
 
    <span data-ttu-id="3f044-171">a.</span><span class="sxs-lookup"><span data-stu-id="3f044-171">a.</span></span> <span data-ttu-id="3f044-172">I den **identitet providern utfärdaren** textruta klistra in värdet för **utfärdar-URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f044-172">In the **Identity Provider Issuer** textbox, paste the value of **Issuer URL** which you have copied from Azure portal.</span></span> 
 
    <span data-ttu-id="3f044-173">b.</span><span class="sxs-lookup"><span data-stu-id="3f044-173">b.</span></span> <span data-ttu-id="3f044-174">I den **identitet providern enkel inloggnings-URL** textruta klistra in värdet för **inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f044-174">In the **Identity Provider Single Sign-On URL** textbox, paste the  value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="3f044-175">c.</span><span class="sxs-lookup"><span data-stu-id="3f044-175">c.</span></span> <span data-ttu-id="3f044-176">Klicka på **spara**</span><span class="sxs-lookup"><span data-stu-id="3f044-176">Click **Save**</span></span>      

    <span data-ttu-id="3f044-177">d.</span><span class="sxs-lookup"><span data-stu-id="3f044-177">d.</span></span> <span data-ttu-id="3f044-178">Skicka den **certifikat (Base64)** certifikat till din [GaggleAMP supportteamet](mailto:sales@gaggleamp.com).</span><span class="sxs-lookup"><span data-stu-id="3f044-178">Send the **Certificate (Base64)** certificate to your [GaggleAMP support team](mailto:sales@gaggleamp.com).</span></span>

> [!TIP]
> <span data-ttu-id="3f044-179">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3f044-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f044-180">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3f044-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f044-181">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f044-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f044-182">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f044-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f044-183">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f044-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3f044-185">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3f044-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f044-186">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3f044-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f044-188">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3f044-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f044-190">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f044-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f044-192">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f044-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f044-194">a.</span><span class="sxs-lookup"><span data-stu-id="3f044-194">a.</span></span> <span data-ttu-id="3f044-195">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f044-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f044-196">b.</span><span class="sxs-lookup"><span data-stu-id="3f044-196">b.</span></span> <span data-ttu-id="3f044-197">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f044-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f044-198">c.</span><span class="sxs-lookup"><span data-stu-id="3f044-198">c.</span></span> <span data-ttu-id="3f044-199">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3f044-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f044-200">d.</span><span class="sxs-lookup"><span data-stu-id="3f044-200">d.</span></span> <span data-ttu-id="3f044-201">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3f044-201">Click **Create**.</span></span>
 
### <a name="creating-a-gaggleamp-test-user"></a><span data-ttu-id="3f044-202">Skapa en testanvändare GaggleAMP</span><span class="sxs-lookup"><span data-stu-id="3f044-202">Creating a GaggleAMP test user</span></span>

<span data-ttu-id="3f044-203">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="3f044-203">The objective of this section is to create a user called Britta Simon in GaggleAMP.</span></span> <span data-ttu-id="3f044-204">GaggleAMP stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="3f044-204">GaggleAMP supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="3f044-205">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3f044-205">There is no action item for you in this section.</span></span> <span data-ttu-id="3f044-206">En ny användare skapas under ett försök att komma åt GaggleAMP om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="3f044-206">A new user is created during an attempt to access GaggleAMP if it doesn't exist yet.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f044-207">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3f044-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f044-208">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till GaggleAMP.</span><span class="sxs-lookup"><span data-stu-id="3f044-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to GaggleAMP.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3f044-210">**Om du vill tilldela GaggleAMP Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f044-210">**To assign Britta Simon to GaggleAMP, perform the following steps:**</span></span>

1. <span data-ttu-id="3f044-211">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f044-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3f044-213">Välj i listan med program **GaggleAMP**.</span><span class="sxs-lookup"><span data-stu-id="3f044-213">In the applications list, select **GaggleAMP**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_app.png) 

3. <span data-ttu-id="3f044-215">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3f044-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3f044-217">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f044-217">Click **Add** button.</span></span> <span data-ttu-id="3f044-218">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f044-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3f044-220">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3f044-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f044-221">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f044-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f044-222">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f044-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f044-223">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f044-223">Testing single sign-on</span></span>

<span data-ttu-id="3f044-224">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3f044-224">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="3f044-225">När du klickar på panelen GaggleAMP på åtkomstpanelen du bör få automatiskt loggat in på ditt GaggleAMP program.</span><span class="sxs-lookup"><span data-stu-id="3f044-225">When you click the GaggleAMP tile in the Access Panel, you should get automatically signed-on to your GaggleAMP application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f044-226">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3f044-226">Additional resources</span></span>

* [<span data-ttu-id="3f044-227">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3f044-227">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f044-228">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3f044-228">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png

