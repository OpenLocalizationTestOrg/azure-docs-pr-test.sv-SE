---
title: "Självstudier: Azure Active Directory-integrering med bild Relay | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och avbildningen Relay."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 0bfbbaee7a74df6508584b7c8846fd07c2dc15c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a><span data-ttu-id="deabc-103">Självstudier: Azure Active Directory-integrering med bild Relay</span><span class="sxs-lookup"><span data-stu-id="deabc-103">Tutorial: Azure Active Directory integration with Image Relay</span></span>

<span data-ttu-id="deabc-104">I kursen får lära du att integrera avbildningen Relay med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="deabc-104">In this tutorial, you learn how to integrate Image Relay with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="deabc-105">Integrera avbildningen Relay med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="deabc-105">Integrating Image Relay with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="deabc-106">Du kan styra i Azure AD som har åtkomst till avbildningen Relay</span><span class="sxs-lookup"><span data-stu-id="deabc-106">You can control in Azure AD who has access to Image Relay</span></span>
- <span data-ttu-id="deabc-107">Du kan aktivera användarna att automatiskt hämta loggat in på bilden Relay (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="deabc-107">You can enable your users to automatically get signed-on to Image Relay (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="deabc-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="deabc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="deabc-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="deabc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="deabc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="deabc-110">Prerequisites</span></span>

<span data-ttu-id="deabc-111">Om du vill konfigurera Azure AD-integrering med bild Relay behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="deabc-111">To configure Azure AD integration with Image Relay, you need the following items:</span></span>

- <span data-ttu-id="deabc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="deabc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="deabc-113">En avbildning Relay enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="deabc-113">An Image Relay single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="deabc-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="deabc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="deabc-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="deabc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="deabc-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="deabc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="deabc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="deabc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="deabc-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="deabc-118">Scenario description</span></span>
<span data-ttu-id="deabc-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="deabc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="deabc-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="deabc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="deabc-121">Att lägga till bilden Relay från galleriet</span><span class="sxs-lookup"><span data-stu-id="deabc-121">Adding Image Relay from the gallery</span></span>
2. <span data-ttu-id="deabc-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="deabc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-image-relay-from-the-gallery"></a><span data-ttu-id="deabc-123">Att lägga till bilden Relay från galleriet</span><span class="sxs-lookup"><span data-stu-id="deabc-123">Adding Image Relay from the gallery</span></span>
<span data-ttu-id="deabc-124">Du måste lägga till bilden Relay från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av avbildningen Relay i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="deabc-124">To configure the integration of Image Relay into Azure AD, you need to add Image Relay from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="deabc-125">**Utför följande steg för att lägga till bilden Relay från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="deabc-125">**To add Image Relay from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="deabc-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="deabc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="deabc-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="deabc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="deabc-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="deabc-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="deabc-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="deabc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="deabc-133">I sökrutan skriver **avbildningen Relay**.</span><span class="sxs-lookup"><span data-stu-id="deabc-133">In the search box, type **Image Relay**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_search.png)

5. <span data-ttu-id="deabc-135">Välj i resultatpanelen **avbildningen Relay**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="deabc-135">In the results panel, select **Image Relay**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="deabc-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="deabc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="deabc-138">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med bild Relay baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="deabc-138">In this section, you configure and test Azure AD single sign-on with Image Relay based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="deabc-139">Azure AD måste du känna till motsvarande användaren i avbildningen Relay till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="deabc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Image Relay is to a user in Azure AD.</span></span> <span data-ttu-id="deabc-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i avbildningen Relay upprättas.</span><span class="sxs-lookup"><span data-stu-id="deabc-140">In other words, a link relationship between an Azure AD user and the related user in Image Relay needs to be established.</span></span>

<span data-ttu-id="deabc-141">I bild Relay tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="deabc-141">In Image Relay, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="deabc-142">Om du vill konfigurera och testa Azure AD enkel inloggning med bild Relay måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="deabc-142">To configure and test Azure AD single sign-on with Image Relay, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="deabc-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="deabc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="deabc-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="deabc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="deabc-145">**[Skapa en avbildning Relay testanvändare](#creating-an-image-relay-test-user)**  – du har en motsvarighet för Britta Simon i avbildningen Relay som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="deabc-145">**[Creating an Image Relay test user](#creating-an-image-relay-test-user)** - to have a counterpart of Britta Simon in Image Relay that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="deabc-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="deabc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="deabc-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="deabc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="deabc-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="deabc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="deabc-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i avbildningen Relay-program.</span><span class="sxs-lookup"><span data-stu-id="deabc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Image Relay application.</span></span>

<span data-ttu-id="deabc-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med bild Relay:**</span><span class="sxs-lookup"><span data-stu-id="deabc-150">**To configure Azure AD single sign-on with Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="deabc-151">I Azure-portalen på den **avbildningen Relay** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="deabc-151">In the Azure portal, on the **Image Relay** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="deabc-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="deabc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

3. <span data-ttu-id="deabc-155">På den **avbildningen Relay domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="deabc-155">On the **Image Relay Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_url.png)

    <span data-ttu-id="deabc-157">a.</span><span class="sxs-lookup"><span data-stu-id="deabc-157">a.</span></span> <span data-ttu-id="deabc-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<companyname>.imagerelay.com/`</span><span class="sxs-lookup"><span data-stu-id="deabc-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.imagerelay.com/`</span></span>

    <span data-ttu-id="deabc-159">b.</span><span class="sxs-lookup"><span data-stu-id="deabc-159">b.</span></span> <span data-ttu-id="deabc-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<companyname>.imagerelay.com/sso/metadata`</span><span class="sxs-lookup"><span data-stu-id="deabc-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.imagerelay.com/sso/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="deabc-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="deabc-161">These values are not real.</span></span> <span data-ttu-id="deabc-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="deabc-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="deabc-163">Kontakta [avbildningen Relay klienten supportteamet](http://support.imagerelay.com/) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="deabc-163">Contact [Image Relay Client support team](http://support.imagerelay.com/) to get these values.</span></span> 
 


4. <span data-ttu-id="deabc-164">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="deabc-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

5. <span data-ttu-id="deabc-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="deabc-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="deabc-168">På den **avbildningen Relay konfiguration** klickar du på **konfigurera avbildningen Relay** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="deabc-168">On the **Image Relay Configuration** section, click **Configure Image Relay** to open **Configure sign-on** window.</span></span> <span data-ttu-id="deabc-169">Kopiera den **Sign-Out URL: en och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="deabc-169">Copy the **Sign-Out Service URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_configure.png) 

7. <span data-ttu-id="deabc-171">I ett nytt webbläsarfönster loggar du in på din bild Relay företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="deabc-171">In another browser window, sign in to your Image Relay company site as an administrator.</span></span>

8. <span data-ttu-id="deabc-172">Klicka på i verktygsfältet högst upp i **användare och behörigheter** arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="deabc-172">In the toolbar on the top, click the **Users & Permissions** workload.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

9. <span data-ttu-id="deabc-174">Klicka på **nya behörighet att skapa**.</span><span class="sxs-lookup"><span data-stu-id="deabc-174">Click **Create New Permission**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

10. <span data-ttu-id="deabc-176">I den **enkel inloggning på inställningar** arbetsbelastning, Välj den **den här gruppen kan bara logga in via enkel inloggning** kryssrutan och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="deabc-176">In the **Single Sign On Settings** workload, select the **This Group can only sign-in via Single Sign On** check box, and then click **Save**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

11. <span data-ttu-id="deabc-178">Gå till **kontoinställningar**.</span><span class="sxs-lookup"><span data-stu-id="deabc-178">Go to **Account Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

12. <span data-ttu-id="deabc-180">Gå till den **enkel inloggning på inställningar** arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="deabc-180">Go to the **Single Sign On Settings** workload.</span></span>
    
     ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

13. <span data-ttu-id="deabc-182">På den **SAML inställningar** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="deabc-182">On the **SAML Settings** dialog, perform the following steps:</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    <span data-ttu-id="deabc-184">a.</span><span class="sxs-lookup"><span data-stu-id="deabc-184">a.</span></span> <span data-ttu-id="deabc-185">I **Inloggningswebbadressen** textruta klistra in värdet för **inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="deabc-185">In **Login URL** textbox, paste the value of **Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="deabc-186">b.</span><span class="sxs-lookup"><span data-stu-id="deabc-186">b.</span></span> <span data-ttu-id="deabc-187">I **logga ut URL** textruta klistra in värdet för **tjänst-URL för enkel Sign-Out** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="deabc-187">In **Logout URL**  textbox, paste the value of **Single Sign-Out Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="deabc-188">c.</span><span class="sxs-lookup"><span data-stu-id="deabc-188">c.</span></span> <span data-ttu-id="deabc-189">Som **Format för namn-Id**väljer **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="deabc-189">As **Name Id Format**, select **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="deabc-190">d.</span><span class="sxs-lookup"><span data-stu-id="deabc-190">d.</span></span> <span data-ttu-id="deabc-191">Som **bindning alternativ för begäranden från Service Provider (bild relä)**väljer **POST bindning**.</span><span class="sxs-lookup"><span data-stu-id="deabc-191">As **Binding Options for Requests from the Service Provider (Image Relay)**, select **POST Binding**.</span></span>

    <span data-ttu-id="deabc-192">e.</span><span class="sxs-lookup"><span data-stu-id="deabc-192">e.</span></span> <span data-ttu-id="deabc-193">Under **x.509-certifikat**, klickar du på **uppdatering certifikat**.</span><span class="sxs-lookup"><span data-stu-id="deabc-193">Under **x.509 Certificate**, click **Update Certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    <span data-ttu-id="deabc-195">f.</span><span class="sxs-lookup"><span data-stu-id="deabc-195">f.</span></span> <span data-ttu-id="deabc-196">Öppna hämtade certifikatet i anteckningar, kopiera innehållet och klistra in den i textrutan x.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="deabc-196">Open the downloaded certificate in notepad, copy the content, and then paste it into the x.509 Certificate textbox.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    <span data-ttu-id="deabc-198">g.</span><span class="sxs-lookup"><span data-stu-id="deabc-198">g.</span></span> <span data-ttu-id="deabc-199">I **in Användaretablering** väljer den **Aktivera in Användaretablering**.</span><span class="sxs-lookup"><span data-stu-id="deabc-199">In **Just-In-Time User Provisioning** section, select the **Enable Just-In-Time User Provisioning**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    <span data-ttu-id="deabc-201">h.</span><span class="sxs-lookup"><span data-stu-id="deabc-201">h.</span></span> <span data-ttu-id="deabc-202">Välj behörighetsgruppen (till exempel **SSO grundläggande**) som tillåts att logga in endast via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="deabc-202">Select the permission group (for example, **SSO Basic**) which is allowed to sign in only through single sign-on.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    <span data-ttu-id="deabc-204">Jag.</span><span class="sxs-lookup"><span data-stu-id="deabc-204">i.</span></span> <span data-ttu-id="deabc-205">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="deabc-205">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="deabc-206">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="deabc-206">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="deabc-207">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="deabc-207">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="deabc-208">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="deabc-208">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="deabc-209">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="deabc-209">Creating an Azure AD test user</span></span>
<span data-ttu-id="deabc-210">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="deabc-210">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="deabc-212">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="deabc-212">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="deabc-213">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="deabc-213">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="deabc-215">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="deabc-215">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="deabc-217">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="deabc-217">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="deabc-219">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="deabc-219">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="deabc-221">a.</span><span class="sxs-lookup"><span data-stu-id="deabc-221">a.</span></span> <span data-ttu-id="deabc-222">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="deabc-222">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="deabc-223">b.</span><span class="sxs-lookup"><span data-stu-id="deabc-223">b.</span></span> <span data-ttu-id="deabc-224">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="deabc-224">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="deabc-225">c.</span><span class="sxs-lookup"><span data-stu-id="deabc-225">c.</span></span> <span data-ttu-id="deabc-226">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="deabc-226">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="deabc-227">d.</span><span class="sxs-lookup"><span data-stu-id="deabc-227">d.</span></span> <span data-ttu-id="deabc-228">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="deabc-228">Click **Create**.</span></span>
 
### <a name="creating-an-image-relay-test-user"></a><span data-ttu-id="deabc-229">Skapa en avbildning Relay testanvändare</span><span class="sxs-lookup"><span data-stu-id="deabc-229">Creating an Image Relay test user</span></span>

<span data-ttu-id="deabc-230">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i avbildningen Relay.</span><span class="sxs-lookup"><span data-stu-id="deabc-230">The objective of this section is to create a user called Britta Simon in Image Relay.</span></span>

<span data-ttu-id="deabc-231">**Utför följande steg för att skapa en användare som kallas Britta Simon i avbildningen Relay:**</span><span class="sxs-lookup"><span data-stu-id="deabc-231">**To create a user called Britta Simon in Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="deabc-232">Inloggning på webbplatsen avbildningen Relay företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="deabc-232">Sign-on to your Image Relay company site as an administrator.</span></span>

2. <span data-ttu-id="deabc-233">Gå till **användare och behörigheter** och välj **skapa SSO användare**.</span><span class="sxs-lookup"><span data-stu-id="deabc-233">Go to **Users & Permissions**     and select **Create SSO User**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

3. <span data-ttu-id="deabc-235">Ange den **e-post**, **Förnamn**, **efternamn**, och **företagets** för användaren du vill etablera och välj behörighetsgruppen (till exempel SSO grundläggande) vilket är den grupp som kan logga in endast via enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="deabc-235">Enter the **Email**, **First Name**, **Last Name**, and **Company** of the user you want to provision and select the permission group (for example, SSO Basic) which is the group that can sign in only through single sign-on.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

4. <span data-ttu-id="deabc-237">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="deabc-237">Click **Create**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="deabc-238">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="deabc-238">Assigning the Azure AD test user</span></span>

<span data-ttu-id="deabc-239">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till avbildningen Relay.</span><span class="sxs-lookup"><span data-stu-id="deabc-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Image Relay.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="deabc-241">**Om du vill tilldela avbildningen Relay Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="deabc-241">**To assign Britta Simon to Image Relay, perform the following steps:**</span></span>

1. <span data-ttu-id="deabc-242">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="deabc-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="deabc-244">Välj i listan med program **avbildningen Relay**.</span><span class="sxs-lookup"><span data-stu-id="deabc-244">In the applications list, select **Image Relay**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_app.png) 

3. <span data-ttu-id="deabc-246">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="deabc-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="deabc-248">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="deabc-248">Click **Add** button.</span></span> <span data-ttu-id="deabc-249">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="deabc-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="deabc-251">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="deabc-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="deabc-252">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="deabc-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="deabc-253">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="deabc-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="deabc-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="deabc-254">Testing single sign-on</span></span>

<span data-ttu-id="deabc-255">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="deabc-255">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>    

<span data-ttu-id="deabc-256">När du klickar på bilden Relay-panelen på åtkomstpanelen du bör få automatiskt loggat in på bilden Relay-programmet.</span><span class="sxs-lookup"><span data-stu-id="deabc-256">When you click the Image Relay tile in the Access Panel, you should get automatically signed-on to your Image Relay application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="deabc-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="deabc-257">Additional resources</span></span>

* [<span data-ttu-id="deabc-258">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="deabc-258">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="deabc-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="deabc-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png

