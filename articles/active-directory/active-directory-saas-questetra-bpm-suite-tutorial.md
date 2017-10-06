---
title: "Självstudier: Azure Active Directory-integrering med Questetra BPM Suite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Questetra BPM Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 4907e3b5751cd79f994fbd2ebcb7faec4eac34e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a><span data-ttu-id="b06aa-103">Självstudier: Azure Active Directory-integrering med Questetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="b06aa-103">Tutorial: Azure Active Directory integration with Questetra BPM Suite</span></span>
<span data-ttu-id="b06aa-104">hello syftet med den här kursen är tooshow du hur toointegrate Questetra BPM Suite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b06aa-104">hello objective of this tutorial is tooshow you how toointegrate Questetra BPM Suite with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="b06aa-105">Integrera Questetra BPM Suite med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b06aa-105">Integrating Questetra BPM Suite with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="b06aa-106">Du kan styra i Azure AD som har åtkomst tooQuestetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="b06aa-106">You can control in Azure AD who has access tooQuestetra BPM Suite</span></span> 
* <span data-ttu-id="b06aa-107">Du kan aktivera din användare tooautomatically get inloggade tooQuestetra BPM Suite (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b06aa-107">You can enable your users tooautomatically get signed-on tooQuestetra BPM Suite (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="b06aa-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b06aa-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="b06aa-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b06aa-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b06aa-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b06aa-110">Prerequisites</span></span>
<span data-ttu-id="b06aa-111">tooconfigure Azure AD-integrering med Questetra BPM Suite måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b06aa-111">tooconfigure Azure AD integration with Questetra BPM Suite, you need hello following items:</span></span>

* <span data-ttu-id="b06aa-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b06aa-112">An Azure AD subscription</span></span>
* <span data-ttu-id="b06aa-113">En [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b06aa-113">An [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b06aa-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b06aa-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="b06aa-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b06aa-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="b06aa-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b06aa-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="b06aa-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b06aa-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="b06aa-118">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="b06aa-118">Scenario Description</span></span>
<span data-ttu-id="b06aa-119">hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b06aa-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="b06aa-120">hello-scenario som beskrivs i den här kursen består av tre huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b06aa-120">hello scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="b06aa-121">Att lägga till Questetra BPM Suite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b06aa-121">Adding Questetra BPM Suite from hello gallery</span></span> 
2. <span data-ttu-id="b06aa-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b06aa-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-questetra-bpm-suite-from-hello-gallery"></a><span data-ttu-id="b06aa-123">Att lägga till Questetra BPM Suite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b06aa-123">Adding Questetra BPM Suite from hello gallery</span></span>
<span data-ttu-id="b06aa-124">tooconfigure hello integrering av Questetra BPM Suite i Azure AD, behöver du tooadd Questetra BPM Suite hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b06aa-124">tooconfigure hello integration of Questetra BPM Suite into Azure AD, you need tooadd Questetra BPM Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b06aa-125">**tooadd Questetra BPM Suite från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b06aa-125">**tooadd Questetra BPM Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b06aa-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="b06aa-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="b06aa-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="b06aa-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="b06aa-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Program][2]

4. <span data-ttu-id="b06aa-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="b06aa-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3]

5. <span data-ttu-id="b06aa-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Program][4]

6. <span data-ttu-id="b06aa-135">Skriv i sökrutan hello **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-135">In hello search box, type **Questetra BPM Suite**.</span></span>
   
    ![Program][5]

7. <span data-ttu-id="b06aa-137">I resultatfönstret hello väljer **Questetra BPM Suite**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b06aa-137">In hello results pane, select **Questetra BPM Suite**, and then click **Complete** tooadd hello application.</span></span>

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b06aa-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b06aa-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b06aa-139">hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med Questetra BPM Suite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b06aa-139">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with Questetra BPM Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b06aa-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Questetra BPM Suite tooan användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b06aa-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Questetra BPM Suite tooan user in Azure AD is.</span></span> <span data-ttu-id="b06aa-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Questetra BPM Suite toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b06aa-141">In other words, a link relationship between an Azure AD user and hello related user in Questetra BPM Suite needs toobe established.</span></span>  
<span data-ttu-id="b06aa-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Questetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="b06aa-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Questetra BPM Suite.</span></span>

<span data-ttu-id="b06aa-143">tooconfigure och testa Azure AD enkel inloggning med Questetra BPM Suite, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b06aa-143">tooconfigure and test Azure AD single sign-on with Questetra BPM Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b06aa-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b06aa-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b06aa-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b06aa-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b06aa-146">**[Skapa en testanvändare Questetra BPM Suite](#creating-a-questetra-bpm-suite-test-user)**  -toohave en motsvarighet för Britta Simon i Questetra BPM Suite som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="b06aa-146">**[Creating a Questetra BPM Suite test user](#creating-a-questetra-bpm-suite-test-user)** - toohave a counterpart of Britta Simon in Questetra BPM Suite that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="b06aa-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b06aa-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b06aa-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b06aa-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b06aa-149">Konfigurera Azure AD-Single Sign-On</span><span class="sxs-lookup"><span data-stu-id="b06aa-149">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="b06aa-150">hello syftet med det här avsnittet är tooenable Azure AD enkel inloggning i hello klassiska Azure-portalen och tooconfigure enkel inloggning i ditt Questetra BPM Suite-program.</span><span class="sxs-lookup"><span data-stu-id="b06aa-150">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your Questetra BPM Suite application.</span></span>

<span data-ttu-id="b06aa-151">**tooconfigure Azure AD enkel inloggning med Questetra BPM Suite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b06aa-151">**tooconfigure Azure AD single sign-on with Questetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b06aa-152">I hello klassiska Azure-portalen på hello **Questetra BPM Suite** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b06aa-152">In hello Azure classic portal, on hello **Questetra BPM Suite** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][8]

2. <span data-ttu-id="b06aa-154">På hello **hur skulle du som användare toosign på tooQuestetra BPM Suite** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-154">On hello **How would you like users toosign on tooQuestetra BPM Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][9]

3. <span data-ttu-id="b06aa-156">Logga in i ett annat webbläsarfönster din **Questetra BPM Suite** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b06aa-156">In a different web browser window, log into your **Questetra BPM Suite** company site as an administrator.</span></span>

4. <span data-ttu-id="b06aa-157">Hello-menyn överst hello **systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-157">In hello menu on hello top, click **System Settings**.</span></span> 
   
    ![Azure AD-Single Sign-On][10]

5. <span data-ttu-id="b06aa-159">tooopen hello **SingleSignOnSAML** klickar du på **SSO (SAML)**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-159">tooopen hello **SingleSignOnSAML** page, click **SSO (SAML)**.</span></span> 
   
    ![Azure AD-Single Sign-On][11]

6. <span data-ttu-id="b06aa-161">I hello klassiska Azure-portalen på hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b06aa-161">In hello Azure classic portal, on hello **Configure App Settings** dialog page, perform hello following steps:</span></span> 
   
    ![Konfigurera Appinställningar för][13]
   
    <span data-ttu-id="b06aa-163">a.</span><span class="sxs-lookup"><span data-stu-id="b06aa-163">a.</span></span> <span data-ttu-id="b06aa-164">Om du **Questetra BPM Suite** företagets plats i hello information om SP, kopiera hello **ACS URL**, och klistra in den i hello **logga URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b06aa-164">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **ACS URL**, and then paste it into hello **Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="b06aa-165">b.</span><span class="sxs-lookup"><span data-stu-id="b06aa-165">b.</span></span> <span data-ttu-id="b06aa-166">Om du **Questetra BPM Suite** företagets plats i hello information om SP, kopiera hello **enhets-ID**, och klistra in den i hello **utfärdar-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b06aa-166">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **Entity ID**, and then paste it into hello **Issuer URL** textbox.</span></span>
   
    <span data-ttu-id="b06aa-167">c.</span><span class="sxs-lookup"><span data-stu-id="b06aa-167">c.</span></span> <span data-ttu-id="b06aa-168">Om du **Questetra BPM Suite** företagets plats i hello information om SP, kopiera hello **ACS URL**, och klistra in den i hello **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b06aa-168">On you **Questetra BPM Suite** company site, in hello SP Information section, copy hello **ACS URL**, and then paste it into hello **Reply URL** textbox.</span></span>
   
    <span data-ttu-id="b06aa-169">d.</span><span class="sxs-lookup"><span data-stu-id="b06aa-169">d.</span></span> <span data-ttu-id="b06aa-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-170">Click **Next**.</span></span>

7. <span data-ttu-id="b06aa-171">På hello **Konfigurera enkel inloggning på Questetra BPM Suite** klickar du på **hämta certifikat**, och sedan spara hello certifikatfilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="b06aa-171">On hello **Configure single sign-on at Questetra BPM Suite** page, click **Download certificate**, and then save hello certificate file locally on your computer.</span></span>
   
    ![Konfigurera enkel inloggning][14]

8. <span data-ttu-id="b06aa-173">Om du **Questetra BPM Suite** företagets plats, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b06aa-173">On you **Questetra BPM Suite** company site, perform hello following steps:</span></span> 
   
    ![Konfigurera enkel inloggning][15]
   
    <span data-ttu-id="b06aa-175">a.</span><span class="sxs-lookup"><span data-stu-id="b06aa-175">a.</span></span> <span data-ttu-id="b06aa-176">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-176">Select **Enable Single Sign-On**.</span></span>
   
    <span data-ttu-id="b06aa-177">b.</span><span class="sxs-lookup"><span data-stu-id="b06aa-177">b.</span></span> <span data-ttu-id="b06aa-178">Kopiera hello på hello klassiska Azure-portalen, **utfärdar-URL** värdet och klistrar in den i hello **enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="b06aa-178">On hello Azure classic portal, copy hello **Issuer URL** value, and then paste it into hello **Entity ID** textbox.</span></span>
   
    <span data-ttu-id="b06aa-179">c.</span><span class="sxs-lookup"><span data-stu-id="b06aa-179">c.</span></span> <span data-ttu-id="b06aa-180">Kopiera hello på hello klassiska Azure-portalen, **inloggning tjänst-URL för enkel** värdet och klistrar in den i hello **inloggning Sidadress** textruta.</span><span class="sxs-lookup"><span data-stu-id="b06aa-180">On hello Azure classic portal, copy hello **Single Sign-On Service URL** value, and then paste it into hello **Sign-in page URL** textbox.</span></span>
   
    <span data-ttu-id="b06aa-181">d.</span><span class="sxs-lookup"><span data-stu-id="b06aa-181">d.</span></span> <span data-ttu-id="b06aa-182">Kopiera hello på hello klassiska Azure-portalen, **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **URL för utloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="b06aa-182">On hello Azure classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Sign-out page URL** textbox.</span></span>
   
    <span data-ttu-id="b06aa-183">e.</span><span class="sxs-lookup"><span data-stu-id="b06aa-183">e.</span></span> <span data-ttu-id="b06aa-184">I hello **NameID format** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-184">In hello **NameID format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="b06aa-185">f.</span><span class="sxs-lookup"><span data-stu-id="b06aa-185">f.</span></span> <span data-ttu-id="b06aa-186">Skapa en Base64-kodade filen från din hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="b06aa-186">Create a base-64 encoded file from your downloaded certificate.</span></span> 

    >[!TIP] 
    ><span data-ttu-id="b06aa-187">Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="b06aa-187">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

    <span data-ttu-id="b06aa-188">g.</span><span class="sxs-lookup"><span data-stu-id="b06aa-188">g.</span></span> <span data-ttu-id="b06aa-189">Öppna din Base64-kodade certifikatet i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den i hello **validering certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="b06aa-189">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it into hello **Validation certificate** textbox.</span></span> 

    <span data-ttu-id="b06aa-190">h.</span><span class="sxs-lookup"><span data-stu-id="b06aa-190">h.</span></span> <span data-ttu-id="b06aa-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-191">Click **Save**.</span></span>

1. <span data-ttu-id="b06aa-192">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-192">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Vad är Azure AD Connect?][17]

2. <span data-ttu-id="b06aa-194">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-194">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Vad är Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b06aa-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b06aa-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="b06aa-197">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b06aa-197">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="b06aa-198">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b06aa-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b06aa-199">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-199">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa testanvändare i Azure AD][100] 

2. <span data-ttu-id="b06aa-201">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="b06aa-201">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="b06aa-202">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-202">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Skapa testanvändare i Azure AD][101] 

4. <span data-ttu-id="b06aa-204">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-204">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Skapa testanvändare i Azure AD][102] 

5. <span data-ttu-id="b06aa-206">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b06aa-206">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Skapa testanvändare i Azure AD][103]
   
    <span data-ttu-id="b06aa-208">a.</span><span class="sxs-lookup"><span data-stu-id="b06aa-208">a.</span></span> <span data-ttu-id="b06aa-209">Som **typ av användare**väljer **ny användare i din organisation**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-209">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="b06aa-210">b.</span><span class="sxs-lookup"><span data-stu-id="b06aa-210">b.</span></span> <span data-ttu-id="b06aa-211">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-211">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="b06aa-212">c.</span><span class="sxs-lookup"><span data-stu-id="b06aa-212">c.</span></span> <span data-ttu-id="b06aa-213">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="b06aa-213">Click Next.</span></span>

6. <span data-ttu-id="b06aa-214">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b06aa-214">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Skapa testanvändare i Azure AD][104] 
   
    <span data-ttu-id="b06aa-216">a.</span><span class="sxs-lookup"><span data-stu-id="b06aa-216">a.</span></span> <span data-ttu-id="b06aa-217">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-217">In hello **First Name** textbox, type **Britta**.</span></span> 
   
    <span data-ttu-id="b06aa-218">b.</span><span class="sxs-lookup"><span data-stu-id="b06aa-218">b.</span></span> <span data-ttu-id="b06aa-219">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-219">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="b06aa-220">c.</span><span class="sxs-lookup"><span data-stu-id="b06aa-220">c.</span></span> <span data-ttu-id="b06aa-221">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-221">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="b06aa-222">d.</span><span class="sxs-lookup"><span data-stu-id="b06aa-222">d.</span></span> <span data-ttu-id="b06aa-223">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-223">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="b06aa-224">e.</span><span class="sxs-lookup"><span data-stu-id="b06aa-224">e.</span></span> <span data-ttu-id="b06aa-225">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-225">Click **Next**.</span></span>

7. <span data-ttu-id="b06aa-226">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-226">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa testanvändare i Azure AD][105]  

8. <span data-ttu-id="b06aa-228">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b06aa-228">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Skapa testanvändare i Azure AD][106]   
   
    <span data-ttu-id="b06aa-230">a.</span><span class="sxs-lookup"><span data-stu-id="b06aa-230">a.</span></span> <span data-ttu-id="b06aa-231">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-231">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="b06aa-232">b.</span><span class="sxs-lookup"><span data-stu-id="b06aa-232">b.</span></span> <span data-ttu-id="b06aa-233">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="b06aa-233">Click **Complete**.</span></span>   

### <a name="creating-a-questetra-bpm-suite-test-user"></a><span data-ttu-id="b06aa-234">Skapa en testanvändare Questetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="b06aa-234">Creating a Questetra BPM Suite test user</span></span>
<span data-ttu-id="b06aa-235">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Questetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="b06aa-235">hello objective of this section is toocreate a user called Britta Simon in Questetra BPM Suite.</span></span>

<span data-ttu-id="b06aa-236">**toocreate en användare som kallas Britta Simon i Questetra BPM Suite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b06aa-236">**toocreate a user called Britta Simon in Questetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b06aa-237">Inloggning tooyour Questetra BPM Suite företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b06aa-237">Sign-on tooyour Questetra BPM Suite company site as an administrator.</span></span>
2. <span data-ttu-id="b06aa-238">Gå för**systeminställningar > användarlistan > Ny användare**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-238">Go too**System Settings > User List > New User**.</span></span> 
3. <span data-ttu-id="b06aa-239">Utför följande hello på hello ny användare dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="b06aa-239">On hello New User dialog, perform hello following steps:</span></span> 
   
    ![Skapa testanvändare][300] 
   
    <span data-ttu-id="b06aa-241">a.</span><span class="sxs-lookup"><span data-stu-id="b06aa-241">a.</span></span> <span data-ttu-id="b06aa-242">I hello **namn** textruta anger Brittas användarnamn i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b06aa-242">In hello **Name** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="b06aa-243">b.</span><span class="sxs-lookup"><span data-stu-id="b06aa-243">b.</span></span> <span data-ttu-id="b06aa-244">I hello **e-post** textruta anger Brittas användarnamn i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b06aa-244">In hello **Email** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="b06aa-245">c.</span><span class="sxs-lookup"><span data-stu-id="b06aa-245">c.</span></span> <span data-ttu-id="b06aa-246">I hello **lösenord** textruta, ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="b06aa-246">In hello **Password** textbox, type a password.</span></span>

4. <span data-ttu-id="b06aa-247">Klicka på **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-247">Click **Add new user**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b06aa-248">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b06aa-248">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="b06aa-249">hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access tooQuestetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="b06aa-249">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooQuestetra BPM Suite.</span></span>

![Vad är Azure AD Connect?][200]

<span data-ttu-id="b06aa-251">**tooassign Britta Simon tooQuestetra BPM Suite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b06aa-251">**tooassign Britta Simon tooQuestetra BPM Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b06aa-252">På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="b06aa-252">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Vad är Azure AD Connect?][201]
2. <span data-ttu-id="b06aa-254">Välj i listan med program hello **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-254">In hello applications list, select **Questetra BPM Suite**.</span></span>
   
    ![Vad är Azure AD Connect?][205]
3. <span data-ttu-id="b06aa-256">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-256">In hello menu on hello top, click **Users**.</span></span>
   
    ![Vad är Azure AD Connect?][202]
4. <span data-ttu-id="b06aa-258">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-258">In hello Users list, select **Britta Simon**.</span></span>
   
    ![Vad är Azure AD Connect?][203]
5. <span data-ttu-id="b06aa-260">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="b06aa-260">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Vad är Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a><span data-ttu-id="b06aa-262">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b06aa-262">Testing Single Sign-On</span></span>
<span data-ttu-id="b06aa-263">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b06aa-263">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="b06aa-264">När du klickar på hello Questetra BPM Suite-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour Questetra BPM Suite-program.</span><span class="sxs-lookup"><span data-stu-id="b06aa-264">When you click hello Questetra BPM Suite tile in hello Access Panel, you should get automatically signed-on tooyour Questetra BPM Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b06aa-265">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b06aa-265">Additional Resources</span></span>
* [<span data-ttu-id="b06aa-266">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b06aa-266">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b06aa-267">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b06aa-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
