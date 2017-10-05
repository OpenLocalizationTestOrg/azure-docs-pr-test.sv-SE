---
title: "Självstudier: Azure Active Directory-integrering med SuccessFactors | Microsoft Docs"
description: "Lär dig hur du använder SuccessFactors med Azure Active Directory för att aktivera enkel inloggning, Automatisk etablering och mycket mer!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: e85a38ccbe25263ac42bc76351416b023fb77c87
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a><span data-ttu-id="61da2-103">Självstudier: Azure Active Directory-integrering med SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="61da2-103">Tutorial: Azure Active Directory integration with SuccessFactors</span></span>
<span data-ttu-id="61da2-104">Syftet med den här kursen är att visa dig hur du integrerar SuccessFactors med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="61da2-104">The objective of this tutorial is to show you how to integrate SuccessFactors with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="61da2-105">Integrera SuccessFactors med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="61da2-105">Integrating SuccessFactors with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="61da2-106">Du kan styra i Azure AD som har åtkomst till SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="61da2-106">You can control in Azure AD who has access to SuccessFactors</span></span>
* <span data-ttu-id="61da2-107">Du kan aktivera användarna att automatiskt hämta loggat in på SuccessFactors (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="61da2-107">You can enable your users to automatically get signed-on to SuccessFactors (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="61da2-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="61da2-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="61da2-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="61da2-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="61da2-110">Krav</span><span class="sxs-lookup"><span data-stu-id="61da2-110">Prerequisites</span></span>
<span data-ttu-id="61da2-111">För att konfigurera Azure AD-integrering med SuccessFactors, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="61da2-111">To configure Azure AD integration with SuccessFactors, you need the following items:</span></span>

* <span data-ttu-id="61da2-112">En giltig Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="61da2-112">A valid Azure subscription</span></span>
* <span data-ttu-id="61da2-113">En klient i SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="61da2-113">A tenant in SuccessFactors</span></span>

> [!NOTE]
> <span data-ttu-id="61da2-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="61da2-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="61da2-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="61da2-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="61da2-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="61da2-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="61da2-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="61da2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="61da2-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="61da2-118">Scenario description</span></span>
<span data-ttu-id="61da2-119">Syftet med den här kursen är att du ska testa Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="61da2-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="61da2-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="61da2-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="61da2-121">Att lägga till SuccessFactors från galleriet</span><span class="sxs-lookup"><span data-stu-id="61da2-121">Adding SuccessFactors from the gallery</span></span>
2. <span data-ttu-id="61da2-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61da2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-successfactors-from-the-gallery"></a><span data-ttu-id="61da2-123">Att lägga till SuccessFactors från galleriet</span><span class="sxs-lookup"><span data-stu-id="61da2-123">Adding SuccessFactors from the gallery</span></span>
<span data-ttu-id="61da2-124">Du måste lägga till SuccessFactors från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SuccessFactors i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="61da2-124">To configure the integration of SuccessFactors into Azure AD, you need to add SuccessFactors from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="61da2-125">**Utför följande steg för att lägga till SuccessFactors från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="61da2-125">**To add SuccessFactors from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="61da2-126">I den klassiska Azure-portalen på panelen vänstra navigeringsfönstret klickar du på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="61da2-126">In the Azure classic portal, on the left navigation panel, click **Active Directory**.</span></span>
   
    ![Konfigurera enkel inloggning][1]
2. <span data-ttu-id="61da2-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="61da2-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="61da2-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="61da2-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Konfigurera enkel inloggning][2]
4. <span data-ttu-id="61da2-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="61da2-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Program][3]
5. <span data-ttu-id="61da2-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="61da2-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Konfigurera enkel inloggning][4]
6. <span data-ttu-id="61da2-135">I den **sökrutan**, typen **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="61da2-135">In the **search box**, type **SuccessFactors**.</span></span>
   
    ![Konfigurera enkel inloggning][5]
