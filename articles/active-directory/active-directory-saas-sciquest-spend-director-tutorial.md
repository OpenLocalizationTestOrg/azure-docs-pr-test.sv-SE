---
title: "Självstudier: Azure Active Directory-integrering med SciQuest tillbringar Director | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SciQuest tillbringar Director."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 47c46f1297054fd96b86c1d8c66e1a55ec151497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a><span data-ttu-id="2d495-103">Självstudier: Azure Active Directory-integrering med SciQuest tillbringar Director</span><span class="sxs-lookup"><span data-stu-id="2d495-103">Tutorial: Azure Active Directory integration with SciQuest Spend Director</span></span>
<span data-ttu-id="2d495-104">hello syftet med den här kursen är tooshow du hur toointegrate SciQuest tillbringar Director med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="2d495-104">hello objective of this tutorial is tooshow you how toointegrate SciQuest Spend Director with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="2d495-105">Integrera SciQuest tillbringar Director med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="2d495-105">Integrating SciQuest Spend Director with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="2d495-106">Du kan styra i Azure AD som har åtkomst tooSciQuest utgifter Director</span><span class="sxs-lookup"><span data-stu-id="2d495-106">You can control in Azure AD who has access tooSciQuest Spend Director</span></span> 
* <span data-ttu-id="2d495-107">Du kan aktivera din användare tooautomatically get inloggade tooSciQuest utgifter Director (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="2d495-107">You can enable your users tooautomatically get signed-on tooSciQuest Spend Director (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="2d495-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2d495-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="2d495-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2d495-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2d495-110">Krav</span><span class="sxs-lookup"><span data-stu-id="2d495-110">Prerequisites</span></span>
<span data-ttu-id="2d495-111">tooconfigure Azure AD-integrering med SciQuest tillbringar Director, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="2d495-111">tooconfigure Azure AD integration with SciQuest Spend Director, you need hello following items:</span></span>

* <span data-ttu-id="2d495-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="2d495-112">An Azure AD subscription</span></span>
* <span data-ttu-id="2d495-113">En SciQuest tillbringar Director enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="2d495-113">A SciQuest Spend Director single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2d495-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="2d495-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="2d495-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="2d495-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="2d495-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="2d495-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="2d495-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2d495-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="2d495-118">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="2d495-118">Scenario Description</span></span>
<span data-ttu-id="2d495-119">hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="2d495-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="2d495-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="2d495-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2d495-121">Att lägga till SciQuest tillbringar Director från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2d495-121">Adding SciQuest Spend Director from hello gallery</span></span> 
2. <span data-ttu-id="2d495-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d495-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciquest-spend-director-from-hello-gallery"></a><span data-ttu-id="2d495-123">Att lägga till SciQuest tillbringar Director från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="2d495-123">Adding SciQuest Spend Director from hello gallery</span></span>
<span data-ttu-id="2d495-124">tooconfigure hello integrering av SciQuest tillbringar Director i Azure AD, behöver du tooadd SciQuest tillbringar Director hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="2d495-124">tooconfigure hello integration of SciQuest Spend Director into Azure AD, you need tooadd SciQuest Spend Director from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2d495-125">**tooadd SciQuest tillbringar Director från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2d495-125">**tooadd SciQuest Spend Director from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d495-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2d495-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="2d495-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="2d495-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="2d495-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d495-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Program][2]

4. <span data-ttu-id="2d495-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="2d495-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3]

5. <span data-ttu-id="2d495-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="2d495-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Program][4]

6. <span data-ttu-id="2d495-135">Skriv i sökrutan hello **sciQuest tillbringar director**.</span><span class="sxs-lookup"><span data-stu-id="2d495-135">In hello search box, type **sciQuest spend director**.</span></span>
   
    ![Program][5]

