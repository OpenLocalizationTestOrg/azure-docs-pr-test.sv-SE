---
title: "Självstudier: Azure Active Directory-integrering med Halogen programvara | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Halogen programvaran och Azure Active Directory."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2ca2298d-9a0c-4f14-925c-fa23f2659d28
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: bdb67de713771d6e306f287c4b13895f6336f7ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a><span data-ttu-id="3a886-103">Självstudier: Azure Active Directory-integrering med Halogen programvara</span><span class="sxs-lookup"><span data-stu-id="3a886-103">Tutorial: Azure Active Directory integration with Halogen Software</span></span>

<span data-ttu-id="3a886-104">I kursen får du lära dig hur toointegrate Halogen programvara med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3a886-104">In this tutorial, you learn how toointegrate Halogen Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3a886-105">Integrera Halogen programvara med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3a886-105">Integrating Halogen Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3a886-106">Du kan styra i Azure AD som har åtkomst tooHalogen programvara</span><span class="sxs-lookup"><span data-stu-id="3a886-106">You can control in Azure AD who has access tooHalogen Software</span></span>
- <span data-ttu-id="3a886-107">Du kan aktivera din användare tooautomatically get inloggade tooHalogen programvara (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3a886-107">You can enable your users tooautomatically get signed-on tooHalogen Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3a886-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3a886-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="3a886-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3a886-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3a886-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3a886-110">Prerequisites</span></span>

<span data-ttu-id="3a886-111">tooconfigure Azure AD-integrering med Halogen programvara, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3a886-111">tooconfigure Azure AD integration with Halogen Software, you need hello following items:</span></span>

- <span data-ttu-id="3a886-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3a886-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3a886-113">En Halogen programvara enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="3a886-113">A Halogen Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3a886-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3a886-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3a886-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3a886-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3a886-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3a886-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3a886-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3a886-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3a886-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3a886-118">Scenario description</span></span>

<span data-ttu-id="3a886-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3a886-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3a886-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3a886-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3a886-121">Lägga till Halogen programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3a886-121">Adding Halogen Software from hello gallery</span></span>
2. <span data-ttu-id="3a886-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3a886-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-halogen-software-from-hello-gallery"></a><span data-ttu-id="3a886-123">Lägga till Halogen programvara från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3a886-123">Adding Halogen Software from hello gallery</span></span>

<span data-ttu-id="3a886-124">tooconfigure hello integrering av Halogen programvara i Azure AD, behöver du tooadd Halogen programvara från hello galleriet tooyour lista över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3a886-124">tooconfigure hello integration of Halogen Software into Azure AD, you need tooadd Halogen Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3a886-125">**tooadd Halogen programvara från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3a886-125">**tooadd Halogen Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a886-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3a886-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3a886-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3a886-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3a886-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3a886-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3a886-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3a886-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3a886-133">Skriv i sökrutan hello **Halogen programvara**.</span><span class="sxs-lookup"><span data-stu-id="3a886-133">In hello search box, type **Halogen Software**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_search.png)

5. <span data-ttu-id="3a886-135">Markera hello resultat på panelen **Halogen programvara**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3a886-135">In hello results panel, select **Halogen Software**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3a886-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3a886-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3a886-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Halogen programvara baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3a886-138">In this section, you configure and test Azure AD single sign-on with Halogen Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3a886-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Halogen programvaran är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3a886-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Halogen Software is tooa user in Azure AD.</span></span> <span data-ttu-id="3a886-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Halogen programvara toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="3a886-140">In other words, a link relationship between an Azure AD user and hello related user in Halogen Software needs toobe established.</span></span>

