---
title: "Självstudier: Azure Active Directory-integrering med Wizergos produktivitetsprogram | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Wizergos produktivitetsprogram."
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
ms.openlocfilehash: 73b3bc05aeb337c12acb7e47c0dbebe6d0196530
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a><span data-ttu-id="492ab-103">Självstudier: Azure Active Directory-integrering med Wizergos produktivitetsprogram</span><span class="sxs-lookup"><span data-stu-id="492ab-103">Tutorial: Azure Active Directory integration with Wizergos Productivity Software</span></span>
<span data-ttu-id="492ab-104">Syftet med den här kursen är att visa dig hur du integrerar Wizergos produktivitetsprogram med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="492ab-104">The objective of this tutorial is to show you how to integrate Wizergos Productivity Software  with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="492ab-105">Integrera Wizergos produktivitetsprogram med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="492ab-105">Integrating Wizergos Productivity Software with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="492ab-106">Du kan styra i Azure AD som har åtkomst till Wizergos produktivitetsprogram</span><span class="sxs-lookup"><span data-stu-id="492ab-106">You can control in Azure AD who has access to Wizergos Productivity Software</span></span>
* <span data-ttu-id="492ab-107">Du kan aktivera användarna att automatiskt hämta loggat in på Wizergos produktivitetsprogram enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="492ab-107">You can enable your users to automatically get signed-on to Wizergos Productivity Software single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="492ab-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="492ab-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="492ab-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="492ab-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="492ab-110">Krav</span><span class="sxs-lookup"><span data-stu-id="492ab-110">Prerequisites</span></span>
<span data-ttu-id="492ab-111">För att konfigurera Azure AD-integrering med Wizergos produktivitetsprogram, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="492ab-111">To configure Azure AD integration with Wizergos Productivity Software, you need the following items:</span></span>

* <span data-ttu-id="492ab-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="492ab-112">An Azure AD subscription</span></span>
* <span data-ttu-id="492ab-113">En prenumeration Wizergos produktivitet programvara SSO aktiverad</span><span class="sxs-lookup"><span data-stu-id="492ab-113">A Wizergos Productivity Software SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="492ab-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="492ab-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="492ab-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="492ab-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="492ab-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="492ab-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="492ab-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="492ab-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="492ab-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="492ab-118">Scenario description</span></span>
<span data-ttu-id="492ab-119">Syftet med den här kursen är att du ska testa Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="492ab-119">The objective of this tutorial is to enable you to test Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="492ab-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="492ab-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="492ab-121">Att lägga till Wizergos produktivitetsprogram från galleriet</span><span class="sxs-lookup"><span data-stu-id="492ab-121">Adding Wizergos Productivity Software from the gallery</span></span>
2. <span data-ttu-id="492ab-122">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="492ab-122">Configuring and testing Azure AD SSO</span></span>