7. <span data-ttu-id="2d495-137">I resultatfönstret hello väljer **SciQuest tillbringar Director**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="2d495-137">In hello results pane, select **SciQuest Spend Director**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Program][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2d495-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d495-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2d495-140">hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med SciQuest tillbringar Director baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="2d495-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with SciQuest Spend Director based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2d495-141">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SciQuest tillbringar Director tooan användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2d495-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SciQuest Spend Director tooan user in Azure AD is.</span></span> <span data-ttu-id="2d495-142">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i SciQuest tillbringar Director toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="2d495-142">In other words, a link relationship between an Azure AD user and hello related user in SciQuest Spend Director needs toobe established.</span></span>  
<span data-ttu-id="2d495-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="2d495-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SciQuest Spend Director.</span></span>

<span data-ttu-id="2d495-144">tooconfigure och testa Azure AD enkel inloggning med SciQuest tillbringar Director, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="2d495-144">tooconfigure and test Azure AD single sign-on with SciQuest Spend Director, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2d495-145">**[Konfigurera Azure AD enda enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="2d495-145">**[Configuring Azure AD Single Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2d495-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d495-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2d495-147">**[Skapa en testanvändare SciQuest tillbringar Director](#creating-a-halogen-software-test-user)**  -toohave en motsvarighet för Britta Simon i SciQuest tillbringar Director som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="2d495-147">**[Creating a SciQuest Spend Director test user](#creating-a-halogen-software-test-user)** - toohave a counterpart of Britta Simon in SciQuest Spend Director that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="2d495-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="2d495-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2d495-149">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="2d495-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-single-sign-on"></a><span data-ttu-id="2d495-150">Konfigurera Azure AD-en enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d495-150">Configuring Azure AD Single Single Sign-On</span></span>
<span data-ttu-id="2d495-151">hello syftet med det här avsnittet är tooenable Azure AD enkel inloggning i hello klassiska Azure-portalen och tooconfigure enkel inloggning i ditt SciQuest tillbringar Director-program.</span><span class="sxs-lookup"><span data-stu-id="2d495-151">hello objective of this section is tooenable Azure AD single sign-on in hello Azure classic portal and tooconfigure single sign-on in your SciQuest Spend Director application.</span></span>

<span data-ttu-id="2d495-152">**tooconfigure Azure AD enkel inloggning med SciQuest tillbringar Director utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2d495-152">**tooconfigure Azure AD single sign-on with SciQuest Spend Director, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d495-153">I hello klassiska Azure-portalen på hello **SciQuest tillbringar Director** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d495-153">In hello Azure classic portal, on hello **SciQuest Spend Director** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][8]

2. <span data-ttu-id="2d495-155">På hello **hur skulle du som användare toosign på tooSciQuest utgifter Director** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="2d495-155">On hello **How would you like users toosign on tooSciQuest Spend Director** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][9]

3. <span data-ttu-id="2d495-157">På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d495-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span> 
   
    ![Konfigurera Appinställningar för][10]
   
     <span data-ttu-id="2d495-159">a.</span><span class="sxs-lookup"><span data-stu-id="2d495-159">a.</span></span> <span data-ttu-id="2d495-160">I hello **logga URL** textruta, Skriv URL: en som används av dina användare toosign på tooyour SciQuest tillbringar Director program med hjälp av hello följer mönstret: *https://.* SciQuest.com/.**</span><span class="sxs-lookup"><span data-stu-id="2d495-160">In hello **Sign On URL** textbox, type your URL used by your users toosign on tooyour SciQuest Spend Director application using hello following pattern: *https://.*sciquest.com/.**</span></span>
   
     <span data-ttu-id="2d495-161">b.</span><span class="sxs-lookup"><span data-stu-id="2d495-161">b.</span></span> <span data-ttu-id="2d495-162">I hello **Reply URL** textruta, typen hello samma värde som du har skrivit in i hello **logga URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="2d495-162">In hello **Reply URL** textbox, type hello same value you have typed into hello **Sign On URL** textbox.</span></span> 
   
     <span data-ttu-id="2d495-163">c.</span><span class="sxs-lookup"><span data-stu-id="2d495-163">c.</span></span> <span data-ttu-id="2d495-164">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2d495-164">Click **Next**.</span></span>

4. <span data-ttu-id="2d495-165">På hello **Konfigurera enkel inloggning på SciQuest tillbringar Director** klickar du på **hämtar metadata**, och sedan spara hello metadatafil lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="2d495-165">On hello **Configure single sign-on at SciQuest Spend Director** page, click **Download metadata**, and then save hello metadata file locally on your computer.</span></span>
   
    ![Vad är Azure AD Connect?][11]

5. <span data-ttu-id="2d495-167">Kontakta SciQuest support tooenable den här autentiseringsmetoden använder hello ovan hämtas metadata.</span><span class="sxs-lookup"><span data-stu-id="2d495-167">Contact SciQuest support tooenable this authentication method using hello above downloaded metadata.</span></span>

6. <span data-ttu-id="2d495-168">Välj hello konfiguration för enkel inloggning bekräftelse på hello klassiska Azure-portalen, och klicka sedan på **Slutför** tooclose hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2d495-168">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span> 
   
    ![Vad är Azure AD Connect?][15]

7. <span data-ttu-id="2d495-170">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="2d495-170">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2d495-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d495-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="2d495-172">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2d495-172">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="2d495-173">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2d495-173">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d495-174">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2d495-174">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Vad är Azure AD Connect?][100] 

