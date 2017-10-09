---
title: "Självstudier: Azure Active Directory-integrering med Wizergos produktivitetsprogram | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och Wizergos produktivitetsprogram."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: acc04396-13c5-4c24-ab9a-30fbc9234ebd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
ms.openlocfilehash: cdd186c38c426dde404ed8bb84700faf9c8c1a46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a><span data-ttu-id="e34b1-103">Självstudier: Azure Active Directory-integrering med Wizergos produktivitetsprogram</span><span class="sxs-lookup"><span data-stu-id="e34b1-103">Tutorial: Azure Active Directory integration with Wizergos Productivity Software</span></span>
<span data-ttu-id="e34b1-104">hello syftet med den här kursen är tooshow du hur toointegrate Wizergos produktivitetsprogram med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="e34b1-104">hello objective of this tutorial is tooshow you how toointegrate Wizergos Productivity Software  with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e34b1-105">Integrera Wizergos produktivitetsprogram med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="e34b1-105">Integrating Wizergos Productivity Software with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="e34b1-106">Du kan styra i Azure AD som har åtkomst tooWizergos produktivitetsprogram</span><span class="sxs-lookup"><span data-stu-id="e34b1-106">You can control in Azure AD who has access tooWizergos Productivity Software</span></span>
* <span data-ttu-id="e34b1-107">Du kan aktivera din användare tooautomatically get inloggade tooWizergos produktivitetsprogram enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="e34b1-107">You can enable your users tooautomatically get signed-on tooWizergos Productivity Software single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="e34b1-108">Du kan hantera dina konton i en central plats - hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e34b1-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="e34b1-109">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e34b1-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e34b1-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e34b1-110">Prerequisites</span></span>
<span data-ttu-id="e34b1-111">tooconfigure Azure AD-integrering med Wizergos produktivitetsprogram måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="e34b1-111">tooconfigure Azure AD integration with Wizergos Productivity Software, you need hello following items:</span></span>

* <span data-ttu-id="e34b1-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="e34b1-112">An Azure AD subscription</span></span>
* <span data-ttu-id="e34b1-113">En prenumeration Wizergos produktivitet programvara SSO aktiverad</span><span class="sxs-lookup"><span data-stu-id="e34b1-113">A Wizergos Productivity Software SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="e34b1-114">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="e34b1-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="e34b1-115">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="e34b1-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="e34b1-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="e34b1-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="e34b1-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e34b1-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e34b1-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="e34b1-118">Scenario description</span></span>
<span data-ttu-id="e34b1-119">hello syftet med den här kursen är tooenable du tootest Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="e34b1-119">hello objective of this tutorial is tooenable you tootest Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="e34b1-120">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="e34b1-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e34b1-121">Att lägga till Wizergos produktivitetsprogram från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e34b1-121">Adding Wizergos Productivity Software from hello gallery</span></span>
2. <span data-ttu-id="e34b1-122">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="e34b1-122">Configuring and testing Azure AD SSO</span></span>

## <a name="adding-wizergos-productivity-software-from-hello-gallery"></a><span data-ttu-id="e34b1-123">Att lägga till Wizergos produktivitetsprogram från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="e34b1-123">Adding Wizergos Productivity Software from hello gallery</span></span>
<span data-ttu-id="e34b1-124">tooconfigure hello integrering av Wizergos produktivitetsprogram i Azure AD, behöver du tooadd Wizergos produktivitetsprogram hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="e34b1-124">tooconfigure hello integration of Wizergos Productivity Software into Azure AD, you need tooadd Wizergos Productivity Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e34b1-125">**tooadd Wizergos produktivitetsprogram från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e34b1-125">**tooadd Wizergos Productivity Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e34b1-126">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-126">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="e34b1-128">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="e34b1-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="e34b1-129">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="e34b1-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Program][2]
4. <span data-ttu-id="e34b1-131">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="e34b1-131">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3]
5. <span data-ttu-id="e34b1-133">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Program][4]
6. <span data-ttu-id="e34b1-135">Skriv i sökrutan hello **Wizergos produktivitetsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-135">In hello search box, type **Wizergos Productivity Software**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. <span data-ttu-id="e34b1-137">Markera hello resultat på panelen **Wizergos produktivitetsprogram**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="e34b1-137">In hello results panel, select **Wizergos Productivity Software**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Att välja hello app i hello-galleriet](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a><span data-ttu-id="e34b1-139">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="e34b1-139">Configure and test Azure AD SSO</span></span>
<span data-ttu-id="e34b1-140">hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD SSO med Wizergos produktivitetsprogram baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="e34b1-140">hello objective of this section is tooshow you how tooconfigure and test Azure AD SSO with Wizergos Productivity Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e34b1-141">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i Wizergos produktivitetsprogram tooan användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e34b1-141">For SSO toowork, Azure AD needs tooknow what hello counterpart user in Wizergos Productivity Software tooan user in Azure AD is.</span></span> <span data-ttu-id="e34b1-142">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i Wizergos produktivitetsprogram toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="e34b1-142">In other words, a link relationship between an Azure AD user and hello related user in Wizergos Productivity Software needs toobe established.</span></span>

