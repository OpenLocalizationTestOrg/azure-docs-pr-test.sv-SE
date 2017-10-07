---
title: "Självstudier: Azure Active Directory-integrering med Voyance | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Voyance."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 539dc1f9-64c9-4dce-b259-2b0b49dcf857
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2017
ms.author: jeedes
ms.openlocfilehash: e2cb9eb6b20e8611a9f6e8529b7c85a8d86ec3e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-voyance"></a><span data-ttu-id="ec4eb-103">Självstudier: Azure Active Directory-integrering med Voyance</span><span class="sxs-lookup"><span data-stu-id="ec4eb-103">Tutorial: Azure Active Directory integration with Voyance</span></span>

<span data-ttu-id="ec4eb-104">I kursen får du lära dig hur toointegrate Voyance med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="ec4eb-104">In this tutorial, you learn how toointegrate Voyance with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ec4eb-105">Integrera Voyance med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-105">Integrating Voyance with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="ec4eb-106">Du kan styra i Azure AD som har åtkomst till tooVoyance</span><span class="sxs-lookup"><span data-stu-id="ec4eb-106">You can control in Azure AD who has access tooVoyance</span></span>
- <span data-ttu-id="ec4eb-107">Du kan aktivera din användare tooautomatically get inloggade tooVoyance (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="ec4eb-107">You can enable your users tooautomatically get signed-on tooVoyance (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ec4eb-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ec4eb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="ec4eb-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ec4eb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec4eb-110">Krav</span><span class="sxs-lookup"><span data-stu-id="ec4eb-110">Prerequisites</span></span>

<span data-ttu-id="ec4eb-111">tooconfigure Azure AD-integrering med Voyance, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-111">tooconfigure Azure AD integration with Voyance, you need hello following items:</span></span>

- <span data-ttu-id="ec4eb-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="ec4eb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ec4eb-113">En Voyance enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="ec4eb-113">A Voyance single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ec4eb-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ec4eb-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ec4eb-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ec4eb-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ec4eb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ec4eb-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="ec4eb-118">Scenario description</span></span>
<span data-ttu-id="ec4eb-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ec4eb-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ec4eb-121">Att lägga till Voyance från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ec4eb-121">Adding Voyance from hello gallery</span></span>
2. <span data-ttu-id="ec4eb-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec4eb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-voyance-from-hello-gallery"></a><span data-ttu-id="ec4eb-123">Att lägga till Voyance från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="ec4eb-123">Adding Voyance from hello gallery</span></span>
<span data-ttu-id="ec4eb-124">tooconfigure hello integrering av Voyance i Azure AD, behöver du tooadd Voyance hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-124">tooconfigure hello integration of Voyance into Azure AD, you need tooadd Voyance from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="ec4eb-125">**tooadd Voyance från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ec4eb-125">**tooadd Voyance from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec4eb-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="ec4eb-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="ec4eb-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="ec4eb-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="ec4eb-133">Skriv i sökrutan hello **Voyance**väljer **Voyance** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-133">In hello search box, type **Voyance**, select  **Voyance**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Voyance i hello resultatlistan](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ec4eb-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec4eb-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ec4eb-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Voyance baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-136">In this section, you configure and test Azure AD single sign-on with Voyance based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ec4eb-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Voyance är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Voyance is tooa user in Azure AD.</span></span> <span data-ttu-id="ec4eb-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Voyance toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-138">In other words, a link relationship between an Azure AD user and hello related user in Voyance needs toobe established.</span></span>

<span data-ttu-id="ec4eb-139">I Voyance, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-139">In Voyance, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="ec4eb-140">tooconfigure och testa Azure AD enkel inloggning med Voyance, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-140">tooconfigure and test Azure AD single sign-on with Voyance, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="ec4eb-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="ec4eb-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ec4eb-143">**[Skapa en testanvändare Voyance](#create-a-voyance-test-user)**  -toohave en motsvarighet för Britta Simon i Voyance som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-143">**[Create a Voyance test user](#create-a-voyance-test-user)** - toohave a counterpart of Britta Simon in Voyance that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="ec4eb-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ec4eb-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ec4eb-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec4eb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ec4eb-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt Voyance program.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Voyance application.</span></span>

<span data-ttu-id="ec4eb-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Voyance:**</span><span class="sxs-lookup"><span data-stu-id="ec4eb-148">**tooconfigure Azure AD single sign-on with Voyance, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec4eb-149">I hello Azure-portalen på hello **Voyance** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-149">In hello Azure portal, on hello **Voyance** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="ec4eb-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_samlbase.png)

3. <span data-ttu-id="ec4eb-153">På hello **Voyance domän och URL: er** avsnittet, utför följande steg om du inte vill tooconfigure hello programmet hello **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-153">On hello **Voyance Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Enkel inloggning information för IDP Voyance domän och URL: er](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url1.png)

    <span data-ttu-id="ec4eb-155">a.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-155">a.</span></span> <span data-ttu-id="ec4eb-156">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.nyansa.com`</span><span class="sxs-lookup"><span data-stu-id="ec4eb-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.nyansa.com`</span></span>

    <span data-ttu-id="ec4eb-157">b.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-157">b.</span></span> <span data-ttu-id="ec4eb-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.nyansa.com/saml/create/`</span><span class="sxs-lookup"><span data-stu-id="ec4eb-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.nyansa.com/saml/create/`</span></span>

4. <span data-ttu-id="ec4eb-159">Kontrollera **visa avancerade inställningar för URL: en** och utför följande steg om du inte vill tooconfigure hello programmet hello **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-159">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![URL: er och Voyance domän med enkel inloggning information för SP](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_url2.png)

    <span data-ttu-id="ec4eb-161">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.nyansa.com/`</span><span class="sxs-lookup"><span data-stu-id="ec4eb-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.nyansa.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="ec4eb-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-162">These values are not real.</span></span> <span data-ttu-id="ec4eb-163">Uppdatera dessa värden med hello faktiska identifierare, Reply URL och inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="ec4eb-164">Kontakta [Voyance klienten supportteamet](mailto:support@nyansa.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-164">Contact [Voyance Client support team](mailto:support@nyansa.com) tooget these values.</span></span> 

5. <span data-ttu-id="ec4eb-165">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-165">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![länk för hämtning av hello-certifikat](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_certificate.png) 

6. <span data-ttu-id="ec4eb-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-167">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning spara](./media/active-directory-saas-voyance-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="ec4eb-169">På hello **Voyance Configuration** klickar du på **konfigurera Voyance** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-169">On hello **Voyance Configuration** section, click **Configure Voyance** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="ec4eb-170">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="ec4eb-170">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Voyance konfiguration](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_configure.png) 

8. <span data-ttu-id="ec4eb-172">I en annan webbläsarfönstret, inloggning tooyour Voyance klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-172">In a different web browser window, sign-on tooyour Voyance tenant as an administrator.</span></span>

9. <span data-ttu-id="ec4eb-173">Gå toohello övre högra hörnet av hello navigeringsfält och klicka på hello nedrullningsbara som säger ”**ABC University**”.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-173">Go toohello top right corner of hello navigation bar and click on hello drop-down that says "**Acme University**".</span></span>
    
    ![Konfigurera enkel inloggning på App sida ABC University](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_001.png) 

10. <span data-ttu-id="ec4eb-175">Klicka på ”**administrationsinställningar**”.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-175">Click "**Admin Settings**".</span></span>

    ![Konfigurera enkel inloggning på App-sida Admin-Inställningar](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_002.png)

11. <span data-ttu-id="ec4eb-177">Klicka på ”**användaråtkomst**” fliken.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-177">Click "**User Access**" tab.</span></span>

    ![Konfigurera enkel inloggning på App-sida-användaråtkomst](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_003.png)

12. <span data-ttu-id="ec4eb-179">Klicka på hello ”**SSO inaktiveras**” knappen tooconfigure Azure AD som en IdP använder SAML 2.0.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-179">Click hello "**SSO is disabled**" button tooconfigure Azure AD as an IdP using SAML 2.0.</span></span>

    ![Konfigurera enkel inloggning på App sida SSO inaktiveras knappen](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_004.png)

13. <span data-ttu-id="ec4eb-181">Gå för**SAML v2** avsnittet och utföra nedanstående steg:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-181">Go too**SAML v2** section and perform below steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida SAML v2](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_005.png)
    
    <span data-ttu-id="ec4eb-183">a.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-183">a.</span></span> <span data-ttu-id="ec4eb-184">Välj **aktiverat**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-184">Select **Enabled**.</span></span>
    
    <span data-ttu-id="ec4eb-185">b.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-185">b.</span></span> <span data-ttu-id="ec4eb-186">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **IdP inloggnings-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-186">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal Into hello **IdP Login URL** textbox.</span></span>

    <span data-ttu-id="ec4eb-187">c.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-187">c.</span></span> <span data-ttu-id="ec4eb-188">Öppna din hämtade Base64-kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **IdP Cert** textruta.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-188">Open your downloaded Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **IdP Cert** textbox.</span></span>
    
    <span data-ttu-id="ec4eb-189">d.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-189">d.</span></span> <span data-ttu-id="ec4eb-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="ec4eb-191">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="ec4eb-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="ec4eb-192">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="ec4eb-193">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ec4eb-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ec4eb-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="ec4eb-194">Create an Azure AD test user</span></span>

<span data-ttu-id="ec4eb-195">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="ec4eb-197">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ec4eb-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec4eb-198">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-voyance-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ec4eb-200">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-voyance-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ec4eb-202">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello webbinställningar](./media/active-directory-saas-voyance-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ec4eb-204">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ec4eb-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello användardialogrutan](./media/active-directory-saas-voyance-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ec4eb-206">a.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-206">a.</span></span> <span data-ttu-id="ec4eb-207">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ec4eb-208">b.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-208">b.</span></span> <span data-ttu-id="ec4eb-209">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ec4eb-210">c.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-210">c.</span></span> <span data-ttu-id="ec4eb-211">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="ec4eb-212">d.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-212">d.</span></span> <span data-ttu-id="ec4eb-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-213">Click **Create**.</span></span>
 
### <a name="create-a-voyance-test-user"></a><span data-ttu-id="ec4eb-214">Skapa en testanvändare Voyance</span><span class="sxs-lookup"><span data-stu-id="ec4eb-214">Create a Voyance test user</span></span>

<span data-ttu-id="ec4eb-215">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Voyance.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-215">hello objective of this section is toocreate a user called Britta Simon in Voyance.</span></span> <span data-ttu-id="ec4eb-216">Voyance stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-216">Voyance supports just-in-time provisioning, which is by default enabled.</span></span> <span data-ttu-id="ec4eb-217">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-217">There is no action item for you in this section.</span></span> <span data-ttu-id="ec4eb-218">En ny användare skapas under ett försök tooaccess Voyance om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-218">A new user is created during an attempt tooaccess Voyance if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="ec4eb-219">Om du behöver toocreate en användare manuellt, måste toocontact [Voyance supportteamet](maiLto:support@nyansa.com).</span><span class="sxs-lookup"><span data-stu-id="ec4eb-219">If you need toocreate a user manually, you need toocontact [Voyance support team](maiLto:support@nyansa.com).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="ec4eb-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="ec4eb-220">Assign hello Azure AD test user</span></span>

<span data-ttu-id="ec4eb-221">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooVoyance.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooVoyance.</span></span>

![Tilldela hello användarroll][200]

<span data-ttu-id="ec4eb-223">**tooassign Britta Simon tooVoyance utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="ec4eb-223">**tooassign Britta Simon tooVoyance, perform hello following steps:**</span></span>

1. <span data-ttu-id="ec4eb-224">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="ec4eb-226">Välj i listan med program hello **Voyance**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-226">In hello applications list, select **Voyance**.</span></span>

    ![Hej Voyance länken i listan med program hello](./media/active-directory-saas-voyance-tutorial/tutorial_voyance_app.png) 

3. <span data-ttu-id="ec4eb-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="ec4eb-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-230">Click **Add** button.</span></span> <span data-ttu-id="ec4eb-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="ec4eb-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="ec4eb-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ec4eb-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ec4eb-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="ec4eb-236">Test single sign-on</span></span>

<span data-ttu-id="ec4eb-237">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="ec4eb-238">Du bör få automatiskt inloggade tooyour Voyance programmet när du klickar på hello Voyance panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="ec4eb-238">When you click hello Voyance tile in hello Access Panel, you should get automatically signed-on tooyour Voyance application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ec4eb-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ec4eb-239">Additional resources</span></span>

* [<span data-ttu-id="ec4eb-240">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ec4eb-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ec4eb-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="ec4eb-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-voyance-tutorial/tutorial_general_203.png

