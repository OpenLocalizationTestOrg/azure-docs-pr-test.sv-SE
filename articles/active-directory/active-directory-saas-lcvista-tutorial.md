---
title: "Självstudier: Azure Active Directory-integrering med LCVista | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och LCVista."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8db80d6e-3275-419f-aa39-6115a7bc9800
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: c19f81da495eb7116b62797d1755d312a23f3805
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lcvista"></a><span data-ttu-id="91b6d-103">Självstudier: Azure Active Directory-integrering med LCVista</span><span class="sxs-lookup"><span data-stu-id="91b6d-103">Tutorial: Azure Active Directory integration with LCVista</span></span>

<span data-ttu-id="91b6d-104">I kursen får lära du att integrera LCVista med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="91b6d-104">In this tutorial, you learn how to integrate LCVista with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="91b6d-105">Integrera LCVista med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="91b6d-105">Integrating LCVista with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="91b6d-106">Du kan styra i Azure AD som har åtkomst till LCVista</span><span class="sxs-lookup"><span data-stu-id="91b6d-106">You can control in Azure AD who has access to LCVista</span></span>
- <span data-ttu-id="91b6d-107">Du kan aktivera användarna att automatiskt hämta loggat in på LCVista (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="91b6d-107">You can enable your users to automatically get signed-on to LCVista (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="91b6d-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91b6d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="91b6d-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="91b6d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91b6d-110">Krav</span><span class="sxs-lookup"><span data-stu-id="91b6d-110">Prerequisites</span></span>

<span data-ttu-id="91b6d-111">För att konfigurera Azure AD-integrering med LCVista, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="91b6d-111">To configure Azure AD integration with LCVista, you need the following items:</span></span>

- <span data-ttu-id="91b6d-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="91b6d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="91b6d-113">En LCVista enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="91b6d-113">A LCVista single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="91b6d-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="91b6d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="91b6d-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="91b6d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="91b6d-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="91b6d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="91b6d-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="91b6d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="91b6d-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="91b6d-118">Scenario description</span></span>
<span data-ttu-id="91b6d-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="91b6d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="91b6d-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="91b6d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="91b6d-121">Att lägga till LCVista från galleriet</span><span class="sxs-lookup"><span data-stu-id="91b6d-121">Adding LCVista from the gallery</span></span>
2. <span data-ttu-id="91b6d-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91b6d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lcvista-from-the-gallery"></a><span data-ttu-id="91b6d-123">Att lägga till LCVista från galleriet</span><span class="sxs-lookup"><span data-stu-id="91b6d-123">Adding LCVista from the gallery</span></span>
<span data-ttu-id="91b6d-124">Du måste lägga till LCVista från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av LCVista i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="91b6d-124">To configure the integration of LCVista into Azure AD, you need to add LCVista from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="91b6d-125">**Utför följande steg för att lägga till LCVista från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="91b6d-125">**To add LCVista from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="91b6d-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91b6d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="91b6d-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="91b6d-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="91b6d-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91b6d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="91b6d-133">I sökrutan skriver **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-133">In the search box, type **LCVista**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_search.png)

5. <span data-ttu-id="91b6d-135">Välj i resultatpanelen **LCVista**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="91b6d-135">In the results panel, select **LCVista**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="91b6d-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91b6d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="91b6d-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med LCVista baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="91b6d-138">In this section, you configure and test Azure AD single sign-on with LCVista based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="91b6d-139">Azure AD måste du känna till användaren i LCVista motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="91b6d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LCVista is to a user in Azure AD.</span></span> <span data-ttu-id="91b6d-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i LCVista upprättas.</span><span class="sxs-lookup"><span data-stu-id="91b6d-140">In other words, a link relationship between an Azure AD user and the related user in LCVista needs to be established.</span></span>

<span data-ttu-id="91b6d-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i LCVista.</span><span class="sxs-lookup"><span data-stu-id="91b6d-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in LCVista.</span></span>

<span data-ttu-id="91b6d-142">Om du vill konfigurera och testa Azure AD enkel inloggning med LCVista, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="91b6d-142">To configure and test Azure AD single sign-on with LCVista, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="91b6d-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="91b6d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="91b6d-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91b6d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="91b6d-145">**[Skapa en testanvändare LCVista](#creating-a-lcvista-test-user)**  – du har en motsvarighet för Britta Simon i LCVista som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="91b6d-145">**[Creating a LCVista test user](#creating-a-lcvista-test-user)** - to have a counterpart of Britta Simon in LCVista that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="91b6d-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91b6d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="91b6d-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="91b6d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="91b6d-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91b6d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="91b6d-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt LCVista program.</span><span class="sxs-lookup"><span data-stu-id="91b6d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LCVista application.</span></span>

<span data-ttu-id="91b6d-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med LCVista:**</span><span class="sxs-lookup"><span data-stu-id="91b6d-150">**To configure Azure AD single sign-on with LCVista, perform the following steps:**</span></span>

1. <span data-ttu-id="91b6d-151">I Azure-portalen på den **LCVista** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-151">In the Azure portal, on the **LCVista** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="91b6d-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91b6d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_samlbase.png)

3. <span data-ttu-id="91b6d-155">På den **LCVista domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="91b6d-155">On the **LCVista Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_url.png)

    <span data-ttu-id="91b6d-157">a.</span><span class="sxs-lookup"><span data-stu-id="91b6d-157">a.</span></span> <span data-ttu-id="91b6d-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.lcvista.com/rainier/login`</span><span class="sxs-lookup"><span data-stu-id="91b6d-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.lcvista.com/rainier/login`</span></span>

    <span data-ttu-id="91b6d-159">b.</span><span class="sxs-lookup"><span data-stu-id="91b6d-159">b.</span></span> <span data-ttu-id="91b6d-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.lcvista.com`</span><span class="sxs-lookup"><span data-stu-id="91b6d-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.lcvista.com`</span></span> 
     
    > [!NOTE] 
    > <span data-ttu-id="91b6d-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="91b6d-161">These values are not the real.</span></span> <span data-ttu-id="91b6d-162">Uppdatera dessa värden med den faktiska identifierare och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="91b6d-162">Update these values with the actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="91b6d-163">Kontakta [LCVista klienten supportteamet](https://lcvista.com/contact) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="91b6d-163">Contact [LCVista Client support team](https://lcvista.com/contact) to get these values.</span></span> 

