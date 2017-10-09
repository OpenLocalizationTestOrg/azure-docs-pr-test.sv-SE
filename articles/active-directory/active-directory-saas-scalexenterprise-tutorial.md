---
title: "Självstudier: Azure Active Directory-integrering med ScaleX Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ScaleX Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.openlocfilehash: e398b98d9e0957b5f92c82359651c345d22c3a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a><span data-ttu-id="8364a-103">Självstudier: Azure Active Directory-integrering med ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="8364a-103">Tutorial: Azure Active Directory integration with ScaleX Enterprise</span></span>

<span data-ttu-id="8364a-104">I kursen får du lära dig hur toointegrate ScaleX Enterprise med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8364a-104">In this tutorial, you learn how toointegrate ScaleX Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8364a-105">Integrera ScaleX Enterprise med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8364a-105">Integrating ScaleX Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8364a-106">Du kan styra i Azure AD som har åtkomst tooScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="8364a-106">You can control in Azure AD who has access tooScaleX Enterprise</span></span>
- <span data-ttu-id="8364a-107">Du kan aktivera din användare tooautomatically get inloggade tooScaleX Enterprise (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8364a-107">You can enable your users tooautomatically get signed-on tooScaleX Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8364a-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8364a-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8364a-109">Om du vill tooknow finns mer information om SaaS appintegrering med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8364a-109">If you want tooknow more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="8364a-110">Vad är programåtkomst och enkel inloggning med [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8364a-110">What is application access and single sign-on with [Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8364a-111">Krav</span><span class="sxs-lookup"><span data-stu-id="8364a-111">Prerequisites</span></span>

<span data-ttu-id="8364a-112">tooconfigure Azure AD-integrering med ScaleX Enterprise, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8364a-112">tooconfigure Azure AD integration with ScaleX Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="8364a-113">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8364a-113">An Azure AD subscription</span></span>
- <span data-ttu-id="8364a-114">En ScaleX Enterprise enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="8364a-114">A ScaleX Enterprise single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8364a-115">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8364a-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8364a-116">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8364a-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8364a-117">Använd inte i produktionsmiljön, om detta är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8364a-117">Do not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="8364a-118">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8364a-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8364a-119">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8364a-119">Scenario description</span></span>
<span data-ttu-id="8364a-120">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8364a-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8364a-121">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8364a-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8364a-122">Att lägga till ScaleX Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8364a-122">Adding ScaleX Enterprise from hello gallery</span></span>
2. <span data-ttu-id="8364a-123">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8364a-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-scalex-enterprise-from-hello-gallery"></a><span data-ttu-id="8364a-124">Att lägga till ScaleX Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8364a-124">Adding ScaleX Enterprise from hello gallery</span></span>
<span data-ttu-id="8364a-125">tooconfigure hello integrering av ScaleX Enterprise i tooAzure AD, behöver du tooadd ScaleX Enterprise hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="8364a-125">tooconfigure hello integration of ScaleX Enterprise in tooAzure AD, you need tooadd ScaleX Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8364a-126">**tooadd ScaleX Enterprise från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8364a-126">**tooadd ScaleX Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8364a-127">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8364a-127">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8364a-129">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="8364a-129">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8364a-130">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="8364a-130">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="8364a-132">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8364a-132">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="8364a-134">Skriv i sökrutan hello **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="8364a-134">In hello search box, type **ScaleX Enterprise**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. <span data-ttu-id="8364a-136">Markera hello resultat på panelen **ScaleX Enterprise**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="8364a-136">In hello results panel, select **ScaleX Enterprise**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8364a-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8364a-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8364a-139">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med ScaleX Enterprise baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8364a-139">In this section, you configure and test Azure AD single sign-on with ScaleX Enterprise based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8364a-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ScaleX företag är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8364a-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ScaleX Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="8364a-141">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i ScaleX Enterprise toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="8364a-141">In other words, a link relationship between an Azure AD user and hello related user in ScaleX Enterprise needs toobe established.</span></span>

<span data-ttu-id="8364a-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ScaleX företag.</span><span class="sxs-lookup"><span data-stu-id="8364a-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ScaleX Enterprise.</span></span>

<span data-ttu-id="8364a-143">tooconfigure och testa Azure AD enkel inloggning med ScaleX Enterprise, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8364a-143">tooconfigure and test Azure AD single sign-on with ScaleX Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8364a-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8364a-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8364a-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8364a-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8364a-146">**[Skapa en testanvändare ScaleX Enterprise](#creating-a-scalex-enterprise-test-user)**  -toohave en motsvarighet för Britta Simon i ScaleX företag som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="8364a-146">**[Creating a ScaleX Enterprise test user](#creating-a-scalex-enterprise-test-user)** - toohave a counterpart of Britta Simon in ScaleX Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8364a-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8364a-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8364a-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8364a-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8364a-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8364a-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8364a-150">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ScaleX Enterprise-programmet.</span><span class="sxs-lookup"><span data-stu-id="8364a-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ScaleX Enterprise application.</span></span>

<span data-ttu-id="8364a-151">**Utför följande hello tooconfigure Azure AD enkel inloggning med ScaleX Enterprise:**</span><span class="sxs-lookup"><span data-stu-id="8364a-151">**tooconfigure Azure AD single sign-on with ScaleX Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="8364a-152">I hello Azure-portalen på hello **ScaleX Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8364a-152">In hello Azure portal, on hello **ScaleX Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="8364a-154">På hello **enkel inloggning** dialogrutan som **läge** Välj **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8364a-154">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. <span data-ttu-id="8364a-156">På hello **ScaleX företagsdomänen och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="8364a-156">On hello **ScaleX Enterprise Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    <span data-ttu-id="8364a-158">a.</span><span class="sxs-lookup"><span data-stu-id="8364a-158">a.</span></span> <span data-ttu-id="8364a-159">I hello **identifierare** textruta hello TYPVÄRDE med hello följande mönster:`https://platform.rescale.com/saml2/<company id>/`</span><span class="sxs-lookup"><span data-stu-id="8364a-159">In hello **Identifier** textbox, type hello value using hello following pattern: `https://platform.rescale.com/saml2/<company id>/`</span></span>

    <span data-ttu-id="8364a-160">b.</span><span class="sxs-lookup"><span data-stu-id="8364a-160">b.</span></span> <span data-ttu-id="8364a-161">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://platform.rescale.com/saml2/<company id>/acs/`</span><span class="sxs-lookup"><span data-stu-id="8364a-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://platform.rescale.com/saml2/<company id>/acs/`</span></span>

4. <span data-ttu-id="8364a-162">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="8364a-162">Check **Show advanced URL settings**, if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    <span data-ttu-id="8364a-164">I hello **inloggnings-URL** textruta hello TYPVÄRDE med hello följande mönster:`https://platform.rescale.com/saml2/<company id>/sso/`</span><span class="sxs-lookup"><span data-stu-id="8364a-164">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://platform.rescale.com/saml2/<company id>/sso/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="8364a-165">Detta är inte hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="8364a-165">These are not hello real values.</span></span> <span data-ttu-id="8364a-166">Uppdatera dessa värden med hello faktiska identifierare, Reply URL eller inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="8364a-166">Update these values with hello actual Identifier, Reply URL or Sign-On URL.</span></span> <span data-ttu-id="8364a-167">Kontakta [ScaleX Enterprise-klienten supportteamet](http://info.rescale.com/contact_sales) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8364a-167">Contact [ScaleX Enterprise Client support team](http://info.rescale.com/contact_sales) tooget these values.</span></span> 

5. <span data-ttu-id="8364a-168">Tillämpningsprogrammet ScaleX förväntar hello SAML intyg i ett specifikt format, vilket kräver att du toomodify attributet mappningar tooyour SAML-token attribut-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="8364a-168">Your ScaleX application expects hello SAML assertions in a specific format, which requires you toomodify custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="8364a-169">Klicka på **visa och redigera andra användarattribut** kryssrutan tooopen hello anpassade attribut inställningar.</span><span class="sxs-lookup"><span data-stu-id="8364a-169">Click **View and edit all other user attributes** checkbox tooopen hello custom attributes settings.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    <span data-ttu-id="8364a-171">a.</span><span class="sxs-lookup"><span data-stu-id="8364a-171">a.</span></span> <span data-ttu-id="8364a-172">Högerklicka på hello attributet **namnet** och klicka på Ta bort.</span><span class="sxs-lookup"><span data-stu-id="8364a-172">Right click hello attribute **name** and click delete.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    <span data-ttu-id="8364a-174">b.</span><span class="sxs-lookup"><span data-stu-id="8364a-174">b.</span></span> <span data-ttu-id="8364a-175">Klicka på **e-postadress** attributet tooopen hello Redigera attribut fönster.</span><span class="sxs-lookup"><span data-stu-id="8364a-175">Click **emailaddress** attribute tooopen hello Edit Attribute window.</span></span> <span data-ttu-id="8364a-176">Ändra dess värde från **user.mail** för**user.userprincipalname** och klicka på Ok.</span><span class="sxs-lookup"><span data-stu-id="8364a-176">Change its value from **user.mail** too**user.userprincipalname** and click Ok.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)   
    
5. <span data-ttu-id="8364a-178">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="8364a-178">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello Certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. <span data-ttu-id="8364a-180">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="8364a-180">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="8364a-182">På hello **ScaleX företagskonfiguration** klickar du på **konfigurera ScaleX Enterprise** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="8364a-182">On hello **ScaleX Enterprise Configuration** section, click **Configure ScaleX Enterprise** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8364a-183">Kopiera hello **SAML enhets-ID** och **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="8364a-183">Copy hello **SAML Entity ID** and **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. <span data-ttu-id="8364a-185">tooconfigure enkel inloggning på **ScaleX Enterprise** sida, inloggning toohello ScaleX Enterprise företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="8364a-185">tooconfigure single sign-on on **ScaleX Enterprise** side, login toohello ScaleX Enterprise company website as an administrator.</span></span>

9. <span data-ttu-id="8364a-186">Klicka på hello-menyn i hello övre högra och välj **Contoso Administration**.</span><span class="sxs-lookup"><span data-stu-id="8364a-186">Click hello menu in hello upper right and select **Contoso Administration**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8364a-187">Contoso är bara ett exempel.</span><span class="sxs-lookup"><span data-stu-id="8364a-187">Contoso is just an example.</span></span> <span data-ttu-id="8364a-188">Detta bör vara faktiska företagets namn.</span><span class="sxs-lookup"><span data-stu-id="8364a-188">This should be your actual Company Name.</span></span> 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. <span data-ttu-id="8364a-190">Välj **integreringar** hello översta menyn och välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="8364a-190">Select **Integrations** from hello top menu and select **Single Sign-On**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. <span data-ttu-id="8364a-192">Fylla hello på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="8364a-192">Complete hello form as follows:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    <span data-ttu-id="8364a-194">a.</span><span class="sxs-lookup"><span data-stu-id="8364a-194">a.</span></span> <span data-ttu-id="8364a-195">Välj **”skapa alla användare som kan autentisera med enkel inloggning”.**</span><span class="sxs-lookup"><span data-stu-id="8364a-195">Select **“Create any user who can authenticate with SSO.”**</span></span>

    <span data-ttu-id="8364a-196">b.</span><span class="sxs-lookup"><span data-stu-id="8364a-196">b.</span></span> <span data-ttu-id="8364a-197">**Saml-leverantör**: klistra in hello värde ***urn: oasis: namn: tc: SAML:2.0:nameid-format: beständiga***</span><span class="sxs-lookup"><span data-stu-id="8364a-197">**Service Provider saml**: Paste hello value ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent***</span></span>

    <span data-ttu-id="8364a-198">c.</span><span class="sxs-lookup"><span data-stu-id="8364a-198">c.</span></span> <span data-ttu-id="8364a-199">**Namnet på identitetsleverantör e-fält i ACS-svar**: klistra in hello värde`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span><span class="sxs-lookup"><span data-stu-id="8364a-199">**Name of Identity Provider email field in ACS response**: Paste hello value `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`</span></span>

    <span data-ttu-id="8364a-200">d.</span><span class="sxs-lookup"><span data-stu-id="8364a-200">d.</span></span> <span data-ttu-id="8364a-201">**Identitet providern EntityDescriptor enhets-ID:** klistra in hello **SAML enhets-ID** värde kopieras från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8364a-201">**Identity Provider EntityDescriptor Entity ID:** Paste hello **SAML Entity ID** value copied from hello Azure portal.</span></span>

    <span data-ttu-id="8364a-202">e.</span><span class="sxs-lookup"><span data-stu-id="8364a-202">e.</span></span> <span data-ttu-id="8364a-203">**Identitet providern SingleSignOnService URL:** klistra in hello **SAML inloggning tjänst-URL för enkel** från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8364a-203">**Identity Provider SingleSignOnService URL:** Paste hello **SAML Single Sign-On Service URL** from hello Azure portal.</span></span>

    <span data-ttu-id="8364a-204">f.</span><span class="sxs-lookup"><span data-stu-id="8364a-204">f.</span></span> <span data-ttu-id="8364a-205">**Providern offentliga X509 identitetscertifikat:** öppna hello X509 certifikat hämtas från hello Azure i anteckningar och klistra in hello innehållet i den här rutan.</span><span class="sxs-lookup"><span data-stu-id="8364a-205">**Identity Provider public X509 certificate:** Open hello X509 certificate downloaded from hello Azure in notepad and paste hello contents in this box.</span></span> <span data-ttu-id="8364a-206">Se till att det finns inga radbrytningar i hello mellersta hello certifikat innehåll.</span><span class="sxs-lookup"><span data-stu-id="8364a-206">Ensure there are no line breaks in hello middle of hello certificate contents.</span></span>
    
    <span data-ttu-id="8364a-207">g.</span><span class="sxs-lookup"><span data-stu-id="8364a-207">g.</span></span> <span data-ttu-id="8364a-208">Kontrollera följande kryssrutorna hello: **aktiverad, kryptera NameID och logga AuthnRequests.**</span><span class="sxs-lookup"><span data-stu-id="8364a-208">Check hello following checkboxes: **Enabled, Encrypt NameID and Sign AuthnRequests.**</span></span>

    <span data-ttu-id="8364a-209">h.</span><span class="sxs-lookup"><span data-stu-id="8364a-209">h.</span></span> <span data-ttu-id="8364a-210">Klicka på **SSO uppdateringsinställningar** toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="8364a-210">Click **Update SSO Settings** toosave hello settings.</span></span>

> [!TIP]
> <span data-ttu-id="8364a-211">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="8364a-211">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8364a-212">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="8364a-212">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8364a-213">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8364a-213">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8364a-214">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8364a-214">Creating an Azure AD test user</span></span>
<span data-ttu-id="8364a-215">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8364a-215">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="8364a-217">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8364a-217">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8364a-218">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="8364a-218">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8364a-220">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="8364a-220">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8364a-222">Hello överkant hello dialogrutan, klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8364a-222">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8364a-224">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8364a-224">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8364a-226">a.</span><span class="sxs-lookup"><span data-stu-id="8364a-226">a.</span></span> <span data-ttu-id="8364a-227">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8364a-227">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8364a-228">b.</span><span class="sxs-lookup"><span data-stu-id="8364a-228">b.</span></span> <span data-ttu-id="8364a-229">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8364a-229">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8364a-230">c.</span><span class="sxs-lookup"><span data-stu-id="8364a-230">c.</span></span> <span data-ttu-id="8364a-231">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8364a-231">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8364a-232">d.</span><span class="sxs-lookup"><span data-stu-id="8364a-232">d.</span></span> <span data-ttu-id="8364a-233">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="8364a-233">Click **Create**.</span></span>
 
### <a name="creating-a-scalex-enterprise-test-user"></a><span data-ttu-id="8364a-234">Skapa en testanvändare ScaleX Enterprise</span><span class="sxs-lookup"><span data-stu-id="8364a-234">Creating a ScaleX Enterprise test user</span></span>

<span data-ttu-id="8364a-235">tooenable Azure AD-användare toolog i tooScaleX Enterprise, måste de vara etablerade i tooScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="8364a-235">tooenable Azure AD users toolog in tooScaleX Enterprise, they must be provisioned in tooScaleX Enterprise.</span></span> <span data-ttu-id="8364a-236">Etablering är en automatisk uppgift hello gäller ScaleX Enterprise, och inga manuella steg krävs.</span><span class="sxs-lookup"><span data-stu-id="8364a-236">In hello case of ScaleX Enterprise, provisioning is an automatic task and no manual steps are required.</span></span> <span data-ttu-id="8364a-237">Alla användare som kan autentisera med autentiseringsuppgifter för enkel inloggning ska etableras automatiskt på hello ScaleX sida.</span><span class="sxs-lookup"><span data-stu-id="8364a-237">Any user who can successfully authenticate with SSO credentials will be automatically provisioned on hello ScaleX side.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8364a-238">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="8364a-238">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8364a-239">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja användare åtkomst tooScaleX Enterprise.</span><span class="sxs-lookup"><span data-stu-id="8364a-239">In this section, you enable Britta Simon toouse Azure single sign-on by granting user access tooScaleX Enterprise.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8364a-241">**tooassign Britta Simon tooScaleX Enterprise, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="8364a-241">**tooassign Britta Simon tooScaleX Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="8364a-242">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="8364a-242">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8364a-244">Välj i listan med program hello **ScaleX Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="8364a-244">In hello applications list, select **ScaleX Enterprise**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. <span data-ttu-id="8364a-246">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="8364a-246">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="8364a-248">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="8364a-248">Click **Add** button.</span></span> <span data-ttu-id="8364a-249">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8364a-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="8364a-251">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="8364a-251">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8364a-252">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8364a-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8364a-253">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8364a-253">Click **Assign** button on **Add Assignment** dialog.</span></span>

### <a name="testing-single-sign-on"></a><span data-ttu-id="8364a-254">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8364a-254">Testing single sign-on</span></span>

<span data-ttu-id="8364a-255">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8364a-255">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8364a-256">Klicka på hello ScaleX Enterprise-panelen i hello åtkomstpanelen, får du automatiskt inloggade tooyour ScaleX företagsprogram.</span><span class="sxs-lookup"><span data-stu-id="8364a-256">Click hello ScaleX Enterprise tile in hello Access Panel, you will get automatically signed-on tooyour ScaleX Enterprise application.</span></span> <span data-ttu-id="8364a-257">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="8364a-257">For more information about hello Access Panel, see [Introduction toohello Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="8364a-258">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8364a-258">Additional resources</span></span>

* [<span data-ttu-id="8364a-259">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8364a-259">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8364a-260">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8364a-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png

