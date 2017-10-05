---
title: "Självstudier: Azure Active Directory-integrering med TOPdesk - offentliga | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och TOPdesk - allmänheten."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: f21fe0b363776974108ff460060e4c15a51a58a3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a><span data-ttu-id="d7e76-103">Självstudier: Azure Active Directory-integrering med TOPdesk - offentliga</span><span class="sxs-lookup"><span data-stu-id="d7e76-103">Tutorial: Azure Active Directory integration with TOPdesk - Public</span></span>

<span data-ttu-id="d7e76-104">I kursen får lära du att integrera TOPdesk - offentliga med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d7e76-104">In this tutorial, you learn how to integrate TOPdesk - Public with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d7e76-105">Integrera TOPdesk - offentliga med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d7e76-105">Integrating TOPdesk - Public with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d7e76-106">Du kan styra i Azure AD som har åtkomst till TOPdesk - allmänheten.</span><span class="sxs-lookup"><span data-stu-id="d7e76-106">You can control in Azure AD who has access to TOPdesk - Public.</span></span>
- <span data-ttu-id="d7e76-107">Du kan aktivera användarna att automatiskt hämta loggat in på TOPdesk - offentlig (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="d7e76-107">You can enable your users to automatically get signed-on to TOPdesk - Public (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d7e76-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="d7e76-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d7e76-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7e76-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d7e76-110">Prerequisites</span></span>

<span data-ttu-id="d7e76-111">Konfigurera Azure AD-integrering med TOPdesk - offentliga, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d7e76-111">To configure Azure AD integration with TOPdesk - Public, you need the following items:</span></span>

- <span data-ttu-id="d7e76-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d7e76-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d7e76-113">En TOPdesk - offentliga enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d7e76-113">A TOPdesk - Public single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d7e76-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d7e76-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d7e76-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d7e76-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d7e76-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d7e76-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d7e76-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d7e76-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d7e76-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d7e76-118">Scenario description</span></span>
<span data-ttu-id="d7e76-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d7e76-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d7e76-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d7e76-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d7e76-121">Lägger till TOPdesk - offentliga från galleriet</span><span class="sxs-lookup"><span data-stu-id="d7e76-121">Adding TOPdesk - Public from the gallery</span></span>
2. <span data-ttu-id="d7e76-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d7e76-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-topdesk---public-from-the-gallery"></a><span data-ttu-id="d7e76-123">Lägger till TOPdesk - offentliga från galleriet</span><span class="sxs-lookup"><span data-stu-id="d7e76-123">Adding TOPdesk - Public from the gallery</span></span>
<span data-ttu-id="d7e76-124">Du måste lägga till TOPdesk - offentliga från galleriet i listan över hanterade SaaS-appar för att konfigurera TOPdesk - offentliga till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="d7e76-124">To configure the integration of TOPdesk - Public into Azure AD, you need to add TOPdesk - Public from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d7e76-125">**Utför följande steg för att lägga till TOPdesk - offentliga från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="d7e76-125">**To add TOPdesk - Public from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d7e76-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d7e76-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="d7e76-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d7e76-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="d7e76-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d7e76-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="d7e76-133">I sökrutan skriver **TOPdesk - offentliga**väljer **TOPdesk - offentliga** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="d7e76-133">In the search box, type **TOPdesk - Public**, select **TOPdesk - Public** from result panel then click **Add** button to add the application.</span></span>

    ![TOPdesk - offentliga i resultatlistan](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d7e76-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d7e76-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d7e76-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TOPdesk - offentliga baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d7e76-136">In this section, you configure and test Azure AD single sign-on with TOPdesk - Public based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d7e76-137">Azure AD måste veta vilka motsvarande användaren i TOPdesk - offentliga är att en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d7e76-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TOPdesk - Public is to a user in Azure AD.</span></span> <span data-ttu-id="d7e76-138">Med andra ord en länk förhållandet mellan en Azure AD-användare och relaterade användaren i TOPdesk - offentliga måste upprättas.</span><span class="sxs-lookup"><span data-stu-id="d7e76-138">In other words, a link relationship between an Azure AD user and the related user in TOPdesk - Public needs to be established.</span></span>

<span data-ttu-id="d7e76-139">I TOPdesk - offentliga, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-139">In TOPdesk - Public, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d7e76-140">Konfigurera och testa Azure AD enkel inloggning med TOPdesk - offentlig, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d7e76-140">To configure and test Azure AD single sign-on with TOPdesk - Public, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d7e76-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d7e76-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7e76-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d7e76-143">**[Skapa en TOPdesk - offentliga testanvändare](#create-a-topdesk---public-test-user)**  – har en motsvarighet för Britta Simon TOPdesk - offentliga som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d7e76-143">**[Create a TOPdesk - Public test user](#create-a-topdesk---public-test-user)** - to have a counterpart of Britta Simon in TOPdesk - Public that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d7e76-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d7e76-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d7e76-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d7e76-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d7e76-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d7e76-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d7e76-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din TOPdesk - offentliga program.</span><span class="sxs-lookup"><span data-stu-id="d7e76-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TOPdesk - Public application.</span></span>

<span data-ttu-id="d7e76-148">**Konfigurera Azure AD enkel inloggning med TOPdesk - offentliga, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d7e76-148">**To configure Azure AD single sign-on with TOPdesk - Public, perform the following steps:**</span></span>

1. <span data-ttu-id="d7e76-149">I Azure-portalen på den **TOPdesk - offentliga** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-149">In the Azure portal, on the **TOPdesk - Public** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="d7e76-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d7e76-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. <span data-ttu-id="d7e76-153">På den **TOPdesk - URL: er och den offentliga domänen** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d7e76-153">On the **TOPdesk - Public Domain and URLs** section, perform the following steps:</span></span>

    ![TOPdesk - URL: er och den offentliga domänen enkel inloggning information](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    <span data-ttu-id="d7e76-155">a.</span><span class="sxs-lookup"><span data-stu-id="d7e76-155">a.</span></span> <span data-ttu-id="d7e76-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.topdesk.net`</span><span class="sxs-lookup"><span data-stu-id="d7e76-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net`</span></span>
    
    <span data-ttu-id="d7e76-157">b.</span><span class="sxs-lookup"><span data-stu-id="d7e76-157">b.</span></span> <span data-ttu-id="d7e76-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.topdesk.net/tas/public/login/verify`</span><span class="sxs-lookup"><span data-stu-id="d7e76-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net/tas/public/login/verify`</span></span>

    <span data-ttu-id="d7e76-159">c.</span><span class="sxs-lookup"><span data-stu-id="d7e76-159">c.</span></span> <span data-ttu-id="d7e76-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<companyname>.topdesk.net/tas/public/login/saml`</span><span class="sxs-lookup"><span data-stu-id="d7e76-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net/tas/public/login/saml`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="d7e76-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d7e76-161">These values are not real.</span></span> <span data-ttu-id="d7e76-162">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="d7e76-162">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="d7e76-163">Svars-URL är explaned senare i självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-163">Reply URL is explaned later in tutorial.</span></span> <span data-ttu-id="d7e76-164">Kontakta [TOPdesk - offentliga klienten supportteamet](https://help.topdesk.com/saas/enterprise/user/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d7e76-164">Contact [TOPdesk - Public Client support team](https://help.topdesk.com/saas/enterprise/user/) to get these values.</span></span>  

4. <span data-ttu-id="d7e76-165">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="d7e76-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. <span data-ttu-id="d7e76-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="d7e76-169">På den **TOPdesk - offentliga konfiguration** klickar du på **konfigurera TOPdesk - offentliga** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d7e76-169">On the **TOPdesk - Public Configuration** section, click **Configure TOPdesk - Public** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d7e76-170">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d7e76-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TOPdesk - offentliga konfiguration](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. <span data-ttu-id="d7e76-172">Logga in på ditt **TOPdesk - offentliga** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="d7e76-172">Sign on to your **TOPdesk - Public** company site as an administrator.</span></span>

8. <span data-ttu-id="d7e76-173">I den **TOPdesk** -menyn klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-173">In the **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="d7e76-174">![Inställningar för](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "inställningar")</span><span class="sxs-lookup"><span data-stu-id="d7e76-174">![Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "Settings")</span></span>

9. <span data-ttu-id="d7e76-175">Klicka på **inloggningsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-175">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="d7e76-176">![Inloggningsinställningar](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "inloggningsinställningar")</span><span class="sxs-lookup"><span data-stu-id="d7e76-176">![Login Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "Login Settings")</span></span>

10. <span data-ttu-id="d7e76-177">Expandera den **inloggningsinställningar** -menyn och klicka sedan på **allmänna**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-177">Expand the **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="d7e76-178">![Allmän](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "Allmänt")</span><span class="sxs-lookup"><span data-stu-id="d7e76-178">![General](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "General")</span></span>

11. <span data-ttu-id="d7e76-179">I den **offentliga** avsnitt i den **SAML inloggningen** konfiguration och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d7e76-179">In the **Public** section of the **SAML login** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="d7e76-180">![Tekniska inställningar](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "tekniska inställningar")</span><span class="sxs-lookup"><span data-stu-id="d7e76-180">![Technical Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "Technical Settings")</span></span>
   
    <span data-ttu-id="d7e76-181">a.</span><span class="sxs-lookup"><span data-stu-id="d7e76-181">a.</span></span> <span data-ttu-id="d7e76-182">Klicka på **hämta** att hämta metadatafilen offentliga och sedan spara det lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="d7e76-182">Click **Download** to download the public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="d7e76-183">b.</span><span class="sxs-lookup"><span data-stu-id="d7e76-183">b.</span></span> <span data-ttu-id="d7e76-184">Öppna metadatafilen hämtade och leta upp den **AssertionConsumerService** nod.</span><span class="sxs-lookup"><span data-stu-id="d7e76-184">Open the downloaded metadata file, and then locate the **AssertionConsumerService** node.</span></span>

    <span data-ttu-id="d7e76-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span><span class="sxs-lookup"><span data-stu-id="d7e76-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span></span>
   
    <span data-ttu-id="d7e76-186">c.</span><span class="sxs-lookup"><span data-stu-id="d7e76-186">c.</span></span> <span data-ttu-id="d7e76-187">Kopiera den **AssertionConsumerService** värde, klistra in det här värdet i den **Reply URL** TextBox-kontroll i **TOPdesk - URL: er och den offentliga domänen** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d7e76-187">Copy the **AssertionConsumerService** value, paste this value in the **Reply URL** textbox in **TOPdesk - Public Domain and URLs** section.</span></span>      
   
12. <span data-ttu-id="d7e76-188">Utför följande steg för att skapa en certifikatfil:</span><span class="sxs-lookup"><span data-stu-id="d7e76-188">To create a certificate file, perform the following steps:</span></span>
    
    <span data-ttu-id="d7e76-189">![Certifikatet](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "certifikat")</span><span class="sxs-lookup"><span data-stu-id="d7e76-189">![Certificate](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "Certificate")</span></span>
    
    <span data-ttu-id="d7e76-190">a.</span><span class="sxs-lookup"><span data-stu-id="d7e76-190">a.</span></span> <span data-ttu-id="d7e76-191">Öppna metadatafilen hämtas från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-191">Open the downloaded metadata file from Azure portal.</span></span>
    
    <span data-ttu-id="d7e76-192">b.</span><span class="sxs-lookup"><span data-stu-id="d7e76-192">b.</span></span> <span data-ttu-id="d7e76-193">Expandera den **RoleDescriptor** nod som har en **xsi: type** av **aggregeringsdesignprocessen: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-193">Expand the **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    
    <span data-ttu-id="d7e76-194">c.</span><span class="sxs-lookup"><span data-stu-id="d7e76-194">c.</span></span> <span data-ttu-id="d7e76-195">Kopiera värdet för den **X509Certificate** nod.</span><span class="sxs-lookup"><span data-stu-id="d7e76-195">Copy the value of the **X509Certificate** node.</span></span>
    
    <span data-ttu-id="d7e76-196">d.</span><span class="sxs-lookup"><span data-stu-id="d7e76-196">d.</span></span> <span data-ttu-id="d7e76-197">Spara den kopierade **X509Certificate** värdet lokalt på din dator i en fil.</span><span class="sxs-lookup"><span data-stu-id="d7e76-197">Save the copied **X509Certificate** value locally on your computer in a file.</span></span>

13. <span data-ttu-id="d7e76-198">I den **offentliga** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-198">In the **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="d7e76-199">![SAML-inloggningen](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-inloggning")</span><span class="sxs-lookup"><span data-stu-id="d7e76-199">![SAML Login](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML Login")</span></span>

14. <span data-ttu-id="d7e76-200">På den **SAML configuration assistenten** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d7e76-200">On the **SAML configuration assistant** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="d7e76-201">![SAML Configuration assistenten](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "assistenten för SAML-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="d7e76-201">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="d7e76-202">a.</span><span class="sxs-lookup"><span data-stu-id="d7e76-202">a.</span></span> <span data-ttu-id="d7e76-203">Att överföra metadatafilen hämtas från Azure-portalen under **Federationsmetadata**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-203">To upload your downloaded metadata file from Azure portal, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="d7e76-204">b.</span><span class="sxs-lookup"><span data-stu-id="d7e76-204">b.</span></span> <span data-ttu-id="d7e76-205">Att överföra din certifikatfil under **certifikat (RSA)**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-205">To upload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="d7e76-206">c.</span><span class="sxs-lookup"><span data-stu-id="d7e76-206">c.</span></span> <span data-ttu-id="d7e76-207">Att överföra logotypfilen som du har fått från supportteamet TOPdesk under **logotypen ikonen**, klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-207">To upload the logo file you got from the TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="d7e76-208">d.</span><span class="sxs-lookup"><span data-stu-id="d7e76-208">d.</span></span> <span data-ttu-id="d7e76-209">I den **användarnamn** textruta typen `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="d7e76-209">In the **User name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="d7e76-210">e.</span><span class="sxs-lookup"><span data-stu-id="d7e76-210">e.</span></span> <span data-ttu-id="d7e76-211">I den **visningsnamn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d7e76-211">In the **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="d7e76-212">f.</span><span class="sxs-lookup"><span data-stu-id="d7e76-212">f.</span></span> <span data-ttu-id="d7e76-213">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d7e76-214">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="d7e76-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d7e76-215">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="d7e76-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d7e76-216">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d7e76-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d7e76-217">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d7e76-217">Create an Azure AD test user</span></span>

<span data-ttu-id="d7e76-218">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d7e76-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="d7e76-220">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="d7e76-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d7e76-221">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-221">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d7e76-223">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-223">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d7e76-225">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d7e76-225">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d7e76-227">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="d7e76-227">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d7e76-229">a.</span><span class="sxs-lookup"><span data-stu-id="d7e76-229">a.</span></span> <span data-ttu-id="d7e76-230">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-230">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d7e76-231">b.</span><span class="sxs-lookup"><span data-stu-id="d7e76-231">b.</span></span> <span data-ttu-id="d7e76-232">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="d7e76-232">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="d7e76-233">c.</span><span class="sxs-lookup"><span data-stu-id="d7e76-233">c.</span></span> <span data-ttu-id="d7e76-234">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="d7e76-234">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="d7e76-235">d.</span><span class="sxs-lookup"><span data-stu-id="d7e76-235">d.</span></span> <span data-ttu-id="d7e76-236">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-236">Click **Create**.</span></span>
 
### <a name="create-a-topdesk---public-test-user"></a><span data-ttu-id="d7e76-237">Skapa en TOPdesk - offentliga testanvändare</span><span class="sxs-lookup"><span data-stu-id="d7e76-237">Create a TOPdesk - Public test user</span></span>

<span data-ttu-id="d7e76-238">För att aktivera Azure AD-användare att logga in på TOPdesk - offentliga de måste etableras i TOPdesk - allmänheten.</span><span class="sxs-lookup"><span data-stu-id="d7e76-238">In order to enable Azure AD users to log into TOPdesk - Public, they must be provisioned into TOPdesk - Public.</span></span>  
<span data-ttu-id="d7e76-239">När det gäller TOPdesk - offentliga etablering är en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d7e76-239">In the case of TOPdesk - Public, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="d7e76-240">Utför följande steg för att konfigurera användaretablering:</span><span class="sxs-lookup"><span data-stu-id="d7e76-240">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="d7e76-241">Logga in på ditt **TOPdesk - offentliga** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="d7e76-241">Sign on to your **TOPdesk - Public** company site as administrator.</span></span>

2. <span data-ttu-id="d7e76-242">Klicka på menyn högst upp **TOPdesk \> ny \> stödfiler \> Person**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-242">In the menu on the top, click **TOPdesk \> New \> Support Files \> Person**.</span></span>
   
    <span data-ttu-id="d7e76-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span><span class="sxs-lookup"><span data-stu-id="d7e76-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span></span>

3. <span data-ttu-id="d7e76-244">I dialogrutan Ny Person att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="d7e76-244">On the New Person dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="d7e76-245">![Ny Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "ny Person")</span><span class="sxs-lookup"><span data-stu-id="d7e76-245">![New Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "New Person")</span></span>
   
    <span data-ttu-id="d7e76-246">a.</span><span class="sxs-lookup"><span data-stu-id="d7e76-246">a.</span></span> <span data-ttu-id="d7e76-247">Klicka på fliken Allmänt.</span><span class="sxs-lookup"><span data-stu-id="d7e76-247">Click the General tab.</span></span>

    <span data-ttu-id="d7e76-248">b.</span><span class="sxs-lookup"><span data-stu-id="d7e76-248">b.</span></span> <span data-ttu-id="d7e76-249">I den **efternamn** textruta skriver ut Simon användarens efternamn</span><span class="sxs-lookup"><span data-stu-id="d7e76-249">In the **Surname** textbox, type Surname of the user like Simon</span></span>
 
    <span data-ttu-id="d7e76-250">c.</span><span class="sxs-lookup"><span data-stu-id="d7e76-250">c.</span></span> <span data-ttu-id="d7e76-251">Välj en **plats** för kontot.</span><span class="sxs-lookup"><span data-stu-id="d7e76-251">Select a **Site** for the account.</span></span>
 
    <span data-ttu-id="d7e76-252">d.</span><span class="sxs-lookup"><span data-stu-id="d7e76-252">d.</span></span> <span data-ttu-id="d7e76-253">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-253">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="d7e76-254">Du kan använda alla andra TOPdesk - offentliga användare skapa verktyg eller API: er som tillhandahålls av TOPdesk - offentliga att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="d7e76-254">You can use any other TOPdesk - Public user account creation tools or APIs provided by TOPdesk - Public to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="d7e76-255">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="d7e76-255">Assign the Azure AD test user</span></span>

<span data-ttu-id="d7e76-256">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till TOPdesk - allmänheten.</span><span class="sxs-lookup"><span data-stu-id="d7e76-256">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TOPdesk - Public.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="d7e76-258">**Att tilldela TOPdesk - Britta Simon offentliga, utför följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d7e76-258">**To assign Britta Simon to TOPdesk - Public, perform the following steps:**</span></span>

1. <span data-ttu-id="d7e76-259">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-259">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d7e76-261">Välj i listan med program **TOPdesk - offentliga**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-261">In the applications list, select **TOPdesk - Public**.</span></span>

    ![TOPdesk - offentliga länken i listan med program](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. <span data-ttu-id="d7e76-263">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d7e76-263">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="d7e76-265">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d7e76-265">Click **Add** button.</span></span> <span data-ttu-id="d7e76-266">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d7e76-266">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="d7e76-268">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="d7e76-268">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d7e76-269">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d7e76-269">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d7e76-270">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d7e76-270">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d7e76-271">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d7e76-271">Test single sign-on</span></span>

<span data-ttu-id="d7e76-272">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d7e76-272">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d7e76-273">När du klickar på TOPdesk - offentliga panelen i åtkomstpanelen, du bör få automatiskt loggat in på ditt TOPdesk - offentliga program.</span><span class="sxs-lookup"><span data-stu-id="d7e76-273">When you click the TOPdesk - Public tile in the Access Panel, you should get automatically signed-on to your TOPdesk - Public application.</span></span>
<span data-ttu-id="d7e76-274">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d7e76-274">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d7e76-275">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d7e76-275">Additional resources</span></span>

* [<span data-ttu-id="d7e76-276">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d7e76-276">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d7e76-277">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d7e76-277">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

