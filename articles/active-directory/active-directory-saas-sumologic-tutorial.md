---
title: "Självstudier: Azure Active Directory-integrering med SumoLogic | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SumoLogic."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: e739106472ccf930b2942eb810dd844f2b1ade7c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a><span data-ttu-id="3b15d-103">Självstudier: Azure Active Directory-integrering med SumoLogic</span><span class="sxs-lookup"><span data-stu-id="3b15d-103">Tutorial: Azure Active Directory integration with SumoLogic</span></span>

<span data-ttu-id="3b15d-104">I kursen får lära du att integrera SumoLogic med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3b15d-104">In this tutorial, you learn how to integrate SumoLogic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3b15d-105">Integrera SumoLogic med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3b15d-105">Integrating SumoLogic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3b15d-106">Du kan styra i Azure AD som har åtkomst till SumoLogic</span><span class="sxs-lookup"><span data-stu-id="3b15d-106">You can control in Azure AD who has access to SumoLogic</span></span>
- <span data-ttu-id="3b15d-107">Du kan aktivera användarna att automatiskt hämta loggat in på SumoLogic (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3b15d-107">You can enable your users to automatically get signed-on to SumoLogic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3b15d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3b15d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3b15d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3b15d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b15d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3b15d-110">Prerequisites</span></span>

<span data-ttu-id="3b15d-111">För att konfigurera Azure AD-integrering med SumoLogic, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3b15d-111">To configure Azure AD integration with SumoLogic, you need the following items:</span></span>

- <span data-ttu-id="3b15d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3b15d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3b15d-113">En SumoLogic enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3b15d-113">A SumoLogic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3b15d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3b15d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3b15d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3b15d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3b15d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3b15d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3b15d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3b15d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3b15d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3b15d-118">Scenario description</span></span>
<span data-ttu-id="3b15d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3b15d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3b15d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3b15d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3b15d-121">Att lägga till SumoLogic från galleriet</span><span class="sxs-lookup"><span data-stu-id="3b15d-121">Adding SumoLogic from the gallery</span></span>
2. <span data-ttu-id="3b15d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b15d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sumologic-from-the-gallery"></a><span data-ttu-id="3b15d-123">Att lägga till SumoLogic från galleriet</span><span class="sxs-lookup"><span data-stu-id="3b15d-123">Adding SumoLogic from the gallery</span></span>
<span data-ttu-id="3b15d-124">Du måste lägga till SumoLogic från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SumoLogic i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3b15d-124">To configure the integration of SumoLogic into Azure AD, you need to add SumoLogic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3b15d-125">**Utför följande steg för att lägga till SumoLogic från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3b15d-125">**To add SumoLogic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3b15d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3b15d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3b15d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3b15d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3b15d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b15d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3b15d-133">I sökrutan skriver **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-133">In the search box, type **SumoLogic**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_search.png)

5. <span data-ttu-id="3b15d-135">Välj i resultatpanelen **SumoLogic**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3b15d-135">In the results panel, select **SumoLogic**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3b15d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b15d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3b15d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SumoLogic baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3b15d-138">In this section, you configure and test Azure AD single sign-on with SumoLogic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3b15d-139">Azure AD måste du känna till användaren i SumoLogic motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3b15d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SumoLogic is to a user in Azure AD.</span></span> <span data-ttu-id="3b15d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SumoLogic upprättas.</span><span class="sxs-lookup"><span data-stu-id="3b15d-140">In other words, a link relationship between an Azure AD user and the related user in SumoLogic needs to be established.</span></span>

<span data-ttu-id="3b15d-141">I SumoLogic, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3b15d-141">In SumoLogic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3b15d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SumoLogic, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3b15d-142">To configure and test Azure AD single sign-on with SumoLogic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3b15d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3b15d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3b15d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b15d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3b15d-145">**[Skapa en testanvändare SumoLogic](#creating-a-sumologic-test-user)**  – du har en motsvarighet för Britta Simon i SumoLogic som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3b15d-145">**[Creating a SumoLogic test user](#creating-a-sumologic-test-user)** - to have a counterpart of Britta Simon in SumoLogic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3b15d-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3b15d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3b15d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3b15d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3b15d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b15d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3b15d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt SumoLogic program.</span><span class="sxs-lookup"><span data-stu-id="3b15d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SumoLogic application.</span></span>

<span data-ttu-id="3b15d-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SumoLogic:**</span><span class="sxs-lookup"><span data-stu-id="3b15d-150">**To configure Azure AD single sign-on with SumoLogic, perform the following steps:**</span></span>

1. <span data-ttu-id="3b15d-151">I Azure-portalen på den **SumoLogic** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-151">In the Azure portal, on the **SumoLogic** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3b15d-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3b15d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_samlbase.png)

3. <span data-ttu-id="3b15d-155">På den **SumoLogic domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b15d-155">On the **SumoLogic Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_url.png)

    <span data-ttu-id="3b15d-157">a.</span><span class="sxs-lookup"><span data-stu-id="3b15d-157">a.</span></span> <span data-ttu-id="3b15d-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenantname>.SumoLogic.com`</span><span class="sxs-lookup"><span data-stu-id="3b15d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.SumoLogic.com`</span></span>

    <span data-ttu-id="3b15d-159">b.</span><span class="sxs-lookup"><span data-stu-id="3b15d-159">b.</span></span> <span data-ttu-id="3b15d-160">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="3b15d-160">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > <span data-ttu-id="3b15d-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3b15d-161">These values are not real.</span></span> <span data-ttu-id="3b15d-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="3b15d-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3b15d-163">Kontakta [SumoLogic klienten supportteamet](https://www.sumologic.com/contact-us/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3b15d-163">Contact [SumoLogic Client support team](https://www.sumologic.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="3b15d-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3b15d-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_certificate.png) 

5. <span data-ttu-id="3b15d-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3b15d-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3b15d-168">På den **SumoLogic Configuration** klickar du på **konfigurera SumoLogic** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3b15d-168">On the **SumoLogic Configuration** section, click **Configure SumoLogic** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3b15d-169">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3b15d-169">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_configure.png) 

7. <span data-ttu-id="3b15d-171">I en annan webbläsarfönster loggar du in på webbplatsen SumoLogic företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="3b15d-171">In a different web browser window, log in to your SumoLogic company site as an administrator.</span></span>

8. <span data-ttu-id="3b15d-172">Gå till **hantera \> säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-172">Go to **Manage \> Security**.</span></span>
   
    <span data-ttu-id="3b15d-173">![Hantera](./media/active-directory-saas-sumologic-tutorial/ic778556.png "hantera")</span><span class="sxs-lookup"><span data-stu-id="3b15d-173">![Manage](./media/active-directory-saas-sumologic-tutorial/ic778556.png "Manage")</span></span>

9. <span data-ttu-id="3b15d-174">Klicka på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-174">Click **SAML**.</span></span>
   
    <span data-ttu-id="3b15d-175">![Globala säkerhetsinställningar](./media/active-directory-saas-sumologic-tutorial/ic778557.png "globala säkerhetsinställningar")</span><span class="sxs-lookup"><span data-stu-id="3b15d-175">![Global security settings](./media/active-directory-saas-sumologic-tutorial/ic778557.png "Global security settings")</span></span>

10. <span data-ttu-id="3b15d-176">Från den **väljer en konfiguration eller skapa en ny** väljer **Azure AD**, och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-176">From the **Select a configuration or create a new one** list, select **Azure AD**, and then click **Configure**.</span></span>
   
    <span data-ttu-id="3b15d-177">![Konfigurera SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "konfigurera SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="3b15d-177">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778558.png "Configure SAML 2.0")</span></span>

11. <span data-ttu-id="3b15d-178">På den **konfigurera SAML 2.0** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b15d-178">On the **Configure SAML 2.0** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="3b15d-179">![Konfigurera SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "konfigurera SAML 2.0")</span><span class="sxs-lookup"><span data-stu-id="3b15d-179">![Configure SAML 2.0](./media/active-directory-saas-sumologic-tutorial/ic778559.png "Configure SAML 2.0")</span></span>
   
    <span data-ttu-id="3b15d-180">a.</span><span class="sxs-lookup"><span data-stu-id="3b15d-180">a.</span></span> <span data-ttu-id="3b15d-181">I den **Konfigurationsnamnet** textruta typen **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-181">In the **Configuration Name** textbox, type **Azure AD**.</span></span> 

    <span data-ttu-id="3b15d-182">b.</span><span class="sxs-lookup"><span data-stu-id="3b15d-182">b.</span></span> <span data-ttu-id="3b15d-183">Välj **felsökningsläge**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-183">Select **Debug Mode**.</span></span>

    <span data-ttu-id="3b15d-184">c.</span><span class="sxs-lookup"><span data-stu-id="3b15d-184">c.</span></span> <span data-ttu-id="3b15d-185">I den **utfärdaren** textruta klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b15d-185">In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="3b15d-186">d.</span><span class="sxs-lookup"><span data-stu-id="3b15d-186">d.</span></span> <span data-ttu-id="3b15d-187">I den **Authn begära URL** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b15d-187">In the **Authn Request URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="3b15d-188">e.</span><span class="sxs-lookup"><span data-stu-id="3b15d-188">e.</span></span> <span data-ttu-id="3b15d-189">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in hela certifikatet till **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="3b15d-189">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>

    <span data-ttu-id="3b15d-190">f.</span><span class="sxs-lookup"><span data-stu-id="3b15d-190">f.</span></span> <span data-ttu-id="3b15d-191">Som **e-attributet**väljer **Använd SAML ämne**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-191">As **Email Attribute**, select **Use SAML subject**.</span></span>  

    <span data-ttu-id="3b15d-192">g.</span><span class="sxs-lookup"><span data-stu-id="3b15d-192">g.</span></span> <span data-ttu-id="3b15d-193">Välj **SP initierade inloggningen Configuration**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-193">Select **SP initiated Login Configuration**.</span></span>

    <span data-ttu-id="3b15d-194">h.</span><span class="sxs-lookup"><span data-stu-id="3b15d-194">h.</span></span> <span data-ttu-id="3b15d-195">I den **inloggningen sökvägen** textruta typen **Azure** och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-195">In the **Login Path** textbox, type **Azure** and click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="3b15d-196">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3b15d-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3b15d-197">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3b15d-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3b15d-198">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3b15d-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3b15d-199">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3b15d-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="3b15d-200">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3b15d-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3b15d-202">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3b15d-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3b15d-203">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3b15d-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3b15d-205">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3b15d-207">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b15d-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3b15d-209">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b15d-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sumologic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3b15d-211">a.</span><span class="sxs-lookup"><span data-stu-id="3b15d-211">a.</span></span> <span data-ttu-id="3b15d-212">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3b15d-213">b.</span><span class="sxs-lookup"><span data-stu-id="3b15d-213">b.</span></span> <span data-ttu-id="3b15d-214">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3b15d-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3b15d-215">c.</span><span class="sxs-lookup"><span data-stu-id="3b15d-215">c.</span></span> <span data-ttu-id="3b15d-216">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3b15d-217">d.</span><span class="sxs-lookup"><span data-stu-id="3b15d-217">d.</span></span> <span data-ttu-id="3b15d-218">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-218">Click **Create**.</span></span>
 
### <a name="creating-a-sumologic-test-user"></a><span data-ttu-id="3b15d-219">Skapa en testanvändare SumoLogic</span><span class="sxs-lookup"><span data-stu-id="3b15d-219">Creating a SumoLogic test user</span></span>

<span data-ttu-id="3b15d-220">För att aktivera Azure AD-användare kan logga in på SumoLogic etableras de för SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="3b15d-220">In order to enable Azure AD users to log in to SumoLogic, they must be provisioned to SumoLogic.</span></span>  

* <span data-ttu-id="3b15d-221">När det gäller SumoLogic är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="3b15d-221">In the case of SumoLogic, provisioning is a manual task.</span></span>

<span data-ttu-id="3b15d-222">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="3b15d-222">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="3b15d-223">Logga in på ditt **SumoLogic** klient.</span><span class="sxs-lookup"><span data-stu-id="3b15d-223">Log in to your **SumoLogic** tenant.</span></span>

2. <span data-ttu-id="3b15d-224">Gå till **hantera \> användare**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-224">Go to **Manage \> Users**.</span></span>
   
    <span data-ttu-id="3b15d-225">![Användare](./media/active-directory-saas-sumologic-tutorial/ic778561.png "användare")</span><span class="sxs-lookup"><span data-stu-id="3b15d-225">![Users](./media/active-directory-saas-sumologic-tutorial/ic778561.png "Users")</span></span>

3. <span data-ttu-id="3b15d-226">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-226">Click **Add**.</span></span>
   
    <span data-ttu-id="3b15d-227">![Användare](./media/active-directory-saas-sumologic-tutorial/ic778562.png "användare")</span><span class="sxs-lookup"><span data-stu-id="3b15d-227">![Users](./media/active-directory-saas-sumologic-tutorial/ic778562.png "Users")</span></span>

4. <span data-ttu-id="3b15d-228">På den **ny användare** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3b15d-228">On the **New User** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="3b15d-229">![Ny användare](./media/active-directory-saas-sumologic-tutorial/ic778563.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="3b15d-229">![New User](./media/active-directory-saas-sumologic-tutorial/ic778563.png "New User")</span></span> 
 
    <span data-ttu-id="3b15d-230">a.</span><span class="sxs-lookup"><span data-stu-id="3b15d-230">a.</span></span> <span data-ttu-id="3b15d-231">Skriv relaterad information av Azure AD-kontot som du vill etablera i den **Förnamn**, **efternamn**, och **e-post** textrutor.</span><span class="sxs-lookup"><span data-stu-id="3b15d-231">Type the related information of the Azure AD account you want to provision into the **First Name**, **Last Name**, and **Email** textboxes.</span></span>
  
    <span data-ttu-id="3b15d-232">b.</span><span class="sxs-lookup"><span data-stu-id="3b15d-232">b.</span></span> <span data-ttu-id="3b15d-233">Välj en roll.</span><span class="sxs-lookup"><span data-stu-id="3b15d-233">Select a role.</span></span>
  
    <span data-ttu-id="3b15d-234">c.</span><span class="sxs-lookup"><span data-stu-id="3b15d-234">c.</span></span> <span data-ttu-id="3b15d-235">Som **Status**väljer **Active**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-235">As **Status**, select **Active**.</span></span>
  
    <span data-ttu-id="3b15d-236">d.</span><span class="sxs-lookup"><span data-stu-id="3b15d-236">d.</span></span> <span data-ttu-id="3b15d-237">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-237">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="3b15d-238">Du kan använda något annat SumoLogic användarens konto skapas verktyg eller API: er som tillhandahålls av SumoLogic att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="3b15d-238">You can use any other SumoLogic user account creation tools or APIs provided by SumoLogic to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3b15d-239">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3b15d-239">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3b15d-240">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SumoLogic.</span><span class="sxs-lookup"><span data-stu-id="3b15d-240">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SumoLogic.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3b15d-242">**Om du vill tilldela SumoLogic Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3b15d-242">**To assign Britta Simon to SumoLogic, perform the following steps:**</span></span>

1. <span data-ttu-id="3b15d-243">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-243">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3b15d-245">Välj i listan med program **SumoLogic**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-245">In the applications list, select **SumoLogic**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sumologic-tutorial/tutorial_sumologic_app.png) 

3. <span data-ttu-id="3b15d-247">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3b15d-247">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3b15d-249">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3b15d-249">Click **Add** button.</span></span> <span data-ttu-id="3b15d-250">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b15d-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3b15d-252">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3b15d-252">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3b15d-253">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b15d-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3b15d-254">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3b15d-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3b15d-255">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3b15d-255">Testing single sign-on</span></span>

<span data-ttu-id="3b15d-256">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3b15d-256">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3b15d-257">När du klickar på panelen SumoLogic på åtkomstpanelen du bör få automatiskt loggat in på ditt SumoLogic program.</span><span class="sxs-lookup"><span data-stu-id="3b15d-257">When you click the SumoLogic tile in the Access Panel, you should get automatically signed-on to your SumoLogic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b15d-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3b15d-258">Additional resources</span></span>

* [<span data-ttu-id="3b15d-259">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3b15d-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3b15d-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3b15d-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sumologic-tutorial/tutorial_general_203.png

