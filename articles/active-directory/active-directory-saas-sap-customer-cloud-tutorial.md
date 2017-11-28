---
title: "Självstudier: Azure Active Directory-integrering med SAP-molnet för kund | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP moln för kunden."
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
ms.openlocfilehash: 0525ea81122458ab3ac24a5bdb0b5f628405dd05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a><span data-ttu-id="7ebe6-103">Självstudier: Azure Active Directory-integrering med SAP-molnet för kunden</span><span class="sxs-lookup"><span data-stu-id="7ebe6-103">Tutorial: Azure Active Directory integration with SAP Cloud for Customer</span></span>

<span data-ttu-id="7ebe6-104">I kursen får du lära dig hur toointegrate SAP-molnet för kunden med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="7ebe6-104">In this tutorial, you learn how toointegrate SAP Cloud for Customer with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7ebe6-105">Integrera SAP-molnet för kunden med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-105">Integrating SAP Cloud for Customer with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7ebe6-106">Du kan styra i Azure AD som har åtkomst tooSAP molnet för kunden</span><span class="sxs-lookup"><span data-stu-id="7ebe6-106">You can control in Azure AD who has access tooSAP Cloud for Customer</span></span>
- <span data-ttu-id="7ebe6-107">Du kan aktivera din användare tooautomatically get inloggade tooSAP molnet för kund (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="7ebe6-107">You can enable your users tooautomatically get signed-on tooSAP Cloud for Customer (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7ebe6-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7ebe6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7ebe6-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7ebe6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ebe6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7ebe6-110">Prerequisites</span></span>

<span data-ttu-id="7ebe6-111">tooconfigure Azure AD-integrering med SAP-molnet för kunden, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-111">tooconfigure Azure AD integration with SAP Cloud for Customer, you need hello following items:</span></span>

- <span data-ttu-id="7ebe6-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ebe6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7ebe6-113">Ett SAP-moln för kunden enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="7ebe6-113">A SAP Cloud for Customer single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7ebe6-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7ebe6-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7ebe6-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7ebe6-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad här: [utvärderingsversion erbjudande](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7ebe6-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7ebe6-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="7ebe6-118">Scenario description</span></span>
<span data-ttu-id="7ebe6-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7ebe6-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7ebe6-121">Lägger till SAP-molnet för kunden från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7ebe6-121">Adding SAP Cloud for Customer from hello gallery</span></span>
2. <span data-ttu-id="7ebe6-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ebe6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-cloud-for-customer-from-hello-gallery"></a><span data-ttu-id="7ebe6-123">Lägger till SAP-molnet för kunden från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="7ebe6-123">Adding SAP Cloud for Customer from hello gallery</span></span>
<span data-ttu-id="7ebe6-124">tooconfigure hello integrering av SAP-molnet för kund till Azure AD, behöver du tooadd SAP-molnet för kunden hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-124">tooconfigure hello integration of SAP Cloud for Customer into Azure AD, you need tooadd SAP Cloud for Customer from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7ebe6-125">**tooadd SAP-molnet för kunden från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-125">**tooadd SAP Cloud for Customer from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ebe6-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7ebe6-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7ebe6-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="7ebe6-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="7ebe6-133">Skriv i sökrutan hello **SAP-molnet för kunden**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-133">In hello search box, type **SAP Cloud for Customer**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. <span data-ttu-id="7ebe6-135">Markera hello resultat på panelen **SAP-molnet för kunden**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-135">In hello results panel, select **SAP Cloud for Customer**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7ebe6-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ebe6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7ebe6-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP-molnet för kund baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-138">In this section, you configure and test Azure AD single sign-on with SAP Cloud for Customer based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7ebe6-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SAP-molnet för kunden är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP Cloud for Customer is tooa user in Azure AD.</span></span> <span data-ttu-id="7ebe6-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i SAP-molnet för kunden toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-140">In other words, a link relationship between an Azure AD user and hello related user in SAP Cloud for Customer needs toobe established.</span></span>

<span data-ttu-id="7ebe6-141">I SAP moln för kunden, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-141">In SAP Cloud for Customer, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7ebe6-142">tooconfigure och testa Azure AD enkel inloggning med SAP-molnet för kunden, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-142">tooconfigure and test Azure AD single sign-on with SAP Cloud for Customer, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7ebe6-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7ebe6-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7ebe6-145">**[Skapa ett SAP-moln för kunden testanvändare](#creating-a-sap-cloud-for-customer-test-user)**  -toohave en motsvarighet för Britta Simon i SAP-molnet för kund som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-145">**[Creating a SAP Cloud for Customer test user](#creating-a-sap-cloud-for-customer-test-user)** - toohave a counterpart of Britta Simon in SAP Cloud for Customer that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7ebe6-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7ebe6-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7ebe6-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ebe6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7ebe6-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i din SAP-molnet för kund-program.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP Cloud for Customer application.</span></span>

<span data-ttu-id="7ebe6-150">**tooconfigure Azure AD enkel inloggning med SAP-molnet för kunden, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-150">**tooconfigure Azure AD single sign-on with SAP Cloud for Customer, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ebe6-151">I hello Azure-portalen på hello **SAP-molnet för kunden** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-151">In hello Azure portal, on hello **SAP Cloud for Customer** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="7ebe6-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. <span data-ttu-id="7ebe6-155">På hello **SAP molnet kunden domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-155">On hello **SAP Cloud for Customer Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    <span data-ttu-id="7ebe6-157">a.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-157">a.</span></span> <span data-ttu-id="7ebe6-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="7ebe6-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    <span data-ttu-id="7ebe6-159">b.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-159">b.</span></span> <span data-ttu-id="7ebe6-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<server name>.crm.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="7ebe6-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<server name>.crm.ondemand.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7ebe6-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-161">These values are not real.</span></span> <span data-ttu-id="7ebe6-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7ebe6-163">Kontakta [SAP-molnet för kunden klienten supportteamet](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-163">Contact [SAP Cloud for Customer Client support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooget these values.</span></span> 

4. <span data-ttu-id="7ebe6-164">På hello **användarattribut** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-164">On hello **User Attributes** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    <span data-ttu-id="7ebe6-166">a.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-166">a.</span></span> <span data-ttu-id="7ebe6-167">I **användar-ID** listan, Välj hello **ExtractMailPrefix()** funktion.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-167">In **User Identifier** list, select hello **ExtractMailPrefix()** function.</span></span>

    <span data-ttu-id="7ebe6-168">b.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-168">b.</span></span> <span data-ttu-id="7ebe6-169">Från hello **e** listan, Välj hello användarattribut som du vill använda toouse för din implementering.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-169">From hello **Mail** list, select hello user attribute you want toouse for your implementation.</span></span>
    <span data-ttu-id="7ebe6-170">Till exempel om du vill toouse hello EmployeeID som unikt identifierare och du har lagrat hello attributvärdet i hello ExtensionAttribute2, väljer du user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-170">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select user.extensionattribute2.</span></span>  

5. <span data-ttu-id="7ebe6-171">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-171">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. <span data-ttu-id="7ebe6-173">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-173">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="7ebe6-175">På hello **SAP-molnet för konfiguration av Customer** klickar du på **SAP konfigurera molnet för kunden** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-175">On hello **SAP Cloud for Customer Configuration** section, click **Configure SAP Cloud for Customer** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7ebe6-176">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-176">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. <span data-ttu-id="7ebe6-178">tooget SSO konfigurerats, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-178">tooget SSO configured, perform hello following steps:</span></span>
   
    <span data-ttu-id="7ebe6-179">a.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-179">a.</span></span> <span data-ttu-id="7ebe6-180">Logga in på molnet SAP för kundens portal med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-180">Login into SAP Cloud for Customer portal with administrator rights.</span></span>
   
    <span data-ttu-id="7ebe6-181">b.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-181">b.</span></span> <span data-ttu-id="7ebe6-182">Navigera toohello **program och vanliga användare hanteringsaktivitet** och klicka på hello **identitetsleverantör** fliken.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-182">Navigate toohello **Application and User Management Common Task** and click hello **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="7ebe6-183">c.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-183">c.</span></span> <span data-ttu-id="7ebe6-184">Klicka på **nya identitetsleverantör** och välj hello metadata XML-fil som du har hämtat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-184">Click **New Identity Provider** and select hello metadata XML file you have downloaded from hello Azure portal.</span></span> <span data-ttu-id="7ebe6-185">Genom att importera hello metadata filöverföringar hello system automatiskt hello krävs Signaturcertifikat och certifikat för kryptering.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-185">By importing hello metadata, hello system automatically uploads hello required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    <span data-ttu-id="7ebe6-187">d.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-187">d.</span></span> <span data-ttu-id="7ebe6-188">Azure Active Directory kräver hello element Assertion konsumenten tjänst-URL i hello SAML-begäran, så Välj hello **inkludera Assertion konsumenten tjänsten URL** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-188">Azure Active Directory requires hello element Assertion Consumer Service URL in hello SAML request, so select hello **Include Assertion Consumer Service URL** checkbox.</span></span>
   
    <span data-ttu-id="7ebe6-189">e.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-189">e.</span></span> <span data-ttu-id="7ebe6-190">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-190">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="7ebe6-191">f.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-191">f.</span></span> <span data-ttu-id="7ebe6-192">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-192">Save your changes.</span></span>
   
    <span data-ttu-id="7ebe6-193">g.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-193">g.</span></span> <span data-ttu-id="7ebe6-194">Klicka på hello **Mina System** fliken.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-194">Click hello **My System** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    <span data-ttu-id="7ebe6-196">h.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-196">h.</span></span> <span data-ttu-id="7ebe6-197">I **Azure AD URL: en inloggning** textruta klistra in **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-197">In **Azure AD Sign On URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    <span data-ttu-id="7ebe6-199">Jag.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-199">i.</span></span> <span data-ttu-id="7ebe6-200">Ange om hello medarbetare manuellt kan välja mellan att logga in med användar-ID och lösenord eller enkel inloggning genom att välja hello **manuell identitet providern markeringen**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-200">Specify whether hello employee can manually choose between logging on with user ID and password or SSO by selecting hello **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="7ebe6-201">j.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-201">j.</span></span> <span data-ttu-id="7ebe6-202">I hello **SSO URL** avsnittet Ange hello-URL som ska användas av dina anställda toosign på toohello system.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-202">In hello **SSO URL** section, specify hello URL that should be used by your employees toosign on toohello system.</span></span> 
    <span data-ttu-id="7ebe6-203">I hello **URL skickas tooEmployee** listan som du kan välja mellan följande alternativ för hello:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-203">In hello **URL Sent tooEmployee** list, you can choose between hello following options:</span></span>
   
    <span data-ttu-id="7ebe6-204">**URL: en icke-SSO**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-204">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="7ebe6-205">hello skickas endast hello normala URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-205">hello system sends only hello normal system URL toohello employee.</span></span> <span data-ttu-id="7ebe6-206">hello medarbetare kan inte logga in med enkel inloggning, och måste använda lösenordet eller certifikatet i stället.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-206">hello employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="7ebe6-207">**URL FÖR ENKEL INLOGGNING**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-207">**SSO URL**</span></span> 
   
    <span data-ttu-id="7ebe6-208">hello skickas endast hello SSO URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-208">hello system sends only hello SSO URL toohello employee.</span></span> <span data-ttu-id="7ebe6-209">hello medarbetare kan logga in med enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-209">hello employee can log on using SSO.</span></span> <span data-ttu-id="7ebe6-210">Autentiseringsbegäran dirigeras via hello IdP.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-210">Authentication request is redirected through hello IdP.</span></span>
   
    <span data-ttu-id="7ebe6-211">**Automatiskt val**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-211">**Automatic Selection**</span></span>
   
    <span data-ttu-id="7ebe6-212">Om enkel inloggning inte är aktiv, skickar hello system hello normala URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-212">If SSO is not active, hello system sends hello normal system URL toohello employee.</span></span> <span data-ttu-id="7ebe6-213">Om enkel inloggning är aktiv kontrolleras Hej om hello medarbetare har ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-213">If SSO is active, hello system checks whether hello employee has a password.</span></span> <span data-ttu-id="7ebe6-214">Om ett lösenord är tillgängligt skickas både SSO Webbadressen och icke-SSO toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-214">If a password is available, both SSO URL and Non-SSO URL are sent toohello employee.</span></span> <span data-ttu-id="7ebe6-215">Men om hello anställda har inget lösenord, skickas endast hello SSO URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-215">However, if hello employee has no password, only hello SSO URL is sent toohello employee.</span></span>
   
    <span data-ttu-id="7ebe6-216">k.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-216">k.</span></span> <span data-ttu-id="7ebe6-217">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-217">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="7ebe6-218">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="7ebe6-218">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7ebe6-219">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-219">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7ebe6-220">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7ebe6-220">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7ebe6-221">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ebe6-221">Creating an Azure AD test user</span></span>
<span data-ttu-id="7ebe6-222">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-222">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="7ebe6-224">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-224">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ebe6-225">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-225">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7ebe6-227">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-227">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7ebe6-229">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-229">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7ebe6-231">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="7ebe6-231">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7ebe6-233">a.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-233">a.</span></span> <span data-ttu-id="7ebe6-234">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-234">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7ebe6-235">b.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-235">b.</span></span> <span data-ttu-id="7ebe6-236">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-236">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7ebe6-237">c.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-237">c.</span></span> <span data-ttu-id="7ebe6-238">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-238">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7ebe6-239">d.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-239">d.</span></span> <span data-ttu-id="7ebe6-240">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-240">Click **Create**.</span></span>
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a><span data-ttu-id="7ebe6-241">Skapa ett SAP-moln för kunden testanvändare</span><span class="sxs-lookup"><span data-stu-id="7ebe6-241">Creating a SAP Cloud for Customer test user</span></span>

<span data-ttu-id="7ebe6-242">I det här avsnittet kan du skapa en användare som kallas Britta Simon i SAP-molnet för kunden.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-242">In this section, you create a user called Britta Simon in SAP Cloud for Customer.</span></span> <span data-ttu-id="7ebe6-243">Se tillsammans med [SAP-molnet för kundtjänst](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello användare i hello SAP-molnet för kund-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-243">Please work with [SAP Cloud for Customer support team](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) tooadd hello users in hello SAP Cloud for Customer platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="7ebe6-244">Kontrollera att NameID värdet ska vara identiskt med hello användarnamn fält i hello SAP-molnet för kund-plattformen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-244">Please make sure that NameID value should match with hello username field in hello SAP Cloud for Customer platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7ebe6-245">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="7ebe6-245">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7ebe6-246">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAP molnet för kunden.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-246">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP Cloud for Customer.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="7ebe6-248">**tooassign Britta Simon tooSAP molnet för kunden, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="7ebe6-248">**tooassign Britta Simon tooSAP Cloud for Customer, perform hello following steps:**</span></span>

1. <span data-ttu-id="7ebe6-249">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-249">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="7ebe6-251">Välj i listan med program hello **SAP-molnet för kunden**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-251">In hello applications list, select **SAP Cloud for Customer**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. <span data-ttu-id="7ebe6-253">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-253">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="7ebe6-255">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-255">Click **Add** button.</span></span> <span data-ttu-id="7ebe6-256">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-256">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="7ebe6-258">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-258">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7ebe6-259">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-259">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7ebe6-260">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-260">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7ebe6-261">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="7ebe6-261">Testing single sign-on</span></span>

<span data-ttu-id="7ebe6-262">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-262">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7ebe6-263">När du klickar på hello SAP-molnet för kund-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour SAP-molnet för kund-program.</span><span class="sxs-lookup"><span data-stu-id="7ebe6-263">When you click hello SAP Cloud for Customer tile in hello Access Panel, you should get automatically signed-on tooyour SAP Cloud for Customer application.</span></span>
<span data-ttu-id="7ebe6-264">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7ebe6-264">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ebe6-265">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7ebe6-265">Additional resources</span></span>

* [<span data-ttu-id="7ebe6-266">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7ebe6-266">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7ebe6-267">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7ebe6-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



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

