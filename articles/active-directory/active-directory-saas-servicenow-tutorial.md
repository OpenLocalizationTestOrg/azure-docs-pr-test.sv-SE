---
title: "Självstudier: Azure Active Directory-integrering med ServiceNow | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och ServiceNow och ServiceNow Express."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: df6a07dd1aa437198fbdb9d0a04ea14f3a320249
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a><span data-ttu-id="91e41-103">Självstudier: Azure Active Directory-integrering med ServiceNow</span><span class="sxs-lookup"><span data-stu-id="91e41-103">Tutorial: Azure Active Directory integration with ServiceNow</span></span>
<span data-ttu-id="91e41-104">I kursen får du lära dig hur toointegrate ServiceNow och ServiceNow Express med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="91e41-104">In this tutorial, you learn how toointegrate ServiceNow and ServiceNow Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="91e41-105">Integrera ServiceNow och ServiceNow Express med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="91e41-105">Integrating ServiceNow and ServiceNow Express with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="91e41-106">Du kan styra i Azure AD som har åtkomst till tooServiceNow och ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="91e41-106">You can control in Azure AD who has access tooServiceNow and ServiceNow Express</span></span>
* <span data-ttu-id="91e41-107">Du kan aktivera din användare tooautomatically get inloggade tooServiceNow och ServiceNow Express (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="91e41-107">You can enable your users tooautomatically get signed-on tooServiceNow and ServiceNow Express (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="91e41-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="91e41-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="91e41-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="91e41-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91e41-110">Krav</span><span class="sxs-lookup"><span data-stu-id="91e41-110">Prerequisites</span></span>
<span data-ttu-id="91e41-111">tooconfigure Azure AD-integrering med ServiceNow och ServiceNow Express, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="91e41-111">tooconfigure Azure AD integration with ServiceNow and ServiceNow Express, you need hello following items:</span></span>

* <span data-ttu-id="91e41-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="91e41-112">An Azure AD subscription</span></span>
* <span data-ttu-id="91e41-113">För ServiceNow, instans eller innehavare för ServiceNow Calgary version eller högre</span><span class="sxs-lookup"><span data-stu-id="91e41-113">For ServiceNow, an instance or tenant of ServiceNow, Calgary version or higher</span></span>
* <span data-ttu-id="91e41-114">För ServiceNow Express, en instans av ServiceNow Express, Helsingfors version eller högre</span><span class="sxs-lookup"><span data-stu-id="91e41-114">For ServiceNow Express, an instance of ServiceNow Express, Helsinki version or higher</span></span>
* <span data-ttu-id="91e41-115">Hej ServiceNow-klient måste ha hello [flera enkel inloggning på Plugin-providern](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) aktiverat.</span><span class="sxs-lookup"><span data-stu-id="91e41-115">hello ServiceNow tenant must have hello [Multiple Provider Single Sign On Plugin](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) enabled.</span></span> <span data-ttu-id="91e41-116">Detta kan göras med [att skicka en serviceförfrågan](https://hi.service-now.com).</span><span class="sxs-lookup"><span data-stu-id="91e41-116">This can be done by [submitting a service request](https://hi.service-now.com).</span></span> 

> [!NOTE]
> <span data-ttu-id="91e41-117">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="91e41-117">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="91e41-118">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="91e41-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="91e41-119">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="91e41-119">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="91e41-120">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="91e41-120">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="91e41-121">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="91e41-121">Scenario description</span></span>
<span data-ttu-id="91e41-122">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="91e41-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="91e41-123">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="91e41-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="91e41-124">Att lägga till ServiceNow från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91e41-124">Adding ServiceNow from hello gallery</span></span>
2. <span data-ttu-id="91e41-125">Konfigurera och testa Azure AD enkel inloggning för ServiceNow eller ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="91e41-125">Configuring and testing Azure AD single sign-on for ServiceNow or ServiceNow Express</span></span>

## <a name="adding-servicenow-from-hello-gallery"></a><span data-ttu-id="91e41-126">Att lägga till ServiceNow från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="91e41-126">Adding ServiceNow from hello gallery</span></span>
<span data-ttu-id="91e41-127">tooconfigure hello integrering av ServiceNow eller ServiceNow Express i Azure AD, behöver du tooadd ServiceNow hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="91e41-127">tooconfigure hello integration of ServiceNow or ServiceNow Express into Azure AD, you need tooadd ServiceNow from hello gallery tooyour list of managed SaaS apps.</span></span> 

<span data-ttu-id="91e41-128">**tooadd ServiceNow från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91e41-128">**tooadd ServiceNow from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e41-129">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="91e41-129">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="91e41-131">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="91e41-131">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="91e41-132">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="91e41-132">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Program][2]
4. <span data-ttu-id="91e41-134">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="91e41-134">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3]
5. <span data-ttu-id="91e41-136">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="91e41-136">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Program][4]
6. <span data-ttu-id="91e41-138">Skriv i sökrutan hello **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="91e41-138">In hello search box, type **ServiceNow**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. <span data-ttu-id="91e41-140">I resultatfönstret hello väljer **ServiceNow**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="91e41-140">In hello results pane, select **ServiceNow**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="91e41-142">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91e41-142">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="91e41-143">Du konfigurera och testa Azure AD enkel inloggning med ServiceNow eller ServiceNow Express baserat på en testanvändare som kallas ”Britta Simon” i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="91e41-143">In this section, you configure and test Azure AD single sign-on with ServiceNow or ServiceNow Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="91e41-144">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i ServiceNow är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="91e41-144">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ServiceNow is tooa user in Azure AD.</span></span> <span data-ttu-id="91e41-145">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i ServiceNow toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="91e41-145">In other words, a link relationship between an Azure AD user and hello related user in ServiceNow needs toobe established.</span></span>
<span data-ttu-id="91e41-146">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="91e41-146">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ServiceNow.</span></span> <span data-ttu-id="91e41-147">tooconfigure och testa Azure AD enkel inloggning med ServiceNow, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="91e41-147">tooconfigure and test Azure AD single sign-on with ServiceNow, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="91e41-148">**[Konfigurera Azure AD enkel inloggning för ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="91e41-148">**[Configuring Azure AD Single Sign-On for ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="91e41-149">**[Konfigurera Azure AD enkel inloggning för ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="91e41-149">**[Configuring Azure AD Single Sign-On for ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - tooenable your users toouse this feature.</span></span>
3. <span data-ttu-id="91e41-150">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91e41-150">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="91e41-151">**[Skapa en testanvändare ServiceNow](#creating-a-servicenow-test-user)**  -toohave en motsvarighet för Britta Simon i ServiceNow som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="91e41-151">**[Creating a ServiceNow test user](#creating-a-servicenow-test-user)** - toohave a counterpart of Britta Simon in ServiceNow that is linked toohello Azure AD representation of her.</span></span>
5. <span data-ttu-id="91e41-152">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="91e41-152">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="91e41-153">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="91e41-153">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

> [!NOTE]
> <span data-ttu-id="91e41-154">Om du vill utelämna tooconfigure ServiceNow steg 2.</span><span class="sxs-lookup"><span data-stu-id="91e41-154">If you want tooconfigure ServiceNow omit step 2.</span></span> <span data-ttu-id="91e41-155">Likaså om du vill tooconfigure ServiceNow Express utelämna steg 1.</span><span class="sxs-lookup"><span data-stu-id="91e41-155">Likewise, if you want tooconfigure ServiceNow Express omit step 1.</span></span>
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a><span data-ttu-id="91e41-156">Konfigurera Azure AD-Single Sign-On för ServiceNow</span><span class="sxs-lookup"><span data-stu-id="91e41-156">Configuring Azure AD Single Sign-On for ServiceNow</span></span>
1. <span data-ttu-id="91e41-157">I klassiska hello Azure AD-portalen, på hello **ServiceNow** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan .</span><span class="sxs-lookup"><span data-stu-id="91e41-157">In hello Azure AD classic portal, on hello **ServiceNow** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="91e41-158">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-158">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="91e41-159">På hello **hur skulle du som användare toosign på tooServiceNow** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-159">On hello **How would you like users toosign on tooServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="91e41-160">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-160">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="91e41-161">På hello **konfigurera Appinställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-161">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="91e41-162">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-162">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="91e41-163">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-163">a.</span></span> <span data-ttu-id="91e41-164">i hello **ServiceNow logga URL** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="91e41-164">in hello **ServiceNow Sign On URL** textbox, type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="91e41-165">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-165">b.</span></span> <span data-ttu-id="91e41-166">I hello **identifierare** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="91e41-166">In hello **Identifier** textbox,type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="91e41-167">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-167">c.</span></span> <span data-ttu-id="91e41-168">Klicka på **Nästa**</span><span class="sxs-lookup"><span data-stu-id="91e41-168">Click **Next**</span></span>

4. <span data-ttu-id="91e41-169">toohave Azure AD automatiskt konfigurerar ServiceNow för SAML-baserad autentisering, ange ditt ServiceNow-instansnamn administratörsanvändarnamnet och administratörslösenord i hello **automatiskt konfigurera enkel inloggning** formuläret och klicka på  *Konfigurera*.</span><span class="sxs-lookup"><span data-stu-id="91e41-169">toohave Azure AD automatically configure ServiceNow for SAML-based authentication, enter your ServiceNow instance name, admin username, and admin password in hello **Auto configure single sign-on** form and click *Configure*.</span></span> <span data-ttu-id="91e41-170">Observera att hello administratörsanvändarnamnet tillhandahålls måste ha hello **security_admin** roll i ServiceNow för den här toowork.</span><span class="sxs-lookup"><span data-stu-id="91e41-170">Note that hello admin username provided must have hello **security_admin** role assigned in ServiceNow for this toowork.</span></span> <span data-ttu-id="91e41-171">Annars toomanually konfigurera ServiceNow toouse Azure AD som en SAML-identitetsprovider klickar du på **manuellt konfigurera hello program för enkel inloggning**, klicka på **nästa** och fullständig hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="91e41-171">Otherwise, toomanually configure ServiceNow toouse Azure AD as a SAML identity provider, click **Manually configure hello application for single sign-on**, then click **Next** and complete hello following steps.</span></span>
   
    <span data-ttu-id="91e41-172">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-172">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="91e41-173">På hello **Konfigurera enkel inloggning på ServiceNow** klickar du på **hämta certifikat**, spara hello certifikatfilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="91e41-173">On hello **Configure single sign-on at ServiceNow** page, click **Download certificate**, save hello certificate file locally on your computer.</span></span>
   
    <span data-ttu-id="91e41-174">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-174">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="91e41-175">Inloggning tooyour ServiceNow-programmet som administratör.</span><span class="sxs-lookup"><span data-stu-id="91e41-175">Sign-on tooyour ServiceNow application as an administrator.</span></span>

7. <span data-ttu-id="91e41-176">Aktivera hello *integrering – flera enkel inloggning installationsprogrammet för providern* plugin-programmet genom att följa hello nästa steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-176">Activate hello *Integration - Multiple Provider Single Sign-On Installer* plugin by following hello next steps:</span></span>
   
    <span data-ttu-id="91e41-177">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-177">a.</span></span> <span data-ttu-id="91e41-178">I navigeringsfönstret hello vänster hello gå för**System Definition** avsnittet och klicka sedan på **plugin-program**.</span><span class="sxs-lookup"><span data-stu-id="91e41-178">In hello navigation pane on hello left side, go too**System Definition** section and then click **Plugins**.</span></span>
   
    <span data-ttu-id="91e41-179">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "aktivera plugin-program")</span><span class="sxs-lookup"><span data-stu-id="91e41-179">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Activate plugin")</span></span>
   
    <span data-ttu-id="91e41-180">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-180">b.</span></span> <span data-ttu-id="91e41-181">Sök efter *integrering – flera enkel inloggning installationsprogrammet för providern*.</span><span class="sxs-lookup"><span data-stu-id="91e41-181">Search for *Integration - Multiple Provider Single Sign-On Installer*.</span></span>
   
    <span data-ttu-id="91e41-182">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "aktivera plugin-program")</span><span class="sxs-lookup"><span data-stu-id="91e41-182">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Activate plugin")</span></span>
   
    <span data-ttu-id="91e41-183">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-183">c.</span></span> <span data-ttu-id="91e41-184">Välj hello plugin-programmet.</span><span class="sxs-lookup"><span data-stu-id="91e41-184">Select hello plugin.</span></span> <span data-ttu-id="91e41-185">Rigth Klicka och välj **aktivera eller uppgradering**.</span><span class="sxs-lookup"><span data-stu-id="91e41-185">Rigth click and select **Activate/Upgrade**.</span></span>
   
    <span data-ttu-id="91e41-186">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-186">d.</span></span> <span data-ttu-id="91e41-187">Klicka på hello **aktivera** knappen.</span><span class="sxs-lookup"><span data-stu-id="91e41-187">Click hello **Activate** button.</span></span>

8. <span data-ttu-id="91e41-188">Hello navigeringsfönstret hello vänster, klicka på **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="91e41-188">In hello navigation pane on hello left side, click **Properties**.</span></span>  
   
    <span data-ttu-id="91e41-189">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-189">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Configure app URL")</span></span>

9. <span data-ttu-id="91e41-190">På hello **flera egenskaper för SSO** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-190">On hello **Multiple Provider SSO Properties** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="91e41-191">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-191">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Configure app URL")</span></span>
   
    <span data-ttu-id="91e41-192">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-192">a.</span></span> <span data-ttu-id="91e41-193">Som **aktivera flera providern SSO**väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="91e41-193">As **Enable multiple provider SSO**, select **Yes**.</span></span>
   
    <span data-ttu-id="91e41-194">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-194">b.</span></span> <span data-ttu-id="91e41-195">Som **Aktivera felsökningsloggning fick hello flera providern SSO integration**väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="91e41-195">As **Enable debug logging got hello multiple provider SSO integration**, select **Yes**.</span></span>
   
    <span data-ttu-id="91e41-196">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-196">c.</span></span> <span data-ttu-id="91e41-197">I **hello fält hello användaren tabell som...**  textruta typen **user_name**.</span><span class="sxs-lookup"><span data-stu-id="91e41-197">In **hello field on hello user table that...** textbox, type **user_name**.</span></span>
   
    <span data-ttu-id="91e41-198">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-198">d.</span></span> <span data-ttu-id="91e41-199">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="91e41-199">Click **Save**.</span></span>

10. <span data-ttu-id="91e41-200">Hello navigeringsfönstret hello vänster, klicka på **x509 certifikat**.</span><span class="sxs-lookup"><span data-stu-id="91e41-200">In hello navigation pane on hello left side, click **x509 Certificates**.</span></span>
    
     <span data-ttu-id="91e41-201">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-201">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Configure single sign-on")</span></span>

