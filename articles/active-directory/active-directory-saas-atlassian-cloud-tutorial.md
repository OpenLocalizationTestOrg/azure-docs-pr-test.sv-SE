---
title: "Självstudier: Azure Active Directory-integrering med molntjänster Atlassian | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Atlassian moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 2891838b56dd15cb5f97dcae391770143a80c781
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a><span data-ttu-id="ca516-103">Självstudier: Azure Active Directory-integrering med Atlassian moln</span><span class="sxs-lookup"><span data-stu-id="ca516-103">Tutorial: Azure Active Directory integration with Atlassian Cloud</span></span>

<span data-ttu-id="ca516-104">I kursen får lära du att integrera Atlassian moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ca516-104">In this tutorial, you learn how to integrate Atlassian Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ca516-105">Integrera Atlassian moln med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ca516-105">Integrating Atlassian Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ca516-106">Du kan styra i Azure AD som har åtkomst till Atlassian moln</span><span class="sxs-lookup"><span data-stu-id="ca516-106">You can control in Azure AD who has access to Atlassian Cloud</span></span>
- <span data-ttu-id="ca516-107">Du kan aktivera användarna att automatiskt hämta loggat in på molnet Atlassian (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ca516-107">You can enable your users to automatically get signed-on to Atlassian Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ca516-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ca516-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ca516-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ca516-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca516-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ca516-110">Prerequisites</span></span>

<span data-ttu-id="ca516-111">För att konfigurera Azure AD-integrering med Atlassian molnet, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="ca516-111">To configure Azure AD integration with Atlassian Cloud, you need the following items:</span></span>

- <span data-ttu-id="ca516-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ca516-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ca516-113">En Atlassian enkel inloggning aktiverad molnprenumeration</span><span class="sxs-lookup"><span data-stu-id="ca516-113">An Atlassian Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ca516-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ca516-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ca516-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ca516-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ca516-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ca516-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ca516-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca516-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ca516-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ca516-118">Scenario description</span></span>
<span data-ttu-id="ca516-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ca516-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ca516-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ca516-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ca516-121">Att lägga till Atlassian moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="ca516-121">Adding Atlassian Cloud from the gallery</span></span>
2. <span data-ttu-id="ca516-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca516-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-atlassian-cloud-from-the-gallery"></a><span data-ttu-id="ca516-123">Att lägga till Atlassian moln från galleriet</span><span class="sxs-lookup"><span data-stu-id="ca516-123">Adding Atlassian Cloud from the gallery</span></span>
<span data-ttu-id="ca516-124">Du måste lägga till Atlassian moln från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Atlassian moln i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ca516-124">To configure the integration of Atlassian Cloud into Azure AD, you need to add Atlassian Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ca516-125">**Utför följande steg för att lägga till Atlassian moln från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="ca516-125">**To add Atlassian Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ca516-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ca516-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ca516-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ca516-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ca516-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ca516-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="ca516-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca516-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="ca516-133">I sökrutan skriver **Atlassian moln**.</span><span class="sxs-lookup"><span data-stu-id="ca516-133">In the search box, type **Atlassian Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. <span data-ttu-id="ca516-135">Välj i resultatpanelen **Atlassian moln**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="ca516-135">In the results panel, select **Atlassian Cloud**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ca516-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca516-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ca516-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Atlassian molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ca516-138">In this section, you configure and test Azure AD single sign-on with Atlassian Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="ca516-139">Azure AD måste du känna till motsvarande användaren i Atlassian molnet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="ca516-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Atlassian Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="ca516-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Atlassian molnet upprättas.</span><span class="sxs-lookup"><span data-stu-id="ca516-140">In other words, a link relationship between an Azure AD user and the related user in Atlassian Cloud needs to be established.</span></span>

<span data-ttu-id="ca516-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Atlassian molnet.</span><span class="sxs-lookup"><span data-stu-id="ca516-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Atlassian Cloud.</span></span>

<span data-ttu-id="ca516-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Atlassian moln, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ca516-142">To configure and test Azure AD single sign-on with Atlassian Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ca516-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ca516-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ca516-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca516-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ca516-145">**[Skapa en testanvändare Atlassian moln](#creating-an-atlassian-cloud-test-user)**  – du har en motsvarighet för Britta Simon i Atlassian moln som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ca516-145">**[Creating an Atlassian Cloud test user](#creating-an-atlassian-cloud-test-user)** - to have a counterpart of Britta Simon in Atlassian Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ca516-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca516-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ca516-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ca516-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ca516-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca516-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ca516-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i tillämpningsprogrammet Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="ca516-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Atlassian Cloud application.</span></span>

<span data-ttu-id="ca516-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Atlassian molnet:**</span><span class="sxs-lookup"><span data-stu-id="ca516-150">**To configure Azure AD single sign-on with Atlassian Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="ca516-151">I Azure-portalen på den **Atlassian moln** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ca516-151">In the Azure portal, on the **Atlassian Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="ca516-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca516-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. <span data-ttu-id="ca516-155">På den **Atlassian moln domän och URL: er** avsnittet, utför följande steg om du vill konfigurera programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="ca516-155">On the **Atlassian Cloud Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    <span data-ttu-id="ca516-157">a.</span><span class="sxs-lookup"><span data-stu-id="ca516-157">a.</span></span> <span data-ttu-id="ca516-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<instancename>.atlassian.net/admin/saml/edit`</span><span class="sxs-lookup"><span data-stu-id="ca516-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<instancename>.atlassian.net/admin/saml/edit`</span></span>

    <span data-ttu-id="ca516-159">b.</span><span class="sxs-lookup"><span data-stu-id="ca516-159">b.</span></span> <span data-ttu-id="ca516-160">I den **Reply URL** textruta Skriv en URL som:`https://id.atlassian.com/login/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="ca516-160">In the **Reply URL** textbox, type a URL as: `https://id.atlassian.com/login/saml/acs`</span></span>

4. <span data-ttu-id="ca516-161">Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du vill konfigurera programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="ca516-161">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    <span data-ttu-id="ca516-163">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<instancename>.atlassian.net`</span><span class="sxs-lookup"><span data-stu-id="ca516-163">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.atlassian.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ca516-164">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ca516-164">These values are not real.</span></span> <span data-ttu-id="ca516-165">Uppdatera dessa värden med den faktiska identifierare och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ca516-165">Update these values with the actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="ca516-166">Du kan få exakta värden från Atlassian Molnkonfigurationen för SAML-skärmen.</span><span class="sxs-lookup"><span data-stu-id="ca516-166">You can get the exact values from Atlassian Cloud SAML Configuration screen.</span></span>
 
5. <span data-ttu-id="ca516-167">På den **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="ca516-167">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. <span data-ttu-id="ca516-169">På den **Atlassian Molnkonfigurationen** klickar du på **konfigurera Atlassian moln** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ca516-169">On the **Atlassian Cloud Configuration** section, click **Configure Atlassian Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="ca516-170">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ca516-170">Copy the **SAML Entity ID and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. <span data-ttu-id="ca516-172">Att hämta SSO konfigurerats för ditt program, logga in på Atlassian-portalen med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="ca516-172">To get SSO configured for your application, login to the Atlassian Portal using the administrator rights.</span></span>

8. <span data-ttu-id="ca516-173">I avsnittet autentisering i det vänstra navigeringsfönstret klickar du på **domäner**.</span><span class="sxs-lookup"><span data-stu-id="ca516-173">In the Authentication section of the left navigation click **Domains**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    <span data-ttu-id="ca516-175">a.</span><span class="sxs-lookup"><span data-stu-id="ca516-175">a.</span></span> <span data-ttu-id="ca516-176">Ange domännamnet i textrutan, och klicka sedan på **Lägg till domän**.</span><span class="sxs-lookup"><span data-stu-id="ca516-176">In the textbox, type your domain name, and then click **Add domain**.</span></span>
        
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    <span data-ttu-id="ca516-178">b.</span><span class="sxs-lookup"><span data-stu-id="ca516-178">b.</span></span> <span data-ttu-id="ca516-179">Om du vill verifiera domänen, klickar du på **Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="ca516-179">To verify the domain, click **Verify**.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    <span data-ttu-id="ca516-181">c.</span><span class="sxs-lookup"><span data-stu-id="ca516-181">c.</span></span> <span data-ttu-id="ca516-182">Hämta domän verifiering HTML-filen och överföra den till rotmappen för din domän webbplatsen och klicka sedan på **verifiera domän**.</span><span class="sxs-lookup"><span data-stu-id="ca516-182">Download the domain verification html file, upload it to the root folder of your domain's website, and then click **Verify domain**.</span></span>
    
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    <span data-ttu-id="ca516-184">d.</span><span class="sxs-lookup"><span data-stu-id="ca516-184">d.</span></span> <span data-ttu-id="ca516-185">När domänen har verifierats, värdet för den **Status** fältet är **verifierad**.</span><span class="sxs-lookup"><span data-stu-id="ca516-185">Once the domain is verified, the value of the **Status** field is **Verified**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. <span data-ttu-id="ca516-187">I det vänstra navigeringsfältet klickar du på **SAML**.</span><span class="sxs-lookup"><span data-stu-id="ca516-187">In the left navigation bar, click **SAML**.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. <span data-ttu-id="ca516-189">Skapa en SAML-konfiguration och Lägg till providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="ca516-189">Create a SAML Configuration and add the Identity provider configuration.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    <span data-ttu-id="ca516-191">a.</span><span class="sxs-lookup"><span data-stu-id="ca516-191">a.</span></span> <span data-ttu-id="ca516-192">I den **identitetsleverantör enhets-ID** text klistra in värdet för **SAML enhets-ID** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ca516-192">In the **Identity provider Entity ID** text box, paste the value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="ca516-193">b.</span><span class="sxs-lookup"><span data-stu-id="ca516-193">b.</span></span> <span data-ttu-id="ca516-194">I den **identitetsleverantör SSO URL** text klistra in värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ca516-194">In the **Identity provider SSO URL** text box, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="ca516-195">c.</span><span class="sxs-lookup"><span data-stu-id="ca516-195">c.</span></span> <span data-ttu-id="ca516-196">Öppna hämtat certifikat från Azure-portalen och kopiera värdena utan start- och avsluta rader och klistra in den i den **offentliga X509 certifikat** rutan.</span><span class="sxs-lookup"><span data-stu-id="ca516-196">Open the downloaded certificate from Azure portal and copy the values without the Begin and End lines and paste it in the **Public X509 certificate** box.</span></span>
    
    <span data-ttu-id="ca516-197">d.</span><span class="sxs-lookup"><span data-stu-id="ca516-197">d.</span></span> <span data-ttu-id="ca516-198">Klicka på **spara konfigurationen** spara inställningarna.</span><span class="sxs-lookup"><span data-stu-id="ca516-198">Click **Save Configuration**  to Save the settings.</span></span>
     
11. <span data-ttu-id="ca516-199">Uppdatera Azure AD-inställningar och kontrollera att du har installerat rätt ID-URL.</span><span class="sxs-lookup"><span data-stu-id="ca516-199">Update the Azure AD settings to make sure that you have setup the correct Identifier URL.</span></span>
  
    <span data-ttu-id="ca516-200">a.</span><span class="sxs-lookup"><span data-stu-id="ca516-200">a.</span></span> <span data-ttu-id="ca516-201">Kopiera den **SP identitet ID** från SAML skärmen och klistra in den i Azure AD som den **identifierare** värde.</span><span class="sxs-lookup"><span data-stu-id="ca516-201">Copy the **SP Identity ID** from the SAML screen and paste it in Azure AD as the **Identifier** value.</span></span>

    <span data-ttu-id="ca516-202">b.</span><span class="sxs-lookup"><span data-stu-id="ca516-202">b.</span></span> <span data-ttu-id="ca516-203">Inloggning på URL: en är klient-URL för ditt Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="ca516-203">Sign On URL is the tenant URL of your Atlassian Cloud.</span></span>   

     ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. <span data-ttu-id="ca516-205">I Azure-portalen klickar du på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca516-205">In the Azure portal, Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="ca516-207">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="ca516-207">You can now read a concise version of these instructions inside the [Azure  portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ca516-208">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="ca516-208">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ca516-209">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ca516-209">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ca516-210">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ca516-210">Creating an Azure AD test user</span></span>
<span data-ttu-id="ca516-211">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca516-211">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="ca516-213">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="ca516-213">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ca516-214">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ca516-214">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ca516-216">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ca516-216">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ca516-218">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca516-218">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ca516-220">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="ca516-220">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ca516-222">a.</span><span class="sxs-lookup"><span data-stu-id="ca516-222">a.</span></span> <span data-ttu-id="ca516-223">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ca516-223">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ca516-224">b.</span><span class="sxs-lookup"><span data-stu-id="ca516-224">b.</span></span> <span data-ttu-id="ca516-225">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ca516-225">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ca516-226">c.</span><span class="sxs-lookup"><span data-stu-id="ca516-226">c.</span></span> <span data-ttu-id="ca516-227">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ca516-227">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ca516-228">d.</span><span class="sxs-lookup"><span data-stu-id="ca516-228">d.</span></span> <span data-ttu-id="ca516-229">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ca516-229">Click **Create**.</span></span>
 
### <a name="creating-an-atlassian-cloud-test-user"></a><span data-ttu-id="ca516-230">Skapa en testanvändare Atlassian moln</span><span class="sxs-lookup"><span data-stu-id="ca516-230">Creating an Atlassian Cloud test user</span></span>

<span data-ttu-id="ca516-231">Om du vill aktivera Azure AD-användare kan logga in på molnet Atlassian etableras de i Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="ca516-231">To enable Azure AD users to log in to Atlassian Cloud, they must be provisioned into Atlassian Cloud.</span></span>  
<span data-ttu-id="ca516-232">Vid Atlassian moln är att etablera en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ca516-232">In case of Atlassian Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="ca516-233">**Utför följande steg om du vill konfigurera ett användarkonto:**</span><span class="sxs-lookup"><span data-stu-id="ca516-233">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="ca516-234">I avsnittet plats klickar du på den **användare** knappen</span><span class="sxs-lookup"><span data-stu-id="ca516-234">In the Site administration section, click the **Users** button</span></span>

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. <span data-ttu-id="ca516-236">Klicka på den **skapa användare** för att skapa en användare i molnet Atlassian</span><span class="sxs-lookup"><span data-stu-id="ca516-236">Click the **Create User** button to create a user in the Atlassian Cloud</span></span>

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. <span data-ttu-id="ca516-238">Ange användarens **e-postadress**, **användarnamn**, och **fullständiga namn** och tilldela åtkomst till program.</span><span class="sxs-lookup"><span data-stu-id="ca516-238">Enter the user's **Email address**, **Username**, and **Full Name** and assign the application access.</span></span> 

    ![Skapa Atlassian moln användare](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. <span data-ttu-id="ca516-240">Klicka på **skapa användare** knappen, kommer att skicka e-postinbjudan för användaren och efter acceptera inbjudan användaren kommer att vara aktiv i systemet.</span><span class="sxs-lookup"><span data-stu-id="ca516-240">Click **Create user** button, it will send the email invitation to the user and after accepting the invitation the user will be active in the system.</span></span> 

>[!NOTE] 
><span data-ttu-id="ca516-241">Du kan också skapa grupp användare genom att klicka på den **Bulk skapa** i avsnittet användare.</span><span class="sxs-lookup"><span data-stu-id="ca516-241">You can also create the bulk users by clicking the **Bulk Create** button in the Users section.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ca516-242">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="ca516-242">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ca516-243">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="ca516-243">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Atlassian Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="ca516-245">**Om du vill tilldela Atlassian moln Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ca516-245">**To assign Britta Simon to Atlassian Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="ca516-246">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ca516-246">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ca516-248">Välj i listan med program **Atlassian moln**.</span><span class="sxs-lookup"><span data-stu-id="ca516-248">In the applications list, select **Atlassian Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. <span data-ttu-id="ca516-250">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ca516-250">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="ca516-252">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ca516-252">Click **Add** button.</span></span> <span data-ttu-id="ca516-253">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca516-253">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="ca516-255">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="ca516-255">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ca516-256">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca516-256">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ca516-257">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ca516-257">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ca516-258">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ca516-258">Testing single sign-on</span></span>

<span data-ttu-id="ca516-259">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ca516-259">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="ca516-260">När du klickar på panelen Atlassian moln på åtkomstpanelen du ska hämta automatiskt loggat in i tillämpningsprogrammet Atlassian moln.</span><span class="sxs-lookup"><span data-stu-id="ca516-260">When you click the Atlassian Cloud tile in the Access Panel, you should get automatically signed-on to your Atlassian Cloud application.</span></span> <span data-ttu-id="ca516-261">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="ca516-261">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ca516-262">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ca516-262">Additional resources</span></span>

* [<span data-ttu-id="ca516-263">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ca516-263">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ca516-264">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ca516-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

