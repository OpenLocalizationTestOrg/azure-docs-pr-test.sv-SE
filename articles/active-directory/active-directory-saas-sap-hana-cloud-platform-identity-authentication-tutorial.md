---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA Cloud Platform identitetsverifiering | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SAP HANA Cloud Platform identitetsverifiering."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 7799bf03cc6705f805a48f329a265a3d84bed55f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a><span data-ttu-id="35988-103">Självstudier: Azure Active Directory-integrering med SAP HANA Cloud Platform identitetsverifiering</span><span class="sxs-lookup"><span data-stu-id="35988-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform Identity Authentication</span></span>

<span data-ttu-id="35988-104">I kursen får lära du att integrera identitetsverifiering för SAP HANA moln plattform med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="35988-104">In this tutorial, you learn how to integrate SAP HANA Cloud Platform Identity Authentication with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="35988-105">Identitetsverifiering för SAP HANA Cloud Platform används som en proxy IdP åtkomst SAP-program med Azure AD som huvudsakliga IdP.</span><span class="sxs-lookup"><span data-stu-id="35988-105">SAP HANA Cloud Platform Identity Authentication is used as a proxy IdP to access SAP applications using Azure AD as the main IdP.</span></span>

<span data-ttu-id="35988-106">Integrera identitetsverifiering för SAP HANA moln plattform med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="35988-106">Integrating SAP HANA Cloud Platform Identity Authentication with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="35988-107">Du kan styra i Azure AD som har åtkomst till SAP-program</span><span class="sxs-lookup"><span data-stu-id="35988-107">You can control in Azure AD who has access to SAP application</span></span>
- <span data-ttu-id="35988-108">Du kan aktivera användarna att automatiskt hämta loggat in på SAP-program enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="35988-108">You can enable your users to automatically get signed-on to SAP applications single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="35988-109">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="35988-109">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="35988-110">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="35988-110">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="35988-111">Krav</span><span class="sxs-lookup"><span data-stu-id="35988-111">Prerequisites</span></span>

<span data-ttu-id="35988-112">För att konfigurera Azure AD-integrering med SAP HANA Cloud Platform identitetsverifiering, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="35988-112">To configure Azure AD integration with SAP HANA Cloud Platform Identity Authentication, you need the following items:</span></span>

- <span data-ttu-id="35988-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="35988-113">An Azure AD subscription</span></span>
- <span data-ttu-id="35988-114">En **identitetsverifiering för SAP HANA Cloud Platform** SSO aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="35988-114">A **SAP HANA Cloud Platform Identity Authentication** SSO enabled subscription</span></span>


>[!NOTE] 
><span data-ttu-id="35988-115">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="35988-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="35988-116">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="35988-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="35988-117">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="35988-117">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="35988-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="35988-118">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="35988-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="35988-119">Scenario description</span></span>
<span data-ttu-id="35988-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="35988-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="35988-121">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="35988-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="35988-122">Att lägga till SAP HANA Cloud Platform identitetsverifiering från galleriet</span><span class="sxs-lookup"><span data-stu-id="35988-122">Adding SAP HANA Cloud Platform Identity Authentication from the gallery</span></span>
2. <span data-ttu-id="35988-123">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="35988-123">Configuring and testing Azure AD SSO</span></span>