4. <span data-ttu-id="91b6d-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="91b6d-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_certificate.png) 

5. <span data-ttu-id="91b6d-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="91b6d-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="91b6d-168">På den **LCVista Configuration** klickar du på **konfigurera LCVista** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="91b6d-168">On the **LCVista Configuration** section, click **Configure LCVista** to open **Configure sign-on** window.</span></span> <span data-ttu-id="91b6d-169">Kopiera den **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="91b6d-169">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_configure.png) 

7.  <span data-ttu-id="91b6d-171">Logga in på ditt LCVista program som administratör.</span><span class="sxs-lookup"><span data-stu-id="91b6d-171">Sign on to your LCVista application as an administrator.</span></span>

8. <span data-ttu-id="91b6d-172">I den **SAML-Config** avsnitt, kontrollera den **aktivera SAML inloggningen** och ange information som anges i under bilden.</span><span class="sxs-lookup"><span data-stu-id="91b6d-172">In the **SAML Config** section, check the **Enable SAML login** and enter the details as mentioned in below image.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_config.png)

    <span data-ttu-id="91b6d-174">a.</span><span class="sxs-lookup"><span data-stu-id="91b6d-174">a.</span></span> <span data-ttu-id="91b6d-175">Klistra in den **utfärdar-URL** som du har kopierat från Azure AD i den **enhets-ID** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="91b6d-175">Paste the **Issuer URL** which you have copied from Azure AD in the **Entity ID** section.</span></span> 

    <span data-ttu-id="91b6d-176">b.</span><span class="sxs-lookup"><span data-stu-id="91b6d-176">b.</span></span> <span data-ttu-id="91b6d-177">Klistra in den **inloggning tjänst-URL för enkel** som du har kopierat från Azure AD i den **URL** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="91b6d-177">Paste the **Single Sign-On Service URL** which you have copied from Azure AD in the **URL** section.</span></span>

    <span data-ttu-id="91b6d-178">c.</span><span class="sxs-lookup"><span data-stu-id="91b6d-178">c.</span></span> <span data-ttu-id="91b6d-179">Från Metadata (XML) som du har hämtat från Azure-portalen, Kopiera värdet **X509Certificate** och klistra in den i den **x509 certifikat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="91b6d-179">From Metadata (XML) which you have downloaded from Azure portal, copy the value **X509Certificate** and paste it in the **x509 Certificate** section.</span></span>

    <span data-ttu-id="91b6d-180">d.</span><span class="sxs-lookup"><span data-stu-id="91b6d-180">d.</span></span> <span data-ttu-id="91b6d-181">I den **förnamn attributet** textruta klistra in värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span><span class="sxs-lookup"><span data-stu-id="91b6d-181">In the **First name attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.</span></span>

    <span data-ttu-id="91b6d-182">e.</span><span class="sxs-lookup"><span data-stu-id="91b6d-182">e.</span></span> <span data-ttu-id="91b6d-183">I den **senaste namnattributet** textruta klistra in värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span><span class="sxs-lookup"><span data-stu-id="91b6d-183">In the **Last name attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.</span></span>

    <span data-ttu-id="91b6d-184">f.</span><span class="sxs-lookup"><span data-stu-id="91b6d-184">f.</span></span> <span data-ttu-id="91b6d-185">I den **e-attributet** textruta klistra in värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="91b6d-185">In the **Email attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="91b6d-186">g.</span><span class="sxs-lookup"><span data-stu-id="91b6d-186">g.</span></span> <span data-ttu-id="91b6d-187">I den **Username-attributet** textruta klistra in värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="91b6d-187">In the **Username attribute** textbox, paste the value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>

    <span data-ttu-id="91b6d-188">e.</span><span class="sxs-lookup"><span data-stu-id="91b6d-188">e.</span></span> <span data-ttu-id="91b6d-189">Klicka på **spara** spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="91b6d-189">Click **Save** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="91b6d-190">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="91b6d-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="91b6d-191">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="91b6d-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="91b6d-192">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="91b6d-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="91b6d-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="91b6d-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="91b6d-194">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91b6d-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="91b6d-196">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="91b6d-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="91b6d-197">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="91b6d-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="91b6d-199">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="91b6d-201">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91b6d-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="91b6d-203">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="91b6d-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lcvista-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="91b6d-205">a.</span><span class="sxs-lookup"><span data-stu-id="91b6d-205">a.</span></span> <span data-ttu-id="91b6d-206">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="91b6d-207">b.</span><span class="sxs-lookup"><span data-stu-id="91b6d-207">b.</span></span> <span data-ttu-id="91b6d-208">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="91b6d-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="91b6d-209">c.</span><span class="sxs-lookup"><span data-stu-id="91b6d-209">c.</span></span> <span data-ttu-id="91b6d-210">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="91b6d-211">d.</span><span class="sxs-lookup"><span data-stu-id="91b6d-211">d.</span></span> <span data-ttu-id="91b6d-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-212">Click **Create**.</span></span>
 