11. <span data-ttu-id="91e41-202">På hello **X.509-certifikat** dialogrutan klickar du på **ny**.</span><span class="sxs-lookup"><span data-stu-id="91e41-202">On hello **X.509 Certificates** dialog, click **New**.</span></span>
    
     <span data-ttu-id="91e41-203">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-203">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Configure single sign-on")</span></span>

12. <span data-ttu-id="91e41-204">På hello **X.509-certifikat** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-204">On hello **X.509 Certificates** dialog, perform hello following steps:</span></span>
    
     <span data-ttu-id="91e41-205">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-205">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
     <span data-ttu-id="91e41-206">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-206">a.</span></span> <span data-ttu-id="91e41-207">Klicka på **Ny**.</span><span class="sxs-lookup"><span data-stu-id="91e41-207">Click **New**.</span></span>
    
     <span data-ttu-id="91e41-208">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-208">b.</span></span> <span data-ttu-id="91e41-209">I hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="91e41-209">In hello **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
     <span data-ttu-id="91e41-210">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-210">c.</span></span> <span data-ttu-id="91e41-211">Välj **Active**.</span><span class="sxs-lookup"><span data-stu-id="91e41-211">Select **Active**.</span></span>
    
     <span data-ttu-id="91e41-212">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-212">d.</span></span> <span data-ttu-id="91e41-213">Som **Format**väljer **PEM**.</span><span class="sxs-lookup"><span data-stu-id="91e41-213">As **Format**, select **PEM**.</span></span>
    
     <span data-ttu-id="91e41-214">e.</span><span class="sxs-lookup"><span data-stu-id="91e41-214">e.</span></span> <span data-ttu-id="91e41-215">Som **typen**väljer **förtroende Store Cert**.</span><span class="sxs-lookup"><span data-stu-id="91e41-215">As **Type**, select **Trust Store Cert**.</span></span>
    
     <span data-ttu-id="91e41-216">f.</span><span class="sxs-lookup"><span data-stu-id="91e41-216">f.</span></span> <span data-ttu-id="91e41-217">Öppna Base64-kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **PEM certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-217">Open your Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **PEM Certificate** textbox.</span></span>
    
     <span data-ttu-id="91e41-218">g.</span><span class="sxs-lookup"><span data-stu-id="91e41-218">g.</span></span> <span data-ttu-id="91e41-219">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="91e41-219">Click **Update**.</span></span>

