---
title: "Självstudier: Azure Active Directory-integrering med Soonr arbetsplats | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Soonr arbetsplats."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: f950b45d0beceab2fa17b7690c9de81ec6603089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a><span data-ttu-id="484c5-103">Självstudier: Azure Active Directory-integrering med Soonr arbetsplats</span><span class="sxs-lookup"><span data-stu-id="484c5-103">Tutorial: Azure Active Directory integration with Soonr Workplace</span></span>

<span data-ttu-id="484c5-104">hello syftet med den här kursen är tooshow du hur toointegrate Soonr arbetsplats med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="484c5-104">hello objective of this tutorial is tooshow you how toointegrate Soonr Workplace with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="484c5-105">Integrera Soonr arbetsplats med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="484c5-105">Integrating Soonr Workplace with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="484c5-106">Du kan styra i Azure AD som har åtkomst tooSoonr arbetsplats</span><span class="sxs-lookup"><span data-stu-id="484c5-106">You can control in Azure AD who has access tooSoonr Workplace</span></span>
- <span data-ttu-id="484c5-107">Du kan aktivera din användare tooautomatically get inloggade tooSoonr arbetsplats (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="484c5-107">You can enable your users tooautomatically get signed-on tooSoonr Workplace (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="484c5-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="484c5-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="484c5-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="484c5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="484c5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="484c5-110">Prerequisites</span></span>

<span data-ttu-id="484c5-111">tooconfigure Azure AD-integrering med Soonr arbetsplats, behöver du hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="484c5-111">tooconfigure Azure AD integration with Soonr Workplace, you need hello following items:</span></span>

- <span data-ttu-id="484c5-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="484c5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="484c5-113">En Soonr arbetsplatsen enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="484c5-113">A Soonr Workplace single-sign on enabled subscription</span></span>


> [!NOTE] 
> <span data-ttu-id="484c5-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="484c5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="484c5-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="484c5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="484c5-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="484c5-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="484c5-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="484c5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="484c5-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="484c5-118">Scenario description</span></span>
<span data-ttu-id="484c5-119">hello syftet med den här kursen är tooenable du tootest Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="484c5-119">hello objective of this tutorial is tooenable you tootest Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="484c5-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="484c5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="484c5-121">Att lägga till Soonr arbetsplats från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="484c5-121">Adding Soonr Workplace from hello gallery</span></span>
2. <span data-ttu-id="484c5-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="484c5-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-soonr-workplace-from-hello-gallery"></a><span data-ttu-id="484c5-123">Att lägga till Soonr arbetsplats från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="484c5-123">Adding Soonr Workplace from hello gallery</span></span>
<span data-ttu-id="484c5-124">tooconfigure hello integrering av Soonr arbetsplats i Azure AD, behöver du tooadd Soonr arbetsplats hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="484c5-124">tooconfigure hello integration of Soonr Workplace into Azure AD, you need tooadd Soonr Workplace from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="484c5-125">**tooadd Soonr arbetsplats från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="484c5-125">**tooadd Soonr Workplace from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="484c5-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="484c5-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="484c5-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="484c5-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="484c5-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="484c5-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Program][2]

4. <span data-ttu-id="484c5-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="484c5-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Program][3]

5. <span data-ttu-id="484c5-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="484c5-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
 
    ![Program][4]

6. <span data-ttu-id="484c5-135">Skriv i sökrutan hello **Soonr arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="484c5-135">In hello search box, type **Soonr Workplace**.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)

7. <span data-ttu-id="484c5-137">I resultatfönstret hello väljer **Soonr arbetsplats**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="484c5-137">In hello results pane, select **Soonr Workplace**, and then click **Complete** tooadd hello application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="484c5-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="484c5-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="484c5-140">hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD enkel inloggning med Soonr arbetsplats baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="484c5-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD single sign-on with Soonr Workplace based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="484c5-141">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Soonr arbetsplats tooan användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="484c5-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Soonr Workplace tooan user in Azure AD is.</span></span> <span data-ttu-id="484c5-142">Med andra ord måste en länk relationen mellan en Azure AD-användare och hello relaterade användare i Soonr arbetsplats toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="484c5-142">In other words, a link relationship between an Azure AD user and hello related user in Soonr Workplace needs toobe established.</span></span>  