## <a name="adding-wizergos-productivity-software-from-the-gallery"></a><span data-ttu-id="492ab-123">Att lägga till Wizergos produktivitetsprogram från galleriet</span><span class="sxs-lookup"><span data-stu-id="492ab-123">Adding Wizergos Productivity Software from the gallery</span></span>
<span data-ttu-id="492ab-124">Du måste lägga till Wizergos produktivitetsprogram från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Wizergos produktivitetsprogram i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="492ab-124">To configure the integration of Wizergos Productivity Software into Azure AD, you need to add Wizergos Productivity Software from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="492ab-125">**Utför följande steg för att lägga till Wizergos produktivitetsprogram från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="492ab-125">**To add Wizergos Productivity Software from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="492ab-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="492ab-126">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="492ab-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="492ab-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="492ab-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="492ab-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Program][2]
4. <span data-ttu-id="492ab-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="492ab-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Program][3]
5. <span data-ttu-id="492ab-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="492ab-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Program][4]
6. <span data-ttu-id="492ab-135">I sökrutan skriver **Wizergos produktivitetsprogram**.</span><span class="sxs-lookup"><span data-stu-id="492ab-135">In the search box, type **Wizergos Productivity Software**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)
7. <span data-ttu-id="492ab-137">Välj i resultatpanelen **Wizergos produktivitetsprogram**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="492ab-137">In the results panel, select **Wizergos Productivity Software**, and then click **Complete** to add the application.</span></span>
   
    ![Markera appen i galleriet](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

## <a name="configure-and-test-azure-ad-sso"></a><span data-ttu-id="492ab-139">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="492ab-139">Configure and test Azure AD SSO</span></span>
<span data-ttu-id="492ab-140">Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Azure AD SSO med Wizergos produktivitetsprogram baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="492ab-140">The objective of this section is to show you how to configure and test Azure AD SSO with Wizergos Productivity Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="492ab-141">För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i Wizergos produktivitetsprogram till en användare i Azure AD är okänt.</span><span class="sxs-lookup"><span data-stu-id="492ab-141">For SSO to work, Azure AD needs to know what the counterpart user in Wizergos Productivity Software to an user in Azure AD is.</span></span> <span data-ttu-id="492ab-142">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Wizergos produktivitetsprogram upprättas.</span><span class="sxs-lookup"><span data-stu-id="492ab-142">In other words, a link relationship between an Azure AD user and the related user in Wizergos Productivity Software needs to be established.</span></span>

<span data-ttu-id="492ab-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Wizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="492ab-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Wizergos Productivity Software.</span></span>

<span data-ttu-id="492ab-144">Om du vill konfigurera och testa Azure AD enkel inloggning med BynWizergos produktivitet Softwareder, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="492ab-144">To configure and test Azure AD single sign-on with BynWizergos Productivity Softwareder, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="492ab-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="492ab-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="492ab-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="492ab-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="492ab-147">**[Skapa en testanvändare Wizergos produktivitetsprogram](#creating-a-wizergos-productivity-software-test-user)**  – har en motsvarighet för Britta Simon Wizergos produktivitetsprogram som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="492ab-147">**[Creating a Wizergos Productivity Software test user](#creating-a-wizergos-productivity-software-test-user)** - to have a counterpart of Britta Simon in Wizergos Productivity Software that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="492ab-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="492ab-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="492ab-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="492ab-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="492ab-150">Konfigurera Azure AD för enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="492ab-150">Configuring Azure AD SSO</span></span>
<span data-ttu-id="492ab-151">I det här avsnittet Aktivera Azure AD enkel inloggning i den klassiska portalen och konfigurera enkel inloggning i tillämpningsprogrammet Wizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="492ab-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your Wizergos Productivity Software application.</span></span>

<span data-ttu-id="492ab-152">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Wizergos produktivitetsprogram:**</span><span class="sxs-lookup"><span data-stu-id="492ab-152">**To configure Azure AD single sign-on with Wizergos Productivity Software, perform the following steps:**</span></span>

1. <span data-ttu-id="492ab-153">I den klassiska portalen på den **Wizergos produktivitetsprogram** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning**  dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="492ab-153">In the classic portal, on the **Wizergos Productivity Software** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 
2. <span data-ttu-id="492ab-155">På den **hur vill du att användarna kan logga in på Wizergos produktivitetsprogram** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="492ab-155">On the **How would you like users to sign on to Wizergos Productivity Software** page, select **Azure AD Single Sign-On**, and then click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)
3. <span data-ttu-id="492ab-157">På den **konfigurera Appinställningar** dialogrutan sidan, klickar du på **nästa**:</span><span class="sxs-lookup"><span data-stu-id="492ab-157">On the **Configure App Settings** dialog page, click **Next**:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)
4. <span data-ttu-id="492ab-159">På den **Konfigurera enkel inloggning på Wizergos produktivitetsprogram** klickar du på **hämta certifikat**, och sedan spara filen på datorn:</span><span class="sxs-lookup"><span data-stu-id="492ab-159">On the **Configure single sign-on at Wizergos Productivity Software** page, click **Download certificate**, and then save the file on your computer:</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)
5. <span data-ttu-id="492ab-161">I en annan webbläsarfönster inloggning till Wizergos produktivitetsprogram-klient som administratör.</span><span class="sxs-lookup"><span data-stu-id="492ab-161">In a different web browser window, sign-on to your Wizergos Productivity Software tenant as an administrator.</span></span>
6. <span data-ttu-id="492ab-162">Välj menyn hamburger **Admin**.</span><span class="sxs-lookup"><span data-stu-id="492ab-162">From the hamburger menu, select **Admin**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)
7. <span data-ttu-id="492ab-164">Markera i Administrationssida på vänstra menyn **AUTENTISERING** och klicka på **Azure AD**.</span><span class="sxs-lookup"><span data-stu-id="492ab-164">In Admin page on left hand menu select **AUTHENTICATION** and click on **Azure AD**.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)
8. <span data-ttu-id="492ab-166">Utför följande steg på **AUTENTISERING** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="492ab-166">Perform the following steps on **AUTHENTICATION** section.</span></span>
   
    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)
  1. <span data-ttu-id="492ab-168">Klicka på **överför** för att ladda upp det hämta certifikatet från Azure AD.</span><span class="sxs-lookup"><span data-stu-id="492ab-168">Click **UPLOAD** button to upload the downloaded certificate from Azure AD.</span></span> 
  2. <span data-ttu-id="492ab-169">I den **utfärdar-URL** textruta ange värdet för **utfärdar-URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="492ab-169">In the **Issuer URL** textbox put the value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
  3. <span data-ttu-id="492ab-170">I den **URL för enkel inloggning** textruta ange värdet för **inloggning tjänst-URL för enkel** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="492ab-170">In the **Single Sign-On URL** textbox put the value of **Single Sign-on Service URL** from Azure AD application configuration wizard.</span></span>
  4. <span data-ttu-id="492ab-171">I den **Sign-Out-URL för enkel** textruta ange värdet för **tjänst-URL för enkel Sign-out** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="492ab-171">In the **Single Sign-Out URL** textbox put the value of **Single Sign-out Service URL** from Azure AD application configuration wizard.</span></span>
  5. <span data-ttu-id="492ab-172">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="492ab-172">Click **Save** button.</span></span>