13. <span data-ttu-id="91e41-220">Hello navigeringsfönstret hello vänster, klicka på **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="91e41-220">In hello navigation pane on hello left side, click **Identity Providers**.</span></span>
    
     <span data-ttu-id="91e41-221">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-221">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Configure single sign-on")</span></span>

14. <span data-ttu-id="91e41-222">På hello **identitetsleverantörer** dialogrutan klickar du på **ny**:</span><span class="sxs-lookup"><span data-stu-id="91e41-222">On hello **Identity Providers** dialog, click **New**:</span></span>
    
     <span data-ttu-id="91e41-223">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-223">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Configure single sign-on")</span></span>

15. <span data-ttu-id="91e41-224">På hello **identitetsleverantörer** dialogrutan klickar du på **SAML2 Update1?**:</span><span class="sxs-lookup"><span data-stu-id="91e41-224">On hello **Identity Providers** dialog, click **SAML2 Update1?**:</span></span>
    
     <span data-ttu-id="91e41-225">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-225">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Configure single sign-on")</span></span>

16. <span data-ttu-id="91e41-226">Utför följande steg hello på hello SAML2 Update1 dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="91e41-226">On hello SAML2 Update1 Properties dialog, perform hello following steps:</span></span>
    
     <span data-ttu-id="91e41-227">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-227">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Configure single sign-on")</span></span>

    <span data-ttu-id="91e41-228">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-228">a.</span></span> <span data-ttu-id="91e41-229">i hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="91e41-229">in hello **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="91e41-230">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-230">b.</span></span> <span data-ttu-id="91e41-231">I hello **Info** textruta typen **e-post** eller **user_name**, beroende på vilket fält används toouniquely identifiera användare i din ServiceNow-distribution.</span><span class="sxs-lookup"><span data-stu-id="91e41-231">In hello **User Field** textbox, type **email** or **user_name**, depending on which field is used toouniquely identify users in your ServiceNow deployment.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="91e41-232">Du kan configue Azure AD tooemit antingen hello Azure AD användar-ID (användarens huvudnamn) eller hello e-postadress som hello Unik identifierare i hello SAML-token genom att gå toohello **ServiceNow > attribut > enkel inloggning** avsnitt i Hej klassiska Azure-portalen och mappningen hello önskad fältet toohello **nameidentifier** attribut.</span><span class="sxs-lookup"><span data-stu-id="91e41-232">You can configue Azure AD tooemit either hello Azure AD user ID (user principal name) or hello email address as hello unique identifier in hello SAML token by going toohello **ServiceNow > Attributes > Single Sign-On** section of hello Azure classic portal and mapping hello desired field toohello **nameidentifier** attribute.</span></span> <span data-ttu-id="91e41-233">hello-värde som lagras för markerade hello-attributet i Azure AD (t.ex. användarens huvudnamn) måste matcha hello-värde som lagras i ServiceNow för hello angivna fält (t.ex. användarnamn)</span><span class="sxs-lookup"><span data-stu-id="91e41-233">hello value stored for hello selected attribute in Azure AD (e.g. user principal name) must match hello value stored in ServiceNow for hello entered field (e.g. user_name)</span></span>

    <span data-ttu-id="91e41-234">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-234">c.</span></span> <span data-ttu-id="91e41-235">I klassiska hello Azure AD-portalen kopiera hello **identitet Provider-ID** värdet och klistrar in den i hello **identitet providern URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-235">In hello Azure AD classic portal, copy hello **Identity Provider ID** value, and then paste it into hello **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="91e41-236">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-236">d.</span></span> <span data-ttu-id="91e41-237">I klassiska hello Azure AD-portalen kopiera hello **autentiserings-URL för begäran** värdet och klistrar in den i hello **identitetsleverantörens AuthnRequest** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-237">In hello Azure AD classic portal, copy hello **Authentication Request URL** value, and then paste it into hello **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="91e41-238">e.</span><span class="sxs-lookup"><span data-stu-id="91e41-238">e.</span></span> <span data-ttu-id="91e41-239">I klassiska hello Azure AD-portalen kopiera hello **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **identitetsleverantörens SingleLogoutRequest** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-239">In hello Azure AD classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="91e41-240">f.</span><span class="sxs-lookup"><span data-stu-id="91e41-240">f.</span></span> <span data-ttu-id="91e41-241">I hello **ServiceNow Homepage** textruta typen hello Webbadressen till startsidan för ServiceNow-instans.</span><span class="sxs-lookup"><span data-stu-id="91e41-241">In hello **ServiceNow Homepage** textbox, type hello URL of your ServiceNow instance homepage.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="91e41-242">Hej ServiceNow instans webbsida är en sammansättning av din **ServieNow klient URL** och **/navpage.do** (t.ex.:`https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="91e41-242">hello ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.:`https://fabrikam.service-now.com/navpage.do`).</span></span>

    <span data-ttu-id="91e41-243">g.</span><span class="sxs-lookup"><span data-stu-id="91e41-243">g.</span></span> <span data-ttu-id="91e41-244">I hello **enhets-ID / utfärdaren** textruta typen hello URL för din ServiceNow-klient.</span><span class="sxs-lookup"><span data-stu-id="91e41-244">In hello **Entity ID / Issuer** textbox, type hello URL of your ServiceNow tenant.</span></span>

    <span data-ttu-id="91e41-245">h.</span><span class="sxs-lookup"><span data-stu-id="91e41-245">h.</span></span> <span data-ttu-id="91e41-246">I hello **målgruppen URL** textruta typen hello URL för din ServiceNow-klient.</span><span class="sxs-lookup"><span data-stu-id="91e41-246">In hello **Audience URL** textbox, type hello URL of your ServiceNow tenant.</span></span> 

    <span data-ttu-id="91e41-247">Jag.</span><span class="sxs-lookup"><span data-stu-id="91e41-247">i.</span></span> <span data-ttu-id="91e41-248">I hello **protokollet bindningen för hello IDP'S SingleLogoutRequest** textruta typen **urn: oasis: namn: tc: SAML:2.0:bindings:HTTP-omdirigera**.</span><span class="sxs-lookup"><span data-stu-id="91e41-248">In hello **Protocol Binding for hello IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>

    <span data-ttu-id="91e41-249">j.</span><span class="sxs-lookup"><span data-stu-id="91e41-249">j.</span></span> <span data-ttu-id="91e41-250">Skriv i hello NameID princip textruta **urn: oasis: namn: tc: SAML:1.1:nameid-format: Okänt**.</span><span class="sxs-lookup"><span data-stu-id="91e41-250">In hello NameID Policy textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>

    <span data-ttu-id="91e41-251">k.</span><span class="sxs-lookup"><span data-stu-id="91e41-251">k.</span></span> <span data-ttu-id="91e41-252">Avmarkera **skapa en AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="91e41-252">Deselect **Create an AuthnContextClass**.</span></span>

    <span data-ttu-id="91e41-253">l.</span><span class="sxs-lookup"><span data-stu-id="91e41-253">l.</span></span> <span data-ttu-id="91e41-254">I hello **AuthnContextClassRef metoden**, typen `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span><span class="sxs-lookup"><span data-stu-id="91e41-254">In hello **AuthnContextClassRef Method**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span></span> <span data-ttu-id="91e41-255">Det här krävs bara om du endast organisation i molnet.</span><span class="sxs-lookup"><span data-stu-id="91e41-255">This is only needed if you are cloud only organization.</span></span> <span data-ttu-id="91e41-256">Om du använder en lokal AD FS eller MFA för autentisering bör du inte konfigurera det här värdet.</span><span class="sxs-lookup"><span data-stu-id="91e41-256">If you are using on-premises ADFS or MFA for authentication then you should not configure this value.</span></span> 

    <span data-ttu-id="91e41-257">m.</span><span class="sxs-lookup"><span data-stu-id="91e41-257">m.</span></span> <span data-ttu-id="91e41-258">I **klockan skeva** textruta typen **60**.</span><span class="sxs-lookup"><span data-stu-id="91e41-258">In **Clock Skew** textbox, type **60**.</span></span>

    <span data-ttu-id="91e41-259">n.</span><span class="sxs-lookup"><span data-stu-id="91e41-259">n.</span></span> <span data-ttu-id="91e41-260">Som **enkel inloggning på skriptet**väljer **MultiSSO_SAML2_Update1**.</span><span class="sxs-lookup"><span data-stu-id="91e41-260">As **Single Sign On Script**, select **MultiSSO_SAML2_Update1**.</span></span>

    <span data-ttu-id="91e41-261">o.</span><span class="sxs-lookup"><span data-stu-id="91e41-261">o.</span></span> <span data-ttu-id="91e41-262">Som **x509 certifikat**väljer hello-certifikat som du har skapat i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="91e41-262">As **x509 Certificate**, select hello certificate you have created in hello previous step.</span></span>

    <span data-ttu-id="91e41-263">p.</span><span class="sxs-lookup"><span data-stu-id="91e41-263">p.</span></span> <span data-ttu-id="91e41-264">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="91e41-264">Click **Submit**.</span></span> 

1. <span data-ttu-id="91e41-265">Välj hello konfiguration för enkel inloggning bekräftelse på klassiska hello Azure AD-portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-265">On hello Azure AD classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="91e41-266">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-266">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="91e41-267">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="91e41-267">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="91e41-268">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-268">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a><span data-ttu-id="91e41-269">Konfigurera Azure AD-Single Sign-On för ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="91e41-269">Configuring Azure AD Single Sign-On for ServiceNow Express</span></span>
1. <span data-ttu-id="91e41-270">I klassiska hello Azure AD-portalen, på hello **ServiceNow** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan .</span><span class="sxs-lookup"><span data-stu-id="91e41-270">In hello Azure AD classic portal, on hello **ServiceNow** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="91e41-271">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-271">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="91e41-272">På hello **hur skulle du som användare toosign på tooServiceNow** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-272">On hello **How would you like users toosign on tooServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="91e41-273">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-273">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="91e41-274">På hello **konfigurera Appinställningar** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-274">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="91e41-275">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-275">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="91e41-276">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-276">a.</span></span> <span data-ttu-id="91e41-277">i hello **ServiceNow logga URL** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="91e41-277">in hello **ServiceNow Sign On URL** textbox, type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="91e41-278">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-278">b.</span></span> <span data-ttu-id="91e41-279">I hello **utfärdar-URL** textruta, Skriv URL: en används av användare toosign på tooyour ServiceNow programmet enligt hello `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="91e41-279">In hello **Issuer URL** textbox,type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="91e41-280">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-280">c.</span></span> <span data-ttu-id="91e41-281">Klicka på **Nästa**</span><span class="sxs-lookup"><span data-stu-id="91e41-281">Click **Next**</span></span>