7. <span data-ttu-id="61da2-137">Välj i resultatpanelen **SuccessFactors**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="61da2-137">In the results panel, select **SuccessFactors**, and then click **Complete** to add the application.</span></span>
   
    ![Konfigurera enkel inloggning][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="61da2-139">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61da2-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="61da2-140">Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Azure AD enkel inloggning med SuccessFactors baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="61da2-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with SuccessFactors based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="61da2-141">För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i SuccessFactors till en användare i Azure AD är okänt.</span><span class="sxs-lookup"><span data-stu-id="61da2-141">For single sign-on to work, Azure AD needs to know what the counterpart user in SuccessFactors to an user in Azure AD is.</span></span> <span data-ttu-id="61da2-142">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SuccessFactors upprättas.</span><span class="sxs-lookup"><span data-stu-id="61da2-142">In other words, a link relationship between an Azure AD user and the related user in SuccessFactors needs to be established.</span></span>

<span data-ttu-id="61da2-143">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="61da2-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SuccessFactors.</span></span>

<span data-ttu-id="61da2-144">Om du vill konfigurera och testa Azure AD enkel inloggning med SuccessFactors, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="61da2-144">To configure and test Azure AD single sign-on with SuccessFactors, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="61da2-145">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="61da2-145">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="61da2-146">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="61da2-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="61da2-147">**[Skapa en testanvändare SuccessFactors](#creating-a-successfactors-test-user)**  – du har en motsvarighet för Britta Simon i SuccessFactors som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="61da2-147">**[Creating a SuccessFactors test user](#creating-a-successfactors-test-user)** - to have a counterpart of Britta Simon in SuccessFactors that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="61da2-148">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="61da2-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="61da2-149">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="61da2-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="61da2-150">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61da2-150">Configuring Azure AD single sign-on</span></span>
<span data-ttu-id="61da2-151">I det här avsnittet Aktivera Azure AD enkel inloggning i den klassiska portalen och konfigurera enkel inloggning i ditt SuccessFactors program.</span><span class="sxs-lookup"><span data-stu-id="61da2-151">In this section, you enable Azure AD single sign-on in the classic portal and configure single sign-on in your SuccessFactors application.</span></span>

<span data-ttu-id="61da2-152">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SuccessFactors:**</span><span class="sxs-lookup"><span data-stu-id="61da2-152">**To configure Azure AD single sign-on with SuccessFactors, perform the following steps:**</span></span>

1. <span data-ttu-id="61da2-153">I den klassiska Azure-portalen på den **SuccessFactors** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="61da2-153">In the Azure classic portal, on the **SuccessFactors** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    ![Konfigurera enkel inloggning][7]
2. <span data-ttu-id="61da2-155">På den **hur vill du att användarna kan logga in på SuccessFactors** väljer **Microsoft Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="61da2-155">On the **How would you like users to sign on to SuccessFactors** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Konfigurera enkel inloggning][8]
3. <span data-ttu-id="61da2-157">På den **konfigurera App-URL** , utför följande steg, och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="61da2-157">On the **Configure App URL** page, perform the following steps, and then click **Next**.</span></span>
   
    ![Konfigurera enkel inloggning][9]
   
    <span data-ttu-id="61da2-159">a.</span><span class="sxs-lookup"><span data-stu-id="61da2-159">a.</span></span> <span data-ttu-id="61da2-160">I den **logga URL** textruta, ange ett URL-Adressen med något av följande mönster:</span><span class="sxs-lookup"><span data-stu-id="61da2-160">In the **Sign On URL** textbox, type a URL using one of the following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
   
    <span data-ttu-id="61da2-161">b.</span><span class="sxs-lookup"><span data-stu-id="61da2-161">b.</span></span> <span data-ttu-id="61da2-162">I den **Reply URL** textruta, ange ett URL-Adressen med något av följande mönster:</span><span class="sxs-lookup"><span data-stu-id="61da2-162">In the **Reply URL** textbox, type a URL using one of the following patterns:</span></span> 
   
    |  |
    | --- |
    | `https://<company name>.successfactors.com/<company name>` |
    | `https://<company name>.sapsf.com/<company name>` |
    | `https://<company name>.successfactors.eu/<company name>` |
    | `https://<company name>.sapsf.eu` |
    | `https://<company name>.sapsf.eu/<company name>` |
   
    <span data-ttu-id="61da2-163">c.</span><span class="sxs-lookup"><span data-stu-id="61da2-163">c.</span></span> <span data-ttu-id="61da2-164">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="61da2-164">Click **Next**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="61da2-165">Observera att detta inte är verkliga värden.</span><span class="sxs-lookup"><span data-stu-id="61da2-165">Please note that these are not the real values.</span></span> <span data-ttu-id="61da2-166">Du måste uppdatera dessa värden med de faktiska logga URL och Reply-URL.</span><span class="sxs-lookup"><span data-stu-id="61da2-166">You have to update these values with the actual Sign On URL and Reply URL.</span></span> <span data-ttu-id="61da2-167">För att få dessa värden kan kontakta [SuccessFactors supportteam](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="61da2-167">To get these values, contact [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

1. <span data-ttu-id="61da2-168">På den **Konfigurera enkel inloggning på SuccessFactors** klickar du på **hämta certifikat**, och sedan spara certifikatfilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="61da2-168">On the **Configure single sign-on at SuccessFactors** page, click **Download certificate**, and then save the certificate file locally on your computer.</span></span>
   
    ![Konfigurera enkel inloggning][10]

2. <span data-ttu-id="61da2-170">Logga in i ett annat webbläsarfönster din **SuccessFactors administrationsportalen** som administratör.</span><span class="sxs-lookup"><span data-stu-id="61da2-170">In a different web browser window, log into your **SuccessFactors admin portal** as an administrator.</span></span>

3. <span data-ttu-id="61da2-171">Besök **programsäkerhet** och inbyggd **enkel inloggning på funktionen**.</span><span class="sxs-lookup"><span data-stu-id="61da2-171">Visit **Application Security** and native to **Single Sign On Feature**.</span></span> 

4. <span data-ttu-id="61da2-172">Placera alla värden i den **återställa Token** och på **spara Token** att aktivera SAML SSO.</span><span class="sxs-lookup"><span data-stu-id="61da2-172">Place any value in the **Reset Token** and click **Save Token** to enable SAML SSO.</span></span>
   
    ![Konfigurera enkel inloggning på app-sida][11]

    > [!NOTE] 
    > <span data-ttu-id="61da2-174">Det här värdet används bara som växeln på/av.</span><span class="sxs-lookup"><span data-stu-id="61da2-174">This value is just used as the on/off switch.</span></span> <span data-ttu-id="61da2-175">Om inget värde har sparats är SAML SSO Aktiverat.</span><span class="sxs-lookup"><span data-stu-id="61da2-175">If any value is saved, the SAML SSO is ON.</span></span> <span data-ttu-id="61da2-176">Om ett tomt värde har sparats är SAML SSO OFF.</span><span class="sxs-lookup"><span data-stu-id="61da2-176">If a blank value is saved the SAML SSO is OFF.</span></span>

1. <span data-ttu-id="61da2-177">Inbyggd nedan skärmbild och utför följande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="61da2-177">Native to below screenshot and perform the following actions.</span></span>
   
    ![Konfigurera enkel inloggning på app-sida][12]
   
    <span data-ttu-id="61da2-179">a.</span><span class="sxs-lookup"><span data-stu-id="61da2-179">a.</span></span> <span data-ttu-id="61da2-180">Välj den **SAML v2 SSO** alternativknapp</span><span class="sxs-lookup"><span data-stu-id="61da2-180">Select the **SAML v2 SSO** Radio Button</span></span>
   
    <span data-ttu-id="61da2-181">b.</span><span class="sxs-lookup"><span data-stu-id="61da2-181">b.</span></span> <span data-ttu-id="61da2-182">Ange SAML garanterar part Name(e.g. SAml issuer + company name).</span><span class="sxs-lookup"><span data-stu-id="61da2-182">Set the SAML Asserting Party Name(e.g. SAml issuer + company name).</span></span>
   
    <span data-ttu-id="61da2-183">c.</span><span class="sxs-lookup"><span data-stu-id="61da2-183">c.</span></span> <span data-ttu-id="61da2-184">I den **SAML utfärdaren** textruta ange värdet för **utfärdar-URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="61da2-184">In the **SAML Issuer** textbox put the value of **Issuer URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="61da2-185">d.</span><span class="sxs-lookup"><span data-stu-id="61da2-185">d.</span></span> <span data-ttu-id="61da2-186">Välj **svar (kunden genereras/IdP/AP)** som **kräver obligatorisk signatur**.</span><span class="sxs-lookup"><span data-stu-id="61da2-186">Select **Response(Customer Generated/IdP/AP)** as **Require Mandatory Signature**.</span></span>
   
    <span data-ttu-id="61da2-187">e.</span><span class="sxs-lookup"><span data-stu-id="61da2-187">e.</span></span> <span data-ttu-id="61da2-188">Välj **aktiverat** som **aktivera SAML-flaggan**.</span><span class="sxs-lookup"><span data-stu-id="61da2-188">Select **Enabled** as **Enable SAML Flag**.</span></span>
   
    <span data-ttu-id="61da2-189">f.</span><span class="sxs-lookup"><span data-stu-id="61da2-189">f.</span></span> <span data-ttu-id="61da2-190">Välj **nr** som **signatur på förfrågan inloggning (SA genereras/SP/RP)**.</span><span class="sxs-lookup"><span data-stu-id="61da2-190">Select **No** as **Login Request Signature(SF Generated/SP/RP)**.</span></span>
   
    <span data-ttu-id="61da2-191">g.</span><span class="sxs-lookup"><span data-stu-id="61da2-191">g.</span></span> <span data-ttu-id="61da2-192">Välj **webbläsare/Post-profilen** som **SAML profil**.</span><span class="sxs-lookup"><span data-stu-id="61da2-192">Select **Browser/Post Profile** as **SAML Profile**.</span></span>
   
    <span data-ttu-id="61da2-193">h.</span><span class="sxs-lookup"><span data-stu-id="61da2-193">h.</span></span> <span data-ttu-id="61da2-194">Välj **nr** som **genomdriva giltighetsperioden för certifikatet**.</span><span class="sxs-lookup"><span data-stu-id="61da2-194">Select **No** as **Enforce Certificate Valid Period**.</span></span>
   
    <span data-ttu-id="61da2-195">Jag.</span><span class="sxs-lookup"><span data-stu-id="61da2-195">i.</span></span> <span data-ttu-id="61da2-196">Kopiera innehållet i filen hämtat certifikat och klistrar in det i den **SAML verifiera certifikatet** textruta.</span><span class="sxs-lookup"><span data-stu-id="61da2-196">Copy the content of the downloaded certificate file, and then paste it into the **SAML Verifying Certificate** textbox.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="61da2-197">Certifikatets innehåll måste börja certifikat och certifikat sluttaggar.</span><span class="sxs-lookup"><span data-stu-id="61da2-197">The certificate content must have begin certificate and end certificate tags.</span></span>

1. <span data-ttu-id="61da2-198">Gå till SAML V2 och utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="61da2-198">Navigate to SAML V2, and then perform the following steps:</span></span>
   
    ![Konfigurera enkel inloggning på app-sida][13]
   
    <span data-ttu-id="61da2-200">a.</span><span class="sxs-lookup"><span data-stu-id="61da2-200">a.</span></span> <span data-ttu-id="61da2-201">Välj **Ja** som **stöder SP-initierad globala logga ut**.</span><span class="sxs-lookup"><span data-stu-id="61da2-201">Select **Yes** as **Support SP-initiated Global Logout**.</span></span>
   
    <span data-ttu-id="61da2-202">b.</span><span class="sxs-lookup"><span data-stu-id="61da2-202">b.</span></span> <span data-ttu-id="61da2-203">I den **globala logga ut tjänst-URL (LogoutRequest mål)** textruta ange värdet för **Remote logga ut URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="61da2-203">In the **Global Logout Service URL (LogoutRequest destination)** textbox put the value of **Remote Logout URL** from Azure AD application configuration wizard.</span></span>
   
    <span data-ttu-id="61da2-204">c.</span><span class="sxs-lookup"><span data-stu-id="61da2-204">c.</span></span> <span data-ttu-id="61da2-205">Välj **nr** som **kräver sp måste kryptera alla NameID elementet**.</span><span class="sxs-lookup"><span data-stu-id="61da2-205">Select **No** as **Require sp must encrypt all NameID element**.</span></span>
   
    <span data-ttu-id="61da2-206">d.</span><span class="sxs-lookup"><span data-stu-id="61da2-206">d.</span></span> <span data-ttu-id="61da2-207">Välj **Ospecificerad** som **NameID Format**.</span><span class="sxs-lookup"><span data-stu-id="61da2-207">Select **unspecified** as **NameID Format**.</span></span>
   
    <span data-ttu-id="61da2-208">e.</span><span class="sxs-lookup"><span data-stu-id="61da2-208">e.</span></span> <span data-ttu-id="61da2-209">Välj **Ja** som **aktivera sp initierade inloggning (AuthnRequest)**.</span><span class="sxs-lookup"><span data-stu-id="61da2-209">Select **Yes** as **Enable sp initiated login (AuthnRequest)**.</span></span>
   
    <span data-ttu-id="61da2-210">f.</span><span class="sxs-lookup"><span data-stu-id="61da2-210">f.</span></span> <span data-ttu-id="61da2-211">I den **begäran om att skicka som företagsomfattande utgivaren** textruta ange värdet för **Remote inloggnings-URL** från guiden Konfigurera program för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="61da2-211">In the **Send request as Company-Wide issuer** textbox put the value of **Remote Login URL** from Azure AD application configuration wizard.</span></span>
2. <span data-ttu-id="61da2-212">Utför de här stegen om du vill göra inloggningen användarnamn inte skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="61da2-212">Perform these steps if you want to make the login usernames Case Insensitive, .</span></span>
   
    <span data-ttu-id="61da2-213">a.</span><span class="sxs-lookup"><span data-stu-id="61da2-213">a.</span></span> <span data-ttu-id="61da2-214">Besök **Företagsinställningar**(nästan längst ned).</span><span class="sxs-lookup"><span data-stu-id="61da2-214">Visit **Company Settings**(near the bottom).</span></span>
   
    <span data-ttu-id="61da2-215">b.</span><span class="sxs-lookup"><span data-stu-id="61da2-215">b.</span></span> <span data-ttu-id="61da2-216">Markera kryssrutan bredvid **aktivera icke skiftlägeskänslig användarnamn**.</span><span class="sxs-lookup"><span data-stu-id="61da2-216">select checkbox near **Enable Non-Case-Sensitive Username**.</span></span>
   
    <span data-ttu-id="61da2-217">c.Click **spara**.</span><span class="sxs-lookup"><span data-stu-id="61da2-217">c.Click **Save**.</span></span>
   
    ![Konfigurera enkel inloggning][29]

    > [!NOTE] 
    > <span data-ttu-id="61da2-219">Om du försöker göra detta kontrollerar systemet om det skapar en dubblett SAML-inloggningsnamn.</span><span class="sxs-lookup"><span data-stu-id="61da2-219">If you try to enable this, the system checks if it will create a duplicate SAML login name.</span></span> <span data-ttu-id="61da2-220">Till exempel om kunden har användarnamn Användare1 och Användare1.</span><span class="sxs-lookup"><span data-stu-id="61da2-220">For example if the customer has usernames User1 and user1.</span></span> <span data-ttu-id="61da2-221">Tar bort skiftlägeskänslighet gör dessa dubbletter.</span><span class="sxs-lookup"><span data-stu-id="61da2-221">Taking away case sensitivity makes these duplicates.</span></span> <span data-ttu-id="61da2-222">Systemet får du ett felmeddelande och kommer inte att aktivera funktionen.</span><span class="sxs-lookup"><span data-stu-id="61da2-222">The system will give you an error message and will not enable the feature.</span></span> <span data-ttu-id="61da2-223">Kunden måste ändra något av användarnamnet så att det är faktiskt stavat olika.</span><span class="sxs-lookup"><span data-stu-id="61da2-223">The customer will need to change one of the usernames so it’s actually spelled different.</span></span> 

1. <span data-ttu-id="61da2-224">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **Slutför** att stänga den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="61da2-224">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
    ![Program][14]
2. <span data-ttu-id="61da2-226">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="61da2-226">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    ![Program][15]

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="61da2-228">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="61da2-228">Creating an Azure AD test user</span></span>
<span data-ttu-id="61da2-229">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="61da2-229">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][16]

<span data-ttu-id="61da2-231">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="61da2-231">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="61da2-232">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="61da2-232">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][17]
2. <span data-ttu-id="61da2-234">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="61da2-234">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="61da2-235">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="61da2-235">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][18]
4. <span data-ttu-id="61da2-237">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="61da2-237">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][19]
5. <span data-ttu-id="61da2-239">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="61da2-239">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD][20]
   
    <span data-ttu-id="61da2-241">a.</span><span class="sxs-lookup"><span data-stu-id="61da2-241">a.</span></span> <span data-ttu-id="61da2-242">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="61da2-242">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="61da2-243">b.</span><span class="sxs-lookup"><span data-stu-id="61da2-243">b.</span></span> <span data-ttu-id="61da2-244">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="61da2-244">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="61da2-245">c.</span><span class="sxs-lookup"><span data-stu-id="61da2-245">c.</span></span> <span data-ttu-id="61da2-246">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="61da2-246">Click **Next**.</span></span>