2. <span data-ttu-id="2d495-176">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="2d495-176">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="2d495-177">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="2d495-177">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Vad är Azure AD Connect?][101] 

4. <span data-ttu-id="2d495-179">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="2d495-179">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Vad är Azure AD Connect?][102] 

5. <span data-ttu-id="2d495-181">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d495-181">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Vad är Azure AD Connect?][103] 
   
    <span data-ttu-id="2d495-183">a.</span><span class="sxs-lookup"><span data-stu-id="2d495-183">a.</span></span> <span data-ttu-id="2d495-184">Som **typ av användare**väljer **ny användare i din organisation**.</span><span class="sxs-lookup"><span data-stu-id="2d495-184">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="2d495-185">b.</span><span class="sxs-lookup"><span data-stu-id="2d495-185">b.</span></span> <span data-ttu-id="2d495-186">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2d495-186">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="2d495-187">c.</span><span class="sxs-lookup"><span data-stu-id="2d495-187">c.</span></span> <span data-ttu-id="2d495-188">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2d495-188">Click **Next**.</span></span>

6. <span data-ttu-id="2d495-189">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d495-189">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Vad är Azure AD Connect?][104] 
   
    <span data-ttu-id="2d495-191">a.</span><span class="sxs-lookup"><span data-stu-id="2d495-191">a.</span></span> <span data-ttu-id="2d495-192">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="2d495-192">In hello **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="2d495-193">b.</span><span class="sxs-lookup"><span data-stu-id="2d495-193">b.</span></span> <span data-ttu-id="2d495-194">I hello **efternamn** txtbox typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d495-194">In hello **Last Name** txtbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="2d495-195">c.</span><span class="sxs-lookup"><span data-stu-id="2d495-195">c.</span></span> <span data-ttu-id="2d495-196">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d495-196">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="2d495-197">d.</span><span class="sxs-lookup"><span data-stu-id="2d495-197">d.</span></span> <span data-ttu-id="2d495-198">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="2d495-198">In hello **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="2d495-199">e.</span><span class="sxs-lookup"><span data-stu-id="2d495-199">e.</span></span> <span data-ttu-id="2d495-200">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="2d495-200">Click **Next**.</span></span>

7. <span data-ttu-id="2d495-201">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="2d495-201">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Vad är Azure AD Connect?][105]  

8. <span data-ttu-id="2d495-203">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2d495-203">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Vad är Azure AD Connect?][106]   
   
    <span data-ttu-id="2d495-205">a.</span><span class="sxs-lookup"><span data-stu-id="2d495-205">a.</span></span> <span data-ttu-id="2d495-206">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="2d495-206">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="2d495-207">b.</span><span class="sxs-lookup"><span data-stu-id="2d495-207">b.</span></span> <span data-ttu-id="2d495-208">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="2d495-208">Click **Complete**.</span></span>   

