---
title: "Självstudier: Azure Active Directory-integrering med Intacct | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Intacct."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c203b192b9da0d280cbd7f6c123219242ee4a3d1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a><span data-ttu-id="e6372-103">Självstudier: Azure Active Directory-integrering med Intacct</span><span class="sxs-lookup"><span data-stu-id="e6372-103">Tutorial: Azure Active Directory integration with Intacct</span></span>

<span data-ttu-id="e6372-104">I kursen får lära du att integrera Intacct med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e6372-104">In this tutorial, you learn how to integrate Intacct with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e6372-105">Integrera Intacct med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e6372-105">Integrating Intacct with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e6372-106">Du kan styra i Azure AD som har åtkomst till Intacct</span><span class="sxs-lookup"><span data-stu-id="e6372-106">You can control in Azure AD who has access to Intacct</span></span>
- <span data-ttu-id="e6372-107">Du kan aktivera användarna att automatiskt hämta loggat in på Intacct (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e6372-107">You can enable your users to automatically get signed-on to Intacct (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e6372-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e6372-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e6372-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e6372-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6372-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e6372-110">Prerequisites</span></span>

<span data-ttu-id="e6372-111">För att konfigurera Azure AD-integrering med Intacct, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="e6372-111">To configure Azure AD integration with Intacct, you need the following items:</span></span>

- <span data-ttu-id="e6372-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e6372-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e6372-113">En Intacct enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="e6372-113">An Intacct single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e6372-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e6372-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e6372-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e6372-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e6372-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e6372-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e6372-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e6372-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e6372-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e6372-118">Scenario description</span></span>
<span data-ttu-id="e6372-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e6372-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e6372-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e6372-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e6372-121">Att lägga till Intacct från galleriet</span><span class="sxs-lookup"><span data-stu-id="e6372-121">Adding Intacct from the gallery</span></span>
2. <span data-ttu-id="e6372-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e6372-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-intacct-from-the-gallery"></a><span data-ttu-id="e6372-123">Att lägga till Intacct från galleriet</span><span class="sxs-lookup"><span data-stu-id="e6372-123">Adding Intacct from the gallery</span></span>
<span data-ttu-id="e6372-124">Du måste lägga till Intacct från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Intacct i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6372-124">To configure the integration of Intacct into Azure AD, you need to add Intacct from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e6372-125">**Utför följande steg för att lägga till Intacct från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="e6372-125">**To add Intacct from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e6372-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e6372-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e6372-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e6372-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e6372-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e6372-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="e6372-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e6372-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="e6372-133">I sökrutan skriver **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="e6372-133">In the search box, type **Intacct**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_search.png)

5. <span data-ttu-id="e6372-135">Välj i resultatpanelen **Intacct**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="e6372-135">In the results panel, select **Intacct**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e6372-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e6372-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e6372-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Intacct baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e6372-138">In this section, you configure and test Azure AD single sign-on with Intacct based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e6372-139">Azure AD måste du känna till användaren i Intacct motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="e6372-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Intacct is to a user in Azure AD.</span></span> <span data-ttu-id="e6372-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Intacct upprättas.</span><span class="sxs-lookup"><span data-stu-id="e6372-140">In other words, a link relationship between an Azure AD user and the related user in Intacct needs to be established.</span></span>

<span data-ttu-id="e6372-141">I Intacct, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="e6372-141">In Intacct, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e6372-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Intacct, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e6372-142">To configure and test Azure AD single sign-on with Intacct, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e6372-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e6372-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e6372-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e6372-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e6372-145">**[Skapa en testanvändare Intacct](#creating-an-intacct-test-user)**  – du har en motsvarighet för Britta Simon i Intacct som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="e6372-145">**[Creating an Intacct test user](#creating-an-intacct-test-user)** - to have a counterpart of Britta Simon in Intacct that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e6372-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e6372-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e6372-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e6372-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e6372-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e6372-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e6372-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Intacct program.</span><span class="sxs-lookup"><span data-stu-id="e6372-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Intacct application.</span></span>

<span data-ttu-id="e6372-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Intacct:**</span><span class="sxs-lookup"><span data-stu-id="e6372-150">**To configure Azure AD single sign-on with Intacct, perform the following steps:**</span></span>

1. <span data-ttu-id="e6372-151">I Azure-portalen på den **Intacct** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="e6372-151">In the Azure portal, on the **Intacct** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="e6372-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e6372-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_samlbase.png)

3. <span data-ttu-id="e6372-155">På den **Intacct domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e6372-155">On the **Intacct Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_url.png)

    <span data-ttu-id="e6372-157">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="e6372-157">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > <span data-ttu-id="e6372-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="e6372-158">This value is not real.</span></span> <span data-ttu-id="e6372-159">Uppdatera det här värdet med det faktiska Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="e6372-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="e6372-160">Kontakta [Intacct supportteamet](https://us.intacct.com/support) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="e6372-160">Contact [Intacct support team](https://us.intacct.com/support) to get this value.</span></span>

4. <span data-ttu-id="e6372-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="e6372-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_certificate.png) 

5. <span data-ttu-id="e6372-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e6372-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e6372-165">På den **Intacct Configuration** klickar du på **konfigurera Intacct** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="e6372-165">On the **Intacct Configuration** section, click **Configure Intacct** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e6372-166">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="e6372-166">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_configure.png) 

7. <span data-ttu-id="e6372-168">I en annan webbläsarfönster loggar du in på ditt Intacct företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="e6372-168">In a different web browser window, sign in to your Intacct company site as an administrator.</span></span>

8. <span data-ttu-id="e6372-169">Klicka på den **företagets** fliken och klicka sedan på **företagets information**.</span><span class="sxs-lookup"><span data-stu-id="e6372-169">Click the **Company** tab, and then click **Company Info**.</span></span>

    <span data-ttu-id="e6372-170">![Företagets](./media/active-directory-saas-intacct-tutorial/ic790037.png "företag")</span><span class="sxs-lookup"><span data-stu-id="e6372-170">![Company](./media/active-directory-saas-intacct-tutorial/ic790037.png "Company")</span></span>

9. <span data-ttu-id="e6372-171">Klicka på den **säkerhet** fliken och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="e6372-171">Click the **Security** tab, and then click **Edit**.</span></span>

    <span data-ttu-id="e6372-172">![Säkerhet](./media/active-directory-saas-intacct-tutorial/ic790038.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="e6372-172">![Security](./media/active-directory-saas-intacct-tutorial/ic790038.png "Security")</span></span>

10. <span data-ttu-id="e6372-173">I den **enkel inloggning (SSO)** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e6372-173">In the **Single sign on (SSO)** section, perform the following steps:</span></span>

    <span data-ttu-id="e6372-174">![Enkel inloggning](./media/active-directory-saas-intacct-tutorial/ic790039.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="e6372-174">![Single sign on](./media/active-directory-saas-intacct-tutorial/ic790039.png "single sign on")</span></span>

    <span data-ttu-id="e6372-175">a.</span><span class="sxs-lookup"><span data-stu-id="e6372-175">a.</span></span> <span data-ttu-id="e6372-176">Välj **aktivera enkel inloggning på**.</span><span class="sxs-lookup"><span data-stu-id="e6372-176">Select **Enable single sign on**.</span></span>

    <span data-ttu-id="e6372-177">b.</span><span class="sxs-lookup"><span data-stu-id="e6372-177">b.</span></span> <span data-ttu-id="e6372-178">Som **identitet providertyp**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="e6372-178">As **Identity provider type**, select **SAML 2.0**.</span></span>

    <span data-ttu-id="e6372-179">c.</span><span class="sxs-lookup"><span data-stu-id="e6372-179">c.</span></span> <span data-ttu-id="e6372-180">I **utfärdar-URL** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6372-180">In **Issuer URL** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="e6372-181">d.</span><span class="sxs-lookup"><span data-stu-id="e6372-181">d.</span></span> <span data-ttu-id="e6372-182">I **Inloggningswebbadressen** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e6372-182">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="e6372-183">e.</span><span class="sxs-lookup"><span data-stu-id="e6372-183">e.</span></span> <span data-ttu-id="e6372-184">Öppna din **Base64-** kodat certifikat i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="e6372-184">Open your **base-64** encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Certificate** box.</span></span>
   
    <span data-ttu-id="e6372-185">f.</span><span class="sxs-lookup"><span data-stu-id="e6372-185">f.</span></span> <span data-ttu-id="e6372-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e6372-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="e6372-187">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="e6372-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e6372-188">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="e6372-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e6372-189">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e6372-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e6372-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6372-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="e6372-191">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e6372-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="e6372-193">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="e6372-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e6372-194">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="e6372-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e6372-196">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="e6372-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e6372-198">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e6372-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e6372-200">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e6372-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-intacct-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e6372-202">a.</span><span class="sxs-lookup"><span data-stu-id="e6372-202">a.</span></span> <span data-ttu-id="e6372-203">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e6372-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e6372-204">b.</span><span class="sxs-lookup"><span data-stu-id="e6372-204">b.</span></span> <span data-ttu-id="e6372-205">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e6372-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e6372-206">c.</span><span class="sxs-lookup"><span data-stu-id="e6372-206">c.</span></span> <span data-ttu-id="e6372-207">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e6372-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e6372-208">d.</span><span class="sxs-lookup"><span data-stu-id="e6372-208">d.</span></span> <span data-ttu-id="e6372-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e6372-209">Click **Create**.</span></span>
 
### <a name="creating-an-intacct-test-user"></a><span data-ttu-id="e6372-210">Skapa en testanvändare Intacct</span><span class="sxs-lookup"><span data-stu-id="e6372-210">Creating an Intacct test user</span></span>

<span data-ttu-id="e6372-211">Om du vill konfigurera Azure AD-användare så att de kan logga in på Intacct, måste de etableras i Intacct.</span><span class="sxs-lookup"><span data-stu-id="e6372-211">To set up Azure AD users so they can sign in to Intacct, they must be provisioned into Intacct.</span></span> <span data-ttu-id="e6372-212">Intacct är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="e6372-212">For Intacct, provisioning is a manual task.</span></span>

<span data-ttu-id="e6372-213">**Utför följande steg för att etablera användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="e6372-213">**To provision user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="e6372-214">Logga in på ditt **Intacct** klient.</span><span class="sxs-lookup"><span data-stu-id="e6372-214">Sign in to your **Intacct** tenant.</span></span>

2. <span data-ttu-id="e6372-215">Klicka på den **företagets** fliken och klicka sedan på **användare**.</span><span class="sxs-lookup"><span data-stu-id="e6372-215">Click the **Company** tab, and then click **Users**.</span></span>

    <span data-ttu-id="e6372-216">![Användare](./media/active-directory-saas-intacct-tutorial/ic790041.png "användare")</span><span class="sxs-lookup"><span data-stu-id="e6372-216">![Users](./media/active-directory-saas-intacct-tutorial/ic790041.png "Users")</span></span>
3. <span data-ttu-id="e6372-217">Klicka på den **Lägg till** fliken.</span><span class="sxs-lookup"><span data-stu-id="e6372-217">Click the **Add** tab.</span></span>

    <span data-ttu-id="e6372-218">![Lägg till](./media/active-directory-saas-intacct-tutorial/ic790042.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="e6372-218">![Add](./media/active-directory-saas-intacct-tutorial/ic790042.png "Add")</span></span>
4. <span data-ttu-id="e6372-219">I den **användarinformation** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="e6372-219">In the **User Information** section, perform the following steps:</span></span>

    <span data-ttu-id="e6372-220">![Användarinformation](./media/active-directory-saas-intacct-tutorial/ic790043.png "användarinformation")</span><span class="sxs-lookup"><span data-stu-id="e6372-220">![User Information](./media/active-directory-saas-intacct-tutorial/ic790043.png "User Information")</span></span>

    <span data-ttu-id="e6372-221">a.</span><span class="sxs-lookup"><span data-stu-id="e6372-221">a.</span></span> <span data-ttu-id="e6372-222">Ange den **användar-ID**, **efternamn**, **Förnamn**, **e-postadress**, **rubrik**, och **Phone** för ett Azure AD-konto som du vill etablera i den **användarinformation** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e6372-222">Enter the **User ID**, the **Last name**, **First name**, the **Email address**, the **Title**, and the **Phone** of an Azure AD account that you want to provision into the **User Information** section.</span></span>

    <span data-ttu-id="e6372-223">b.</span><span class="sxs-lookup"><span data-stu-id="e6372-223">b.</span></span> <span data-ttu-id="e6372-224">Välj den **administratörsrättigheter** för ett Azure AD-konto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="e6372-224">Select the **Admin privileges** of an Azure AD account that you want to provision.</span></span>
   
    <span data-ttu-id="e6372-225">c.</span><span class="sxs-lookup"><span data-stu-id="e6372-225">c.</span></span> <span data-ttu-id="e6372-226">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e6372-226">Click **Save**.</span></span> <span data-ttu-id="e6372-227">Kontoinnehavaren Azure AD tar emot ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="e6372-227">The Azure AD account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

>[!NOTE]
><span data-ttu-id="e6372-228">Du kan använda andra Intacct användare skapa verktyg och API: er som tillhandahålls av Intacct för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="e6372-228">To provision Azure AD user accounts, you can use other Intacct user account creation tools or APIs that are provided by Intacct.</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e6372-229">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="e6372-229">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e6372-230">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Intacct.</span><span class="sxs-lookup"><span data-stu-id="e6372-230">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Intacct.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="e6372-232">**Om du vill tilldela Intacct Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e6372-232">**To assign Britta Simon to Intacct, perform the following steps:**</span></span>

1. <span data-ttu-id="e6372-233">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="e6372-233">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="e6372-235">Välj i listan med program **Intacct**.</span><span class="sxs-lookup"><span data-stu-id="e6372-235">In the applications list, select **Intacct**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-intacct-tutorial/tutorial_intacct_app.png) 

3. <span data-ttu-id="e6372-237">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e6372-237">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="e6372-239">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e6372-239">Click **Add** button.</span></span> <span data-ttu-id="e6372-240">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e6372-240">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="e6372-242">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="e6372-242">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e6372-243">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e6372-243">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e6372-244">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e6372-244">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e6372-245">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e6372-245">Testing single sign-on</span></span>

<span data-ttu-id="e6372-246">I det här avsnittet testa du Azure AD enkel inloggning konfigurationen med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e6372-246">In this section, you test your Azure AD single sign-on configuration by using the Access Panel.</span></span>

<span data-ttu-id="e6372-247">När du klickar på panelen Intacct på åtkomstpanelen bör du vara automatiskt inloggad i tillämpningsprogrammet Intacct.</span><span class="sxs-lookup"><span data-stu-id="e6372-247">When you click the Intacct tile in the Access Panel, you should be automatically signed in to your Intacct application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6372-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e6372-248">Additional resources</span></span>

* [<span data-ttu-id="e6372-249">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e6372-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e6372-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e6372-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intacct-tutorial/tutorial_general_203.png

