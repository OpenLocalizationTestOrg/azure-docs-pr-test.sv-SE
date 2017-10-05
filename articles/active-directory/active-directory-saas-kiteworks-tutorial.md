---
title: "Självstudier: Azure Active Directory-integrering med Kiteworks | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Kiteworks."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f7984aaf-ab1f-4a85-9646-a9523f5275d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jeedes
ms.openlocfilehash: 2fd9b346cb6d838069ef94ee9c2a8d113f22779c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kiteworks"></a><span data-ttu-id="6c5d4-103">Självstudier: Azure Active Directory-integrering med Kiteworks</span><span class="sxs-lookup"><span data-stu-id="6c5d4-103">Tutorial: Azure Active Directory integration with Kiteworks</span></span>

<span data-ttu-id="6c5d4-104">I kursen får lära du att integrera Kiteworks med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6c5d4-104">In this tutorial, you learn how to integrate Kiteworks with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6c5d4-105">Integrera Kiteworks med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-105">Integrating Kiteworks with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="6c5d4-106">Du kan styra i Azure AD som har åtkomst till Kiteworks</span><span class="sxs-lookup"><span data-stu-id="6c5d4-106">You can control in Azure AD who has access to Kiteworks</span></span>
- <span data-ttu-id="6c5d4-107">Du kan aktivera användarna att automatiskt hämta loggat in på Kiteworks (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6c5d4-107">You can enable your users to automatically get signed-on to Kiteworks (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6c5d4-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6c5d4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="6c5d4-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6c5d4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c5d4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6c5d4-110">Prerequisites</span></span>

<span data-ttu-id="6c5d4-111">För att konfigurera Azure AD-integrering med Kiteworks, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-111">To configure Azure AD integration with Kiteworks, you need the following items:</span></span>

- <span data-ttu-id="6c5d4-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6c5d4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6c5d4-113">En Kiteworks enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="6c5d4-113">A Kiteworks single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6c5d4-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6c5d4-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6c5d4-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6c5d4-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6c5d4-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6c5d4-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6c5d4-118">Scenario description</span></span>
<span data-ttu-id="6c5d4-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6c5d4-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6c5d4-121">Att lägga till Kiteworks från galleriet</span><span class="sxs-lookup"><span data-stu-id="6c5d4-121">Adding Kiteworks from the gallery</span></span>
2. <span data-ttu-id="6c5d4-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c5d4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kiteworks-from-the-gallery"></a><span data-ttu-id="6c5d4-123">Att lägga till Kiteworks från galleriet</span><span class="sxs-lookup"><span data-stu-id="6c5d4-123">Adding Kiteworks from the gallery</span></span>
<span data-ttu-id="6c5d4-124">Du måste lägga till Kiteworks från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Kiteworks i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-124">To configure the integration of Kiteworks into Azure AD, you need to add Kiteworks from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="6c5d4-125">**Utför följande steg för att lägga till Kiteworks från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="6c5d4-125">**To add Kiteworks from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="6c5d4-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6c5d4-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="6c5d4-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6c5d4-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6c5d4-133">I sökrutan skriver **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-133">In the search box, type **Kiteworks**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_search.png)

5. <span data-ttu-id="6c5d4-135">Välj i resultatpanelen **Kiteworks**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-135">In the results panel, select **Kiteworks**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6c5d4-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c5d4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6c5d4-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Kiteworks baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-138">In this section, you configure and test Azure AD single sign-on with Kiteworks based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6c5d4-139">Azure AD måste du känna till användaren i Kiteworks motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Kiteworks is to a user in Azure AD.</span></span> <span data-ttu-id="6c5d4-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Kiteworks upprättas.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-140">In other words, a link relationship between an Azure AD user and the related user in Kiteworks needs to be established.</span></span>

<span data-ttu-id="6c5d4-141">I Kiteworks, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-141">In Kiteworks, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="6c5d4-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Kiteworks, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-142">To configure and test Azure AD single sign-on with Kiteworks, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="6c5d4-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="6c5d4-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6c5d4-145">**[Skapa en testanvändare Kiteworks](#creating-a-kiteworks-test-user)**  – du har en motsvarighet för Britta Simon i Kiteworks som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-145">**[Creating a Kiteworks test user](#creating-a-kiteworks-test-user)** - to have a counterpart of Britta Simon in Kiteworks that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="6c5d4-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6c5d4-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6c5d4-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c5d4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6c5d4-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Kiteworks program.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kiteworks application.</span></span>

<span data-ttu-id="6c5d4-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Kiteworks:**</span><span class="sxs-lookup"><span data-stu-id="6c5d4-150">**To configure Azure AD single sign-on with Kiteworks, perform the following steps:**</span></span>

1. <span data-ttu-id="6c5d4-151">I Azure-portalen på den **Kiteworks** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-151">In the Azure portal, on the **Kiteworks** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6c5d4-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_samlbase.png)

3. <span data-ttu-id="6c5d4-155">På den **Kiteworks domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-155">On the **Kiteworks Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_url.png)

    <span data-ttu-id="6c5d4-157">a.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-157">a.</span></span> <span data-ttu-id="6c5d4-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<subdomain>.kiteworks.com`</span><span class="sxs-lookup"><span data-stu-id="6c5d4-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.kiteworks.com`</span></span>

    <span data-ttu-id="6c5d4-159">b.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-159">b.</span></span> <span data-ttu-id="6c5d4-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span><span class="sxs-lookup"><span data-stu-id="6c5d4-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.kiteworks.com/sp/module.php/saml/sp/saml2-acs.php/sp-sso`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="6c5d4-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-161">These values are not real.</span></span> <span data-ttu-id="6c5d4-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="6c5d4-163">Kontakta [Kiteworks klienten supportteamet](http://accellion.com/support) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-163">Contact [Kiteworks Client support team](http://accellion.com/support) to get these values.</span></span> 
 
4. <span data-ttu-id="6c5d4-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_certificate.png) 

5. <span data-ttu-id="6c5d4-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6c5d4-168">På den **Kiteworks Configuration** klickar du på **konfigurera Kiteworks** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-168">On the **Kiteworks Configuration** section, click **Configure Kiteworks** to open **Configure sign-on** window.</span></span> <span data-ttu-id="6c5d4-169">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6c5d4-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_configure.png) 

7. <span data-ttu-id="6c5d4-171">Logga in på webbplatsen Kiteworks företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-171">Sign on to your Kiteworks company site as an administrator.</span></span>

8. <span data-ttu-id="6c5d4-172">Klicka på i verktygsfältet högst upp **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-172">In the toolbar on the top, click **Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_06.png) 

9. <span data-ttu-id="6c5d4-174">I den **autentisering och auktorisering** klickar du på **SSO installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-174">In the **Authentication and Authorization** section, click **SSO Setup**.</span></span> 
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_07.png)
 
10. <span data-ttu-id="6c5d4-176">Utför följande steg på konfigurationssidan för enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-176">On the SSO Setup page, perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_09.png)   

    <span data-ttu-id="6c5d4-178">a.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-178">a.</span></span> <span data-ttu-id="6c5d4-179">Välj **autentisera via enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-179">Select **Authenticate via SSO**.</span></span>

    <span data-ttu-id="6c5d4-180">b.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-180">b.</span></span> <span data-ttu-id="6c5d4-181">Välj **initiera AuthnRequest**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-181">Select **Initiate AuthnRequest**.</span></span>

    <span data-ttu-id="6c5d4-182">c.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-182">c.</span></span> <span data-ttu-id="6c5d4-183">I den **IDP enhets-ID** textruta klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-183">In the **IDP Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="6c5d4-184">d.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-184">d.</span></span> <span data-ttu-id="6c5d4-185">I den **inloggning tjänst-URL för enkel** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-185">In the **Single Sign-On Service URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6c5d4-186">e.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-186">e.</span></span> <span data-ttu-id="6c5d4-187">I den **tjänst-URL för enkel logga ut** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-187">In the **Single Logout Service URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6c5d4-188">f.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-188">f.</span></span> <span data-ttu-id="6c5d4-189">Öppna din hämtat certifikat i anteckningar, kopiera innehållet och klistrar in det i den **RSA offentligt nyckelcertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-189">Open your downloaded certificate in Notepad, copy the content, and then paste it into the **RSA Public Key Certificate** textbox.</span></span>
 
    <span data-ttu-id="6c5d4-190">g.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-190">g.</span></span> <span data-ttu-id="6c5d4-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="6c5d4-192">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="6c5d4-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="6c5d4-193">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="6c5d4-194">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6c5d4-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6c5d4-195">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6c5d4-195">Creating an Azure AD test user</span></span>
<span data-ttu-id="6c5d4-196">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-196">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6c5d4-198">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="6c5d4-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="6c5d4-199">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-199">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6c5d4-201">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-201">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6c5d4-203">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-203">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6c5d4-205">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="6c5d4-205">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-kiteworks-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6c5d4-207">a.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-207">a.</span></span> <span data-ttu-id="6c5d4-208">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-208">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6c5d4-209">b.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-209">b.</span></span> <span data-ttu-id="6c5d4-210">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-210">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6c5d4-211">c.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-211">c.</span></span> <span data-ttu-id="6c5d4-212">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-212">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="6c5d4-213">d.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-213">d.</span></span> <span data-ttu-id="6c5d4-214">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-214">Click **Create**.</span></span>
 
### <a name="creating-a-kiteworks-test-user"></a><span data-ttu-id="6c5d4-215">Skapa en testanvändare Kiteworks</span><span class="sxs-lookup"><span data-stu-id="6c5d4-215">Creating a Kiteworks test user</span></span>

<span data-ttu-id="6c5d4-216">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-216">The objective of this section is to create a user called Britta Simon in Kiteworks.</span></span>

<span data-ttu-id="6c5d4-217">Kiteworks stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-217">Kiteworks supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="6c5d4-218">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-218">There is no action item for you in this section.</span></span> <span data-ttu-id="6c5d4-219">En ny användare skapas under ett försök att komma åt Kitewors om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-219">A new user is created during an attempt to access Kitewors if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="6c5d4-220">Om du behöver skapa en användare manuellt, måste du kontakta den [Kiteworks supportteam](http://accellion.com/support).</span><span class="sxs-lookup"><span data-stu-id="6c5d4-220">If you need to create a user manually, you need to contact the [Kiteworks support team](http://accellion.com/support).</span></span>
 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="6c5d4-221">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="6c5d4-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="6c5d4-222">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Kiteworks.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kiteworks.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6c5d4-224">**Om du vill tilldela Kiteworks Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6c5d4-224">**To assign Britta Simon to Kiteworks, perform the following steps:**</span></span>

1. <span data-ttu-id="6c5d4-225">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6c5d4-227">Välj i listan med program **Kiteworks**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-227">In the applications list, select **Kiteworks**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-kiteworks-tutorial/tutorial_kiteworks_app.png) 

3. <span data-ttu-id="6c5d4-229">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6c5d4-231">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-231">Click **Add** button.</span></span> <span data-ttu-id="6c5d4-232">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6c5d4-234">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="6c5d4-235">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6c5d4-236">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6c5d4-237">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6c5d4-237">Testing single sign-on</span></span>

<span data-ttu-id="6c5d4-238">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-238">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="6c5d4-239">När du klickar på panelen Kiteworks på åtkomstpanelen du bör få automatiskt loggat in på ditt Kiteworks program.</span><span class="sxs-lookup"><span data-stu-id="6c5d4-239">When you click the Kiteworks tile in the Access Panel, you should get automatically signed-on to your Kiteworks application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6c5d4-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6c5d4-240">Additional resources</span></span>

* [<span data-ttu-id="6c5d4-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6c5d4-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6c5d4-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6c5d4-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kiteworks-tutorial/tutorial_general_203.png