### <a name="creating-a-sciquest-spend-director-test-user"></a><span data-ttu-id="2d495-209">Skapa en testanvändare SciQuest tillbringar Director</span><span class="sxs-lookup"><span data-stu-id="2d495-209">Creating a SciQuest Spend Director test user</span></span>
<span data-ttu-id="2d495-210">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="2d495-210">hello objective of this section is toocreate a user called Britta Simon in SciQuest Spend Director.</span></span>

<span data-ttu-id="2d495-211">Du behöver toocontact supportteamet SciQuest tillbringar Director och ge dem hello information om din test konto tooget som den har skapats.</span><span class="sxs-lookup"><span data-stu-id="2d495-211">You need toocontact your SciQuest Spend Director support team and provide them with hello details about your test account tooget it created.</span></span>

<span data-ttu-id="2d495-212">Du kan också användas just-in-time etablering, en enkel inloggning funktion som stöds av SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="2d495-212">Alternatively, you can also leverage just-in-time provisioning, a single sign-on feature that is supported by SciQuest Spend Director.</span></span>  
<span data-ttu-id="2d495-213">Om just-in-time-etablering aktiveras kan skapas användare automatiskt av SciQuest tillbringar Director under ett försök att enkel inloggning om de inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="2d495-213">If just-in-time provisioning is enabled, users are automatically created by SciQuest Spend Director during a single sign-on attempt if they don't exist.</span></span> <span data-ttu-id="2d495-214">Den här funktionen eliminerar behovet av hello toomanually skapa motsvarighet för enkel inloggning användare.</span><span class="sxs-lookup"><span data-stu-id="2d495-214">This feature eliminates hello need toomanually create single sign-on counterpart users.</span></span>

<span data-ttu-id="2d495-215">tooget just-in-time-etablering aktiverad måste du toocontact du är supportteamet SciQuest tillbringar Director.</span><span class="sxs-lookup"><span data-stu-id="2d495-215">tooget just-in-time provisioning enabled, you need toocontact your your SciQuest Spend Director support team.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2d495-216">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="2d495-216">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="2d495-217">hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access tooSciQuest utgifter Director.</span><span class="sxs-lookup"><span data-stu-id="2d495-217">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSciQuest Spend Director.</span></span>

![Vad är Azure AD Connect?][200]

<span data-ttu-id="2d495-219">**tooassign Britta Simon tooSciQuest utgifter Director utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="2d495-219">**tooassign Britta Simon tooSciQuest Spend Director, perform hello following steps:**</span></span>

1. <span data-ttu-id="2d495-220">På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="2d495-220">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Vad är Azure AD Connect?][201]

2. <span data-ttu-id="2d495-222">Välj i listan med program hello **SciQuest tillbringar Director**.</span><span class="sxs-lookup"><span data-stu-id="2d495-222">In hello applications list, select **SciQuest Spend Director**.</span></span>
   
    ![Vad är Azure AD Connect?][202]

3. <span data-ttu-id="2d495-224">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="2d495-224">In hello menu on hello top, click **Users**.</span></span>
   
    ![Vad är Azure AD Connect?][203]

4. <span data-ttu-id="2d495-226">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="2d495-226">In hello Users list, select **Britta Simon**.</span></span>
   
    ![Vad är Azure AD Connect?][204]

5. <span data-ttu-id="2d495-228">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="2d495-228">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Vad är Azure AD Connect?][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="2d495-230">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="2d495-230">Testing Single Sign-On</span></span>
<span data-ttu-id="2d495-231">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2d495-231">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="2d495-232">Du bör få automatiskt inloggade tooyour SciQuest tillbringar Director programmet när du klickar på hello SciQuest tillbringar Director-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="2d495-232">When you click hello SciQuest Spend Director tile in hello Access Panel, you should get automatically signed-on tooyour SciQuest Spend Director application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2d495-233">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2d495-233">Additional Resources</span></span>
* [<span data-ttu-id="2d495-234">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="2d495-234">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2d495-235">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="2d495-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

