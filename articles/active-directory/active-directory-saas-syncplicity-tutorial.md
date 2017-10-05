---
title: "Självstudier: Azure Active Directory-integrering med Syncplicity | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Syncplicity."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 1321fa71bcd625d6ea754432bfb402d3919e38f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a><span data-ttu-id="9724e-103">Självstudier: Azure Active Directory-integrering med Syncplicity</span><span class="sxs-lookup"><span data-stu-id="9724e-103">Tutorial: Azure Active Directory integration with Syncplicity</span></span>

<span data-ttu-id="9724e-104">I kursen får lära du att integrera Syncplicity med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="9724e-104">In this tutorial, you learn how to integrate Syncplicity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9724e-105">Integrera Syncplicity med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9724e-105">Integrating Syncplicity with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9724e-106">Du kan styra i Azure AD som har åtkomst till Syncplicity</span><span class="sxs-lookup"><span data-stu-id="9724e-106">You can control in Azure AD who has access to Syncplicity</span></span>
- <span data-ttu-id="9724e-107">Du kan aktivera användarna att automatiskt hämta loggat in på Syncplicity (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="9724e-107">You can enable your users to automatically get signed-on to Syncplicity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9724e-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9724e-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9724e-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9724e-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9724e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9724e-110">Prerequisites</span></span>

<span data-ttu-id="9724e-111">För att konfigurera Azure AD-integrering med Syncplicity, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="9724e-111">To configure Azure AD integration with Syncplicity, you need the following items:</span></span>

- <span data-ttu-id="9724e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="9724e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9724e-113">En Syncplicity enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="9724e-113">A Syncplicity single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9724e-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9724e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9724e-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="9724e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9724e-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="9724e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9724e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9724e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9724e-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="9724e-118">Scenario description</span></span>
<span data-ttu-id="9724e-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="9724e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9724e-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="9724e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9724e-121">Att lägga till Syncplicity från galleriet</span><span class="sxs-lookup"><span data-stu-id="9724e-121">Adding Syncplicity from the gallery</span></span>
2. <span data-ttu-id="9724e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9724e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-syncplicity-from-the-gallery"></a><span data-ttu-id="9724e-123">Att lägga till Syncplicity från galleriet</span><span class="sxs-lookup"><span data-stu-id="9724e-123">Adding Syncplicity from the gallery</span></span>
<span data-ttu-id="9724e-124">Du måste lägga till Syncplicity från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Syncplicity i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="9724e-124">To configure the integration of Syncplicity into Azure AD, you need to add Syncplicity from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9724e-125">**Utför följande steg för att lägga till Syncplicity från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="9724e-125">**To add Syncplicity from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9724e-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9724e-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9724e-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="9724e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9724e-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9724e-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="9724e-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9724e-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="9724e-133">I sökrutan skriver **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="9724e-133">In the search box, type **Syncplicity**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_search.png)

5. <span data-ttu-id="9724e-135">Välj i resultatpanelen **Syncplicity**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="9724e-135">In the results panel, select **Syncplicity**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9724e-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9724e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9724e-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Syncplicity baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="9724e-138">In this section, you configure and test Azure AD single sign-on with Syncplicity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="9724e-139">Azure AD måste du känna till användaren i Syncplicity motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="9724e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Syncplicity is to a user in Azure AD.</span></span> <span data-ttu-id="9724e-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Syncplicity upprättas.</span><span class="sxs-lookup"><span data-stu-id="9724e-140">In other words, a link relationship between an Azure AD user and the related user in Syncplicity needs to be established.</span></span>

<span data-ttu-id="9724e-141">I Syncplicity, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="9724e-141">In Syncplicity, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9724e-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Syncplicity, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="9724e-142">To configure and test Azure AD single sign-on with Syncplicity, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9724e-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="9724e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9724e-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9724e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9724e-145">**[Skapa en testanvändare Syncplicity](#creating-a-syncplicity-test-user)**  – du har en motsvarighet för Britta Simon i Syncplicity som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="9724e-145">**[Creating a Syncplicity test user](#creating-a-syncplicity-test-user)** - to have a counterpart of Britta Simon in Syncplicity that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9724e-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9724e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9724e-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="9724e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9724e-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9724e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9724e-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Syncplicity program.</span><span class="sxs-lookup"><span data-stu-id="9724e-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Syncplicity application.</span></span>

<span data-ttu-id="9724e-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Syncplicity:**</span><span class="sxs-lookup"><span data-stu-id="9724e-150">**To configure Azure AD single sign-on with Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="9724e-151">I Azure-portalen på den **Syncplicity** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9724e-151">In the Azure portal, on the **Syncplicity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="9724e-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="9724e-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_samlbase.png)

3. <span data-ttu-id="9724e-155">På den **Syncplicity domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9724e-155">On the **Syncplicity Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_url.png)

    <span data-ttu-id="9724e-157">a.</span><span class="sxs-lookup"><span data-stu-id="9724e-157">a.</span></span> <span data-ttu-id="9724e-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.syncplicity.com`</span><span class="sxs-lookup"><span data-stu-id="9724e-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.syncplicity.com`</span></span>

    <span data-ttu-id="9724e-159">b.</span><span class="sxs-lookup"><span data-stu-id="9724e-159">b.</span></span> <span data-ttu-id="9724e-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.syncplicity.com/sp`</span><span class="sxs-lookup"><span data-stu-id="9724e-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.syncplicity.com/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9724e-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="9724e-161">These values are not real.</span></span> <span data-ttu-id="9724e-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="9724e-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9724e-163">Kontakta [Syncplicity klienten supportteamet](https://www.syncplicity.com/contact-us) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="9724e-163">Contact [Syncplicity Client support team](https://www.syncplicity.com/contact-us) to get these values.</span></span> 
 

4. <span data-ttu-id="9724e-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="9724e-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_certificate.png) 

  
5. <span data-ttu-id="9724e-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="9724e-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9724e-168">På den **Syncplicity Configuration** klickar du på **konfigurera Syncplicity** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="9724e-168">On the **Syncplicity Configuration** section, click **Configure Syncplicity** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9724e-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="9724e-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_configure.png) 

