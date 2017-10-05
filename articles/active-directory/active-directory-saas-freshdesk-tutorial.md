---
title: "Självstudier: Azure Active Directory-integrering med FreshDesk | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och FreshDesk."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: f4b47e345a40b64f69ad8a4618564662b4a6c879
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a><span data-ttu-id="a7b78-103">Självstudier: Azure Active Directory-integrering med FreshDesk</span><span class="sxs-lookup"><span data-stu-id="a7b78-103">Tutorial: Azure Active Directory integration with FreshDesk</span></span>

<span data-ttu-id="a7b78-104">I kursen får lära du att integrera FreshDesk med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a7b78-104">In this tutorial, you learn how to integrate FreshDesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a7b78-105">Integrera FreshDesk med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a7b78-105">Integrating FreshDesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a7b78-106">Du kan styra i Azure AD som har åtkomst till FreshDesk</span><span class="sxs-lookup"><span data-stu-id="a7b78-106">You can control in Azure AD who has access to FreshDesk</span></span>
- <span data-ttu-id="a7b78-107">Du kan aktivera användarna att automatiskt hämta loggat in på FreshDesk (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a7b78-107">You can enable your users to automatically get signed-on to FreshDesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a7b78-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="a7b78-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="a7b78-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a7b78-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7b78-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a7b78-110">Prerequisites</span></span>

<span data-ttu-id="a7b78-111">För att konfigurera Azure AD-integrering med FreshDesk, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="a7b78-111">To configure Azure AD integration with FreshDesk, you need the following items:</span></span>

- <span data-ttu-id="a7b78-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a7b78-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a7b78-113">En FreshDesk enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="a7b78-113">A FreshDesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a7b78-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a7b78-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a7b78-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a7b78-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a7b78-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a7b78-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="a7b78-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a7b78-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a7b78-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a7b78-118">Scenario description</span></span>
<span data-ttu-id="a7b78-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a7b78-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a7b78-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a7b78-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a7b78-121">Att lägga till FreshDesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="a7b78-121">Adding FreshDesk from the gallery</span></span>
2. <span data-ttu-id="a7b78-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7b78-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshdesk-from-the-gallery"></a><span data-ttu-id="a7b78-123">Att lägga till FreshDesk från galleriet</span><span class="sxs-lookup"><span data-stu-id="a7b78-123">Adding FreshDesk from the gallery</span></span>
<span data-ttu-id="a7b78-124">Du måste lägga till FreshDesk från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av FreshDesk i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a7b78-124">To configure the integration of FreshDesk into Azure AD, you need to add FreshDesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a7b78-125">**Utför följande steg för att lägga till FreshDesk från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="a7b78-125">**To add FreshDesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a7b78-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a7b78-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a7b78-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a7b78-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="a7b78-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7b78-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="a7b78-133">I sökrutan skriver **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-133">In the search box, type **FreshDesk**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. <span data-ttu-id="a7b78-135">Välj i resultatpanelen **FreshDesk**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="a7b78-135">In the results panel, select **FreshDesk**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a7b78-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7b78-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a7b78-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med FreshDesk baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a7b78-138">In this section, you configure and test Azure AD single sign-on with FreshDesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a7b78-139">Azure AD måste du känna till användaren i FreshDesk motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="a7b78-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FreshDesk is to a user in Azure AD.</span></span> <span data-ttu-id="a7b78-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i FreshDesk upprättas.</span><span class="sxs-lookup"><span data-stu-id="a7b78-140">In other words, a link relationship between an Azure AD user and the related user in FreshDesk needs to be established.</span></span>

<span data-ttu-id="a7b78-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="a7b78-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FreshDesk.</span></span>

<span data-ttu-id="a7b78-142">Om du vill konfigurera och testa Azure AD enkel inloggning med FreshDesk, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a7b78-142">To configure and test Azure AD single sign-on with FreshDesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a7b78-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a7b78-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a7b78-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a7b78-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a7b78-145">**[Skapa en testanvändare FreshDesk](#creating-a-freshdesk-test-user)**  – du har en motsvarighet för Britta Simon i FreshDesk som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="a7b78-145">**[Creating a FreshDesk test user](#creating-a-freshdesk-test-user)** - to have a counterpart of Britta Simon in FreshDesk that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="a7b78-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a7b78-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a7b78-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a7b78-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a7b78-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7b78-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a7b78-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt FreshDesk program.</span><span class="sxs-lookup"><span data-stu-id="a7b78-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FreshDesk application.</span></span>

<span data-ttu-id="a7b78-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med FreshDesk:**</span><span class="sxs-lookup"><span data-stu-id="a7b78-150">**To configure Azure AD single sign-on with FreshDesk, perform the following steps:**</span></span>

1. <span data-ttu-id="a7b78-151">I Azure-hanteringsportalen på den **FreshDesk** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-151">In the Azure Management portal, on the **FreshDesk** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="a7b78-153">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="a7b78-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. <span data-ttu-id="a7b78-155">På den **FreshDesk domän och URL: er** avsnittet, ange den **inloggnings-URL** som: `https://<tenant-name>.freshdesk.com` eller något annat värde Freshdesk har förslag.</span><span class="sxs-lookup"><span data-stu-id="a7b78-155">On the **FreshDesk Domain and URLs** section, please enter the **Sign-on URL** as: `https://<tenant-name>.freshdesk.com` or any other value Freshdesk has suggested.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > <span data-ttu-id="a7b78-157">Observera att detta inte är det verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="a7b78-157">Please note that this is not the real value.</span></span> <span data-ttu-id="a7b78-158">Du måste uppdatera värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="a7b78-158">You have to update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="a7b78-159">Kontakta [FreshDesk klienten supportteamet](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="a7b78-159">Contact [FreshDesk Client support team](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) to get this value.</span></span>  

4. <span data-ttu-id="a7b78-160">På den **SAML-signeringscertifikat** klickar du på **certifikat** och spara certifikatet på datorn.</span><span class="sxs-lookup"><span data-stu-id="a7b78-160">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. <span data-ttu-id="a7b78-162">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="a7b78-162">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a7b78-164">På den **FreshDesk Configuration** klickar du på **konfigurera FreshDesk** öppna Konfigurera inloggning fönster.</span><span class="sxs-lookup"><span data-stu-id="a7b78-164">On the **FreshDesk Configuration** section, click **Configure FreshDesk** to open Configure sign-on window.</span></span> <span data-ttu-id="a7b78-165">Kopiera SAML inloggning tjänst-URL för enkel och Sign-Out URL från den **Snabbreferens** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a7b78-165">Copy the SAML Single Sign-On Service URL and Sign-Out URL from the **Quick Reference** section.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. <span data-ttu-id="a7b78-167">Logga in på webbplatsen Freshdesk företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="a7b78-167">In a different web browser window, log into your Freshdesk company site as an administrator.</span></span>

8. <span data-ttu-id="a7b78-168">Klicka på menyn högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-168">In the menu on the top, click **Admin**.</span></span>
   
   <span data-ttu-id="a7b78-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="a7b78-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span></span>

9. <span data-ttu-id="a7b78-170">I den **allmänna inställningar** klickar du på **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-170">In the **General Settings** tab, click **Security**.</span></span>
   
   <span data-ttu-id="a7b78-171">![Säkerhet](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "säkerhet")</span><span class="sxs-lookup"><span data-stu-id="a7b78-171">![Security](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Security")</span></span>

10. <span data-ttu-id="a7b78-172">I den **säkerhet** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a7b78-172">In the **Security** section, perform the following steps:</span></span>
   
    <span data-ttu-id="a7b78-173">![Enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="a7b78-173">![Single Sign On](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Single Sign On")</span></span>
   
    <span data-ttu-id="a7b78-174">a.</span><span class="sxs-lookup"><span data-stu-id="a7b78-174">a.</span></span> <span data-ttu-id="a7b78-175">För **enkel inloggning (SSO)**väljer **på**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-175">For **Single Sign On (SSO)**, select **On**.</span></span>

    <span data-ttu-id="a7b78-176">b.</span><span class="sxs-lookup"><span data-stu-id="a7b78-176">b.</span></span> <span data-ttu-id="a7b78-177">Välj **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-177">Select **SAML SSO**.</span></span>

    <span data-ttu-id="a7b78-178">c.</span><span class="sxs-lookup"><span data-stu-id="a7b78-178">c.</span></span> <span data-ttu-id="a7b78-179">Typ av **SAML inloggning tjänst-URL för enkel** du kopierade från Azure-portalen i den **inloggnings-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="a7b78-179">Type the **SAML Single Sign-On Service URL** you copied from Azure portal into the **SAML Login URL** textbox.</span></span>

    <span data-ttu-id="a7b78-180">d.</span><span class="sxs-lookup"><span data-stu-id="a7b78-180">d.</span></span> <span data-ttu-id="a7b78-181">Typ av **Sign-Out URL** du kopierade från Azure-portalen i den **logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="a7b78-181">Type the **Sign-Out URL**  you copied from Azure portal into the **Logout URL** textbox.</span></span>

    <span data-ttu-id="a7b78-182">e.</span><span class="sxs-lookup"><span data-stu-id="a7b78-182">e.</span></span> <span data-ttu-id="a7b78-183">Kopiera den **tumavtrycket** värde från det hämta certifikatet från Azure-portalen och klistrar in det i den **säkerhet certifikat fingeravtryck** textruta.</span><span class="sxs-lookup"><span data-stu-id="a7b78-183">Copy the **Thumbprint** value from the downloaded certificate from Azure portal and paste it into the **Security Certificate Fingerprint** textbox.</span></span>  
 
    >[!TIP]
    ><span data-ttu-id="a7b78-184">Mer information finns i [hur du hämtar ett certifikat tumavtrycksvärde](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="a7b78-184">For more details, see [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span> 
    
    <span data-ttu-id="a7b78-185">f.</span><span class="sxs-lookup"><span data-stu-id="a7b78-185">f.</span></span> <span data-ttu-id="a7b78-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-186">Click **Save**.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a7b78-187">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a7b78-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="a7b78-188">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a7b78-188">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="a7b78-190">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="a7b78-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a7b78-191">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="a7b78-191">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a7b78-193">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="a7b78-193">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a7b78-195">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7b78-195">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a7b78-197">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a7b78-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a7b78-199">a.</span><span class="sxs-lookup"><span data-stu-id="a7b78-199">a.</span></span> <span data-ttu-id="a7b78-200">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a7b78-201">b.</span><span class="sxs-lookup"><span data-stu-id="a7b78-201">b.</span></span> <span data-ttu-id="a7b78-202">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a7b78-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a7b78-203">c.</span><span class="sxs-lookup"><span data-stu-id="a7b78-203">c.</span></span> <span data-ttu-id="a7b78-204">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a7b78-205">d.</span><span class="sxs-lookup"><span data-stu-id="a7b78-205">d.</span></span> <span data-ttu-id="a7b78-206">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-206">Click **Create**.</span></span>
 
### <a name="creating-a-freshdesk-test-user"></a><span data-ttu-id="a7b78-207">Skapa en testanvändare FreshDesk</span><span class="sxs-lookup"><span data-stu-id="a7b78-207">Creating a FreshDesk test user</span></span>

<span data-ttu-id="a7b78-208">För att aktivera Azure AD-användare att logga in på FreshDesk etableras de i FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="a7b78-208">In order to enable Azure AD users to log into FreshDesk, they must be provisioned into FreshDesk.</span></span>  
<span data-ttu-id="a7b78-209">När det gäller FreshDesk är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a7b78-209">In the case of FreshDesk, provisioning is a manual task.</span></span>

<span data-ttu-id="a7b78-210">**Utför följande steg för att etablera en användarkonton:**</span><span class="sxs-lookup"><span data-stu-id="a7b78-210">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="a7b78-211">Logga in på ditt **Freshdesk** klient.</span><span class="sxs-lookup"><span data-stu-id="a7b78-211">Log in to your **Freshdesk** tenant.</span></span>
2. <span data-ttu-id="a7b78-212">Klicka på menyn högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-212">In the menu on the top, click **Admin**.</span></span>
   
   <span data-ttu-id="a7b78-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="a7b78-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span></span>

3. <span data-ttu-id="a7b78-214">I den **allmänna inställningar** klickar du på **agenter**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-214">In the **General Settings** tab, click **Agents**.</span></span>
   
   <span data-ttu-id="a7b78-215">![Agenter](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "agenter")</span><span class="sxs-lookup"><span data-stu-id="a7b78-215">![Agents](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")</span></span>

4. <span data-ttu-id="a7b78-216">Klicka på **ny Agent**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-216">Click **New Agent**.</span></span>
   
    <span data-ttu-id="a7b78-217">![Ny Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "ny Agent")</span><span class="sxs-lookup"><span data-stu-id="a7b78-217">![New Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "New Agent")</span></span>

5. <span data-ttu-id="a7b78-218">I dialogrutan agentinformation utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="a7b78-218">On the Agent Information dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="a7b78-219">![Agentinformation](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "agentinformation")</span><span class="sxs-lookup"><span data-stu-id="a7b78-219">![Agent Information](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent Information")</span></span>
   
   <span data-ttu-id="a7b78-220">a.</span><span class="sxs-lookup"><span data-stu-id="a7b78-220">a.</span></span> <span data-ttu-id="a7b78-221">I den **fullständiga namn** textruta skriver du namnet på Azure AD-kontot som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="a7b78-221">In the **Full Name** textbox, type the name of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="a7b78-222">b.</span><span class="sxs-lookup"><span data-stu-id="a7b78-222">b.</span></span> <span data-ttu-id="a7b78-223">I den **e-post** textruta typen Azure AD Azure AD-kontot som du vill etablera e-postadress.</span><span class="sxs-lookup"><span data-stu-id="a7b78-223">In the **Email** textbox, type the Azure AD email address of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="a7b78-224">c.</span><span class="sxs-lookup"><span data-stu-id="a7b78-224">c.</span></span> <span data-ttu-id="a7b78-225">I den **rubrik** textruta Ange rubriken för Azure AD-kontot som du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="a7b78-225">In the **Title** textbox, type the title of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="a7b78-226">d.</span><span class="sxs-lookup"><span data-stu-id="a7b78-226">d.</span></span> <span data-ttu-id="a7b78-227">Välj **agenter rollen**, och klicka sedan på **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-227">Select **Agents role**, and then click **Assign**.</span></span>
       
   <span data-ttu-id="a7b78-228">e.</span><span class="sxs-lookup"><span data-stu-id="a7b78-228">e.</span></span> <span data-ttu-id="a7b78-229">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-229">Click **Save**.</span></span>     
   
    >[!NOTE]
    ><span data-ttu-id="a7b78-230">Azure AD-kontoinnehavaren får ett e-postmeddelande som innehåller en länk för att bekräfta kontot innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="a7b78-230">The Azure AD account holder will get an email that includes a link to confirm the account before it is activated.</span></span> 
    > 
    
    >[!NOTE]
    ><span data-ttu-id="a7b78-231">Du kan använda något annat Freshdesk användarens konto skapas verktyg eller API: er som tillhandahålls av Freshdesk att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="a7b78-231">You can use any other Freshdesk user account creation tools or APIs provided by Freshdesk to provision AAD user accounts.</span></span> <span data-ttu-id="a7b78-232">att FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="a7b78-232">to FreshDesk.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a7b78-233">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="a7b78-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a7b78-234">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning med sin åtkomst beviljas till Box.</span><span class="sxs-lookup"><span data-stu-id="a7b78-234">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Box.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="a7b78-236">**Om du vill tilldela FreshDesk Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a7b78-236">**To assign Britta Simon to FreshDesk, perform the following steps:**</span></span>

1. <span data-ttu-id="a7b78-237">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-237">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="a7b78-239">Välj i listan med program **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-239">In the applications list, select **FreshDesk**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. <span data-ttu-id="a7b78-241">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="a7b78-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="a7b78-243">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a7b78-243">Click **Add** button.</span></span> <span data-ttu-id="a7b78-244">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7b78-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="a7b78-246">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="a7b78-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a7b78-247">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7b78-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a7b78-248">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a7b78-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a7b78-249">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a7b78-249">Testing single sign-on</span></span>

<span data-ttu-id="a7b78-250">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a7b78-250">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a7b78-251">Du bör få inloggningssidan att hämta loggat in på ditt FreshDesk program när du klickar på panelen FreshDesk på åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a7b78-251">When you click the FreshDesk tile in the Access Panel, you should get login page to get signed-on to your FreshDesk application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7b78-252">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a7b78-252">Additional resources</span></span>

* [<span data-ttu-id="a7b78-253">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a7b78-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a7b78-254">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a7b78-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

