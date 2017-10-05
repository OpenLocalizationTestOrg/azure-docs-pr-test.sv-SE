---
title: "Självstudier: Azure Active Directory-integrering med socker CRM | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och socker CRM."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: jeedes
ms.openlocfilehash: c27aef24e859522b8001ecb747906abdca14d87a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sugar-crm"></a><span data-ttu-id="7a837-103">Självstudier: Azure Active Directory-integrering med socker CRM</span><span class="sxs-lookup"><span data-stu-id="7a837-103">Tutorial: Azure Active Directory integration with Sugar CRM</span></span>

<span data-ttu-id="7a837-104">I kursen får lära du att integrera socker CRM med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7a837-104">In this tutorial, you learn how to integrate Sugar CRM with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7a837-105">Integrera socker CRM med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7a837-105">Integrating Sugar CRM with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7a837-106">Du kan styra i Azure AD som har åtkomst till socker CRM</span><span class="sxs-lookup"><span data-stu-id="7a837-106">You can control in Azure AD who has access to Sugar CRM</span></span>
- <span data-ttu-id="7a837-107">Du kan aktivera användarna att automatiskt hämta loggat in på socker CRM (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7a837-107">You can enable your users to automatically get signed-on to Sugar CRM (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7a837-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7a837-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="7a837-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7a837-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a837-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7a837-110">Prerequisites</span></span>

<span data-ttu-id="7a837-111">För att konfigurera Azure AD-integrering med socker CRM, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7a837-111">To configure Azure AD integration with Sugar CRM, you need the following items:</span></span>

- <span data-ttu-id="7a837-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7a837-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7a837-113">En socker CRM enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7a837-113">A Sugar CRM single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7a837-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7a837-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7a837-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7a837-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7a837-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7a837-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7a837-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7a837-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7a837-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7a837-118">Scenario description</span></span>
<span data-ttu-id="7a837-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7a837-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7a837-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7a837-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7a837-121">Att lägga till socker CRM från galleriet</span><span class="sxs-lookup"><span data-stu-id="7a837-121">Adding Sugar CRM from the gallery</span></span>
2. <span data-ttu-id="7a837-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a837-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sugar-crm-from-the-gallery"></a><span data-ttu-id="7a837-123">Att lägga till socker CRM från galleriet</span><span class="sxs-lookup"><span data-stu-id="7a837-123">Adding Sugar CRM from the gallery</span></span>
<span data-ttu-id="7a837-124">Du måste lägga till socker CRM från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av socker CRM i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7a837-124">To configure the integration of Sugar CRM into Azure AD, you need to add Sugar CRM from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7a837-125">**Utför följande steg för att lägga till socker CRM från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7a837-125">**To add Sugar CRM from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7a837-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7a837-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7a837-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7a837-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7a837-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7a837-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7a837-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a837-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7a837-133">I sökrutan skriver **socker CRM**.</span><span class="sxs-lookup"><span data-stu-id="7a837-133">In the search box, type **Sugar CRM**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_search.png)

5. <span data-ttu-id="7a837-135">Välj i resultatpanelen **socker CRM**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7a837-135">In the results panel, select **Sugar CRM**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7a837-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a837-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7a837-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med socker CRM baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7a837-138">In this section, you configure and test Azure AD single sign-on with Sugar CRM based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7a837-139">Azure AD måste du känna till användaren i socker CRM motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7a837-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Sugar CRM is to a user in Azure AD.</span></span> <span data-ttu-id="7a837-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i socker CRM upprättas.</span><span class="sxs-lookup"><span data-stu-id="7a837-140">In other words, a link relationship between an Azure AD user and the related user in Sugar CRM needs to be established.</span></span>

