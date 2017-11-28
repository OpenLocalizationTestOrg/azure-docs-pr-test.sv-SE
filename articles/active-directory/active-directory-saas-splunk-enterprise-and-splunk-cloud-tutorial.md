---
title: "Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk molntjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Splunk Enterprise och Splunk moln."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 9bb6817cb31dce684cd9cc1c567fa3efc8906ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a><span data-ttu-id="8287b-103">Självstudier: Azure Active Directory-integrering med Splunk Enterprise och Splunk moln</span><span class="sxs-lookup"><span data-stu-id="8287b-103">Tutorial: Azure Active Directory integration with Splunk Enterprise and Splunk Cloud</span></span>

<span data-ttu-id="8287b-104">I kursen får du lära dig hur toointegrate Splunk Enterprise och Splunk moln med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="8287b-104">In this tutorial, you learn how toointegrate Splunk Enterprise and Splunk Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8287b-105">Integrera Splunk Enterprise och Splunk moln med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="8287b-105">Integrating Splunk Enterprise and Splunk Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8287b-106">Du kan styra i Azure AD som har åtkomst tooSplunk Enterprise och Splunk moln</span><span class="sxs-lookup"><span data-stu-id="8287b-106">You can control in Azure AD who has access tooSplunk Enterprise and Splunk Cloud</span></span>
- <span data-ttu-id="8287b-107">Du kan aktivera din användare tooautomatically get inloggade tooSplunk Enterprise och Splunk moln enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="8287b-107">You can enable your users tooautomatically get signed-on tooSplunk Enterprise and Splunk Cloud single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="8287b-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="8287b-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="8287b-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8287b-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8287b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8287b-110">Prerequisites</span></span>

<span data-ttu-id="8287b-111">tooconfigure Azure AD-integrering med Splunk Enterprise och Splunk molnet, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="8287b-111">tooconfigure Azure AD integration with Splunk Enterprise and Splunk Cloud, you need hello following items:</span></span>

- <span data-ttu-id="8287b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="8287b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8287b-113">En Splunk Enterprise eller Splunk moln SSO aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="8287b-113">A Splunk Enterprise or Splunk Cloud SSO enabled subscription</span></span>


>[!NOTE]
><span data-ttu-id="8287b-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="8287b-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="8287b-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="8287b-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8287b-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="8287b-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="8287b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8287b-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="8287b-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="8287b-118">Scenario description</span></span>
<span data-ttu-id="8287b-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="8287b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="8287b-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="8287b-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8287b-121">Lägga till Splunk Enterprise och Splunk moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8287b-121">Adding Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
2. <span data-ttu-id="8287b-122">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="8287b-122">Configuring and testing Azure AD SSO</span></span>


## <a name="add-splunk-enterprise-and-splunk-cloud-from-hello-gallery"></a><span data-ttu-id="8287b-123">Lägg till Splunk Enterprise och Splunk moln från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="8287b-123">Add Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
<span data-ttu-id="8287b-124">tooconfigure hello integrering av Splunk Enterprise och Splunk moln i Azure AD, behöver du tooadd Splunk Enterprise och Splunk moln hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="8287b-124">tooconfigure hello integration of Splunk Enterprise and Splunk Cloud into Azure AD, you need tooadd Splunk Enterprise and Splunk Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8287b-125">**tooadd Splunk Enterprise och Splunk moln från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8287b-125">**tooadd Splunk Enterprise and Splunk Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8287b-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8287b-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]

2. <span data-ttu-id="8287b-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="8287b-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="8287b-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="8287b-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Program][2]

4. <span data-ttu-id="8287b-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="8287b-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Program][3]

5. <span data-ttu-id="8287b-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="8287b-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>

    ![Program][4]

6. <span data-ttu-id="8287b-135">Skriv i sökrutan hello **Splunk Enterprise eller Splunk moln**.</span><span class="sxs-lookup"><span data-stu-id="8287b-135">In hello search box, type **Splunk Enterprise or Splunk Cloud**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. <span data-ttu-id="8287b-137">I resultatfönstret hello väljer **Splunk Enterprise och Splunk moln**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="8287b-137">In hello results pane, select **Splunk Enterprise and Splunk Cloud**, and then click **Complete** tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="8287b-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8287b-139">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="8287b-140">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="8287b-140">In this section, you configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8287b-141">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Splunk Enterprise och Splunk molnet är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="8287b-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Splunk Enterprise and Splunk Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="8287b-142">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användaren i Splunk Enterprise och Splunk molnet toobe upprätta.</span><span class="sxs-lookup"><span data-stu-id="8287b-142">In other words, a link relationship between an Azure AD user and hello related user in Splunk Enterprise and Splunk Cloud needs toobe established.</span></span>

