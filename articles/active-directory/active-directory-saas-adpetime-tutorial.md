---
title: "Självstudier: Azure Active Directory-integrering med ADP eTime | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och ADP eTime."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 31fed307a32e629d00aab7cc9d5167ee16d83936
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a><span data-ttu-id="8f2a8-103">Självstudier: Azure Active Directory-integrering med ADP eTime</span><span class="sxs-lookup"><span data-stu-id="8f2a8-103">Tutorial: Azure Active Directory integration with ADP eTime</span></span>

<span data-ttu-id="8f2a8-104">I kursen får lära du att integrera ADP eTime med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8f2a8-104">In this tutorial, you learn how to integrate ADP eTime with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8f2a8-105">Integrera ADP eTime med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-105">Integrating ADP eTime with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8f2a8-106">Du kan styra i Azure AD som har tillgång till ADP eTime</span><span class="sxs-lookup"><span data-stu-id="8f2a8-106">You can control in Azure AD who has access to ADP eTime</span></span>
- <span data-ttu-id="8f2a8-107">Du kan aktivera användarna att automatiskt hämta loggat in på ADP eTime (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8f2a8-107">You can enable your users to automatically get signed-on to ADP eTime (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8f2a8-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8f2a8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8f2a8-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8f2a8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f2a8-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8f2a8-110">Prerequisites</span></span>

<span data-ttu-id="8f2a8-111">Om du vill konfigurera Azure AD-integrering med ADP eTime behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-111">To configure Azure AD integration with ADP eTime, you need the following items:</span></span>

- <span data-ttu-id="8f2a8-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8f2a8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8f2a8-113">En ADP eTime enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8f2a8-113">An ADP eTime single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8f2a8-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8f2a8-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8f2a8-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8f2a8-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f2a8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8f2a8-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8f2a8-118">Scenario description</span></span>
<span data-ttu-id="8f2a8-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8f2a8-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8f2a8-121">Att lägga till ADP eTime från galleriet</span><span class="sxs-lookup"><span data-stu-id="8f2a8-121">Adding ADP eTime from the gallery</span></span>
2. <span data-ttu-id="8f2a8-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f2a8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adp-etime-from-the-gallery"></a><span data-ttu-id="8f2a8-123">Att lägga till ADP eTime från galleriet</span><span class="sxs-lookup"><span data-stu-id="8f2a8-123">Adding ADP eTime from the gallery</span></span>
<span data-ttu-id="8f2a8-124">Du måste lägga till ADP eTime från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av ADP eTime i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-124">To configure the integration of ADP eTime into Azure AD, you need to add ADP eTime from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8f2a8-125">**Utför följande steg för att lägga till ADP eTime från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="8f2a8-125">**To add ADP eTime from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8f2a8-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8f2a8-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8f2a8-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8f2a8-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8f2a8-133">I sökrutan skriver **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-133">In the search box, type **ADP eTime**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_search.png)

5. <span data-ttu-id="8f2a8-135">Välj i resultatpanelen **ADP eTime**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-135">In the results panel, select **ADP eTime**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8f2a8-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f2a8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8f2a8-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ADP eTime baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-138">In this section, you configure and test Azure AD single sign-on with ADP eTime based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8f2a8-139">Azure AD måste du känna till användaren i ADP eTime motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ADP eTime is to a user in Azure AD.</span></span> <span data-ttu-id="8f2a8-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i ADP eTime upprättas.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-140">In other words, a link relationship between an Azure AD user and the related user in ADP eTime needs to be established.</span></span>

<span data-ttu-id="8f2a8-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ADP eTime.</span></span>

