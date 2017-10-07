---
title: "Självstudier: Azure Active Directory-integrering med SuccessFactors | Microsoft Docs"
description: "Lär dig hur toouse SuccessFactors med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 3f7895d7d5e26fda27f555ae2f14a1645b50dcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a><span data-ttu-id="a856a-103">Självstudier: Azure Active Directory-integrering med SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="a856a-103">Tutorial: Azure Active Directory integration with SuccessFactors</span></span>
<span data-ttu-id="a856a-104">hello syftet med den här kursen är tooshow du hur toointegrate SuccessFactors med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a856a-104">hello objective of this tutorial is tooshow you how toointegrate SuccessFactors with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a856a-105">Integrera SuccessFactors med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="a856a-105">Integrating SuccessFactors with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="a856a-106">Du kan styra i Azure AD som har åtkomst till tooSuccessFactors</span><span class="sxs-lookup"><span data-stu-id="a856a-106">You can control in Azure AD who has access tooSuccessFactors</span></span>
* <span data-ttu-id="a856a-107">Du kan aktivera din användare tooautomatically get inloggade tooSuccessFactors (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="a856a-107">You can enable your users tooautomatically get signed-on tooSuccessFactors (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="a856a-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a856a-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="a856a-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a856a-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a856a-110">Krav</span><span class="sxs-lookup"><span data-stu-id="a856a-110">Prerequisites</span></span>
<span data-ttu-id="a856a-111">tooconfigure Azure AD-integrering med SuccessFactors, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="a856a-111">tooconfigure Azure AD integration with SuccessFactors, you need hello following items:</span></span>

* <span data-ttu-id="a856a-112">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="a856a-112">A valid Azure subscription</span></span>
* <span data-ttu-id="a856a-113">En klient i SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="a856a-113">A tenant in SuccessFactors</span></span>

> [!NOTE]
> <span data-ttu-id="a856a-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a856a-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="a856a-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="a856a-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="a856a-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="a856a-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="a856a-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a856a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a856a-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="a856a-118">Scenario description</span></span>
<span data-ttu-id="a856a-119">hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="a856a-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="a856a-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="a856a-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a856a-121">Att lägga till SuccessFactors från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a856a-121">Adding SuccessFactors from hello gallery</span></span>
2. <span data-ttu-id="a856a-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a856a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-successfactors-from-hello-gallery"></a><span data-ttu-id="a856a-123">Att lägga till SuccessFactors från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="a856a-123">Adding SuccessFactors from hello gallery</span></span>
<span data-ttu-id="a856a-124">tooconfigure hello integrering av SuccessFactors i Azure AD, behöver du tooadd SuccessFactors hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="a856a-124">tooconfigure hello integration of SuccessFactors into Azure AD, you need tooadd SuccessFactors from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a856a-125">**tooadd SuccessFactors från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a856a-125">**tooadd SuccessFactors from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a856a-126">I hello klassiska Azure-portalen på hello vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a856a-126">In hello Azure classic portal, on hello left navigation panel, click **Active Directory**.</span></span>
   
    ![Konfigurera enkel inloggning][1]
2. <span data-ttu-id="a856a-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="a856a-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="a856a-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="a856a-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Konfigurera enkel inloggning][2]
4. <span data-ttu-id="a856a-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="a856a-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3]
5. <span data-ttu-id="a856a-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="a856a-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Konfigurera enkel inloggning][4]
6. <span data-ttu-id="a856a-135">I hello **sökrutan**, typen **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="a856a-135">In hello **search box**, type **SuccessFactors**.</span></span>
   
    ![Konfigurera enkel inloggning][5]
