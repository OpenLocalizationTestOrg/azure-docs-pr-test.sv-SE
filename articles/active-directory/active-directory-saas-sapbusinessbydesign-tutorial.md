---
title: "Självstudier: Azure Active Directory-integrering med SAP Business ByDesign | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SAP Business ByDesign."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: ab76a0ac1ef954efd3c66e6f565514b889ed9444
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a><span data-ttu-id="7ced5-103">Självstudier: Azure Active Directory-integration med SAP Business ByDesign</span><span class="sxs-lookup"><span data-stu-id="7ced5-103">Tutorial: Azure Active Directory integration with SAP Business ByDesign</span></span>

<span data-ttu-id="7ced5-104">Lär dig hur du integrerar SAP Business ByDesign med Azure Active Directory (AD Azure) i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-104">In this tutorial, you learn how to integrate SAP Business ByDesign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7ced5-105">Integrera SAP Business ByDesign med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7ced5-105">Integrating SAP Business ByDesign with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="7ced5-106">Du kan styra i Azure AD som har åtkomst till SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="7ced5-106">You can control in Azure AD who has access to SAP Business ByDesign.</span></span>
- <span data-ttu-id="7ced5-107">Du kan aktivera användarna att automatiskt hämta loggat in på SAP Business ByDesign (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="7ced5-107">You can enable your users to automatically get signed-on to SAP Business ByDesign (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="7ced5-108">Du kan hantera dina konton i en central plats - Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="7ced5-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7ced5-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ced5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7ced5-110">Prerequisites</span></span>

<span data-ttu-id="7ced5-111">Om du vill konfigurera Azure AD-integrering med SAP Business ByDesign behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="7ced5-111">To configure Azure AD integration with SAP Business ByDesign, you need the following items:</span></span>

- <span data-ttu-id="7ced5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ced5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7ced5-113">En SAP Business ByDesign enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ced5-113">An SAP Business ByDesign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7ced5-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ced5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7ced5-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7ced5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7ced5-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7ced5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7ced5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7ced5-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7ced5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7ced5-118">Scenario description</span></span>
<span data-ttu-id="7ced5-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ced5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7ced5-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ced5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7ced5-121">Att lägga till SAP Business ByDesign från galleriet</span><span class="sxs-lookup"><span data-stu-id="7ced5-121">Adding SAP Business ByDesign from the gallery</span></span>
2. <span data-ttu-id="7ced5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ced5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-business-bydesign-from-the-gallery"></a><span data-ttu-id="7ced5-123">Att lägga till SAP Business ByDesign från galleriet</span><span class="sxs-lookup"><span data-stu-id="7ced5-123">Adding SAP Business ByDesign from the gallery</span></span>
<span data-ttu-id="7ced5-124">Du måste lägga till SAP Business ByDesign från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SAP Business ByDesign i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ced5-124">To configure the integration of SAP Business ByDesign into Azure AD, you need to add SAP Business ByDesign from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="7ced5-125">**Utför följande steg för att lägga till SAP Business ByDesign från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="7ced5-125">**To add SAP Business ByDesign from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="7ced5-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7ced5-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Azure Active Directory-knappen][1]

2. <span data-ttu-id="7ced5-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="7ced5-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-129">Then go to **All applications**.</span></span>

    ![Bladet Enterprise program][2]
    
3. <span data-ttu-id="7ced5-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ced5-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Knappen Nytt program][3]

4. <span data-ttu-id="7ced5-133">I sökrutan skriver **SAP Business ByDesign**väljer **SAP Business ByDesign** resultatet-panelen klickar **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="7ced5-133">In the search box, type **SAP Business ByDesign**, select **SAP Business ByDesign** from result panel then click **Add** button to add the application.</span></span>

    ![SAP Business ByDesign i resultatlistan](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="7ced5-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ced5-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="7ced5-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP Business ByDesign baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7ced5-136">In this section, you configure and test Azure AD single sign-on with SAP Business ByDesign based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7ced5-137">Azure AD måste du känna till användaren i SAP Business ByDesign motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="7ced5-137">For single sign-on to work, Azure AD needs to know what the counterpart user in SAP Business ByDesign is to a user in Azure AD.</span></span> <span data-ttu-id="7ced5-138">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SAP Business ByDesign upprättas.</span><span class="sxs-lookup"><span data-stu-id="7ced5-138">In other words, a link relationship between an Azure AD user and the related user in SAP Business ByDesign needs to be established.</span></span>

<span data-ttu-id="7ced5-139">I SAP Business ByDesign tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-139">In SAP Business ByDesign, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="7ced5-140">Om du vill konfigurera och testa Azure AD enkel inloggning med SAP Business ByDesign, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ced5-140">To configure and test Azure AD single sign-on with SAP Business ByDesign, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="7ced5-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="7ced5-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ced5-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7ced5-143">**[Skapa en SAP Business ByDesign testanvändare](#create-an-sap-business-bydesign-test-user)**  – du har en motsvarighet för Britta Simon i SAP Business ByDesign som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7ced5-143">**[Create an SAP Business ByDesign test user](#create-an-sap-business-bydesign-test-user)** - to have a counterpart of Britta Simon in SAP Business ByDesign that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="7ced5-144">**[Tilldela Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ced5-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7ced5-145">**[Testa enkel inloggning](#test-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7ced5-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="7ced5-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ced5-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="7ced5-147">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt program för SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="7ced5-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAP Business ByDesign application.</span></span>

<span data-ttu-id="7ced5-148">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SAP Business ByDesign:**</span><span class="sxs-lookup"><span data-stu-id="7ced5-148">**To configure Azure AD single sign-on with SAP Business ByDesign, perform the following steps:**</span></span>

1. <span data-ttu-id="7ced5-149">I Azure-portalen på den **SAP Business ByDesign** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-149">In the Azure portal, on the **SAP Business ByDesign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="7ced5-151">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ced5-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. <span data-ttu-id="7ced5-153">På den **SAP Business ByDesign domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ced5-153">On the **SAP Business ByDesign Domain and URLs** section, perform the following steps:</span></span>

    ![URL: er och SAP Business ByDesign domän med enkel inloggning information](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    <span data-ttu-id="7ced5-155">a.</span><span class="sxs-lookup"><span data-stu-id="7ced5-155">a.</span></span> <span data-ttu-id="7ced5-156">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="7ced5-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<servername>.sapbydesign.com`</span></span>

    <span data-ttu-id="7ced5-157">b.</span><span class="sxs-lookup"><span data-stu-id="7ced5-157">b.</span></span> <span data-ttu-id="7ced5-158">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="7ced5-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<servername>.sapbydesign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7ced5-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7ced5-159">These values are not real.</span></span> <span data-ttu-id="7ced5-160">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7ced5-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7ced5-161">Kontakta [SAP Business ByDesign Client supportteamet](https://www.sap.com/products/cloud-analytics.support.html) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7ced5-161">Contact [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) to get these values.</span></span>

4. <span data-ttu-id="7ced5-162">På den **användarattribut** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ced5-162">On the **User Attributes** section, perform the following steps:</span></span>

    ![SAP Business ByDesign attributet avsnitt](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    <span data-ttu-id="7ced5-164">a.</span><span class="sxs-lookup"><span data-stu-id="7ced5-164">a.</span></span> <span data-ttu-id="7ced5-165">I **användar-ID** väljer den **ExtractMailPrefix()** funktion.</span><span class="sxs-lookup"><span data-stu-id="7ced5-165">In **User Identifier** list, select the **ExtractMailPrefix()** function.</span></span>
    
    <span data-ttu-id="7ced5-166">b.</span><span class="sxs-lookup"><span data-stu-id="7ced5-166">b.</span></span> <span data-ttu-id="7ced5-167">Från den **e** väljer användarattribut som du vill använda för din implementering.</span><span class="sxs-lookup"><span data-stu-id="7ced5-167">From the **Mail** list, select the user attribute you want to use for your implementation.</span></span> <span data-ttu-id="7ced5-168">Till exempel om du vill använda EmployeeID som unikt identifierare och du har lagrat attributvärdet i ExtensionAttribute2, välj sedan user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="7ced5-168">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select user.extensionattribute2.</span></span>     

5. <span data-ttu-id="7ced5-169">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="7ced5-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Länken hämta certifikatet](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. <span data-ttu-id="7ced5-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="7ced5-173">På den **SAP Business ByDesign Configuration** klickar du på **konfigurera SAP Business ByDesign** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7ced5-173">On the **SAP Business ByDesign Configuration** section, click **Configure SAP Business ByDesign** to open **Configure sign-on** window.</span></span> <span data-ttu-id="7ced5-174">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7ced5-174">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![SAP Business ByDesign konfiguration](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. <span data-ttu-id="7ced5-176">Utför följande steg för att få SSO konfigurerats för ditt program:</span><span class="sxs-lookup"><span data-stu-id="7ced5-176">To get SSO configured for your application, perform the following steps:</span></span>
   
    <span data-ttu-id="7ced5-177">a.</span><span class="sxs-lookup"><span data-stu-id="7ced5-177">a.</span></span> <span data-ttu-id="7ced5-178">Logga in på din SAP Business ByDesign portal med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="7ced5-178">Sign on to your SAP Business ByDesign portal with administrator rights.</span></span>
   
    <span data-ttu-id="7ced5-179">b.</span><span class="sxs-lookup"><span data-stu-id="7ced5-179">b.</span></span> <span data-ttu-id="7ced5-180">Gå till **program och vanliga användare hanteringsaktivitet** och klicka på den **identitetsleverantör** fliken.</span><span class="sxs-lookup"><span data-stu-id="7ced5-180">Navigate to **Application and User Management Common Task** and click the **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="7ced5-181">c.</span><span class="sxs-lookup"><span data-stu-id="7ced5-181">c.</span></span> <span data-ttu-id="7ced5-182">Klicka på **nya identitetsleverantör** och välj metadata XML-filen som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-182">Click **New Identity Provider** and select the metadata XML file that you have downloaded from the Azure portal.</span></span> <span data-ttu-id="7ced5-183">Genom att importera metadata filöverföringar systemet automatiskt krävs Signaturcertifikat och certifikat för kryptering.</span><span class="sxs-lookup"><span data-stu-id="7ced5-183">By importing the metadata, the system automatically uploads the required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    <span data-ttu-id="7ced5-185">d.</span><span class="sxs-lookup"><span data-stu-id="7ced5-185">d.</span></span> <span data-ttu-id="7ced5-186">Att inkludera den **Assertion konsument-tjänstens URL** i SAML-begäran Välj **inkludera Assertion konsumenten tjänsten URL**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-186">To include the **Assertion Consumer Service URL** into the SAML request, select **Include Assertion Consumer Service URL**.</span></span>
   
    <span data-ttu-id="7ced5-187">e.</span><span class="sxs-lookup"><span data-stu-id="7ced5-187">e.</span></span> <span data-ttu-id="7ced5-188">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-188">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="7ced5-189">f.</span><span class="sxs-lookup"><span data-stu-id="7ced5-189">f.</span></span> <span data-ttu-id="7ced5-190">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7ced5-190">Save your changes.</span></span>
   
    <span data-ttu-id="7ced5-191">g.</span><span class="sxs-lookup"><span data-stu-id="7ced5-191">g.</span></span> <span data-ttu-id="7ced5-192">Klicka på den **Mina System** fliken.</span><span class="sxs-lookup"><span data-stu-id="7ced5-192">Click the **My System** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    <span data-ttu-id="7ced5-194">h.</span><span class="sxs-lookup"><span data-stu-id="7ced5-194">h.</span></span> <span data-ttu-id="7ced5-195">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från Azure portal till den **Azure AD URL: en inloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="7ced5-195">Paste **SAML Single Sign-On Service URL**, which you have copied from the Azure portal it into the **Azure AD Sign On URL** textbox.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    <span data-ttu-id="7ced5-197">Jag.</span><span class="sxs-lookup"><span data-stu-id="7ced5-197">i.</span></span> <span data-ttu-id="7ced5-198">Ange om medarbetaren kan välja mellan att logga in med användar-ID och lösenord eller enkel inloggning genom att välja **manuell identitet providern markeringen**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-198">Specify whether the employee can manually choose between logging on with user ID and password or SSO by selecting **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="7ced5-199">j.</span><span class="sxs-lookup"><span data-stu-id="7ced5-199">j.</span></span> <span data-ttu-id="7ced5-200">I den **SSO URL** avsnitt, ange den URL som ska användas av anställda att logga in på systemet.</span><span class="sxs-lookup"><span data-stu-id="7ced5-200">In the **SSO URL** section, specify the URL that should be used by the employee to logon to the system.</span></span> 
    <span data-ttu-id="7ced5-201">I den URL skickas till medarbetare listrutan, kan du välja mellan följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="7ced5-201">In the URL Sent to Employee dropdown list, you can choose between the following options:</span></span>
   
    <span data-ttu-id="7ced5-202">**URL: en icke-SSO**</span><span class="sxs-lookup"><span data-stu-id="7ced5-202">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="7ced5-203">Systemet skickar bara normala URL-Adressen till anställde.</span><span class="sxs-lookup"><span data-stu-id="7ced5-203">The system sends only the normal system URL to the employee.</span></span> <span data-ttu-id="7ced5-204">Medarbetaren kan inte logga in med enkel inloggning, och måste använda lösenordet eller certifikatet i stället.</span><span class="sxs-lookup"><span data-stu-id="7ced5-204">The employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="7ced5-205">**URL FÖR ENKEL INLOGGNING**</span><span class="sxs-lookup"><span data-stu-id="7ced5-205">**SSO URL**</span></span> 
   
    <span data-ttu-id="7ced5-206">SSO URL skickas till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="7ced5-206">The system sends only the SSO URL to the employee.</span></span> <span data-ttu-id="7ced5-207">Medarbetaren kan logga in med enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ced5-207">The employee can log on using SSO.</span></span> <span data-ttu-id="7ced5-208">Autentiseringsbegäran dirigeras via IdP.</span><span class="sxs-lookup"><span data-stu-id="7ced5-208">Authentication request is redirected through the IdP.</span></span>
   
    <span data-ttu-id="7ced5-209">**Automatiskt val**</span><span class="sxs-lookup"><span data-stu-id="7ced5-209">**Automatic Selection**</span></span>
   
    <span data-ttu-id="7ced5-210">Om enkel inloggning inte är aktiv, skickar systemets normala URL: en till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="7ced5-210">If SSO is not active, the system sends the normal system URL to the employee.</span></span> <span data-ttu-id="7ced5-211">Om enkel inloggning är aktiv, kontrollerar systemet om medarbetaren har ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ced5-211">If SSO is active, the system checks whether the employee has a password.</span></span> <span data-ttu-id="7ced5-212">Om det finns ett lösenord skickas både SSO Webbadressen och icke-SSO till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="7ced5-212">If a password is available, both SSO URL and Non-SSO URL are sent to the employee.</span></span> <span data-ttu-id="7ced5-213">Om medarbetaren har inget lösenord, skickas endast URL för enkel inloggning till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="7ced5-213">However, if the employee has no password, only the SSO URL is sent to the employee.</span></span>
   
    <span data-ttu-id="7ced5-214">k.</span><span class="sxs-lookup"><span data-stu-id="7ced5-214">k.</span></span> <span data-ttu-id="7ced5-215">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7ced5-215">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="7ced5-216">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="7ced5-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="7ced5-217">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="7ced5-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="7ced5-218">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7ced5-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="7ced5-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ced5-219">Create an Azure AD test user</span></span>

<span data-ttu-id="7ced5-220">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ced5-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="7ced5-222">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="7ced5-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="7ced5-223">I Azure-portalen i den vänstra rutan klickar du på den **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-223">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Azure Active Directory-knappen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="7ced5-225">Om du vill visa en lista över användare, gå till **användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-225">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![”Användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="7ced5-227">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i den **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ced5-227">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![Knappen Lägg till](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="7ced5-229">I den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ced5-229">In the **User** dialog box, perform the following steps:</span></span>

    ![Dialogrutan användare](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    <span data-ttu-id="7ced5-231">a.</span><span class="sxs-lookup"><span data-stu-id="7ced5-231">a.</span></span> <span data-ttu-id="7ced5-232">I den **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-232">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7ced5-233">b.</span><span class="sxs-lookup"><span data-stu-id="7ced5-233">b.</span></span> <span data-ttu-id="7ced5-234">I den **användarnamn** Skriv användarens Britta Simon e-postadress.</span><span class="sxs-lookup"><span data-stu-id="7ced5-234">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="7ced5-235">c.</span><span class="sxs-lookup"><span data-stu-id="7ced5-235">c.</span></span> <span data-ttu-id="7ced5-236">Välj den **visa lösenordet** kryssrutan och sedan skriva ned det värde som visas i den **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="7ced5-236">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="7ced5-237">d.</span><span class="sxs-lookup"><span data-stu-id="7ced5-237">d.</span></span> <span data-ttu-id="7ced5-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-238">Click **Create**.</span></span>
 
### <a name="create-an-sap-business-bydesign-test-user"></a><span data-ttu-id="7ced5-239">Skapa en SAP Business ByDesign testanvändare</span><span class="sxs-lookup"><span data-stu-id="7ced5-239">Create an SAP Business ByDesign test user</span></span>

<span data-ttu-id="7ced5-240">I det här avsnittet skapar du en användare som kallas Britta Simon i SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="7ced5-240">In this section, you create a user called Britta Simon in SAP Business ByDesign.</span></span> <span data-ttu-id="7ced5-241">Se tillsammans med [SAP Business ByDesign Client supportteamet](https://www.sap.com/products/cloud-analytics.support.html) att lägga till användare i SAP Business ByDesign-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-241">Please work with [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) to add the users in the SAP Business ByDesign platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="7ced5-242">Kontrollera att NameID värdet ska vara identiskt med fältet för användarnamn i SAP Business ByDesign-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-242">Please make sure that NameID value should match with the username field in the SAP Business ByDesign platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="7ced5-243">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="7ced5-243">Assign the Azure AD test user</span></span>

<span data-ttu-id="7ced5-244">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="7ced5-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAP Business ByDesign.</span></span>

![Tilldela rollen][200] 

<span data-ttu-id="7ced5-246">**Om du vill tilldela SAP Business ByDesign Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7ced5-246">**To assign Britta Simon to SAP Business ByDesign, perform the following steps:**</span></span>

1. <span data-ttu-id="7ced5-247">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7ced5-249">Välj i listan med program **SAP Business ByDesign**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-249">In the applications list, select **SAP Business ByDesign**.</span></span>

    ![Länken SAP Business ByDesign i listan med program](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. <span data-ttu-id="7ced5-251">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7ced5-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Länken ”användare och grupper”][202]

4. <span data-ttu-id="7ced5-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ced5-253">Click **Add** button.</span></span> <span data-ttu-id="7ced5-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ced5-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Fönstret Lägg till tilldelning][203]

5. <span data-ttu-id="7ced5-256">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="7ced5-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="7ced5-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ced5-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7ced5-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ced5-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="7ced5-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ced5-259">Test single sign-on</span></span>

<span data-ttu-id="7ced5-260">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7ced5-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="7ced5-261">När du klickar på panelen SAP Business ByDesign på åtkomstpanelen du bör få automatiskt loggat in på ditt SAP Business ByDesign program.</span><span class="sxs-lookup"><span data-stu-id="7ced5-261">When you click the SAP Business ByDesign tile in the Access Panel, you should get automatically signed-on to your SAP Business ByDesign application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ced5-262">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7ced5-262">Additional resources</span></span>

* [<span data-ttu-id="7ced5-263">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ced5-263">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7ced5-264">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7ced5-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

