---
title: "Självstudier: Azure Active Directory-integrering med ITRP | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ITRP."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: fae1c7b6b0e04c1e23123d3aee7913cb3131e645
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a><span data-ttu-id="41103-103">Självstudier: Azure Active Directory-integrering med ITRP</span><span class="sxs-lookup"><span data-stu-id="41103-103">Tutorial: Azure Active Directory integration with ITRP</span></span>

<span data-ttu-id="41103-104">I kursen får lära du att integrera ITRP med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="41103-104">In this tutorial, you learn how to integrate ITRP with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="41103-105">Integrera ITRP med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="41103-105">Integrating ITRP with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="41103-106">Du kan styra i Azure AD som har åtkomst till ITRP</span><span class="sxs-lookup"><span data-stu-id="41103-106">You can control in Azure AD who has access to ITRP</span></span>
- <span data-ttu-id="41103-107">Du kan aktivera användarna att automatiskt hämta loggat in på ITRP (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="41103-107">You can enable your users to automatically get signed-on to ITRP (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="41103-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="41103-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="41103-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="41103-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41103-110">Krav</span><span class="sxs-lookup"><span data-stu-id="41103-110">Prerequisites</span></span>

<span data-ttu-id="41103-111">För att konfigurera Azure AD-integrering med ITRP, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="41103-111">To configure Azure AD integration with ITRP, you need the following items:</span></span>

- <span data-ttu-id="41103-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="41103-112">An Azure AD subscription</span></span>
- <span data-ttu-id="41103-113">En ITRP enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="41103-113">An ITRP single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="41103-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="41103-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="41103-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="41103-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="41103-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="41103-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="41103-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="41103-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="41103-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="41103-118">Scenario description</span></span>
<span data-ttu-id="41103-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="41103-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="41103-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="41103-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="41103-121">Att lägga till ITRP från galleriet</span><span class="sxs-lookup"><span data-stu-id="41103-121">Adding ITRP from the gallery</span></span>
2. <span data-ttu-id="41103-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="41103-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-itrp-from-the-gallery"></a><span data-ttu-id="41103-123">Att lägga till ITRP från galleriet</span><span class="sxs-lookup"><span data-stu-id="41103-123">Adding ITRP from the gallery</span></span>
<span data-ttu-id="41103-124">Du måste lägga till ITRP från galleriet i listan över hanterade SaaS-appar för att konfigurera ITRP i till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="41103-124">To configure the integration of ITRP in to Azure AD, you need to add ITRP from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="41103-125">**Utför följande steg för att lägga till ITRP från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="41103-125">**To add ITRP from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="41103-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="41103-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="41103-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="41103-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="41103-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="41103-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="41103-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="41103-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="41103-133">I sökrutan skriver **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="41103-133">In the search box, type **ITRP**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_search.png)

5. <span data-ttu-id="41103-135">Välj i resultatpanelen **ITRP**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="41103-135">In the results panel, select **ITRP**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="41103-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="41103-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="41103-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ITRP baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="41103-138">In this section, you configure and test Azure AD single sign-on with ITRP based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="41103-139">Azure AD måste du känna till användaren i ITRP motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="41103-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ITRP is to a user in Azure AD.</span></span> <span data-ttu-id="41103-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ITRP upprättas.</span><span class="sxs-lookup"><span data-stu-id="41103-140">In other words, a link relationship between an Azure AD user and the related user in ITRP needs to be established.</span></span>

<span data-ttu-id="41103-141">I ITRP, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="41103-141">In ITRP, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="41103-142">Om du vill konfigurera och testa Azure AD enkel inloggning med ITRP, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="41103-142">To configure and test Azure AD single sign-on with ITRP, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="41103-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="41103-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="41103-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="41103-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="41103-145">**[Skapa en ITRP testanvändare](#creating-an-itrp-test-user)**  – du har en motsvarighet för Britta Simon i ITRP som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="41103-145">**[Creating an ITRP test user](#creating-an-itrp-test-user)** - to have a counterpart of Britta Simon in ITRP that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="41103-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="41103-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="41103-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="41103-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="41103-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="41103-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="41103-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt ITRP program.</span><span class="sxs-lookup"><span data-stu-id="41103-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ITRP application.</span></span>

<span data-ttu-id="41103-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ITRP:**</span><span class="sxs-lookup"><span data-stu-id="41103-150">**To configure Azure AD single sign-on with ITRP, perform the following steps:**</span></span>

1. <span data-ttu-id="41103-151">I Azure-portalen på den **ITRP** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="41103-151">In the Azure portal, on the **ITRP** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="41103-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="41103-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_samlbase.png)

3. <span data-ttu-id="41103-155">På den **ITRP domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="41103-155">On the **ITRP Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_url.png)

    <span data-ttu-id="41103-157">a.</span><span class="sxs-lookup"><span data-stu-id="41103-157">a.</span></span> <span data-ttu-id="41103-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="41103-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.itrp.com`</span></span>

    <span data-ttu-id="41103-159">b.</span><span class="sxs-lookup"><span data-stu-id="41103-159">b.</span></span> <span data-ttu-id="41103-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<tenant-name>.itrp.com`</span><span class="sxs-lookup"><span data-stu-id="41103-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<tenant-name>.itrp.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="41103-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="41103-161">These values are not real.</span></span> <span data-ttu-id="41103-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="41103-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="41103-163">Kontakta [ITRP klienten supportteamet](https://www.itrp.com/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="41103-163">Contact [ITRP Client support team](https://www.itrp.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="41103-164">På den **SAML-signeringscertifikat** avsnittet, kopiera den **TUMAVTRYCKET** värdet för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="41103-164">On the **SAML Signing Certificate** section, copy the **THUMBPRINT** value of certificate.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_certificate.png) 

5. <span data-ttu-id="41103-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="41103-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="41103-168">På den **ITRP Configuration** klickar du på **konfigurera ITRP** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="41103-168">On the **ITRP Configuration** section, click **Configure ITRP** to open **Configure sign-on** window.</span></span> <span data-ttu-id="41103-169">Kopiera den **SAML enkel inloggning Service Webbadressen och Sign-Out** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="41103-169">Copy the **SAML Single Sign-On Service URL and Sign-Out URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_configure.png) 