7. <span data-ttu-id="9724e-171">Logga in på ditt **Syncplicity** klient.</span><span class="sxs-lookup"><span data-stu-id="9724e-171">Sign in to your **Syncplicity** tenant.</span></span>

8. <span data-ttu-id="9724e-172">Klicka på menyn högst upp **admin**väljer **inställningar**, och klicka sedan på **anpassad domän och enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="9724e-172">In the menu on the top, click **admin**, select **settings**, and then click **Custom domain and single sign-on**.</span></span>
   
    <span data-ttu-id="9724e-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span><span class="sxs-lookup"><span data-stu-id="9724e-173">![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/ic769545.png "Syncplicity")</span></span>

9. <span data-ttu-id="9724e-174">På den **enkel inloggning (SSO)** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9724e-174">On the **Single Sign-On (SSO)** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="9724e-175">![Enkel inloggning \(enkel inloggning\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span><span class="sxs-lookup"><span data-stu-id="9724e-175">![Single Sign-On \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/ic769550.png "Single Sign-On \\\(SSO\\\)")</span></span>   

    <span data-ttu-id="9724e-176">a.</span><span class="sxs-lookup"><span data-stu-id="9724e-176">a.</span></span> <span data-ttu-id="9724e-177">I den **anpassad domän** textruta skriver du namnet på din domän.</span><span class="sxs-lookup"><span data-stu-id="9724e-177">In the **Custom Domain** textbox, type the name of your domain.</span></span>
  
    <span data-ttu-id="9724e-178">b.</span><span class="sxs-lookup"><span data-stu-id="9724e-178">b.</span></span> <span data-ttu-id="9724e-179">Välj **aktiverat** som **enkel inloggning Status**.</span><span class="sxs-lookup"><span data-stu-id="9724e-179">Select **Enabled** as **Single Sign-On Status**.</span></span>

    <span data-ttu-id="9724e-180">c.</span><span class="sxs-lookup"><span data-stu-id="9724e-180">c.</span></span> <span data-ttu-id="9724e-181">I den **enhets-Id** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9724e-181">In the **Entity Id** textbox, Paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9724e-182">d.</span><span class="sxs-lookup"><span data-stu-id="9724e-182">d.</span></span> <span data-ttu-id="9724e-183">I den **inloggning Sidadress** textruta klistra in den **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9724e-183">In the **Sign-in page URL** textbox, Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9724e-184">e.</span><span class="sxs-lookup"><span data-stu-id="9724e-184">e.</span></span> <span data-ttu-id="9724e-185">I den **logga ut Sidadress** textruta klistra in den **Sign-Out URL** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9724e-185">In the **Logout page URL** textbox, Paste the **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="9724e-186">f.</span><span class="sxs-lookup"><span data-stu-id="9724e-186">f.</span></span> <span data-ttu-id="9724e-187">I **providern identitetscertifikat**, klickar du på **Välj fil**, och sedan ladda upp certifikatet som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9724e-187">In **Identity Provider Certificate**, click **Choose file**, and then upload the certificate which you have downloaded from the Azure portal.</span></span> 

    <span data-ttu-id="9724e-188">g.</span><span class="sxs-lookup"><span data-stu-id="9724e-188">g.</span></span> <span data-ttu-id="9724e-189">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="9724e-189">Click **SAVE CHANGES**.</span></span>

