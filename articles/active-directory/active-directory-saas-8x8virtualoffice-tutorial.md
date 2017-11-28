---
title: "Självstudier: Azure Active Directory-integrering med 8 x 8 virtuella Office | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och 8 x 8 Virtual Office."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: d8dcf0171b93fec15347e810a1b525bd815dbf04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a><span data-ttu-id="333a3-103">Självstudier: Azure Active Directory-integrering med 8 x 8 virtuella Office</span><span class="sxs-lookup"><span data-stu-id="333a3-103">Tutorial: Azure Active Directory integration with 8x8 Virtual Office</span></span>

<span data-ttu-id="333a3-104">I kursen får lära du att integrera 8 x 8 virtuella Office med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="333a3-104">In this tutorial, you learn how to integrate 8x8 Virtual Office with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="333a3-105">Integrera 8 x 8 virtuella Office med Azure AD kan du med följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="333a3-105">Integrating 8x8 Virtual Office with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="333a3-106">Du kan styra i Azure AD som har åtkomst till 8 x 8 virtuella Office</span><span class="sxs-lookup"><span data-stu-id="333a3-106">You can control in Azure AD who has access to 8x8 Virtual Office</span></span>
- <span data-ttu-id="333a3-107">Du kan aktivera användarna att automatiskt hämta loggat in på 8 x 8 virtuella Office (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="333a3-107">You can enable your users to automatically get signed-on to 8x8 Virtual Office (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="333a3-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="333a3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="333a3-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="333a3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="333a3-110">Krav</span><span class="sxs-lookup"><span data-stu-id="333a3-110">Prerequisites</span></span>

<span data-ttu-id="333a3-111">Om du vill konfigurera Azure AD-integrering med 8 x 8 virtuella Office behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="333a3-111">To configure Azure AD integration with 8x8 Virtual Office, you need the following items:</span></span>

- <span data-ttu-id="333a3-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="333a3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="333a3-113">En 8 x 8 virtuella Office enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="333a3-113">A 8x8 Virtual Office single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="333a3-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="333a3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="333a3-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="333a3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="333a3-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="333a3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="333a3-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="333a3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="333a3-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="333a3-118">Scenario description</span></span>
<span data-ttu-id="333a3-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="333a3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="333a3-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="333a3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="333a3-121">Lägger till 8 x 8 virtuella Office från galleriet</span><span class="sxs-lookup"><span data-stu-id="333a3-121">Adding 8x8 Virtual Office from the gallery</span></span>
2. <span data-ttu-id="333a3-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="333a3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-8x8-virtual-office-from-the-gallery"></a><span data-ttu-id="333a3-123">Lägger till 8 x 8 virtuella Office från galleriet</span><span class="sxs-lookup"><span data-stu-id="333a3-123">Adding 8x8 Virtual Office from the gallery</span></span>
<span data-ttu-id="333a3-124">Du måste lägga till 8 x 8 virtuella Office från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av 8 x 8 virtuella Office i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="333a3-124">To configure the integration of 8x8 Virtual Office into Azure AD, you need to add 8x8 Virtual Office from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="333a3-125">**Utför följande steg för att lägga till 8 x 8 virtuella Office från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="333a3-125">**To add 8x8 Virtual Office from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="333a3-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="333a3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="333a3-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="333a3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="333a3-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="333a3-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="333a3-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="333a3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="333a3-133">I sökrutan skriver **8 x 8 virtuella Office**.</span><span class="sxs-lookup"><span data-stu-id="333a3-133">In the search box, type **8x8 Virtual Office**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_search.png)

5. <span data-ttu-id="333a3-135">Välj i resultatpanelen **8 x 8 virtuella Office**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="333a3-135">In the results panel, select **8x8 Virtual Office**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="333a3-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="333a3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="333a3-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med 8 x 8 virtuella Office baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="333a3-138">In this section, you configure and test Azure AD single sign-on with 8x8 Virtual Office based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="333a3-139">För enkel inloggning ska fungera, Azure AD som behöver veta vilka motsvarande användaren i 8 x 8 virtuella Office är att en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="333a3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 8x8 Virtual Office is to a user in Azure AD.</span></span> <span data-ttu-id="333a3-140">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i 8 x 8 virtuella Office måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="333a3-140">In other words, a link relationship between an Azure AD user and the related user in 8x8 Virtual Office needs to be established.</span></span>

<span data-ttu-id="333a3-141">8 x 8 virtuella kontoret, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="333a3-141">In 8x8 Virtual Office, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="333a3-142">Om du vill konfigurera och testa Azure AD enkel inloggning med 8 x 8 virtuella Office, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="333a3-142">To configure and test Azure AD single sign-on with 8x8 Virtual Office, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="333a3-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="333a3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="333a3-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="333a3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="333a3-145">**[Skapa en testanvändare för 8 x 8 virtuella Office](#creating-a-8x8-virtual-office-test-user)**  – du har en motsvarighet för Britta Simon i 8 x 8 virtuella Office som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="333a3-145">**[Creating a 8x8 Virtual Office test user](#creating-a-8x8-virtual-office-test-user)** - to have a counterpart of Britta Simon in 8x8 Virtual Office that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="333a3-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="333a3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="333a3-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="333a3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="333a3-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="333a3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="333a3-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt 8 x 8 virtuella Office-program.</span><span class="sxs-lookup"><span data-stu-id="333a3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 8x8 Virtual Office application.</span></span>

<span data-ttu-id="333a3-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med 8 x 8 virtuella Office:**</span><span class="sxs-lookup"><span data-stu-id="333a3-150">**To configure Azure AD single sign-on with 8x8 Virtual Office, perform the following steps:**</span></span>

1. <span data-ttu-id="333a3-151">I Azure-portalen på den **8 x 8 virtuella Office** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="333a3-151">In the Azure portal, on the **8x8 Virtual Office** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="333a3-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="333a3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_samlbase.png)

3. <span data-ttu-id="333a3-155">På den **8 x 8 virtuella Office domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="333a3-155">On the **8x8 Virtual Office Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    <span data-ttu-id="333a3-157">a.</span><span class="sxs-lookup"><span data-stu-id="333a3-157">a.</span></span> <span data-ttu-id="333a3-158">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="333a3-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>

    <span data-ttu-id="333a3-159">| `https://sso.8x8.com/<companyname>` |
    | `https://www.8x8.com/<companyname>` |
    | `https://sso.8x8pilot.com/<companyname>` |</span><span class="sxs-lookup"><span data-stu-id="333a3-159">| `https://sso.8x8.com/<companyname>` |
 | `https://www.8x8.com/<companyname>` |
 | `https://sso.8x8pilot.com/<companyname>` |</span></span>

    <span data-ttu-id="333a3-160">b.</span><span class="sxs-lookup"><span data-stu-id="333a3-160">b.</span></span> <span data-ttu-id="333a3-161">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="333a3-161">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>

    <span data-ttu-id="333a3-162">| `https://<subdomain>.8x8.com/saml2` |
    | `https://<subdomain>.8x8pilot.com/saml2`|</span><span class="sxs-lookup"><span data-stu-id="333a3-162">| `https://<subdomain>.8x8.com/saml2` |
 | `https://<subdomain>.8x8pilot.com/saml2`|</span></span>

    > [!NOTE] 
    > <span data-ttu-id="333a3-163">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="333a3-163">These values are not real.</span></span> <span data-ttu-id="333a3-164">Uppdatera dessa värden med den faktiska identifierare och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="333a3-164">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="333a3-165">Kontakta [8 x 8 virtuella Office supportteamet](https://www.8x8.com/about-us/contact-us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="333a3-165">Contact [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us) to get these values.</span></span>
 


4. <span data-ttu-id="333a3-166">På den **SAML-signeringscertifikat** klickar du på **certifikat (Raw)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="333a3-166">On the **SAML Signing Certificate** section, click **Certificate (Raw)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

5. <span data-ttu-id="333a3-168">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="333a3-168">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="333a3-170">På den **8 x 8 virtuella Office Configuration** klickar du på **konfigurera 8 x 8 virtuella Office** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="333a3-170">On the **8x8 Virtual Office Configuration** section, click **Configure 8x8 Virtual Office** to open **Configure sign-on** window.</span></span> <span data-ttu-id="333a3-171">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="333a3-171">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_configure.png) 

7. <span data-ttu-id="333a3-173">Inloggning till 8 x 8 virtuella Office-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="333a3-173">Sign-on to your 8x8 Virtual Office tenant as an administrator.</span></span>

8. <span data-ttu-id="333a3-174">Välj **virtuella Office konto hanterare** på panelen för programmet.</span><span class="sxs-lookup"><span data-stu-id="333a3-174">Select **Virtual Office Account Mgr** on Application Panel.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. <span data-ttu-id="333a3-176">Välj **företag** konto för att hantera och klickar på **logga In** knappen.</span><span class="sxs-lookup"><span data-stu-id="333a3-176">Select **Business** account to manage and click **Sign In** button.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. <span data-ttu-id="333a3-178">Klicka på **konton** fliken i listan över menyn.</span><span class="sxs-lookup"><span data-stu-id="333a3-178">Click **Accounts** tab in the menu list.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. <span data-ttu-id="333a3-180">Klicka på **enkel inloggning** i listan över konton.</span><span class="sxs-lookup"><span data-stu-id="333a3-180">Click **Single Sign On** in the list of Accounts.</span></span>
   
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. <span data-ttu-id="333a3-182">Välj **enkel inloggning** under autentiseringsmetod och klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="333a3-182">Select **Single Sign On** under Authentication method and click **SAML**.</span></span>
    
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. <span data-ttu-id="333a3-184">Kopiera **SAML SSO URL**, **enkel rör ut Tjänstwebbadress** och **utfärdar-URL** från Azure AD för att **logga In URL: en**, **logga ut URL** och **utfärdar-URL** i 8 x 8 virtuella Office.</span><span class="sxs-lookup"><span data-stu-id="333a3-184">Copy **SAML SSO URL**, **Single Sing Out Service URL** and **Issuer URL** from Azure AD to **Sign In URL**, **Sign Out URL** and **Issuer URL** in 8x8 Virtual Office.</span></span> 
    
    ![Konfigurera på App-sida](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
14. <span data-ttu-id="333a3-186">Klicka på **webbläsare** om du vill ladda upp certifikatet som du hämtade från Azure AD, och klicka sedan på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="333a3-186">Click **Browser** button to upload the certificate which you downloaded from Azure AD, and click the **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="333a3-187">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="333a3-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="333a3-188">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="333a3-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="333a3-189">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="333a3-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="333a3-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="333a3-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="333a3-191">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="333a3-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="333a3-193">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="333a3-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="333a3-194">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="333a3-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="333a3-196">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="333a3-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="333a3-198">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="333a3-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="333a3-200">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="333a3-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="333a3-202">a.</span><span class="sxs-lookup"><span data-stu-id="333a3-202">a.</span></span> <span data-ttu-id="333a3-203">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="333a3-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="333a3-204">b.</span><span class="sxs-lookup"><span data-stu-id="333a3-204">b.</span></span> <span data-ttu-id="333a3-205">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="333a3-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="333a3-206">c.</span><span class="sxs-lookup"><span data-stu-id="333a3-206">c.</span></span> <span data-ttu-id="333a3-207">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="333a3-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="333a3-208">d.</span><span class="sxs-lookup"><span data-stu-id="333a3-208">d.</span></span> <span data-ttu-id="333a3-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="333a3-209">Click **Create**.</span></span>
 
### <a name="creating-a-8x8-virtual-office-test-user"></a><span data-ttu-id="333a3-210">Skapa en 8 x 8 virtuella Office testanvändare</span><span class="sxs-lookup"><span data-stu-id="333a3-210">Creating a 8x8 Virtual Office test user</span></span>

<span data-ttu-id="333a3-211">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i 8 x 8 virtuella Office.</span><span class="sxs-lookup"><span data-stu-id="333a3-211">The objective of this section is to create a user called Britta Simon in 8x8 Virtual Office.</span></span> <span data-ttu-id="333a3-212">8 x 8 virtuella Office stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="333a3-212">8x8 Virtual Office supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="333a3-213">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="333a3-213">There is no action item for you in this section.</span></span> <span data-ttu-id="333a3-214">En ny användare skapas under ett försök att komma åt 8 x 8 virtuella Office om det inte finns.</span><span class="sxs-lookup"><span data-stu-id="333a3-214">A new user is created during an attempt to access 8x8 Virtual Office if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="333a3-215">Om du behöver skapa en användare manuellt, måste du kontakta den [8 x 8 virtuella Office supportteamet](https://www.8x8.com/about-us/contact-us).</span><span class="sxs-lookup"><span data-stu-id="333a3-215">If you need to create a user manually, you need to contact the [8x8 Virtual Office support team](https://www.8x8.com/about-us/contact-us).</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="333a3-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="333a3-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="333a3-217">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till 8 x 8 virtuella Office.</span><span class="sxs-lookup"><span data-stu-id="333a3-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 8x8 Virtual Office.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="333a3-219">**Om du vill tilldela 8 x 8 virtuella Office Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="333a3-219">**To assign Britta Simon to 8x8 Virtual Office, perform the following steps:**</span></span>

1. <span data-ttu-id="333a3-220">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="333a3-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="333a3-222">Välj i listan med program **8 x 8 virtuella Office**.</span><span class="sxs-lookup"><span data-stu-id="333a3-222">In the applications list, select **8x8 Virtual Office**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. <span data-ttu-id="333a3-224">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="333a3-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="333a3-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="333a3-226">Click **Add** button.</span></span> <span data-ttu-id="333a3-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="333a3-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="333a3-229">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="333a3-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="333a3-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="333a3-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="333a3-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="333a3-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="333a3-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="333a3-232">Testing single sign-on</span></span>

<span data-ttu-id="333a3-233">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="333a3-233">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="333a3-234">När du klickar på panelen 8 x 8 virtuella Office på åtkomstpanelen du bör få automatiskt loggat in på ditt 8 x 8 virtuella Office-program.</span><span class="sxs-lookup"><span data-stu-id="333a3-234">When you click the 8x8 Virtual Office tile in the Access Panel, you should get automatically signed-on to your 8x8 Virtual Office application.</span></span>
<span data-ttu-id="333a3-235">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="333a3-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="333a3-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="333a3-236">Additional resources</span></span>

* [<span data-ttu-id="333a3-237">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="333a3-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="333a3-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="333a3-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png