7. <span data-ttu-id="a856a-137">Markera hello resultat på panelen **SuccessFactors**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="a856a-137">In hello results panel, select **SuccessFactors**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Konfigurera enkel inloggning][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a856a-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a856a-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a856a-140">hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med SuccessFactors baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="a856a-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with SuccessFactors based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a856a-141">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SuccessFactors tooan användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a856a-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SuccessFactors tooan user in Azure AD is.</span></span> <span data-ttu-id="a856a-142">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SuccessFactors toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="a856a-142">In other words, a link relationship between an Azure AD user and hello related user in SuccessFactors needs toobe established.</span></span>

<span data-ttu-id="a856a-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="a856a-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SuccessFactors.</span></span>

<span data-ttu-id="a856a-144">tooconfigure och testa Azure AD enkel inloggning med SuccessFactors, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="a856a-144">tooconfigure and test Azure AD single sign-on with SuccessFactors, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a856a-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on) ** -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="a856a-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a856a-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user) ** -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a856a-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a856a-147">**[Skapa en testanvändare SuccessFactors](#creating-a-successfactors-test-user) ** -toohave en motsvarighet för Britta Simon i SuccessFactors som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="a856a-147">**[Creating a SuccessFactors test user](#creating-a-successfactors-test-user)** - toohave a counterpart of Britta Simon in SuccessFactors that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="a856a-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user) ** -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="a856a-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a856a-149">**[Testa enkel inloggning](#testing-single-sign-on) ** -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="a856a-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a856a-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a856a-150">Configuring Azure AD single sign-on</span></span>
<span data-ttu-id="a856a-151">I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i ditt SuccessFactors program.</span><span class="sxs-lookup"><span data-stu-id="a856a-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your SuccessFactors application.</span></span>

<span data-ttu-id="a856a-152">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med SuccessFactors:**</span><span class="sxs-lookup"><span data-stu-id="a856a-152">**tooconfigure Azure AD single sign-on with SuccessFactors, perform hello following steps:**</span></span>

1. <span data-ttu-id="a856a-153">I hello klassiska Azure-portalen på hello **SuccessFactors** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a856a-153">In hello Azure classic portal, on hello **SuccessFactors** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    ![Konfigurera enkel inloggning][7]
2. <span data-ttu-id="a856a-155">På hello **hur skulle du som användare toosign på tooSuccessFactors** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a856a-155">On hello **How would you like users toosign on tooSuccessFactors** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurera enkel inloggning][8]
3. <span data-ttu-id="a856a-157">På hello **konfigurera App-URL** , utför följande steg hello, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a856a-157">On hello **Configure App URL** page, perform hello following steps, and then click **Next**.</span></span>
   
    ![Konfigurera enkel inloggning][9]
   
    <span data-ttu-id="a856a-159">a.</span><span class="sxs-lookup"><span data-stu-id="a856a-159">a.</span></span> <span data-ttu-id="a856a-160">I hello **logga URL** textruta, ange ett URL-Adressen med något av följande mönster hello:</span><span class="sxs-lookup"><span data-stu-id="a856a-160">In hello **Sign On URL** textbox, type a URL using one of hello following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    <span data-ttu-id="a856a-161">b.</span><span class="sxs-lookup"><span data-stu-id="a856a-161">b.</span></span> <span data-ttu-id="a856a-162">I hello **Reply URL** textruta, ange ett URL-Adressen med något av följande mönster hello:</span><span class="sxs-lookup"><span data-stu-id="a856a-162">In hello **Reply URL** textbox, type a URL using one of hello following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    <span data-ttu-id="a856a-163">c.</span><span class="sxs-lookup"><span data-stu-id="a856a-163">c.</span></span> <span data-ttu-id="a856a-164">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a856a-164">Click **Next**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="a856a-165">Observera att detta inte är hello verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="a856a-165">Please note that these are not hello real values.</span></span> <span data-ttu-id="a856a-166">Du har tooupdate dessa värden med hello faktiska logga URL och svars-URL.</span><span class="sxs-lookup"><span data-stu-id="a856a-166">You have tooupdate these values with hello actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="a856a-167">tooget dessa värden, kontakta [SuccessFactors supportteam](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="a856a-167">tooget these values, contact [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

1. <span data-ttu-id="a856a-168">På hello **Konfigurera enkel inloggning på SuccessFactors** klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="a856a-168">On hello **Configure single sign-on at SuccessFactors** page, click **Download certificate**, and then save hello certificate file locally on your computer.</span></span>
   
    ![Konfigurera enkel inloggning][10]

2. <span data-ttu-id="a856a-170">Logga in i ett annat webbläsarfönster din **SuccessFactors administrationsportalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="a856a-170">In a different web browser window, log into your **SuccessFactors admin portal** as an administrator.</span></span>

3. <span data-ttu-id="a856a-171">Besök **programsäkerhet** och egna för**enkel inloggning på funktionen**.</span><span class="sxs-lookup"><span data-stu-id="a856a-171">Visit **Application Security** and native too**Single Sign On Feature**.</span></span> 

4. <span data-ttu-id="a856a-172">Placera alla värden i hello **återställa Token** och på **spara Token** tooenable SAML SSO.</span><span class="sxs-lookup"><span data-stu-id="a856a-172">Place any value in hello **Reset Token** and click **Save Token** tooenable SAML SSO.</span></span>
   
    ![Konfigurera enkel inloggning på app-sida][11]

    > [!NOTE] 
    > <span data-ttu-id="a856a-174">Det här värdet används bara som hello strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="a856a-174">This value is just used as hello on/off switch.</span></span> <span data-ttu-id="a856a-175">Om inget värde har sparats är hello SAML SSO Aktiverat.</span><span class="sxs-lookup"><span data-stu-id="a856a-175">If any value is saved, hello SAML SSO is ON.</span></span> <span data-ttu-id="a856a-176">Om ett tomt värde har sparats är hello SAML SSO OFF.</span><span class="sxs-lookup"><span data-stu-id="a856a-176">If a blank value is saved hello SAML SSO is OFF.</span></span>

1. <span data-ttu-id="a856a-177">Skärmbild av interna toobelow och utför följande åtgärder hello.</span><span class="sxs-lookup"><span data-stu-id="a856a-177">Native toobelow screenshot and perform hello following actions.</span></span>
   
    ![Konfigurera enkel inloggning på app-sida][12]
   
    <span data-ttu-id="a856a-179">a.</span><span class="sxs-lookup"><span data-stu-id="a856a-179">a.</span></span> <span data-ttu-id="a856a-180">Välj hello **SAML v2 SSO** alternativknapp</span><span class="sxs-lookup"><span data-stu-id="a856a-180">Select hello **SAML v2 SSO** Radio Button</span></span>
   
    <span data-ttu-id="a856a-181">b.</span><span class="sxs-lookup"><span data-stu-id="a856a-181">b.</span></span> <span data-ttu-id="a856a-182">Ange hello SAML garanterar part Name(e.g. SAml issuer + company name).</span><span class="sxs-lookup"><span data-stu-id="a856a-182">Set hello SAML Asserting Party Name(e.g. SAml issuer + company name).</span></span>
   
    <span data-ttu-id="a856a-183">c.</span><span class="sxs-lookup"><span data-stu-id="a856a-183">c.</span></span> <span data-ttu-id="a856a-184">I hello **SAML utfärdaren** textruta placera hello värdet för **utfärdar-URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a856a-184">In hello **SAML Issuer** textbox put hello value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="a856a-185">d.</span><span class="sxs-lookup"><span data-stu-id="a856a-185">d.</span></span> <span data-ttu-id="a856a-186">Välj **svar (kunden genereras/IdP/AP)** som **kräver obligatorisk signatur**.</span><span class="sxs-lookup"><span data-stu-id="a856a-186">Select **Response(Customer Generated/IdP/AP)** as **Require Mandatory Signature**.</span></span>
   
    <span data-ttu-id="a856a-187">e.</span><span class="sxs-lookup"><span data-stu-id="a856a-187">e.</span></span> <span data-ttu-id="a856a-188">Välj **aktiverat** som **aktivera SAML-flaggan**.</span><span class="sxs-lookup"><span data-stu-id="a856a-188">Select **Enabled** as **Enable SAML Flag**.</span></span>
   
    <span data-ttu-id="a856a-189">f.</span><span class="sxs-lookup"><span data-stu-id="a856a-189">f.</span></span> <span data-ttu-id="a856a-190">Välj **nr** som **signatur på förfrågan inloggning (SA genereras/SP/RP)**.</span><span class="sxs-lookup"><span data-stu-id="a856a-190">Select **No** as **Login Request Signature(SF Generated/SP/RP)**.</span></span>
   
    <span data-ttu-id="a856a-191">g.</span><span class="sxs-lookup"><span data-stu-id="a856a-191">g.</span></span> <span data-ttu-id="a856a-192">Välj **webbläsare/Post-profilen** som **SAML profil**.</span><span class="sxs-lookup"><span data-stu-id="a856a-192">Select **Browser/Post Profile** as **SAML Profile**.</span></span>
   
    <span data-ttu-id="a856a-193">h.</span><span class="sxs-lookup"><span data-stu-id="a856a-193">h.</span></span> <span data-ttu-id="a856a-194">Välj **nr** som **genomdriva giltighetsperioden för certifikatet**.</span><span class="sxs-lookup"><span data-stu-id="a856a-194">Select **No** as **Enforce Certificate Valid Period**.</span></span>
   
    <span data-ttu-id="a856a-195">Jag.</span><span class="sxs-lookup"><span data-stu-id="a856a-195">i.</span></span> <span data-ttu-id="a856a-196">Kopiera hello innehållet hello hämtat certifikatfilen och klistra in den i hello **SAML verifiera certifikatet** textruta.</span><span class="sxs-lookup"><span data-stu-id="a856a-196">Copy hello content of hello downloaded certificate file, and then paste it into hello **SAML Verifying Certificate** textbox.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a856a-197">Hej certifikatinnehåll måste börja certifikat och certifikat sluttaggar.</span><span class="sxs-lookup"><span data-stu-id="a856a-197">hello certificate content must have begin certificate and end certificate tags.</span></span>

1. <span data-ttu-id="a856a-198">Navigera tooSAML V2 och utför sedan hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a856a-198">Navigate tooSAML V2, and then perform hello following steps:</span></span>
   
    ![Konfigurera enkel inloggning på app-sida][13]
   
    <span data-ttu-id="a856a-200">a.</span><span class="sxs-lookup"><span data-stu-id="a856a-200">a.</span></span> <span data-ttu-id="a856a-201">Välj **Ja** som **stöder SP-initierad globala logga ut**.</span><span class="sxs-lookup"><span data-stu-id="a856a-201">Select **Yes** as **Support SP-initiated Global Logout**.</span></span>
   
    <span data-ttu-id="a856a-202">b.</span><span class="sxs-lookup"><span data-stu-id="a856a-202">b.</span></span> <span data-ttu-id="a856a-203">I hello **globala logga ut tjänst-URL (LogoutRequest mål)** textruta placera hello värdet för **Remote logga ut URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a856a-203">In hello **Global Logout Service URL (LogoutRequest destination)** textbox put hello value of **Remote Logout URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="a856a-204">c.</span><span class="sxs-lookup"><span data-stu-id="a856a-204">c.</span></span> <span data-ttu-id="a856a-205">Välj **nr** som **kräver sp måste kryptera alla NameID elementet**.</span><span class="sxs-lookup"><span data-stu-id="a856a-205">Select **No** as **Require sp must encrypt all NameID element**.</span></span>
   
    <span data-ttu-id="a856a-206">d.</span><span class="sxs-lookup"><span data-stu-id="a856a-206">d.</span></span> <span data-ttu-id="a856a-207">Välj **Ospecificerad** som **NameID Format**.</span><span class="sxs-lookup"><span data-stu-id="a856a-207">Select **unspecified** as **NameID Format**.</span></span>
   
    <span data-ttu-id="a856a-208">e.</span><span class="sxs-lookup"><span data-stu-id="a856a-208">e.</span></span> <span data-ttu-id="a856a-209">Välj **Ja** som **aktivera sp initierade inloggning (AuthnRequest)**.</span><span class="sxs-lookup"><span data-stu-id="a856a-209">Select **Yes** as **Enable sp initiated login (AuthnRequest)**.</span></span>
   
    <span data-ttu-id="a856a-210">f.</span><span class="sxs-lookup"><span data-stu-id="a856a-210">f.</span></span> <span data-ttu-id="a856a-211">I hello **begäran om att skicka som företagsomfattande utgivaren** textruta placera hello värdet för **Remote inloggnings-URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a856a-211">In hello **Send request as Company-Wide issuer** textbox put hello value of **Remote Login URL** from Azure AD application configuration wizard.</span></span>
2. <span data-ttu-id="a856a-212">Utför de här stegen om du vill toomake hello inloggning användarnamn inte skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="a856a-212">Perform these steps if you want toomake hello login usernames Case Insensitive, .</span></span>
   
    <span data-ttu-id="a856a-213">a.</span><span class="sxs-lookup"><span data-stu-id="a856a-213">a.</span></span> <span data-ttu-id="a856a-214">Besök **Företagsinställningar**(nära hello längst ned).</span><span class="sxs-lookup"><span data-stu-id="a856a-214">Visit **Company Settings**(near hello bottom).</span></span>
   
    <span data-ttu-id="a856a-215">b.</span><span class="sxs-lookup"><span data-stu-id="a856a-215">b.</span></span> <span data-ttu-id="a856a-216">Markera kryssrutan bredvid **aktivera icke skiftlägeskänslig användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="a856a-216">select checkbox near **Enable Non-Case-Sensitive Username**.</span></span>
   
    <span data-ttu-id="a856a-217">c.Click **spara**.</span><span class="sxs-lookup"><span data-stu-id="a856a-217">c.Click **Save**.</span></span>
   
    ![Konfigurera enkel inloggning][29]

    > [!NOTE] 
    > <span data-ttu-id="a856a-219">Om du försöker tooenable detta, kontrolleras Hej om det skapar en dubblett SAML-inloggningsnamn.</span><span class="sxs-lookup"><span data-stu-id="a856a-219">If you try tooenable this, hello system checks if it will create a duplicate SAML login name.</span></span> <span data-ttu-id="a856a-220">Om exempelvis hello kunden har användarnamn Användare1 och Användare1.</span><span class="sxs-lookup"><span data-stu-id="a856a-220">For example if hello customer has usernames User1 and user1.</span></span> <span data-ttu-id="a856a-221">Tar bort skiftlägeskänslighet gör dessa dubbletter.</span><span class="sxs-lookup"><span data-stu-id="a856a-221">Taking away case sensitivity makes these duplicates.</span></span> <span data-ttu-id="a856a-222">hello system får du ett felmeddelande och kommer inte att aktivera hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="a856a-222">hello system will give you an error message and will not enable hello feature.</span></span> <span data-ttu-id="a856a-223">hello kunden behöver toochange en hello användarnamn så att det verkligen är stavat olika.</span><span class="sxs-lookup"><span data-stu-id="a856a-223">hello customer will need toochange one of hello usernames so it’s actually spelled different.</span></span> 

1. <span data-ttu-id="a856a-224">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a856a-224">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
    ![Program][14]
2. <span data-ttu-id="a856a-226">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a856a-226">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    ![Program][15]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a856a-228">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="a856a-228">Creating an Azure AD test user</span></span>
<span data-ttu-id="a856a-229">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a856a-229">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][16]

<span data-ttu-id="a856a-231">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a856a-231">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a856a-232">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a856a-232">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][17]
2. <span data-ttu-id="a856a-234">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="a856a-234">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="a856a-235">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="a856a-235">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][18]
4. <span data-ttu-id="a856a-237">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="a856a-237">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][19]
5. <span data-ttu-id="a856a-239">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a856a-239">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD][20]
   
    <span data-ttu-id="a856a-241">a.</span><span class="sxs-lookup"><span data-stu-id="a856a-241">a.</span></span> <span data-ttu-id="a856a-242">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="a856a-242">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="a856a-243">b.</span><span class="sxs-lookup"><span data-stu-id="a856a-243">b.</span></span> <span data-ttu-id="a856a-244">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a856a-244">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="a856a-245">c.</span><span class="sxs-lookup"><span data-stu-id="a856a-245">c.</span></span> <span data-ttu-id="a856a-246">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a856a-246">Click **Next**.</span></span>