<span data-ttu-id="8287b-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Splunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="8287b-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Splunk Enterprise and Splunk Cloud.</span></span>

<span data-ttu-id="8287b-144">tooconfigure och testa Azure AD enkel inloggning med Splunk Enterprise och Splunk molnet, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="8287b-144">tooconfigure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8287b-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="8287b-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8287b-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8287b-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8287b-147">**[Skapa en testanvändare Splunk Enterprise och Splunk moln](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  -toohave en motsvarighet för Britta Simon i Splunk företag och Splunk moln som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="8287b-147">**[Creating a Splunk Enterprise and Splunk Cloud test user](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** - toohave a counterpart of Britta Simon in Splunk Enterprise and Splunk Cloud that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="8287b-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="8287b-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8287b-149">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="8287b-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="8287b-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8287b-150">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="8287b-151">I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Splunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="8287b-151">In this section, you enable Azure AD SSO in hello classic portal and configure SSO in your Splunk Enterprise and Splunk Cloud application.</span></span>


<span data-ttu-id="8287b-152">**tooconfigure Azure AD enkel inloggning med Splunk Enterprise och Splunk moln, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="8287b-152">**tooconfigure Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="8287b-153">I hello klassiska portalen på hello **Splunk Enterprise och Splunk moln** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="8287b-153">In hello classic portal, on hello **Splunk Enterprise and Splunk Cloud** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
     
    ![Konfigurera enkel inloggning][6] 

2. <span data-ttu-id="8287b-155">På hello **hur vill du användare toosign på tooSplunk Enterprise och Splunk moln** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8287b-155">On hello **How would you like users toosign on tooSplunk Enterprise and Splunk Cloud** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. <span data-ttu-id="8287b-157">På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8287b-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. <span data-ttu-id="8287b-159">I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour Splunk Enterprise och Splunk moln program med hjälp av hello följande mönster:`https://<splunkserverUrl>/en-US/app/launcher/home`</span><span class="sxs-lookup"><span data-stu-id="8287b-159">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Splunk Enterprise and Splunk Cloud application using hello following pattern: `https://<splunkserverUrl>/en-US/app/launcher/home`</span></span>
  2. <span data-ttu-id="8287b-160">I hello **identifierare** textruta typen hello URL-Adressen till din Splunk.</span><span class="sxs-lookup"><span data-stu-id="8287b-160">In hello **Identifier** textbox, type hello URL of your Splunk Server.</span></span>
  3. <span data-ttu-id="8287b-161">I hello **Reply URL** textruta typen hello URL med hello följande mönster:`https://<splunkserver>/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="8287b-161">In hello **Reply URL** textbox, type hello URL with hello following pattern: `https://<splunkserver>/saml/acs`</span></span>
  4. <span data-ttu-id="8287b-162">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8287b-162">Click **Next**.</span></span>
 
4. <span data-ttu-id="8287b-163">På hello **Konfigurera enkel inloggning på Splunk Enterprise och Splunk moln** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8287b-163">On hello **Configure single sign-on at Splunk Enterprise and Splunk Cloud** page, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. <span data-ttu-id="8287b-165">Klicka på **hämtar metadata**, och spara sedan hello på datorn.</span><span class="sxs-lookup"><span data-stu-id="8287b-165">Click **Download metadata**, and then save hello file on your computer.</span></span>
  2. <span data-ttu-id="8287b-166">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8287b-166">Click **Next**.</span></span>

5. <span data-ttu-id="8287b-167">tooget SSO konfigurerats för ditt program, kontakta Splunk Enterprise och Splunk moln supportteamet och ge dem hello följande:</span><span class="sxs-lookup"><span data-stu-id="8287b-167">tooget SSO configured for your application, contact Splunk Enterprise and Splunk Cloud support team and provide them with hello following:</span></span>

    * <span data-ttu-id="8287b-168">hello hämtas **federaton metadata**</span><span class="sxs-lookup"><span data-stu-id="8287b-168">hello downloaded **federaton metadata**</span></span>
6. <span data-ttu-id="8287b-169">Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="8287b-169">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Azure AD-Single Sign-On][10]

7. <span data-ttu-id="8287b-171">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="8287b-171">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="8287b-173">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="8287b-173">Create an Azure AD test user</span></span>
<span data-ttu-id="8287b-174">I det här avsnittet skapar du en testanvändare i hello klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8287b-174">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="8287b-176">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8287b-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8287b-177">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8287b-177">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="8287b-179">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="8287b-179">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="8287b-180">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="8287b-180">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8287b-182">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="8287b-182">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="8287b-184">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8287b-184">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="8287b-186">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="8287b-186">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="8287b-187">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8287b-187">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="8287b-188">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8287b-188">Click **Next**.</span></span>

6.  <span data-ttu-id="8287b-189">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8287b-189">On hello **User Profile** dialog page, perform hello following steps:</span></span>
  
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. <span data-ttu-id="8287b-191">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="8287b-191">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="8287b-192">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="8287b-192">In hello **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="8287b-193">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="8287b-193">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="8287b-194">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="8287b-194">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="8287b-195">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="8287b-195">Click **Next**.</span></span>

7. <span data-ttu-id="8287b-196">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="8287b-196">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="8287b-198">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8287b-198">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. <span data-ttu-id="8287b-200">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="8287b-200">Write down hello value of hello **New Password**.</span></span>
  2. <span data-ttu-id="8287b-201">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="8287b-201">Click **Complete**.</span></span>   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a><span data-ttu-id="8287b-202">Skapa en Splunk Enterprise och Splunk moln testanvändare</span><span class="sxs-lookup"><span data-stu-id="8287b-202">Create a Splunk Enterprise and Splunk Cloud test user</span></span>

<span data-ttu-id="8287b-203">I det här avsnittet kan du skapa en användare som kallas Britta Simon i Splunk företag och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="8287b-203">In this section, you create a user called Britta Simon in Splunk Enterprise and Splunk Cloud.</span></span> <span data-ttu-id="8287b-204">Se tillsammans med Splunk Enterprise och Splunk Cloud support-teamet tooadd hello användare i hello Splunk Enterprise och Splunk molnplattform.</span><span class="sxs-lookup"><span data-stu-id="8287b-204">Please work with Splunk Enterprise and Splunk Cloud support team tooadd hello users in hello Splunk Enterprise and Splunk Cloud platform.</span></span>


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="8287b-205">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="8287b-205">Assign hello Azure AD test user</span></span>

<span data-ttu-id="8287b-206">I det här avsnittet kan du aktivera Britta Simon toouse Azure SSOy bevilja sin åtkomst tooSplunk Enterprise och Splunk moln.</span><span class="sxs-lookup"><span data-stu-id="8287b-206">In this section, you enable Britta Simon toouse Azure SSOy granting her access tooSplunk Enterprise and Splunk Cloud.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="8287b-208">**tooassign Britta Simon tooSplunk Enterprise och Splunk moln, utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="8287b-208">**tooassign Britta Simon tooSplunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="8287b-209">På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="8287b-209">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="8287b-211">Välj i listan med program hello **Splunk Enterprise och Splunk moln**.</span><span class="sxs-lookup"><span data-stu-id="8287b-211">In hello applications list, select **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. <span data-ttu-id="8287b-213">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="8287b-213">In hello menu on hello top, click **Users**.</span></span>

    ![Tilldela användare][203]

4. <span data-ttu-id="8287b-215">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="8287b-215">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="8287b-216">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="8287b-216">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="8287b-218">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="8287b-218">Test single sign-on</span></span>

<span data-ttu-id="8287b-219">I det här avsnittet kan du testa din Azure AD-SSOonfiguration med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8287b-219">In this section, you test your Azure AD SSOonfiguration using hello Access Panel.</span></span>

<span data-ttu-id="8287b-220">Du bör få automatiskt inloggade tooyour Splunk Enterprise och Splunk moln när du klickar på hello Splunk Enterprise och Splunk moln-panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="8287b-220">When you click hello Splunk Enterprise and Splunk Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Splunk Enterprise and Splunk Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="8287b-221">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="8287b-221">Additional resources</span></span>

* [<span data-ttu-id="8287b-222">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="8287b-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8287b-223">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="8287b-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