4. <span data-ttu-id="91e41-282">Klicka på **manuellt konfigurera hello program för enkel inloggning**, klicka på **nästa** och fullständig hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="91e41-282">Click **Manually configure hello application for single sign-on**, then click **Next** and complete hello following steps.</span></span>
   
    <span data-ttu-id="91e41-283">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-283">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="91e41-284">På hello **Konfigurera enkel inloggning på ServiceNow** klickar du på **hämta certifikat**, spara hello certifikatfilen lokalt på datorn och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-284">On hello **Configure single sign-on at ServiceNow** page, click **Download certificate**, save hello certificate file locally on your computer, and then click **Next**.</span></span>
   
    <span data-ttu-id="91e41-285">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-285">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="91e41-286">Inloggning tooyour ServiceNow Express-program som administratör.</span><span class="sxs-lookup"><span data-stu-id="91e41-286">Sign-on tooyour ServiceNow Express application as an administrator.</span></span>

7. <span data-ttu-id="91e41-287">Hello navigeringsfönstret hello vänster, klicka på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="91e41-287">In hello navigation pane on hello left side, click **Single Sign-On**.</span></span>  
   
    <span data-ttu-id="91e41-288">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-288">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Configure app URL")</span></span>

8. <span data-ttu-id="91e41-289">På hello **enkel inloggning** dialogrutan klickar du på hello konfigurationsikon på hello övre högra och ange hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="91e41-289">On hello **Single Sign-On** dialog, click hello configuration icon on hello upper right and set hello following properties:</span></span>
   
    <span data-ttu-id="91e41-290">![Konfigurera app-URL](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "konfigurera app-URL")</span><span class="sxs-lookup"><span data-stu-id="91e41-290">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Configure app URL")</span></span>
   
    <span data-ttu-id="91e41-291">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-291">a.</span></span> <span data-ttu-id="91e41-292">Växla **aktivera flera providern SSO** toohello höger.</span><span class="sxs-lookup"><span data-stu-id="91e41-292">Toggle **Enable multiple provider SSO** toohello right.</span></span>
   
    <span data-ttu-id="91e41-293">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-293">b.</span></span> <span data-ttu-id="91e41-294">Växla **Aktivera felsökningsloggning för hello flera providern SSO integration** toohello höger.</span><span class="sxs-lookup"><span data-stu-id="91e41-294">Toggle **Enable debug logging for hello multiple provider SSO integration** toohello right.</span></span>
   
    <span data-ttu-id="91e41-295">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-295">c.</span></span> <span data-ttu-id="91e41-296">I **hello fält hello användaren tabell som...**  textruta typen **user_name**.</span><span class="sxs-lookup"><span data-stu-id="91e41-296">In **hello field on hello user table that...** textbox, type **user_name**.</span></span>
