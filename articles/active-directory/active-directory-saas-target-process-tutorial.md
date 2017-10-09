---
title: "Självstudier: Azure Active Directory-integrering med TargetProcess | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och TargetProcess."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 05c574e2c18d7f73edc6c094093a6e59d46b8e6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a><span data-ttu-id="327c7-103">Självstudier: Azure Active Directory-integrering med TargetProcess</span><span class="sxs-lookup"><span data-stu-id="327c7-103">Tutorial: Azure Active Directory integration with TargetProcess</span></span>

<span data-ttu-id="327c7-104">I kursen får du lära dig hur toointegrate TargetProcess med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="327c7-104">In this tutorial, you learn how toointegrate TargetProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="327c7-105">Integrera TargetProcess med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="327c7-105">Integrating TargetProcess with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="327c7-106">Du kan styra i Azure AD som har åtkomst till tooTargetProcess</span><span class="sxs-lookup"><span data-stu-id="327c7-106">You can control in Azure AD who has access tooTargetProcess</span></span>
- <span data-ttu-id="327c7-107">Du kan aktivera din användare tooautomatically get inloggade tooTargetProcess (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="327c7-107">You can enable your users tooautomatically get signed-on tooTargetProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="327c7-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="327c7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="327c7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="327c7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="327c7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="327c7-110">Prerequisites</span></span>

<span data-ttu-id="327c7-111">tooconfigure Azure AD-integrering med TargetProcess, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="327c7-111">tooconfigure Azure AD integration with TargetProcess, you need hello following items:</span></span>

- <span data-ttu-id="327c7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="327c7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="327c7-113">En TargetProcess enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="327c7-113">A TargetProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="327c7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="327c7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="327c7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="327c7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="327c7-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="327c7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="327c7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du [hämta en utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="327c7-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="327c7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="327c7-118">Scenario description</span></span>
<span data-ttu-id="327c7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="327c7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="327c7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="327c7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="327c7-121">Lägg till TargetProcess från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="327c7-121">Add TargetProcess from hello gallery</span></span>
2. <span data-ttu-id="327c7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="327c7-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-targetprocess-from-hello-gallery"></a><span data-ttu-id="327c7-123">Lägg till TargetProcess från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="327c7-123">Add TargetProcess from hello gallery</span></span>
<span data-ttu-id="327c7-124">tooconfigure hello integrering av TargetProcess i Azure AD, behöver du tooadd TargetProcess hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="327c7-124">tooconfigure hello integration of TargetProcess into Azure AD, you need tooadd TargetProcess from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="327c7-125">**tooadd TargetProcess från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="327c7-125">**tooadd TargetProcess from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="327c7-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="327c7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="327c7-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="327c7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="327c7-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="327c7-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="327c7-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="327c7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="327c7-133">Skriv i sökrutan hello **TargetProcess**väljer **TargetProcess** resultatet-panelen klickar **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="327c7-133">In hello search box, type **TargetProcess**, select **TargetProcess**  from result panel then click **Add** button tooadd hello application.</span></span>

    ![Lägg till TargetProcess från galleriet](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="327c7-135">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="327c7-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="327c7-136">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med TargetProcess baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="327c7-136">In this section, you configure and test Azure AD single sign-on with TargetProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="327c7-137">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i TargetProcess är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="327c7-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in TargetProcess is tooa user in Azure AD.</span></span> <span data-ttu-id="327c7-138">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i TargetProcess toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="327c7-138">In other words, a link relationship between an Azure AD user and hello related user in TargetProcess needs toobe established.</span></span>

<span data-ttu-id="327c7-139">I TargetProcess, tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="327c7-139">In TargetProcess, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="327c7-140">tooconfigure och testa Azure AD enkel inloggning med TargetProcess, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="327c7-140">tooconfigure and test Azure AD single sign-on with TargetProcess, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="327c7-141">**[Konfigurera Azure AD enkel inloggning](#configure-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="327c7-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="327c7-142">**[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="327c7-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="327c7-143">**[Skapa en testanvändare TargetProcess](#create-a-targetprocess-test-user)**  -toohave en motsvarighet för Britta Simon i TargetProcess som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="327c7-143">**[Create a TargetProcess test user](#create-a-targetprocess-test-user)** - toohave a counterpart of Britta Simon in TargetProcess that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="327c7-144">**[Tilldela hello Azure AD-testanvändare](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="327c7-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="327c7-145">**[Testa enkel inloggning](#test-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="327c7-145">**[Test Single Sign-On](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="327c7-146">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="327c7-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="327c7-147">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i ditt TargetProcess program.</span><span class="sxs-lookup"><span data-stu-id="327c7-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your TargetProcess application.</span></span>

<span data-ttu-id="327c7-148">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med TargetProcess:**</span><span class="sxs-lookup"><span data-stu-id="327c7-148">**tooconfigure Azure AD single sign-on with TargetProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="327c7-149">I hello Azure-portalen på hello **TargetProcess** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="327c7-149">In hello Azure portal, on hello **TargetProcess** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="327c7-151">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="327c7-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![SAML-baserade inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. <span data-ttu-id="327c7-153">På hello **TargetProcess domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="327c7-153">On hello **TargetProcess Domain and URLs** section, perform hello following steps:</span></span>

    ![Avsnittet TargetProcess domän och URL: er](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    <span data-ttu-id="327c7-155">a.</span><span class="sxs-lookup"><span data-stu-id="327c7-155">a.</span></span> <span data-ttu-id="327c7-156">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="327c7-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    <span data-ttu-id="327c7-157">b.</span><span class="sxs-lookup"><span data-stu-id="327c7-157">b.</span></span> <span data-ttu-id="327c7-158">I hello **identifierare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<subdomain>.tpondemand.com/`</span><span class="sxs-lookup"><span data-stu-id="327c7-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.tpondemand.com/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="327c7-159">Dessa värden är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="327c7-159">These values are not real.</span></span> <span data-ttu-id="327c7-160">Uppdatera dessa värden med hello faktiska inloggnings-URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="327c7-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="327c7-161">Kontakta [TargetProcess klienten supportteamet](mailto:support@targetprocess.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="327c7-161">Contact [TargetProcess Client support team](mailto:support@targetprocess.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="327c7-162">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="327c7-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Signeringscertifikat för SAML-avsnitt](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. <span data-ttu-id="327c7-164">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="327c7-164">Click **Save** button.</span></span>

    ![Knappen Spara](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="327c7-166">På hello **TargetProcess Configuration** klickar du på **konfigurera TargetProcess** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="327c7-166">On hello **TargetProcess Configuration** section, click **Configure TargetProcess** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="327c7-167">Kopiera hello **SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="327c7-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![TargetProcess konfigurationsavsnitt](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. <span data-ttu-id="327c7-169">Inloggning tooyour TargetProcess programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="327c7-169">Sign-on tooyour TargetProcess application as an administrator.</span></span>

8. <span data-ttu-id="327c7-170">Hello-menyn överst hello **installationsprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="327c7-170">In hello menu on hello top, click **Setup**.</span></span>
   
    ![Konfiguration](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. <span data-ttu-id="327c7-172">Klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="327c7-172">Click **Settings**.</span></span>
   
    ![Inställningar](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. <span data-ttu-id="327c7-174">Klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="327c7-174">Click **Single Sign-on**.</span></span>
   
    ![Klicka på enkel inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. <span data-ttu-id="327c7-176">På hello enkel inloggning dialogrutan Inställningar, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="327c7-176">On hello Single Sign-on settings dialog, perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    <span data-ttu-id="327c7-178">a.</span><span class="sxs-lookup"><span data-stu-id="327c7-178">a.</span></span> <span data-ttu-id="327c7-179">Klicka på **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="327c7-179">Click **Enable Single Sign-on**.</span></span>
    
    <span data-ttu-id="327c7-180">b.</span><span class="sxs-lookup"><span data-stu-id="327c7-180">b.</span></span> <span data-ttu-id="327c7-181">I **inloggnings-URL** textruta klistra in hello värdet för **SAML inloggning tjänst-URL för enkel** som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="327c7-181">In **Sign-on URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="327c7-182">c.</span><span class="sxs-lookup"><span data-stu-id="327c7-182">c.</span></span> <span data-ttu-id="327c7-183">Öppna din hämtat certifikat i anteckningar, kopiera hello innehåll, och klistra in den i hello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="327c7-183">Open your downloaded certificate in notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span>
    
    <span data-ttu-id="327c7-184">d.</span><span class="sxs-lookup"><span data-stu-id="327c7-184">d.</span></span> <span data-ttu-id="327c7-185">Klicka på **aktivera JIT etablering**.</span><span class="sxs-lookup"><span data-stu-id="327c7-185">click **Enable JIT Provisioning**.</span></span>

    <span data-ttu-id="327c7-186">e.</span><span class="sxs-lookup"><span data-stu-id="327c7-186">e.</span></span> <span data-ttu-id="327c7-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="327c7-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="327c7-188">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="327c7-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="327c7-189">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="327c7-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="327c7-190">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="327c7-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="327c7-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="327c7-191">Create an Azure AD test user</span></span>
<span data-ttu-id="327c7-192">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="327c7-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="327c7-194">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="327c7-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="327c7-195">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="327c7-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="327c7-197">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="327c7-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![toodisplay hello lista över användare](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="327c7-199">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="327c7-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Knappen Lägg till](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="327c7-201">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="327c7-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Avsnittet för användare](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="327c7-203">a.</span><span class="sxs-lookup"><span data-stu-id="327c7-203">a.</span></span> <span data-ttu-id="327c7-204">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="327c7-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="327c7-205">b.</span><span class="sxs-lookup"><span data-stu-id="327c7-205">b.</span></span> <span data-ttu-id="327c7-206">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="327c7-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="327c7-207">c.</span><span class="sxs-lookup"><span data-stu-id="327c7-207">c.</span></span> <span data-ttu-id="327c7-208">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="327c7-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="327c7-209">d.</span><span class="sxs-lookup"><span data-stu-id="327c7-209">d.</span></span> <span data-ttu-id="327c7-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="327c7-210">Click **Create**.</span></span>
 
### <a name="create-a-targetprocess-test-user"></a><span data-ttu-id="327c7-211">Skapa en testanvändare TargetProcess</span><span class="sxs-lookup"><span data-stu-id="327c7-211">Create a TargetProcess test user</span></span>

<span data-ttu-id="327c7-212">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i TargetProcess.</span><span class="sxs-lookup"><span data-stu-id="327c7-212">hello objective of this section is toocreate a user called Britta Simon in TargetProcess.</span></span>

<span data-ttu-id="327c7-213">TargetProcess stöder just-in-time-etablering.</span><span class="sxs-lookup"><span data-stu-id="327c7-213">TargetProcess supports just-in-time provisioning.</span></span> <span data-ttu-id="327c7-214">Du har redan aktiverats i [konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="327c7-214">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="327c7-215">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="327c7-215">There is no action item for you in this section.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="327c7-216">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="327c7-216">Assign hello Azure AD test user</span></span>

<span data-ttu-id="327c7-217">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooTargetProcess.</span><span class="sxs-lookup"><span data-stu-id="327c7-217">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooTargetProcess.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="327c7-219">**tooassign Britta Simon tooTargetProcess utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="327c7-219">**tooassign Britta Simon tooTargetProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="327c7-220">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="327c7-220">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="327c7-222">Välj i listan med program hello **TargetProcess**.</span><span class="sxs-lookup"><span data-stu-id="327c7-222">In hello applications list, select **TargetProcess**.</span></span>

    ![TargetProcess i listan över appar](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. <span data-ttu-id="327c7-224">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="327c7-224">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="327c7-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="327c7-226">Click **Add** button.</span></span> <span data-ttu-id="327c7-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="327c7-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="327c7-229">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="327c7-229">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="327c7-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="327c7-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="327c7-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="327c7-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="327c7-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="327c7-232">Test single sign-on</span></span>

<span data-ttu-id="327c7-233">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="327c7-233">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="327c7-234">Du bör få automatiskt inloggade tooyour TargetProcess programmet när du klickar på hello TargetProcess panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="327c7-234">When you click hello TargetProcess tile in hello Access Panel, you should get automatically signed-on tooyour TargetProcess application.</span></span> <span data-ttu-id="327c7-235">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="327c7-235">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="327c7-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="327c7-236">Additional resources</span></span>

* [<span data-ttu-id="327c7-237">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="327c7-237">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="327c7-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="327c7-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png