6. <span data-ttu-id="61da2-247">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="61da2-247">On the **User Profile** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD][21]
   
    <span data-ttu-id="61da2-249">a.</span><span class="sxs-lookup"><span data-stu-id="61da2-249">a.</span></span> <span data-ttu-id="61da2-250">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="61da2-250">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="61da2-251">b.</span><span class="sxs-lookup"><span data-stu-id="61da2-251">b.</span></span> <span data-ttu-id="61da2-252">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="61da2-252">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="61da2-253">c.</span><span class="sxs-lookup"><span data-stu-id="61da2-253">c.</span></span> <span data-ttu-id="61da2-254">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="61da2-254">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="61da2-255">d.</span><span class="sxs-lookup"><span data-stu-id="61da2-255">d.</span></span> <span data-ttu-id="61da2-256">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="61da2-256">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="61da2-257">e.</span><span class="sxs-lookup"><span data-stu-id="61da2-257">e.</span></span> <span data-ttu-id="61da2-258">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="61da2-258">Click **Next**.</span></span>
7. <span data-ttu-id="61da2-259">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="61da2-259">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD][22]
8. <span data-ttu-id="61da2-261">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="61da2-261">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD][23]
   
    <span data-ttu-id="61da2-263">a.</span><span class="sxs-lookup"><span data-stu-id="61da2-263">a.</span></span> <span data-ttu-id="61da2-264">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="61da2-264">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="61da2-265">b.</span><span class="sxs-lookup"><span data-stu-id="61da2-265">b.</span></span> <span data-ttu-id="61da2-266">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="61da2-266">Click **Complete**.</span></span>  

