---
title: "Självstudier: Azure Active Directory-integrering med Zscaler Beta | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Zscaler Beta."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 56b846ae-a1e7-45ae-a79d-992a87f075ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 72b4efc6b3bb58e63a399ab26c42984f070d9307
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-beta"></a><span data-ttu-id="a9695-103">Självstudier: Azure Active Directory-integrering med Zscaler Beta</span><span class="sxs-lookup"><span data-stu-id="a9695-103">Tutorial: Azure Active Directory integration with Zscaler Beta</span></span>

<span data-ttu-id="a9695-104">I kursen får lära du att integrera Zscaler Beta med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a9695-104">In this tutorial, you learn how to integrate Zscaler Beta with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a9695-105">Integrera Zscaler Beta med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a9695-105">Integrating Zscaler Beta with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a9695-106">Du kan styra i Azure AD som har åtkomst till Zscaler Beta</span><span class="sxs-lookup"><span data-stu-id="a9695-106">You can control in Azure AD who has access to Zscaler Beta</span></span>
- <span data-ttu-id="a9695-107">Du kan aktivera användarna att automatiskt hämta loggat in på Zscaler Beta (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a9695-107">You can enable your users to automatically get signed-on to Zscaler Beta (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a9695-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a9695-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a9695-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a9695-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a9695-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a9695-110">Prerequisites</span></span>

<span data-ttu-id="a9695-111">För att konfigurera Azure AD-integrering med Zscaler Beta, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a9695-111">To configure Azure AD integration with Zscaler Beta, you need the following items:</span></span>

- <span data-ttu-id="a9695-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a9695-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a9695-113">En Zscaler Beta enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="a9695-113">A Zscaler Beta single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a9695-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a9695-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a9695-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a9695-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a9695-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a9695-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a9695-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a9695-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a9695-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a9695-118">Scenario description</span></span>
<span data-ttu-id="a9695-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a9695-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a9695-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a9695-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a9695-121">Att lägga till Zscaler Beta från galleriet</span><span class="sxs-lookup"><span data-stu-id="a9695-121">Adding Zscaler Beta from the gallery</span></span>
2. <span data-ttu-id="a9695-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9695-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zscaler-beta-from-the-gallery"></a><span data-ttu-id="a9695-123">Att lägga till Zscaler Beta från galleriet</span><span class="sxs-lookup"><span data-stu-id="a9695-123">Adding Zscaler Beta from the gallery</span></span>
<span data-ttu-id="a9695-124">Du måste lägga till Zscaler Beta från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Zscaler Beta i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a9695-124">To configure the integration of Zscaler Beta into Azure AD, you need to add Zscaler Beta from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a9695-125">**Utför följande steg för att lägga till Zscaler Beta från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a9695-125">**To add Zscaler Beta from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a9695-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a9695-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a9695-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a9695-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a9695-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a9695-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a9695-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a9695-133">I sökrutan skriver **Zscaler Beta**.</span><span class="sxs-lookup"><span data-stu-id="a9695-133">In the search box, type **Zscaler Beta**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_search.png)

5. <span data-ttu-id="a9695-135">Välj i resultatpanelen **Zscaler Beta**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a9695-135">In the results panel, select **Zscaler Beta**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a9695-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9695-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a9695-138">Du konfigurera och testa Azure AD enkel inloggning med Zscaler Beta baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a9695-138">In this section, you configure and test Azure AD single sign-on with Zscaler Beta based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a9695-139">Azure AD måste du känna till användaren i Zscaler Beta motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a9695-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Zscaler Beta is to a user in Azure AD.</span></span> <span data-ttu-id="a9695-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zscaler Beta upprättas.</span><span class="sxs-lookup"><span data-stu-id="a9695-140">In other words, a link relationship between an Azure AD user and the related user in Zscaler Beta needs to be established.</span></span>