> [!TIP]
> <span data-ttu-id="9724e-190">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="9724e-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9724e-191">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="9724e-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9724e-192">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9724e-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9724e-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9724e-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="9724e-194">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9724e-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="9724e-196">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="9724e-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9724e-197">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="9724e-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9724e-199">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="9724e-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9724e-201">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9724e-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9724e-203">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="9724e-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-syncplicity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9724e-205">a.</span><span class="sxs-lookup"><span data-stu-id="9724e-205">a.</span></span> <span data-ttu-id="9724e-206">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9724e-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9724e-207">b.</span><span class="sxs-lookup"><span data-stu-id="9724e-207">b.</span></span> <span data-ttu-id="9724e-208">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9724e-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9724e-209">c.</span><span class="sxs-lookup"><span data-stu-id="9724e-209">c.</span></span> <span data-ttu-id="9724e-210">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="9724e-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9724e-211">d.</span><span class="sxs-lookup"><span data-stu-id="9724e-211">d.</span></span> <span data-ttu-id="9724e-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9724e-212">Click **Create**.</span></span>
 
### <a name="creating-a-syncplicity-test-user"></a><span data-ttu-id="9724e-213">Skapa en testanvändare Syncplicity</span><span class="sxs-lookup"><span data-stu-id="9724e-213">Creating a Syncplicity test user</span></span>
<span data-ttu-id="9724e-214">För AAD-användare för att kunna logga in, måste de etableras till Syncplicity program.</span><span class="sxs-lookup"><span data-stu-id="9724e-214">For AAD users to be able to sign in, they must be provisioned to Syncplicity application.</span></span> <span data-ttu-id="9724e-215">Det här avsnittet beskrivs hur du skapar användarkonton i AAD i Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="9724e-215">This section describes how to create AAD user accounts in Syncplicity.</span></span>

<span data-ttu-id="9724e-216">**Utför följande steg för att etablera ett användarkonto för Syncplicity:**</span><span class="sxs-lookup"><span data-stu-id="9724e-216">**To provision a user account to Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="9724e-217">Logga in på ditt **Syncplicity** klient (till exempel: `https://company.Syncplicity.com`).</span><span class="sxs-lookup"><span data-stu-id="9724e-217">Log in to your **Syncplicity** tenant (for example: `https://company.Syncplicity.com`).</span></span>

2. <span data-ttu-id="9724e-218">Klicka på **admin** och välj **användarkonton**.</span><span class="sxs-lookup"><span data-stu-id="9724e-218">Click **admin** and select **user accounts**.</span></span>

