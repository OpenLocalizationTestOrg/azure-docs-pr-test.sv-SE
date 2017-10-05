---
title: "Självstudier: Azure Active Directory-integrering med molntjänster Lifesize | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Lifesize moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 7542360f9c75786bf400553090ba0a891d9c2fcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a><span data-ttu-id="8cdbc-103">Självstudier: Azure Active Directory-integrering med Lifesize moln</span><span class="sxs-lookup"><span data-stu-id="8cdbc-103">Tutorial: Azure Active Directory integration with Lifesize Cloud</span></span>

<span data-ttu-id="8cdbc-104">I kursen får lära du att integrera Lifesize moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8cdbc-104">In this tutorial, you learn how to integrate Lifesize Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8cdbc-105">Integrera Lifesize moln med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-105">Integrating Lifesize Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8cdbc-106">Du kan styra i Azure AD som har åtkomst till Lifesize moln</span><span class="sxs-lookup"><span data-stu-id="8cdbc-106">You can control in Azure AD who has access to Lifesize Cloud</span></span>
- <span data-ttu-id="8cdbc-107">Du kan aktivera användarna att automatiskt hämta loggat in på molnet Lifesize (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8cdbc-107">You can enable your users to automatically get signed-on to Lifesize Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8cdbc-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8cdbc-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8cdbc-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8cdbc-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8cdbc-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8cdbc-110">Prerequisites</span></span>

<span data-ttu-id="8cdbc-111">För att konfigurera Azure AD-integrering med Lifesize molnet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-111">To configure Azure AD integration with Lifesize Cloud, you need the following items:</span></span>

- <span data-ttu-id="8cdbc-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8cdbc-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8cdbc-113">En Lifesize enkel inloggning aktiverad molnprenumeration</span><span class="sxs-lookup"><span data-stu-id="8cdbc-113">A Lifesize Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8cdbc-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8cdbc-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8cdbc-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8cdbc-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8cdbc-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8cdbc-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8cdbc-118">Scenario description</span></span>
<span data-ttu-id="8cdbc-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8cdbc-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8cdbc-121">Att lägga till Lifesize moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="8cdbc-121">Adding Lifesize Cloud from the gallery</span></span>
2. <span data-ttu-id="8cdbc-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8cdbc-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lifesize-cloud-from-the-gallery"></a><span data-ttu-id="8cdbc-123">Att lägga till Lifesize moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="8cdbc-123">Adding Lifesize Cloud from the gallery</span></span>
<span data-ttu-id="8cdbc-124">Du måste lägga till Lifesize moln från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Lifesize moln i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-124">To configure the integration of Lifesize Cloud into Azure AD, you need to add Lifesize Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8cdbc-125">**Utför följande steg för att lägga till Lifesize moln från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-125">**To add Lifesize Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8cdbc-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8cdbc-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8cdbc-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8cdbc-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8cdbc-133">I sökrutan skriver **Lifesize moln**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-133">In the search box, type **Lifesize Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. <span data-ttu-id="8cdbc-135">Välj i resultatpanelen **Lifesize moln**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-135">In the results panel, select **Lifesize Cloud**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8cdbc-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8cdbc-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8cdbc-138">Du konfigurera och testa Azure AD enkel inloggning med Lifesize molnet baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-138">In this section, you configure and test Azure AD single sign-on with Lifesize Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8cdbc-139">Azure AD måste du känna till motsvarande användaren i Lifesize molnet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lifesize Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="8cdbc-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Lifesize molnet upprättas.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-140">In other words, a link relationship between an Azure AD user and the related user in Lifesize Cloud needs to be established.</span></span>

<span data-ttu-id="8cdbc-141">I Lifesize moln, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-141">In Lifesize Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8cdbc-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Lifesize moln, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-142">To configure and test Azure AD single sign-on with Lifesize Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8cdbc-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8cdbc-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8cdbc-145">**[Skapa en testanvändare Lifesize moln](#creating-a-lifesize-cloud-test-user)**  – du har en motsvarighet för Britta Simon i Lifesize moln som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-145">**[Creating a Lifesize Cloud test user](#creating-a-lifesize-cloud-test-user)** - to have a counterpart of Britta Simon in Lifesize Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8cdbc-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8cdbc-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8cdbc-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8cdbc-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8cdbc-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Lifesize moln.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lifesize Cloud application.</span></span>

<span data-ttu-id="8cdbc-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Lifesize molnet:**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-150">**To configure Azure AD single sign-on with Lifesize Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="8cdbc-151">I Azure-portalen på den **Lifesize moln** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-151">In the Azure portal, on the **Lifesize Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8cdbc-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. <span data-ttu-id="8cdbc-155">På den **Lifesize moln domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-155">On the **Lifesize Cloud Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    <span data-ttu-id="8cdbc-157">a.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-157">a.</span></span> <span data-ttu-id="8cdbc-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://login.lifesizecloud.com/ls/?acs`</span><span class="sxs-lookup"><span data-stu-id="8cdbc-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/ls/?acs`</span></span>

    <span data-ttu-id="8cdbc-159">b.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-159">b.</span></span> <span data-ttu-id="8cdbc-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://login.lifesizecloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="8cdbc-160">In the **Identifier** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/<companyname>`</span></span>

     
4. <span data-ttu-id="8cdbc-161">Kontrollera **visa avancerade inställningar för URL: en**, utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-161">Check **Show advanced URL settings**, perform the following step:</span></span>    
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    <span data-ttu-id="8cdbc-163">I den **Relay tillstånd** textruta Skriv en URL med följande mönster:`https://webapp.lifesizecloud.com/?ent=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="8cdbc-163">In the **Relay state** textbox, type a URL using the following pattern: `https://webapp.lifesizecloud.com/?ent=<identifier>`</span></span>
   
   > [!NOTE] 
   ><span data-ttu-id="8cdbc-164">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-164">Please note that these are not the real values.</span></span> <span data-ttu-id="8cdbc-165">Du måste uppdatera dessa värden med den faktiska inloggnings-URL, Relay tillstånd och identifierare.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-165">you have to update these values with the actual Sign-On URL, Relay State, and Identifier.</span></span> <span data-ttu-id="8cdbc-166">Kontakta [Lifesize Cloud klienten supportteamet](https://www.lifesize.com/support) få inloggnings-URL, och identifierar-värden och du kan hämta Relay tillstånd värde från SSO-konfigurationen som beskrivs senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-166">Contact [Lifesize Cloud Client support team](https://www.lifesize.com/support) to get Sign-On URL, and Identifier values and you can get Relay State  value from SSO Configuration that is explained later in the tutorial.</span></span>

4. <span data-ttu-id="8cdbc-167">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-167">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. <span data-ttu-id="8cdbc-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8cdbc-171">På den **Lifesize Molnkonfigurationen** klickar du på **konfigurera Lifesize moln** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-171">On the **Lifesize Cloud Configuration** section, click **Configure Lifesize Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8cdbc-172">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-172">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. <span data-ttu-id="8cdbc-174">Att hämta SSO konfigurerats för ditt program, logga in på programmet Lifesize moln med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-174">To get SSO configured for your application, login into the Lifesize Cloud application with Admin privileges.</span></span>

8. <span data-ttu-id="8cdbc-175">Klicka på ditt namn i övre högra hörnet och klicka sedan på den **avancerade inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-175">In the top right corner click on your name and then click on the **Advance Settings**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. <span data-ttu-id="8cdbc-177">I avancerade inställningar nu klickar du på den **SSO Configuration** länk.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-177">In the Advance Settings now click on the **SSO Configuration** link.</span></span> <span data-ttu-id="8cdbc-178">Konfigurationssidan för enkel inloggning för din instans öppnas.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-178">It will open the SSO Configuration page for your instance.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. <span data-ttu-id="8cdbc-180">Nu konfigurera följande värden i Användargränssnittet för SSO-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-180">Now configure the following values in the SSO configuration UI.</span></span>    
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    <span data-ttu-id="8cdbc-182">a.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-182">a.</span></span> <span data-ttu-id="8cdbc-183">I **identitet providern utfärdaren** textruta klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-183">In **Identity Provider Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="8cdbc-184">b.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-184">b.</span></span>  <span data-ttu-id="8cdbc-185">I **Inloggningswebbadressen** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-185">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="8cdbc-186">c.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-186">c.</span></span> <span data-ttu-id="8cdbc-187">Öppna din Base64-kodade certifikatet i anteckningar som hämtas från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **X.509-certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-187">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="8cdbc-188">d.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-188">d.</span></span> <span data-ttu-id="8cdbc-189">Mappningar för textrutan förnamn ange värdet som i SAML-attributet **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-189">In the SAML Attribute mappings for the First Name text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span></span>
    
    <span data-ttu-id="8cdbc-190">e.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-190">e.</span></span> <span data-ttu-id="8cdbc-191">I mappningen för SAML-attributet för den **efternamn** textrutan anger du värdet som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-191">In the SAML Attribute mapping for the **Last Name** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span></span>
    
    <span data-ttu-id="8cdbc-192">f.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-192">f.</span></span> <span data-ttu-id="8cdbc-193">I mappningen för SAML-attributet för den **e-post** textrutan anger du värdet som **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-193">In the SAML Attribute mapping for the **Email** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span></span>

11. <span data-ttu-id="8cdbc-194">Kontrollera konfigurationen kan du klicka på den **Test** knappen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-194">To check the configuration you can click on the **Test** button.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="8cdbc-195">För lyckad testning måste du slutföra guiden för konfiguration av i Azure AD och även ge åtkomst till användare eller grupper som kan utföra testet.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-195">For successful testing you need to complete the configuration wizard in Azure AD and also provide access to users or groups who can perform the test.</span></span>

12. <span data-ttu-id="8cdbc-196">Aktivera SSO genom att kontrollera om den **aktivera enkel inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-196">Enable the SSO by checking on the **Enable SSO** button.</span></span>

13. <span data-ttu-id="8cdbc-197">Klicka på den **uppdatering** knappen så att alla inställningar har sparats.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-197">Now click on the **Update** button so that all the settings are saved.</span></span> <span data-ttu-id="8cdbc-198">Detta genererar RelayState-värdet.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-198">This will generate the RelayState value.</span></span> <span data-ttu-id="8cdbc-199">Kopiera värdet RelayState genereras i textrutan, klistra in den i den **Relay tillstånd** textruta under **Lifesize moln domän och URL: er** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-199">Copy the RelayState value, which is generated in the text box, paste it in the **Relay State** textbox under **Lifesize Cloud Domain and URLs** section.</span></span> 

> [!TIP]
> <span data-ttu-id="8cdbc-200">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8cdbc-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8cdbc-201">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8cdbc-202">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8cdbc-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8cdbc-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8cdbc-203">Creating an Azure AD test user</span></span>

<span data-ttu-id="8cdbc-204">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8cdbc-206">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8cdbc-207">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8cdbc-209">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8cdbc-211">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8cdbc-213">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8cdbc-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8cdbc-215">a.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-215">a.</span></span> <span data-ttu-id="8cdbc-216">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8cdbc-217">b.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-217">b.</span></span> <span data-ttu-id="8cdbc-218">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8cdbc-219">c.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-219">c.</span></span> <span data-ttu-id="8cdbc-220">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8cdbc-221">d.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-221">d.</span></span> <span data-ttu-id="8cdbc-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-222">Click **Create**.</span></span>
 
### <a name="creating-a-lifesize-cloud-test-user"></a><span data-ttu-id="8cdbc-223">Skapa en testanvändare Lifesize moln</span><span class="sxs-lookup"><span data-stu-id="8cdbc-223">Creating a Lifesize Cloud test user</span></span>

<span data-ttu-id="8cdbc-224">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Lifesize molnet.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-224">In this section, you create a user called Britta Simon in Lifesize Cloud.</span></span> <span data-ttu-id="8cdbc-225">Lifesize molnet stöder automatisk användaretablering.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-225">Lifesize cloud does support automatic user provisioning.</span></span> <span data-ttu-id="8cdbc-226">Efter en lyckad autentisering vid Azure AD användaren automatiskt att etableras i programmet.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-226">After successful authentication at Azure AD, the user will be automatically provisioned in the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8cdbc-227">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8cdbc-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8cdbc-228">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Lifesize moln.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lifesize Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8cdbc-230">**Om du vill tilldela Lifesize moln Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8cdbc-230">**To assign Britta Simon to Lifesize Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="8cdbc-231">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8cdbc-233">Välj i listan med program **Lifesize moln**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-233">In the applications list, select **Lifesize Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. <span data-ttu-id="8cdbc-235">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8cdbc-237">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-237">Click **Add** button.</span></span> <span data-ttu-id="8cdbc-238">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8cdbc-240">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8cdbc-241">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8cdbc-242">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8cdbc-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8cdbc-243">Testing single sign-on</span></span>

<span data-ttu-id="8cdbc-244">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8cdbc-245">När du klickar på panelen Lifesize moln på åtkomstpanelen bör du få inloggningssidan för Lifesize molnapp.</span><span class="sxs-lookup"><span data-stu-id="8cdbc-245">When you click the Lifesize Cloud tile in the Access Panel, you should get login page of Lifesize Cloud application.</span></span>
<span data-ttu-id="8cdbc-246">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8cdbc-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8cdbc-247">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8cdbc-247">Additional resources</span></span>

* [<span data-ttu-id="8cdbc-248">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8cdbc-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8cdbc-249">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8cdbc-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