6. <span data-ttu-id="a856a-247">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a856a-247">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD][21]
   
    <span data-ttu-id="a856a-249">a.</span><span class="sxs-lookup"><span data-stu-id="a856a-249">a.</span></span> <span data-ttu-id="a856a-250">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="a856a-250">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="a856a-251">b.</span><span class="sxs-lookup"><span data-stu-id="a856a-251">b.</span></span> <span data-ttu-id="a856a-252">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="a856a-252">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="a856a-253">c.</span><span class="sxs-lookup"><span data-stu-id="a856a-253">c.</span></span> <span data-ttu-id="a856a-254">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="a856a-254">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="a856a-255">d.</span><span class="sxs-lookup"><span data-stu-id="a856a-255">d.</span></span> <span data-ttu-id="a856a-256">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="a856a-256">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="a856a-257">e.</span><span class="sxs-lookup"><span data-stu-id="a856a-257">e.</span></span> <span data-ttu-id="a856a-258">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="a856a-258">Click **Next**.</span></span>
7. <span data-ttu-id="a856a-259">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a856a-259">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][22]
8. <span data-ttu-id="a856a-261">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="a856a-261">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD][23]
   
    <span data-ttu-id="a856a-263">a.</span><span class="sxs-lookup"><span data-stu-id="a856a-263">a.</span></span> <span data-ttu-id="a856a-264">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="a856a-264">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="a856a-265">b.</span><span class="sxs-lookup"><span data-stu-id="a856a-265">b.</span></span> <span data-ttu-id="a856a-266">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="a856a-266">Click **Complete**.</span></span>  