9. <span data-ttu-id="91e41-297">På hello **enkel inloggning** dialogrutan klickar du på **Lägg till nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="91e41-297">On hello **Single Sign-On** dialog, click **Add New Certificate**.</span></span>
   
    <span data-ttu-id="91e41-298">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-298">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Configure single sign-on")</span></span>
10. <span data-ttu-id="91e41-299">På hello **X.509-certifikat** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-299">On hello **X.509 Certificates** dialog, perform hello following steps:</span></span>
    
    <span data-ttu-id="91e41-300">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-300">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
    <span data-ttu-id="91e41-301">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-301">a.</span></span> <span data-ttu-id="91e41-302">I hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="91e41-302">In hello **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
    <span data-ttu-id="91e41-303">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-303">b.</span></span> <span data-ttu-id="91e41-304">Välj **Active**.</span><span class="sxs-lookup"><span data-stu-id="91e41-304">Select **Active**.</span></span>
    
    <span data-ttu-id="91e41-305">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-305">c.</span></span> <span data-ttu-id="91e41-306">Som **Format**väljer **PEM**.</span><span class="sxs-lookup"><span data-stu-id="91e41-306">As **Format**, select **PEM**.</span></span>
    
    <span data-ttu-id="91e41-307">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-307">d.</span></span> <span data-ttu-id="91e41-308">Som **typen**väljer **förtroende Store Cert**.</span><span class="sxs-lookup"><span data-stu-id="91e41-308">As **Type**, select **Trust Store Cert**.</span></span>
    
    <span data-ttu-id="91e41-309">e.</span><span class="sxs-lookup"><span data-stu-id="91e41-309">e.</span></span> <span data-ttu-id="91e41-310">Skapa en Base64-kodad fil från din hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="91e41-310">Create a Base64 encoded file from your downloaded certificate.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="91e41-311">Mer information finns i [hur tooconvert en binär certifikat i en textfil](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="91e41-311">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>
    > 
    > 
    
    <span data-ttu-id="91e41-312">f.</span><span class="sxs-lookup"><span data-stu-id="91e41-312">f.</span></span> <span data-ttu-id="91e41-313">Öppna Base64-kodat certifikat i anteckningar, kopiera hello innehållet i den i Urklipp, och klistra in den toohello **PEM certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-313">Open your Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **PEM Certificate** textbox.</span></span>
    
    <span data-ttu-id="91e41-314">g.</span><span class="sxs-lookup"><span data-stu-id="91e41-314">g.</span></span> <span data-ttu-id="91e41-315">Klicka på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="91e41-315">Click **Update**.</span></span>
11. <span data-ttu-id="91e41-316">På hello **enkel inloggning** dialogrutan klickar du på **lägga till nya IdP**.</span><span class="sxs-lookup"><span data-stu-id="91e41-316">On hello **Single Sign-On** dialog, click **Add New IdP**.</span></span>
    
    <span data-ttu-id="91e41-317">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-317">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Configure single sign-on")</span></span>