<span data-ttu-id="a9695-141">I Zscaler Beta, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="a9695-141">In Zscaler Beta, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a9695-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Zscaler Beta, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a9695-142">To configure and test Azure AD single sign-on with Zscaler Beta, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a9695-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a9695-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a9695-144">**[Konfigurera proxyinställningar](#configuring-proxy-settings)**  - om du vill konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="a9695-144">**[Configuring proxy settings](#configuring-proxy-settings)** - to configure the proxy settings in Internet Explorer</span></span>
3. <span data-ttu-id="a9695-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a9695-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="a9695-146">**[Skapa en testanvändare Zscaler Beta](#creating-a-zscaler-beta-test-user)**  – du har en motsvarighet för Britta Simon i betaversionen av Zscaler som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="a9695-146">**[Creating a Zscaler Beta test user](#creating-a-zscaler-beta-test-user)** - to have a counterpart of Britta Simon in Zscaler Beta that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="a9695-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a9695-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="a9695-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a9695-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a9695-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9695-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a9695-150">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Zscaler Beta-program.</span><span class="sxs-lookup"><span data-stu-id="a9695-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zscaler Beta application.</span></span>

<span data-ttu-id="a9695-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Zscaler Beta:**</span><span class="sxs-lookup"><span data-stu-id="a9695-151">**To configure Azure AD single sign-on with Zscaler Beta, perform the following steps:**</span></span>

1. <span data-ttu-id="a9695-152">I Azure-portalen på den **Zscaler Beta** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a9695-152">In the Azure portal, on the **Zscaler Beta** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a9695-154">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a9695-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_samlbase.png)

3. <span data-ttu-id="a9695-156">På den **Zscaler Beta domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a9695-156">On the **Zscaler Beta Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_url.png)

    <span data-ttu-id="a9695-158">Skriv den URL som används av dina användare logga in i tillämpningsprogrammet Zscaler Beta i textrutan inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a9695-158">In the Sign-on URL textbox, type the URL used by your users to sign-on to your Zscaler Beta application.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a9695-159">Du måste uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a9695-159">You have to update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="a9695-160">Kontakta [Zscaler Betaklienten supportteamet](https://www.zscaler.com/company/contact) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="a9695-160">Contact [Zscaler Beta Client support team](https://www.zscaler.com/company/contact) to get this value.</span></span> 

4. <span data-ttu-id="a9695-161">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="a9695-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_certificate.png) 

5. <span data-ttu-id="a9695-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a9695-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a9695-165">På den **Zscaler Beta Configuration** klickar du på **konfigurera Zscaler Beta** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="a9695-165">On the **Zscaler Beta Configuration** section, click **Configure Zscaler Beta** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a9695-166">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="a9695-166">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_configure.png) 

7. <span data-ttu-id="a9695-168">I en annan webbläsarfönster loggar du in på webbplatsen Zscaler Beta företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="a9695-168">In a different web browser window, log in to your Zscaler Beta company site as an administrator.</span></span>

8. <span data-ttu-id="a9695-169">Klicka på menyn högst upp **Administration**.</span><span class="sxs-lookup"><span data-stu-id="a9695-169">In the menu on the top, click **Administration**.</span></span>
   
    <span data-ttu-id="a9695-170">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="a9695-170">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800206.png "Administration")</span></span>

9. <span data-ttu-id="a9695-171">Under **hantera administratörer & roller**, klickar du på **hantera användare och autentisering**.</span><span class="sxs-lookup"><span data-stu-id="a9695-171">Under **Manage Administrators & Roles**, click **Manage Users & Authentication**.</span></span>   
            
    <span data-ttu-id="a9695-172">![Hantera användare och autentisering](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "hantera användare och autentisering")</span><span class="sxs-lookup"><span data-stu-id="a9695-172">![Manage Users & Authentication](./media/active-directory-saas-zscaler-beta-tutorial/ic800207.png "Manage Users & Authentication")</span></span>

10. <span data-ttu-id="a9695-173">I den **Välj autentiseringsalternativ för din organisation** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a9695-173">In the **Choose Authentication Options for your Organization** section, perform the following steps:</span></span>   
                
    <span data-ttu-id="a9695-174">![Autentisering](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "autentisering")</span><span class="sxs-lookup"><span data-stu-id="a9695-174">![Authentication](./media/active-directory-saas-zscaler-beta-tutorial/ic800208.png "Authentication")</span></span>
   
    <span data-ttu-id="a9695-175">a.</span><span class="sxs-lookup"><span data-stu-id="a9695-175">a.</span></span> <span data-ttu-id="a9695-176">Välj **autentisera med SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a9695-176">Select **Authenticate using SAML Single Sign-On**.</span></span>

    <span data-ttu-id="a9695-177">b.</span><span class="sxs-lookup"><span data-stu-id="a9695-177">b.</span></span> <span data-ttu-id="a9695-178">Klicka på **konfigureras SAML enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a9695-178">Click **Configure SAML Single Sign-On Parameters**.</span></span>