<span data-ttu-id="e34b1-143">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i Wizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="e34b1-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Wizergos Productivity Software.</span></span>

<span data-ttu-id="e34b1-144">tooconfigure och testa Azure AD enkel inloggning med BynWizergos produktivitet Softwareder, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="e34b1-144">tooconfigure and test Azure AD single sign-on with BynWizergos Productivity Softwareder, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e34b1-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="e34b1-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e34b1-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e34b1-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e34b1-147">**[Skapa en testanvändare Wizergos produktivitetsprogram](#creating-a-wizergos-productivity-software-test-user)**  -toohave en motsvarighet för Britta Simon i Wizergos produktivitetsprogram som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="e34b1-147">**[Creating a Wizergos Productivity Software test user](#creating-a-wizergos-productivity-software-test-user)** - toohave a counterpart of Britta Simon in Wizergos Productivity Software that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="e34b1-148">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="e34b1-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e34b1-149">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="e34b1-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="e34b1-150">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e34b1-150">Configuring Azure AD SSO</span></span>
<span data-ttu-id="e34b1-151">I det här avsnittet Aktivera Azure AD enkel inloggning i hello klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Wizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="e34b1-151">In this section, you enable Azure AD single sign-on in hello classic portal and configure single sign-on in your Wizergos Productivity Software application.</span></span>

<span data-ttu-id="e34b1-152">**tooconfigure Azure AD enkel inloggning med Wizergos produktivitetsprogram utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e34b1-152">**tooconfigure Azure AD single sign-on with Wizergos Productivity Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="e34b1-153">I hello klassiska portalen på hello **Wizergos produktivitetsprogram** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e34b1-153">In hello classic portal, on hello **Wizergos Productivity Software** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 
2. <span data-ttu-id="e34b1-155">På hello **hur skulle du som användare toosign på tooWizergos produktivitetsprogram** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="e34b1-155">On hello **How would you like users toosign on tooWizergos Productivity Software** page, select **Azure AD Single Sign-On**, and then click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. <span data-ttu-id="e34b1-157">På hello **konfigurera Appinställningar** dialogrutan sidan, klickar du på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="e34b1-157">On hello **Configure App Settings** dialog page, click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. <span data-ttu-id="e34b1-159">På hello **Konfigurera enkel inloggning på Wizergos produktivitetsprogram** klickar du på **hämta certifikat**, och spara sedan hello på datorn:</span><span class="sxs-lookup"><span data-stu-id="e34b1-159">On hello **Configure single sign-on at Wizergos Productivity Software** page, click **Download certificate**, and then save hello file on your computer:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. <span data-ttu-id="e34b1-161">I en annan webbläsarfönstret, inloggning tooyour Wizergos produktivitetsprogram klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="e34b1-161">In a different web browser window, sign-on tooyour Wizergos Productivity Software tenant as an administrator.</span></span>
6. <span data-ttu-id="e34b1-162">Hej hamburger-menyn, Välj **Admin**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-162">From hello hamburger menu, select **Admin**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. <span data-ttu-id="e34b1-164">Markera i Administrationssida på vänstra menyn **AUTENTISERING** och klicka på **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-164">In Admin page on left hand menu select **AUTHENTICATION** and click on **Azure AD**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. <span data-ttu-id="e34b1-166">Utför följande steg hello **AUTENTISERING** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="e34b1-166">Perform hello following steps on **AUTHENTICATION** section.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. <span data-ttu-id="e34b1-168">Klicka på **överför** knappen tooupload hello hämtat certifikat från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e34b1-168">Click **UPLOAD** button tooupload hello downloaded certificate from Azure AD.</span></span> 
  2. <span data-ttu-id="e34b1-169">I hello **utfärdar-URL** textruta placera hello värdet för **utfärdar-URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e34b1-169">In hello **Issuer URL** textbox put hello value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
  3. <span data-ttu-id="e34b1-170">I hello **URL för enkel inloggning** textruta placera hello värdet för **inloggning tjänst-URL för enkel** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e34b1-170">In hello **Single Sign-On URL** textbox put hello value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
  4. <span data-ttu-id="e34b1-171">I hello **Sign-Out-URL för enkel** textruta placera hello värdet för **tjänst-URL för enkel Sign-out** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e34b1-171">In hello **Single Sign-Out URL** textbox put hello value of **Single Sign-out Service URL** from Azure AD application configuration wizard.</span></span>
  5. <span data-ttu-id="e34b1-172">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e34b1-172">Click **Save** button.</span></span>
9. <span data-ttu-id="e34b1-173">Välj hello konfiguration för enkel inloggning bekräftelse i hello klassiska portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-173">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][10]
10. <span data-ttu-id="e34b1-175">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-175">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
    
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e34b1-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="e34b1-177">Create an Azure AD test user</span></span>
<span data-ttu-id="e34b1-178">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e34b1-178">hello objective of this section is toocreate a test user in hello classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="e34b1-180">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="e34b1-180">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e34b1-181">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-181">In hello **Azure classic Portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="e34b1-183">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="e34b1-183">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="e34b1-184">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-184">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="e34b1-186">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-186">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="e34b1-188">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e34b1-188">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="e34b1-190">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="e34b1-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="e34b1-191">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-191">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="e34b1-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-192">Click **Next**.</span></span>
6. <span data-ttu-id="e34b1-193">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e34b1-193">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="e34b1-195">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-195">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="e34b1-196">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-196">In hello **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="e34b1-197">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-197">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="e34b1-198">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-198">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="e34b1-199">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-199">Click **Next**.</span></span>
7. <span data-ttu-id="e34b1-200">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-200">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="e34b1-202">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="e34b1-202">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. <span data-ttu-id="e34b1-204">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-204">Write down hello value of hello **New Password**.</span></span>
  2. <span data-ttu-id="e34b1-205">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="e34b1-205">Click **Complete**.</span></span>   

### <a name="create-a-wizergos-productivity-software-test-user"></a><span data-ttu-id="e34b1-206">Skapa en testanvändare Wizergos produktivitetsprogram</span><span class="sxs-lookup"><span data-stu-id="e34b1-206">Create a Wizergos Productivity Software test user</span></span>
<span data-ttu-id="e34b1-207">I det här avsnittet skapar du en användare som kallas Britta Simon i Wizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="e34b1-207">In this section, you create a user called Britta Simon in Wizergos Productivity Software.</span></span> <span data-ttu-id="e34b1-208">Se tillsammans med Wizergos produktivitetsprogram supportteamet via [ support@wizergos.com ](emailTo:support@wizergos.com) tooadd hello användare i hello Wizergos produktivitetsprogram plattform.</span><span class="sxs-lookup"><span data-stu-id="e34b1-208">Please work with Wizergos Productivity Software support team via [support@wizergos.com](emailTo:support@wizergos.com) tooadd hello users in hello Wizergos Productivity Software platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e34b1-209">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="e34b1-209">Assign hello Azure AD test user</span></span>
<span data-ttu-id="e34b1-210">hello syftet med det här avsnittet är tooenabling Britta Simon toouse Azure SSO genom att ge sina access tooWizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="e34b1-210">hello objective of this section is tooenabling Britta Simon toouse Azure SSO by granting her access tooWizergos Productivity Software.</span></span>

  ![Tilldela användare][200]

<span data-ttu-id="e34b1-212">**tooassign Britta Simon tooWizergos produktivitetsprogram, utför följande steg hello:**</span><span class="sxs-lookup"><span data-stu-id="e34b1-212">**tooassign Britta Simon tooWizergos Productivity Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="e34b1-213">På hello klassiska portalen tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="e34b1-213">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Tilldela användare][201]
2. <span data-ttu-id="e34b1-215">Välj i listan med program hello **Wizergos produktivitetsprogram**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-215">In hello applications list, select **Wizergos Productivity Software**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. <span data-ttu-id="e34b1-217">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-217">In hello menu on hello top, click **Users**.</span></span>
   
    ![Tilldela användare][203]
4. <span data-ttu-id="e34b1-219">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-219">In hello Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="e34b1-220">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="e34b1-220">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="e34b1-222">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="e34b1-222">Test single sign-on</span></span>
<span data-ttu-id="e34b1-223">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e34b1-223">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="e34b1-224">Du bör få automatiskt inloggade tooyour Wizergos produktivitetsprogram programmet när du klickar på hello Wizergos produktivitetsprogram panelen i hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="e34b1-224">When you click hello Wizergos Productivity Software tile in hello Access Panel, you should get automatically signed-on tooyour Wizergos Productivity Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e34b1-225">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e34b1-225">Additional resources</span></span>
* [<span data-ttu-id="e34b1-226">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e34b1-226">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e34b1-227">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e34b1-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
