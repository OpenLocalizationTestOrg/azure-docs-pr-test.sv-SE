---
title: "Självstudier: Azure Active Directory-integrering med vice | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och vice."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 51aed908208b7a40ea2ab710dffe84370b573991
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a><span data-ttu-id="5d5f3-103">Självstudier: Azure Active Directory-integrering med vice</span><span class="sxs-lookup"><span data-stu-id="5d5f3-103">Tutorial: Azure Active Directory integration with Deputy</span></span>

<span data-ttu-id="5d5f3-104">I kursen får lära du att integrera vice med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="5d5f3-104">In this tutorial, you learn how to integrate Deputy with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5d5f3-105">Integrera vice med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-105">Integrating Deputy with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="5d5f3-106">Du kan styra i Azure AD som har åtkomst till vice</span><span class="sxs-lookup"><span data-stu-id="5d5f3-106">You can control in Azure AD who has access to Deputy</span></span>
- <span data-ttu-id="5d5f3-107">Du kan aktivera användarna att automatiskt hämta loggat in på vice (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="5d5f3-107">You can enable your users to automatically get signed-on to Deputy (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5d5f3-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="5d5f3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="5d5f3-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5d5f3-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d5f3-110">Krav</span><span class="sxs-lookup"><span data-stu-id="5d5f3-110">Prerequisites</span></span>

<span data-ttu-id="5d5f3-111">Om du vill konfigurera Azure AD-integrering med vice behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-111">To configure Azure AD integration with Deputy, you need the following items:</span></span>

- <span data-ttu-id="5d5f3-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="5d5f3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5d5f3-113">En vice enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="5d5f3-113">A Deputy single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="5d5f3-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="5d5f3-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5d5f3-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="5d5f3-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5d5f3-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5d5f3-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="5d5f3-118">Scenario description</span></span>
<span data-ttu-id="5d5f3-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5d5f3-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5d5f3-121">Att lägga till vice från galleriet</span><span class="sxs-lookup"><span data-stu-id="5d5f3-121">Adding Deputy from the gallery</span></span>
2. <span data-ttu-id="5d5f3-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5d5f3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-deputy-from-the-gallery"></a><span data-ttu-id="5d5f3-123">Att lägga till vice från galleriet</span><span class="sxs-lookup"><span data-stu-id="5d5f3-123">Adding Deputy from the gallery</span></span>
<span data-ttu-id="5d5f3-124">Du måste lägga till vice från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av vice i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-124">To configure the integration of Deputy into Azure AD, you need to add Deputy from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5d5f3-125">**Utför följande steg för att lägga till vice från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="5d5f3-125">**To add Deputy from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5d5f3-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5d5f3-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="5d5f3-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="5d5f3-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="5d5f3-133">I sökrutan skriver **vice**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-133">In the search box, type **Deputy**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_search.png)

5. <span data-ttu-id="5d5f3-135">Välj i resultatpanelen **vice**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-135">In the results panel, select **Deputy**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5d5f3-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5d5f3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5d5f3-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med vice baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-138">In this section, you configure and test Azure AD single sign-on with Deputy based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="5d5f3-139">Azure AD måste du känna till användaren i vice motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Deputy is to a user in Azure AD.</span></span> <span data-ttu-id="5d5f3-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i vice upprättas.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-140">In other words, a link relationship between an Azure AD user and the related user in Deputy needs to be established.</span></span>

<span data-ttu-id="5d5f3-141">I vice, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-141">In Deputy, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="5d5f3-142">Om du vill konfigurera och testa Azure AD enkel inloggning med vice, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-142">To configure and test Azure AD single sign-on with Deputy, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5d5f3-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5d5f3-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5d5f3-145">**[Skapa en testanvändare vice](#creating-a-deputy-test-user)**  – har en motsvarighet för Britta Simon vice som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-145">**[Creating a Deputy test user](#creating-a-deputy-test-user)** - to have a counterpart of Britta Simon in Deputy that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="5d5f3-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5d5f3-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5d5f3-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5d5f3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5d5f3-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet vice.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Deputy application.</span></span>

<span data-ttu-id="5d5f3-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med vice:**</span><span class="sxs-lookup"><span data-stu-id="5d5f3-150">**To configure Azure AD single sign-on with Deputy, perform the following steps:**</span></span>

1. <span data-ttu-id="5d5f3-151">I Azure-portalen på den **vice** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-151">In the Azure portal, on the **Deputy** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="5d5f3-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_samlbase.png)

3. <span data-ttu-id="5d5f3-155">På den **URL: er och vice domän** om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-155">On the **Deputy Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url1.png)

    <span data-ttu-id="5d5f3-157">a.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-157">a.</span></span> <span data-ttu-id="5d5f3-158">I den **identifierare** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    |  |
    | ----|
    | `https://<subdomain>.<region>.au.deputy.com` |
    | `https://<subdomain>.<region>.ent-au.deputy.com` |
    | `https://<subdomain>.<region>.na.deputy.com`|
    | `https://<subdomain>.<region>.ent-na.deputy.com`|
    | `https://<subdomain>.<region>.eu.deputy.com` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com` |
    | `https://<subdomain>.<region>.as.deputy.com` |
    | `https://<subdomain>.<region>.ent-as.deputy.com` |
    | `https://<subdomain>.<region>.la.deputy.com` |
    | `https://<subdomain>.<region>.ent-la.deputy.com` |
    | `https://<subdomain>.<region>.af.deputy.com` |
    | `https://<subdomain>.<region>.ent-af.deputy.com` |
    | `https://<subdomain>.<region>.an.deputy.com` |
    | `https://<subdomain>.<region>.ent-an.deputy.com` |
    | `https://<subdomain>.<region>.deputy.com` |

    <span data-ttu-id="5d5f3-159">b.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-159">b.</span></span> <span data-ttu-id="5d5f3-160">I den **Reply URL** textruta Skriv en URL med följande mönster:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |----|
    | `https://<subdomain>.<region>.au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-au.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-na.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-eu.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-as.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-la.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-af.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.ent-an.deputy.com/exec/devapp/samlacs.` |
    | `https://<subdomain>.<region>.deputy.com/exec/devapp/samlacs.` |

4. <span data-ttu-id="5d5f3-161">Kontrollera **visa avancerade inställningar för URL: en**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="5d5f3-162">Om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_url2.png)

    <span data-ttu-id="5d5f3-164">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<your-subdomain>.<region>.deputy.com`</span><span class="sxs-lookup"><span data-stu-id="5d5f3-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<your-subdomain>.<region>.deputy.com`</span></span>
    
    >[!NOTE]
    > <span data-ttu-id="5d5f3-165">Vice region suffix är valfritt eller den ska använda något av följande: Australien | na | Europa | som | la | af | en | ent Australien | ent na | ent Europa | ent-som | överordnad la | överordnad af | en överordnad</span><span class="sxs-lookup"><span data-stu-id="5d5f3-165">Deputy region suffix is optional, or it should use one of these: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5d5f3-166">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-166">These values are not real.</span></span> <span data-ttu-id="5d5f3-167">Uppdatera dessa värden med den faktiska identifierare Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-167">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="5d5f3-168">Kontakta [vice supportteamet](https://www.deputy.com/call-centers-customer-support-scheduling-software) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-168">Contact [Deputy support team](https://www.deputy.com/call-centers-customer-support-scheduling-software) to get these values.</span></span> 

5. <span data-ttu-id="5d5f3-169">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-169">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_certificate.png) 

6. <span data-ttu-id="5d5f3-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="5d5f3-173">På den **vice Configuration** klickar du på **konfigurera vice** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-173">On the **Deputy Configuration** section, click **Configure Deputy** to open **Configure sign-on** window.</span></span> <span data-ttu-id="5d5f3-174">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="5d5f3-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_configure.png) 

8. <span data-ttu-id="5d5f3-176">Navigera till följande URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span><span class="sxs-lookup"><span data-stu-id="5d5f3-176">Navigate to the following URL:[https://(your-subdomain).deputy.com/exec/config/system_config]( https://(your-subdomain).deputy.com/exec/config/system_config).</span></span> <span data-ttu-id="5d5f3-177">Gå till **säkerhetsinställningar** och på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-177">Go to **Security Settings** and click **Edit**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

9. <span data-ttu-id="5d5f3-179">På den här **säkerhetsinställningar** utför nedanstående steg.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-179">On this **Security Settings** page, perform below steps.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)
    
    <span data-ttu-id="5d5f3-181">a.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-181">a.</span></span> <span data-ttu-id="5d5f3-182">Aktivera **inloggning via sociala medier**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-182">Enable **Social Login**.</span></span>
   
    <span data-ttu-id="5d5f3-183">b.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-183">b.</span></span> <span data-ttu-id="5d5f3-184">Öppna din Base64-kodat certifikat hämtas från Azure-portalen i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **OpenSSL certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-184">Open your Base64 encoded certificate downloaded from Azure portal in notepad, copy the content of it into your clipboard, and then paste it to the **OpenSSL Certificate** textbox.</span></span>
   
    <span data-ttu-id="5d5f3-185">c.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-185">c.</span></span> <span data-ttu-id="5d5f3-186">Ange i textrutan SAML SSO-URL`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span><span class="sxs-lookup"><span data-stu-id="5d5f3-186">In the SAML SSO URL textbox, type `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`</span></span>
    
    <span data-ttu-id="5d5f3-187">d.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-187">d.</span></span> <span data-ttu-id="5d5f3-188">I textrutan URL för SAML SSO ersätter `<your subdomain>` med din underdomänen.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-188">In the SAML SSO URL textbox, replace `<your subdomain>` with your subdomain.</span></span>
   
    <span data-ttu-id="5d5f3-189">e.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-189">e.</span></span> <span data-ttu-id="5d5f3-190">I textrutan URL för SAML SSO ersätter `<saml sso url>` med den **SAML inloggning tjänst-URL för enkel** du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-190">In the SAML SSO URL textbox, replace `<saml sso url>` with the **SAML Single Sign-On Service URL** you have copied from the Azure portal.</span></span>
   
    <span data-ttu-id="5d5f3-191">f.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-191">f.</span></span> <span data-ttu-id="5d5f3-192">Klicka på **Spara inställningar**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-192">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="5d5f3-193">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="5d5f3-193">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="5d5f3-194">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-194">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="5d5f3-195">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="5d5f3-195">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5d5f3-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="5d5f3-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="5d5f3-197">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-197">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="5d5f3-199">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="5d5f3-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5d5f3-200">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-200">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5d5f3-202">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-202">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5d5f3-204">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-204">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5d5f3-206">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5d5f3-208">a.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-208">a.</span></span> <span data-ttu-id="5d5f3-209">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5d5f3-210">b.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-210">b.</span></span> <span data-ttu-id="5d5f3-211">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5d5f3-212">c.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-212">c.</span></span> <span data-ttu-id="5d5f3-213">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="5d5f3-214">d.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-214">d.</span></span> <span data-ttu-id="5d5f3-215">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-215">Click **Create**.</span></span>
 
### <a name="creating-a-deputy-test-user"></a><span data-ttu-id="5d5f3-216">Skapa en vice testanvändare</span><span class="sxs-lookup"><span data-stu-id="5d5f3-216">Creating a Deputy test user</span></span>

<span data-ttu-id="5d5f3-217">Om du vill aktivera Azure AD-användare kan logga in på vice etableras de i vice.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-217">To enable Azure AD users to log in to Deputy, they must be provisioned into Deputy.</span></span> <span data-ttu-id="5d5f3-218">Vid vice är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-218">In case of Deputy, provisioning is a manual task.</span></span>

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="5d5f3-219">Utför följande steg om du vill konfigurera ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="5d5f3-219">To provision a user account, perform the following steps:</span></span>
1. <span data-ttu-id="5d5f3-220">Logga in på webbplatsen vice företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-220">Log in to your Deputy company site as an administrator.</span></span>

2. <span data-ttu-id="5d5f3-221">I det övre navigeringsfönstret, klickar du på **personer**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-221">On the top navigation pane, click **People**.</span></span>
   
   <span data-ttu-id="5d5f3-222">![Personer](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "personer")</span><span class="sxs-lookup"><span data-stu-id="5d5f3-222">![People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "People")</span></span>

3. <span data-ttu-id="5d5f3-223">Klicka på den **lägga till personer** och klicka sedan på **lägga till en enda person**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-223">Click the **Add People** button and click **Add a single person**.</span></span>
   
   <span data-ttu-id="5d5f3-224">![Lägg till personer](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "lägga till personer")</span><span class="sxs-lookup"><span data-stu-id="5d5f3-224">![Add People](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Add People")</span></span>

4. <span data-ttu-id="5d5f3-225">Utför följande steg och klicka på **Spara & bjuda in**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-225">Perform the following steps and click **Save & Invite**.</span></span>
   
   <span data-ttu-id="5d5f3-226">![Ny användare](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "ny användare")</span><span class="sxs-lookup"><span data-stu-id="5d5f3-226">![New User](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "New User")</span></span>

   <span data-ttu-id="5d5f3-227">a.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-227">a.</span></span> <span data-ttu-id="5d5f3-228">I den **namn** textruta, ange namnet på användaren som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-228">In the **Name** textbox, type name of the user like **BrittaSimon**.</span></span>
   
   <span data-ttu-id="5d5f3-229">b.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-229">b.</span></span> <span data-ttu-id="5d5f3-230">I den **e-post** textruta skriver en Azure AD-kontot som du vill etablera e-postadress.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-230">In the **Email** textbox, type the email address of an Azure AD account you want to provision.</span></span>
   
   <span data-ttu-id="5d5f3-231">c.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-231">c.</span></span> <span data-ttu-id="5d5f3-232">I den **fungerar på** textruta typnamn företag.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-232">In the **Work at** textbox, type the business name.</span></span>
   
   <span data-ttu-id="5d5f3-233">d.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-233">d.</span></span> <span data-ttu-id="5d5f3-234">Klicka på **Spara & bjuda in** knappen.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-234">Click **Save & Invite** button.</span></span>

5. <span data-ttu-id="5d5f3-235">AAD kontoinnehavaren får ett e-postmeddelande och följer en länk för att bekräfta sina konton innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-235">The AAD account holder receives an email and follows a link to confirm their account before it becomes active.</span></span> <span data-ttu-id="5d5f3-236">Du kan använda något annat vice användarens konto skapas verktyg eller API: er som tillhandahålls av vice etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-236">You can use any other Deputy user account creation tools or APIs provided by Deputy to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="5d5f3-237">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="5d5f3-237">Assigning the Azure AD test user</span></span>

<span data-ttu-id="5d5f3-238">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till vice.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-238">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Deputy.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="5d5f3-240">**Om du vill tilldela vice Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="5d5f3-240">**To assign Britta Simon to Deputy, perform the following steps:**</span></span>

1. <span data-ttu-id="5d5f3-241">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-241">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="5d5f3-243">Välj i listan med program **vice**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-243">In the applications list, select **Deputy**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_app.png) 

3. <span data-ttu-id="5d5f3-245">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-245">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="5d5f3-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-247">Click **Add** button.</span></span> <span data-ttu-id="5d5f3-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="5d5f3-250">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-250">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="5d5f3-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5d5f3-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="5d5f3-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="5d5f3-253">Testing single sign-on</span></span>

<span data-ttu-id="5d5f3-254">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-254">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="5d5f3-255">När du klickar på panelen vice på åtkomstpanelen du bör få automatiskt loggat in på ditt vice program.</span><span class="sxs-lookup"><span data-stu-id="5d5f3-255">When you click the Deputy tile in the Access Panel, you should get automatically signed-on to your Deputy application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5d5f3-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5d5f3-256">Additional resources</span></span>

* [<span data-ttu-id="5d5f3-257">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d5f3-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5d5f3-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="5d5f3-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png

