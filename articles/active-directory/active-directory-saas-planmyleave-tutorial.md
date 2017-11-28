---
title: "Självstudier: Azure Active Directory-integrering med PlanMyLeave | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: 44a6782e44ef22fc957544960be1742f9eed6e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a><span data-ttu-id="3dfe3-103">Självstudier: Azure Active Directory-integrering med PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="3dfe3-103">Tutorial: Azure Active Directory integration with PlanMyLeave</span></span>

<span data-ttu-id="3dfe3-104">I kursen får du lära dig hur toointegrate PlanMyLeave med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-104">In this tutorial, you learn how toointegrate PlanMyLeave with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3dfe3-105">Integrera PlanMyLeave med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-105">Integrating PlanMyLeave with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="3dfe3-106">Du kan styra i Azure AD som har åtkomst till tooPlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="3dfe3-106">You can control in Azure AD who has access tooPlanMyLeave</span></span>
- <span data-ttu-id="3dfe3-107">Du kan aktivera din användare tooautomatically get inloggade tooPlanMyLeave (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="3dfe3-107">You can enable your users tooautomatically get signed-on tooPlanMyLeave (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3dfe3-108">Du kan hantera dina konton i en central plats - hello Azure Management portal</span><span class="sxs-lookup"><span data-stu-id="3dfe3-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="3dfe3-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3dfe3-110">Krav</span><span class="sxs-lookup"><span data-stu-id="3dfe3-110">Prerequisites</span></span>

<span data-ttu-id="3dfe3-111">tooconfigure Azure AD-integrering med PlanMyLeave, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-111">tooconfigure Azure AD integration with PlanMyLeave, you need hello following items:</span></span>

- <span data-ttu-id="3dfe3-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3dfe3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3dfe3-113">En PlanMyLeave enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="3dfe3-113">A PlanMyLeave single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="3dfe3-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="3dfe3-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3dfe3-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="3dfe3-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="3dfe3-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="3dfe3-118">Scenario description</span></span>
<span data-ttu-id="3dfe3-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3dfe3-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3dfe3-121">Att lägga till PlanMyLeave från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3dfe3-121">Adding PlanMyLeave from hello gallery</span></span>
2. <span data-ttu-id="3dfe3-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3dfe3-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-planmyleave-from-hello-gallery"></a><span data-ttu-id="3dfe3-123">Att lägga till PlanMyLeave från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="3dfe3-123">Adding PlanMyLeave from hello gallery</span></span>
<span data-ttu-id="3dfe3-124">tooconfigure hello integrering av PlanMyLeave i Azure AD, behöver du tooadd PlanMyLeave hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-124">tooconfigure hello integration of PlanMyLeave into Azure AD, you need tooadd PlanMyLeave from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="3dfe3-125">**tooadd PlanMyLeave från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3dfe3-125">**tooadd PlanMyLeave from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="3dfe3-126">I hello  **[Azure-hanteringsportalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3dfe3-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="3dfe3-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="3dfe3-131">Klicka på **Lägg till** hello längst upp i hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="3dfe3-133">Skriv i sökrutan hello **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-133">In hello search box, type **PlanMyLeave**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. <span data-ttu-id="3dfe3-135">Markera hello resultat på panelen **PlanMyLeave**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-135">In hello results panel, select **PlanMyLeave**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3dfe3-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3dfe3-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3dfe3-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PlanMyLeave baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-138">In this section, you configure and test Azure AD single sign-on with PlanMyLeave based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3dfe3-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i PlanMyLeave är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PlanMyLeave is tooa user in Azure AD.</span></span> <span data-ttu-id="3dfe3-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i PlanMyLeave toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-140">In other words, a link relationship between an Azure AD user and hello related user in PlanMyLeave needs toobe established.</span></span>

<span data-ttu-id="3dfe3-141">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in PlanMyLeave.</span></span>

<span data-ttu-id="3dfe3-142">tooconfigure och testa Azure AD enkel inloggning med PlanMyLeave, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-142">tooconfigure and test Azure AD single sign-on with PlanMyLeave, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="3dfe3-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="3dfe3-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3dfe3-145">**[Skapa en testanvändare PlanMyLeave](#creating-a-planmyleave-test-user)**  -toohave en motsvarighet för Britta Simon i PlanMyLeave som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-145">**[Creating a PlanMyLeave test user](#creating-a-planmyleave-test-user)** - toohave a counterpart of Britta Simon in PlanMyLeave that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="3dfe3-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3dfe3-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3dfe3-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3dfe3-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3dfe3-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure Management portal och konfigurera enkel inloggning i ditt PlanMyLeave program.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your PlanMyLeave application.</span></span>

<span data-ttu-id="3dfe3-150">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med PlanMyLeave:**</span><span class="sxs-lookup"><span data-stu-id="3dfe3-150">**tooconfigure Azure AD single sign-on with PlanMyLeave, perform hello following steps:**</span></span>

1. <span data-ttu-id="3dfe3-151">I hello Azure Management portal på hello **PlanMyLeave** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-151">In hello Azure Management portal, on hello **PlanMyLeave** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="3dfe3-153">På hello **enkel inloggning** dialogrutan sida som **läge** Välj **SAML-baserade inloggning** tooenable för enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-153">On hello **Single sign-on** dialog page, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. <span data-ttu-id="3dfe3-155">På hello **PlanMyLeave domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-155">On hello **PlanMyLeave Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    <span data-ttu-id="3dfe3-157">a.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-157">a.</span></span> <span data-ttu-id="3dfe3-158">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company-name>.planmyleave.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="3dfe3-158">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company-name>.planmyleave.com/Login.aspx`</span></span>
    
    <span data-ttu-id="3dfe3-159">b.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-159">b.</span></span> <span data-ttu-id="3dfe3-160">I hello **Identiferare** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company-name>.planmyleave.com`</span><span class="sxs-lookup"><span data-stu-id="3dfe3-160">In hello **Identifer** textbox, type a URL using hello following pattern: `https://<company-name>.planmyleave.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3dfe3-161">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="3dfe3-162">Du har tooupdate dessa värden med hello faktiska inloggning URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-162">You have tooupdate these values with hello actual Sign On URL and Identifier.</span></span> <span data-ttu-id="3dfe3-163">Kontakta [PlanMyLeave supportteamet](mailto:support@planmyleave.com) tooget dessa värden.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-163">Contact [PlanMyLeave support team](mailto:support@planmyleave.com) tooget these values.</span></span>

4. <span data-ttu-id="3dfe3-164">På hello **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-164">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. <span data-ttu-id="3dfe3-166">På hello **skapa nya certifikat** dialogrutan Klicka hello kalendern och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-166">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="3dfe3-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-167">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="3dfe3-169">På hello **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-169">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. <span data-ttu-id="3dfe3-171">På popup-fönster hello **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-171">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="3dfe3-173">På hello **SAML-signeringscertifikat** klickar du på **certifikat (base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-173">On hello **SAML Signing Certificate** section, click **Certificate (base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. <span data-ttu-id="3dfe3-175">På hello **PlanMyLeave Configuration** klickar du på **konfigurera PlanMyLeave** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-175">On hello **PlanMyLeave Configuration** section, click **Configure PlanMyLeave** tooopen **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. <span data-ttu-id="3dfe3-178">Logga in på din PlanMyLeave klient som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-178">In a different web browser window, log into your PlanMyLeave tenant as an administrator.</span></span>

11. <span data-ttu-id="3dfe3-179">Gå för**systeminställningarna**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-179">Go too**System Setup**.</span></span> <span data-ttu-id="3dfe3-180">På hello **säkerhetshantering** Klicka på avsnittet **företagets SAML inställningar** .</span><span class="sxs-lookup"><span data-stu-id="3dfe3-180">Then on hello **Security Management** section click **Company SAML settings** .</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. <span data-ttu-id="3dfe3-182">På hello **SAML inställningar** klickar du på ikonen för redigeraren.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-182">On hello **SAML Settings** section, click editor icon.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. <span data-ttu-id="3dfe3-184">På hello **SAML uppdateringsinställningar** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-184">On hello **Update SAML Settings** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    <span data-ttu-id="3dfe3-186">a.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-186">a.</span></span>  <span data-ttu-id="3dfe3-187">I hello **inloggnings-URL** textruta placera hello värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-187">In hello **Login URL** textbox, put hello value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="3dfe3-188">b.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-188">b.</span></span>  <span data-ttu-id="3dfe3-189">Öppna filen hämtat certifikat i anteckningar, kopiera endast hello mellan hello---börjar certifikat--- och---slut certifikat---det till Urklipp, och klistra in den toohello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-189">Open your downloaded certificate file in notepad, copy only hello content between hello ---Begin Certificate--- and ---End certificate---- of it into your clipboard, and then paste it toohello **Certificate** textbox.</span></span>

    <span data-ttu-id="3dfe3-190">c.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-190">c.</span></span> <span data-ttu-id="3dfe3-191">Ange ”**har aktiverats**” för ”**Ja**”.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-191">Set "**Is Enable**" too"**Yes**".</span></span>

    <span data-ttu-id="3dfe3-192">d.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-192">d.</span></span> <span data-ttu-id="3dfe3-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-193">Click **Save**.</span></span>



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3dfe3-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="3dfe3-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="3dfe3-195">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-195">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="3dfe3-197">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3dfe3-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="3dfe3-198">I hello **Azure-hanteringsportalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-198">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3dfe3-200">Gå för**användare och grupper** och på **alla användare** toodisplay hello lista över användare.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-200">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3dfe3-202">Hello överst i dialogrutan hello klickar du på **Lägg till** tooopen hello **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-202">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3dfe3-204">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3dfe3-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3dfe3-206">a.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-206">a.</span></span> <span data-ttu-id="3dfe3-207">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3dfe3-208">b.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-208">b.</span></span> <span data-ttu-id="3dfe3-209">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3dfe3-210">c.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-210">c.</span></span> <span data-ttu-id="3dfe3-211">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="3dfe3-212">d.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-212">d.</span></span> <span data-ttu-id="3dfe3-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-213">Click **Create**.</span></span> 



### <a name="creating-a-planmyleave-test-user"></a><span data-ttu-id="3dfe3-214">Skapa en testanvändare PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="3dfe3-214">Creating a PlanMyLeave test user</span></span>

<span data-ttu-id="3dfe3-215">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-215">hello objective of this section is toocreate a user called Britta Simon in PlanMyLeave.</span></span> <span data-ttu-id="3dfe3-216">PlanMyLeave stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-216">PlanMyLeave supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="3dfe3-217">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-217">There is no action item for you in this section.</span></span> <span data-ttu-id="3dfe3-218">En ny användare skapas under ett försök tooaccess PlanMyLeave om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-218">A new user will be created during an attempt tooaccess PlanMyLeave if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="3dfe3-219">Om du behöver toocreate en användare manuellt, måste toocontact [PlanMyLeave supportteamet](mailto:support@planmyleave.com).</span><span class="sxs-lookup"><span data-stu-id="3dfe3-219">If you need toocreate an user manually, you need toocontact [PlanMyLeave support team](mailto:support@planmyleave.com).</span></span>



### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="3dfe3-220">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="3dfe3-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="3dfe3-221">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooPlanMyLeave sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooPlanMyLeave.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="3dfe3-223">**tooassign Britta Simon tooPlanMyLeave utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="3dfe3-223">**tooassign Britta Simon tooPlanMyLeave, perform hello following steps:**</span></span>

1. <span data-ttu-id="3dfe3-224">I hello Azure Management portal öppnar du hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-224">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="3dfe3-226">Välj i listan med program hello **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-226">In hello applications list, select **PlanMyLeave**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. <span data-ttu-id="3dfe3-228">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="3dfe3-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-230">Click **Add** button.</span></span> <span data-ttu-id="3dfe3-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="3dfe3-233">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="3dfe3-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3dfe3-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="3dfe3-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="3dfe3-236">Testing single sign-on</span></span>

<span data-ttu-id="3dfe3-237">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="3dfe3-238">Du bör få automatiskt inloggade tooyour PlanMyLeave programmet när du klickar på hello PlanMyLeave panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="3dfe3-238">When you click hello PlanMyLeave tile in hello Access Panel, you should get automatically signed-on tooyour PlanMyLeave application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="3dfe3-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3dfe3-239">Additional resources</span></span>

* [<span data-ttu-id="3dfe3-240">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3dfe3-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3dfe3-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="3dfe3-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png