12. <span data-ttu-id="91e41-318">På hello **lägga till nya identitetsleverantör** dialogrutan under **konfigurera identitetsleverantören**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-318">On hello **Add New Identity Provider** dialog, under **Configure Identity Provider**, perform hello following steps:</span></span>
    
    <span data-ttu-id="91e41-319">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-319">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Configure single sign-on")</span></span>

    <span data-ttu-id="91e41-320">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-320">a.</span></span> <span data-ttu-id="91e41-321">i hello **namn** textruta, ange ett namn för din konfiguration (t.ex.: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="91e41-321">In hello **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="91e41-322">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-322">b.</span></span> <span data-ttu-id="91e41-323">I klassiska hello Azure AD-portalen kopiera hello **identitet Provider-ID** värdet och klistrar in den i hello **identitet providern URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-323">In hello Azure AD classic portal, copy hello **Identity Provider ID** value, and then paste it into hello **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="91e41-324">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-324">c.</span></span> <span data-ttu-id="91e41-325">I klassiska hello Azure AD-portalen kopiera hello **autentiserings-URL för begäran** värdet och klistrar in den i hello **identitetsleverantörens AuthnRequest** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-325">In hello Azure AD classic portal, copy hello **Authentication Request URL** value, and then paste it into hello **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="91e41-326">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-326">d.</span></span> <span data-ttu-id="91e41-327">I klassiska hello Azure AD-portalen kopiera hello **tjänst-URL för enkel Sign-Out** värdet och klistrar in den i hello **identitetsleverantörens SingleLogoutRequest** textruta.</span><span class="sxs-lookup"><span data-stu-id="91e41-327">In hello Azure AD classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="91e41-328">e.</span><span class="sxs-lookup"><span data-stu-id="91e41-328">e.</span></span> <span data-ttu-id="91e41-329">Som **providern identitetscertifikat**väljer hello-certifikat som du har skapat i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="91e41-329">As **Identity Provider Certificate**, select hello certificate you have created in hello previous step.</span></span>


1. <span data-ttu-id="91e41-330">Klicka på **avancerade inställningar**, och under **ytterligare identitetsegenskaper providern**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-330">Click **Advanced Settings**, and under **Additional Identity Provider Properties**, perform hello following steps:</span></span>
   
    <span data-ttu-id="91e41-331">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-331">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="91e41-332">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-332">a.</span></span> <span data-ttu-id="91e41-333">I hello **protokollet bindningen för hello IDP'S SingleLogoutRequest** textruta typen **urn: oasis: namn: tc: SAML:2.0:bindings:HTTP-omdirigera**.</span><span class="sxs-lookup"><span data-stu-id="91e41-333">In hello **Protocol Binding for hello IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>
   
    <span data-ttu-id="91e41-334">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-334">b.</span></span> <span data-ttu-id="91e41-335">I hello **NameID princip** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: Okänt**.</span><span class="sxs-lookup"><span data-stu-id="91e41-335">In hello **NameID Policy** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>    
   
    <span data-ttu-id="91e41-336">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-336">c.</span></span> <span data-ttu-id="91e41-337">I hello **AuthnContextClassRef metoden**, typen **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span><span class="sxs-lookup"><span data-stu-id="91e41-337">In hello **AuthnContextClassRef Method**, type **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span></span>
   
    <span data-ttu-id="91e41-338">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-338">d.</span></span> <span data-ttu-id="91e41-339">Avmarkera **skapa en AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="91e41-339">Deselect **Create an AuthnContextClass**.</span></span>