<span data-ttu-id="35988-124">Innan du dyker till teknisk information, är det viktigt att förstå de begrepp som du ska titta på.</span><span class="sxs-lookup"><span data-stu-id="35988-124">Before diving into the technical details, it is vital to understand the concepts you're going to look at.</span></span> <span data-ttu-id="35988-125">Identitetsverifiering för SAP HANA Cloud Platform och Azure Active Directory federation gör att du kan implementera enkel inloggning över program eller tjänster som skyddas av AAD (som en IdP) med SAP-program och tjänster som skyddas av SAP HANA Cloud Platform identitet Autentisering.</span><span class="sxs-lookup"><span data-stu-id="35988-125">The SAP HANA Cloud Platform Identity Authentication and Azure Active Directory federation enables you to implement SSO across applications or services protected by AAD (as an IdP) with SAP applications and services protected by SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="35988-126">För närvarande fungerar identitetsverifiering för SAP HANA Cloud Platform som en Proxy-identitetsprovider till SAP-program.</span><span class="sxs-lookup"><span data-stu-id="35988-126">Currently, SAP HANA Cloud Platform Identity Authentication acts as a Proxy Identity Provider to SAP-applications.</span></span> <span data-ttu-id="35988-127">Azure Active Directory fungerar i sin tur som inledande identitetsleverantören i den här installationen.</span><span class="sxs-lookup"><span data-stu-id="35988-127">Azure Active Directory in turn acts as the leading Identity Provider in this setup.</span></span> 

<span data-ttu-id="35988-128">Följande diagram illustrerar detta:</span><span class="sxs-lookup"><span data-stu-id="35988-128">The following diagram illustrates this:</span></span>    

