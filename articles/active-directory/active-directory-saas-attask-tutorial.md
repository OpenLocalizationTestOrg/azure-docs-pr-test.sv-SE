---
title: "Självstudier: Azure Active Directory-integrering med @Task| Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och @Task."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: aab8bd2f-f9dd-42da-a18e-d707865687d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 0840763622086a02a27cfafff3b741bc66cec498
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-task"></a><span data-ttu-id="1df4e-103">Självstudier: Azure Active Directory-integrering med@Task</span><span class="sxs-lookup"><span data-stu-id="1df4e-103">Tutorial: Azure Active Directory integration with @Task</span></span>
<span data-ttu-id="1df4e-104">hello syftet med den här kursen är tooshow du hur toointegrate @Task med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="1df4e-104">hello objective of this tutorial is tooshow you how toointegrate @Task with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="1df4e-105">Integrera @Task med Azure AD tillhandahåller hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="1df4e-105">Integrating @Task with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="1df4e-106">Du kan styra i Azure AD som har åtkomsttoo@Task</span><span class="sxs-lookup"><span data-stu-id="1df4e-106">You can control in Azure AD who has access too@Task</span></span>
* <span data-ttu-id="1df4e-107">Du kan aktivera användarna tooautomatically hämta inloggade too@Task (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="1df4e-107">You can enable your users tooautomatically get signed-on too@Task (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="1df4e-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1df4e-108">You can manage your accounts in one central location - hello Azure classic Portal</span></span>

<span data-ttu-id="1df4e-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1df4e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1df4e-110">Krav</span><span class="sxs-lookup"><span data-stu-id="1df4e-110">Prerequisites</span></span>
<span data-ttu-id="1df4e-111">tooconfigure Azure AD-integrering med @Task, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="1df4e-111">tooconfigure Azure AD integration with @Task, you need hello following items:</span></span>

* <span data-ttu-id="1df4e-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="1df4e-112">An Azure AD subscription</span></span>
* <span data-ttu-id="1df4e-113">En @Task enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="1df4e-113">An @Task single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1df4e-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="1df4e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="1df4e-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="1df4e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="1df4e-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="1df4e-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="1df4e-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1df4e-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="1df4e-118">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="1df4e-118">Scenario Description</span></span>
<span data-ttu-id="1df4e-119">hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="1df4e-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="1df4e-120">hello-scenario som beskrivs i den här kursen består av tre huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="1df4e-120">hello scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="1df4e-121">Lägga till @Task från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1df4e-121">Adding @Task from hello gallery</span></span> 
2. <span data-ttu-id="1df4e-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1df4e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-task-from-hello-gallery"></a><span data-ttu-id="1df4e-123">Lägga till @Task från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="1df4e-123">Adding @Task from hello gallery</span></span>
<span data-ttu-id="1df4e-124">tooconfigure hello integrering av @Task till Azure AD, behöver du tooadd @Task hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="1df4e-124">tooconfigure hello integration of @Task into Azure AD, you need tooadd @Task from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="1df4e-125">**tooadd @Task från galleriet hello utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1df4e-125">**tooadd @Task from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="1df4e-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1] 
2. <span data-ttu-id="1df4e-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="1df4e-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="1df4e-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="1df4e-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Program][2] 
4. <span data-ttu-id="1df4e-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="1df4e-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3] 
5. <span data-ttu-id="1df4e-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Program][4] 
6. <span data-ttu-id="1df4e-135">Skriv i sökrutan hello  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="1df4e-135">In hello search box, type **@Task**.</span></span>
   
    ![Program][5] 
7. <span data-ttu-id="1df4e-137">I resultatfönstret hello väljer  **@Task** , och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="1df4e-137">In hello results pane, select **@Task**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Program][30] 

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1df4e-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1df4e-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1df4e-140">hello syftet med det här avsnittet är tooshow du hur tooconfigure och testa Azure AD med enkel inloggning med @Task baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="1df4e-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with @Task based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="1df4e-141">För enkel inloggning toowork Azure AD måste tooknow vilka hello motsvarighet användare i @Task tooan användaren i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="1df4e-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in @Task tooan user in Azure AD is.</span></span> <span data-ttu-id="1df4e-142">Med andra ord en länk relation mellan en Azure AD-användare och hello relaterade användare i @Task måste toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="1df4e-142">In other words, a link relationship between an Azure AD user and hello related user in @Task needs toobe established.</span></span>   
<span data-ttu-id="1df4e-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i @Task.</span><span class="sxs-lookup"><span data-stu-id="1df4e-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in @Task.</span></span>

