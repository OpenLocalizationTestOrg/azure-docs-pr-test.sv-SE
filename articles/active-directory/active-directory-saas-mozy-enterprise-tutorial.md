---
title: "Självstudier: Azure Active Directory-integrering med Mozy Enterprise | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Mozy Enterprise."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.openlocfilehash: bab0df4f3621b784cd8edfda3c8e10fe5a7ced9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a><span data-ttu-id="4e6b6-103">Självstudier: Azure Active Directory-integrering med Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="4e6b6-103">Tutorial: Azure Active Directory integration with Mozy Enterprise</span></span>

<span data-ttu-id="4e6b6-104">I kursen får du lära dig hur toointegrate Mozy Enterprise med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="4e6b6-104">In this tutorial, you learn how toointegrate Mozy Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="4e6b6-105">Integrera Mozy Enterprise med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-105">Integrating Mozy Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4e6b6-106">Du kan styra i Azure AD som har åtkomst tooMozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="4e6b6-106">You can control in Azure AD who has access tooMozy Enterprise</span></span>
- <span data-ttu-id="4e6b6-107">Du kan aktivera din användare tooautomatically get inloggade tooMozy Enterprise (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="4e6b6-107">You can enable your users tooautomatically get signed-on tooMozy Enterprise (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="4e6b6-108">Du kan hantera dina konton i en central plats - hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4e6b6-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="4e6b6-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4e6b6-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e6b6-110">Krav</span><span class="sxs-lookup"><span data-stu-id="4e6b6-110">Prerequisites</span></span>

<span data-ttu-id="4e6b6-111">tooconfigure Azure AD-integrering med Mozy Enterprise, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-111">tooconfigure Azure AD integration with Mozy Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="4e6b6-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="4e6b6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="4e6b6-113">En Mozy Enterprise enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="4e6b6-113">A Mozy Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="4e6b6-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="4e6b6-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4e6b6-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="4e6b6-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4e6b6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4e6b6-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="4e6b6-118">Scenario description</span></span>
<span data-ttu-id="4e6b6-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="4e6b6-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4e6b6-121">Att lägga till Mozy Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4e6b6-121">Adding Mozy Enterprise from hello gallery</span></span>
2. <span data-ttu-id="4e6b6-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e6b6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mozy-enterprise-from-hello-gallery"></a><span data-ttu-id="4e6b6-123">Att lägga till Mozy Enterprise från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="4e6b6-123">Adding Mozy Enterprise from hello gallery</span></span>
<span data-ttu-id="4e6b6-124">tooconfigure hello integrering av Mozy Enterprise i Azure AD, behöver du tooadd Mozy Enterprise hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-124">tooconfigure hello integration of Mozy Enterprise into Azure AD, you need tooadd Mozy Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4e6b6-125">**tooadd Mozy Enterprise från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e6b6-125">**tooadd Mozy Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e6b6-126">I hello  **[Azure-portalen](https://portal.azure.com)**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4e6b6-128">Navigera för**företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4e6b6-129">Gå sedan för**alla program**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-129">Then go too**All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="4e6b6-131">tooadd nya program, klickar du på **nytt program** hello längst upp i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="4e6b6-133">Skriv i sökrutan hello **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-133">In hello search box, type **Mozy Enterprise**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. <span data-ttu-id="4e6b6-135">Markera hello resultat på panelen **Mozy Enterprise**, och klicka sedan på **Lägg till** knappen tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-135">In hello results panel, select **Mozy Enterprise**, and then click **Add** button tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="4e6b6-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e6b6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="4e6b6-138">Du konfigurera och testa Azure AD enkel inloggning med Mozy Enterprise baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-138">In this section, you configure and test Azure AD single sign-on with Mozy Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4e6b6-139">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Mozy företag är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mozy Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="4e6b6-140">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Mozy Enterprise toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-140">In other words, a link relationship between an Azure AD user and hello related user in Mozy Enterprise needs toobe established.</span></span>

<span data-ttu-id="4e6b6-141">Tilldela hello värdet för hello i Mozy Enterprise **användarnamn** i Azure AD som hello värde för hello **användarnamn** tooestablish hello länken relationen.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-141">In Mozy Enterprise, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="4e6b6-142">tooconfigure och testa Azure AD enkel inloggning med Mozy Enterprise, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-142">tooconfigure and test Azure AD single sign-on with Mozy Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4e6b6-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4e6b6-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4e6b6-145">**[Skapa en testanvändare Mozy Enterprise](#creating-a-mozy-enterprise-test-user)**  -toohave en motsvarighet för Britta Simon i Mozy företag som är länkade toohello Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-145">**[Creating a Mozy Enterprise test user](#creating-a-mozy-enterprise-test-user)** - toohave a counterpart of Britta Simon in Mozy Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="4e6b6-146">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4e6b6-147">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="4e6b6-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e6b6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="4e6b6-149">I det här avsnittet Aktivera Azure AD enkel inloggning i hello Azure-portalen och konfigurera enkel inloggning i Mozy Enterprise-programmet.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mozy Enterprise application.</span></span>

<span data-ttu-id="4e6b6-150">**Utför följande hello tooconfigure Azure AD enkel inloggning med Mozy Enterprise:**</span><span class="sxs-lookup"><span data-stu-id="4e6b6-150">**tooconfigure Azure AD single sign-on with Mozy Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e6b6-151">I hello Azure-portalen på hello **Mozy Enterprise** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-151">In hello Azure portal, on hello **Mozy Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="4e6b6-153">På hello **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** tooenable enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. <span data-ttu-id="4e6b6-155">På hello **Mozy företagsdomänen och URL: er** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-155">On hello **Mozy Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    <span data-ttu-id="4e6b6-157">I hello **inloggnings-URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<tenantname>.Mozyenterprise.com`</span><span class="sxs-lookup"><span data-stu-id="4e6b6-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenantname>.Mozyenterprise.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4e6b6-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-158">This value is not real.</span></span> <span data-ttu-id="4e6b6-159">Uppdatera det här värdet med hello faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="4e6b6-160">Kontakta [Mozy Enterprise-klienten supportteamet](http://support.mozy.com/) tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-160">Contact [Mozy Enterprise Client support team](http://support.mozy.com/) tooget this value.</span></span>

4. <span data-ttu-id="4e6b6-161">På hello **SAML-signeringscertifikat** klickar du på **Certificate(Base64)** och spara sedan hello certifikat på datorn.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. <span data-ttu-id="4e6b6-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="4e6b6-165">På hello **Mozy företagskonfiguration** klickar du på **konfigurera Mozy Enterprise** tooopen **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-165">On hello **Mozy Enterprise Configuration** section, click **Configure Mozy Enterprise** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="4e6b6-166">Kopiera hello **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från hello **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="4e6b6-166">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. <span data-ttu-id="4e6b6-168">Logga in på webbplatsen Mozy Enterprise företag som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-168">In a different web browser window, log into your Mozy Enterprise company site as an administrator.</span></span>

8. <span data-ttu-id="4e6b6-169">I hello **Configuration** klickar du på **autentiseringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-169">In hello **Configuration** section, click **Authentication Policy**.</span></span>
   
   <span data-ttu-id="4e6b6-170">![Autentiseringsprincip](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "autentiseringsprincip")</span><span class="sxs-lookup"><span data-stu-id="4e6b6-170">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "Authentication policy")</span></span>

9. <span data-ttu-id="4e6b6-171">På hello **autentiseringsprincip** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-171">On hello **Authentication Policy** section, perform hello following steps:</span></span>
   
   <span data-ttu-id="4e6b6-172">![Autentiseringsprincip](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "autentiseringsprincip")</span><span class="sxs-lookup"><span data-stu-id="4e6b6-172">![Authentication policy](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "Authentication policy")</span></span>
   
   <span data-ttu-id="4e6b6-173">a.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-173">a.</span></span> <span data-ttu-id="4e6b6-174">Välj **katalogtjänsten** som **Provider**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-174">Select **Directory Service** as **Provider**.</span></span>
   
   <span data-ttu-id="4e6b6-175">b.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-175">b.</span></span> <span data-ttu-id="4e6b6-176">Välj **använder LDAP Push**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-176">Select **Use LDAP Push**.</span></span>
   
   <span data-ttu-id="4e6b6-177">c.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-177">c.</span></span> <span data-ttu-id="4e6b6-178">Klicka på hello **SAML-autentisering** fliken.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-178">Click hello **SAML Authentication** tab.</span></span>
   
   <span data-ttu-id="4e6b6-179">d.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-179">d.</span></span> <span data-ttu-id="4e6b6-180">Klistra in **SAML enkel inloggning Tjänstwebbadress**, som du har kopierat från hello Azure-portalen i hello **autentiserings-URL för** textruta.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-180">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Authentication URL** textbox.</span></span>
   
   <span data-ttu-id="4e6b6-181">e.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-181">e.</span></span> <span data-ttu-id="4e6b6-182">Klistra in **SAML enhets-ID**, som du har kopierat från hello Azure-portalen i hello **SAML Endpoint** textruta.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-182">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **SAML Endpoint** textbox.</span></span>
   
   <span data-ttu-id="4e6b6-183">f.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-183">f.</span></span> <span data-ttu-id="4e6b6-184">Öppna din hämtade Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in hello hela certifikatet till **SAML certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-184">Open your downloaded base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste hello entire Certificate into **SAML Certificate** textbox.</span></span>
   
   <span data-ttu-id="4e6b6-185">g.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-185">g.</span></span> <span data-ttu-id="4e6b6-186">Välj **aktivera enkel inloggning för administratörer toolog in med sina nätverksinloggningsuppgifter**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-186">Select **Enable SSO for Admins toolog in with their network credentials**.</span></span>
   
   <span data-ttu-id="4e6b6-187">h.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-187">h.</span></span> <span data-ttu-id="4e6b6-188">Klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-188">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="4e6b6-189">Du kan nu läsa en kortare version av dessa anvisningar i hello [Azure-portalen](https://portal.azure.com), medan du ställer in hello appen!</span><span class="sxs-lookup"><span data-stu-id="4e6b6-189">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="4e6b6-190">När du lägger till den här appen från hello **Active Directory > företagsprogram** avsnittet, klicka bara på hello **enkel inloggning** flik och åtkomst hello inbäddade dokumentationen via hello  **Konfigurationen** avsnittet längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-190">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="4e6b6-191">Du kan läsa mer om hello inbäddade dokumentationen funktionen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="4e6b6-191">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="4e6b6-192">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e6b6-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="4e6b6-193">hello syftet med det här avsnittet är toocreate en testanvändare i hello Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-193">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="4e6b6-195">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e6b6-195">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e6b6-196">I hello **Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-196">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4e6b6-198">toodisplay hello lista över användare, gå för**användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-198">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4e6b6-200">tooopen hello **användare** dialogrutan klickar du på **Lägg till** på hello överkant hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-200">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4e6b6-202">På hello **användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-202">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="4e6b6-204">a.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-204">a.</span></span> <span data-ttu-id="4e6b6-205">I hello **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-205">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="4e6b6-206">b.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-206">b.</span></span> <span data-ttu-id="4e6b6-207">I hello **användarnamn** textruta typen hello **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-207">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="4e6b6-208">c.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-208">c.</span></span> <span data-ttu-id="4e6b6-209">Välj **visa lösenordet** och Skriv ned hello värdet för hello **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-209">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="4e6b6-210">d.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-210">d.</span></span> <span data-ttu-id="4e6b6-211">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-211">Click **Create**.</span></span>
 
### <a name="creating-a-mozy-enterprise-test-user"></a><span data-ttu-id="4e6b6-212">Skapa en testanvändare Mozy Enterprise</span><span class="sxs-lookup"><span data-stu-id="4e6b6-212">Creating a Mozy Enterprise test user</span></span>

<span data-ttu-id="4e6b6-213">I ordning tooenable Azure AD-användare toolog i Mozy Enterprise, måste de etableras i Mozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-213">In order tooenable Azure AD users toolog into Mozy Enterprise, they must be provisioned into Mozy Enterprise.</span></span> <span data-ttu-id="4e6b6-214">Hello gäller Mozy Enterprise är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-214">In hello case of Mozy Enterprise, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="4e6b6-215">Du kan använda något annat Mozy Enterprise användarens konto skapas verktyg eller API: er som tillhandahålls av Mozy Enterprise tooprovision AAD användarkonton.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-215">You can use any other Mozy Enterprise user account creation tools or APIs provided by Mozy Enterprise tooprovision AAD user accounts.</span></span>

<span data-ttu-id="4e6b6-216">**tooprovision användarkonton, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="4e6b6-216">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e6b6-217">Logga in tooyour **Mozy Enterprise** klient.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-217">Log in tooyour **Mozy Enterprise** tenant.</span></span>

2. <span data-ttu-id="4e6b6-218">Klicka på **användare**, och klicka sedan på **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-218">Click **Users**, and then click **Add New User**.</span></span>
   
   <span data-ttu-id="4e6b6-219">![Användare](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "användare")</span><span class="sxs-lookup"><span data-stu-id="4e6b6-219">![Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "Users")</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="4e6b6-220">Hej **Lägg till nya användare** alternativet visas bara om **Mozy** är markerad som hello provider under **autentiseringsprincip**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-220">hello **Add New User** option is only displayed only if **Mozy** is selected as hello provider under **Authentication policy**.</span></span> <span data-ttu-id="4e6b6-221">Om SAML-autentisering är konfigurerad, sedan hello användare läggs till automatiskt på sin första inloggning via enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-221">If SAML Authentication is configured, then hello users are added automatically on their first login through Single sign on.</span></span>
    
3. <span data-ttu-id="4e6b6-222">Utför följande hello på hello nya användardialogrutan:</span><span class="sxs-lookup"><span data-stu-id="4e6b6-222">On hello new user dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="4e6b6-223">![Lägg till användare](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="4e6b6-223">![Add Users](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "Add Users")</span></span>
   
   <span data-ttu-id="4e6b6-224">a.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-224">a.</span></span> <span data-ttu-id="4e6b6-225">Från hello **väljer du en grupp** väljer du en grupp.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-225">From hello **Choose a Group** list, select a group.</span></span>
   
   <span data-ttu-id="4e6b6-226">b.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-226">b.</span></span> <span data-ttu-id="4e6b6-227">Från hello **vilken typ av användare** väljer du en typ.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-227">From hello **What type of user** list, select a type.</span></span>
   
   <span data-ttu-id="4e6b6-228">c.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-228">c.</span></span> <span data-ttu-id="4e6b6-229">I hello **användarnamn** textruta hello-typnamn för hello Azure AD-användare.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-229">In hello **Username** textbox, type hello name of hello Azure AD user.</span></span>
   
   <span data-ttu-id="4e6b6-230">d.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-230">d.</span></span> <span data-ttu-id="4e6b6-231">I hello **e-post** textruta typen hello användarens e-postadress hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-231">In hello **Email** textbox, type hello email address of hello Azure AD user.</span></span>
   
   <span data-ttu-id="4e6b6-232">e.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-232">e.</span></span> <span data-ttu-id="4e6b6-233">Välj **skicka e-post för användaren instruktion**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-233">Select **Send user instruction email**.</span></span>
   
   <span data-ttu-id="4e6b6-234">f.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-234">f.</span></span> <span data-ttu-id="4e6b6-235">Klicka på **lägga till användare**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-235">Click **Add User(s)**.</span></span>

     >[!NOTE]
     > <span data-ttu-id="4e6b6-236">När du har skapat hello användaren skickas ett e-postmeddelande toohello Azure AD-användare som innehåller en länk tooconfirm hello-konto innan den aktiveras.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-236">After creating hello user, an email will be sent toohello Azure AD user that includes a link tooconfirm hello account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="4e6b6-237">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4e6b6-237">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="4e6b6-238">I det här avsnittet kan aktivera du Britta Simon toouse Azure enkel inloggning genom att bevilja åtkomst tooMozy Enterprise.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-238">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMozy Enterprise.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="4e6b6-240">**tooassign Britta Simon tooMozy Enterprise, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="4e6b6-240">**tooassign Britta Simon tooMozy Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="4e6b6-241">I hello Azure-portalen, öppna hello program visa och navigera toohello directory vy och gå för**företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-241">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="4e6b6-243">Välj i listan med program hello **Mozy Enterprise**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-243">In hello applications list, select **Mozy Enterprise**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. <span data-ttu-id="4e6b6-245">Hello-menyn hello vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-245">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="4e6b6-247">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-247">Click **Add** button.</span></span> <span data-ttu-id="4e6b6-248">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-248">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="4e6b6-250">På **användare och grupper** markerar **Britta Simon** i hello användarlistan.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-250">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4e6b6-251">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-251">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4e6b6-252">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-252">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="4e6b6-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="4e6b6-253">Testing single sign-on</span></span>

<span data-ttu-id="4e6b6-254">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-254">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="4e6b6-255">När du klickar på hello Mozy Enterprise-panelen i hello åtkomstpanelen bör du hämta inloggningssidan Mozy affärsprogram.</span><span class="sxs-lookup"><span data-stu-id="4e6b6-255">When you click hello Mozy Enterprise tile in hello Access Panel, you should get login page of Mozy Enterprise application.</span></span>
<span data-ttu-id="4e6b6-256">Läs mer om hello åtkomstpanelen [introduktion toohello åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4e6b6-256">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e6b6-257">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="4e6b6-257">Additional resources</span></span>

* [<span data-ttu-id="4e6b6-258">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4e6b6-258">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4e6b6-259">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="4e6b6-259">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png

