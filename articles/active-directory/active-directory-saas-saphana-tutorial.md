---
title: "Självstudier: Azure Active Directory-integrering med SAP HANA | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP HANA."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 5ff6bfde0b1d7ab14025a4bc45199f9f826affd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a><span data-ttu-id="2f208-103">Självstudier: Azure Active Directory-integrering med SAP HANA</span><span class="sxs-lookup"><span data-stu-id="2f208-103">Tutorial: Azure Active Directory integration with SAP HANA</span></span>

<span data-ttu-id="2f208-104">I kursen får du lära dig hur toointegrate SAP HANA med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2f208-104">In this tutorial, you learn how toointegrate SAP HANA with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2f208-105">Integrera SAP HANA med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2f208-105">Integrating SAP HANA with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2f208-106">Du kan styra i Azure AD som har åtkomst tooSAP HANA</span><span class="sxs-lookup"><span data-stu-id="2f208-106">You can control in Azure AD who has access tooSAP HANA</span></span>
- <span data-ttu-id="2f208-107">Du kan aktivera din användare tooautomatically get inloggade tooSAP HANA (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2f208-107">You can enable your users tooautomatically get signed-on tooSAP HANA (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2f208-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2f208-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2f208-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2f208-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f208-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2f208-110">Prerequisites</span></span>

<span data-ttu-id="2f208-111">tooconfigure Azure AD-integrering med SAP HANA måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2f208-111">tooconfigure Azure AD integration with SAP HANA, you need hello following items:</span></span>

- <span data-ttu-id="2f208-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2f208-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2f208-113">En SAP HANA enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="2f208-113">A SAP HANA single sign-on enabled subscription</span></span>
- <span data-ttu-id="2f208-114">Körs HANA instansen antingen på alla offentliga IaaS lokalt, virtuella Azure-datorer eller stora SAP-instanser i Azure</span><span class="sxs-lookup"><span data-stu-id="2f208-114">A running HANA Instance either on any public IaaS, on-premises, Azure VMs or SAP Large Instances in Azure</span></span>
- <span data-ttu-id="2f208-115">Hej XSA Administration webbgränssnitt samt HANA Studio installerat på hello HANA instans.</span><span class="sxs-lookup"><span data-stu-id="2f208-115">hello XSA Administration Web Interface as well as HANA Studio installed on hello HANA instance.</span></span>

> [!NOTE]
> <span data-ttu-id="2f208-116">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2f208-116">tootest hello steps in this tutorial, we do not recommend using a production environment of SAP HANA.</span></span> <span data-ttu-id="2f208-117">Testa hello integration först i utvecklings- eller mellanlagring av programmet hello-miljön och Använd hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2f208-117">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="2f208-118">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2f208-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2f208-119">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2f208-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2f208-120">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2f208-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2f208-121">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="2f208-121">Scenario description</span></span>
<span data-ttu-id="2f208-122">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2f208-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2f208-123">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2f208-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2f208-124">Att lägga till SAP HANA från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2f208-124">Adding SAP HANA from hello gallery</span></span>
2. <span data-ttu-id="2f208-125">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2f208-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-hana-from-hello-gallery"></a><span data-ttu-id="2f208-126">Att lägga till SAP HANA från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2f208-126">Adding SAP HANA from hello gallery</span></span>
<span data-ttu-id="2f208-127">tooconfigure hello integrering av SAP HANA i Azure AD, behöver du tooadd SAP HANA hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2f208-127">tooconfigure hello integration of SAP HANA into Azure AD, you need tooadd SAP HANA from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2f208-128">**tooadd SAP HANA från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2f208-128">**tooadd SAP HANA from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f208-129">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2f208-129">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="2f208-131">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="2f208-131">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2f208-132">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="2f208-132">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="2f208-134">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2f208-134">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="2f208-136">Skriv i sökrutan hello **SAP HANA**väljer **SAP HANA** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2f208-136">In hello search box, type **SAP HANA**, select **SAP HANA** from result panel then click **Add** button tooadd hello application.</span></span> 

    ![hello nytt program](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2f208-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2f208-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2f208-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP HANA baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2f208-139">In this section, you configure and test Azure AD single sign-on with SAP HANA based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2f208-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SAP HANA är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f208-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP HANA is tooa user in Azure AD.</span></span> <span data-ttu-id="2f208-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SAP HANA toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2f208-141">In other words, a link relationship between an Azure AD user and hello related user in SAP HANA needs toobe established.</span></span>

<span data-ttu-id="2f208-142">Tilldela hello värdet för hello i SAP HANA **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="2f208-142">In SAP HANA, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="2f208-143">tooconfigure och testa Azure AD enkel inloggning med SAP HANA, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2f208-143">tooconfigure and test Azure AD single sign-on with SAP HANA, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2f208-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2f208-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2f208-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f208-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2f208-146">**[Skapa en testanvändare för SAP HANA](#creating-a-sap-hana-test-user)**  -toohave en motsvarighet för Britta Simon i SAP HANA som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="2f208-146">**[Creating a SAP HANA test user](#creating-a-sap-hana-test-user)** - toohave a counterpart of Britta Simon in SAP HANA that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2f208-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2f208-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2f208-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2f208-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2f208-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2f208-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2f208-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2f208-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP HANA application.</span></span>

<span data-ttu-id="2f208-151">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SAP HANA:**</span><span class="sxs-lookup"><span data-stu-id="2f208-151">**tooconfigure Azure AD single sign-on with SAP HANA, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f208-152">I hello Azure-portalen på hello **SAP HANA** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="2f208-152">In hello Azure portal, on hello **SAP HANA** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="2f208-154">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2f208-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. <span data-ttu-id="2f208-156">På hello **SAP HANA domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2f208-156">On hello **SAP HANA Domain and URLs** section, perform hello following steps:</span></span>

    ![URL: er och domän med enkel inloggning information](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    <span data-ttu-id="2f208-158">a.</span><span class="sxs-lookup"><span data-stu-id="2f208-158">a.</span></span> <span data-ttu-id="2f208-159">I hello **identifierare** textruta typ som:`HA100`</span><span class="sxs-lookup"><span data-stu-id="2f208-159">In hello **Identifier** textbox, type as: `HA100`</span></span> 

    <span data-ttu-id="2f208-160">b.</span><span class="sxs-lookup"><span data-stu-id="2f208-160">b.</span></span> <span data-ttu-id="2f208-161">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`</span><span class="sxs-lookup"><span data-stu-id="2f208-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2f208-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="2f208-162">These values are not real.</span></span> <span data-ttu-id="2f208-163">Uppdatera dessa värden med hello faktiska identifierare och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="2f208-163">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="2f208-164">Kontakta [SAP HANA klienten supportteamet](https://cloudplatform.sap.com/contact.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="2f208-164">Contact [SAP HANA Client support team](https://cloudplatform.sap.com/contact.html) tooget these values.</span></span> 

4. <span data-ttu-id="2f208-165">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="2f208-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    ><span data-ttu-id="2f208-167">Om certifikatet inte är aktiv sedan aktivera den genom att klicka på hello ”kontrollera nya certifikat active” kryssrutan i hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2f208-167">If certificate is not active then make it active by clicking hello “Make new certificate active” checkbox in hello Azure AD.</span></span> 

5. <span data-ttu-id="2f208-168">SAP HANA program förväntar hello SAML intyg i ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="2f208-168">SAP HANA application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="2f208-169">hello följande skärmbild visar ett exempel för det här.</span><span class="sxs-lookup"><span data-stu-id="2f208-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="2f208-170">Vi har här mappat hello **användar-ID** med **ExtractMailPrefix()** funktion **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="2f208-170">Here we have mapped hello **User Identifier** with **ExtractMailPrefix()** function of **user.mail**.</span></span> <span data-ttu-id="2f208-171">Detta ger hello prefixvärdet av e-post för hello-användare som är hello unikt användar-ID.</span><span class="sxs-lookup"><span data-stu-id="2f208-171">This gives hello prefix value of email of hello user which is hello unique User ID.</span></span> <span data-ttu-id="2f208-172">Toohello SAP HANA-programmet skickas i varje lyckat svar.</span><span class="sxs-lookup"><span data-stu-id="2f208-172">This is sent toohello SAP HANA application in every successful response.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. <span data-ttu-id="2f208-174">I hello **användarattribut** avsnittet hello **enkel inloggning** dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="2f208-174">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="2f208-175">a.</span><span class="sxs-lookup"><span data-stu-id="2f208-175">a.</span></span> <span data-ttu-id="2f208-176">I hello **användar-ID** listrutan, Välj **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="2f208-176">In hello **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>
    
    <span data-ttu-id="2f208-177">b.</span><span class="sxs-lookup"><span data-stu-id="2f208-177">b.</span></span> <span data-ttu-id="2f208-178">I hello **e** listrutan, Välj **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="2f208-178">In hello **Mail** dropdown list, select **user.mail**.</span></span>

7. <span data-ttu-id="2f208-179">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="2f208-179">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="2f208-181">tooconfigure enkel inloggning på **SAP HANA** sida, inloggning tooyour **HANA XSA webbkonsolen** genom att bläddra toohello respektive HTTPS-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2f208-181">tooconfigure single sign-on on **SAP HANA** side, login tooyour **HANA XSA Web Console**  by browsing toohello respective HTTPS-endpoint.</span></span>

    > [!Note]
    > <span data-ttu-id="2f208-182">I standardkonfigurationen hello omdirigerar hello URL hello begäran tooa inloggningsskärm, vilket kräver hello autentiseringsuppgifterna för ett autentiserat SAP HANA-databas toocomplete hello inloggningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="2f208-182">In hello default configuration, hello URL redirects hello request tooa logon screen, which requires hello credentials of an authenticated SAP HANA database user toocomplete hello logon process.</span></span> <span data-ttu-id="2f208-183">hello-användare som loggar in måste ha hello privilegier krävs tooperform SAML administrationsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2f208-183">hello user who logs on must have hello privileges required tooperform SAML administration tasks.</span></span>

9. <span data-ttu-id="2f208-184">Hello XSA Web Interface, navigera för**SAML-identitetsprovider** och därifrån klickar du på hello **”+”** -knappen på hello längst ned på hello skärmen toodisplay hello lägga till information om identitet rutan och utföra Hej följande steg:</span><span class="sxs-lookup"><span data-stu-id="2f208-184">In hello XSA Web Interface, navigate too**SAML Identity Provider** and from there, click hello **“+”** -button on hello bottom of hello screen toodisplay hello Add Identity Provider Info pane and perform hello following steps:</span></span>

    ![Lägg till identitetsleverantören.](./media/active-directory-saas-saphana-tutorial/sap1.png)

    <span data-ttu-id="2f208-186">a.</span><span class="sxs-lookup"><span data-stu-id="2f208-186">a.</span></span> <span data-ttu-id="2f208-187">I hello **lägga till information om identitet** rutan klistra in hello innehållet i hello XML-Metadata, som du har hämtat från Azure-portalen i hello **Metadata** textruta.</span><span class="sxs-lookup"><span data-stu-id="2f208-187">In hello **Add Identity Provider Info** pane, paste hello contents of hello Metadata XML, which you have downloaded from Azure portal into hello **Metadata** textbox.</span></span>

    ![Lägg till inställningar för identitetsleverantören.](./media/active-directory-saas-saphana-tutorial/sap2.png)

    <span data-ttu-id="2f208-189">b.</span><span class="sxs-lookup"><span data-stu-id="2f208-189">b.</span></span> <span data-ttu-id="2f208-190">Om hello innehållet i XML-dokumentet för hello är giltiga hello vid parsning av process extraherar hello information som krävs för tooinsert till hello **ämne, enhets-ID och utfärdaren** fälten i hello allmänna Data Skrivbordsstorlek och hello URL fält i hello Skärmen målområdet, till exempel  **Base Webbadressen och SingleSignOn (*)**.</span><span class="sxs-lookup"><span data-stu-id="2f208-190">If hello contents of hello XML document are valid, hello parsing process extracts hello information required tooinsert into hello **Subject, Entity ID, and Issuer** fields in hello General Data screen area, and hello URL fields in hello Destination screen area, for example, **Base URL and SingleSignOn URL (*)**.</span></span>

    ![Lägg till inställningar för identitetsleverantören.](./media/active-directory-saas-saphana-tutorial/sap3.png)

    <span data-ttu-id="2f208-192">c.</span><span class="sxs-lookup"><span data-stu-id="2f208-192">c.</span></span> <span data-ttu-id="2f208-193">Hello namnrutan för hello allmänna Data Skrivbordsstorlek, ange ett namn för hello ny SAML SSO identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="2f208-193">In hello Name box of hello General Data screen area, enter a name for hello new SAML SSO identity provider.</span></span>

    > [!Note]
    > <span data-ttu-id="2f208-194">hello SAML IDP hello namn är obligatoriskt och måste vara unikt. den visas i hello listan med tillgängliga SAML-IDPs som visas, om du väljer SAML som hello autentiseringsmetod för SAP HANA XS program toouse, till exempel i hello Skrivbordsstorlek för autentisering av hello XS artefakt administrationsverktyget.</span><span class="sxs-lookup"><span data-stu-id="2f208-194">hello name of hello SAML IDP is mandatory and must be unique; it appears in hello list of available SAML IDPs that is displayed, if you select SAML as hello authentication method for SAP HANA XS applications toouse, for example, in hello Authentication screen area of hello XS Artifact Administration tool.</span></span>

10. <span data-ttu-id="2f208-195">Spara hello information om hello ny SAML-identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="2f208-195">Save hello details of hello new SAML identity provider.</span></span> <span data-ttu-id="2f208-196">Välj **spara** toosave hello information om hello SAML-identitetsprovider och Lägg till hello nya SAML IDP toohello listan över kända SAML IDPs.</span><span class="sxs-lookup"><span data-stu-id="2f208-196">Choose **Save** toosave hello details of hello SAML identity provider and add hello new SAML IDP toohello list of known SAML IDPs.</span></span>

    ![Knappen Spara](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. <span data-ttu-id="2f208-198">I HANA Studio i hello Systemegenskaper hello **Configuration** fliken, bara filtrera inställningar av **saml** och justera hello **assertion_timeout** från **10 sek** för**120 sek**.</span><span class="sxs-lookup"><span data-stu-id="2f208-198">In HANA Studio within hello system properties of hello **Configuration** tab, just filter settings by **saml** and adjust hello **assertion_timeout** from **10 sec** too**120 sec**.</span></span>

    ![assertion_timeout inställning](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> <span data-ttu-id="2f208-200">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="2f208-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2f208-201">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="2f208-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2f208-202">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2f208-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2f208-203">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f208-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="2f208-204">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f208-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="2f208-206">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2f208-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f208-207">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="2f208-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2f208-209">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="2f208-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2f208-211">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2f208-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2f208-213">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2f208-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2f208-215">a.</span><span class="sxs-lookup"><span data-stu-id="2f208-215">a.</span></span> <span data-ttu-id="2f208-216">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2f208-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2f208-217">b.</span><span class="sxs-lookup"><span data-stu-id="2f208-217">b.</span></span> <span data-ttu-id="2f208-218">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2f208-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2f208-219">c.</span><span class="sxs-lookup"><span data-stu-id="2f208-219">c.</span></span> <span data-ttu-id="2f208-220">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2f208-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2f208-221">d.</span><span class="sxs-lookup"><span data-stu-id="2f208-221">d.</span></span> <span data-ttu-id="2f208-222">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2f208-222">Click **Create**.</span></span>
 
### <a name="creating-a-sap-hana-test-user"></a><span data-ttu-id="2f208-223">Skapa en SAP HANA testanvändare</span><span class="sxs-lookup"><span data-stu-id="2f208-223">Creating a SAP HANA test user</span></span>

<span data-ttu-id="2f208-224">tooenable Azure AD-användare toolog i tooSAP HANA, måste de etableras till SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2f208-224">tooenable Azure AD users toolog in tooSAP HANA, they must be provisioned into SAP HANA.</span></span>
<span data-ttu-id="2f208-225">SAP HANA stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="2f208-225">SAP HANA supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="2f208-226">Om du behöver toocreate en användare manuellt utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2f208-226">If you need toocreate a user manually, perform hello following steps:</span></span>

>[!Note]
><span data-ttu-id="2f208-227">Du kan ändra hello extern autentisering som används av hello användare.</span><span class="sxs-lookup"><span data-stu-id="2f208-227">You can change hello external authentication used by hello user.</span></span>
<span data-ttu-id="2f208-228">Externa användare autentiseras med hjälp av ett externt system, till exempel Kerberos-system.</span><span class="sxs-lookup"><span data-stu-id="2f208-228">External users are authenticated using an external system, for example a Kerberos system.</span></span> <span data-ttu-id="2f208-229">Detaljerad information om externa identiteter, kontakta din [domänadministratören](https://cloudplatform.sap.com/contact.html).</span><span class="sxs-lookup"><span data-stu-id="2f208-229">For detailed information about external identities, contact your [domain administrator](https://cloudplatform.sap.com/contact.html).</span></span>

1. <span data-ttu-id="2f208-230">Öppna hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) som administratör och aktivera hello DB-användare för SAML SSO.</span><span class="sxs-lookup"><span data-stu-id="2f208-230">Open hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) as an administrator and enable hello DB-User for SAML SSO.</span></span>

    ![Skapa användare](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. <span data-ttu-id="2f208-232">Skalstreck hello osynliga kryssrutan toohello till vänster i **SAML** och hello konfigurera länken.</span><span class="sxs-lookup"><span data-stu-id="2f208-232">Tick hello invisible checkbox toohello left of **SAML** and follow hello Configure link.</span></span>

3. <span data-ttu-id="2f208-233">Klicka på **Lägg till** tooadd hello SAML IDP och klicka på **OK** välja hello lämpliga SAML IDP.</span><span class="sxs-lookup"><span data-stu-id="2f208-233">Click **Add** tooadd hello SAML IDP and click **OK** selecting hello appropriate SAML IDP.</span></span>

4. <span data-ttu-id="2f208-234">Lägg till hello **externa identitet** (t.ex.</span><span class="sxs-lookup"><span data-stu-id="2f208-234">Add hello **External Identity** (ex.</span></span> <span data-ttu-id="2f208-235">Här BrittaSimon) eller välj **”alla”** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f208-235">BrittaSimon here) or choose **"Any"** and click **OK**.</span></span>

    >[!Note]
    ><span data-ttu-id="2f208-236">Om ”alla” kryssrutan inte är markerad och sedan hello användarnamnet i HANA måste tooexactly matchar hello namnet på hello användare i hello UPN innan hello domänsuffix (d.v.s. BrittaSimon@contoso.com skulle bli BrittaSimon i HANA).</span><span class="sxs-lookup"><span data-stu-id="2f208-236">If "ANY" check-box is not checked, then hello user name in HANA needs tooexactly match hello name of hello user in hello UPN before hello domain suffix (i.e. BrittaSimon@contoso.com would become BrittaSimon in HANA).</span></span>

5. <span data-ttu-id="2f208-237">I testsyfte kan du tilldela alla **”XS”** roller toohello användare.</span><span class="sxs-lookup"><span data-stu-id="2f208-237">For testing purposes, assign all **"XS"** roles toohello user.</span></span>

    ![tilldela roller](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > <span data-ttu-id="2f208-239">Du bör ge de behörigheterna som är lämpliga för din användningsfall endast.</span><span class="sxs-lookup"><span data-stu-id="2f208-239">You should give those permissions appropriate for your use cases, only.</span></span>

6. <span data-ttu-id="2f208-240">Spara hello användare.</span><span class="sxs-lookup"><span data-stu-id="2f208-240">Save hello user.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2f208-241">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2f208-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2f208-242">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2f208-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP HANA.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="2f208-244">**tooassign Britta Simon tooSAP HANA, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="2f208-244">**tooassign Britta Simon tooSAP HANA, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f208-245">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="2f208-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="2f208-247">Välj i listan med program hello **SAP HANA**.</span><span class="sxs-lookup"><span data-stu-id="2f208-247">In hello applications list, select **SAP HANA**.</span></span>

    ![Tilldela användare](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. <span data-ttu-id="2f208-249">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="2f208-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202] 

4. <span data-ttu-id="2f208-251">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="2f208-251">Click **Add** button.</span></span> <span data-ttu-id="2f208-252">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2f208-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="2f208-254">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="2f208-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2f208-255">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2f208-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2f208-256">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2f208-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2f208-257">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2f208-257">Testing single sign-on</span></span>

<span data-ttu-id="2f208-258">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f208-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="2f208-259">När du klickar på hello SAP HANA-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour SAP HANA-programmet.</span><span class="sxs-lookup"><span data-stu-id="2f208-259">When you click hello SAP HANA tile in hello Access Panel, you should get automatically signed-on tooyour SAP HANA application.</span></span>
<span data-ttu-id="2f208-260">Läs mer om åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2f208-260">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f208-261">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2f208-261">Additional resources</span></span>

* [<span data-ttu-id="2f208-262">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2f208-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2f208-263">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2f208-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

