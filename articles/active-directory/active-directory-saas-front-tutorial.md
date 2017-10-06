---
title: "Självstudier: Azure Active Directory-integrering med framför | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och fram."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 4be363a3d338ec9268f3324daab4a80346ec3131
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a><span data-ttu-id="d00be-103">Självstudier: Azure Active Directory-integrering med framför</span><span class="sxs-lookup"><span data-stu-id="d00be-103">Tutorial: Azure Active Directory integration with Front</span></span>

<span data-ttu-id="d00be-104">I kursen får du lära dig hur toointegrate framför med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d00be-104">In this tutorial, you learn how toointegrate Front with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d00be-105">Integrera framför med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d00be-105">Integrating Front with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d00be-106">Du kan styra i Azure AD som har åtkomst till tooFront.</span><span class="sxs-lookup"><span data-stu-id="d00be-106">You can control in Azure AD who has access tooFront.</span></span>
- <span data-ttu-id="d00be-107">Du kan låta dina användare tooautomatically get inloggade tooFront (Single Sign-On) med sina Azure AD-konton.</span><span class="sxs-lookup"><span data-stu-id="d00be-107">You can enable your users tooautomatically get signed-on tooFront (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="d00be-108">Du kan hantera dina konton i en central plats - hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d00be-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="d00be-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d00be-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d00be-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d00be-110">Prerequisites</span></span>

<span data-ttu-id="d00be-111">tooconfigure Azure AD-integrering med framför, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d00be-111">tooconfigure Azure AD integration with Front, you need hello following items:</span></span>

- <span data-ttu-id="d00be-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d00be-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d00be-113">En framför enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="d00be-113">A Front single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d00be-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d00be-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d00be-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d00be-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d00be-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d00be-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d00be-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d00be-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d00be-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d00be-118">Scenario description</span></span>
<span data-ttu-id="d00be-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d00be-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d00be-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d00be-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d00be-121">Att lägga till framför från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d00be-121">Adding Front from hello gallery</span></span>
2. <span data-ttu-id="d00be-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d00be-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-front-from-hello-gallery"></a><span data-ttu-id="d00be-123">Att lägga till framför från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d00be-123">Adding Front from hello gallery</span></span>
<span data-ttu-id="d00be-124">tooconfigure hello integrering av framför i Azure AD, behöver du tooadd framför hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d00be-124">tooconfigure hello integration of Front into Azure AD, you need tooadd Front from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d00be-125">**tooadd fram från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d00be-125">**tooadd Front from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d00be-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d00be-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory-knappen][1]

2. <span data-ttu-id="d00be-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d00be-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d00be-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d00be-129">Then go too**All applications**.</span></span>

    ![hello Enterprise program bladet][2]
    
3. <span data-ttu-id="d00be-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d00be-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello-knappen för nytt program][3]

4. <span data-ttu-id="d00be-133">Skriv i sökrutan hello **främre**väljer **främre** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d00be-133">In hello search box, type **Front**, select **Front** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Framför i hello resultatlistan](./media/active-directory-saas-front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d00be-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d00be-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="d00be-136">I det här avsnittet, konfigurera och testa Azure AD enkel inloggning med framför baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d00be-136">In this section, you configure and test Azure AD single sign-on with Front based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d00be-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren fram är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d00be-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Front is tooa user in Azure AD.</span></span> <span data-ttu-id="d00be-138">Med andra ord måste en länk mellan en Azure AD-användare och hello fram relaterade användaren toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="d00be-138">In other words, a link relationship between an Azure AD user and hello related user in Front needs toobe established.</span></span>

<span data-ttu-id="d00be-139">Framför, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d00be-139">In Front, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d00be-140">tooconfigure och testa Azure AD enkel inloggning med framför, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d00be-140">tooconfigure and test Azure AD single sign-on with Front, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d00be-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d00be-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d00be-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d00be-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d00be-143">**[Skapa en testanvändare framför](#create-a-front-test-user)**  -toohave en motsvarighet Britta Simon fram som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d00be-143">**[Create a Front test user](#create-a-front-test-user)** - toohave a counterpart of Britta Simon in Front that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d00be-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d00be-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d00be-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d00be-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="d00be-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d00be-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="d00be-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt program fram.</span><span class="sxs-lookup"><span data-stu-id="d00be-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Front application.</span></span>

<span data-ttu-id="d00be-148">**tooconfigure Azure AD enkel inloggning med framsidan, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d00be-148">**tooconfigure Azure AD single sign-on with Front, perform hello following steps:**</span></span>

1. <span data-ttu-id="d00be-149">I hello Azure-portalen på hello **främre** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d00be-149">In hello Azure portal, on hello **Front** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning länk][4]

2. <span data-ttu-id="d00be-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d00be-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Enkel inloggning dialogrutan](./media/active-directory-saas-front-tutorial/tutorial_front_samlbase.png)

3. <span data-ttu-id="d00be-153">På hello **främre domän och URL: er** om du vill tooconfigure hello programmet i **IDP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="d00be-153">On hello **Front Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_url1.png)

    <span data-ttu-id="d00be-155">a.</span><span class="sxs-lookup"><span data-stu-id="d00be-155">a.</span></span> <span data-ttu-id="d00be-156">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="d00be-156">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com`</span></span>

    <span data-ttu-id="d00be-157">b.</span><span class="sxs-lookup"><span data-stu-id="d00be-157">b.</span></span> <span data-ttu-id="d00be-158">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.frontapp.com/sso/saml/callback`</span><span class="sxs-lookup"><span data-stu-id="d00be-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com/sso/saml/callback`</span></span>

4. <span data-ttu-id="d00be-159">Kontrollera **visa avancerade inställningar för URL: en**, om du inte vill tooconfigure hello programmet i **SP** initierade läge:</span><span class="sxs-lookup"><span data-stu-id="d00be-159">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_url2.png)

    <span data-ttu-id="d00be-161">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<companyname>.frontapp.com`</span><span class="sxs-lookup"><span data-stu-id="d00be-161">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.frontapp.com`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="d00be-162">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="d00be-162">These values are not real.</span></span> <span data-ttu-id="d00be-163">Uppdatera dessa värden med hello faktiska identifierare Reply URL och inloggnings-URL som beskrivs senare i självstudiekursen eller kontakta [främre klienten supportteamet](mailto:support@frontapp.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d00be-163">Update these values with hello actual Identifier, Reply URL, and Sign-On URL which are explained later in tutorial or contact [Front Client support team](mailto:support@frontapp.com) tooget these values.</span></span> 

5. <span data-ttu-id="d00be-164">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="d00be-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_certificate.png) 

6. <span data-ttu-id="d00be-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d00be-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="d00be-168">På hello **främre Configuration** klickar du på **konfigurera främre** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d00be-168">On hello **Front Configuration** section, click **Configure Front** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d00be-169">Kopiera hello **Sign-Out URL, SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d00be-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-front-tutorial/tutorial_front_configure.png) 

8. <span data-ttu-id="d00be-171">Inloggning tooyour framför innehavaren som administratör.</span><span class="sxs-lookup"><span data-stu-id="d00be-171">Sign-on tooyour Front tenant as an administrator.</span></span>

9. <span data-ttu-id="d00be-172">Gå för**inställningar (kugghjulet ikonen längst ned hello hello vänstra sidopanelen) > Inställningar**.</span><span class="sxs-lookup"><span data-stu-id="d00be-172">Go too**Settings (cog icon at hello bottom of hello left sidebar) > Preferences**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

10. <span data-ttu-id="d00be-174">Klicka på **enkel inloggning** länk.</span><span class="sxs-lookup"><span data-stu-id="d00be-174">Click **Single Sign On** link.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

11. <span data-ttu-id="d00be-176">Välj **SAML** i hello listrutan av **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d00be-176">Select **SAML** in hello drop-down list of **Single Sign On**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

12. <span data-ttu-id="d00be-178">I hello **startpunkten** textruta placera hello värdet för **inloggning tjänst-URL för enkel** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d00be-178">In hello **Entry Point** textbox put hello value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

13. <span data-ttu-id="d00be-180">Öppna din hämtade **Certificate(Base64)** filen i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **signeringscertifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="d00be-180">Open your downloaded **Certificate(Base64)** file in notepad, copy hello content of it into your clipboard, and then paste it toohello **Signing certificate** textbox.</span></span>
    
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

14. <span data-ttu-id="d00be-182">På hello **tjänstinställningar providern** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d00be-182">On hello **Service provider settings** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

    <span data-ttu-id="d00be-184">a.</span><span class="sxs-lookup"><span data-stu-id="d00be-184">a.</span></span> <span data-ttu-id="d00be-185">Kopiera hello värdet för **enhets-ID** och klistra in den i hello **identifierare** TextBox-kontroll i **främre domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d00be-185">Copy hello value of **Entity ID** and paste it into hello **Identifier** textbox in **Front Domain and URLs** section in Azure portal.</span></span>

    <span data-ttu-id="d00be-186">b.</span><span class="sxs-lookup"><span data-stu-id="d00be-186">b.</span></span> <span data-ttu-id="d00be-187">Kopiera hello värdet för **ACS URL** och klistra in den i hello **inloggnings-URL** TextBox-kontroll i **främre domän och URL: er** avsnitt i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d00be-187">Copy hello value of **ACS URL** and paste it into hello **Sign-on URL** textbox in **Front Domain and URLs** section in Azure portal.</span></span>
    
15. <span data-ttu-id="d00be-188">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d00be-188">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="d00be-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d00be-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d00be-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d00be-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d00be-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d00be-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d00be-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d00be-192">Create an Azure AD test user</span></span>

<span data-ttu-id="d00be-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d00be-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Skapa en testanvändare i Azure AD][100]

<span data-ttu-id="d00be-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d00be-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d00be-196">Klicka på hello i hello Azure-portalen hello vänster **Azure Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="d00be-196">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory-knappen](./media/active-directory-saas-front-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="d00be-198">toodisplay hello lista över användare, gå för**användare och grupper**, och klicka sedan på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d00be-198">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![Hej ”användare och grupper” och ”alla användare” länkar](./media/active-directory-saas-front-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="d00be-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** hello överst i hello **alla användare** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d00be-200">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello webbinställningar](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="d00be-202">I hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d00be-202">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello användardialogrutan](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

    <span data-ttu-id="d00be-204">a.</span><span class="sxs-lookup"><span data-stu-id="d00be-204">a.</span></span> <span data-ttu-id="d00be-205">I hello **namn** skriver **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d00be-205">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d00be-206">b.</span><span class="sxs-lookup"><span data-stu-id="d00be-206">b.</span></span> <span data-ttu-id="d00be-207">I hello **användarnamn** rutan typen hello användarens e-postadress Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d00be-207">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="d00be-208">c.</span><span class="sxs-lookup"><span data-stu-id="d00be-208">c.</span></span> <span data-ttu-id="d00be-209">Välj hello **visa lösenordet** kryssrutan och sedan skriva ned hello-värde som visas i hello **lösenord** rutan.</span><span class="sxs-lookup"><span data-stu-id="d00be-209">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="d00be-210">d.</span><span class="sxs-lookup"><span data-stu-id="d00be-210">d.</span></span> <span data-ttu-id="d00be-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d00be-211">Click **Create**.</span></span>
 
### <a name="create-a-front-test-user"></a><span data-ttu-id="d00be-212">Skapa en testanvändare framför</span><span class="sxs-lookup"><span data-stu-id="d00be-212">Create a Front test user</span></span>

<span data-ttu-id="d00be-213">I det här avsnittet skapar du en användare som kallas Britta Simon fram.</span><span class="sxs-lookup"><span data-stu-id="d00be-213">In this section, you create a user called Britta Simon in Front.</span></span> <span data-ttu-id="d00be-214">Arbeta med [främre klienten supportteamet](mailto:support@frontapp.com) att lägga till hello användare i hello framför plattform.</span><span class="sxs-lookup"><span data-stu-id="d00be-214">Work with [Front Client support team](mailto:support@frontapp.com) to add hello users in hello Front platform.</span></span> <span data-ttu-id="d00be-215">Användare måste skapas och aktiveras innan du använder enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d00be-215">Users must be created and activated before you use single sign-on.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="d00be-216">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d00be-216">Assign hello Azure AD test user</span></span>

<span data-ttu-id="d00be-217">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooFront.</span><span class="sxs-lookup"><span data-stu-id="d00be-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooFront.</span></span>

![Tilldela hello användarroll][200] 

<span data-ttu-id="d00be-219">**tooassign Britta Simon tooFront utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d00be-219">**tooassign Britta Simon tooFront, perform hello following steps:**</span></span>

1. <span data-ttu-id="d00be-220">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d00be-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d00be-222">Välj i listan med program hello **främre**.</span><span class="sxs-lookup"><span data-stu-id="d00be-222">In hello applications list, select **Front**.</span></span>

    ![hello framför länken i listan med program hello](./media/active-directory-saas-front-tutorial/tutorial_front_app.png)  

3. <span data-ttu-id="d00be-224">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d00be-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Hej ”användare och grupper” länk][202]

4. <span data-ttu-id="d00be-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d00be-226">Click **Add** button.</span></span> <span data-ttu-id="d00be-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d00be-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello Lägg uppdrag fönstret][203]

5. <span data-ttu-id="d00be-229">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d00be-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d00be-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d00be-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d00be-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d00be-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d00be-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d00be-232">Test single sign-on</span></span>

<span data-ttu-id="d00be-233">hello syftet med det här avsnittet är tootest din Azure AD SSOconfiguration med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d00be-233">hello objective of this section is tootest your Azure AD SSOconfiguration using hello Access Panel.</span></span>

<span data-ttu-id="d00be-234">Du bör få automatiskt inloggade tooyour framför programmet när du klickar på hello framför panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d00be-234">When you click hello Front tile in hello Access Panel, you should get automatically signed-on tooyour Front application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d00be-235">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d00be-235">Additional resources</span></span>

* [<span data-ttu-id="d00be-236">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d00be-236">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d00be-237">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d00be-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-front-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png