2. <span data-ttu-id="91e41-340">Under **ytterligare egenskaper**, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-340">Under **Additional Service Provider Properties**, perform hello following steps:</span></span>
   
    <span data-ttu-id="91e41-341">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-341">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="91e41-342">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-342">a.</span></span> <span data-ttu-id="91e41-343">I hello **ServiceNow Homepage** textruta typen hello Webbadressen till startsidan för ServiceNow-instans.</span><span class="sxs-lookup"><span data-stu-id="91e41-343">In hello **ServiceNow Homepage** textbox, type hello URL of your ServiceNow instance homepage.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="91e41-344">Hej ServiceNow instans webbsida är en sammansättning av din **ServieNow klient URL** och **/navpage.do** (t.ex.: `https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="91e41-344">hello ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.: `https://fabrikam.service-now.com/navpage.do`).</span></span>
    > 
    > 
   
    <span data-ttu-id="91e41-345">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-345">b.</span></span> <span data-ttu-id="91e41-346">I hello **enhets-ID / utfärdaren** textruta typen hello URL för din ServiceNow-klient.</span><span class="sxs-lookup"><span data-stu-id="91e41-346">In hello **Entity ID / Issuer** textbox, type hello URL of your ServiceNow tenant.</span></span>
   
    <span data-ttu-id="91e41-347">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-347">c.</span></span> <span data-ttu-id="91e41-348">I hello **Målgrupps-URI** textruta typen hello URL för din ServiceNow-klient.</span><span class="sxs-lookup"><span data-stu-id="91e41-348">In hello **Audience URI** textbox, type hello URL of your ServiceNow tenant.</span></span> 
   
    <span data-ttu-id="91e41-349">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-349">d.</span></span> <span data-ttu-id="91e41-350">I **klockan skeva** textruta typen **60**.</span><span class="sxs-lookup"><span data-stu-id="91e41-350">In **Clock Skew** textbox, type **60**.</span></span>
   
    <span data-ttu-id="91e41-351">e.</span><span class="sxs-lookup"><span data-stu-id="91e41-351">e.</span></span> <span data-ttu-id="91e41-352">I hello **Info** textruta typen **e-post** eller **user_name**, beroende på vilket fält används toouniquely identifiera användare i din ServiceNow-distribution.</span><span class="sxs-lookup"><span data-stu-id="91e41-352">In hello **User Field** textbox, type **email** or **user_name**, depending on which field is used toouniquely identify users in your ServiceNow deployment.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="91e41-353">Du kan configue Azure AD tooemit antingen hello Azure AD användar-ID (användarens huvudnamn) eller hello e-postadress som hello Unik identifierare i hello SAML-token genom att gå toohello **ServiceNow > attribut > enkel inloggning** avsnitt i Hej klassiska Azure-portalen och mappningen hello önskad fältet toohello **nameidentifier** attribut.</span><span class="sxs-lookup"><span data-stu-id="91e41-353">You can configue Azure AD tooemit either hello Azure AD user ID (user principal name) or hello email address as hello unique identifier in hello SAML token by going toohello **ServiceNow > Attributes > Single Sign-On** section of hello Azure classic portal and mapping hello desired field toohello **nameidentifier** attribute.</span></span> <span data-ttu-id="91e41-354">hello-värde som lagras för markerade hello-attributet i Azure AD (t.ex. användarens huvudnamn) måste matcha hello-värde som lagras i ServiceNow för hello angivna fält (t.ex. användarnamn)</span><span class="sxs-lookup"><span data-stu-id="91e41-354">hello value stored for hello selected attribute in Azure AD (e.g. user principal name) must match hello value stored in ServiceNow for hello entered field (e.g. user_name)</span></span>
    > 
    > 
   
    <span data-ttu-id="91e41-355">f.</span><span class="sxs-lookup"><span data-stu-id="91e41-355">f.</span></span> <span data-ttu-id="91e41-356">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="91e41-356">Click **Save**.</span></span> 

3. <span data-ttu-id="91e41-357">Välj hello konfiguration för enkel inloggning bekräftelse på klassiska hello Azure AD-portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-357">On hello Azure AD classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="91e41-358">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-358">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

4. <span data-ttu-id="91e41-359">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="91e41-359">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="91e41-360">![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Konfigurera enkel inloggning")</span><span class="sxs-lookup"><span data-stu-id="91e41-360">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="91e41-361">Konfigurera användaretablering</span><span class="sxs-lookup"><span data-stu-id="91e41-361">Configuring user provisioning</span></span>
<span data-ttu-id="91e41-362">hello syftet med det här avsnittet är toooutline hur tooenable användaretablering Active Directory-användare konton tooServiceNow.</span><span class="sxs-lookup"><span data-stu-id="91e41-362">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooServiceNow.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="91e41-363">tooconfigure användaretablering, utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="91e41-363">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="91e41-364">I hello Azure Management klassiska portalen på hello **ServiceNow** integreringssidan för programmet, klickar du på **konfigurera användaretablering**.</span><span class="sxs-lookup"><span data-stu-id="91e41-364">In hello Azure Management classic portal, on hello **ServiceNow** application integration page, click **Configure user provisioning**.</span></span> 
   
    <span data-ttu-id="91e41-365">![Användaretablering](./media/active-directory-saas-servicenow-tutorial/IC769498.png "användaretablering")</span><span class="sxs-lookup"><span data-stu-id="91e41-365">![User provisioning](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")</span></span>

2. <span data-ttu-id="91e41-366">På hello **ange ditt ServiceNow autentiseringsuppgifter tooenable automatisk användaretablering** anger hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="91e41-366">On hello **Enter your ServiceNow credentials tooenable automatic user provisioning** page, provide hello following configuration settings:</span></span>
   
     <span data-ttu-id="91e41-367">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-367">a.</span></span> <span data-ttu-id="91e41-368">I hello **ServiceNow instansnamn** textruta typen hello ServiceNow-instansnamnet.</span><span class="sxs-lookup"><span data-stu-id="91e41-368">In hello **ServiceNow Instance Name** textbox, type hello ServiceNow instance name.</span></span>
   
     <span data-ttu-id="91e41-369">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-369">b.</span></span> <span data-ttu-id="91e41-370">I hello **ServiceNow-administratörsanvändarnamnet** textruta hello-typnamn för hello ServiceNow-administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="91e41-370">In hello **ServiceNow Admin User Name** textbox, type hello name of hello ServiceNow admin account.</span></span>
   
     <span data-ttu-id="91e41-371">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-371">c.</span></span> <span data-ttu-id="91e41-372">I hello **ServiceNow adminlösenord** textruta typen hello lösenordet för kontot.</span><span class="sxs-lookup"><span data-stu-id="91e41-372">In hello **ServiceNow Admin Password** textbox, type hello password for this account.</span></span>
   
     <span data-ttu-id="91e41-373">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-373">d.</span></span> <span data-ttu-id="91e41-374">Klicka på **Validera** tooverify din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="91e41-374">Click **validate** tooverify your configuration.</span></span>
   
     <span data-ttu-id="91e41-375">e.</span><span class="sxs-lookup"><span data-stu-id="91e41-375">e.</span></span> <span data-ttu-id="91e41-376">Klicka på hello **nästa** knappen tooopen hello **nästa steg** sidan.</span><span class="sxs-lookup"><span data-stu-id="91e41-376">Click hello **Next** button tooopen hello **Next steps** page.</span></span>
   
     <span data-ttu-id="91e41-377">f.</span><span class="sxs-lookup"><span data-stu-id="91e41-377">f.</span></span> <span data-ttu-id="91e41-378">Om du vill tooprovision alla användare toothis program, väljer du ”**automatiskt etablera alla användarkonton i hello directory toothis program**”.</span><span class="sxs-lookup"><span data-stu-id="91e41-378">If you want tooprovision all users toothis application, select “**Automatically provision all user accounts in hello directory toothis application**”.</span></span> 
   
    <span data-ttu-id="91e41-379">![Nästa steg](./media/active-directory-saas-servicenow-tutorial/IC698804.png "nästa steg")</span><span class="sxs-lookup"><span data-stu-id="91e41-379">![Next Steps](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")</span></span>
   
     <span data-ttu-id="91e41-380">g.</span><span class="sxs-lookup"><span data-stu-id="91e41-380">g.</span></span> <span data-ttu-id="91e41-381">På hello **nästa steg** klickar du på **Slutför** toosave din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="91e41-381">On hello **Next steps** page, click **Complete** toosave your configuration.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="91e41-382">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="91e41-382">Creating an Azure AD test user</span></span>
<span data-ttu-id="91e41-383">I det här avsnittet skapar du en testanvändare i hello klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="91e41-383">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="91e41-385">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91e41-385">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e41-386">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="91e41-386">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="91e41-388">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="91e41-388">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="91e41-389">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="91e41-389">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="91e41-391">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="91e41-391">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="91e41-393">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-393">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="91e41-395">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-395">a.</span></span> <span data-ttu-id="91e41-396">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="91e41-396">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="91e41-397">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-397">b.</span></span> <span data-ttu-id="91e41-398">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="91e41-398">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="91e41-399">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-399">c.</span></span> <span data-ttu-id="91e41-400">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-400">Click **Next**.</span></span>

6. <span data-ttu-id="91e41-401">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-401">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   <span data-ttu-id="91e41-403">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-403">a.</span></span> <span data-ttu-id="91e41-404">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="91e41-404">In hello **First Name** textbox, type **Britta**.</span></span>  
   
   <span data-ttu-id="91e41-405">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-405">b.</span></span> <span data-ttu-id="91e41-406">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="91e41-406">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
   <span data-ttu-id="91e41-407">c.</span><span class="sxs-lookup"><span data-stu-id="91e41-407">c.</span></span> <span data-ttu-id="91e41-408">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="91e41-408">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="91e41-409">d.</span><span class="sxs-lookup"><span data-stu-id="91e41-409">d.</span></span> <span data-ttu-id="91e41-410">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="91e41-410">In hello **Role** list, select **User**.</span></span>
   
   <span data-ttu-id="91e41-411">e.</span><span class="sxs-lookup"><span data-stu-id="91e41-411">e.</span></span> <span data-ttu-id="91e41-412">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-412">Click **Next**.</span></span>

7. <span data-ttu-id="91e41-413">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="91e41-413">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="91e41-415">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="91e41-415">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="91e41-417">a.</span><span class="sxs-lookup"><span data-stu-id="91e41-417">a.</span></span> <span data-ttu-id="91e41-418">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="91e41-418">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="91e41-419">b.</span><span class="sxs-lookup"><span data-stu-id="91e41-419">b.</span></span> <span data-ttu-id="91e41-420">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="91e41-420">Click **Complete**.</span></span>   

### <a name="creating-a-servicenow-test-user"></a><span data-ttu-id="91e41-421">Skapa en testanvändare ServiceNow</span><span class="sxs-lookup"><span data-stu-id="91e41-421">Creating a ServiceNow test user</span></span>
<span data-ttu-id="91e41-422">I det här avsnittet skapar du en användare som kallas Britta Simon i ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="91e41-422">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="91e41-423">I det här avsnittet skapar du en användare som kallas Britta Simon i ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="91e41-423">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="91e41-424">Om du inte vet hur tooadd i ditt ServiceNow eller ServiceNow Express användarkonton, Kontakta supportteamet för ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="91e41-424">If you don't know how tooadd a user in your ServiceNow or ServiceNow Express account, contact ServiceNow support team.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="91e41-425">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="91e41-425">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="91e41-426">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooServiceNow sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="91e41-426">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooServiceNow.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="91e41-428">**tooassign Britta Simon tooServiceNow utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="91e41-428">**tooassign Britta Simon tooServiceNow, perform hello following steps:**</span></span>

1. <span data-ttu-id="91e41-429">På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="91e41-429">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Tilldela användare][201] 

2. <span data-ttu-id="91e41-431">Välj i listan med program hello **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="91e41-431">In hello applications list, select **ServiceNow**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. <span data-ttu-id="91e41-433">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="91e41-433">In hello menu on hello top, click **Users**.</span></span>
   
    ![Tilldela användare][203] 

4. <span data-ttu-id="91e41-435">Välj i listan för alla användare av hello **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="91e41-435">In hello All Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="91e41-436">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="91e41-436">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="91e41-438">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="91e41-438">Testing single sign-on</span></span>
<span data-ttu-id="91e41-439">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="91e41-439">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="91e41-440">Du bör få automatiskt inloggade tooyour ServiceNow programmet när du klickar på hello ServiceNow-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="91e41-440">When you click hello ServiceNow tile in hello Access Panel, you should get automatically signed-on tooyour ServiceNow application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="91e41-441">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="91e41-441">Additional resources</span></span>
* [<span data-ttu-id="91e41-442">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="91e41-442">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="91e41-443">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="91e41-443">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