<span data-ttu-id="7a837-141">I socker CRM, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7a837-141">In Sugar CRM, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7a837-142">Om du vill konfigurera och testa Azure AD enkel inloggning med socker CRM, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7a837-142">To configure and test Azure AD single sign-on with Sugar CRM, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7a837-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7a837-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7a837-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7a837-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7a837-145">**[Skapa en testanvändare socker CRM](#creating-a-sugar-crm-test-user)**  – du har en motsvarighet för Britta Simon i socker CRM som är länkad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7a837-145">**[Creating a Sugar CRM test user](#creating-a-sugar-crm-test-user)** - to have a counterpart of Britta Simon in Sugar CRM that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7a837-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7a837-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7a837-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7a837-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7a837-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a837-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7a837-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt socker CRM-program.</span><span class="sxs-lookup"><span data-stu-id="7a837-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Sugar CRM application.</span></span>

<span data-ttu-id="7a837-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med socker CRM:**</span><span class="sxs-lookup"><span data-stu-id="7a837-150">**To configure Azure AD single sign-on with Sugar CRM, perform the following steps:**</span></span>

1. <span data-ttu-id="7a837-151">I Azure-portalen på den **socker CRM** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7a837-151">In the Azure portal, on the **Sugar CRM** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7a837-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7a837-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_samlbase.png)

3. <span data-ttu-id="7a837-155">På den **socker CRM-domänen och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a837-155">On the **Sugar CRM Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_url.png)

    <span data-ttu-id="7a837-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="7a837-157">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.sugarondemand.com` |
    | `https://<companyname>.trial.sugarcrm` |

    > [!NOTE] 
    > <span data-ttu-id="7a837-158">Värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7a837-158">The value is not real.</span></span> <span data-ttu-id="7a837-159">Uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="7a837-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="7a837-160">Kontakta [socker CRM-klienten supportteamet](https://support.sugarcrm.com/) värdet hämtas.</span><span class="sxs-lookup"><span data-stu-id="7a837-160">Contact [Sugar CRM Client support team](https://support.sugarcrm.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="7a837-161">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7a837-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_certificate.png) 

5. <span data-ttu-id="7a837-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a837-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7a837-165">På den **socker CRM Configuration** klickar du på **konfigurera socker CRM** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7a837-165">On the **Sugar CRM Configuration** section, click **Configure Sugar CRM** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7a837-166">Kopiera den **Sign-Out URL och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7a837-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_configure.png) 

7. <span data-ttu-id="7a837-168">I en annan webbläsarfönster loggar du in på webbplatsen socker CRM företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="7a837-168">In a different web browser window, log in to your Sugar CRM company site as an administrator.</span></span>

8. <span data-ttu-id="7a837-169">Gå till **Admin**.</span><span class="sxs-lookup"><span data-stu-id="7a837-169">Go to **Admin**.</span></span>
   
    <span data-ttu-id="7a837-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="7a837-170">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

9. <span data-ttu-id="7a837-171">I den **Administration** klickar du på **lösenordshantering**.</span><span class="sxs-lookup"><span data-stu-id="7a837-171">In the **Administration** section, click **Password Management**.</span></span>
   
    <span data-ttu-id="7a837-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="7a837-172">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795889.png "Administration")</span></span>

10. <span data-ttu-id="7a837-173">Välj **aktivera SAML-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="7a837-173">Select **Enable SAML Authentication**.</span></span>
   
    <span data-ttu-id="7a837-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="7a837-174">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795890.png "Administration")</span></span>

11. <span data-ttu-id="7a837-175">I den **SAML-autentisering** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a837-175">In the **SAML Authentication** section, perform the following steps:</span></span>
   
    <span data-ttu-id="7a837-176">![SAML-autentisering](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML-autentisering")</span><span class="sxs-lookup"><span data-stu-id="7a837-176">![SAML Authentication](./media/active-directory-saas-sugarcrm-tutorial/ic795891.png "SAML Authentication")</span></span>  
 
    <span data-ttu-id="7a837-177">a.</span><span class="sxs-lookup"><span data-stu-id="7a837-177">a.</span></span> <span data-ttu-id="7a837-178">I den **Inloggningswebbadressen** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a837-178">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="7a837-179">b.</span><span class="sxs-lookup"><span data-stu-id="7a837-179">b.</span></span> <span data-ttu-id="7a837-180">I den **Servicenivåmål URL** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a837-180">In the **SLO URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
  
    <span data-ttu-id="7a837-181">c.</span><span class="sxs-lookup"><span data-stu-id="7a837-181">c.</span></span> <span data-ttu-id="7a837-182">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in hela certifikatet till **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="7a837-182">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="7a837-183">d.</span><span class="sxs-lookup"><span data-stu-id="7a837-183">d.</span></span> <span data-ttu-id="7a837-184">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7a837-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7a837-185">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7a837-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7a837-186">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7a837-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7a837-187">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7a837-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7a837-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7a837-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="7a837-189">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7a837-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7a837-191">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7a837-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7a837-192">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7a837-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7a837-194">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7a837-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7a837-196">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a837-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7a837-198">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a837-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sugarcrm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7a837-200">a.</span><span class="sxs-lookup"><span data-stu-id="7a837-200">a.</span></span> <span data-ttu-id="7a837-201">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7a837-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7a837-202">b.</span><span class="sxs-lookup"><span data-stu-id="7a837-202">b.</span></span> <span data-ttu-id="7a837-203">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7a837-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7a837-204">c.</span><span class="sxs-lookup"><span data-stu-id="7a837-204">c.</span></span> <span data-ttu-id="7a837-205">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7a837-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="7a837-206">d.</span><span class="sxs-lookup"><span data-stu-id="7a837-206">d.</span></span> <span data-ttu-id="7a837-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7a837-207">Click **Create**.</span></span>
 
### <a name="creating-a-sugar-crm-test-user"></a><span data-ttu-id="7a837-208">Skapa en testanvändare socker CRM</span><span class="sxs-lookup"><span data-stu-id="7a837-208">Creating a Sugar CRM test user</span></span>

<span data-ttu-id="7a837-209">För att aktivera Azure AD-användare kan logga in på socker CRM etableras de till socker CRM.</span><span class="sxs-lookup"><span data-stu-id="7a837-209">In order to enable Azure AD users to log in to Sugar CRM, they must be provisioned to Sugar CRM.</span></span>

<span data-ttu-id="7a837-210">När det gäller socker CRM är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7a837-210">In the case of Sugar CRM, provisioning is a manual task.</span></span>

<span data-ttu-id="7a837-211">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="7a837-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="7a837-212">Logga in på ditt **socker CRM** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="7a837-212">Log in to your **Sugar CRM** company site as administrator.</span></span>

2. <span data-ttu-id="7a837-213">Gå till **Admin**.</span><span class="sxs-lookup"><span data-stu-id="7a837-213">Go to **Admin**.</span></span>
   
    <span data-ttu-id="7a837-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="7a837-214">![Admin](./media/active-directory-saas-sugarcrm-tutorial/ic795888.png "Admin")</span></span>

3. <span data-ttu-id="7a837-215">I den **Administration** klickar du på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="7a837-215">In the **Administration** section, click **User Management**.</span></span>
   
    <span data-ttu-id="7a837-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="7a837-216">![Administration](./media/active-directory-saas-sugarcrm-tutorial/ic795893.png "Administration")</span></span>

4. <span data-ttu-id="7a837-217">Gå till **användare \> Skapa ny användare**.</span><span class="sxs-lookup"><span data-stu-id="7a837-217">Go to **Users \> Create New User**.</span></span>
   
    <span data-ttu-id="7a837-218">![Skapa ny användare](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Skapa ny användare")</span><span class="sxs-lookup"><span data-stu-id="7a837-218">![Create New User](./media/active-directory-saas-sugarcrm-tutorial/ic795894.png "Create New User")</span></span>

5. <span data-ttu-id="7a837-219">På den **användarprofil** fliken, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7a837-219">On the **User Profile** tab, perform the following steps:</span></span>
   
    <span data-ttu-id="7a837-220">![Ny användare](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="7a837-220">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795895.png "New User")</span></span>

    <span data-ttu-id="7a837-221">a.</span><span class="sxs-lookup"><span data-stu-id="7a837-221">a.</span></span> <span data-ttu-id="7a837-222">Typ av **användarnamn**, **efternamn**, och **e-postadress** av en giltig Azure Active Directory-användare i den relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="7a837-222">Type the **user name**, **last name**, and **email address** of a valid Azure Active Directory user into the related textboxes.</span></span>
  
6. <span data-ttu-id="7a837-223">Som **Status**väljer **Active**.</span><span class="sxs-lookup"><span data-stu-id="7a837-223">As **Status**, select **Active**.</span></span>

7. <span data-ttu-id="7a837-224">Utför följande steg på fliken lösenord:</span><span class="sxs-lookup"><span data-stu-id="7a837-224">On the Password tab, perform the following steps:</span></span>
   
    <span data-ttu-id="7a837-225">![Ny användare](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="7a837-225">![New User](./media/active-directory-saas-sugarcrm-tutorial/ic795896.png "New User")</span></span>

    <span data-ttu-id="7a837-226">a.</span><span class="sxs-lookup"><span data-stu-id="7a837-226">a.</span></span> <span data-ttu-id="7a837-227">Ange lösenordet i textrutan relaterade.</span><span class="sxs-lookup"><span data-stu-id="7a837-227">Type the password into the related textbox.</span></span>

    <span data-ttu-id="7a837-228">b.</span><span class="sxs-lookup"><span data-stu-id="7a837-228">b.</span></span> <span data-ttu-id="7a837-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7a837-229">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="7a837-230">Du kan använda något annat socker CRM användarens konto skapas verktyg eller API: er som tillhandahålls av socker CRM etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="7a837-230">You can use any other Sugar CRM user account creation tools or APIs provided by Sugar CRM to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="7a837-231">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7a837-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="7a837-232">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till socker CRM.</span><span class="sxs-lookup"><span data-stu-id="7a837-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Sugar CRM.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7a837-234">**Om du vill tilldela socker CRM Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7a837-234">**To assign Britta Simon to Sugar CRM, perform the following steps:**</span></span>

1. <span data-ttu-id="7a837-235">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7a837-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7a837-237">Välj i listan med program **socker CRM**.</span><span class="sxs-lookup"><span data-stu-id="7a837-237">In the applications list, select **Sugar CRM**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sugarcrm-tutorial/tutorial_sugarcrm_app.png) 

3. <span data-ttu-id="7a837-239">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7a837-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7a837-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7a837-241">Click **Add** button.</span></span> <span data-ttu-id="7a837-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a837-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7a837-244">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7a837-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7a837-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a837-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7a837-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7a837-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7a837-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7a837-247">Testing single sign-on</span></span>

<span data-ttu-id="7a837-248">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7a837-248">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7a837-249">När du klickar på panelen socker CRM på åtkomstpanelen du bör få automatiskt loggat in på ditt socker CRM-program.</span><span class="sxs-lookup"><span data-stu-id="7a837-249">When you click the Sugar CRM tile in the Access Panel, you should get automatically signed-on to your Sugar CRM application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7a837-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7a837-250">Additional resources</span></span>

* [<span data-ttu-id="7a837-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7a837-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7a837-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7a837-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sugarcrm-tutorial/tutorial_general_203.png