<span data-ttu-id="3a886-141">Tilldela hello värdet hello Halogen programvaran **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="3a886-141">In Halogen Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="3a886-142">tooconfigure och testa Azure AD enkel inloggning med Halogen programvara, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3a886-142">tooconfigure and test Azure AD single sign-on with Halogen Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3a886-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3a886-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3a886-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3a886-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3a886-145">**[Skapa en testanvändare Halogen programvara](#creating-a-halogen-software-test-user)**  -toohave en motsvarighet för Britta Simon Halogen programvara som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="3a886-145">**[Creating a Halogen Software test user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in Halogen Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="3a886-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3a886-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3a886-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3a886-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3a886-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3a886-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3a886-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Halogen programmet.</span><span class="sxs-lookup"><span data-stu-id="3a886-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Halogen Software application.</span></span>

<span data-ttu-id="3a886-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Halogen programvara:**</span><span class="sxs-lookup"><span data-stu-id="3a886-150">**tooconfigure Azure AD single sign-on with Halogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a886-151">I hello Azure-portalen på hello **Halogen programvara** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3a886-151">In hello Azure portal, on hello **Halogen Software** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3a886-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3a886-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_samlbase.png)

3. <span data-ttu-id="3a886-155">På hello **Halogen programvara domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3a886-155">On hello **Halogen Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_url.png)

    <span data-ttu-id="3a886-157">a.</span><span class="sxs-lookup"><span data-stu-id="3a886-157">a.</span></span> <span data-ttu-id="3a886-158">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="3a886-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://global.hgncloud.com/<companyname>`</span></span>

    <span data-ttu-id="3a886-159">b.</span><span class="sxs-lookup"><span data-stu-id="3a886-159">b.</span></span> <span data-ttu-id="3a886-160">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret: `https://global.halogensoftware.com/<companyname>`,`https://global.hgncloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="3a886-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://global.halogensoftware.com/<companyname>`, `https://global.hgncloud.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3a886-161">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="3a886-161">These values are not real.</span></span> <span data-ttu-id="3a886-162">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="3a886-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3a886-163">Kontakta [Halogen Programvaruklienten supportteamet](https://support.halogensoftware.com/) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3a886-163">Contact [Halogen Software Client support team](https://support.halogensoftware.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="3a886-164">På hello **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan hello metadata på datorn.</span><span class="sxs-lookup"><span data-stu-id="3a886-164">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_certificate.png) 

5. <span data-ttu-id="3a886-166">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3a886-166">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3a886-168">I ett annat webbläsarfönster inloggning tooyour **Halogen programvara** program som administratör.</span><span class="sxs-lookup"><span data-stu-id="3a886-168">In a different browser window, sign-on tooyour **Halogen Software** application as an administrator.</span></span>

7. <span data-ttu-id="3a886-169">Klicka på hello **alternativ** fliken.</span><span class="sxs-lookup"><span data-stu-id="3a886-169">Click hello **Options** tab.</span></span> 
   
    ![Vad är Azure AD Connect?][12]

8. <span data-ttu-id="3a886-171">Hello vänstra navigeringsfönstret, klicka på **SAML-konfiguration**.</span><span class="sxs-lookup"><span data-stu-id="3a886-171">In hello left navigation pane, click **SAML Configuration**.</span></span> 
   
    ![Vad är Azure AD Connect?][13]

9. <span data-ttu-id="3a886-173">På hello **SAML-konfiguration** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3a886-173">On hello **SAML Configuration** page, perform hello following steps:</span></span> 

    ![Vad är Azure AD Connect?][14]

     <span data-ttu-id="3a886-175">a.</span><span class="sxs-lookup"><span data-stu-id="3a886-175">a.</span></span> <span data-ttu-id="3a886-176">Som **Unik identifierare**väljer **NameID**.</span><span class="sxs-lookup"><span data-stu-id="3a886-176">As **Unique Identifier**, select **NameID**.</span></span>

     <span data-ttu-id="3a886-177">b.</span><span class="sxs-lookup"><span data-stu-id="3a886-177">b.</span></span> <span data-ttu-id="3a886-178">Som **unika identifierare Maps till**väljer **användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="3a886-178">As **Unique Identifier Maps To**, select **Username**.</span></span>
  
     <span data-ttu-id="3a886-179">c.</span><span class="sxs-lookup"><span data-stu-id="3a886-179">c.</span></span> <span data-ttu-id="3a886-180">tooupload metadata för hämtade filer, klickar du på **Bläddra** tooselect hello-fil, och sedan **överför filen**.</span><span class="sxs-lookup"><span data-stu-id="3a886-180">tooupload your downloaded metadata file, click **Browse** tooselect hello file, and then **Upload File**.</span></span>
 
     <span data-ttu-id="3a886-181">d.</span><span class="sxs-lookup"><span data-stu-id="3a886-181">d.</span></span> <span data-ttu-id="3a886-182">tootest hello konfiguration, klickar du på **kör Test**.</span><span class="sxs-lookup"><span data-stu-id="3a886-182">tootest hello configuration, click **Run Test**.</span></span> 
    
    >[!NOTE]
    ><span data-ttu-id="3a886-183">Du behöver toowait för hello-meddelande ”*hello SAML test har slutförts. Stäng det här fönstret*”.</span><span class="sxs-lookup"><span data-stu-id="3a886-183">You need toowait for hello message "*hello SAML test is complete. Please close this window*".</span></span> <span data-ttu-id="3a886-184">Stäng hello öppnade webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="3a886-184">Then, close hello opened browser window.</span></span> <span data-ttu-id="3a886-185">Hej **aktivera SAML** kryssrutan är endast aktiverad om hello test har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3a886-185">hello **Enable SAML** checkbox is only enabled if hello test has been completed.</span></span> 
     
     <span data-ttu-id="3a886-186">e.</span><span class="sxs-lookup"><span data-stu-id="3a886-186">e.</span></span> <span data-ttu-id="3a886-187">Välj **aktivera SAML**.</span><span class="sxs-lookup"><span data-stu-id="3a886-187">Select **Enable SAML**.</span></span>
    
     <span data-ttu-id="3a886-188">f.</span><span class="sxs-lookup"><span data-stu-id="3a886-188">f.</span></span> <span data-ttu-id="3a886-189">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="3a886-189">Click **Save Changes**.</span></span> 

> [!TIP]
> <span data-ttu-id="3a886-190">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="3a886-190">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="3a886-191">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="3a886-191">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="3a886-192">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3a886-192">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3a886-193">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3a886-193">Creating an Azure AD test user</span></span>

<span data-ttu-id="3a886-194">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3a886-194">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3a886-196">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3a886-196">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a886-197">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3a886-197">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3a886-199">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="3a886-199">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3a886-201">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3a886-201">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3a886-203">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3a886-203">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-halogen-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3a886-205">a.</span><span class="sxs-lookup"><span data-stu-id="3a886-205">a.</span></span> <span data-ttu-id="3a886-206">I hello **namn** textruta typnamn som **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3a886-206">In hello **Name** textbox, type name as **BrittaSimon**.</span></span>

    <span data-ttu-id="3a886-207">b.</span><span class="sxs-lookup"><span data-stu-id="3a886-207">b.</span></span> <span data-ttu-id="3a886-208">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3a886-208">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3a886-209">c.</span><span class="sxs-lookup"><span data-stu-id="3a886-209">c.</span></span> <span data-ttu-id="3a886-210">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3a886-210">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3a886-211">d.</span><span class="sxs-lookup"><span data-stu-id="3a886-211">d.</span></span> <span data-ttu-id="3a886-212">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3a886-212">Click **Create**.</span></span>
 
### <a name="creating-a-halogen-software-test-user"></a><span data-ttu-id="3a886-213">Skapa en testanvändare Halogen programvara</span><span class="sxs-lookup"><span data-stu-id="3a886-213">Creating a Halogen Software test user</span></span>

<span data-ttu-id="3a886-214">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon Halogen programvaran.</span><span class="sxs-lookup"><span data-stu-id="3a886-214">hello objective of this section is toocreate a user called Britta Simon in Halogen Software.</span></span>

<span data-ttu-id="3a886-215">**toocreate en användare som kallas Britta Simon i Halogen programvara, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3a886-215">**toocreate a user called Britta Simon in Halogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a886-216">Logga in tooyour **Halogen programvara** program som administratör.</span><span class="sxs-lookup"><span data-stu-id="3a886-216">Sign on tooyour **Halogen Software** application as an administrator.</span></span>

2. <span data-ttu-id="3a886-217">Klicka på hello **användaren Center** fliken och klicka sedan på **skapa användare**.</span><span class="sxs-lookup"><span data-stu-id="3a886-217">Click hello **User Center** tab, and then click **Create User**.</span></span>
   
    ![Vad är Azure AD Connect?][300]  

3. <span data-ttu-id="3a886-219">På hello **ny användare** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3a886-219">On hello **New User** dialog page, perform hello following steps:</span></span>
   
    ![Vad är Azure AD Connect?][301]

    <span data-ttu-id="3a886-221">a.</span><span class="sxs-lookup"><span data-stu-id="3a886-221">a.</span></span> <span data-ttu-id="3a886-222">I hello **Förnamn** textruta Ange först namnet på hello användaren som **Britta**.</span><span class="sxs-lookup"><span data-stu-id="3a886-222">In hello **First Name** textbox, type first name of hello user like **Britta**.</span></span>
    
    <span data-ttu-id="3a886-223">b.</span><span class="sxs-lookup"><span data-stu-id="3a886-223">b.</span></span> <span data-ttu-id="3a886-224">I hello **efternamn** textruta Skriv Efternamn för hello användare som **Simon**.</span><span class="sxs-lookup"><span data-stu-id="3a886-224">In hello **Last Name** textbox, type last name of hello user like **Simon**.</span></span> 

    <span data-ttu-id="3a886-225">c.</span><span class="sxs-lookup"><span data-stu-id="3a886-225">c.</span></span> <span data-ttu-id="3a886-226">I hello **användarnamn** textruta typen **Britta Simon**, hello användarnamn som hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3a886-226">In hello **Username** textbox, type **Britta Simon**, hello user name as in hello Azure portal.</span></span>

    <span data-ttu-id="3a886-227">d.</span><span class="sxs-lookup"><span data-stu-id="3a886-227">d.</span></span> <span data-ttu-id="3a886-228">I hello **lösenord** textruta, ange ett lösenord för Britta.</span><span class="sxs-lookup"><span data-stu-id="3a886-228">In hello **Password** textbox, type a password for Britta.</span></span>
    
    <span data-ttu-id="3a886-229">e.</span><span class="sxs-lookup"><span data-stu-id="3a886-229">e.</span></span> <span data-ttu-id="3a886-230">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3a886-230">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3a886-231">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3a886-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3a886-232">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooHalogen programvara.</span><span class="sxs-lookup"><span data-stu-id="3a886-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooHalogen Software.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3a886-234">**tooassign Britta Simon tooHalogen programvara, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3a886-234">**tooassign Britta Simon tooHalogen Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="3a886-235">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3a886-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3a886-237">Välj i listan med program hello **Halogen programvara**.</span><span class="sxs-lookup"><span data-stu-id="3a886-237">In hello applications list, select **Halogen Software**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-halogen-software-tutorial/tutorial_halogensoftware_app.png) 

3. <span data-ttu-id="3a886-239">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3a886-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3a886-241">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3a886-241">Click **Add** button.</span></span> <span data-ttu-id="3a886-242">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3a886-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3a886-244">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3a886-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3a886-245">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3a886-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3a886-246">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3a886-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3a886-247">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3a886-247">Testing single sign-on</span></span>

<span data-ttu-id="3a886-248">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3a886-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="3a886-249">Du bör få automatiskt inloggade tooyour Halogen program när du klickar på hello Halogen programvara panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3a886-249">When you click hello Halogen Software tile in hello Access Panel, you should get automatically signed-on tooyour Halogen Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3a886-250">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3a886-250">Additional resources</span></span>

* [<span data-ttu-id="3a886-251">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3a886-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3a886-252">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3a886-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_04.png

[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png

[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png

[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png

[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_general_203.png

[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png

[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png
