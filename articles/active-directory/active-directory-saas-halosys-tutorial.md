---
title: "Självstudier: Azure Active Directory-integrering med Halosys | Microsoft Docs"
description: "Lär dig hur toouse Halosys med Azure Active Directory tooenable enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 42a0eb7c-5cb7-44a9-b00b-b0e7df4b63e8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 18043ed1b6f7ab45c59cfd36252bef1621618e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-halosys"></a><span data-ttu-id="0e1e7-103">Självstudier: Azure Active Directory-integrering med Halosys</span><span class="sxs-lookup"><span data-stu-id="0e1e7-103">Tutorial: Azure Active Directory integration with Halosys</span></span>

<span data-ttu-id="0e1e7-104">I kursen får du lära dig hur toointegrate Halosys med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="0e1e7-104">In this tutorial, you learn how toointegrate Halosys with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0e1e7-105">Integrera Halosys med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-105">Integrating Halosys with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0e1e7-106">Du kan styra i Azure AD som har åtkomst till tooHalosys</span><span class="sxs-lookup"><span data-stu-id="0e1e7-106">You can control in Azure AD who has access tooHalosys</span></span>
- <span data-ttu-id="0e1e7-107">Du kan aktivera din användare tooautomatically get inloggade tooHalosys (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="0e1e7-107">You can enable your users tooautomatically get signed-on tooHalosys (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0e1e7-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0e1e7-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="0e1e7-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0e1e7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0e1e7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="0e1e7-110">Prerequisites</span></span>

<span data-ttu-id="0e1e7-111">tooconfigure Azure AD-integrering med Halosys, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-111">tooconfigure Azure AD integration with Halosys, you need hello following items:</span></span>

- <span data-ttu-id="0e1e7-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0e1e7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0e1e7-113">En Halosys enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="0e1e7-113">A Halosys single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="0e1e7-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="0e1e7-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0e1e7-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="0e1e7-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0e1e7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="0e1e7-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="0e1e7-118">Scenario description</span></span>
<span data-ttu-id="0e1e7-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="0e1e7-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0e1e7-121">Att lägga till Halosys från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0e1e7-121">Adding Halosys from hello gallery</span></span>
2. <span data-ttu-id="0e1e7-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0e1e7-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-halosys-from-hello-gallery"></a><span data-ttu-id="0e1e7-123">Att lägga till Halosys från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="0e1e7-123">Adding Halosys from hello gallery</span></span>
<span data-ttu-id="0e1e7-124">tooconfigure hello integrering av Halosys i Azure AD, behöver du tooadd Halosys hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-124">tooconfigure hello integration of Halosys into Azure AD, you need tooadd Halosys from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0e1e7-125">**tooadd Halosys från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0e1e7-125">**tooadd Halosys from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e1e7-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]
2. <span data-ttu-id="0e1e7-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="0e1e7-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Program][2]

4. <span data-ttu-id="0e1e7-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Program][3]

5. <span data-ttu-id="0e1e7-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>

    ![Program][4]

6. <span data-ttu-id="0e1e7-135">Skriv i sökrutan hello **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-135">In hello search box, type **Halosys**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_01.png)
    
7. <span data-ttu-id="0e1e7-137">I resultatfönstret hello väljer **Halosys**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-137">In hello results pane, select **Halosys**, and then click **Complete** tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_011.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0e1e7-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0e1e7-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0e1e7-140">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Halosys baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-140">In this section, you configure and test Azure AD single sign-on with Halosys based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0e1e7-141">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Halosys är tooa i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Halosys is tooa user in Azure AD.</span></span> <span data-ttu-id="0e1e7-142">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Halosys toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-142">In other words, a link relationship between an Azure AD user and hello related user in Halosys needs toobe established.</span></span>

<span data-ttu-id="0e1e7-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Halosys.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Halosys.</span></span>

<span data-ttu-id="0e1e7-144">tooconfigure och testa Azure AD enkel inloggning med Halosys, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-144">tooconfigure and test Azure AD single sign-on with Halosys, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0e1e7-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0e1e7-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0e1e7-147">**[Skapa en testanvändare Halosys](#creating-a-halosys-test-user)**  -toohave en motsvarighet för Britta Simon i Halosys som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-147">**[Creating a Halosys test user](#creating-a-halosys-test-user)** - toohave a counterpart of Britta Simon in Halosys that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="0e1e7-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0e1e7-149">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0e1e7-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0e1e7-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0e1e7-151">I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i ditt Halosys program.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Halosys application.</span></span>


<span data-ttu-id="0e1e7-152">**Utför följande steg hello tooconfigure Azure AD enkel inloggning med Halosys:**</span><span class="sxs-lookup"><span data-stu-id="0e1e7-152">**tooconfigure Azure AD single sign-on with Halosys, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e1e7-153">I hello klassiska portalen på hello **Halosys** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-153">In hello classic portal, on hello **Halosys** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
     
    ![Konfigurera enkel inloggning][6] 

2. <span data-ttu-id="0e1e7-155">På hello **hur skulle du som användare toosign på tooHalosys** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-155">On hello **How would you like users toosign on tooHalosys** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_03.png) 

3. <span data-ttu-id="0e1e7-157">På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_04.png) 

    <span data-ttu-id="0e1e7-159">a.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-159">a.</span></span> <span data-ttu-id="0e1e7-160">I hello **logga URL** textruta typen hello URL som används av dina användare toosign på tooyour Halosys program med hjälp av hello följer mönstret: `https://<company-name>.Halosys.com/client-api/api`.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-160">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Halosys application using hello following pattern: `https://<company-name>.Halosys.com/client-api/api`.</span></span>

    <span data-ttu-id="0e1e7-161">b.In hello **Identifierare** textruta typen hello URL i hello följer mönstret: `https://<company-name>.Halosys.com`.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-161">b.In hello **Identifier URL** textbox, type hello URL in hello following pattern: `https://<company-name>.Halosys.com`.</span></span> 
         
4. <span data-ttu-id="0e1e7-162">På hello **Konfigurera enkel inloggning på Halosys** klickar du på **hämtar metadata**, och spara sedan hello på datorn:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-162">On hello **Configure single sign-on at Halosys** page, click **Download metadata**, and then save hello file on your computer:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_05.png)
   
5. <span data-ttu-id="0e1e7-164">tooget SSO konfigurerats för ditt program, Kontakta supportteamet för Halosys och ge dem hello följande:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-164">tooget SSO configured for your application, contact Halosys support team and provide them with hello following:</span></span>

    <span data-ttu-id="0e1e7-165">• hello hämtas **metadatafil**</span><span class="sxs-lookup"><span data-stu-id="0e1e7-165">• hello downloaded **metadata file**</span></span>
    
    <span data-ttu-id="0e1e7-166">• hello **SAML SSO-URL**</span><span class="sxs-lookup"><span data-stu-id="0e1e7-166">• hello **SAML SSO URL**</span></span>
    

6. <span data-ttu-id="0e1e7-167">Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-167">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Azure AD-Single Sign-On][10]

7. <span data-ttu-id="0e1e7-169">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-169">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0e1e7-171">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0e1e7-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="0e1e7-172">I det här avsnittet skapar du en testanvändare i hello klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-172">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>


![Skapa Azure AD-användare][20]

<span data-ttu-id="0e1e7-174">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0e1e7-174">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e1e7-175">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-175">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="0e1e7-177">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-177">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="0e1e7-178">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-178">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0e1e7-180">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-180">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="0e1e7-182">På hello **berätta om den här användaren** dialogrutan utför följande steg hello: ![att skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span><span class="sxs-lookup"><span data-stu-id="0e1e7-182">On hello **Tell us about this user** dialog page, perform hello following steps:  ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_05.png)</span></span> 

    <span data-ttu-id="0e1e7-183">a.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-183">a.</span></span> <span data-ttu-id="0e1e7-184">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-184">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="0e1e7-185">b.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-185">b.</span></span> <span data-ttu-id="0e1e7-186">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-186">In hello User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0e1e7-187">c.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-187">c.</span></span> <span data-ttu-id="0e1e7-188">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-188">Click **Next**.</span></span>

6.  <span data-ttu-id="0e1e7-189">På hello **användarprofil** dialogrutan utför följande steg hello: ![att skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span><span class="sxs-lookup"><span data-stu-id="0e1e7-189">On hello **User Profile** dialog page, perform hello following steps: ![Creating an Azure AD test user](./media/active-directory-saas-Halosys-tutorial/create_aaduser_06.png)</span></span> 

    <span data-ttu-id="0e1e7-190">a.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-190">a.</span></span> <span data-ttu-id="0e1e7-191">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-191">In hello **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="0e1e7-192">b.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-192">b.</span></span> <span data-ttu-id="0e1e7-193">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-193">In hello **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="0e1e7-194">c.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-194">c.</span></span> <span data-ttu-id="0e1e7-195">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-195">In hello **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="0e1e7-196">d.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-196">d.</span></span> <span data-ttu-id="0e1e7-197">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-197">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="0e1e7-198">e.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-198">e.</span></span> <span data-ttu-id="0e1e7-199">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-199">Click **Next**.</span></span>

7. <span data-ttu-id="0e1e7-200">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-200">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="0e1e7-202">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="0e1e7-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-Halosys-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="0e1e7-204">a.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-204">a.</span></span> <span data-ttu-id="0e1e7-205">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-205">Write down hello value of hello **New Password**.</span></span>

    <span data-ttu-id="0e1e7-206">b.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-206">b.</span></span> <span data-ttu-id="0e1e7-207">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="0e1e7-207">Click **Complete**.</span></span>   



### <a name="creating-a-halosys-test-user"></a><span data-ttu-id="0e1e7-208">Skapa en testanvändare Halosys</span><span class="sxs-lookup"><span data-stu-id="0e1e7-208">Creating a Halosys test user</span></span>

<span data-ttu-id="0e1e7-209">I det här avsnittet skapar du en användare som kallas Britta Simon i Halosys.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-209">In this section, you create a user called Britta Simon in Halosys.</span></span> <span data-ttu-id="0e1e7-210">Se tillsammans med Halosys support-teamet tooadd hello användare i hello Halosys plattform.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-210">Please work with Halosys support team tooadd hello users in hello Halosys platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0e1e7-211">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="0e1e7-211">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="0e1e7-212">I det här avsnittet Aktivera Britta Simon toouse Azure enkel inloggning genom att tilldela tooHalosys sin åtkomst.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-212">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooHalosys.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="0e1e7-214">**tooassign Britta Simon tooHalosys utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="0e1e7-214">**tooassign Britta Simon tooHalosys, perform hello following steps:**</span></span>

1. <span data-ttu-id="0e1e7-215">På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-215">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="0e1e7-217">Välj i listan med program hello **Halosys**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-217">In hello applications list, select **Halosys**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-Halosys-tutorial/tutorial_Halosys_50.png) 

3. <span data-ttu-id="0e1e7-219">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-219">In hello menu on hello top, click **Users**.</span></span>

    ![Tilldela användare][203]

4. <span data-ttu-id="0e1e7-221">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-221">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="0e1e7-222">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-222">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Tilldela användare][205]


### <a name="testing-single-sign-on"></a><span data-ttu-id="0e1e7-224">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="0e1e7-224">Testing single sign-on</span></span>

<span data-ttu-id="0e1e7-225">I det här avsnittet kan testa du Azure AD enkel inloggning konfigurationen med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0e1e7-226">Du bör få automatiskt inloggade tooyour Halosys programmet när du klickar på hello Halosys panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="0e1e7-226">When you click hello Halosys tile in hello Access Panel, you should get automatically signed-on tooyour Halosys application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="0e1e7-227">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0e1e7-227">Additional resources</span></span>

* [<span data-ttu-id="0e1e7-228">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0e1e7-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0e1e7-229">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="0e1e7-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-Halosys-tutorial/tutorial_general_205.png