11. <span data-ttu-id="a9695-179">På den **konfigurera SAML enkel inloggning parametrar** dialogrutan sida, utför följande steg och klicka sedan på **klar**</span><span class="sxs-lookup"><span data-stu-id="a9695-179">On the **Configure SAML Single Sign-On Parameters** dialog page, perform the following steps, and then click **Done**</span></span>

    <span data-ttu-id="a9695-180">![Enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a9695-180">![Single Sign-On](./media/active-directory-saas-zscaler-beta-tutorial/ic800209.png "Single Sign-On")</span></span>
    
    <span data-ttu-id="a9695-181">a.</span><span class="sxs-lookup"><span data-stu-id="a9695-181">a.</span></span> <span data-ttu-id="a9695-182">Klistra in den **SAML inloggning tjänst-URL för enkel** -värde som du har kopierat från Azure-portalen i den **URL för SAML-portalen som användare om autentisering skickas** textruta.</span><span class="sxs-lookup"><span data-stu-id="a9695-182">Paste the **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **URL of the SAML Portal to which users are sent for authentication** textbox.</span></span>
    
    <span data-ttu-id="a9695-183">b.</span><span class="sxs-lookup"><span data-stu-id="a9695-183">b.</span></span> <span data-ttu-id="a9695-184">I den **attributet som innehåller inloggningsnamnet** textruta typen **NameID**.</span><span class="sxs-lookup"><span data-stu-id="a9695-184">In the **Attribute containing Login Name** textbox, type **NameID**.</span></span>
    
    <span data-ttu-id="a9695-185">c.</span><span class="sxs-lookup"><span data-stu-id="a9695-185">c.</span></span> <span data-ttu-id="a9695-186">Om du vill överföra din hämtat certifikat klickar du på **Zscaler pem**.</span><span class="sxs-lookup"><span data-stu-id="a9695-186">To upload your downloaded certificate, click **Zscaler pem**.</span></span>
    
    <span data-ttu-id="a9695-187">d.</span><span class="sxs-lookup"><span data-stu-id="a9695-187">d.</span></span> <span data-ttu-id="a9695-188">Välj **aktivera etablering av SAML-automatiskt**.</span><span class="sxs-lookup"><span data-stu-id="a9695-188">Select **Enable SAML Auto-Provisioning**.</span></span>

12. <span data-ttu-id="a9695-189">På den **konfigurera användarautentisering** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a9695-189">On the **Configure User Authentication** dialog page, perform the following steps:</span></span>

    <span data-ttu-id="a9695-190">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="a9695-190">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic800210.png "Administration")</span></span>
    
    <span data-ttu-id="a9695-191">a.</span><span class="sxs-lookup"><span data-stu-id="a9695-191">a.</span></span> <span data-ttu-id="a9695-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a9695-192">Click **Save**.</span></span>

    <span data-ttu-id="a9695-193">b.</span><span class="sxs-lookup"><span data-stu-id="a9695-193">b.</span></span> <span data-ttu-id="a9695-194">Klicka på **aktivera nu**.</span><span class="sxs-lookup"><span data-stu-id="a9695-194">Click **Activate Now**.</span></span>

## <a name="configuring-proxy-settings"></a><span data-ttu-id="a9695-195">Konfigurera proxyinställningar</span><span class="sxs-lookup"><span data-stu-id="a9695-195">Configuring proxy settings</span></span>
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a><span data-ttu-id="a9695-196">Konfigurera proxyinställningarna i Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="a9695-196">To configure the proxy settings in Internet Explorer</span></span>

1. <span data-ttu-id="a9695-197">Starta **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="a9695-197">Start **Internet Explorer**.</span></span>

