---
title: "Självstudier: Azure Active Directory-integrering med Bynder | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Bynder."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: a2a8477580d28fe422f2836f483dff286bc71c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a><span data-ttu-id="377d5-103">Självstudier: Azure Active Directory-integrering med Bynder</span><span class="sxs-lookup"><span data-stu-id="377d5-103">Tutorial: Azure Active Directory integration with Bynder</span></span>
<span data-ttu-id="377d5-104">hello syftet med den här kursen är tooshow du hur toointegrate Bynder med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="377d5-104">hello objective of this tutorial is tooshow you how toointegrate Bynder with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="377d5-105">Integrera Bynder med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="377d5-105">Integrating Bynder with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="377d5-106">Du kan styra i Azure AD som har åtkomst till tooBynder</span><span class="sxs-lookup"><span data-stu-id="377d5-106">You can control in Azure AD who has access tooBynder</span></span>
* <span data-ttu-id="377d5-107">Du kan aktivera för dina användare tooautomatically get inloggade tooBynder enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="377d5-107">You can enable your users tooautomatically get signed-on tooBynder single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="377d5-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="377d5-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="377d5-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="377d5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="377d5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="377d5-110">Prerequisites</span></span>
<span data-ttu-id="377d5-111">tooconfigure Azure AD-integrering med Bynder, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="377d5-111">tooconfigure Azure AD integration with Bynder, you need hello following items:</span></span>

* <span data-ttu-id="377d5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="377d5-112">An Azure AD subscription</span></span>
* <span data-ttu-id="377d5-113">En Bynder enkel inloggning (SSO) aktiverat prenumeration</span><span class="sxs-lookup"><span data-stu-id="377d5-113">A Bynder single-sign on (SSO) enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="377d5-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="377d5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="377d5-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="377d5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="377d5-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="377d5-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="377d5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="377d5-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="377d5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="377d5-118">Scenario description</span></span>
<span data-ttu-id="377d5-119">hello syftet med den här kursen är tooenable du tootest Microsoft Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="377d5-119">hello objective of this tutorial is tooenable you tootest Microsoft Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="377d5-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="377d5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="377d5-121">Att lägga till Bynder från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="377d5-121">Adding Bynder from hello gallery</span></span>
2. <span data-ttu-id="377d5-122">Konfigurera och testa Microsoft Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="377d5-122">Configuring and testing Microsoft Azure AD SSO</span></span>

