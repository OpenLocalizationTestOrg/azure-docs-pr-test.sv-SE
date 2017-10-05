---
title: "Självstudier: Azure Active Directory-integrering med PlanMyLeave | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och PlanMyLeave."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: ba418a641b339a0d94a3c7b2596d37fbd88a30c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a><span data-ttu-id="11296-103">Självstudier: Azure Active Directory-integrering med PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="11296-103">Tutorial: Azure Active Directory integration with PlanMyLeave</span></span>

<span data-ttu-id="11296-104">I kursen får lära du att integrera PlanMyLeave med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="11296-104">In this tutorial, you learn how to integrate PlanMyLeave with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="11296-105">Integrera PlanMyLeave med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="11296-105">Integrating PlanMyLeave with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="11296-106">Du kan styra i Azure AD som har åtkomst till PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="11296-106">You can control in Azure AD who has access to PlanMyLeave</span></span>
- <span data-ttu-id="11296-107">Du kan aktivera användarna att automatiskt hämta loggat in på PlanMyLeave (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="11296-107">You can enable your users to automatically get signed-on to PlanMyLeave (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="11296-108">Du kan hantera dina konton i en central plats - till Azure-hanteringsportalen</span><span class="sxs-lookup"><span data-stu-id="11296-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="11296-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="11296-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="11296-110">Krav</span><span class="sxs-lookup"><span data-stu-id="11296-110">Prerequisites</span></span>

<span data-ttu-id="11296-111">För att konfigurera Azure AD-integrering med PlanMyLeave, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="11296-111">To configure Azure AD integration with PlanMyLeave, you need the following items:</span></span>

- <span data-ttu-id="11296-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="11296-112">An Azure AD subscription</span></span>
- <span data-ttu-id="11296-113">En PlanMyLeave enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="11296-113">A PlanMyLeave single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="11296-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="11296-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="11296-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="11296-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="11296-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="11296-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="11296-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion av en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="11296-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="11296-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="11296-118">Scenario description</span></span>
<span data-ttu-id="11296-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="11296-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="11296-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="11296-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="11296-121">Att lägga till PlanMyLeave från galleriet</span><span class="sxs-lookup"><span data-stu-id="11296-121">Adding PlanMyLeave from the gallery</span></span>
2. <span data-ttu-id="11296-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11296-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-planmyleave-from-the-gallery"></a><span data-ttu-id="11296-123">Att lägga till PlanMyLeave från galleriet</span><span class="sxs-lookup"><span data-stu-id="11296-123">Adding PlanMyLeave from the gallery</span></span>
<span data-ttu-id="11296-124">Du måste lägga till PlanMyLeave från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av PlanMyLeave i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="11296-124">To configure the integration of PlanMyLeave into Azure AD, you need to add PlanMyLeave from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="11296-125">**Utför följande steg för att lägga till PlanMyLeave från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="11296-125">**To add PlanMyLeave from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="11296-126">I den  **[Azure-hanteringsportalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="11296-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="11296-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="11296-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="11296-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="11296-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="11296-131">Klicka på **Lägg till** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11296-131">Click **Add** button on the top of the dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="11296-133">I sökrutan skriver **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="11296-133">In the search box, type **PlanMyLeave**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. <span data-ttu-id="11296-135">Välj i resultatpanelen **PlanMyLeave**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="11296-135">In the results panel, select **PlanMyLeave**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="11296-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11296-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="11296-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med PlanMyLeave baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="11296-138">In this section, you configure and test Azure AD single sign-on with PlanMyLeave based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="11296-139">Azure AD måste du känna till användaren i PlanMyLeave motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="11296-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PlanMyLeave is to a user in Azure AD.</span></span> <span data-ttu-id="11296-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i PlanMyLeave upprättas.</span><span class="sxs-lookup"><span data-stu-id="11296-140">In other words, a link relationship between an Azure AD user and the related user in PlanMyLeave needs to be established.</span></span>

<span data-ttu-id="11296-141">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="11296-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in PlanMyLeave.</span></span>

<span data-ttu-id="11296-142">Om du vill konfigurera och testa Azure AD enkel inloggning med PlanMyLeave, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="11296-142">To configure and test Azure AD single sign-on with PlanMyLeave, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="11296-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="11296-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="11296-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11296-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="11296-145">**[Skapa en testanvändare PlanMyLeave](#creating-a-planmyleave-test-user)**  – du har en motsvarighet för Britta Simon i PlanMyLeave som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="11296-145">**[Creating a PlanMyLeave test user](#creating-a-planmyleave-test-user)** - to have a counterpart of Britta Simon in PlanMyLeave that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="11296-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="11296-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="11296-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="11296-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="11296-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11296-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="11296-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-hanteringsportalen och konfigurera enkel inloggning i ditt PlanMyLeave program.</span><span class="sxs-lookup"><span data-stu-id="11296-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your PlanMyLeave application.</span></span>

<span data-ttu-id="11296-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med PlanMyLeave:**</span><span class="sxs-lookup"><span data-stu-id="11296-150">**To configure Azure AD single sign-on with PlanMyLeave, perform the following steps:**</span></span>

1. <span data-ttu-id="11296-151">I Azure-hanteringsportalen på den **PlanMyLeave** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="11296-151">In the Azure Management portal, on the **PlanMyLeave** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="11296-153">På den **enkel inloggning** dialogrutan sida som **läge** Välj **SAML-baserade inloggning** att aktivera enkel inloggning på.</span><span class="sxs-lookup"><span data-stu-id="11296-153">On the **Single sign-on** dialog page, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. <span data-ttu-id="11296-155">På den **PlanMyLeave domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="11296-155">On the **PlanMyLeave Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    <span data-ttu-id="11296-157">a.</span><span class="sxs-lookup"><span data-stu-id="11296-157">a.</span></span> <span data-ttu-id="11296-158">I den **logga URL** textruta Skriv en URL med följande mönster:`https://<company-name>.planmyleave.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="11296-158">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<company-name>.planmyleave.com/Login.aspx`</span></span>
    
    <span data-ttu-id="11296-159">b.</span><span class="sxs-lookup"><span data-stu-id="11296-159">b.</span></span> <span data-ttu-id="11296-160">I den **Identiferare** textruta Skriv en URL med följande mönster:`https://<company-name>.planmyleave.com`</span><span class="sxs-lookup"><span data-stu-id="11296-160">In the **Identifer** textbox, type a URL using the following pattern: `https://<company-name>.planmyleave.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="11296-161">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="11296-161">Please note that these are not the real values.</span></span> <span data-ttu-id="11296-162">Du måste uppdatera dessa värden med den faktiska logga URL och identifierare.</span><span class="sxs-lookup"><span data-stu-id="11296-162">You have to update these values with the actual Sign On URL and Identifier.</span></span> <span data-ttu-id="11296-163">Kontakta [PlanMyLeave supportteamet](mailto:support@planmyleave.com) att hämta dessa värden.</span><span class="sxs-lookup"><span data-stu-id="11296-163">Contact [PlanMyLeave support team](mailto:support@planmyleave.com) to get these values.</span></span>

4. <span data-ttu-id="11296-164">På den **SAML-signeringscertifikat** klickar du på **Skapa nytt certifikat**.</span><span class="sxs-lookup"><span data-stu-id="11296-164">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. <span data-ttu-id="11296-166">På den **skapa nya certifikat** dialogrutan, klicka på kalenderikonen och välj en **förfallodatum**.</span><span class="sxs-lookup"><span data-stu-id="11296-166">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="11296-167">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="11296-167">Then click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="11296-169">På den **SAML-signeringscertifikat** väljer **aktivera nya certifikatet** och på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="11296-169">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. <span data-ttu-id="11296-171">På popup-fönstret **förnyelsecertifikat** -fönstret klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="11296-171">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="11296-173">På den **SAML-signeringscertifikat** klickar du på **certifikat (base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="11296-173">On the **SAML Signing Certificate** section, click **Certificate (base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. <span data-ttu-id="11296-175">På den **PlanMyLeave Configuration** klickar du på **konfigurera PlanMyLeave** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="11296-175">On the **PlanMyLeave Configuration** section, click **Configure PlanMyLeave** to open **Configure sign-on** window.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. <span data-ttu-id="11296-178">Logga in på din PlanMyLeave klient som en administratör i en annan webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="11296-178">In a different web browser window, log into your PlanMyLeave tenant as an administrator.</span></span>

11. <span data-ttu-id="11296-179">Gå till **systeminställningarna**.</span><span class="sxs-lookup"><span data-stu-id="11296-179">Go to **System Setup**.</span></span> <span data-ttu-id="11296-180">På den **säkerhetshantering** Klicka på avsnittet **företagets SAML inställningar** .</span><span class="sxs-lookup"><span data-stu-id="11296-180">Then on the **Security Management** section click **Company SAML settings** .</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. <span data-ttu-id="11296-182">På den **SAML inställningar** klickar du på ikonen för redigeraren.</span><span class="sxs-lookup"><span data-stu-id="11296-182">On the **SAML Settings** section, click editor icon.</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. <span data-ttu-id="11296-184">På den **SAML uppdateringsinställningar** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="11296-184">On the **Update SAML Settings** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning på App-sida](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    <span data-ttu-id="11296-186">a.</span><span class="sxs-lookup"><span data-stu-id="11296-186">a.</span></span>  <span data-ttu-id="11296-187">I den **inloggnings-URL** textruta, ange värdet för **SAML inloggning tjänst-URL för enkel** från Azure AD-konfigurationsfönstret.</span><span class="sxs-lookup"><span data-stu-id="11296-187">In the **Login URL** textbox, put the value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="11296-188">b.</span><span class="sxs-lookup"><span data-stu-id="11296-188">b.</span></span>  <span data-ttu-id="11296-189">Öppna filen hämtat certifikat i anteckningar, kopiera endast innehållet mellan---börjar certifikat--- och---slut certifikatet---för den till Urklipp, och klistra in det till den **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="11296-189">Open your downloaded certificate file in notepad, copy only the content between the ---Begin Certificate--- and ---End certificate---- of it into your clipboard, and then paste it to the **Certificate** textbox.</span></span>

    <span data-ttu-id="11296-190">c.</span><span class="sxs-lookup"><span data-stu-id="11296-190">c.</span></span> <span data-ttu-id="11296-191">Ange ”**har aktiverats**” till ”**Ja**”.</span><span class="sxs-lookup"><span data-stu-id="11296-191">Set "**Is Enable**" to "**Yes**".</span></span>

    <span data-ttu-id="11296-192">d.</span><span class="sxs-lookup"><span data-stu-id="11296-192">d.</span></span> <span data-ttu-id="11296-193">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="11296-193">Click **Save**.</span></span>



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="11296-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="11296-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="11296-195">Syftet med det här avsnittet är att skapa en testanvändare i Azure Management portal kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="11296-195">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="11296-197">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="11296-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="11296-198">I den **Azure-hanteringsportalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="11296-198">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="11296-200">Gå till **användare och grupper** och på **alla användare** att visa en lista över användare.</span><span class="sxs-lookup"><span data-stu-id="11296-200">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="11296-202">Klicka på överst i dialogrutan **Lägg till** att öppna den **användaren** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11296-202">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="11296-204">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="11296-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="11296-206">a.</span><span class="sxs-lookup"><span data-stu-id="11296-206">a.</span></span> <span data-ttu-id="11296-207">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="11296-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="11296-208">b.</span><span class="sxs-lookup"><span data-stu-id="11296-208">b.</span></span> <span data-ttu-id="11296-209">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="11296-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="11296-210">c.</span><span class="sxs-lookup"><span data-stu-id="11296-210">c.</span></span> <span data-ttu-id="11296-211">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="11296-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="11296-212">d.</span><span class="sxs-lookup"><span data-stu-id="11296-212">d.</span></span> <span data-ttu-id="11296-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="11296-213">Click **Create**.</span></span> 



### <a name="creating-a-planmyleave-test-user"></a><span data-ttu-id="11296-214">Skapa en testanvändare PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="11296-214">Creating a PlanMyLeave test user</span></span>

<span data-ttu-id="11296-215">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="11296-215">The objective of this section is to create a user called Britta Simon in PlanMyLeave.</span></span> <span data-ttu-id="11296-216">PlanMyLeave stöder just-in-time-etablering, vilket är aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="11296-216">PlanMyLeave supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="11296-217">Det finns ingen åtgärd objekt i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11296-217">There is no action item for you in this section.</span></span> <span data-ttu-id="11296-218">En ny användare skapas vid ett försök att komma åt PlanMyLeave om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="11296-218">A new user will be created during an attempt to access PlanMyLeave if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="11296-219">Om du behöver skapa en användare manuellt måste du kontakta [PlanMyLeave supportteamet](mailto:support@planmyleave.com).</span><span class="sxs-lookup"><span data-stu-id="11296-219">If you need to create an user manually, you need to contact [PlanMyLeave support team](mailto:support@planmyleave.com).</span></span>



### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="11296-220">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="11296-220">Assigning the Azure AD test user</span></span>

<span data-ttu-id="11296-221">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="11296-221">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to PlanMyLeave.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="11296-223">**Om du vill tilldela PlanMyLeave Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="11296-223">**To assign Britta Simon to PlanMyLeave, perform the following steps:**</span></span>

1. <span data-ttu-id="11296-224">Öppna vyn program i Azure-hanteringsportalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="11296-224">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="11296-226">Välj i listan med program **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="11296-226">In the applications list, select **PlanMyLeave**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. <span data-ttu-id="11296-228">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="11296-228">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="11296-230">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="11296-230">Click **Add** button.</span></span> <span data-ttu-id="11296-231">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11296-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="11296-233">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="11296-233">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="11296-234">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11296-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="11296-235">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="11296-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="11296-236">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="11296-236">Testing single sign-on</span></span>

<span data-ttu-id="11296-237">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="11296-237">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="11296-238">När du klickar på panelen PlanMyLeave på åtkomstpanelen du bör få automatiskt loggat in på ditt PlanMyLeave program.</span><span class="sxs-lookup"><span data-stu-id="11296-238">When you click the PlanMyLeave tile in the Access Panel, you should get automatically signed-on to your PlanMyLeave application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="11296-239">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="11296-239">Additional resources</span></span>

* [<span data-ttu-id="11296-240">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="11296-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="11296-241">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="11296-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png