9. <span data-ttu-id="492ab-173">I den klassiska portalen markerar du konfigurationen för enkel inloggning bekräftelsen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="492ab-173">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][10]
10. <span data-ttu-id="492ab-175">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="492ab-175">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
    
    ![Azure AD-Single Sign-On][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="492ab-177">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="492ab-177">Create an Azure AD test user</span></span>
<span data-ttu-id="492ab-178">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="492ab-178">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="492ab-180">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="492ab-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="492ab-181">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="492ab-181">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="492ab-183">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="492ab-183">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="492ab-184">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="492ab-184">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="492ab-186">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="492ab-186">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="492ab-188">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="492ab-188">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="492ab-190">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="492ab-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="492ab-191">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="492ab-191">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="492ab-192">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="492ab-192">Click **Next**.</span></span>
6. <span data-ttu-id="492ab-193">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="492ab-193">On the **User Profile** dialog page, perform the following steps:</span></span>
   
   ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="492ab-195">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="492ab-195">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="492ab-196">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="492ab-196">In the **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="492ab-197">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="492ab-197">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="492ab-198">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="492ab-198">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="492ab-199">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="492ab-199">Click **Next**.</span></span>
7. <span data-ttu-id="492ab-200">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="492ab-200">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="492ab-202">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="492ab-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)
  1. <span data-ttu-id="492ab-204">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="492ab-204">Write down the value of the **New Password**.</span></span>
  2. <span data-ttu-id="492ab-205">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="492ab-205">Click **Complete**.</span></span>   

### <a name="create-a-wizergos-productivity-software-test-user"></a><span data-ttu-id="492ab-206">Skapa en testanvändare Wizergos produktivitetsprogram</span><span class="sxs-lookup"><span data-stu-id="492ab-206">Create a Wizergos Productivity Software test user</span></span>
<span data-ttu-id="492ab-207">I det här avsnittet skapar du en användare som kallas Britta Simon i Wizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="492ab-207">In this section, you create a user called Britta Simon in Wizergos Productivity Software.</span></span> <span data-ttu-id="492ab-208">Se tillsammans med Wizergos produktivitetsprogram supportteamet via [ support@wizergos.com ](emailTo:support@wizergos.com) att lägga till användare i Wizergos produktivitetsprogram-plattformen.</span><span class="sxs-lookup"><span data-stu-id="492ab-208">Please work with Wizergos Productivity Software support team via [support@wizergos.com](emailTo:support@wizergos.com) to add the users in the Wizergos Productivity Software platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="492ab-209">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="492ab-209">Assign the Azure AD test user</span></span>
<span data-ttu-id="492ab-210">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure SSO genom att bevilja sin åtkomst till Wizergos produktivitetsprogram.</span><span class="sxs-lookup"><span data-stu-id="492ab-210">The objective of this section is to enabling Britta Simon to use Azure SSO by granting her access to Wizergos Productivity Software.</span></span>

  ![Tilldela användare][200]

<span data-ttu-id="492ab-212">**Om du vill tilldela Wizergos produktivitetsprogram Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="492ab-212">**To assign Britta Simon to Wizergos Productivity Software, perform the following steps:**</span></span>

1. <span data-ttu-id="492ab-213">På den klassiska portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="492ab-213">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Tilldela användare][201]
2. <span data-ttu-id="492ab-215">Välj i listan med program **Wizergos produktivitetsprogram**.</span><span class="sxs-lookup"><span data-stu-id="492ab-215">In the applications list, select **Wizergos Productivity Software**.</span></span>
   
    ![Konfigurera enkel inloggning](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)
3. <span data-ttu-id="492ab-217">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="492ab-217">In the menu on the top, click **Users**.</span></span>
   
    ![Tilldela användare][203]
4. <span data-ttu-id="492ab-219">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="492ab-219">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="492ab-220">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="492ab-220">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="492ab-222">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="492ab-222">Test single sign-on</span></span>
<span data-ttu-id="492ab-223">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="492ab-223">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="492ab-224">När du klickar på panelen Wizergos produktivitetsprogram på åtkomstpanelen du bör få automatiskt loggat in på ditt Wizergos produktivitetsprogram program.</span><span class="sxs-lookup"><span data-stu-id="492ab-224">When you click the Wizergos Productivity Software tile in the Access Panel, you should get automatically signed-on to your Wizergos Productivity Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="492ab-225">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="492ab-225">Additional resources</span></span>
* [<span data-ttu-id="492ab-226">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="492ab-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="492ab-227">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="492ab-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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