2. <span data-ttu-id="a9695-198">Välj **Internetalternativ** från den **verktyg** menyn för att öppna den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-198">Select **Internet options** from the **Tools** menu for open the **Internet Options** dialog.</span></span>   
    
     <span data-ttu-id="a9695-199">![Internet-alternativ](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Internet-alternativ")</span><span class="sxs-lookup"><span data-stu-id="a9695-199">![Internet Options](./media/active-directory-saas-zscaler-beta-tutorial/ic769492.png "Internet Options")</span></span>

3. <span data-ttu-id="a9695-200">Klicka på den **anslutningar** fliken.</span><span class="sxs-lookup"><span data-stu-id="a9695-200">Click the **Connections** tab.</span></span>   
  
     <span data-ttu-id="a9695-201">![Anslutningar](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "anslutningar")</span><span class="sxs-lookup"><span data-stu-id="a9695-201">![Connections](./media/active-directory-saas-zscaler-beta-tutorial/ic769493.png "Connections")</span></span>

4. <span data-ttu-id="a9695-202">Klicka på **LAN-inställningar** att öppna den **LAN-inställningar** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-202">Click **LAN settings** to open the **LAN Settings** dialog.</span></span>

5. <span data-ttu-id="a9695-203">Utför följande steg i avsnittet Proxy-server:</span><span class="sxs-lookup"><span data-stu-id="a9695-203">In the Proxy server section, perform the following steps:</span></span>   
   
    <span data-ttu-id="a9695-204">![Proxyserver](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "proxyserver")</span><span class="sxs-lookup"><span data-stu-id="a9695-204">![Proxy server](./media/active-directory-saas-zscaler-beta-tutorial/ic769494.png "Proxy server")</span></span>

    <span data-ttu-id="a9695-205">a.</span><span class="sxs-lookup"><span data-stu-id="a9695-205">a.</span></span> <span data-ttu-id="a9695-206">Välj **använder en proxyserver för nätverket**.</span><span class="sxs-lookup"><span data-stu-id="a9695-206">Select **Use a proxy server for your LAN**.</span></span>

    <span data-ttu-id="a9695-207">b.</span><span class="sxs-lookup"><span data-stu-id="a9695-207">b.</span></span> <span data-ttu-id="a9695-208">Ange i textrutan adress **gateway.zscalerbeta.net**.</span><span class="sxs-lookup"><span data-stu-id="a9695-208">In the Address textbox, type **gateway.zscalerbeta.net**.</span></span>

    <span data-ttu-id="a9695-209">c.</span><span class="sxs-lookup"><span data-stu-id="a9695-209">c.</span></span> <span data-ttu-id="a9695-210">Ange i textrutan Port **80**.</span><span class="sxs-lookup"><span data-stu-id="a9695-210">In the Port textbox, type **80**.</span></span>

    <span data-ttu-id="a9695-211">d.</span><span class="sxs-lookup"><span data-stu-id="a9695-211">d.</span></span> <span data-ttu-id="a9695-212">Välj **Använd ingen proxyserver för lokala adresser**.</span><span class="sxs-lookup"><span data-stu-id="a9695-212">Select **Bypass proxy server for local addresses**.</span></span>

    <span data-ttu-id="a9695-213">e.</span><span class="sxs-lookup"><span data-stu-id="a9695-213">e.</span></span> <span data-ttu-id="a9695-214">Klicka på **OK** att stänga den **inställningar för lokalt nätverk (LAN)** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-214">Click **OK** to close the **Local Area Network (LAN) Settings** dialog.</span></span>

6. <span data-ttu-id="a9695-215">Klicka på **OK** att stänga den **Internetalternativ** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-215">Click **OK** to close the **Internet Options** dialog.</span></span>

> [!TIP]
> <span data-ttu-id="a9695-216">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="a9695-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a9695-217">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="a9695-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a9695-218">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a9695-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a9695-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a9695-219">Creating an Azure AD test user</span></span>
<span data-ttu-id="a9695-220">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a9695-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a9695-222">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a9695-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a9695-223">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a9695-223">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a9695-225">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="a9695-225">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a9695-227">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-227">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a9695-229">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a9695-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-zscaler-beta-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a9695-231">a.</span><span class="sxs-lookup"><span data-stu-id="a9695-231">a.</span></span> <span data-ttu-id="a9695-232">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a9695-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a9695-233">b.</span><span class="sxs-lookup"><span data-stu-id="a9695-233">b.</span></span> <span data-ttu-id="a9695-234">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a9695-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a9695-235">c.</span><span class="sxs-lookup"><span data-stu-id="a9695-235">c.</span></span> <span data-ttu-id="a9695-236">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a9695-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a9695-237">d.</span><span class="sxs-lookup"><span data-stu-id="a9695-237">d.</span></span> <span data-ttu-id="a9695-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a9695-238">Click **Create**.</span></span>
 
### <a name="creating-a-zscaler-beta-test-user"></a><span data-ttu-id="a9695-239">Skapa en testanvändare Zscaler Beta</span><span class="sxs-lookup"><span data-stu-id="a9695-239">Creating a Zscaler Beta test user</span></span>

<span data-ttu-id="a9695-240">Om du vill aktivera Azure AD-användare kan logga in på Zscaler Beta, måste de etableras Zscaler Beta.</span><span class="sxs-lookup"><span data-stu-id="a9695-240">To enable Azure AD users to log in to Zscaler Beta, they must be provisioned to Zscaler Beta.</span></span> <span data-ttu-id="a9695-241">När det gäller Zscaler Beta är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a9695-241">In the case of Zscaler Beta, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="a9695-242">Utför följande steg för att konfigurera användaretablering:</span><span class="sxs-lookup"><span data-stu-id="a9695-242">To configure user provisioning, perform the following steps:</span></span>

1. <span data-ttu-id="a9695-243">Logga in på ditt **Zscaler Beta** klient.</span><span class="sxs-lookup"><span data-stu-id="a9695-243">Log in to your **Zscaler Beta** tenant.</span></span>

2. <span data-ttu-id="a9695-244">Klicka på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="a9695-244">Click **Administration**.</span></span>   
   
    <span data-ttu-id="a9695-245">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "Administration")</span><span class="sxs-lookup"><span data-stu-id="a9695-245">![Administration](./media/active-directory-saas-zscaler-beta-tutorial/ic781035.png "Administration")</span></span>

