---
title: "Självstudier: Azure Active Directory-integrering med anpassningsbar Suite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och anpassningsbar Suite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: af309c27ab74098c1e229c80adb11c96dc2774fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a><span data-ttu-id="d8430-103">Självstudier: Azure Active Directory-integrering med anpassningsbar Suite</span><span class="sxs-lookup"><span data-stu-id="d8430-103">Tutorial: Azure Active Directory integration with Adaptive Suite</span></span>

<span data-ttu-id="d8430-104">I kursen får du lära dig hur toointegrate anpassningsbar Suite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="d8430-104">In this tutorial, you learn how toointegrate Adaptive Suite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d8430-105">Integrera anpassningsbar Suite med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="d8430-105">Integrating Adaptive Suite with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d8430-106">Du kan styra i Azure AD som har åtkomst tooAdaptive Suite</span><span class="sxs-lookup"><span data-stu-id="d8430-106">You can control in Azure AD who has access tooAdaptive Suite</span></span>
- <span data-ttu-id="d8430-107">Du kan aktivera din användare tooautomatically get inloggade tooAdaptive Suite (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="d8430-107">You can enable your users tooautomatically get signed-on tooAdaptive Suite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d8430-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="d8430-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d8430-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d8430-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8430-110">Krav</span><span class="sxs-lookup"><span data-stu-id="d8430-110">Prerequisites</span></span>

<span data-ttu-id="d8430-111">tooconfigure Azure AD-integrering med anpassningsbar Suite måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="d8430-111">tooconfigure Azure AD integration with Adaptive Suite, you need hello following items:</span></span>

- <span data-ttu-id="d8430-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8430-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d8430-113">En anpassningsbar Suite enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="d8430-113">An Adaptive Suite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d8430-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8430-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d8430-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="d8430-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d8430-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="d8430-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d8430-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8430-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d8430-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="d8430-118">Scenario description</span></span>
<span data-ttu-id="d8430-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="d8430-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d8430-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8430-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d8430-121">Lägga till anpassad Suite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d8430-121">Adding Adaptive Suite from hello gallery</span></span>
2. <span data-ttu-id="d8430-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8430-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adaptive-suite-from-hello-gallery"></a><span data-ttu-id="d8430-123">Lägga till anpassad Suite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="d8430-123">Adding Adaptive Suite from hello gallery</span></span>
<span data-ttu-id="d8430-124">tooconfigure hello integrering av anpassningsbar Suite i Azure AD, behöver du tooadd anpassningsbar Suite hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="d8430-124">tooconfigure hello integration of Adaptive Suite into Azure AD, you need tooadd Adaptive Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d8430-125">**tooadd anpassningsbar Suite från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8430-125">**tooadd Adaptive Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8430-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8430-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d8430-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="d8430-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d8430-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8430-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="d8430-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8430-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="d8430-133">Skriv i sökrutan hello **anpassningsbar Suite**.</span><span class="sxs-lookup"><span data-stu-id="d8430-133">In hello search box, type **Adaptive Suite**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. <span data-ttu-id="d8430-135">Markera hello resultat på panelen **anpassningsbar Suite**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="d8430-135">In hello results panel, select **Adaptive Suite**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d8430-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8430-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d8430-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med anpassningsbar Suite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="d8430-138">In this section, you configure and test Azure AD single sign-on with Adaptive Suite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d8430-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i anpassningsbar Suite är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="d8430-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Adaptive Suite is tooa user in Azure AD.</span></span> <span data-ttu-id="d8430-140">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i anpassningsbar Suite toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="d8430-140">In other words, a link relationship between an Azure AD user and hello related user in Adaptive Suite needs toobe established.</span></span>

<span data-ttu-id="d8430-141">Tilldela hello värdet hello i anpassningsbar Suite **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="d8430-141">In Adaptive Suite, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d8430-142">tooconfigure och testa Azure AD enkel inloggning med anpassningsbar Suite, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="d8430-142">tooconfigure and test Azure AD single sign-on with Adaptive Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d8430-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="d8430-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d8430-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8430-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d8430-145">**[Skapa en anpassningsbar Suite testanvändare](#creating-an-adaptive-suite-test-user)**  -toohave en motsvarighet för Britta Simon i anpassningsbar Suite som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="d8430-145">**[Creating an Adaptive Suite test user](#creating-an-adaptive-suite-test-user)** - toohave a counterpart of Britta Simon in Adaptive Suite that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d8430-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8430-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d8430-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="d8430-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d8430-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8430-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d8430-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i anpassningsbar Suite-program.</span><span class="sxs-lookup"><span data-stu-id="d8430-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Adaptive Suite application.</span></span>

<span data-ttu-id="d8430-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med anpassningsbar Suite:**</span><span class="sxs-lookup"><span data-stu-id="d8430-150">**tooconfigure Azure AD single sign-on with Adaptive Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8430-151">I hello Azure-portalen på hello **anpassningsbar Suite** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="d8430-151">In hello Azure portal, on hello **Adaptive Suite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="d8430-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="d8430-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. <span data-ttu-id="d8430-155">På hello **anpassningsbar Suite domän och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8430-155">On hello **Adaptive Suite Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    <span data-ttu-id="d8430-157">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span><span class="sxs-lookup"><span data-stu-id="d8430-157">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span></span>

    >[!NOTE]
    > <span data-ttu-id="d8430-158">Du kan hämta värdet från hello anpassningsbar Suite **SAML SSO inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="d8430-158">You can get this value from hello Adaptive Suite’s **SAML SSO Settings** page.</span></span>
    >  

4. <span data-ttu-id="d8430-159">På hello **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="d8430-159">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. <span data-ttu-id="d8430-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8430-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d8430-163">På hello **anpassningsbar Suite Configuration** klickar du på **konfigurera anpassningsbar Suite** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="d8430-163">On hello **Adaptive Suite Configuration** section, click **Configure Adaptive Suite** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d8430-164">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="d8430-164">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. <span data-ttu-id="d8430-166">Logga in tooyour anpassningsbar Suite företagets webbplats som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="d8430-166">In a different web browser window, log in tooyour Adaptive Suite company site as an administrator.</span></span>

8. <span data-ttu-id="d8430-167">Gå för**Admin**.</span><span class="sxs-lookup"><span data-stu-id="d8430-167">Go too**Admin**.</span></span>
   
    <span data-ttu-id="d8430-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="d8430-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>

9. <span data-ttu-id="d8430-169">I hello **användare och roller** klickar du på **hantera inställningar för SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="d8430-169">In hello **Users and Roles** section, click **Manage SAML SSO Settings**.</span></span>
   
    <span data-ttu-id="d8430-170">![Hantera inställningar för SAML SSO](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "hantera SAML SSO-inställningar")</span><span class="sxs-lookup"><span data-stu-id="d8430-170">![Manage SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings")</span></span>

10. <span data-ttu-id="d8430-171">På hello **SAML SSO inställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8430-171">On hello **SAML SSO Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="d8430-172">![Inställningar för SAML SSO](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO-inställningar")</span><span class="sxs-lookup"><span data-stu-id="d8430-172">![SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO Settings")</span></span>

    <span data-ttu-id="d8430-173">a.</span><span class="sxs-lookup"><span data-stu-id="d8430-173">a.</span></span> <span data-ttu-id="d8430-174">I hello **identitet providernamn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d8430-174">In hello **Identity provider name** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="d8430-175">b.</span><span class="sxs-lookup"><span data-stu-id="d8430-175">b.</span></span> <span data-ttu-id="d8430-176">Klistra in hello **SAML enhets-ID** kopieras värdet från Azure-portalen till hello **identitetsleverantör enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="d8430-176">Paste hello **SAML Entity ID** value copied from Azure portal into hello **Identity provider Entity ID** textbox.</span></span>
  
    <span data-ttu-id="d8430-177">c.</span><span class="sxs-lookup"><span data-stu-id="d8430-177">c.</span></span> <span data-ttu-id="d8430-178">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** kopieras värdet från Azure-portalen till hello **identitetsleverantör SSO URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="d8430-178">Paste hello **SAML Single Sign-On Service URL** value copied from Azure portal into hello **Identity provider SSO URL** textbox.</span></span>
  
    <span data-ttu-id="d8430-179">d.</span><span class="sxs-lookup"><span data-stu-id="d8430-179">d.</span></span> <span data-ttu-id="d8430-180">Klistra in hello **SAML enkel inloggning Tjänstwebbadress** kopieras värdet från Azure-portalen till hello **anpassad logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="d8430-180">Paste hello **SAML Single Sign-On Service URL** value copied from Azure portal into hello **Custom logout URL** textbox.</span></span>
  
    <span data-ttu-id="d8430-181">e.</span><span class="sxs-lookup"><span data-stu-id="d8430-181">e.</span></span> <span data-ttu-id="d8430-182">tooupload hämtade certifikatet, klicka på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="d8430-182">tooupload your downloaded certificate, click **Choose file**.</span></span>
  
    <span data-ttu-id="d8430-183">f.</span><span class="sxs-lookup"><span data-stu-id="d8430-183">f.</span></span> <span data-ttu-id="d8430-184">Välj hello följande, för:</span><span class="sxs-lookup"><span data-stu-id="d8430-184">Select hello following, for:</span></span>
    * <span data-ttu-id="d8430-185">**Användar-id för SAML**väljer **användarens användarnamn för anpassningsbar insikter**.</span><span class="sxs-lookup"><span data-stu-id="d8430-185">**SAML user id**, select **User’s Adaptive Insights user name**.</span></span>
    * <span data-ttu-id="d8430-186">**Id för SAML Användarplats**väljer **användar-id i NameID ämne**.</span><span class="sxs-lookup"><span data-stu-id="d8430-186">**SAML user id location**, select **User id in NameID of Subject**.</span></span>
    * <span data-ttu-id="d8430-187">**Format för SAML-NameID**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="d8430-187">**SAML NameID format**, select **Email address**.</span></span>
    * <span data-ttu-id="d8430-188">**Aktivera SAML**väljer **Tillåt SAML SSO och anpassningsbar insikter direktinloggning**.</span><span class="sxs-lookup"><span data-stu-id="d8430-188">**Enable SAML**, select **Allow SAML SSO and direct Adaptive Insights login**.</span></span>
    
    <span data-ttu-id="d8430-189">g.</span><span class="sxs-lookup"><span data-stu-id="d8430-189">g.</span></span> <span data-ttu-id="d8430-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d8430-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d8430-191">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="d8430-191">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d8430-192">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="d8430-192">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d8430-193">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d8430-193">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d8430-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8430-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="d8430-195">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8430-195">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="d8430-197">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8430-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8430-198">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="d8430-198">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d8430-200">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="d8430-200">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d8430-202">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8430-202">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d8430-204">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8430-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d8430-206">a.</span><span class="sxs-lookup"><span data-stu-id="d8430-206">a.</span></span> <span data-ttu-id="d8430-207">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d8430-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d8430-208">b.</span><span class="sxs-lookup"><span data-stu-id="d8430-208">b.</span></span> <span data-ttu-id="d8430-209">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d8430-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d8430-210">c.</span><span class="sxs-lookup"><span data-stu-id="d8430-210">c.</span></span> <span data-ttu-id="d8430-211">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d8430-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d8430-212">d.</span><span class="sxs-lookup"><span data-stu-id="d8430-212">d.</span></span> <span data-ttu-id="d8430-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d8430-213">Click **Create**.</span></span>
 
### <a name="creating-an-adaptive-suite-test-user"></a><span data-ttu-id="d8430-214">Skapa en anpassningsbar Suite testanvändare</span><span class="sxs-lookup"><span data-stu-id="d8430-214">Creating an Adaptive Suite test user</span></span>

<span data-ttu-id="d8430-215">tooenable Azure AD-användare toolog i tooAdaptive Suite de att etableras i anpassningsbar Suite.</span><span class="sxs-lookup"><span data-stu-id="d8430-215">tooenable Azure AD users toolog in tooAdaptive Suite, they must be provisioned into Adaptive Suite.</span></span>  

* <span data-ttu-id="d8430-216">I hello fall av anpassningsbar Suite är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d8430-216">In hello case of Adaptive Suite, provisioning is a manual task.</span></span>

<span data-ttu-id="d8430-217">**tooconfigure användaretablering, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="d8430-217">**tooconfigure user provisioning, perform hello following steps:**</span></span> 

1. <span data-ttu-id="d8430-218">Logga in tooyour **anpassningsbar Suite** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="d8430-218">Log in tooyour **Adaptive Suite** company site as an administrator.</span></span>
2. <span data-ttu-id="d8430-219">Gå för**Admin**.</span><span class="sxs-lookup"><span data-stu-id="d8430-219">Go too**Admin**.</span></span>
   
   <span data-ttu-id="d8430-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="d8430-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>
3. <span data-ttu-id="d8430-221">I hello **användare och roller** klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="d8430-221">In hello **Users and Roles** section, click **Add User**.</span></span>
   
   <span data-ttu-id="d8430-222">![Lägg till användare](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="d8430-222">![Add User](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "Add User")</span></span>
4. <span data-ttu-id="d8430-223">I hello **ny användare** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d8430-223">In hello **New User** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="d8430-224">![Skicka](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "skicka")</span><span class="sxs-lookup"><span data-stu-id="d8430-224">![Submit](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "Submit")</span></span>   

   <span data-ttu-id="d8430-225">a.</span><span class="sxs-lookup"><span data-stu-id="d8430-225">a.</span></span> <span data-ttu-id="d8430-226">Typen hello **namn**, **inloggning**, **e-post**, **lösenord** en giltig Azure Active Directory-användare som du vill tooprovision till hello relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="d8430-226">Type hello **Name**, **Login**, **Email**, **Password** of a valid Azure Active Directory user you want tooprovision into hello related textboxes.</span></span>
  
   <span data-ttu-id="d8430-227">b.</span><span class="sxs-lookup"><span data-stu-id="d8430-227">b.</span></span> <span data-ttu-id="d8430-228">Välj en **rollen**.</span><span class="sxs-lookup"><span data-stu-id="d8430-228">Select a **Role**.</span></span>
  
   <span data-ttu-id="d8430-229">c.</span><span class="sxs-lookup"><span data-stu-id="d8430-229">c.</span></span> <span data-ttu-id="d8430-230">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="d8430-230">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="d8430-231">Du kan använda något annat anpassningsbar Suite användarens konto skapas verktyg eller API: er som tillhandahålls av anpassningsbar Suite tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="d8430-231">You can use any other Adaptive Suite user account creation tools or APIs provided by Adaptive Suite tooprovision AAD user accounts.</span></span>
>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d8430-232">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="d8430-232">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d8430-233">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooAdaptive Suite.</span><span class="sxs-lookup"><span data-stu-id="d8430-233">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAdaptive Suite.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="d8430-235">**tooassign Britta Simon tooAdaptive Suite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="d8430-235">**tooassign Britta Simon tooAdaptive Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8430-236">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="d8430-236">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="d8430-238">Välj i listan med program hello **anpassningsbar Suite**.</span><span class="sxs-lookup"><span data-stu-id="d8430-238">In hello applications list, select **Adaptive Suite**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. <span data-ttu-id="d8430-240">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="d8430-240">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="d8430-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="d8430-242">Click **Add** button.</span></span> <span data-ttu-id="d8430-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8430-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="d8430-245">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="d8430-245">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d8430-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8430-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d8430-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d8430-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d8430-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="d8430-248">Testing single sign-on</span></span>

<span data-ttu-id="d8430-249">hello syftet med det här avsnittet är tootest din Microsoft Azure AD enkel inloggning konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="d8430-249">hello objective of this section is tootest your Microsoft Azure AD Single Sign-On configuration using hello Access Panel.</span></span>

<span data-ttu-id="d8430-250">När du klickar på hello anpassningsbar Suite-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour anpassningsbar Suite-program.</span><span class="sxs-lookup"><span data-stu-id="d8430-250">When you click hello Adaptive Suite tile in hello Access Panel, you should get automatically signed-on tooyour Adaptive Suite application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="d8430-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d8430-251">Additional resources</span></span>

* [<span data-ttu-id="d8430-252">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d8430-252">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8430-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d8430-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