![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

<span data-ttu-id="35988-130">Med den här installationen konfigureras din SAP HANA Cloud Platform identitetsverifiering klient som ett betrodda program i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="35988-130">With this setup, your SAP HANA Cloud Platform Identity Authentication tenant will be configured as a trusted application in Azure Active Directory.</span></span> 

<span data-ttu-id="35988-131">Alla SAP-program och tjänster som du vill skydda via det här sättet konfigureras senare i hanteringskonsolen för SAP HANA Cloud Platform identitetsverifiering!</span><span class="sxs-lookup"><span data-stu-id="35988-131">All SAP applications and services you want to protect through this way are subsequently configured in the SAP HANA Cloud Platform Identity Authentication management console!</span></span>

<span data-ttu-id="35988-132">Det innebär att auktorisering för att bevilja åtkomst till SAP-program och tjänster måste äga rum i identitetsverifiering för SAP HANA moln plattform för en sådan konfiguration (i stället konfigurerar auktorisering i Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="35988-132">This means that authorization for granting access to SAP applications and services needs to take place in SAP HANA Cloud Platform Identity Authentication for such a setup (as opposed to configuring authorization in Azure Active Directory).</span></span>

<span data-ttu-id="35988-133">Genom att konfigurera identitetsverifiering för SAP HANA moln plattform som ett program via Azure Active Directory Marketplace, behöver du inte ta hand om att konfigurera nödvändiga enskilda anspråk / SAML intyg och omvandlingar som behövs för att producera ett giltigt Autentiseringstoken för SAP-program.</span><span class="sxs-lookup"><span data-stu-id="35988-133">By configuring SAP HANA Cloud Platform Identity Authentication as an application through the Azure Active Directory Marketplace, you don't need to take care of configuring needed individual claims / SAML assertions and transformations needed to produce a valid authentication token for SAP applications.</span></span>

>[!NOTE] 
><span data-ttu-id="35988-134">För närvarande har Web SSO testats av båda parter endast.</span><span class="sxs-lookup"><span data-stu-id="35988-134">Currently Web SSO has been tested by both parties, only.</span></span> <span data-ttu-id="35988-135">Flöden som krävs för App-API och API-API-kommunikation ska fungera men har inte testats, ännu.</span><span class="sxs-lookup"><span data-stu-id="35988-135">Flows needed for App-to-API or API-to-API communication should work but have not been tested, yet.</span></span> <span data-ttu-id="35988-136">De kommer att testas som en del av efterföljande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="35988-136">They will be tested as part of subsequent activities.</span></span>
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-the-gallery"></a><span data-ttu-id="35988-137">Lägg till SAP HANA Cloud Platform identitetsverifiering från galleriet</span><span class="sxs-lookup"><span data-stu-id="35988-137">Add SAP HANA Cloud Platform Identity Authentication from the gallery</span></span>
<span data-ttu-id="35988-138">Du måste lägga till SAP HANA Cloud Platform identitetsverifiering från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av identitetsverifiering för SAP HANA Cloud-plattformen i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="35988-138">To configure the integration of SAP HANA Cloud Platform Identity Authentication into Azure AD, you need to add SAP HANA Cloud Platform Identity Authentication from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="35988-139">**Utför följande steg för att lägga till SAP HANA Cloud Platform identitetsverifiering från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="35988-139">**To add SAP HANA Cloud Platform Identity Authentication from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="35988-140">I den [ **Azure-hanteringsportalen**](https://portal.azure.com), klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="35988-140">In the [**Azure Management portal**](https://portal.azure.com), on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="35988-142">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="35988-142">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="35988-143">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="35988-143">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="35988-145">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35988-145">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="35988-147">I sökrutan skriver **identitetsverifiering för SAP HANA Cloud Platform**.</span><span class="sxs-lookup"><span data-stu-id="35988-147">In the search box, type **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. <span data-ttu-id="35988-149">Välj i resultatpanelen **identitetsverifiering för SAP HANA Cloud Platform**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="35988-149">In the results panel, select **SAP HANA Cloud Platform Identity Authentication**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="35988-151">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="35988-151">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="35988-152">I det här avsnittet kan du konfigurera och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="35988-152">In this section, you configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="35988-153">För SSO ska fungera måste Azure AD du känna till motsvarande användaren i identitetsverifiering för SAP HANA Cloud Platform till en användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="35988-153">For SSO to work, Azure AD needs to know what the counterpart user in SAP HANA Cloud Platform Identity Authentication is to a user in Azure AD.</span></span> <span data-ttu-id="35988-154">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SAP HANA Cloud Platform identitetsverifiering upprättas.</span><span class="sxs-lookup"><span data-stu-id="35988-154">In other words, a link relationship between an Azure AD user and the related user in SAP HANA Cloud Platform Identity Authentication needs to be established.</span></span>

<span data-ttu-id="35988-155">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="35988-155">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="35988-156">Om du vill konfigurera och testa Azure AD SSO med SAP HANA Cloud Platform identitetsverifiering, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="35988-156">To configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="35988-157">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="35988-157">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="35988-158">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="35988-158">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="35988-159">**[Skapa en testanvändare för SAP HANA Cloud Platform identitetsverifiering](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  – du har en motsvarighet för Britta Simon i SAP HANA Cloud Platform identitetsverifiering som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="35988-159">**[Creating a SAP HANA Cloud Platform Identity Authentication test user](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)** - to have a counterpart of Britta Simon in SAP HANA Cloud Platform Identity Authentication that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="35988-160">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="35988-160">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="35988-161">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="35988-161">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="35988-162">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="35988-162">Configuring Azure AD SSO</span></span>

<span data-ttu-id="35988-163">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i din app för identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="35988-163">In this section, you enable Azure AD SSO in the Azure Management portal and configure single sign-on in your SAP HANA Cloud Platform Identity Authentication application.</span></span>

<span data-ttu-id="35988-164">Identitetsverifiering för SAP HANA Cloud Platform program förväntar SAML-intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="35988-164">SAP HANA Cloud Platform Identity Authentication application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="35988-165">Du kan hantera värden för attributen från den ”**användarattribut**” avsnitt på sidan för integrering av programmet.</span><span class="sxs-lookup"><span data-stu-id="35988-165">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="35988-166">Följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="35988-166">The following screenshot shows an example for this.</span></span>

![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

<span data-ttu-id="35988-168">**Utför följande steg för att konfigurera Azure AD SSO med identitetsverifiering för SAP HANA Cloud Platform:**</span><span class="sxs-lookup"><span data-stu-id="35988-168">**To configure Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, perform the following steps:**</span></span>

1. <span data-ttu-id="35988-169">I Azure-hanteringsportalen på den **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="35988-169">In the Azure Management portal, on the **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="35988-171">På den **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="35988-171">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning][5]

3. <span data-ttu-id="35988-173">I den **användarattribut** avsnitt på den **enkel inloggning** dialogrutan om tillämpningsprogrammet SAP förväntar sig ett attribut till exempel ”förnamn”.</span><span class="sxs-lookup"><span data-stu-id="35988-173">In the **User Attributes** section on the **Single sign-on** dialog, if your SAP application expects an attribute for example "firstName".</span></span> <span data-ttu-id="35988-174">Lägg till attributet ”förnamn” i dialogrutan för SAML-token attribut.</span><span class="sxs-lookup"><span data-stu-id="35988-174">On the SAML token attributes dialog, add the "firstName" attribute.</span></span>
 1. <span data-ttu-id="35988-175">Klicka på **Lägg till attributet** att öppna den **lägga till attributet** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35988-175">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>
 
    ![Konfigurera enkel inloggning][6]

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. <span data-ttu-id="35988-178">I den **attributnamn** textruta ange attributet name ”förnamn”.</span><span class="sxs-lookup"><span data-stu-id="35988-178">In the **Attribute Name** textbox, type the attribute name "firstName".</span></span>
 3. <span data-ttu-id="35988-179">Från den **attributvärdet** väljer attributvärdet ”user.givenname”.</span><span class="sxs-lookup"><span data-stu-id="35988-179">From the **Attribute Value** list, select the attribute value "user.givenname".</span></span>
 4. <span data-ttu-id="35988-180">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="35988-180">Click **Ok**.</span></span>

4. <span data-ttu-id="35988-181">På den **SAP HANA Cloud Platform identitet autentisering domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="35988-181">On the **SAP HANA Cloud Platform Identity Authentication Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. <span data-ttu-id="35988-183">I den **logga URL** textruta anger tecknet på URL: en för SAP-program.</span><span class="sxs-lookup"><span data-stu-id="35988-183">In the **Sign On URL** textbox, type the sign on URL for the SAP application.</span></span>
 2. <span data-ttu-id="35988-184">I den **identifierare** textruta Skriv det värde som följer mönstret:`<entity-id>.accounts.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="35988-184">In the **Identifier** textbox, type the value following pattern: `<entity-id>.accounts.ondemand.com`</span></span> 
    * <span data-ttu-id="35988-185">Om du inte vet det här värdet, följ dokumentationen för SAP HANA Cloud Platform identitetsverifiering på [klientkonfiguration SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span><span class="sxs-lookup"><span data-stu-id="35988-185">If you don't know this value, please follow the SAP HANA Cloud Platform Identity Authentication documentation on [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span></span>

5. <span data-ttu-id="35988-186">På den **konfiguration för SAP HANA Cloud Platform identitet verifiering** klickar du på **konfigurera SAP HANA Cloud Platform identitetsverifiering** att öppna **konfigurerainloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35988-186">On the **SAP HANA Cloud Platform Identity Authentication Configuration** section, click **Configure SAP HANA Cloud Platform Identity Authentication** to open **Configure sign-on** dialog.</span></span> <span data-ttu-id="35988-187">Klicka på **SAML XML-Metadata** och spara filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="35988-187">Then, click on **SAML XML Metadata** and save the file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. <span data-ttu-id="35988-190">Gå till SAP HANA Cloud Platform identitet autentisering administrationskonsolen för att få SSO konfigurerats för ditt program.</span><span class="sxs-lookup"><span data-stu-id="35988-190">To get SSO configured for your application, go to SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span> <span data-ttu-id="35988-191">URL som har följande mönster:`https://<tenant-id>.accounts.ondemand.com/admin`</span><span class="sxs-lookup"><span data-stu-id="35988-191">The URL has the following pattern: `https://<tenant-id>.accounts.ondemand.com/admin`</span></span>
 * <span data-ttu-id="35988-192">Följ dokumentationen på identitetsverifiering för SAP HANA Cloud Platform till [konfigurera Microsoft Azure AD som företagets identitetsleverantör vid SAP HANA Cloud Platform identitetsverifiering](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span><span class="sxs-lookup"><span data-stu-id="35988-192">Then, follow the documentation on SAP HANA Cloud Platform Identity Authentication to [Configure Microsoft Azure AD as Corporate Identity Provider at SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span></span> 

7. <span data-ttu-id="35988-193">Klicka på Azure-hanteringsportalen **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="35988-193">In the Azure Management portal, click **Save** button.</span></span>
8. <span data-ttu-id="35988-194">Fortsätt följande steg om du vill lägga till och aktivera enkel inloggning för ett annat SAP-program.</span><span class="sxs-lookup"><span data-stu-id="35988-194">Continue the following steps only if you want to add and enable SSO for another SAP application.</span></span> <span data-ttu-id="35988-195">Upprepa steg under avsnittet ”lägger till SAP HANA-plattformen identitetsautentisering i molnet från galleriet” att lägga till en annan instans av identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="35988-195">Repeat steps under the section “Adding SAP HANA Cloud Platform Identity Authentication from the gallery” to add another instance of SAP HANA Cloud Platform Identity Authentication.</span></span>
9. <span data-ttu-id="35988-196">I Azure-hanteringsportalen på den **identitetsverifiering för SAP HANA Cloud Platform** integreringssidan för programmet, klickar du på **inloggning länkade**.</span><span class="sxs-lookup"><span data-stu-id="35988-196">In the Azure Management portal, on the **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Linked Sign-on**.</span></span>

    ![Konfigurera länkad inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. <span data-ttu-id="35988-198">Spara konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="35988-198">Then, save the configuration.</span></span>

>[!NOTE] 
><span data-ttu-id="35988-199">Det nya programmet använder SSO-konfigurationen för det tidigare SAP-programmet.</span><span class="sxs-lookup"><span data-stu-id="35988-199">The new application will leverage the SSO configuration for the previous SAP application.</span></span> <span data-ttu-id="35988-200">Kontrollera att du använder samma företagets identitetsleverantörer i SAP HANA Cloud Platform identitet autentisering-administrationskonsolen.</span><span class="sxs-lookup"><span data-stu-id="35988-200">Please make sure you use the same Corporate Identity Providers in the SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span>
>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="35988-201">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="35988-201">Create an Azure AD test user</span></span>
<span data-ttu-id="35988-202">Syftet med det här avsnittet är att skapa en testanvändare i den nya portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="35988-202">The objective of this section is to create a test user in the new portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="35988-204">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="35988-204">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="35988-205">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="35988-205">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="35988-207">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="35988-207">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="35988-209">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35988-209">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="35988-211">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="35988-211">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. <span data-ttu-id="35988-213">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="35988-213">In the **Name** textbox, type **BrittaSimon**.</span></span>
  2. <span data-ttu-id="35988-214">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="35988-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>
  3. <span data-ttu-id="35988-215">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="35988-215">Select **Show Password** and write down the value of the **Password**.</span></span>
  4. <span data-ttu-id="35988-216">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="35988-216">Click **Create**.</span></span> 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a><span data-ttu-id="35988-217">Skapa en SAP HANA Cloud Platform identitetsverifiering testanvändare</span><span class="sxs-lookup"><span data-stu-id="35988-217">Create a SAP HANA Cloud Platform Identity Authentication test user</span></span>

<span data-ttu-id="35988-218">Du behöver inte skapa en användare på identitetsverifiering för SAP HANA moln plattform.</span><span class="sxs-lookup"><span data-stu-id="35988-218">You don't need to create an user on SAP HANA Cloud Platform Identity Authentication.</span></span> <span data-ttu-id="35988-219">Användare i arkivet för Azure AD-användare kan använda funktionen för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="35988-219">Users who are in the Azure AD user store can use the SSO functionality.</span></span>

<span data-ttu-id="35988-220">SAP HANA Cloud Platform identitetsverifiering har stöd för identitetsfederation-alternativet.</span><span class="sxs-lookup"><span data-stu-id="35988-220">SAP HANA Cloud Platform Identity Authentication supports the Identity Federation option.</span></span> <span data-ttu-id="35988-221">Det här alternativet innebär att du kan kontrollera om de användare som autentiseras av företagets identitetsleverantören finns i den store för SAP HANA Cloud Platform identitet användarautentisering.</span><span class="sxs-lookup"><span data-stu-id="35988-221">This option allows the application to check if the users authenticated by the corporate identity provider exist in the user store of SAP HANA Cloud Platform Identity Authentication.</span></span> 

<span data-ttu-id="35988-222">I standardinställningen, är identitetsfederation-alternativet inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="35988-222">In the default setting, the Identity Federation option is disabled.</span></span> <span data-ttu-id="35988-223">Om identitetsfederation är aktiverad kan endast användare som importeras i identitetsverifiering för SAP HANA Cloud Platform åtkomst till programmet.</span><span class="sxs-lookup"><span data-stu-id="35988-223">If Identity Federation is enabled, only the users that are imported in SAP HANA Cloud Platform Identity Authentication are able to access the application.</span></span> 

<span data-ttu-id="35988-224">Mer information om hur du aktiverar eller inaktiverar identitetsfederation med identitetsverifiering för SAP HANA Cloud Platform finns i Aktivera identitetsfederation med identitetsverifiering för SAP HANA Cloud Platform i [konfigurera identitetsfederation med den Store SAP HANA Cloud Platform identitet användarautentiseringen. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span><span class="sxs-lookup"><span data-stu-id="35988-224">For more information about how to enable or disable Identity Federation with SAP HANA Cloud Platform Identity Authentication, see Enable Identity Federation with SAP HANA Cloud Platform Identity Authentication in [Configure Identity Federation with the User Store of SAP HANA Cloud Platform Identity Authentication.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="35988-225">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="35988-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="35988-226">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till SAP HANA Cloud Platform identitetsverifiering.</span><span class="sxs-lookup"><span data-stu-id="35988-226">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to SAP HANA Cloud Platform Identity Authentication.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="35988-228">**Om du vill tilldela identitetsverifiering för SAP HANA Cloud Platform Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="35988-228">**To assign Britta Simon to SAP HANA Cloud Platform Identity Authentication, perform the following steps:**</span></span>

1. <span data-ttu-id="35988-229">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="35988-229">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="35988-231">Välj i listan med program **identitetsverifiering för SAP HANA Cloud Platform**.</span><span class="sxs-lookup"><span data-stu-id="35988-231">In the applications list, select **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. <span data-ttu-id="35988-233">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="35988-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="35988-235">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="35988-235">Click **Add** button.</span></span> <span data-ttu-id="35988-236">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35988-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="35988-238">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="35988-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="35988-239">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35988-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="35988-240">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="35988-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="test-single-sign-on"></a><span data-ttu-id="35988-241">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="35988-241">Test single sign-on</span></span>

<span data-ttu-id="35988-242">I det här avsnittet kan du testa din Azure AD SSO-konfiguration med hjälp av åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="35988-242">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="35988-243">När du klickar på panelen identitetsverifiering för SAP HANA Cloud Platform på åtkomstpanelen du ska hämta automatiskt loggat in på ditt program för SAP HANA Cloud Platform identitetsverifiering.</span><span class="sxs-lookup"><span data-stu-id="35988-243">When you click the SAP HANA Cloud Platform Identity Authentication tile in the Access Panel, you should get automatically signed-on to your SAP HANA Cloud Platform Identity Authentication application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="35988-244">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="35988-244">Additional resources</span></span>

* [<span data-ttu-id="35988-245">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="35988-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="35988-246">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="35988-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png