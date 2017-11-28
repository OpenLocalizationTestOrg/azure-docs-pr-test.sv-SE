---
title: "Självstudier: Azure Active Directory-integrering med en Zscaler | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och en Zscaler."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 7d655c482a16c991a819eec84c84556d2f288a75
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-one"></a><span data-ttu-id="b4a67-103">Självstudier: Azure Active Directory-integrering med en Zscaler</span><span class="sxs-lookup"><span data-stu-id="b4a67-103">Tutorial: Azure Active Directory integration with Zscaler One</span></span>

<span data-ttu-id="b4a67-104">Lär dig hur du integrerar en Zscaler med Azure Active Directory (AD Azure) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="b4a67-104">In this tutorial, you learn how to integrate Zscaler One with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b4a67-105">Integrera en Zscaler med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b4a67-105">Integrating Zscaler One with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b4a67-106">Du kan styra i Azure AD som har åtkomst till en Zscaler</span><span class="sxs-lookup"><span data-stu-id="b4a67-106">You can control in Azure AD who has access to Zscaler One</span></span>
- <span data-ttu-id="b4a67-107">Du kan aktivera användarna att automatiskt hämta loggat in på en Zscaler (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b4a67-107">You can enable your users to automatically get signed-on to Zscaler One (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b4a67-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b4a67-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b4a67-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b4a67-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4a67-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b4a67-110">Prerequisites</span></span>

<span data-ttu-id="b4a67-111">För att konfigurera Azure AD-integrering med en Zscaler, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b4a67-111">To configure Azure AD integration with Zscaler One, you need the following items:</span></span>

- <span data-ttu-id="b4a67-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b4a67-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b4a67-113">En Zscaler en enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b4a67-113">A Zscaler One single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b4a67-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b4a67-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b4a67-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b4a67-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b4a67-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b4a67-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b4a67-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b4a67-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b4a67-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="b4a67-118">Scenario description</span></span>
<span data-ttu-id="b4a67-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b4a67-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b4a67-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b4a67-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b4a67-121">Att lägga till en Zscaler från galleriet</span><span class="sxs-lookup"><span data-stu-id="b4a67-121">Adding Zscaler One from the gallery</span></span>
2. <span data-ttu-id="b4a67-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4a67-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-one-from-the-gallery"></a><span data-ttu-id="b4a67-123">Att lägga till en Zscaler från galleriet</span><span class="sxs-lookup"><span data-stu-id="b4a67-123">Adding Zscaler One from the gallery</span></span>
<span data-ttu-id="b4a67-124">Du måste lägga till en Zscaler från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av en Zscaler i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b4a67-124">To configure the integration of Zscaler One into Azure AD, you need to add Zscaler One from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b4a67-125">**Om du vill lägga till en Zscaler från galleriet, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b4a67-125">**To add Zscaler One from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b4a67-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b4a67-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b4a67-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b4a67-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="b4a67-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="b4a67-133">I sökrutan skriver **Zscaler en**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-133">In the search box, type **Zscaler One**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_search.png)

5. <span data-ttu-id="b4a67-135">Välj i resultatpanelen **Zscaler en**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b4a67-135">In the results panel, select **Zscaler One**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b4a67-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4a67-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b4a67-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Zscaler en baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b4a67-138">In this section, you configure and test Azure AD single sign-on with Zscaler One based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b4a67-139">Azure AD måste du känna till användaren i Zscaler en motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="b4a67-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler One is to a user in Azure AD.</span></span> <span data-ttu-id="b4a67-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i en Zscaler upprättas.</span><span class="sxs-lookup"><span data-stu-id="b4a67-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler One needs to be established.</span></span>

<span data-ttu-id="b4a67-141">I en Zscaler, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="b4a67-141">In Zscaler One, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b4a67-142">Om du vill konfigurera och testa Azure AD enkel inloggning med en Zscaler, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b4a67-142">To configure and test Azure AD single sign-on with Zscaler One, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b4a67-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b4a67-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b4a67-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  - om du vill konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b4a67-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="b4a67-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b4a67-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="b4a67-146">**[Skapa en Zscaler en testanvändare](#creating-a-zscaler-one-test-user)**  – du har en motsvarighet för Britta Simon i Zscaler en som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="b4a67-146">**[Creating a Zscaler One test user](#creating-a-zscaler-one-test-user)** - to have a counterpart of Britta Simon in Zscaler One that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="b4a67-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b4a67-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="b4a67-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b4a67-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b4a67-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4a67-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b4a67-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Zscaler ett program.</span><span class="sxs-lookup"><span data-stu-id="b4a67-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler One application.</span></span>

<span data-ttu-id="b4a67-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med en Zscaler:**</span><span class="sxs-lookup"><span data-stu-id="b4a67-151">**To configure Azure AD single sign-on with Zscaler One, perform the following steps:**</span></span>

1. <span data-ttu-id="b4a67-152">I Azure-portalen på den **Zscaler en** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-152">In the Azure portal, on the **Zscaler One** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="b4a67-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b4a67-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_samlbase.png)

