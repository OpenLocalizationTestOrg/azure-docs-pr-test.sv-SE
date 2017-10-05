---
title: "Självstudier: Azure Active Directory-integrering med SAP-molnet för kund | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SAP moln för kunden."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: e4d945525a45704f34e1d9e742220928a516f341
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a><span data-ttu-id="3d62b-103">Självstudier: Azure Active Directory-integrering med SAP-molnet för kunden</span><span class="sxs-lookup"><span data-stu-id="3d62b-103">Tutorial: Azure Active Directory integration with SAP Cloud for Customer</span></span>

<span data-ttu-id="3d62b-104">I kursen får lära du att integrera SAP-molnet för kunden med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3d62b-104">In this tutorial, you learn how to integrate SAP Cloud for Customer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3d62b-105">Integrera SAP-molnet för kunden med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3d62b-105">Integrating SAP Cloud for Customer with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3d62b-106">Du kan styra i Azure AD som har åtkomst till SAP moln för kunden</span><span class="sxs-lookup"><span data-stu-id="3d62b-106">You can control in Azure AD who has access to SAP Cloud for Customer</span></span>
- <span data-ttu-id="3d62b-107">Du kan aktivera användarna att automatiskt hämta loggat in på molnet SAP för kund (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3d62b-107">You can enable your users to automatically get signed-on to SAP Cloud for Customer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3d62b-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3d62b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3d62b-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3d62b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d62b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3d62b-110">Prerequisites</span></span>

<span data-ttu-id="3d62b-111">För att konfigurera Azure AD-integrering med SAP-molnet för kunden, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3d62b-111">To configure Azure AD integration with SAP Cloud for Customer, you need the following items:</span></span>

- <span data-ttu-id="3d62b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3d62b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3d62b-113">Ett SAP-moln för kunden enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3d62b-113">A SAP Cloud for Customer single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3d62b-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3d62b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3d62b-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3d62b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3d62b-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3d62b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3d62b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3d62b-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3d62b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3d62b-118">Scenario description</span></span>
<span data-ttu-id="3d62b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3d62b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3d62b-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3d62b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3d62b-121">Lägger till SAP-molnet för kunden från galleriet</span><span class="sxs-lookup"><span data-stu-id="3d62b-121">Adding SAP Cloud for Customer from the gallery</span></span>
2. <span data-ttu-id="3d62b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d62b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a><span data-ttu-id="3d62b-123">Lägger till SAP-molnet för kunden från galleriet</span><span class="sxs-lookup"><span data-stu-id="3d62b-123">Adding SAP Cloud for Customer from the gallery</span></span>
<span data-ttu-id="3d62b-124">Om du vill konfigurera molnet för SAP-integrering för kund till Azure AD som du behöver lägga till SAP-molnet för kunden från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3d62b-124">To configure the integration of SAP Cloud for Customer into Azure AD, you need to add SAP Cloud for Customer from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3d62b-125">**Utför följande steg för att lägga till SAP-molnet för kunden från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="3d62b-125">**To add SAP Cloud for Customer from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3d62b-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3d62b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3d62b-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3d62b-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3d62b-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d62b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3d62b-133">I sökrutan skriver **SAP-molnet för kunden**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-133">In the search box, type **SAP Cloud for Customer**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. <span data-ttu-id="3d62b-135">Välj i resultatpanelen **SAP-molnet för kunden**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="3d62b-135">In the results panel, select **SAP Cloud for Customer**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3d62b-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d62b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3d62b-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP-molnet för kund baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3d62b-138">In this section, you configure and test Azure AD single sign-on with SAP Cloud for Customer based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3d62b-139">Azure AD måste du känna till motsvarande användaren i SAP-molnet för kund till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="3d62b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SAP Cloud for Customer is to a user in Azure AD.</span></span> <span data-ttu-id="3d62b-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SAP-molnet för kunden upprättas.</span><span class="sxs-lookup"><span data-stu-id="3d62b-140">In other words, a link relationship between an Azure AD user and the related user in SAP Cloud for Customer needs to be established.</span></span>

<span data-ttu-id="3d62b-141">I SAP moln för kunden, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-141">In SAP Cloud for Customer, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3d62b-142">Om du vill konfigurera och testa Azure AD enkel inloggning med SAP-molnet för kunden, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3d62b-142">To configure and test Azure AD single sign-on with SAP Cloud for Customer, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3d62b-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3d62b-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d62b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3d62b-145">**[Skapa ett SAP-moln för kunden testanvändare](#creating-a-sap-cloud-for-customer-test-user)**  – du har en motsvarighet för Britta Simon i SAP-molnet för kund som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3d62b-145">**[Creating a SAP Cloud for Customer test user](#creating-a-sap-cloud-for-customer-test-user)** - to have a counterpart of Britta Simon in SAP Cloud for Customer that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3d62b-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3d62b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3d62b-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3d62b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3d62b-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d62b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3d62b-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i din SAP-molnet för kund-program.</span><span class="sxs-lookup"><span data-stu-id="3d62b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SAP Cloud for Customer application.</span></span>

<span data-ttu-id="3d62b-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SAP-molnet för kunden:**</span><span class="sxs-lookup"><span data-stu-id="3d62b-150">**To configure Azure AD single sign-on with SAP Cloud for Customer, perform the following steps:**</span></span>

1. <span data-ttu-id="3d62b-151">I Azure-portalen på den **SAP-molnet för kunden** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-151">In the Azure portal, on the **SAP Cloud for Customer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3d62b-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3d62b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. <span data-ttu-id="3d62b-155">På den **SAP molnet kunden domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3d62b-155">On the **SAP Cloud for Customer Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    <span data-ttu-id="3d62b-157">a.</span><span class="sxs-lookup"><span data-stu-id="3d62b-157">a.</span></span> <span data-ttu-id="3d62b-158">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="3d62b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    <span data-ttu-id="3d62b-159">b.</span><span class="sxs-lookup"><span data-stu-id="3d62b-159">b.</span></span> <span data-ttu-id="3d62b-160">I den **identifierare** textruta Skriv en URL med följande mönster:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="3d62b-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3d62b-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3d62b-161">These values are not real.</span></span> <span data-ttu-id="3d62b-162">Uppdatera dessa värden med den faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="3d62b-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3d62b-163">Kontakta [SAP-molnet för kunden klienten supportteamet](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3d62b-163">Contact [SAP Cloud for Customer Client support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) to get these values.</span></span> 

4. <span data-ttu-id="3d62b-164">På den **användarattribut** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3d62b-164">On the **User Attributes** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    <span data-ttu-id="3d62b-166">a.</span><span class="sxs-lookup"><span data-stu-id="3d62b-166">a.</span></span> <span data-ttu-id="3d62b-167">I **användar-ID** väljer den **ExtractMailPrefix()** funktion.</span><span class="sxs-lookup"><span data-stu-id="3d62b-167">In **User Identifier** list, select the **ExtractMailPrefix()** function.</span></span>

    <span data-ttu-id="3d62b-168">b.</span><span class="sxs-lookup"><span data-stu-id="3d62b-168">b.</span></span> <span data-ttu-id="3d62b-169">Från den **e** väljer användarattribut som du vill använda för din implementering.</span><span class="sxs-lookup"><span data-stu-id="3d62b-169">From the **Mail** list, select the user attribute you want to use for your implementation.</span></span>
    <span data-ttu-id="3d62b-170">Till exempel om du vill använda EmployeeID som unikt identifierare och du har lagrat attributvärdet i ExtensionAttribute2, välj sedan user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="3d62b-170">For example, if you want to use the EmployeeID as unique user identifier and you have stored the attribute value in the ExtensionAttribute2, then select user.extensionattribute2.</span></span>  

5. <span data-ttu-id="3d62b-171">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="3d62b-171">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. <span data-ttu-id="3d62b-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-173">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="3d62b-175">På den **SAP-molnet för konfiguration av Customer** klickar du på **SAP konfigurera molnet för kunden** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3d62b-175">On the **SAP Cloud for Customer Configuration** section, click **Configure SAP Cloud for Customer** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3d62b-176">Kopiera den **SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3d62b-176">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. <span data-ttu-id="3d62b-178">Utför följande steg för att få SSO konfigurerats:</span><span class="sxs-lookup"><span data-stu-id="3d62b-178">To get SSO configured, perform the following steps:</span></span>
   
    <span data-ttu-id="3d62b-179">a.</span><span class="sxs-lookup"><span data-stu-id="3d62b-179">a.</span></span> <span data-ttu-id="3d62b-180">Logga in på molnet SAP för kundens portal med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="3d62b-180">Login into SAP Cloud for Customer portal with administrator rights.</span></span>
   
    <span data-ttu-id="3d62b-181">b.</span><span class="sxs-lookup"><span data-stu-id="3d62b-181">b.</span></span> <span data-ttu-id="3d62b-182">Navigera till den **program och vanliga användare hanteringsaktivitet** och klicka på den **identitetsleverantör** fliken.</span><span class="sxs-lookup"><span data-stu-id="3d62b-182">Navigate to the **Application and User Management Common Task** and click the **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="3d62b-183">c.</span><span class="sxs-lookup"><span data-stu-id="3d62b-183">c.</span></span> <span data-ttu-id="3d62b-184">Klicka på **nya identitetsleverantör** och välj metadata XML-fil som du har hämtat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-184">Click **New Identity Provider** and select the metadata XML file you have downloaded from the Azure portal.</span></span> <span data-ttu-id="3d62b-185">Genom att importera metadata filöverföringar systemet automatiskt krävs Signaturcertifikat och certifikat för kryptering.</span><span class="sxs-lookup"><span data-stu-id="3d62b-185">By importing the metadata, the system automatically uploads the required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    <span data-ttu-id="3d62b-187">d.</span><span class="sxs-lookup"><span data-stu-id="3d62b-187">d.</span></span> <span data-ttu-id="3d62b-188">Azure Active Directory kräver element Assertion konsumenten tjänst-URL i SAML-begäran, så du väljer den **inkludera Assertion konsumenten tjänsten URL** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="3d62b-188">Azure Active Directory requires the element Assertion Consumer Service URL in the SAML request, so select the **Include Assertion Consumer Service URL** checkbox.</span></span>
   
    <span data-ttu-id="3d62b-189">e.</span><span class="sxs-lookup"><span data-stu-id="3d62b-189">e.</span></span> <span data-ttu-id="3d62b-190">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-190">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="3d62b-191">f.</span><span class="sxs-lookup"><span data-stu-id="3d62b-191">f.</span></span> <span data-ttu-id="3d62b-192">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3d62b-192">Save your changes.</span></span>
   
    <span data-ttu-id="3d62b-193">g.</span><span class="sxs-lookup"><span data-stu-id="3d62b-193">g.</span></span> <span data-ttu-id="3d62b-194">Klicka på den **Mina System** fliken.</span><span class="sxs-lookup"><span data-stu-id="3d62b-194">Click the **My System** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    <span data-ttu-id="3d62b-196">h.</span><span class="sxs-lookup"><span data-stu-id="3d62b-196">h.</span></span> <span data-ttu-id="3d62b-197">I **Azure AD URL: en inloggning** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-197">In **Azure AD Sign On URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    <span data-ttu-id="3d62b-199">Jag.</span><span class="sxs-lookup"><span data-stu-id="3d62b-199">i.</span></span> <span data-ttu-id="3d62b-200">Ange om medarbetaren kan välja mellan att logga in med användar-ID och lösenord eller enkel inloggning genom att välja den **manuell identitet providern markeringen**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-200">Specify whether the employee can manually choose between logging on with user ID and password or SSO by selecting the **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="3d62b-201">j.</span><span class="sxs-lookup"><span data-stu-id="3d62b-201">j.</span></span> <span data-ttu-id="3d62b-202">I den **SSO URL** avsnitt, ange den URL som ska användas av dina anställda för att logga in på systemet.</span><span class="sxs-lookup"><span data-stu-id="3d62b-202">In the **SSO URL** section, specify the URL that should be used by your employees to sign on to the system.</span></span> 
    <span data-ttu-id="3d62b-203">I den **URL skickas till medarbetare** lista kan du välja mellan följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="3d62b-203">In the **URL Sent to Employee** list, you can choose between the following options:</span></span>
   
    <span data-ttu-id="3d62b-204">**URL: en icke-SSO**</span><span class="sxs-lookup"><span data-stu-id="3d62b-204">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="3d62b-205">Systemet skickar bara normala URL-Adressen till anställde.</span><span class="sxs-lookup"><span data-stu-id="3d62b-205">The system sends only the normal system URL to the employee.</span></span> <span data-ttu-id="3d62b-206">Medarbetaren kan inte logga in med enkel inloggning, och måste använda lösenordet eller certifikatet i stället.</span><span class="sxs-lookup"><span data-stu-id="3d62b-206">The employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="3d62b-207">**URL FÖR ENKEL INLOGGNING**</span><span class="sxs-lookup"><span data-stu-id="3d62b-207">**SSO URL**</span></span> 
   
    <span data-ttu-id="3d62b-208">SSO URL skickas till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="3d62b-208">The system sends only the SSO URL to the employee.</span></span> <span data-ttu-id="3d62b-209">Medarbetaren kan logga in med enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3d62b-209">The employee can log on using SSO.</span></span> <span data-ttu-id="3d62b-210">Autentiseringsbegäran dirigeras via IdP.</span><span class="sxs-lookup"><span data-stu-id="3d62b-210">Authentication request is redirected through the IdP.</span></span>
   
    <span data-ttu-id="3d62b-211">**Automatiskt val**</span><span class="sxs-lookup"><span data-stu-id="3d62b-211">**Automatic Selection**</span></span>
   
    <span data-ttu-id="3d62b-212">Om enkel inloggning inte är aktiv, skickar systemets normala URL: en till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="3d62b-212">If SSO is not active, the system sends the normal system URL to the employee.</span></span> <span data-ttu-id="3d62b-213">Om enkel inloggning är aktiv, kontrollerar systemet om medarbetaren har ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="3d62b-213">If SSO is active, the system checks whether the employee has a password.</span></span> <span data-ttu-id="3d62b-214">Om det finns ett lösenord skickas både SSO Webbadressen och icke-SSO till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="3d62b-214">If a password is available, both SSO URL and Non-SSO URL are sent to the employee.</span></span> <span data-ttu-id="3d62b-215">Om medarbetaren har inget lösenord, skickas endast URL för enkel inloggning till medarbetaren.</span><span class="sxs-lookup"><span data-stu-id="3d62b-215">However, if the employee has no password, only the SSO URL is sent to the employee.</span></span>
   
    <span data-ttu-id="3d62b-216">k.</span><span class="sxs-lookup"><span data-stu-id="3d62b-216">k.</span></span> <span data-ttu-id="3d62b-217">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3d62b-217">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="3d62b-218">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="3d62b-218">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3d62b-219">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="3d62b-219">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3d62b-220">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3d62b-220">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3d62b-221">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3d62b-221">Creating an Azure AD test user</span></span>
<span data-ttu-id="3d62b-222">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3d62b-222">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3d62b-224">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="3d62b-224">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3d62b-225">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3d62b-225">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3d62b-227">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-227">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3d62b-229">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d62b-229">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3d62b-231">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="3d62b-231">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3d62b-233">a.</span><span class="sxs-lookup"><span data-stu-id="3d62b-233">a.</span></span> <span data-ttu-id="3d62b-234">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-234">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3d62b-235">b.</span><span class="sxs-lookup"><span data-stu-id="3d62b-235">b.</span></span> <span data-ttu-id="3d62b-236">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3d62b-236">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3d62b-237">c.</span><span class="sxs-lookup"><span data-stu-id="3d62b-237">c.</span></span> <span data-ttu-id="3d62b-238">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-238">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3d62b-239">d.</span><span class="sxs-lookup"><span data-stu-id="3d62b-239">d.</span></span> <span data-ttu-id="3d62b-240">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-240">Click **Create**.</span></span>
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a><span data-ttu-id="3d62b-241">Skapa ett SAP-moln för kunden testanvändare</span><span class="sxs-lookup"><span data-stu-id="3d62b-241">Creating a SAP Cloud for Customer test user</span></span>

<span data-ttu-id="3d62b-242">I det här avsnittet kan du skapa en användare som kallas Britta Simon i SAP-molnet för kunden.</span><span class="sxs-lookup"><span data-stu-id="3d62b-242">In this section, you create a user called Britta Simon in SAP Cloud for Customer.</span></span> <span data-ttu-id="3d62b-243">Se tillsammans med [SAP-molnet för kundtjänst](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) att lägga till användare i SAP-molnet för kund-plattformen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-243">Please work with [SAP Cloud for Customer support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) to add the users in the SAP Cloud for Customer platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="3d62b-244">Kontrollera att NameID värdet ska vara identiskt med fältet för användarnamn i SAP-molnet för kund-plattformen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-244">Please make sure that NameID value should match with the username field in the SAP Cloud for Customer platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3d62b-245">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="3d62b-245">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3d62b-246">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till SAP-molnet för kunden.</span><span class="sxs-lookup"><span data-stu-id="3d62b-246">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SAP Cloud for Customer.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3d62b-248">**Om du vill tilldela moln SAP Britta Simon för kunden, utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3d62b-248">**To assign Britta Simon to SAP Cloud for Customer, perform the following steps:**</span></span>

1. <span data-ttu-id="3d62b-249">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-249">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3d62b-251">Välj i listan med program **SAP-molnet för kunden**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-251">In the applications list, select **SAP Cloud for Customer**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. <span data-ttu-id="3d62b-253">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3d62b-253">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3d62b-255">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3d62b-255">Click **Add** button.</span></span> <span data-ttu-id="3d62b-256">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d62b-256">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3d62b-258">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="3d62b-258">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3d62b-259">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d62b-259">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3d62b-260">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3d62b-260">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3d62b-261">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3d62b-261">Testing single sign-on</span></span>

<span data-ttu-id="3d62b-262">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3d62b-262">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3d62b-263">När du klickar på SAP-molnet för kund-panelen på åtkomstpanelen du ska hämta automatiskt loggat in på din SAP-molnet för kund-program.</span><span class="sxs-lookup"><span data-stu-id="3d62b-263">When you click the SAP Cloud for Customer tile in the Access Panel, you should get automatically signed-on to your SAP Cloud for Customer application.</span></span>
<span data-ttu-id="3d62b-264">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3d62b-264">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3d62b-265">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3d62b-265">Additional resources</span></span>

* [<span data-ttu-id="3d62b-266">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3d62b-266">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3d62b-267">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3d62b-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png

