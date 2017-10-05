---
title: "Självstudier: Azure Active Directory-integrering med ThousandEyes | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ThousandEyes."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.openlocfilehash: 392add7d5f0a55598b8b90760f5c3f2d1e67ac02
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a><span data-ttu-id="43934-103">Självstudier: Azure Active Directory-integrering med ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="43934-103">Tutorial: Azure Active Directory integration with ThousandEyes</span></span>

<span data-ttu-id="43934-104">I kursen får lära du att integrera ThousandEyes med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="43934-104">In this tutorial, you learn how to integrate ThousandEyes with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="43934-105">Integrera ThousandEyes med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="43934-105">Integrating ThousandEyes with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="43934-106">Du kan styra i Azure AD som har åtkomst till ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="43934-106">You can control in Azure AD who has access to ThousandEyes</span></span>
- <span data-ttu-id="43934-107">Du kan aktivera användarna att automatiskt hämta loggat in på ThousandEyes (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="43934-107">You can enable your users to automatically get signed-on to ThousandEyes (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="43934-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="43934-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="43934-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43934-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43934-110">Krav</span><span class="sxs-lookup"><span data-stu-id="43934-110">Prerequisites</span></span>

<span data-ttu-id="43934-111">För att konfigurera Azure AD-integrering med ThousandEyes, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="43934-111">To configure Azure AD integration with ThousandEyes, you need the following items:</span></span>

- <span data-ttu-id="43934-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="43934-112">An Azure AD subscription</span></span>
- <span data-ttu-id="43934-113">En ThousandEyes enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="43934-113">A ThousandEyes single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="43934-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="43934-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="43934-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="43934-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="43934-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="43934-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="43934-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43934-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="43934-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="43934-118">Scenario description</span></span>
<span data-ttu-id="43934-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="43934-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="43934-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="43934-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="43934-121">Att lägga till ThousandEyes från galleriet</span><span class="sxs-lookup"><span data-stu-id="43934-121">Adding ThousandEyes from the gallery</span></span>
2. <span data-ttu-id="43934-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43934-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-thousandeyes-from-the-gallery"></a><span data-ttu-id="43934-123">Att lägga till ThousandEyes från galleriet</span><span class="sxs-lookup"><span data-stu-id="43934-123">Adding ThousandEyes from the gallery</span></span>
<span data-ttu-id="43934-124">Du måste lägga till ThousandEyes från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ThousandEyes i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43934-124">To configure the integration of ThousandEyes into Azure AD, you need to add ThousandEyes from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="43934-125">**Utför följande steg för att lägga till ThousandEyes från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="43934-125">**To add ThousandEyes from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="43934-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43934-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="43934-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="43934-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="43934-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="43934-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="43934-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43934-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="43934-133">I sökrutan skriver **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="43934-133">In the search box, type **ThousandEyes**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. <span data-ttu-id="43934-135">Välj i resultatpanelen **ThousandEyes**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="43934-135">In the results panel, select **ThousandEyes**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="43934-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43934-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="43934-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ThousandEyes baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="43934-138">In this section, you configure and test Azure AD single sign-on with ThousandEyes based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="43934-139">Azure AD måste du känna till användaren i ThousandEyes motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="43934-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ThousandEyes is to a user in Azure AD.</span></span> <span data-ttu-id="43934-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ThousandEyes upprättas.</span><span class="sxs-lookup"><span data-stu-id="43934-140">In other words, a link relationship between an Azure AD user and the related user in ThousandEyes needs to be established.</span></span>

<span data-ttu-id="43934-141">I ThousandEyes, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="43934-141">In ThousandEyes, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="43934-142">Om du vill konfigurera och testa Azure AD enkel inloggning med ThousandEyes, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="43934-142">To configure and test Azure AD single sign-on with ThousandEyes, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="43934-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="43934-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="43934-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43934-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="43934-145">**[Skapa en testanvändare ThousandEyes](#creating-a-thousandeyes-test-user)**  – du har en motsvarighet för Britta Simon i ThousandEyes som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="43934-145">**[Creating a ThousandEyes test user](#creating-a-thousandeyes-test-user)** - to have a counterpart of Britta Simon in ThousandEyes that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="43934-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43934-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="43934-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="43934-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="43934-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43934-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="43934-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt ThousandEyes program.</span><span class="sxs-lookup"><span data-stu-id="43934-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ThousandEyes application.</span></span>

<span data-ttu-id="43934-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ThousandEyes:**</span><span class="sxs-lookup"><span data-stu-id="43934-150">**To configure Azure AD single sign-on with ThousandEyes, perform the following steps:**</span></span>

1. <span data-ttu-id="43934-151">I Azure-portalen på den **ThousandEyes** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="43934-151">In the Azure portal, on the **ThousandEyes** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="43934-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43934-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. <span data-ttu-id="43934-155">På den **ThousandEyes domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43934-155">On the **ThousandEyes Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    <span data-ttu-id="43934-157">I den **inloggnings-URL** textruta Skriv en URL som:`https://app.thousandeyes.com/login/sso`</span><span class="sxs-lookup"><span data-stu-id="43934-157">In the **Sign-on URL** textbox, type a URL as: `https://app.thousandeyes.com/login/sso`</span></span>

4. <span data-ttu-id="43934-158">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="43934-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. <span data-ttu-id="43934-160">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="43934-160">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="43934-162">På den **ThousandEyes Configuration** klickar du på **konfigurera ThousandEyes** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="43934-162">On the **ThousandEyes Configuration** section, click **Configure ThousandEyes** to open **Configure sign-on** window.</span></span> <span data-ttu-id="43934-163">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="43934-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. <span data-ttu-id="43934-165">I en annan webbläsarfönstret, logga in på ditt **ThousandEyes** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="43934-165">In a different web browser window, sign on to your **ThousandEyes** company site as an administrator.</span></span>

8. <span data-ttu-id="43934-166">Klicka på menyn högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="43934-166">In the menu on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="43934-167">![Inställningar för](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="43934-167">![Settings](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "Settings")</span></span>

9. <span data-ttu-id="43934-168">Klicka på **konto**</span><span class="sxs-lookup"><span data-stu-id="43934-168">Click **Account**</span></span>
   
    <span data-ttu-id="43934-169">![Kontot](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "konto")</span><span class="sxs-lookup"><span data-stu-id="43934-169">![Account](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "Account")</span></span>

10. <span data-ttu-id="43934-170">Klicka på den **säkerhets- och** fliken.</span><span class="sxs-lookup"><span data-stu-id="43934-170">Click the **Security & Authentication** tab.</span></span>
   
    <span data-ttu-id="43934-171">![Säkerhets- och](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "säkerhets- och autentisering")</span><span class="sxs-lookup"><span data-stu-id="43934-171">![Security & Authentication](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "Security & Authentication")</span></span>

11. <span data-ttu-id="43934-172">I den **installationsprogrammet för enkel inloggning** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43934-172">In the **Setup Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="43934-173">![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="43934-173">![Setup Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Setup Single Sign-On")</span></span>
  
    <span data-ttu-id="43934-174">a.</span><span class="sxs-lookup"><span data-stu-id="43934-174">a.</span></span> <span data-ttu-id="43934-175">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="43934-175">Select **Enable Single Sign-On**.</span></span>
  
    <span data-ttu-id="43934-176">b.</span><span class="sxs-lookup"><span data-stu-id="43934-176">b.</span></span> <span data-ttu-id="43934-177">I **URL för inloggningssidan** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="43934-177">In **Login Page URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="43934-178">c.</span><span class="sxs-lookup"><span data-stu-id="43934-178">c.</span></span> <span data-ttu-id="43934-179">I **logga ut Sidadress** textruta klistra in **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="43934-179">In **Logout Page URL** textbox, paste **Sign-Out URL** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="43934-180">d.</span><span class="sxs-lookup"><span data-stu-id="43934-180">d.</span></span> <span data-ttu-id="43934-181">**Identitet providern utfärdaren** textruta klistra in **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="43934-181">**Identity Provider Issuer** textbox, paste **SAML Entity ID** which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="43934-182">e.</span><span class="sxs-lookup"><span data-stu-id="43934-182">e.</span></span> <span data-ttu-id="43934-183">I **verifieringscertifikat**, klickar du på **Välj fil**, och sedan ladda upp det certifikat som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="43934-183">In **Verification Certificate**, click **Choose file**, and then upload the certificate you have downloaded from Azure portal.</span></span>
  
    <span data-ttu-id="43934-184">f.</span><span class="sxs-lookup"><span data-stu-id="43934-184">f.</span></span> <span data-ttu-id="43934-185">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="43934-185">Click **Save**.</span></span>
 
> [!TIP]
> <span data-ttu-id="43934-186">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="43934-186">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="43934-187">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="43934-187">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="43934-188">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="43934-188">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="43934-189">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="43934-189">Creating an Azure AD test user</span></span>
<span data-ttu-id="43934-190">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43934-190">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="43934-192">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="43934-192">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="43934-193">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43934-193">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="43934-195">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="43934-195">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="43934-197">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43934-197">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="43934-199">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43934-199">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="43934-201">a.</span><span class="sxs-lookup"><span data-stu-id="43934-201">a.</span></span> <span data-ttu-id="43934-202">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="43934-202">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="43934-203">b.</span><span class="sxs-lookup"><span data-stu-id="43934-203">b.</span></span> <span data-ttu-id="43934-204">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="43934-204">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="43934-205">c.</span><span class="sxs-lookup"><span data-stu-id="43934-205">c.</span></span> <span data-ttu-id="43934-206">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="43934-206">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="43934-207">d.</span><span class="sxs-lookup"><span data-stu-id="43934-207">d.</span></span> <span data-ttu-id="43934-208">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="43934-208">Click **Create**.</span></span>
 
### <a name="creating-a-thousandeyes-test-user"></a><span data-ttu-id="43934-209">Skapa en testanvändare ThousandEyes</span><span class="sxs-lookup"><span data-stu-id="43934-209">Creating a ThousandEyes test user</span></span>

<span data-ttu-id="43934-210">För att aktivera Azure AD-användare att logga in på ThousandEyes etableras de i ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="43934-210">In order to enable Azure AD users to log into ThousandEyes, they must be provisioned into ThousandEyes.</span></span>  
<span data-ttu-id="43934-211">När det gäller ThousandEyes är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="43934-211">In the case of ThousandEyes, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="43934-212">Du kan använda något annat ThousandEyes användarens konto skapas verktyg eller API: er som tillhandahålls av ThousandEyes för att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="43934-212">You can use any other ThousandEyes user account creation tools or APIs provided by ThousandEyes to provision Azure Active Directory user accounts.</span></span>

<span data-ttu-id="43934-213">**Utför följande steg för att etablera ett användarkonto för ThousandEyes:**</span><span class="sxs-lookup"><span data-stu-id="43934-213">**To provision a user account to ThousandEyes, perform the following steps:**</span></span>

1. <span data-ttu-id="43934-214">Logga in på webbplatsen ThousandEyes företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="43934-214">Log into your ThousandEyes company site as an administrator.</span></span>

2. <span data-ttu-id="43934-215">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="43934-215">Click **Settings**.</span></span>
   
    <span data-ttu-id="43934-216">![Inställningar för](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="43934-216">![Settings](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Settings")</span></span>

3. <span data-ttu-id="43934-217">Klicka på **konto**.</span><span class="sxs-lookup"><span data-stu-id="43934-217">Click **Account**.</span></span>
   
    <span data-ttu-id="43934-218">![Kontot](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "konto")</span><span class="sxs-lookup"><span data-stu-id="43934-218">![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")</span></span>

4. <span data-ttu-id="43934-219">Klicka på den **konton och användare** fliken.</span><span class="sxs-lookup"><span data-stu-id="43934-219">Click the **Accounts & Users** tab.</span></span>
   
    <span data-ttu-id="43934-220">![Konton och användare](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "konton och användare")</span><span class="sxs-lookup"><span data-stu-id="43934-220">![Accounts & Users](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Accounts & Users")</span></span>

5. <span data-ttu-id="43934-221">I den **lägga till användare och konton** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43934-221">In the **Add Users & Accounts** section, perform the following steps:</span></span>
   
    <span data-ttu-id="43934-222">![Lägga till användarkonton](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "lägga till användarkonton")</span><span class="sxs-lookup"><span data-stu-id="43934-222">![Add User Accounts](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Add User Accounts")</span></span>   
  
    <span data-ttu-id="43934-223">a.</span><span class="sxs-lookup"><span data-stu-id="43934-223">a.</span></span> <span data-ttu-id="43934-224">I **namn** textruta, ange namnet på användaren som **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="43934-224">In **Name** textbox, type the name of user like **Britta Simon**.</span></span>

    <span data-ttu-id="43934-225">b.</span><span class="sxs-lookup"><span data-stu-id="43934-225">b.</span></span> <span data-ttu-id="43934-226">I **e-post** textruta, ange den e-posten för användare som  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="43934-226">In **Email** textbox, type the email of user like **brittasimon@contoso.com**.</span></span>
   
    <span data-ttu-id="43934-227">b.</span><span class="sxs-lookup"><span data-stu-id="43934-227">b.</span></span> <span data-ttu-id="43934-228">Klicka på **lägga till nya användare konto**.</span><span class="sxs-lookup"><span data-stu-id="43934-228">Click **Add New User to Account**.</span></span>
      
     >[!NOTE]
     ><span data-ttu-id="43934-229">Azure Active Directory kontoinnehavaren får ett e-postmeddelande med en länk för att bekräfta och aktivera kontot.</span><span class="sxs-lookup"><span data-stu-id="43934-229">The Azure Active Directory account holder will get an email including a link to confirm and activate the account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="43934-230">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="43934-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="43934-231">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ThousandEyes.</span><span class="sxs-lookup"><span data-stu-id="43934-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ThousandEyes.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="43934-233">**Om du vill tilldela ThousandEyes Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="43934-233">**To assign Britta Simon to ThousandEyes, perform the following steps:**</span></span>

1. <span data-ttu-id="43934-234">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="43934-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="43934-236">Välj i listan med program **ThousandEyes**.</span><span class="sxs-lookup"><span data-stu-id="43934-236">In the applications list, select **ThousandEyes**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. <span data-ttu-id="43934-238">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="43934-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="43934-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="43934-240">Click **Add** button.</span></span> <span data-ttu-id="43934-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43934-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="43934-243">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="43934-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="43934-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43934-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="43934-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43934-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="43934-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43934-246">Testing single sign-on</span></span>

<span data-ttu-id="43934-247">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="43934-247">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="43934-248">När du klickar på panelen ThousandEyes på åtkomstpanelen du bör få automatiskt loggat in på ditt ThousandEyes program.</span><span class="sxs-lookup"><span data-stu-id="43934-248">When you click the ThousandEyes tile in the Access Panel, you should get automatically signed-on to your ThousandEyes application.</span></span>

<span data-ttu-id="43934-249">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="43934-249">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43934-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="43934-250">Additional resources</span></span>

* [<span data-ttu-id="43934-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="43934-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="43934-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="43934-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png