### <a name="creating-a-successfactors-test-user"></a><span data-ttu-id="a856a-267">Skapa en testanvändare SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="a856a-267">Creating a SuccessFactors test user</span></span>
<span data-ttu-id="a856a-268">I ordning tooenable Azure AD-användare toolog i SuccessFactors, måste de etableras i SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="a856a-268">In order tooenable Azure AD users toolog into SuccessFactors, they must be provisioned into SuccessFactors.</span></span>  
<span data-ttu-id="a856a-269">Hello gäller SuccessFactors är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a856a-269">In hello case of SuccessFactors, provisioning is a manual task.</span></span>

<span data-ttu-id="a856a-270">tooget-användare som har skapats i SuccessFactors måste toocontact hello [SuccessFactors supportteam](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="a856a-270">tooget users created in SuccessFactors, you need toocontact hello [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a856a-271">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="a856a-271">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="a856a-272">hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att tilldela tooSuccessFactors sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a856a-272">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSuccessFactors.</span></span>

![Tilldela användare][24]

<span data-ttu-id="a856a-274">**tooassign Britta Simon tooSuccessFactors utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="a856a-274">**tooassign Britta Simon tooSuccessFactors, perform hello following steps:**</span></span>

1. <span data-ttu-id="a856a-275">På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="a856a-275">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Tilldela användare][25]
2. <span data-ttu-id="a856a-277">Välj i listan med program hello **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="a856a-277">In hello applications list, select **SuccessFactors**.</span></span>
   
    ![Konfigurera enkel inloggning][26]
3. <span data-ttu-id="a856a-279">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="a856a-279">In hello menu on hello top, click **Users**.</span></span>
   
    ![Tilldela användare][27]
4. <span data-ttu-id="a856a-281">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="a856a-281">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="a856a-282">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="a856a-282">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][28]

### <a name="testing-single-sign-on"></a><span data-ttu-id="a856a-284">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="a856a-284">Testing single sign-on</span></span>
<span data-ttu-id="a856a-285">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a856a-285">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a856a-286">Du bör få automatiskt inloggade tooyour SuccessFactors programmet när du klickar på hello SuccessFactors panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="a856a-286">When you click hello SuccessFactors tile in hello Access Panel, you should get automatically signed-on tooyour SuccessFactors application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a856a-287">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a856a-287">Additional resources</span></span>
* [<span data-ttu-id="a856a-288">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a856a-288">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a856a-289">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a856a-289">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