<span data-ttu-id="1df4e-144">tooconfigure och testa Azure AD enkel inloggning med @Task, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="1df4e-144">tooconfigure and test Azure AD single sign-on with @Task, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="1df4e-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="1df4e-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="1df4e-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1df4e-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1df4e-147">**[Skapa en @Tasktest användare](#creating-a-halogen-software-test-user)**  -toohave en motsvarighet för Britta Simon i @Taskthat är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="1df4e-147">**[Creating a @Tasktest user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in @Taskthat is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="1df4e-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="1df4e-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1df4e-149">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="1df4e-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1df4e-150">Konfigurera Azure AD-Single Sign-On</span><span class="sxs-lookup"><span data-stu-id="1df4e-150">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="1df4e-151">hello syftet med det här avsnittet är tooenable Azure AD enkel inloggning i hello klassiska Azure-portalen och tooconfigure enkel inloggning i din @Task program.</span><span class="sxs-lookup"><span data-stu-id="1df4e-151">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your @Task application.</span></span>

<span data-ttu-id="1df4e-152">**tooconfigure Azure AD enkel inloggning med @Task, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1df4e-152">**tooconfigure Azure AD single sign-on with @Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="1df4e-153">I hello klassiska Azure-portalen på hello  **@Task**  integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="1df4e-153">In hello Azure classic portal, on hello **@Task** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 
2. <span data-ttu-id="1df4e-155">På hello **hur skulle du som användare toosign på too@Task**  väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-155">On hello **How would you like users toosign on too@Task** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][7] 
3. <span data-ttu-id="1df4e-157">På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1df4e-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>
   
    ![Konfigurera Appinställningar för][8] 
   
     <span data-ttu-id="1df4e-159">a.</span><span class="sxs-lookup"><span data-stu-id="1df4e-159">a.</span></span> <span data-ttu-id="1df4e-160">I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour @Task program (t.ex.:*https://<Tenant name>.attask ondemand.com*).</span><span class="sxs-lookup"><span data-stu-id="1df4e-160">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour @Task application (e.g.:*https://<Tenant name>.attask-ondemand.com*).</span></span>
   
     <span data-ttu-id="1df4e-161">b.</span><span class="sxs-lookup"><span data-stu-id="1df4e-161">b.</span></span> <span data-ttu-id="1df4e-162">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-162">Click **Next**.</span></span>
4. <span data-ttu-id="1df4e-163">På hello **Konfigurera enkel inloggning vid @Task**  klickar du på **hämtar metadata**, spara hello metadatafil lokalt på datorn och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-163">On hello **Configure single sign-on at @Task** page, click **Download metadata**, save hello metadata file locally on your computer, and then click **Next**.</span></span>
   
    ![Vad är Azure AD Connect?][9] 
5. <span data-ttu-id="1df4e-165">Inloggning tooyour @Task företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="1df4e-165">Sign-on tooyour @Task company site as administrator.</span></span>
6. <span data-ttu-id="1df4e-166">Gå för**enkel inloggning på konfigurationen**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-166">Go too**Single Sign On Configuration**.</span></span>
7. <span data-ttu-id="1df4e-167">På hello **enkel inloggning** dialogrutan, utför följande steg hello</span><span class="sxs-lookup"><span data-stu-id="1df4e-167">On hello **Single Sign-On** dialog, perform hello following steps</span></span>
   
    ![Konfigurera enkel inloggning][23]
   
    <span data-ttu-id="1df4e-169">a.</span><span class="sxs-lookup"><span data-stu-id="1df4e-169">a.</span></span> <span data-ttu-id="1df4e-170">Som **typen**väljer **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-170">As **Type**, select **SAML 2.0**.</span></span>
   
    <span data-ttu-id="1df4e-171">b.</span><span class="sxs-lookup"><span data-stu-id="1df4e-171">b.</span></span> <span data-ttu-id="1df4e-172">Välj **Service Provider-ID**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-172">Select **Service Provider ID**.</span></span>
   
    <span data-ttu-id="1df4e-173">c.</span><span class="sxs-lookup"><span data-stu-id="1df4e-173">c.</span></span> <span data-ttu-id="1df4e-174">Kopiera hello på hello klassiska Azure-portalen, **Remote inloggnings-URL**, och klistra in den i hello **Portal Inloggningswebbadressen** textruta.</span><span class="sxs-lookup"><span data-stu-id="1df4e-174">On hello Azure classic portal, copy hello **Remote Login URL**, and then paste it into hello **Login Portal URL** textbox.</span></span>
   
    <span data-ttu-id="1df4e-175">d.</span><span class="sxs-lookup"><span data-stu-id="1df4e-175">d.</span></span> <span data-ttu-id="1df4e-176">Kopiera hello på hello klassiska Azure-portalen, **tjänst-URL för enkel Sign-Out**, och klistra in den i hello **Sign-Out URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="1df4e-176">On hello Azure classic portal, copy hello **Single Sign-Out Service URL**, and then paste it into hello **Sign-Out URL** textbox.</span></span>
   
    <span data-ttu-id="1df4e-177">e.</span><span class="sxs-lookup"><span data-stu-id="1df4e-177">e.</span></span> <span data-ttu-id="1df4e-178">Kopiera hello på hello klassiska Azure-portalen, **ändra lösenord URL**, och klistra in den i hello **ändra lösenord URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="1df4e-178">On hello Azure classic portal, copy hello **Change Password URL**, and then paste it into hello **Change Password URL** textbox.</span></span>
   
    <span data-ttu-id="1df4e-179">f.</span><span class="sxs-lookup"><span data-stu-id="1df4e-179">f.</span></span> <span data-ttu-id="1df4e-180">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-180">Click **Save**.</span></span>
8. <span data-ttu-id="1df4e-181">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-181">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Vad är Azure AD Connect?][10]
9. <span data-ttu-id="1df4e-183">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-183">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Vad är Azure AD Connect?][11]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1df4e-185">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="1df4e-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="1df4e-186">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="1df4e-186">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>  

![Skapa Azure AD-användare][20]

<span data-ttu-id="1df4e-188">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1df4e-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="1df4e-189">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-189">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 
2. <span data-ttu-id="1df4e-191">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="1df4e-191">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="1df4e-192">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-192">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
4. <span data-ttu-id="1df4e-194">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-194">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 
5. <span data-ttu-id="1df4e-196">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1df4e-196">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="1df4e-198">a.</span><span class="sxs-lookup"><span data-stu-id="1df4e-198">a.</span></span> <span data-ttu-id="1df4e-199">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="1df4e-199">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="1df4e-200">b.</span><span class="sxs-lookup"><span data-stu-id="1df4e-200">b.</span></span> <span data-ttu-id="1df4e-201">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-201">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="1df4e-202">c.</span><span class="sxs-lookup"><span data-stu-id="1df4e-202">c.</span></span> <span data-ttu-id="1df4e-203">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-203">Click **Next**.</span></span>
6. <span data-ttu-id="1df4e-204">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1df4e-204">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
   
    <span data-ttu-id="1df4e-206">a.</span><span class="sxs-lookup"><span data-stu-id="1df4e-206">a.</span></span> <span data-ttu-id="1df4e-207">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-207">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="1df4e-208">b.</span><span class="sxs-lookup"><span data-stu-id="1df4e-208">b.</span></span> <span data-ttu-id="1df4e-209">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-209">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="1df4e-210">c.</span><span class="sxs-lookup"><span data-stu-id="1df4e-210">c.</span></span> <span data-ttu-id="1df4e-211">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-211">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="1df4e-212">d.</span><span class="sxs-lookup"><span data-stu-id="1df4e-212">d.</span></span> <span data-ttu-id="1df4e-213">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-213">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="1df4e-214">e.</span><span class="sxs-lookup"><span data-stu-id="1df4e-214">e.</span></span> <span data-ttu-id="1df4e-215">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-215">Click **Next**.</span></span>

7. <span data-ttu-id="1df4e-216">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-216">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
8. <span data-ttu-id="1df4e-218">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1df4e-218">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="1df4e-220">a.</span><span class="sxs-lookup"><span data-stu-id="1df4e-220">a.</span></span> <span data-ttu-id="1df4e-221">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-221">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="1df4e-222">b.</span><span class="sxs-lookup"><span data-stu-id="1df4e-222">b.</span></span> <span data-ttu-id="1df4e-223">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="1df4e-223">Click **Complete**.</span></span>   

### <a name="creating-an-task-test-user"></a><span data-ttu-id="1df4e-224">Skapa en @Task testanvändare</span><span class="sxs-lookup"><span data-stu-id="1df4e-224">Creating an @Task test user</span></span>
<span data-ttu-id="1df4e-225">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i @Task.</span><span class="sxs-lookup"><span data-stu-id="1df4e-225">hello objective of this section is toocreate a user called Britta Simon in @Task.</span></span>

<span data-ttu-id="1df4e-226">**toocreate en användare som kallas Britta Simon i @Task, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1df4e-226">**toocreate a user called Britta Simon in @Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="1df4e-227">Logga in tooyour @Task företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="1df4e-227">Sign on tooyour @Task company site as administrator.</span></span>
2. <span data-ttu-id="1df4e-228">Hello-menyn överst hello **personer**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-228">In hello menu on hello top, click **People**.</span></span>
3. <span data-ttu-id="1df4e-229">Klicka på **ny Person**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-229">Click **New Person**.</span></span> 
4. <span data-ttu-id="1df4e-230">Utför följande hello på hello ny Person dialogrutan:</span><span class="sxs-lookup"><span data-stu-id="1df4e-230">On hello New Person dialog, perform hello following steps:</span></span>
   
    ![Skapa en @Task testanvändare][21] 
   
    <span data-ttu-id="1df4e-232">a.</span><span class="sxs-lookup"><span data-stu-id="1df4e-232">a.</span></span> <span data-ttu-id="1df4e-233">I hello **Förnamn** textruta skriver ”Britta”.</span><span class="sxs-lookup"><span data-stu-id="1df4e-233">In hello **First Name** textbox, type "Britta".</span></span>
   
    <span data-ttu-id="1df4e-234">b.</span><span class="sxs-lookup"><span data-stu-id="1df4e-234">b.</span></span> <span data-ttu-id="1df4e-235">I hello **efternamn** textruta skriver ”Simon”.</span><span class="sxs-lookup"><span data-stu-id="1df4e-235">In hello **Last Name** textbox, type "Simon".</span></span>
   
    <span data-ttu-id="1df4e-236">c.</span><span class="sxs-lookup"><span data-stu-id="1df4e-236">c.</span></span> <span data-ttu-id="1df4e-237">I hello **e-postadress** textruta Skriv Britta Simon e-postadress i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="1df4e-237">In hello **Email Address** textbox, type Britta Simon's email address in Azure Active Directory.</span></span>
   
    <span data-ttu-id="1df4e-238">d.</span><span class="sxs-lookup"><span data-stu-id="1df4e-238">d.</span></span> <span data-ttu-id="1df4e-239">Klicka på **lägga till personen**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-239">Click **Add Person**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="1df4e-240">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="1df4e-240">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="1df4e-241">hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access too@Task.</span><span class="sxs-lookup"><span data-stu-id="1df4e-241">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access too@Task.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="1df4e-243">**tooassign Britta Simon too@Task, utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="1df4e-243">**tooassign Britta Simon too@Task, perform hello following steps:**</span></span>

1. <span data-ttu-id="1df4e-244">På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="1df4e-244">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Tilldela användare][201] 
2. <span data-ttu-id="1df4e-246">Välj i listan med program hello  **@Task** .</span><span class="sxs-lookup"><span data-stu-id="1df4e-246">In hello applications list, select **@Task**.</span></span>
   
    ![Tilldela användare][202] 
3. <span data-ttu-id="1df4e-248">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-248">In hello menu on hello top, click **Users**.</span></span>
   
    ![Tilldela användare][203] 
4. <span data-ttu-id="1df4e-250">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-250">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="1df4e-251">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="1df4e-251">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="1df4e-253">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="1df4e-253">Testing Single Sign-On</span></span>
<span data-ttu-id="1df4e-254">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="1df4e-254">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="1df4e-255">När du klickar på hello @Task panelen i Hej åtkomstpanelen, du får automatiskt inloggade tooyour @Task program.</span><span class="sxs-lookup"><span data-stu-id="1df4e-255">When you click hello @Task tile in hello Access Panel, you should get automatically signed-on tooyour @Task application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1df4e-256">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1df4e-256">Additional Resources</span></span>
* [<span data-ttu-id="1df4e-257">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1df4e-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1df4e-258">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="1df4e-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