3. <span data-ttu-id="b4a67-156">På den **Zscaler en domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b4a67-156">On the **Zscaler One Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_url.png)

    <span data-ttu-id="b4a67-158">Skriv den URL som används av dina användare logga in i tillämpningsprogrammet Zscaler en i textrutan inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="b4a67-158">In the Sign-on URL textbox, type the URL used by your users to sign-on to your Zscaler One application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b4a67-159">Du måste uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="b4a67-159">You have to update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="b4a67-160">Kontakta [Zscaler en klient supportteamet](https://www.zscaler.com/company/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="b4a67-160">Contact [Zscaler One Client support team](https://www.zscaler.com/company/contact) to get these values.</span></span>

4. <span data-ttu-id="b4a67-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b4a67-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_certificate.png) 

5. <span data-ttu-id="b4a67-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="b4a67-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b4a67-165">På den **Zscaler en konfiguration** klickar du på **konfigurera Zscaler en** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="b4a67-165">On the **Zscaler One Configuration** section, click **Configure Zscaler One** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b4a67-166">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="b4a67-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_configure.png) 

7. <span data-ttu-id="b4a67-168">I en annan webbläsarfönster loggar du in på webbplatsen Zscaler ett företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="b4a67-168">In a different web browser window, log in to your Zscaler One company site as an administrator.</span></span>

