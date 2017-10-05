---
title: "Självstudier: Azure Active Directory-integrering med zoomning | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och zoomning."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: aab491f162fd4d24c6ff4d8858f2edd96dda30d4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a><span data-ttu-id="7e995-103">Självstudier: Azure Active Directory-integrering med zoomning</span><span class="sxs-lookup"><span data-stu-id="7e995-103">Tutorial: Azure Active Directory integration with Zoom</span></span>

<span data-ttu-id="7e995-104">I kursen får lära du att integrera zoomning med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7e995-104">In this tutorial, you learn how to integrate Zoom with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7e995-105">Integrera zoomning med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7e995-105">Integrating Zoom with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7e995-106">Du kan styra i Azure AD som har åtkomst till zoomning.</span><span class="sxs-lookup"><span data-stu-id="7e995-106">You can control in Azure AD who has access to Zoom.</span></span>
- <span data-ttu-id="7e995-107">Du kan aktivera användarna att automatiskt hämta loggat in på Zooma (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="7e995-107">You can enable your users to automatically get signed-on to Zoom (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="7e995-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e995-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="7e995-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7e995-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e995-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7e995-110">Prerequisites</span></span>

<span data-ttu-id="7e995-111">För att konfigurera Azure AD-integrering med zoomning, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7e995-111">To configure Azure AD integration with Zoom, you need the following items:</span></span>

- <span data-ttu-id="7e995-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7e995-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7e995-113">En zoomning enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7e995-113">A Zoom single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7e995-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7e995-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7e995-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7e995-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7e995-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7e995-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7e995-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7e995-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7e995-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7e995-118">Scenario description</span></span>
<span data-ttu-id="7e995-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7e995-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7e995-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7e995-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7e995-121">Att lägga till zoomning från galleriet</span><span class="sxs-lookup"><span data-stu-id="7e995-121">Adding Zoom from the gallery</span></span>
2. <span data-ttu-id="7e995-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e995-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoom-from-the-gallery"></a><span data-ttu-id="7e995-123">Att lägga till zoomning från galleriet</span><span class="sxs-lookup"><span data-stu-id="7e995-123">Adding Zoom from the gallery</span></span>
<span data-ttu-id="7e995-124">Du måste lägga till zoomning från galleriet i listan över hanterade SaaS-appar för att konfigurera Zooma till Azure AD-integrering.</span><span class="sxs-lookup"><span data-stu-id="7e995-124">To configure the integration of Zoom into Azure AD, you need to add Zoom from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7e995-125">**Utför följande steg för att lägga till zoomning från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7e995-125">**To add Zoom from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7e995-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7e995-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="7e995-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7e995-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7e995-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7e995-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="7e995-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e995-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="7e995-133">I sökrutan skriver **Zooma**väljer **Zooma** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7e995-133">In the search box, type **Zoom**, select **Zoom** from result panel then click **Add** button to add the application.</span></span>

    ![Zooma in resultatlistan](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="7e995-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e995-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="7e995-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med zoomning baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7e995-136">In this section, you configure and test Azure AD single sign-on with Zoom based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7e995-137">Azure AD måste du känna till användaren i zoomning motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7e995-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Zoom is to a user in Azure AD.</span></span> <span data-ttu-id="7e995-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Zoom upprättas.</span><span class="sxs-lookup"><span data-stu-id="7e995-138">In other words, a link relationship between an Azure AD user and the related user in Zoom needs to be established.</span></span>

<span data-ttu-id="7e995-139">I zoomning, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7e995-139">In Zoom, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7e995-140">Om du vill konfigurera och testa Azure AD enkel inloggning med zoomning, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7e995-140">To configure and test Azure AD single sign-on with Zoom, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7e995-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7e995-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7e995-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7e995-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7e995-143">**[Skapa en testanvändare zoomning](#create-a-zoom-test-user)**  – har en motsvarighet för Britta Simon zoomning som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7e995-143">**[Create a Zoom test user](#create-a-zoom-test-user)** - to have a counterpart of Britta Simon in Zoom that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7e995-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7e995-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7e995-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7e995-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="7e995-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e995-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="7e995-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program för zoomning.</span><span class="sxs-lookup"><span data-stu-id="7e995-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Zoom application.</span></span>

<span data-ttu-id="7e995-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med zoomning:**</span><span class="sxs-lookup"><span data-stu-id="7e995-148">**To configure Azure AD single sign-on with Zoom, perform the following steps:**</span></span>

1. <span data-ttu-id="7e995-149">I Azure-portalen på den **Zooma** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7e995-149">In the Azure portal, on the **Zoom** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="7e995-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7e995-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_samlbase.png)

3. <span data-ttu-id="7e995-153">På den **URL: er och zooma domän** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7e995-153">On the **Zoom Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och zoomning domän med enkel inloggning information](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_url.png)

    <span data-ttu-id="7e995-155">a.</span><span class="sxs-lookup"><span data-stu-id="7e995-155">a.</span></span> <span data-ttu-id="7e995-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="7e995-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.zoom.us`</span></span>

    <span data-ttu-id="7e995-157">b.</span><span class="sxs-lookup"><span data-stu-id="7e995-157">b.</span></span> <span data-ttu-id="7e995-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="7e995-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.zoom.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7e995-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7e995-159">These values are not real.</span></span> <span data-ttu-id="7e995-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7e995-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7e995-161">Kontakta [Zooma klienten supportteamet](https://support.zoom.us/hc) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7e995-161">Contact [Zoom Client support team](https://support.zoom.us/hc) to get these values.</span></span> 
 
4. <span data-ttu-id="7e995-162">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7e995-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_certificate.png) 

5. <span data-ttu-id="7e995-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7e995-164">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-zoom-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7e995-166">På den **Zooma Configuration** klickar du på **konfigurera Zooma** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7e995-166">On the **Zoom Configuration** section, click **Configure Zoom** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7e995-167">Kopiera den **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7e995-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Zooma konfiguration](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_configure.png) 

7. <span data-ttu-id="7e995-169">I en annan webbläsarfönster loggar du in på webbplatsen zoomning företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="7e995-169">In a different web browser window, log in to your Zoom company site as an administrator.</span></span>

8. <span data-ttu-id="7e995-170">Klicka på den **enkel inloggning** fliken.</span><span class="sxs-lookup"><span data-stu-id="7e995-170">Click the **Single Sign-On** tab.</span></span>
   
    <span data-ttu-id="7e995-171">![Enkel inloggning fliken](./media/active-directory-saas-zoom-tutorial/IC784700.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7e995-171">![Single sign-on tab](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single sign-on")</span></span>

9. <span data-ttu-id="7e995-172">Klicka på den **säkerhetskontroll** fliken och gå sedan till den **enkel inloggning** inställningar.</span><span class="sxs-lookup"><span data-stu-id="7e995-172">Click the **Security Control** tab, and then go to the **Single Sign-On** settings.</span></span>

10. <span data-ttu-id="7e995-173">Utför följande steg i avsnittet enkel inloggning:</span><span class="sxs-lookup"><span data-stu-id="7e995-173">In the Single Sign-On section, perform the following steps:</span></span>
   
    <span data-ttu-id="7e995-174">![Enkel inloggning avsnittet](./media/active-directory-saas-zoom-tutorial/IC784701.png "enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="7e995-174">![Single sign-on section](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single sign-on")</span></span>
   
    <span data-ttu-id="7e995-175">a.</span><span class="sxs-lookup"><span data-stu-id="7e995-175">a.</span></span> <span data-ttu-id="7e995-176">I den **inloggning Sidadress** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e995-176">In the **Sign-in page URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="7e995-177">b.</span><span class="sxs-lookup"><span data-stu-id="7e995-177">b.</span></span> <span data-ttu-id="7e995-178">I den **URL för utloggning** textruta klistra in värdet för **Sign-Out URL**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e995-178">In the **Sign-out page URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
     
    <span data-ttu-id="7e995-179">c.</span><span class="sxs-lookup"><span data-stu-id="7e995-179">c.</span></span> <span data-ttu-id="7e995-180">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistra in den till den **providern identitetscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="7e995-180">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **Identity provider certificate** textbox.</span></span>

    <span data-ttu-id="7e995-181">d.</span><span class="sxs-lookup"><span data-stu-id="7e995-181">d.</span></span> <span data-ttu-id="7e995-182">I den **utfärdaren** textruta klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e995-182">In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="7e995-183">e.</span><span class="sxs-lookup"><span data-stu-id="7e995-183">e.</span></span> <span data-ttu-id="7e995-184">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="7e995-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="7e995-185">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7e995-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7e995-186">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7e995-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7e995-187">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7e995-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7e995-188">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7e995-188">Create an Azure AD test user</span></span>

<span data-ttu-id="7e995-189">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7e995-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="7e995-191">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7e995-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7e995-192">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="7e995-192">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-zoom-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="7e995-194">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7e995-194">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-zoom-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="7e995-196">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e995-196">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-zoom-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="7e995-198">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7e995-198">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-zoom-tutorial/create_aaduser_04.png)

    <span data-ttu-id="7e995-200">a.</span><span class="sxs-lookup"><span data-stu-id="7e995-200">a.</span></span> <span data-ttu-id="7e995-201">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7e995-201">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7e995-202">b.</span><span class="sxs-lookup"><span data-stu-id="7e995-202">b.</span></span> <span data-ttu-id="7e995-203">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="7e995-203">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="7e995-204">c.</span><span class="sxs-lookup"><span data-stu-id="7e995-204">c.</span></span> <span data-ttu-id="7e995-205">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="7e995-205">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="7e995-206">d.</span><span class="sxs-lookup"><span data-stu-id="7e995-206">d.</span></span> <span data-ttu-id="7e995-207">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7e995-207">Click **Create**.</span></span>
 
### <a name="create-a-zoom-test-user"></a><span data-ttu-id="7e995-208">Skapa en testanvändare zoomning</span><span class="sxs-lookup"><span data-stu-id="7e995-208">Create a Zoom test user</span></span>

<span data-ttu-id="7e995-209">För att aktivera Azure AD-användare kan logga in på Zooma etableras de i zoomning.</span><span class="sxs-lookup"><span data-stu-id="7e995-209">In order to enable Azure AD users to log in to Zoom, they must be provisioned into Zoom.</span></span> <span data-ttu-id="7e995-210">Zooma är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="7e995-210">In the case of Zoom, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-account-perform-the-following-steps"></a><span data-ttu-id="7e995-211">Utför följande steg om du vill konfigurera ett användarkonto:</span><span class="sxs-lookup"><span data-stu-id="7e995-211">To provision a user account, perform the following steps:</span></span>

1. <span data-ttu-id="7e995-212">Logga in på ditt **Zooma** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="7e995-212">Log in to your **Zoom** company site as an administrator.</span></span>
 
2. <span data-ttu-id="7e995-213">Klicka på den **kontohantering** fliken och klicka sedan på **Användarhantering**.</span><span class="sxs-lookup"><span data-stu-id="7e995-213">Click the **Account Management** tab, and then click **User Management**.</span></span>

3. <span data-ttu-id="7e995-214">Klicka på avsnittet Användarhantering **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="7e995-214">In the User Management section, click **Add users**.</span></span>
   
    <span data-ttu-id="7e995-215">![Användarhantering](./media/active-directory-saas-zoom-tutorial/IC784703.png "användarhantering")</span><span class="sxs-lookup"><span data-stu-id="7e995-215">![User management](./media/active-directory-saas-zoom-tutorial/IC784703.png "User management")</span></span>

4. <span data-ttu-id="7e995-216">På den **lägga till användare** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7e995-216">On the **Add users** page, perform the following steps:</span></span>
   
    <span data-ttu-id="7e995-217">![Lägg till användare](./media/active-directory-saas-zoom-tutorial/IC784704.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="7e995-217">![Add users](./media/active-directory-saas-zoom-tutorial/IC784704.png "Add users")</span></span>
   
    <span data-ttu-id="7e995-218">a.</span><span class="sxs-lookup"><span data-stu-id="7e995-218">a.</span></span> <span data-ttu-id="7e995-219">Som **användartyp**väljer **grundläggande**.</span><span class="sxs-lookup"><span data-stu-id="7e995-219">As **User Type**, select **Basic**.</span></span>

    <span data-ttu-id="7e995-220">b.</span><span class="sxs-lookup"><span data-stu-id="7e995-220">b.</span></span> <span data-ttu-id="7e995-221">I den **e-postmeddelanden** textruta, Skriv ett giltigt Azure AD e-postadress konto du vill etablera.</span><span class="sxs-lookup"><span data-stu-id="7e995-221">In the **Emails** textbox, type the email address of a valid Azure AD account you want to provision.</span></span>

    <span data-ttu-id="7e995-222">c.</span><span class="sxs-lookup"><span data-stu-id="7e995-222">c.</span></span> <span data-ttu-id="7e995-223">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7e995-223">Click **Add**.</span></span>

> [!NOTE]
> <span data-ttu-id="7e995-224">Du kan använda något annat zoomning användarens konto skapas verktyg eller API: er som tillhandahålls av zoomning att etablera Azure Active Directory användarkonton.</span><span class="sxs-lookup"><span data-stu-id="7e995-224">You can use any other Zoom user account creation tools or APIs provided by Zoom to provision Azure Active Directory user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="7e995-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7e995-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="7e995-226">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till zoomning.</span><span class="sxs-lookup"><span data-stu-id="7e995-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Zoom.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="7e995-228">**Om du vill tilldela zoomning Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7e995-228">**To assign Britta Simon to Zoom, perform the following steps:**</span></span>

1. <span data-ttu-id="7e995-229">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7e995-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7e995-231">Välj i listan med program **Zooma**.</span><span class="sxs-lookup"><span data-stu-id="7e995-231">In the applications list, select **Zoom**.</span></span>

    ![Zooma-länken i listan med program](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_app.png)  

3. <span data-ttu-id="7e995-233">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7e995-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="7e995-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7e995-235">Click **Add** button.</span></span> <span data-ttu-id="7e995-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e995-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="7e995-238">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7e995-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7e995-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e995-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7e995-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e995-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="7e995-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7e995-241">Test single sign-on</span></span>

<span data-ttu-id="7e995-242">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7e995-242">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7e995-243">När du klickar på panelen zoomning på åtkomstpanelen du ska hämta automatiskt loggat in på ditt program för zoomning.</span><span class="sxs-lookup"><span data-stu-id="7e995-243">When you click the Zoom tile in the Access Panel, you should get automatically signed-on to your Zoom application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e995-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7e995-244">Additional resources</span></span>

* [<span data-ttu-id="7e995-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7e995-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7e995-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7e995-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_203.png