<span data-ttu-id="8f2a8-142">Om du vill konfigurera och testa Azure AD enkel inloggning med ADP eTime, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-142">To configure and test Azure AD single sign-on with ADP eTime, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8f2a8-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8f2a8-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8f2a8-145">**[Skapa en testanvändare för ADP eTime](#creating-an-adp-etime-test-user)**  – du har en motsvarighet för Britta Simon i ADP eTime som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-145">**[Creating an ADP eTime test user](#creating-an-adp-etime-test-user)** - to have a counterpart of Britta Simon in ADP eTime that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8f2a8-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8f2a8-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8f2a8-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f2a8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8f2a8-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt ADP eTime program.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ADP eTime application.</span></span>

<span data-ttu-id="8f2a8-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med ADP eTime:**</span><span class="sxs-lookup"><span data-stu-id="8f2a8-150">**To configure Azure AD single sign-on with ADP eTime, perform the following steps:**</span></span>

1. <span data-ttu-id="8f2a8-151">I Azure-portalen på den **ADP eTime** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-151">In the Azure portal, on the **ADP eTime** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8f2a8-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_samlbase.png)

3. <span data-ttu-id="8f2a8-155">På den **ADP eTime domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-155">On the **ADP eTime Domain and URLs** section, perform the following step:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_url.png)

    <span data-ttu-id="8f2a8-157">a.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-157">a.</span></span> <span data-ttu-id="8f2a8-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<servername>.adp.com`</span><span class="sxs-lookup"><span data-stu-id="8f2a8-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<servername>.adp.com`</span></span>

    <span data-ttu-id="8f2a8-159">b.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-159">b.</span></span> <span data-ttu-id="8f2a8-160">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span><span class="sxs-lookup"><span data-stu-id="8f2a8-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<servername>.adp.com/affwebservices/public/saml2assertionconsumer`</span></span> 
 
    > [!NOTE] 
    > <span data-ttu-id="8f2a8-161">Dessa värden är inte verkligt.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-161">These values are not the real.</span></span> <span data-ttu-id="8f2a8-162">Uppdatera dessa värden med den faktiska Reply URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-162">Update these values with the actual Reply URL and Identifier.</span></span> <span data-ttu-id="8f2a8-163">Kontakta [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-163">Contact [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) to get these values.</span></span>

4. <span data-ttu-id="8f2a8-164">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_certificate.png) 

5. <span data-ttu-id="8f2a8-166">ADP eTime program förväntar SAML-intyg i ett specifikt format, vilket kräver att du kan lägga till anpassade attributmappning konfigurationen för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-166">The ADP eTime application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="8f2a8-167">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-167">The following screenshot shows an example for this.</span></span> <span data-ttu-id="8f2a8-168">Anspråkets namn kommer alltid att **”PersonImmutableID”** och värdet som vi har mappats till ExtensionAttribute2 som innehåller EmployeeID för användaren.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-168">The claim name will always be **"PersonImmutableID"** and the value of which we have mapped to ExtensionAttribute2 which contains the EmployeeID of the user.</span></span> 

    <span data-ttu-id="8f2a8-169">Här mappning från Azure AD till ADP eTime kommer att utföras på EmployeeID men du kan mappa till ett annat värde som också baserat på dina inställningar för programmet.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-169">Here the user mapping from Azure AD to ADP eTime will be done on the EmployeeID but you can map this to a different value also based on your application settings.</span></span> <span data-ttu-id="8f2a8-170">Så kan du arbeta med [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) först att använda rätt ID för en användare och mappa värdet med den **”PersonImmutableID”** anspråk.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-170">So please work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) first to use the correct identifier of a user and map that value with the **"PersonImmutableID"** claim.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_attribute.png)

6. <span data-ttu-id="8f2a8-172">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan Konfigurera attribut för SAML-token som visas i bilden och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-172">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image and perform the following steps:</span></span>
    
    | <span data-ttu-id="8f2a8-173">Attributets namn</span><span class="sxs-lookup"><span data-stu-id="8f2a8-173">Attribute Name</span></span> | <span data-ttu-id="8f2a8-174">Attributvärdet</span><span class="sxs-lookup"><span data-stu-id="8f2a8-174">Attribute Value</span></span> |
    | ------------------- | -------------------- |    
    | <span data-ttu-id="8f2a8-175">PersonImmutableID</span><span class="sxs-lookup"><span data-stu-id="8f2a8-175">PersonImmutableID</span></span> | <span data-ttu-id="8f2a8-176">User.extensionattribute2</span><span class="sxs-lookup"><span data-stu-id="8f2a8-176">user.extensionattribute2</span></span> |
    
    <span data-ttu-id="8f2a8-177">a.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-177">a.</span></span> <span data-ttu-id="8f2a8-178">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-178">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_04.png)

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="8f2a8-181">b.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-181">b.</span></span> <span data-ttu-id="8f2a8-182">I den **namn** textruta ange attributets namn visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-182">In the **Name** textbox, type the attribute name shown for that row.</span></span>

    <span data-ttu-id="8f2a8-183">c.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-183">c.</span></span> <span data-ttu-id="8f2a8-184">Från den **värdet** listan, ange det attributvärde som visas för den raden.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-184">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="8f2a8-185">d.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-185">d.</span></span> <span data-ttu-id="8f2a8-186">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-186">Click **Ok**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8f2a8-187">Innan du kan konfigurera SAML-kontroll måste du kontakta din [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) och begära värdet för attributet för unik identifierare för din klient.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-187">Before you can configure the SAML assertion, you need to contact your [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="8f2a8-188">Du behöver det här värdet för att konfigurera det anpassade anspråket för ditt program.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-188">You need this value to configure the custom claim for your application.</span></span> 

7. <span data-ttu-id="8f2a8-189">På den **ADP eTime Configuration** klickar du på **konfigurera ADP eTime** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-189">On the **ADP eTime Configuration** section, click **Configure ADP eTime** to open **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_configure.png) 

8. <span data-ttu-id="8f2a8-191">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-191">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="8f2a8-193">Konfigurera enkel inloggning på **ADP eTime** sida, måste du skicka den hämtade **XML-Metadata för** till [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx).</span><span class="sxs-lookup"><span data-stu-id="8f2a8-193">To configure single sign-on on **ADP eTime** side, you need to send the downloaded **Metadata XML** to [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx).</span></span> 

> [!TIP]
> <span data-ttu-id="8f2a8-194">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="8f2a8-194">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8f2a8-195">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-195">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8f2a8-196">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8f2a8-196">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8f2a8-197">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8f2a8-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="8f2a8-198">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-198">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8f2a8-200">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="8f2a8-200">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8f2a8-201">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-201">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8f2a8-203">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-203">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8f2a8-205">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-205">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8f2a8-207">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="8f2a8-207">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8f2a8-209">a.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-209">a.</span></span> <span data-ttu-id="8f2a8-210">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-210">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8f2a8-211">b.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-211">b.</span></span> <span data-ttu-id="8f2a8-212">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-212">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8f2a8-213">c.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-213">c.</span></span> <span data-ttu-id="8f2a8-214">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-214">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8f2a8-215">d.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-215">d.</span></span> <span data-ttu-id="8f2a8-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-216">Click **Create**.</span></span>
 
### <a name="creating-an-adp-etime-test-user"></a><span data-ttu-id="8f2a8-217">Skapa en ADP eTime testanvändare</span><span class="sxs-lookup"><span data-stu-id="8f2a8-217">Creating an ADP eTime test user</span></span>

<span data-ttu-id="8f2a8-218">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-218">The objective of this section is to create a user called Britta Simon in ADP eTime.</span></span> <span data-ttu-id="8f2a8-219">Arbeta med [ADP eTime supportteamet](https://www.adp.com/contact-us/overview.aspx) att lägga till användare i ADP eTime kontot.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-219">Work with [ADP eTime support team](https://www.adp.com/contact-us/overview.aspx) to add the users in the ADP eTime account.</span></span> 
   
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8f2a8-220">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="8f2a8-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8f2a8-221">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till ADP eTime.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-221">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ADP eTime.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8f2a8-223">**Om du vill tilldela ADP eTime Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8f2a8-223">**To assign Britta Simon to ADP eTime, perform the following steps:**</span></span>

1. <span data-ttu-id="8f2a8-224">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-224">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8f2a8-226">Välj i listan med program **ADP eTime**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-226">In the applications list, select **ADP eTime**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_app.png) 

3. <span data-ttu-id="8f2a8-228">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8f2a8-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-230">Click **Add** button.</span></span> <span data-ttu-id="8f2a8-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8f2a8-233">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8f2a8-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8f2a8-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8f2a8-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8f2a8-236">Testing single sign-on</span></span>

<span data-ttu-id="8f2a8-237">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8f2a8-238">När du klickar på panelen ADP eTime på åtkomstpanelen du bör få automatiskt loggat in på ditt ADP eTime program.</span><span class="sxs-lookup"><span data-stu-id="8f2a8-238">When you click the ADP eTime tile in the Access Panel, you should get automatically signed-on to your ADP eTime application.</span></span>
<span data-ttu-id="8f2a8-239">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8f2a8-239">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8f2a8-240">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8f2a8-240">Additional resources</span></span>

* [<span data-ttu-id="8f2a8-241">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8f2a8-241">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8f2a8-242">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8f2a8-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png