8. <span data-ttu-id="b4a67-169">Klicka på menyn högst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-169">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="b4a67-170">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="b4a67-170">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="b4a67-171">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="b4a67-172">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="b4a67-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="b4a67-173">I den **Välj autentiseringsalternativ för din organisation** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b4a67-173">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="b4a67-174">![Autentisering](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="b4a67-174">![Authentication](./media/active-directory-saas-zscaler-one-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="b4a67-175">a.</span><span class="sxs-lookup"><span data-stu-id="b4a67-175">a.</span></span> <span data-ttu-id="b4a67-176">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="b4a67-177">b.</span><span class="sxs-lookup"><span data-stu-id="b4a67-177">b.</span></span> <span data-ttu-id="b4a67-178">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="b4a67-179">På den **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="b4a67-179">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="b4a67-180">![Enkel inloggning](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="b4a67-180">![Single Sign-On](./media/active-directory-saas-zscaler-one-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="b4a67-181">a.</span><span class="sxs-lookup"><span data-stu-id="b4a67-181">a.</span></span> <span data-ttu-id="b4a67-182">Klistra in den **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat från Azure-portalen i den **URL för SAML-portalen som användare om autentisering skickas** textruta.</span><span class="sxs-lookup"><span data-stu-id="b4a67-182">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="b4a67-183">b.</span><span class="sxs-lookup"><span data-stu-id="b4a67-183">b.</span></span> <span data-ttu-id="b4a67-184">I den **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-184">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="b4a67-185">c.</span><span class="sxs-lookup"><span data-stu-id="b4a67-185">c.</span></span> <span data-ttu-id="b4a67-186">Om du vill överföra din hämtat certifikat klickar du på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-186">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="b4a67-187">d.</span><span class="sxs-lookup"><span data-stu-id="b4a67-187">d.</span></span> <span data-ttu-id="b4a67-188">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="b4a67-189">På den **konfigurera användarautentisering** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b4a67-189">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="b4a67-190">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="b4a67-190">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="b4a67-191">a.</span><span class="sxs-lookup"><span data-stu-id="b4a67-191">a.</span></span> <span data-ttu-id="b4a67-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-192">Click **Save**.</span></span>

    <span data-ttu-id="b4a67-193">b.</span><span class="sxs-lookup"><span data-stu-id="b4a67-193">b.</span></span> <span data-ttu-id="b4a67-194">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="b4a67-195">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="b4a67-195">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="b4a67-196">Konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b4a67-196">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="b4a67-197">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="b4a67-198">Välj **Internetalternativ** från den **verktyg** menyn för att öppna den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-198">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="b4a67-199">![Internet-alternativ](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="b4a67-199">![Internet Options](./media/active-directory-saas-zscaler-one-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="b4a67-200">Klicka på den **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="b4a67-200">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="b4a67-201">![Anslutningar](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="b4a67-201">![Connections](./media/active-directory-saas-zscaler-one-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="b4a67-202">Klicka på **LAN-inställningar** att öppna den **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-202">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="b4a67-203">Utför följande steg i avsnittet Proxy-server:</span><span class="sxs-lookup"><span data-stu-id="b4a67-203">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="b4a67-204">![Proxyserver](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="b4a67-204">![Proxy server](./media/active-directory-saas-zscaler-one-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="b4a67-205">a.</span><span class="sxs-lookup"><span data-stu-id="b4a67-205">a.</span></span> <span data-ttu-id="b4a67-206">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="b4a67-207">b.</span><span class="sxs-lookup"><span data-stu-id="b4a67-207">b.</span></span> <span data-ttu-id="b4a67-208">Ange i textrutan adress **gateway.zscalerone.net**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-208">In the Address textbox, type **gateway.zscalerone.net**.</span></span>

    <span data-ttu-id="b4a67-209">c.</span><span class="sxs-lookup"><span data-stu-id="b4a67-209">c.</span></span> <span data-ttu-id="b4a67-210">Ange i textrutan Port **80**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-210">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="b4a67-211">d.</span><span class="sxs-lookup"><span data-stu-id="b4a67-211">d.</span></span> <span data-ttu-id="b4a67-212">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="b4a67-213">e.</span><span class="sxs-lookup"><span data-stu-id="b4a67-213">e.</span></span> <span data-ttu-id="b4a67-214">Klicka på **OK** att stänga den **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-214">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="b4a67-215">Klicka på **OK** att stänga den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-215">Click **OK** to close the **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="b4a67-216">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="b4a67-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b4a67-217">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="b4a67-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b4a67-218">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b4a67-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b4a67-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b4a67-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="b4a67-220">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b4a67-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="b4a67-222">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b4a67-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b4a67-223">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="b4a67-223">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b4a67-225">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-225">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b4a67-227">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-227">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b4a67-229">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b4a67-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-one-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b4a67-231">a.</span><span class="sxs-lookup"><span data-stu-id="b4a67-231">a.</span></span> <span data-ttu-id="b4a67-232">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b4a67-233">b.</span><span class="sxs-lookup"><span data-stu-id="b4a67-233">b.</span></span> <span data-ttu-id="b4a67-234">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b4a67-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b4a67-235">c.</span><span class="sxs-lookup"><span data-stu-id="b4a67-235">c.</span></span> <span data-ttu-id="b4a67-236">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b4a67-237">d.</span><span class="sxs-lookup"><span data-stu-id="b4a67-237">d.</span></span> <span data-ttu-id="b4a67-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-one-test-user"></a><span data-ttu-id="b4a67-239">Skapa en Zscaler en testanvändare</span><span class="sxs-lookup"><span data-stu-id="b4a67-239">Creating a Zscaler One test user</span></span>

<span data-ttu-id="b4a67-240">Om du vill aktivera Azure AD-användare kan logga in på en Zscaler, måste de etableras till en Zscaler.</span><span class="sxs-lookup"><span data-stu-id="b4a67-240">To enable Azure AD users to log in to Zscaler One, they must be provisioned to Zscaler One.</span></span> <span data-ttu-id="b4a67-241">När det gäller en Zscaler är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b4a67-241">In the case of Zscaler One, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="b4a67-242">Utför följande steg för att konfigurera användaretablering:</span><span class="sxs-lookup"><span data-stu-id="b4a67-242">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="b4a67-243">Logga in på ditt **Zscaler en** klient.</span><span class="sxs-lookup"><span data-stu-id="b4a67-243">Log in to your **Zscaler One** tenant.</span></span>

2. <span data-ttu-id="b4a67-244">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="b4a67-245">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="b4a67-245">![Administration](./media/active-directory-saas-zscaler-one-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="b4a67-246">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="b4a67-247">![Lägg till](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="b4a67-247">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="b4a67-248">I den **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-248">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="b4a67-249">![Lägg till](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="b4a67-249">![Add](./media/active-directory-saas-zscaler-one-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="b4a67-250">Utför följande steg i avsnittet Lägg till användare:</span><span class="sxs-lookup"><span data-stu-id="b4a67-250">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="b4a67-251">![Lägg till användare](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="b4a67-251">![Add User](./media/active-directory-saas-zscaler-one-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="b4a67-252">a.</span><span class="sxs-lookup"><span data-stu-id="b4a67-252">a.</span></span> <span data-ttu-id="b4a67-253">Typ av **användar-ID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper** och **avdelning** av ett giltigt Azure AD-kontot som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="b4a67-253">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid Azure AD account you want to provision.</span></span>

    <span data-ttu-id="b4a67-254">b.</span><span class="sxs-lookup"><span data-stu-id="b4a67-254">b.</span></span> <span data-ttu-id="b4a67-255">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="b4a67-256">Du kan använda andra Zscaler en användare skapa verktyg och API: er som tillhandahålls av en Zscaler för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b4a67-256">You can use any other Zscaler One user account creation tools or APIs provided by Zscaler One to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b4a67-257">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b4a67-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b4a67-258">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till en Zscaler.</span><span class="sxs-lookup"><span data-stu-id="b4a67-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler One.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b4a67-260">**Om du vill tilldela en Zscaler Britta Simon, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b4a67-260">**To assign Britta Simon to Zscaler One, perform the following steps:**</span></span>

1. <span data-ttu-id="b4a67-261">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="b4a67-263">Välj i listan med program **Zscaler en**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-263">In the applications list, select **Zscaler One**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-one-tutorial/tutorial_zscalerone_app.png) 

3. <span data-ttu-id="b4a67-265">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="b4a67-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="b4a67-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="b4a67-267">Click **Add** button.</span></span> <span data-ttu-id="b4a67-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="b4a67-270">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="b4a67-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b4a67-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b4a67-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b4a67-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b4a67-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b4a67-273">Testing single sign-on</span></span>

<span data-ttu-id="b4a67-274">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b4a67-274">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b4a67-275">När du klickar på en Zscaler panelen på åtkomstpanelen du bör få automatiskt loggat in på ditt Zscaler ett program.</span><span class="sxs-lookup"><span data-stu-id="b4a67-275">When you click the Zscaler One tile in the Access Panel, you should get automatically signed-on to your Zscaler One application.</span></span>
<span data-ttu-id="b4a67-276">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b4a67-276">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b4a67-277">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b4a67-277">Additional resources</span></span>

* [<span data-ttu-id="b4a67-278">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b4a67-278">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b4a67-279">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b4a67-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-one-tutorial/tutorial_general_203.png

