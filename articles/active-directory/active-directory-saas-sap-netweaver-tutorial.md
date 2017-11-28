---
title: "Självstudier: Azure Active Directory-integrering med SAP NetWeaver | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SAP NetWeaver."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: 69008734864e1e258a0c2ec872e51aa331491fbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a><span data-ttu-id="6fc69-103">Självstudier: Azure Active Directory-integration med SAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="6fc69-103">Tutorial: Azure Active Directory integration with SAP NetWeaver</span></span>

<span data-ttu-id="6fc69-104">I kursen får du lära dig hur toointegrate SAP NetWeaver med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="6fc69-104">In this tutorial, you learn how toointegrate SAP NetWeaver with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6fc69-105">Integrera SAP NetWeaver med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="6fc69-105">Integrating SAP NetWeaver with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6fc69-106">Du kan styra i Azure AD som har åtkomst tooSAP NetWeaver</span><span class="sxs-lookup"><span data-stu-id="6fc69-106">You can control in Azure AD who has access tooSAP NetWeaver</span></span>
- <span data-ttu-id="6fc69-107">Du kan aktivera din användare tooautomatically get inloggade tooSAP NetWeaver (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="6fc69-107">You can enable your users tooautomatically get signed-on tooSAP NetWeaver (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6fc69-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6fc69-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6fc69-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6fc69-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6fc69-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6fc69-110">Prerequisites</span></span>

<span data-ttu-id="6fc69-111">tooconfigure Azure AD-integrering med SAP NetWeaver måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="6fc69-111">tooconfigure Azure AD integration with SAP NetWeaver, you need hello following items:</span></span>

- <span data-ttu-id="6fc69-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="6fc69-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6fc69-113">En SAP NetWeaver enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="6fc69-113">An SAP NetWeaver single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6fc69-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6fc69-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6fc69-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="6fc69-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6fc69-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="6fc69-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6fc69-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6fc69-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6fc69-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="6fc69-118">Scenario description</span></span>
<span data-ttu-id="6fc69-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="6fc69-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6fc69-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="6fc69-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6fc69-121">Att lägga till SAP NetWeaver från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6fc69-121">Adding SAP NetWeaver from hello gallery</span></span>
2. <span data-ttu-id="6fc69-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6fc69-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-netweaver-from-hello-gallery"></a><span data-ttu-id="6fc69-123">Att lägga till SAP NetWeaver från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="6fc69-123">Adding SAP NetWeaver from hello gallery</span></span>
<span data-ttu-id="6fc69-124">tooconfigure hello integrering av SAP NetWeaver i Azure AD, behöver du tooadd SAP NetWeaver hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="6fc69-124">tooconfigure hello integration of SAP NetWeaver into Azure AD, you need tooadd SAP NetWeaver from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6fc69-125">**tooadd SAP NetWeaver från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6fc69-125">**tooadd SAP NetWeaver from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fc69-126">I hello  **[Azure Portal](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6fc69-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6fc69-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6fc69-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="6fc69-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6fc69-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="6fc69-133">Skriv i sökrutan hello **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-133">In hello search box, type **SAP NetWeaver**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

5. <span data-ttu-id="6fc69-135">Markera hello resultat på panelen **SAP NetWeaver**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="6fc69-135">In hello results panel, select **SAP NetWeaver**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6fc69-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6fc69-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6fc69-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med SAP NetWeaver baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="6fc69-138">In this section, you configure and test Azure AD single sign-on with SAP NetWeaver based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="6fc69-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SAP NetWeaver är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="6fc69-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP NetWeaver is tooa user in Azure AD.</span></span> <span data-ttu-id="6fc69-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SAP NetWeaver toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="6fc69-140">In other words, a link relationship between an Azure AD user and hello related user in SAP NetWeaver needs toobe established.</span></span>

<span data-ttu-id="6fc69-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="6fc69-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SAP NetWeaver.</span></span>

<span data-ttu-id="6fc69-142">tooconfigure och testa Azure AD enkel inloggning med SAP NetWeaver, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="6fc69-142">tooconfigure and test Azure AD single sign-on with SAP NetWeaver, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6fc69-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="6fc69-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6fc69-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6fc69-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6fc69-145">**[Skapa en testanvändare för SAP NetWeaver](#creating-an-sap-netweaver-test-user)**  -toohave en motsvarighet för Britta Simon i SAP NetWeaver som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="6fc69-145">**[Creating an SAP NetWeaver test user](#creating-an-sap-netweaver-test-user)** - toohave a counterpart of Britta Simon in SAP NetWeaver that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6fc69-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6fc69-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6fc69-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="6fc69-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6fc69-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6fc69-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6fc69-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program för SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="6fc69-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP NetWeaver application.</span></span>

<span data-ttu-id="6fc69-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SAP NetWeaver:**</span><span class="sxs-lookup"><span data-stu-id="6fc69-150">**tooconfigure Azure AD single sign-on with SAP NetWeaver, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fc69-151">I hello Azure-portalen på hello **SAP NetWeaver** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-151">In hello Azure portal, on hello **SAP NetWeaver** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="6fc69-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="6fc69-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

3. <span data-ttu-id="6fc69-155">På hello **URL: er och SAP NetWeaver domän** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6fc69-155">On hello **SAP NetWeaver Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    <span data-ttu-id="6fc69-157">a.</span><span class="sxs-lookup"><span data-stu-id="6fc69-157">a.</span></span> <span data-ttu-id="6fc69-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="6fc69-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="6fc69-159">b.</span><span class="sxs-lookup"><span data-stu-id="6fc69-159">b.</span></span> <span data-ttu-id="6fc69-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<your company instance of SAP NetWeaver>`</span><span class="sxs-lookup"><span data-stu-id="6fc69-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<your company instance of SAP NetWeaver>`</span></span>

    <span data-ttu-id="6fc69-161">c.</span><span class="sxs-lookup"><span data-stu-id="6fc69-161">c.</span></span> <span data-ttu-id="6fc69-162">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span><span class="sxs-lookup"><span data-stu-id="6fc69-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="6fc69-163">Dessa värden är inte hello verkliga.</span><span class="sxs-lookup"><span data-stu-id="6fc69-163">These values are not hello real.</span></span> <span data-ttu-id="6fc69-164">Uppdatera dessa värden med hello faktiska identifierare och Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="6fc69-164">Update these values with hello actual Identifier and Reply URL and Sign-On URL.</span></span> <span data-ttu-id="6fc69-165">Vi rekommenderar här du toouse hello unikt värde i strängen i hello identifierare.</span><span class="sxs-lookup"><span data-stu-id="6fc69-165">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="6fc69-166">Kontakta [SAP NetWeaver klienten supportteamet](https://www.sap.com/support.html) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="6fc69-166">Contact [SAP NetWeaver Client support team](https://www.sap.com/support.html) tooget these values.</span></span> 

4. <span data-ttu-id="6fc69-167">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara hello XML-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="6fc69-167">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

5. <span data-ttu-id="6fc69-169">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="6fc69-169">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="6fc69-171">På hello **SAP NetWeaver Configuration** klickar du på **konfigurera SAP NetWeaver** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="6fc69-171">On hello **SAP NetWeaver Configuration** section, click **Configure SAP NetWeaver** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6fc69-172">Kopiera hello **SAML enhets-ID** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="6fc69-172">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

7. <span data-ttu-id="6fc69-174">tooconfigure enkel inloggning på **SAP NetWeaver** sida, behöver du toosend hello hämtas **XML-Metadata för** och **SAML enhets-ID** för[SAP NetWeaver stöd ](https://www.sap.com/support.html).</span><span class="sxs-lookup"><span data-stu-id="6fc69-174">tooconfigure single sign-on on **SAP NetWeaver** side, you need toosend hello downloaded **Metadata XML** and **SAML Entity ID** too[SAP NetWeaver support](https://www.sap.com/support.html).</span></span> 

> [!TIP]
> <span data-ttu-id="6fc69-175">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="6fc69-175">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6fc69-176">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="6fc69-176">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6fc69-177">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6fc69-177">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6fc69-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="6fc69-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="6fc69-179">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6fc69-179">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="6fc69-181">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="6fc69-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fc69-182">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="6fc69-182">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6fc69-184">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-184">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6fc69-186">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6fc69-186">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6fc69-188">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6fc69-188">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-sap-netweaver-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6fc69-190">a.</span><span class="sxs-lookup"><span data-stu-id="6fc69-190">a.</span></span> <span data-ttu-id="6fc69-191">I hello **namn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-191">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="6fc69-192">b.</span><span class="sxs-lookup"><span data-stu-id="6fc69-192">b.</span></span> <span data-ttu-id="6fc69-193">I hello **användarnamn** textruta typen hello **e-postadress** av Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6fc69-193">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="6fc69-194">c.</span><span class="sxs-lookup"><span data-stu-id="6fc69-194">c.</span></span> <span data-ttu-id="6fc69-195">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-195">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6fc69-196">d.</span><span class="sxs-lookup"><span data-stu-id="6fc69-196">d.</span></span> <span data-ttu-id="6fc69-197">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-197">Click **Create**.</span></span>
 
### <a name="creating-an-sap-netweaver-test-user"></a><span data-ttu-id="6fc69-198">Skapa en SAP NetWeaver testanvändare</span><span class="sxs-lookup"><span data-stu-id="6fc69-198">Creating an SAP NetWeaver test user</span></span>

<span data-ttu-id="6fc69-199">I det här avsnittet skapar du en användare som kallas Britta Simon i SAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="6fc69-199">In this section, you create a user called Britta Simon in SAP NetWeaver.</span></span> <span data-ttu-id="6fc69-200">Arbeta med din [SAP NetWeaver stöd](https://www.sap.com/support.html) tooadd hello användare i hello SAP NetWeaver plattform.</span><span class="sxs-lookup"><span data-stu-id="6fc69-200">Work with your [SAP NetWeaver support](https://www.sap.com/support.html) tooadd hello users in hello SAP NetWeaver platform.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6fc69-201">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="6fc69-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6fc69-202">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooSAP NetWeaver.</span><span class="sxs-lookup"><span data-stu-id="6fc69-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP NetWeaver.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="6fc69-204">**tooassign Britta Simon tooSAP NetWeaver, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="6fc69-204">**tooassign Britta Simon tooSAP NetWeaver, perform hello following steps:**</span></span>

1. <span data-ttu-id="6fc69-205">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="6fc69-207">Välj i listan med program hello **SAP NetWeaver**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-207">In hello applications list, select **SAP NetWeaver**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

3. <span data-ttu-id="6fc69-209">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="6fc69-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="6fc69-211">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="6fc69-211">Click **Add** button.</span></span> <span data-ttu-id="6fc69-212">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6fc69-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="6fc69-214">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="6fc69-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6fc69-215">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6fc69-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6fc69-216">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6fc69-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6fc69-217">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="6fc69-217">Testing single sign-on</span></span>

<span data-ttu-id="6fc69-218">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6fc69-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6fc69-219">Du bör få automatiskt inloggade tooyour SAP NetWeaver programmet när du klickar på hello SAP NetWeaver panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="6fc69-219">When you click hello SAP NetWeaver tile in hello Access Panel, you should get automatically signed-on tooyour SAP NetWeaver application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6fc69-220">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="6fc69-220">Additional resources</span></span>

* [<span data-ttu-id="6fc69-221">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6fc69-221">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6fc69-222">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="6fc69-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-netweaver-tutorial/tutorial_general_203.png