### <a name="creating-a-successfactors-test-user"></a><span data-ttu-id="61da2-267">Skapa en testanvändare SuccessFactors</span><span class="sxs-lookup"><span data-stu-id="61da2-267">Creating a SuccessFactors test user</span></span>
<span data-ttu-id="61da2-268">För att aktivera Azure AD-användare att logga in på SuccessFactors etableras de i SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="61da2-268">In order to enable Azure AD users to log into SuccessFactors, they must be provisioned into SuccessFactors.</span></span>  
<span data-ttu-id="61da2-269">När det gäller SuccessFactors är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="61da2-269">In the case of SuccessFactors, provisioning is a manual task.</span></span>

<span data-ttu-id="61da2-270">För att få användare som skapats i SuccessFactors, måste du kontakta den [SuccessFactors supportteam](https://www.successfactors.com/en_us/support.html).</span><span class="sxs-lookup"><span data-stu-id="61da2-270">To get users created in SuccessFactors, you need to contact the [SuccessFactors support team](https://www.successfactors.com/en_us/support.html).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="61da2-271">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="61da2-271">Assigning the Azure AD test user</span></span>
<span data-ttu-id="61da2-272">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till SuccessFactors.</span><span class="sxs-lookup"><span data-stu-id="61da2-272">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to SuccessFactors.</span></span>

![Tilldela användare][24]

<span data-ttu-id="61da2-274">**Om du vill tilldela SuccessFactors Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="61da2-274">**To assign Britta Simon to SuccessFactors, perform the following steps:**</span></span>

1. <span data-ttu-id="61da2-275">På den klassiska portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="61da2-275">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Tilldela användare][25]
2. <span data-ttu-id="61da2-277">Välj i listan med program **SuccessFactors**.</span><span class="sxs-lookup"><span data-stu-id="61da2-277">In the applications list, select **SuccessFactors**.</span></span>
   
    ![Konfigurera enkel inloggning][26]