## <a name="add-bynder-from-hello-gallery"></a><span data-ttu-id="377d5-123">Lägg till Bynder från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="377d5-123">Add Bynder from hello gallery</span></span>
<span data-ttu-id="377d5-124">tooconfigure hello integrering av Bynder i Azure AD, behöver du tooadd Bynder hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="377d5-124">tooconfigure hello integration of Bynder into Azure AD, you need tooadd Bynder from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="377d5-125">**tooadd Bynder från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="377d5-125">**tooadd Bynder from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="377d5-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="377d5-126">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="377d5-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="377d5-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="377d5-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="377d5-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Program][2]
4. <span data-ttu-id="377d5-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="377d5-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3]
5. <span data-ttu-id="377d5-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="377d5-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Program][4]
6. <span data-ttu-id="377d5-135">Skriv i sökrutan hello **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="377d5-135">In hello search box, type **Bynder**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. <span data-ttu-id="377d5-137">Markera hello resultat på panelen **Bynder**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="377d5-137">In hello results panel, select **Bynder**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Att välja hello app i hello-galleriet](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a><span data-ttu-id="377d5-139">Konfigurera och testa Microsoft Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="377d5-139">Configure and test Microsoft Azure AD SSO</span></span>
<span data-ttu-id="377d5-140">hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Microsoft Azure AD SSO med Bynder baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="377d5-140">hello objective of this section is tooshow you how tooconfigure and test Microsoft Azure AD SSO with Bynder based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="377d5-141">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Bynder tooan användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="377d5-141">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Bynder tooan user in Azure AD is.</span></span> <span data-ttu-id="377d5-142">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Bynder toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="377d5-142">In other words, a link relationship between an Azure AD user and hello related user in Bynder needs toobe established.</span></span>

<span data-ttu-id="377d5-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Bynder.</span><span class="sxs-lookup"><span data-stu-id="377d5-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Bynder.</span></span>

<span data-ttu-id="377d5-144">tooconfigure och testa Microsoft Azure AD SSO med Bynder, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="377d5-144">tooconfigure and test Microsoft Azure AD SSO with Bynder, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="377d5-145">**[Konfigurera Microsoft Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="377d5-145">**[Configuring Microsoft Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="377d5-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Microsoft Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="377d5-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Microsoft Azure AD Single Sign-On with Britta Simon.</span></span>
3. <span data-ttu-id="377d5-147">**[Skapa en testanvändare Bynder](#creating-a-bynder-test-user)**  -toohave en motsvarighet för Britta Simon i Bynder som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="377d5-147">**[Creating a Bynder test user](#creating-a-bynder-test-user)** - toohave a counterpart of Britta Simon in Bynder that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="377d5-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Microsoft Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="377d5-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Microsoft Azure AD Single Sign-On.</span></span>
5. <span data-ttu-id="377d5-149">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="377d5-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-microsoft-azure-ad-sso"></a><span data-ttu-id="377d5-150">Konfigurera Microsoft Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="377d5-150">Configuring Microsoft Azure AD SSO</span></span>
<span data-ttu-id="377d5-151">I det här avsnittet att aktivera Microsoft Azure AD SSO i hello klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Bynder.</span><span class="sxs-lookup"><span data-stu-id="377d5-151">In this section, you enable Microsoft Azure AD SSO in hello classic portal and configure SSO in your Bynder application.</span></span>

<span data-ttu-id="377d5-152">**tooconfigure Microsoft Azure AD SSO med Bynder, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="377d5-152">**tooconfigure Microsoft Azure AD SSO with Bynder, perform hello following steps:**</span></span>

1. <span data-ttu-id="377d5-153">I hello klassiska portalen på hello **Bynder** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="377d5-153">In hello classic portal, on hello **Bynder** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 
2. <span data-ttu-id="377d5-155">På hello **hur skulle du som användare toosign på tooBynder** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-155">On hello **How would you like users toosign on tooBynder** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. <span data-ttu-id="377d5-157">På hello **konfigurera Appinställningar** dialogrutan sidan om du vill tooconfigure hello programmet i **IDP initierade läge**, utföra hello följande och klickar på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="377d5-157">On hello **Configure App Settings** dialog page, If you wish tooconfigure hello application in **IDP initiated mode**, perform hello following steps and click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. <span data-ttu-id="377d5-159">I hello **Reply URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.getbynder.com/sso/SAML/authenticate/`</span><span class="sxs-lookup"><span data-stu-id="377d5-159">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.getbynder.com/sso/SAML/authenticate/`</span></span>
  2. <span data-ttu-id="377d5-160">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-160">Click **Next**.</span></span>
4. <span data-ttu-id="377d5-161">Om du inte vill tooconfigure hello programmet i **SP initierade läge** på hello **konfigurera Appinställningar** dialogrutan sidan, klickar på hello **”visa avancerade inställningar (valfritt)”**och ange sedan hello **logga URL** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-161">If you wish tooconfigure hello application in **SP initiated mode** on hello **Configure App Settings** dialog page, then click on hello **“Show advanced settings (optional)”** and then enter hello **Sign On URL** and click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. <span data-ttu-id="377d5-163">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret:`https://<company name>.getbynder.com/login/`</span><span class="sxs-lookup"><span data-stu-id="377d5-163">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company name>.getbynder.com/login/`</span></span>
  2. <span data-ttu-id="377d5-164">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-164">Click **Next**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="377d5-165">hello-värdet för hello logga URL i den här kursen är bara en placeholfer.</span><span class="sxs-lookup"><span data-stu-id="377d5-165">hello value for hello Sign On URL in this tutorial is just a placeholfer.</span></span> <span data-ttu-id="377d5-166">tooget hello faktiska värdet för din miljö, kontakta Bynder.</span><span class="sxs-lookup"><span data-stu-id="377d5-166">tooget hello actual vlaue for your environment, contact Bynder.</span></span>
   >

5. <span data-ttu-id="377d5-167">På hello **Konfigurera enkel inloggning på Bynder** , utföra hello följande och klickar på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="377d5-167">On hello **Configure single sign-on at Bynder** page, perform hello following steps and click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. <span data-ttu-id="377d5-169">Klicka på **hämtar metadata**, och spara sedan hello på datorn.</span><span class="sxs-lookup"><span data-stu-id="377d5-169">Click **Download metadata**, and then save hello file on your computer.</span></span>
  2. <span data-ttu-id="377d5-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-170">Click **Next**.</span></span>
6. <span data-ttu-id="377d5-171">tooget SSO konfigurerats för programmet, kontaktar du supportteamet Bynder.</span><span class="sxs-lookup"><span data-stu-id="377d5-171">tooget SSO configured for your application, contact your Bynder support team.</span></span> <span data-ttu-id="377d5-172">Koppla hello hämtade metadatafil och dela den med Bynder team tooset in enkel inloggning på sidan.</span><span class="sxs-lookup"><span data-stu-id="377d5-172">Attach hello downloaded metadata file and share it with Bynder team tooset up SSO on their side.</span></span>
7. <span data-ttu-id="377d5-173">Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-173">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][10]
8. <span data-ttu-id="377d5-175">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="377d5-175">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="377d5-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="377d5-177">Create an Azure AD test user</span></span>
<span data-ttu-id="377d5-178">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="377d5-178">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="377d5-180">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="377d5-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="377d5-181">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="377d5-181">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="377d5-183">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="377d5-183">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="377d5-184">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="377d5-184">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="377d5-186">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="377d5-186">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="377d5-188">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="377d5-188">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. <span data-ttu-id="377d5-190">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="377d5-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="377d5-191">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="377d5-191">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="377d5-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-192">Click **Next**.</span></span>
6. <span data-ttu-id="377d5-193">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="377d5-193">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="377d5-195">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="377d5-195">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="377d5-196">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="377d5-196">In hello **Last Name** textbox, type, **Simon**.</span></span> 
  3. <span data-ttu-id="377d5-197">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="377d5-197">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="377d5-198">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="377d5-198">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="377d5-199">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-199">Click **Next**.</span></span>
7. <span data-ttu-id="377d5-200">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="377d5-200">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="377d5-202">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="377d5-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. <span data-ttu-id="377d5-204">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="377d5-204">Write down hello value of hello **New Password**.</span></span>
   2. <span data-ttu-id="377d5-205">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="377d5-205">Click **Complete**.</span></span>   

### <a name="create-a-bynder-test-user"></a><span data-ttu-id="377d5-206">Skapa en testanvändare Bynder</span><span class="sxs-lookup"><span data-stu-id="377d5-206">Create a Bynder test user</span></span>
<span data-ttu-id="377d5-207">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i Bynder.</span><span class="sxs-lookup"><span data-stu-id="377d5-207">hello objective of this section is toocreate a user called Britta Simon in Bynder.</span></span> <span data-ttu-id="377d5-208">Bynder stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="377d5-208">Bynder supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="377d5-209">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="377d5-209">There is no action item for you in this section.</span></span> <span data-ttu-id="377d5-210">En ny användare skapas under ett försök tooaccess Bynder om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="377d5-210">A new user will be created during an attempt tooaccess Bynder if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="377d5-211">Om du behöver toocreate en användare manuellt, måste toocontact hello Bynder supportteamet.</span><span class="sxs-lookup"><span data-stu-id="377d5-211">If you need toocreate an user manually, you need toocontact hello Bynder support team.</span></span> 
> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="377d5-212">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="377d5-212">Assign hello Azure AD test user</span></span>
<span data-ttu-id="377d5-213">hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure SSO genom att tilldela tooBynder sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="377d5-213">hello objective of this section is tooenabling Britta Simon toouse Azure SSO by granting her access tooBynder.</span></span>

   ![Tilldela användare][200]

<span data-ttu-id="377d5-215">**tooassign Britta Simon tooBynder utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="377d5-215">**tooassign Britta Simon tooBynder, perform hello following steps:**</span></span>

1. <span data-ttu-id="377d5-216">På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="377d5-216">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Tilldela användare][201]
2. <span data-ttu-id="377d5-218">Välj i listan med program hello **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="377d5-218">In hello applications list, select **Bynder**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. <span data-ttu-id="377d5-220">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="377d5-220">In hello menu on hello top, click **Users**.</span></span>
   
    ![Tilldela användare][203]
4. <span data-ttu-id="377d5-222">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="377d5-222">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="377d5-223">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="377d5-223">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="377d5-225">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="377d5-225">Test single sign-on</span></span>
<span data-ttu-id="377d5-226">hello syftet med det här avsnittet är tootest din Microsoft Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="377d5-226">hello objective of this section is tootest your Microsoft Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="377d5-227">Du bör få automatiskt inloggade tooyour Bynder programmet när du klickar på hello Bynder panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="377d5-227">When you click hello Bynder tile in hello Access Panel, you should get automatically signed-on tooyour Bynder application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="377d5-228">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="377d5-228">Additional resources</span></span>
* [<span data-ttu-id="377d5-229">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="377d5-229">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="377d5-230">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="377d5-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