3. <span data-ttu-id="9724e-219">Klicka på **lägga till en användare**.</span><span class="sxs-lookup"><span data-stu-id="9724e-219">Click **ADD A USER**.</span></span>
   
    <span data-ttu-id="9724e-220">![Hantera användare](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "hantera användare")</span><span class="sxs-lookup"><span data-stu-id="9724e-220">![Manage Users](./media/active-directory-saas-syncplicity-tutorial/ic769764.png "Manage Users")</span></span>

4. <span data-ttu-id="9724e-221">Typ av **e-adresser** ett AAD-konto som du vill etablera, väljer du **användare** som **rollen**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9724e-221">Type the **Email addressess** of an AAD account you want to provision, select **User** as **Role**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="9724e-222">![Kontoinformation](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "kontoinformation")</span><span class="sxs-lookup"><span data-stu-id="9724e-222">![Account Information](./media/active-directory-saas-syncplicity-tutorial/ic769765.png "Account Information")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="9724e-223">AAD kontoinnehavaren får ett e-postmeddelande med en länk för att bekräfta och aktivera kontot.</span><span class="sxs-lookup"><span data-stu-id="9724e-223">The AAD account holder  gets an email including a link to confirm and activate the account.</span></span> 
    > 

5. <span data-ttu-id="9724e-224">Välj en grupp i företaget som de nya användarna ska bli medlem i och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9724e-224">Select a group in your company that your new user should become a member of, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="9724e-225">![Gruppmedlemskap](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "gruppmedlemskap")</span><span class="sxs-lookup"><span data-stu-id="9724e-225">![Group Membership](./media/active-directory-saas-syncplicity-tutorial/ic769772.png "Group Membership")</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="9724e-226">Om det finns inga grupper visas, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9724e-226">If there are no groups listed, click **NEXT**.</span></span> 
    > 

6. <span data-ttu-id="9724e-227">Välj mapparna du vill placera under Syncplicitys kontroll på användarens dator och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="9724e-227">Select the folders you would like to place under Syncplicity’s control on the user’s computer, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="9724e-228">![Syncplicity mappar](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity mappar")</span><span class="sxs-lookup"><span data-stu-id="9724e-228">![Syncplicity Folders](./media/active-directory-saas-syncplicity-tutorial/ic769773.png "Syncplicity Folders")</span></span>

>[!NOTE]
><span data-ttu-id="9724e-229">Du kan använda något annat Syncplicity användarens konto skapas verktyg eller API: er som tillhandahålls av Syncplicity att etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="9724e-229">You can use any other Syncplicity user account creation tools or APIs provided by Syncplicity to provision AAD user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9724e-230">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="9724e-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9724e-231">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Syncplicity.</span><span class="sxs-lookup"><span data-stu-id="9724e-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Syncplicity.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="9724e-233">**Om du vill tilldela Syncplicity Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="9724e-233">**To assign Britta Simon to Syncplicity, perform the following steps:**</span></span>

1. <span data-ttu-id="9724e-234">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="9724e-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="9724e-236">Välj i listan med program **Syncplicity**.</span><span class="sxs-lookup"><span data-stu-id="9724e-236">In the applications list, select **Syncplicity**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-syncplicity-tutorial/tutorial_syncplicity_app.png) 

3. <span data-ttu-id="9724e-238">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="9724e-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="9724e-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9724e-240">Click **Add** button.</span></span> <span data-ttu-id="9724e-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9724e-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="9724e-243">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="9724e-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9724e-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9724e-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9724e-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9724e-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9724e-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="9724e-246">Testing single sign-on</span></span>

<span data-ttu-id="9724e-247">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="9724e-247">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9724e-248">När du klickar på panelen Syncplicity på åtkomstpanelen du bör få automatiskt loggat in på ditt Syncplicity program.</span><span class="sxs-lookup"><span data-stu-id="9724e-248">When you click the Syncplicity tile in the Access Panel, you should get automatically signed-on to your Syncplicity application.</span></span>
## <a name="additional-resources"></a><span data-ttu-id="9724e-249">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9724e-249">Additional resources</span></span>

* [<span data-ttu-id="9724e-250">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="9724e-250">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9724e-251">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="9724e-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-syncplicity-tutorial/tutorial_general_203.png