7. <span data-ttu-id="41103-171">I en annan webbläsarfönster loggar du in på webbplatsen ITRP företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="41103-171">In a different web browser window, log in to your ITRP company site as an administrator.</span></span>

8. <span data-ttu-id="41103-172">Klicka på i verktygsfältet högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="41103-172">In the toolbar on the top, click **Settings**.</span></span>
   
    <span data-ttu-id="41103-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span><span class="sxs-lookup"><span data-stu-id="41103-173">![ITRP](./media/active-directory-saas-itrp-tutorial/ic775570.png "ITRP")</span></span>

8. <span data-ttu-id="41103-174">I det vänstra navigeringsfönstret väljer **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="41103-174">In the left navigation pane, select **Single Sign-On**.</span></span>
   
    <span data-ttu-id="41103-175">![Enkel inloggning](./media/active-directory-saas-itrp-tutorial/ic775571.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="41103-175">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775571.png "Single Sign-On")</span></span>

9. <span data-ttu-id="41103-176">Utför följande steg i konfigurationsavsnittet enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="41103-176">In the Single Sign-On configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="41103-177">![Enkel inloggning](./media/active-directory-saas-itrp-tutorial/ic775572.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="41103-177">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775572.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="41103-178">![Enkel inloggning](./media/active-directory-saas-itrp-tutorial/ic775573.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="41103-178">![Single Sign-On](./media/active-directory-saas-itrp-tutorial/ic775573.png "Single Sign-On")</span></span>   

    <span data-ttu-id="41103-179">a.</span><span class="sxs-lookup"><span data-stu-id="41103-179">a.</span></span> <span data-ttu-id="41103-180">Klicka på **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="41103-180">Click **Enable**.</span></span>

    <span data-ttu-id="41103-181">b.</span><span class="sxs-lookup"><span data-stu-id="41103-181">b.</span></span> <span data-ttu-id="41103-182">I **Remote logga ut URL** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="41103-182">In **Remote Log Out URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="41103-183">c.</span><span class="sxs-lookup"><span data-stu-id="41103-183">c.</span></span> <span data-ttu-id="41103-184">I **SAML SSO URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="41103-184">In **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="41103-185">d.In **certifikat fingeravtryck** textruta klistra in den **tumavtrycket** värdet för certifikat som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="41103-185">d.In **Certificate Fingerprint** textbox, paste the **Thumbprint** value of certificate, which you have copied from Azure portal.</span></span> 
      
10. <span data-ttu-id="41103-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="41103-186">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="41103-187">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="41103-187">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="41103-188">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="41103-188">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="41103-189">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="41103-189">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="41103-190">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="41103-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="41103-191">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="41103-191">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="41103-193">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="41103-193">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="41103-194">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="41103-194">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="41103-196">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="41103-196">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="41103-198">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="41103-198">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="41103-200">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="41103-200">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-itrp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="41103-202">a.</span><span class="sxs-lookup"><span data-stu-id="41103-202">a.</span></span> <span data-ttu-id="41103-203">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="41103-203">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="41103-204">b.</span><span class="sxs-lookup"><span data-stu-id="41103-204">b.</span></span> <span data-ttu-id="41103-205">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="41103-205">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="41103-206">c.</span><span class="sxs-lookup"><span data-stu-id="41103-206">c.</span></span> <span data-ttu-id="41103-207">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="41103-207">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="41103-208">d.</span><span class="sxs-lookup"><span data-stu-id="41103-208">d.</span></span> <span data-ttu-id="41103-209">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="41103-209">Click **Create**.</span></span>
 
### <a name="creating-an-itrp-test-user"></a><span data-ttu-id="41103-210">Skapa en testanvändare ITRP</span><span class="sxs-lookup"><span data-stu-id="41103-210">Creating an ITRP test user</span></span>

<span data-ttu-id="41103-211">Om du vill aktivera Azure AD-användare kan logga in på ITRP måste de vara etablerade i till ITRP.</span><span class="sxs-lookup"><span data-stu-id="41103-211">To enable Azure AD users to log in to ITRP, they must be provisioned in to ITRP.</span></span>  

<span data-ttu-id="41103-212">När det gäller ITRP är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="41103-212">In the case of ITRP, provisioning is a manual task.</span></span>

<span data-ttu-id="41103-213">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="41103-213">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="41103-214">Logga in på ditt **ITRP** klient.</span><span class="sxs-lookup"><span data-stu-id="41103-214">Log in to your **ITRP** tenant.</span></span>

2. <span data-ttu-id="41103-215">Klicka på i verktygsfältet högst upp **poster**.</span><span class="sxs-lookup"><span data-stu-id="41103-215">In the toolbar on the top, click **Records**.</span></span>
   
    <span data-ttu-id="41103-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="41103-216">![Admin](./media/active-directory-saas-itrp-tutorial/ic775575.png "Admin")</span></span>

3. <span data-ttu-id="41103-217">Popup-menyn väljer du **personer**.</span><span class="sxs-lookup"><span data-stu-id="41103-217">From the popup menu, select **People**.</span></span>
   
    <span data-ttu-id="41103-218">![Personer](./media/active-directory-saas-itrp-tutorial/ic775587.png "personer")</span><span class="sxs-lookup"><span data-stu-id="41103-218">![People](./media/active-directory-saas-itrp-tutorial/ic775587.png "People")</span></span>

4. <span data-ttu-id="41103-219">Klicka på **Lägg till ny Person** (”+”).</span><span class="sxs-lookup"><span data-stu-id="41103-219">Click **Add New Person** (“+”).</span></span>
   
    <span data-ttu-id="41103-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="41103-220">![Admin](./media/active-directory-saas-itrp-tutorial/ic775576.png "Admin")</span></span>

5. <span data-ttu-id="41103-221">I dialogrutan Lägg till ny Person att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="41103-221">On the Add New Person dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="41103-222">![Användaren](./media/active-directory-saas-itrp-tutorial/ic775577.png "användare")</span><span class="sxs-lookup"><span data-stu-id="41103-222">![User](./media/active-directory-saas-itrp-tutorial/ic775577.png "User")</span></span> 
      
    <span data-ttu-id="41103-223">a.</span><span class="sxs-lookup"><span data-stu-id="41103-223">a.</span></span> <span data-ttu-id="41103-224">Typ av **namn**, **e-post** av en giltig AAD-konto som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="41103-224">Type the **Name**, **Email** of a valid AAD account you want to provision.</span></span>

    <span data-ttu-id="41103-225">b.</span><span class="sxs-lookup"><span data-stu-id="41103-225">b.</span></span> <span data-ttu-id="41103-226">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="41103-226">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="41103-227">Du kan använda något annat ITRP användarens konto skapas verktyg eller API: er som tillhandahålls av ITRP att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="41103-227">You can use any other ITRP user account creation tools or APIs provided by ITRP to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="41103-228">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="41103-228">Assigning the Azure AD test user</span></span>

<span data-ttu-id="41103-229">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ITRP.</span><span class="sxs-lookup"><span data-stu-id="41103-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ITRP.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="41103-231">**Om du vill tilldela ITRP Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="41103-231">**To assign Britta Simon to ITRP, perform the following steps:**</span></span>

1. <span data-ttu-id="41103-232">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="41103-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="41103-234">Välj i listan med program **ITRP**.</span><span class="sxs-lookup"><span data-stu-id="41103-234">In the applications list, select **ITRP**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-itrp-tutorial/tutorial_itrp_app.png) 

3. <span data-ttu-id="41103-236">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="41103-236">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="41103-238">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="41103-238">Click **Add** button.</span></span> <span data-ttu-id="41103-239">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="41103-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="41103-241">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="41103-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="41103-242">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="41103-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="41103-243">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="41103-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="41103-244">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="41103-244">Testing single sign-on</span></span>

<span data-ttu-id="41103-245">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="41103-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="41103-246">När du klickar på panelen ITRP på åtkomstpanelen du bör få automatiskt loggat in på ditt ITRP program.</span><span class="sxs-lookup"><span data-stu-id="41103-246">When you click the ITRP tile in the Access Panel, you should get automatically signed-on to your ITRP application.</span></span>
<span data-ttu-id="41103-247">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="41103-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41103-248">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="41103-248">Additional resources</span></span>

* [<span data-ttu-id="41103-249">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="41103-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="41103-250">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="41103-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-itrp-tutorial/tutorial_general_203.png