3. <span data-ttu-id="61da2-279">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="61da2-279">In the menu on the top, click **Users**.</span></span>
   
    ![Tilldela användare][27]
4. <span data-ttu-id="61da2-281">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="61da2-281">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="61da2-282">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="61da2-282">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][28]

### <a name="testing-single-sign-on"></a><span data-ttu-id="61da2-284">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="61da2-284">Testing single sign-on</span></span>
<span data-ttu-id="61da2-285">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="61da2-285">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="61da2-286">När du klickar på panelen SuccessFactors på åtkomstpanelen du bör få automatiskt loggat in på ditt SuccessFactors program.</span><span class="sxs-lookup"><span data-stu-id="61da2-286">When you click the SuccessFactors tile in the Access Panel, you should get automatically signed-on to your SuccessFactors application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="61da2-287">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="61da2-287">Additional resources</span></span>
* [<span data-ttu-id="61da2-288">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="61da2-288">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="61da2-289">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="61da2-289">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_00.png
[1]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_01.png
[6]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_02.png
[7]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_03.png
[8]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_04.png
[9]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_05.png
[10]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_06.png

[11]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_09.png
[14]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_05.png
[15]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_06.png

[16]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_00.png
[17]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_01.png
[18]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_02.png
[19]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_03.png
[20]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_04.png
[21]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_05.png
[22]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_06.png
[23]: ./media/active-directory-saas-successfactors-tutorial/create_aaduser_07.png

[24]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_07.png
[25]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_08.png
[26]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_11.png
[27]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_09.png
[28]: ./media/active-directory-saas-successfactors-tutorial/tutorial_general_10.png
[29]: ./media/active-directory-saas-successfactors-tutorial/tutorial_successfactors_10.png
