---
title: "Självstudier: Azure Active Directory-integrering med SAP Business ByDesign | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP Business ByDesign."
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
ms.openlocfilehash: c14714fd27f8d7fc555f25c7be83fad2b0d7f333
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a><span data-ttu-id="3f9f6-103">Självstudier: Azure Active Directory-integration med SAP Business ByDesign</span><span class="sxs-lookup"><span data-stu-id="3f9f6-103">Tutorial: Azure Active Directory integration with SAP Business ByDesign</span></span>

<span data-ttu-id="3f9f6-104">I kursen får du lära dig hur toointegrate SAP Business ByDesign med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3f9f6-104">In this tutorial, you learn how toointegrate SAP Business ByDesign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f9f6-105">Integrera SAP Business ByDesign med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-105">Integrating SAP Business ByDesign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3f9f6-106">Du kan styra i Azure AD som har åtkomst tooSAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-106">You can control in Azure AD who has access tooSAP Business ByDesign.</span></span>
- <span data-ttu-id="3f9f6-107">Du kan låta dina användare tooautomatically get inloggade tooSAP Business ByDesign (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-107">You can enable your users tooautomatically get signed-on tooSAP Business ByDesign (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="3f9f6-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="3f9f6-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f9f6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f9f6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3f9f6-110">Prerequisites</span></span>

<span data-ttu-id="3f9f6-111">tooconfigure Azure AD-integrering med SAP Business ByDesign måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-111">tooconfigure Azure AD integration with SAP Business ByDesign, you need hello following items:</span></span>

- <span data-ttu-id="3f9f6-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f9f6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f9f6-113">En SAP Business ByDesign enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3f9f6-113">An SAP Business ByDesign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f9f6-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f9f6-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f9f6-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f9f6-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f9f6-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f9f6-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3f9f6-118">Scenario description</span></span>
<span data-ttu-id="3f9f6-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f9f6-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f9f6-121">Att lägga till SAP Business ByDesign från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3f9f6-121">Adding SAP Business ByDesign from hello gallery</span></span>
2. <span data-ttu-id="3f9f6-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f9f6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-business-bydesign-from-hello-gallery"></a><span data-ttu-id="3f9f6-123">Att lägga till SAP Business ByDesign från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3f9f6-123">Adding SAP Business ByDesign from hello gallery</span></span>
<span data-ttu-id="3f9f6-124">tooconfigure hello integrering av SAP Business ByDesign i Azure AD, behöver du tooadd SAP Business ByDesign hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-124">tooconfigure hello integration of SAP Business ByDesign into Azure AD, you need tooadd SAP Business ByDesign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3f9f6-125">**tooadd SAP Business ByDesign från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-125">**tooadd SAP Business ByDesign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f9f6-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="3f9f6-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3f9f6-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="3f9f6-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="3f9f6-133">Skriv i sökrutan hello **SAP Business ByDesign**väljer **SAP Business ByDesign** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-133">In hello search box, type **SAP Business ByDesign**, select **SAP Business ByDesign** from result panel then click **Add** button tooadd hello application.</span></span>

    ![SAP Business ByDesign i hello resultatlistan](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="3f9f6-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f9f6-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="3f9f6-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP Business ByDesign baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-136">In this section, you configure and test Azure AD single sign-on with SAP Business ByDesign based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3f9f6-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SAP Business ByDesign är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP Business ByDesign is tooa user in Azure AD.</span></span> <span data-ttu-id="3f9f6-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SAP Business ByDesign toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-138">In other words, a link relationship between an Azure AD user and hello related user in SAP Business ByDesign needs toobe established.</span></span>

<span data-ttu-id="3f9f6-139">Tilldela hello värdet för hello i SAP Business ByDesign **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-139">In SAP Business ByDesign, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3f9f6-140">tooconfigure och testa Azure AD enkel inloggning med SAP Business ByDesign, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-140">tooconfigure and test Azure AD single sign-on with SAP Business ByDesign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3f9f6-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3f9f6-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3f9f6-143">**[Skapa en SAP Business ByDesign testanvändare](#create-an-sap-business-bydesign-test-user)**  -toohave en motsvarighet för Britta Simon i SAP Business ByDesign som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-143">**[Create an SAP Business ByDesign test user](#create-an-sap-business-bydesign-test-user)** - toohave a counterpart of Britta Simon in SAP Business ByDesign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3f9f6-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3f9f6-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="3f9f6-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f9f6-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="3f9f6-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP Business ByDesign application.</span></span>

<span data-ttu-id="3f9f6-148">**tooconfigure Azure AD enkel inloggning med SAP Business ByDesign utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-148">**tooconfigure Azure AD single sign-on with SAP Business ByDesign, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f9f6-149">I hello Azure-portalen på hello **SAP Business ByDesign** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-149">In hello Azure portal, on hello **SAP Business ByDesign** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="3f9f6-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. <span data-ttu-id="3f9f6-153">På hello **SAP Business ByDesign domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-153">On hello **SAP Business ByDesign Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och SAP Business ByDesign domän med enkel inloggning information](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    <span data-ttu-id="3f9f6-155">a.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-155">a.</span></span> <span data-ttu-id="3f9f6-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="3f9f6-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<servername>.sapbydesign.com`</span></span>

    <span data-ttu-id="3f9f6-157">b.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-157">b.</span></span> <span data-ttu-id="3f9f6-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="3f9f6-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<servername>.sapbydesign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f9f6-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-159">These values are not real.</span></span> <span data-ttu-id="3f9f6-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3f9f6-161">Kontakta [SAP Business ByDesign Client supportteamet](https://www.sap.com/products/cloud-analytics.support.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-161">Contact [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) tooget these values.</span></span>

4. <span data-ttu-id="3f9f6-162">På hello **användarattribut** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-162">On hello **User Attributes** section, perform hello following steps:</span></span>

    ![SAP Business ByDesign attributet avsnitt](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    <span data-ttu-id="3f9f6-164">a.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-164">a.</span></span> <span data-ttu-id="3f9f6-165">I **användar-ID** listan, Välj hello **ExtractMailPrefix()** funktion.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-165">In **User Identifier** list, select hello **ExtractMailPrefix()** function.</span></span>
    
    <span data-ttu-id="3f9f6-166">b.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-166">b.</span></span> <span data-ttu-id="3f9f6-167">Från hello **e** listan, Välj hello användarattribut som du vill använda toouse för din implementering.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-167">From hello **Mail** list, select hello user attribute you want toouse for your implementation.</span></span> <span data-ttu-id="3f9f6-168">Till exempel om du vill toouse hello EmployeeID som unikt identifierare och du har lagrat hello attributvärdet i hello ExtensionAttribute2, väljer du user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-168">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select user.extensionattribute2.</span></span>   

5. <span data-ttu-id="3f9f6-169">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. <span data-ttu-id="3f9f6-171">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-171">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="3f9f6-173">På hello **SAP Business ByDesign Configuration** klickar du på **konfigurera SAP Business ByDesign** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-173">On hello **SAP Business ByDesign Configuration** section, click **Configure SAP Business ByDesign** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="3f9f6-174">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![SAP Business ByDesign konfiguration](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. <span data-ttu-id="3f9f6-176">tooget SSO konfigurerats för ditt program utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-176">tooget SSO configured for your application, perform hello following steps:</span></span>
   
    <span data-ttu-id="3f9f6-177">a.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-177">a.</span></span> <span data-ttu-id="3f9f6-178">Inloggning tooyour SAP Business ByDesign portal med administratörsrättigheter.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-178">Sign on tooyour SAP Business ByDesign portal with administrator rights.</span></span>
   
    <span data-ttu-id="3f9f6-179">b.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-179">b.</span></span> <span data-ttu-id="3f9f6-180">Navigera för**program och vanliga användare hanteringsaktivitet** och klicka på hello **identitetsleverantör** fliken.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-180">Navigate too**Application and User Management Common Task** and click hello **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="3f9f6-181">c.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-181">c.</span></span> <span data-ttu-id="3f9f6-182">Klicka på **nya identitetsleverantör** och välj hello metadata XML-fil som du har hämtat från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-182">Click **New Identity Provider** and select hello metadata XML file that you have downloaded from hello Azure portal.</span></span> <span data-ttu-id="3f9f6-183">Genom att importera hello metadata filöverföringar hello system automatiskt hello krävs Signaturcertifikat och certifikat för kryptering.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-183">By importing hello metadata, hello system automatically uploads hello required signature certificate and encryption certificate.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    <span data-ttu-id="3f9f6-185">d.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-185">d.</span></span> <span data-ttu-id="3f9f6-186">tooinclude hello **Assertion konsument-tjänstens URL** till hello SAML-begäran, Välj **inkludera Assertion konsumenten tjänsten URL**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-186">tooinclude hello **Assertion Consumer Service URL** into hello SAML request, select **Include Assertion Consumer Service URL**.</span></span>
   
    <span data-ttu-id="3f9f6-187">e.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-187">e.</span></span> <span data-ttu-id="3f9f6-188">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-188">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="3f9f6-189">f.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-189">f.</span></span> <span data-ttu-id="3f9f6-190">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-190">Save your changes.</span></span>
   
    <span data-ttu-id="3f9f6-191">g.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-191">g.</span></span> <span data-ttu-id="3f9f6-192">Klicka på hello **Mina System** fliken.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-192">Click hello **My System** tab.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    <span data-ttu-id="3f9f6-194">h.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-194">h.</span></span> <span data-ttu-id="3f9f6-195">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen den i hello **Azure AD URL: en inloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-195">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal it into hello **Azure AD Sign On URL** textbox.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    <span data-ttu-id="3f9f6-197">Jag.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-197">i.</span></span> <span data-ttu-id="3f9f6-198">Ange om hello medarbetare manuellt kan välja mellan att logga in med användar-ID och lösenord eller enkel inloggning genom att välja **manuell identitet providern markeringen**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-198">Specify whether hello employee can manually choose between logging on with user ID and password or SSO by selecting **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="3f9f6-199">j.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-199">j.</span></span> <span data-ttu-id="3f9f6-200">I hello **SSO URL** avsnittet Ange hello-URL som ska användas av hello medarbetare toologon toohello system.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-200">In hello **SSO URL** section, specify hello URL that should be used by hello employee toologon toohello system.</span></span> 
    <span data-ttu-id="3f9f6-201">Du kan välja mellan följande alternativ för hello i hello URL skickas tooEmployee listrutan:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-201">In hello URL Sent tooEmployee dropdown list, you can choose between hello following options:</span></span>
   
    <span data-ttu-id="3f9f6-202">**URL: en icke-SSO**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-202">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="3f9f6-203">hello skickas endast hello normala URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-203">hello system sends only hello normal system URL toohello employee.</span></span> <span data-ttu-id="3f9f6-204">hello medarbetare kan inte logga in med enkel inloggning, och måste använda lösenordet eller certifikatet i stället.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-204">hello employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="3f9f6-205">**URL FÖR ENKEL INLOGGNING**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-205">**SSO URL**</span></span> 
   
    <span data-ttu-id="3f9f6-206">hello skickas endast hello SSO URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-206">hello system sends only hello SSO URL toohello employee.</span></span> <span data-ttu-id="3f9f6-207">hello medarbetare kan logga in med enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-207">hello employee can log on using SSO.</span></span> <span data-ttu-id="3f9f6-208">Autentiseringsbegäran dirigeras via hello IdP.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-208">Authentication request is redirected through hello IdP.</span></span>
   
    <span data-ttu-id="3f9f6-209">**Automatiskt val**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-209">**Automatic Selection**</span></span>
   
    <span data-ttu-id="3f9f6-210">Om enkel inloggning inte är aktiv, skickar hello system hello normala URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-210">If SSO is not active, hello system sends hello normal system URL toohello employee.</span></span> <span data-ttu-id="3f9f6-211">Om enkel inloggning är aktiv kontrolleras Hej om hello medarbetare har ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-211">If SSO is active, hello system checks whether hello employee has a password.</span></span> <span data-ttu-id="3f9f6-212">Om ett lösenord är tillgängligt skickas både SSO Webbadressen och icke-SSO toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-212">If a password is available, both SSO URL and Non-SSO URL are sent toohello employee.</span></span> <span data-ttu-id="3f9f6-213">Men om hello anställda har inget lösenord, skickas endast hello SSO URL toohello medarbetare.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-213">However, if hello employee has no password, only hello SSO URL is sent toohello employee.</span></span>
   
    <span data-ttu-id="3f9f6-214">k.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-214">k.</span></span> <span data-ttu-id="3f9f6-215">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-215">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="3f9f6-216">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="3f9f6-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3f9f6-217">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3f9f6-218">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f9f6-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="3f9f6-219">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f9f6-219">Create an Azure AD test user</span></span>

<span data-ttu-id="3f9f6-220">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="3f9f6-222">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f9f6-223">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-223">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="3f9f6-225">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-225">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="3f9f6-227">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-227">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="3f9f6-229">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3f9f6-229">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    <span data-ttu-id="3f9f6-231">a.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-231">a.</span></span> <span data-ttu-id="3f9f6-232">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-232">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f9f6-233">b.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-233">b.</span></span> <span data-ttu-id="3f9f6-234">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-234">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="3f9f6-235">c.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-235">c.</span></span> <span data-ttu-id="3f9f6-236">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-236">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="3f9f6-237">d.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-237">d.</span></span> <span data-ttu-id="3f9f6-238">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-238">Click **Create**.</span></span>
 
### <a name="create-an-sap-business-bydesign-test-user"></a><span data-ttu-id="3f9f6-239">Skapa en SAP Business ByDesign testanvändare</span><span class="sxs-lookup"><span data-stu-id="3f9f6-239">Create an SAP Business ByDesign test user</span></span>

<span data-ttu-id="3f9f6-240">I det här avsnittet skapar du en användare som kallas Britta Simon i SAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-240">In this section, you create a user called Britta Simon in SAP Business ByDesign.</span></span> <span data-ttu-id="3f9f6-241">Se tillsammans med [SAP Business ByDesign Client supportteamet](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello användare i hello SAP Business ByDesign plattform.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-241">Please work with [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello users in hello SAP Business ByDesign platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="3f9f6-242">Kontrollera att NameID värdet ska vara identiskt med hello användarnamn fält i hello SAP Business ByDesign plattform.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-242">Please make sure that NameID value should match with hello username field in hello SAP Business ByDesign platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="3f9f6-243">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3f9f6-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="3f9f6-244">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAP Business ByDesign.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP Business ByDesign.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="3f9f6-246">**tooassign Britta Simon tooSAP Business ByDesign utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3f9f6-246">**tooassign Britta Simon tooSAP Business ByDesign, perform hello following steps:**</span></span>

1. <span data-ttu-id="3f9f6-247">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3f9f6-249">Välj i listan med program hello **SAP Business ByDesign**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-249">In hello applications list, select **SAP Business ByDesign**.</span></span>

    ![hello SAP Business ByDesign länken i listan med program hello](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. <span data-ttu-id="3f9f6-251">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="3f9f6-253">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-253">Click **Add** button.</span></span> <span data-ttu-id="3f9f6-254">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="3f9f6-256">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3f9f6-257">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f9f6-258">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="3f9f6-259">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3f9f6-259">Test single sign-on</span></span>

<span data-ttu-id="3f9f6-260">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3f9f6-261">Du bör få automatiskt inloggade tooyour SAP Business ByDesign programmet när du klickar på hello SAP Business ByDesign panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3f9f6-261">When you click hello SAP Business ByDesign tile in hello Access Panel, you should get automatically signed-on tooyour SAP Business ByDesign application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3f9f6-262">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3f9f6-262">Additional resources</span></span>

* [<span data-ttu-id="3f9f6-263">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3f9f6-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f9f6-264">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3f9f6-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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