<span data-ttu-id="484c5-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** Soonr arbetsplatsen.</span><span class="sxs-lookup"><span data-stu-id="484c5-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Soonr Workplace.</span></span>

<span data-ttu-id="484c5-144">tooconfigure och testa Azure AD enkel inloggning med Soonr arbetsplats, måste toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="484c5-144">tooconfigure and test Azure AD single sign-on with Soonr Workplace, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="484c5-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="484c5-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="484c5-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="484c5-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="484c5-147">**[Skapa en testanvändare Soonr arbetsplats](#creating-a-soonr-workplace-test-user)**  -toohave en motsvarighet för Britta Simon Soonr arbetsplatsen som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="484c5-147">**[Creating a Soonr Workplace test user](#creating-a-soonr-workplace-test-user)** - toohave a counterpart of Britta Simon in Soonr Workplace that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="484c5-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="484c5-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="484c5-149">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="484c5-149">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="484c5-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="484c5-150">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="484c5-151">I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Soonr arbetsplats.</span><span class="sxs-lookup"><span data-stu-id="484c5-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Soonr Workplace application.</span></span>


<span data-ttu-id="484c5-152">**Utför följande hello tooconfigure Azure AD enkel inloggning med Soonr arbetsplats:**</span><span class="sxs-lookup"><span data-stu-id="484c5-152">**tooconfigure Azure AD single sign-on with Soonr Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="484c5-153">I hello klassiska Azure-portalen på hello **Soonr arbetsplats** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="484c5-153">In hello Azure classic portal, on hello **Soonr Workplace** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>

    ![Konfigurera enkel inloggning][6] 

2. <span data-ttu-id="484c5-155">På hello **hur skulle du som användare toosign på tooSoonr arbetsplats** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="484c5-155">On hello **How would you like users toosign on tooSoonr Workplace** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 

3. <span data-ttu-id="484c5-157">På hello **konfigurera Appinställningar** dialogrutan utför följande steg hello:.</span><span class="sxs-lookup"><span data-stu-id="484c5-157">On hello **Configure App Settings** dialog page, perform hello following steps:.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    <span data-ttu-id="484c5-159">a.</span><span class="sxs-lookup"><span data-stu-id="484c5-159">a.</span></span> <span data-ttu-id="484c5-160">I hello **logga URL** textruta, ange ett URL-Adressen med hello följer mönstret: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span><span class="sxs-lookup"><span data-stu-id="484c5-160">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.</span></span>

    <span data-ttu-id="484c5-161">b.</span><span class="sxs-lookup"><span data-stu-id="484c5-161">b.</span></span> <span data-ttu-id="484c5-162">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="484c5-162">Click **Next**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="484c5-163">Observera att detta inte är hello verkliga värdet.</span><span class="sxs-lookup"><span data-stu-id="484c5-163">Please note that this is not hello real value.</span></span> <span data-ttu-id="484c5-164">Du har tooupdate värdet med hello faktiska inloggning URL.</span><span class="sxs-lookup"><span data-stu-id="484c5-164">You have tooupdate this value with hello actual Sign On URL.</span></span> <span data-ttu-id="484c5-165">Kontakta Soonr arbetsplats support-teamet tooget det här värdet.</span><span class="sxs-lookup"><span data-stu-id="484c5-165">Contact Soonr Workplace support team tooget this value.</span></span>

4. <span data-ttu-id="484c5-166">På hello **Konfigurera enkel inloggning på Soonr arbetsplats** klickar du på **hämtar metadata** och spara hello-filen på datorn:</span><span class="sxs-lookup"><span data-stu-id="484c5-166">On hello **Configure single sign-on at Soonr Workplace** page, click **Download metadata** and then save hello file on your computer:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 

5. <span data-ttu-id="484c5-168">tooget SSO konfigurerats för ditt program Kontakta supportteamet Soonr arbetsplats och ge dem hello följande:</span><span class="sxs-lookup"><span data-stu-id="484c5-168">tooget SSO configured for your application, contact your Soonr Workplace support team and provide them with hello following:</span></span> 

    <span data-ttu-id="484c5-169">• hello hämtas **Metadata** fil</span><span class="sxs-lookup"><span data-stu-id="484c5-169">•  hello downloaded **Metadata** file</span></span>

    <span data-ttu-id="484c5-170">• hello **utfärdar-URL**</span><span class="sxs-lookup"><span data-stu-id="484c5-170">•  hello **Issuer URL**</span></span>

    <span data-ttu-id="484c5-171">• hello **SAML SSO-URL**</span><span class="sxs-lookup"><span data-stu-id="484c5-171">•  hello **SAML SSO URL**</span></span>

    <span data-ttu-id="484c5-172">• hello **tjänst-URL för enkel Sign-Out**</span><span class="sxs-lookup"><span data-stu-id="484c5-172">•  hello **Single Sign-Out Service URL**</span></span>

    >[!NOTE]
    ><span data-ttu-id="484c5-173">Det här programmet har ersatts av <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask arbetsplats</a> och du kan se <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">detta</a> självstudiekursen för att konfigurera hello program med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="484c5-173">This application is superseded by <a href="https://azure.microsoft.com/en-us/marketplace/partners/autotask-corporataion/autotask/">Autotask Workplace</a> and you can refer <a href="https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-autotaskworkplace-tutorial">this</a> tutorial for configuring hello application with Azure AD.</span></span>
   
6. <span data-ttu-id="484c5-174">Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska Azure-portalen, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="484c5-174">In hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>

    ![Azure AD-Single Sign-On][10]

7. <span data-ttu-id="484c5-176">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="484c5-176">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
  
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="484c5-178">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="484c5-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="484c5-179">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="484c5-179">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>  

![Skapa Azure AD-användare][20]

<span data-ttu-id="484c5-181">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="484c5-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="484c5-182">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="484c5-182">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="484c5-184">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="484c5-184">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="484c5-185">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="484c5-185">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="484c5-187">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="484c5-187">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="484c5-189">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="484c5-189">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 

    <span data-ttu-id="484c5-191">a.</span><span class="sxs-lookup"><span data-stu-id="484c5-191">a.</span></span> <span data-ttu-id="484c5-192">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="484c5-192">As Type Of User, select New user in your organization.</span></span>

    <span data-ttu-id="484c5-193">b.</span><span class="sxs-lookup"><span data-stu-id="484c5-193">b.</span></span> <span data-ttu-id="484c5-194">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="484c5-194">In hello User Name **textbox**, type **BrittaSimon**.</span></span>

    <span data-ttu-id="484c5-195">c.</span><span class="sxs-lookup"><span data-stu-id="484c5-195">c.</span></span> <span data-ttu-id="484c5-196">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="484c5-196">Click **Next**.</span></span>

6.  <span data-ttu-id="484c5-197">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="484c5-197">On hello **User Profile** dialog page, perform hello following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 

    <span data-ttu-id="484c5-199">a.</span><span class="sxs-lookup"><span data-stu-id="484c5-199">a.</span></span> <span data-ttu-id="484c5-200">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="484c5-200">In hello **First Name** textbox, type **Britta**.</span></span>  

    <span data-ttu-id="484c5-201">b.</span><span class="sxs-lookup"><span data-stu-id="484c5-201">b.</span></span> <span data-ttu-id="484c5-202">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="484c5-202">In hello **Last Name** textbox, type, **Simon**.</span></span>

    <span data-ttu-id="484c5-203">c.</span><span class="sxs-lookup"><span data-stu-id="484c5-203">c.</span></span> <span data-ttu-id="484c5-204">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="484c5-204">In hello **Display Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="484c5-205">d.</span><span class="sxs-lookup"><span data-stu-id="484c5-205">d.</span></span> <span data-ttu-id="484c5-206">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="484c5-206">In hello **Role** list, select **User**.</span></span>

    <span data-ttu-id="484c5-207">e.</span><span class="sxs-lookup"><span data-stu-id="484c5-207">e.</span></span> <span data-ttu-id="484c5-208">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="484c5-208">Click **Next**.</span></span>

7. <span data-ttu-id="484c5-209">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="484c5-209">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="484c5-211">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="484c5-211">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 

    <span data-ttu-id="484c5-213">a.</span><span class="sxs-lookup"><span data-stu-id="484c5-213">a.</span></span> <span data-ttu-id="484c5-214">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="484c5-214">Write down hello value of hello **New Password**.</span></span>

    <span data-ttu-id="484c5-215">b.</span><span class="sxs-lookup"><span data-stu-id="484c5-215">b.</span></span> <span data-ttu-id="484c5-216">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="484c5-216">Click **Complete**.</span></span>   



### <a name="creating-a-soonr-workplace-test-user"></a><span data-ttu-id="484c5-217">Skapa en testanvändare Soonr arbetsplats</span><span class="sxs-lookup"><span data-stu-id="484c5-217">Creating a Soonr Workplace test user</span></span>

<span data-ttu-id="484c5-218">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon Soonr arbetsplatsen.</span><span class="sxs-lookup"><span data-stu-id="484c5-218">hello objective of this section is toocreate a user called Britta Simon in Soonr Workplace.</span></span> <span data-ttu-id="484c5-219">Se tillsammans med Soonr arbetsplats support-teamet toocreate en användare i hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="484c5-219">Please work with Soonr Workplace support team toocreate a user in hello platform.</span></span> <span data-ttu-id="484c5-220">Du kan höja hello-supportärende med Soonr från <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">här</a>.</span><span class="sxs-lookup"><span data-stu-id="484c5-220">You can raise hello support ticket with Soonr from <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">here</a>.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="484c5-221">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="484c5-221">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="484c5-222">hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure enkel inloggning genom att ge sina access tooSoonr arbetsplats.</span><span class="sxs-lookup"><span data-stu-id="484c5-222">hello objective of this section is tooenabling Britta Simon toouse Azure single sign-on by granting her access tooSoonr Workplace.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="484c5-224">**tooassign Britta Simon tooSoonr arbetsplats utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="484c5-224">**tooassign Britta Simon tooSoonr Workplace, perform hello following steps:**</span></span>

1. <span data-ttu-id="484c5-225">På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="484c5-225">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="484c5-227">Välj i listan med program hello **Soonr arbetsplats**.</span><span class="sxs-lookup"><span data-stu-id="484c5-227">In hello applications list, select **Soonr Workplace**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 

1. <span data-ttu-id="484c5-229">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="484c5-229">In hello menu on hello top, click **Users**.</span></span>

    ![Tilldela användare][203] 

1. <span data-ttu-id="484c5-231">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="484c5-231">In hello Users list, select **Britta Simon**.</span></span>

2. <span data-ttu-id="484c5-232">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="484c5-232">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Tilldela användare][205]



### <a name="testing-single-sign-on"></a><span data-ttu-id="484c5-234">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="484c5-234">Testing single sign-on</span></span>

<span data-ttu-id="484c5-235">hello syftet med det här avsnittet är tootest din Azure AD-konfiguration för enkel inloggning med hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="484c5-235">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>  
<span data-ttu-id="484c5-236">Du bör få automatiskt inloggade tooyour Soonr arbetsplats programmet när du klickar på hello Soonr arbetsplats panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="484c5-236">When you click hello Soonr Workplace tile in hello Access Panel, you should get automatically signed-on tooyour Soonr Workplace application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="484c5-237">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="484c5-237">Additional resources</span></span>

* [<span data-ttu-id="484c5-238">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="484c5-238">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="484c5-239">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="484c5-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png