### <a name="creating-a-lcvista-test-user"></a><span data-ttu-id="91b6d-213">Skapa en testanvändare LCVista</span><span class="sxs-lookup"><span data-stu-id="91b6d-213">Creating a LCVista test user</span></span>

<span data-ttu-id="91b6d-214">I det här avsnittet skapar du en användare som kallas Britta Simon i LCVista.</span><span class="sxs-lookup"><span data-stu-id="91b6d-214">In this section, you create a user called Britta Simon in LCVista.</span></span> <span data-ttu-id="91b6d-215">Du behöver kontakta [LCVista klienten supportteamet](https://lcvista.com/contact) att lägga till användare i LCVista-programmet.</span><span class="sxs-lookup"><span data-stu-id="91b6d-215">You need to contact [LCVista Client support team](https://lcvista.com/contact) to add the users in the LCVista application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="91b6d-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="91b6d-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="91b6d-217">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till LCVista.</span><span class="sxs-lookup"><span data-stu-id="91b6d-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LCVista.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="91b6d-219">**Om du vill tilldela LCVista Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91b6d-219">**To assign Britta Simon to LCVista, perform the following steps:**</span></span>

1. <span data-ttu-id="91b6d-220">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="91b6d-222">Välj i listan med program **LCVista**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-222">In the applications list, select **LCVista**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lcvista-tutorial/tutorial_lcvista_app.png) 

3. <span data-ttu-id="91b6d-224">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="91b6d-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="91b6d-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="91b6d-226">Click **Add** button.</span></span> <span data-ttu-id="91b6d-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91b6d-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="91b6d-229">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="91b6d-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="91b6d-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91b6d-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="91b6d-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91b6d-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="91b6d-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91b6d-232">Testing single sign-on</span></span>

<span data-ttu-id="91b6d-233">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="91b6d-233">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="91b6d-234">Klicka på panelen LCVista på åtkomstpanelen, du omdirigeras till organisation inloggning på sidan.</span><span class="sxs-lookup"><span data-stu-id="91b6d-234">Click the LCVista tile in the Access Panel, you will be redirected to Organization sign on page.</span></span> <span data-ttu-id="91b6d-235">Efter genomförd inloggning du kommer att loggat in på ditt LCVista program.</span><span class="sxs-lookup"><span data-stu-id="91b6d-235">After successful login, you will be signed-on to your LCVista application.</span></span> <span data-ttu-id="91b6d-236">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="91b6d-236">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="91b6d-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="91b6d-237">Additional resources</span></span>

* [<span data-ttu-id="91b6d-238">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="91b6d-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="91b6d-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="91b6d-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lcvista-tutorial/tutorial_general_203.png

