---
title: "Självstudier: Azure Active Directory-integrering med mänskligheten | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och mänskligheten."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6aa771e9-31c6-48d1-8dde-024bebc06943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.openlocfilehash: 327cc1e3d0836e79376e0a3cd5a4422a967f5943
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-humanity"></a><span data-ttu-id="df051-103">Självstudier: Azure Active Directory-integrering med mänskligheten</span><span class="sxs-lookup"><span data-stu-id="df051-103">Tutorial: Azure Active Directory integration with Humanity</span></span>

<span data-ttu-id="df051-104">I kursen får lära du att integrera mänskligheten med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="df051-104">In this tutorial, you learn how to integrate Humanity with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="df051-105">Integrera mänskligheten med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="df051-105">Integrating Humanity with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="df051-106">Du kan styra i Azure AD som har åtkomst till mänskligheten</span><span class="sxs-lookup"><span data-stu-id="df051-106">You can control in Azure AD who has access to Humanity</span></span>
- <span data-ttu-id="df051-107">Du kan aktivera användarna att automatiskt hämta loggat in på mänskligheten (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="df051-107">You can enable your users to automatically get signed-on to Humanity (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="df051-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="df051-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="df051-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="df051-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df051-110">Krav</span><span class="sxs-lookup"><span data-stu-id="df051-110">Prerequisites</span></span>

<span data-ttu-id="df051-111">För att konfigurera Azure AD-integrering med mänskligheten, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="df051-111">To configure Azure AD integration with Humanity, you need the following items:</span></span>

- <span data-ttu-id="df051-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="df051-112">An Azure AD subscription</span></span>
- <span data-ttu-id="df051-113">En mänskligheten enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="df051-113">A Humanity single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="df051-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="df051-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="df051-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="df051-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="df051-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="df051-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="df051-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="df051-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="df051-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="df051-118">Scenario description</span></span>
<span data-ttu-id="df051-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="df051-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="df051-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="df051-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="df051-121">Att lägga till mänskligheten från galleriet</span><span class="sxs-lookup"><span data-stu-id="df051-121">Adding Humanity from the gallery</span></span>
2. <span data-ttu-id="df051-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df051-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-humanity-from-the-gallery"></a><span data-ttu-id="df051-123">Att lägga till mänskligheten från galleriet</span><span class="sxs-lookup"><span data-stu-id="df051-123">Adding Humanity from the gallery</span></span>
<span data-ttu-id="df051-124">Du måste lägga till mänskligheten från galleriet i listan över hanterade SaaS-appar för att konfigurera mänskligheten till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="df051-124">To configure the integration of Humanity into Azure AD, you need to add Humanity from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="df051-125">**Utför följande steg för att lägga till mänskligheten från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="df051-125">**To add Humanity from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="df051-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="df051-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="df051-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="df051-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="df051-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="df051-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="df051-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df051-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="df051-133">I sökrutan skriver **mänskligheten**.</span><span class="sxs-lookup"><span data-stu-id="df051-133">In the search box, type **Humanity**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_search.png)

5. <span data-ttu-id="df051-135">Välj i resultatpanelen **mänskligheten**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="df051-135">In the results panel, select **Humanity**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="df051-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df051-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="df051-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med mänskligheten baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="df051-138">In this section, you configure and test Azure AD single sign-on with Humanity based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="df051-139">Azure AD måste du känna till användaren i mänskligheten motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="df051-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Humanity is to a user in Azure AD.</span></span> <span data-ttu-id="df051-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i mänskligheten upprättas.</span><span class="sxs-lookup"><span data-stu-id="df051-140">In other words, a link relationship between an Azure AD user and the related user in Humanity needs to be established.</span></span>

<span data-ttu-id="df051-141">I mänskligheten, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="df051-141">In Humanity, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="df051-142">Om du vill konfigurera och testa Azure AD enkel inloggning med mänskligheten, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="df051-142">To configure and test Azure AD single sign-on with Humanity, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="df051-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="df051-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="df051-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df051-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="df051-145">**[Skapa en testanvändare mänskligheten](#creating-a-humanity-test-user)**  – har en motsvarighet för Britta Simon mänskligheten som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="df051-145">**[Creating a Humanity test user](#creating-a-humanity-test-user)** - to have a counterpart of Britta Simon in Humanity that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="df051-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="df051-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="df051-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="df051-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="df051-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df051-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="df051-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet mänskligheten.</span><span class="sxs-lookup"><span data-stu-id="df051-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Humanity application.</span></span>

<span data-ttu-id="df051-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med mänskligheten:**</span><span class="sxs-lookup"><span data-stu-id="df051-150">**To configure Azure AD single sign-on with Humanity, perform the following steps:**</span></span>

1. <span data-ttu-id="df051-151">I Azure-portalen på den **mänskligheten** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="df051-151">In the Azure portal, on the **Humanity** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="df051-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="df051-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_samlbase.png)

3. <span data-ttu-id="df051-155">På den **mänskligheten domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df051-155">On the **Humanity Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_url.png)

    <span data-ttu-id="df051-157">a.</span><span class="sxs-lookup"><span data-stu-id="df051-157">a.</span></span> <span data-ttu-id="df051-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://company.humanity.com/includes/saml/`</span><span class="sxs-lookup"><span data-stu-id="df051-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://company.humanity.com/includes/saml/`</span></span>

    <span data-ttu-id="df051-159">b.</span><span class="sxs-lookup"><span data-stu-id="df051-159">b.</span></span> <span data-ttu-id="df051-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://company.humanity.com/app/`</span><span class="sxs-lookup"><span data-stu-id="df051-160">In the **Identifier** textbox, type a URL using the following pattern: `https://company.humanity.com/app/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="df051-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="df051-161">These values are not real.</span></span> <span data-ttu-id="df051-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="df051-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="df051-163">Kontakta [mänskligheten klienten supportteamet](https://www.humanity.com/support/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="df051-163">Contact [Humanity Client support team](https://www.humanity.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="df051-164">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="df051-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_certificate.png) 

5. <span data-ttu-id="df051-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="df051-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="df051-168">På den **mänskligheten Configuration** klickar du på **konfigurera mänskligheten** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="df051-168">On the **Humanity Configuration** section, click **Configure Humanity** to open **Configure sign-on** window.</span></span> <span data-ttu-id="df051-169">Kopiera den **SAML inloggning tjänst-URL för enkel och Sign-Out URL** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="df051-169">Copy the **SAML Single Sign-On Service URL, and Sign-Out URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_configure.png) 

7. <span data-ttu-id="df051-171">I en annan webbläsarfönstret, logga in på ditt **mänskligheten** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="df051-171">In a different web browser window, log in to your **Humanity** company site as an administrator.</span></span>

8. <span data-ttu-id="df051-172">Klicka på menyn högst upp **Admin**.</span><span class="sxs-lookup"><span data-stu-id="df051-172">In the menu on the top, click **Admin**.</span></span>
   
    <span data-ttu-id="df051-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="df051-173">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

9. <span data-ttu-id="df051-174">Under **integrering**, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="df051-174">Under **Integration**, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="df051-175">![Enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="df051-175">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786620.png "Single Sign-On")</span></span>

10. <span data-ttu-id="df051-176">I den **enkel inloggning** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df051-176">In the **Single Sign-On** section, perform the following steps:</span></span>
   
    <span data-ttu-id="df051-177">![Enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="df051-177">![Single Sign-On](./media/active-directory-saas-shiftplanning-tutorial/iC786905.png "Single Sign-On")</span></span>
   
    <span data-ttu-id="df051-178">a.</span><span class="sxs-lookup"><span data-stu-id="df051-178">a.</span></span> <span data-ttu-id="df051-179">Välj **SAML aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="df051-179">Select **SAML Enabled**.</span></span>

    <span data-ttu-id="df051-180">b.</span><span class="sxs-lookup"><span data-stu-id="df051-180">b.</span></span> <span data-ttu-id="df051-181">Välj **Tillåt lösenord inloggningen**.</span><span class="sxs-lookup"><span data-stu-id="df051-181">Select **Allow Password Login**.</span></span>

    <span data-ttu-id="df051-182">c.</span><span class="sxs-lookup"><span data-stu-id="df051-182">c.</span></span> <span data-ttu-id="df051-183">Klistra in den **SAML enkel inloggning Tjänstwebbadress** värde i den **utfärdar-URL för SAML** textruta.</span><span class="sxs-lookup"><span data-stu-id="df051-183">Paste the **SAML Single Sign-On Service URL** value into the **SAML Issuer URL** textbox.</span></span>

    <span data-ttu-id="df051-184">d.</span><span class="sxs-lookup"><span data-stu-id="df051-184">d.</span></span> <span data-ttu-id="df051-185">Klistra in den **Sign-Out URL** värde i den **Remote logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="df051-185">Paste the **Sign-Out URL** value into the **Remote Logout URL** textbox.</span></span>
   
    <span data-ttu-id="df051-186">e.</span><span class="sxs-lookup"><span data-stu-id="df051-186">e.</span></span> <span data-ttu-id="df051-187">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="df051-187">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>

11. <span data-ttu-id="df051-188">Klicka på **Spara inställningar**.</span><span class="sxs-lookup"><span data-stu-id="df051-188">Click **Save Settings**.</span></span>

> [!TIP]
> <span data-ttu-id="df051-189">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="df051-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="df051-190">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="df051-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="df051-191">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="df051-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="df051-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="df051-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="df051-193">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df051-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="df051-195">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="df051-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="df051-196">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="df051-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="df051-198">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="df051-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="df051-200">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df051-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="df051-202">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df051-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-shiftplanning-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="df051-204">a.</span><span class="sxs-lookup"><span data-stu-id="df051-204">a.</span></span> <span data-ttu-id="df051-205">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="df051-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="df051-206">b.</span><span class="sxs-lookup"><span data-stu-id="df051-206">b.</span></span> <span data-ttu-id="df051-207">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="df051-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="df051-208">c.</span><span class="sxs-lookup"><span data-stu-id="df051-208">c.</span></span> <span data-ttu-id="df051-209">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="df051-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="df051-210">d.</span><span class="sxs-lookup"><span data-stu-id="df051-210">d.</span></span> <span data-ttu-id="df051-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="df051-211">Click **Create**.</span></span>
 
### <a name="creating-a-humanity-test-user"></a><span data-ttu-id="df051-212">Skapa en testanvändare mänskligheten</span><span class="sxs-lookup"><span data-stu-id="df051-212">Creating a Humanity test user</span></span>

<span data-ttu-id="df051-213">För att aktivera Azure AD-användare kan logga in på mänskligheten etableras de i mänskligheten.</span><span class="sxs-lookup"><span data-stu-id="df051-213">In order to enable Azure AD users to log in to Humanity, they must be provisioned into Humanity.</span></span> <span data-ttu-id="df051-214">När det gäller mänskligheten är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="df051-214">In the case of Humanity, provisioning is a manual task.</span></span>

<span data-ttu-id="df051-215">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="df051-215">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="df051-216">Logga in på ditt **mänskligheten** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="df051-216">Log in to your **Humanity** company site as an administrator.</span></span>

2. <span data-ttu-id="df051-217">Klicka på **Admin**.</span><span class="sxs-lookup"><span data-stu-id="df051-217">Click **Admin**.</span></span>
   
    <span data-ttu-id="df051-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="df051-218">![Admin](./media/active-directory-saas-shiftplanning-tutorial/iC786619.png "Admin")</span></span>

3. <span data-ttu-id="df051-219">Klicka på **personal**.</span><span class="sxs-lookup"><span data-stu-id="df051-219">Click **Staff**.</span></span>
   
    <span data-ttu-id="df051-220">![Personal](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "personal")</span><span class="sxs-lookup"><span data-stu-id="df051-220">![Staff](./media/active-directory-saas-shiftplanning-tutorial/ic786623.png "Staff")</span></span>

4. <span data-ttu-id="df051-221">Under **åtgärder**, klickar du på **lägga till anställda**.</span><span class="sxs-lookup"><span data-stu-id="df051-221">Under **Actions**, click **Add Employees**.</span></span>
   
    <span data-ttu-id="df051-222">![Lägg till medarbetare](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "lägga till anställda")</span><span class="sxs-lookup"><span data-stu-id="df051-222">![Add Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786624.png "Add Employees")</span></span>

5. <span data-ttu-id="df051-223">I den **lägga till anställda** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df051-223">In the **Add Employees** section, perform the following steps:</span></span>
   
    <span data-ttu-id="df051-224">![Spara anställda](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "spara anställda")</span><span class="sxs-lookup"><span data-stu-id="df051-224">![Save Employees](./media/active-directory-saas-shiftplanning-tutorial/iC786625.png "Save Employees")</span></span>
   
    <span data-ttu-id="df051-225">a.</span><span class="sxs-lookup"><span data-stu-id="df051-225">a.</span></span> <span data-ttu-id="df051-226">Typ av **Förnamn**, **efternamn**, och **e-post** av en giltig AAD-konto som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="df051-226">Type the **First Name**, **Last Name**, and **Email** of a valid AAD account you want to provision into the related textboxes.</span></span>

    <span data-ttu-id="df051-227">b.</span><span class="sxs-lookup"><span data-stu-id="df051-227">b.</span></span> <span data-ttu-id="df051-228">Klicka på **spara anställda**.</span><span class="sxs-lookup"><span data-stu-id="df051-228">Click **Save Employees**.</span></span>

>[!NOTE]
><span data-ttu-id="df051-229">Du kan använda något annat mänskligheten användarens konto skapas verktyg eller API: er som tillhandahålls av mänskligheten etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="df051-229">You can use any other Humanity user account creation tools or APIs provided by Humanity to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="df051-230">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="df051-230">Assigning the Azure AD test user</span></span>

<span data-ttu-id="df051-231">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till mänskligheten.</span><span class="sxs-lookup"><span data-stu-id="df051-231">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Humanity.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="df051-233">**Om du vill tilldela mänskligheten Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="df051-233">**To assign Britta Simon to Humanity, perform the following steps:**</span></span>

1. <span data-ttu-id="df051-234">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="df051-234">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="df051-236">Välj i listan med program **mänskligheten**.</span><span class="sxs-lookup"><span data-stu-id="df051-236">In the applications list, select **Humanity**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-shiftplanning-tutorial/tutorial_humanity_app.png) 

3. <span data-ttu-id="df051-238">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="df051-238">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="df051-240">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="df051-240">Click **Add** button.</span></span> <span data-ttu-id="df051-241">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df051-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="df051-243">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="df051-243">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="df051-244">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df051-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="df051-245">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df051-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="df051-246">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df051-246">Testing single sign-on</span></span>

<span data-ttu-id="df051-247">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="df051-247">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="df051-248">När du klickar på panelen mänskligheten på åtkomstpanelen du bör få automatiskt loggat in på ditt mänskligheten program.</span><span class="sxs-lookup"><span data-stu-id="df051-248">When you click the Humanity tile in the Access Panel, you should get automatically signed-on to your Humanity application.</span></span>
<span data-ttu-id="df051-249">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="df051-249">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df051-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="df051-250">Additional resources</span></span>

* [<span data-ttu-id="df051-251">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="df051-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="df051-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="df051-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-shiftplanning-tutorial/tutorial_general_203.png