3. <span data-ttu-id="a9695-246">Klicka på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="a9695-246">Click **User Management**.</span></span>   
        
     <span data-ttu-id="a9695-247">![Lägg till](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="a9695-247">![Add](./media/active-directory-saas-zscaler-beta-tutorial/ic781036.png "Add")</span></span>

4. <span data-ttu-id="a9695-248">I den **användare** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a9695-248">In the **Users** tab, click **Add**.</span></span>
      
    <span data-ttu-id="a9695-249">![Lägg till](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "Lägg till")</span><span class="sxs-lookup"><span data-stu-id="a9695-249">![Add](./media/active-directory-saas-zscaler-beta-tutorial/ic781037.png "Add")</span></span>

5. <span data-ttu-id="a9695-250">Utför följande steg i avsnittet Lägg till användare:</span><span class="sxs-lookup"><span data-stu-id="a9695-250">In the Add User section, perform the following steps:</span></span>
        
    <span data-ttu-id="a9695-251">![Lägg till användare](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="a9695-251">![Add User](./media/active-directory-saas-zscaler-beta-tutorial/ic781038.png "Add User")</span></span>
   
    <span data-ttu-id="a9695-252">a.</span><span class="sxs-lookup"><span data-stu-id="a9695-252">a.</span></span> <span data-ttu-id="a9695-253">Typ av **användar-ID**, **användarens visningsnamn**, **lösenord**, **Bekräfta lösenord**, och välj sedan **grupper** och **avdelning** av ett giltigt Azure AD-kontot som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="a9695-253">Type the **UserID**, **User Display Name**, **Password**, **Confirm Password**, and then select **Groups** and the **Department** of a valid Azure AD account you want to provision.</span></span>

    <span data-ttu-id="a9695-254">b.</span><span class="sxs-lookup"><span data-stu-id="a9695-254">b.</span></span> <span data-ttu-id="a9695-255">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a9695-255">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="a9695-256">Du kan använda andra Zscaler Beta användare skapa verktyg eller tillhandahålls av Zscaler Beta-API: er för att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a9695-256">You can use any other Zscaler Beta user account creation tools or APIs provided by Zscaler Beta to provision Azure AD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a9695-257">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a9695-257">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a9695-258">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Zscaler Beta.</span><span class="sxs-lookup"><span data-stu-id="a9695-258">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zscaler Beta.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a9695-260">**Om du vill tilldela Zscaler Beta Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a9695-260">**To assign Britta Simon to Zscaler Beta, perform the following steps:**</span></span>

1. <span data-ttu-id="a9695-261">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a9695-261">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a9695-263">Välj i listan med program **Zscaler Beta**.</span><span class="sxs-lookup"><span data-stu-id="a9695-263">In the applications list, select **Zscaler Beta**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-zscaler-beta-tutorial/tutorial_zscalerbeta_app.png) 

3. <span data-ttu-id="a9695-265">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a9695-265">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a9695-267">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a9695-267">Click **Add** button.</span></span> <span data-ttu-id="a9695-268">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-268">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a9695-270">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a9695-270">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a9695-271">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-271">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a9695-272">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a9695-272">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a9695-273">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a9695-273">Testing single sign-on</span></span>

<span data-ttu-id="a9695-274">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a9695-274">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a9695-275">När du klickar på panelen Zscaler Beta på åtkomstpanelen du bör få automatiskt loggat in på ditt Zscaler Beta-program.</span><span class="sxs-lookup"><span data-stu-id="a9695-275">When you click the Zscaler Beta tile in the Access Panel, you should get automatically signed-on to your Zscaler Beta application.</span></span>
<span data-ttu-id="a9695-276">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a9695-276">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a9695-277">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a9695-277">Additional resources</span></span>

* [<span data-ttu-id="a9695-278">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a9695-278">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a9695-279">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a9695-279">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-beta-tutorial/tutorial_general_203.png

