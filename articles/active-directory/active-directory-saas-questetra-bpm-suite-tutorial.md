---
title: "Självstudier: Azure Active Directory-integrering med Questetra BPM Suite | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Questetra BPM Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: jeedes
ms.openlocfilehash: 7ae75446c9d19ce15a82caa9604658a528ab9941
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a><span data-ttu-id="b296b-103">Självstudier: Azure Active Directory-integrering med Questetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="b296b-103">Tutorial: Azure Active Directory integration with Questetra BPM Suite</span></span>
<span data-ttu-id="b296b-104">Syftet med den här kursen är att visa dig hur du integrerar Questetra BPM Suite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b296b-104">The objective of this tutorial is to show you how to integrate Questetra BPM Suite with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="b296b-105">Integrera Questetra BPM Suite med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b296b-105">Integrating Questetra BPM Suite with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="b296b-106">Du kan styra i Azure AD som har åtkomst till Questetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="b296b-106">You can control in Azure AD who has access to Questetra BPM Suite</span></span> 
* <span data-ttu-id="b296b-107">Du kan aktivera användarna att automatiskt hämta loggat in på Questetra BPM Suite (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b296b-107">You can enable your users to automatically get signed-on to Questetra BPM Suite (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="b296b-108">Du kan hantera dina konton i en central plats – den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b296b-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="b296b-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b296b-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b296b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="b296b-110">Prerequisites</span></span>
<span data-ttu-id="b296b-111">För att konfigurera Azure AD-integrering med Questetra BPM Suite, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="b296b-111">To configure Azure AD integration with Questetra BPM Suite, you need the following items:</span></span>

* <span data-ttu-id="b296b-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b296b-112">An Azure AD subscription</span></span>
* <span data-ttu-id="b296b-113">En [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="b296b-113">An [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b296b-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b296b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="b296b-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b296b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="b296b-116">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b296b-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="b296b-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b296b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="b296b-118">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="b296b-118">Scenario Description</span></span>
<span data-ttu-id="b296b-119">Syftet med den här kursen är att du ska testa Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b296b-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="b296b-120">Det scenario som beskrivs i den här kursen består av tre huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b296b-120">The scenario outlined in this tutorial consists of three main building blocks:</span></span>

1. <span data-ttu-id="b296b-121">Att lägga till Questetra BPM Suite från galleriet</span><span class="sxs-lookup"><span data-stu-id="b296b-121">Adding Questetra BPM Suite from the gallery</span></span> 
2. <span data-ttu-id="b296b-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b296b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-questetra-bpm-suite-from-the-gallery"></a><span data-ttu-id="b296b-123">Att lägga till Questetra BPM Suite från galleriet</span><span class="sxs-lookup"><span data-stu-id="b296b-123">Adding Questetra BPM Suite from the gallery</span></span>
<span data-ttu-id="b296b-124">Du måste lägga till Questetra BPM Suite från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Questetra BPM Suite i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b296b-124">To configure the integration of Questetra BPM Suite into Azure AD, you need to add Questetra BPM Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b296b-125">**Utför följande steg för att lägga till Questetra BPM Suite från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="b296b-125">**To add Questetra BPM Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b296b-126">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b296b-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="b296b-128">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="b296b-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="b296b-129">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="b296b-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Program][2]

4. <span data-ttu-id="b296b-131">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="b296b-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Program][3]

5. <span data-ttu-id="b296b-133">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="b296b-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Program][4]

6. <span data-ttu-id="b296b-135">I sökrutan skriver **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="b296b-135">In the search box, type **Questetra BPM Suite**.</span></span>
   
    ![Program][5]

7. <span data-ttu-id="b296b-137">I resultatfönstret, Välj **Questetra BPM Suite**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="b296b-137">In the results pane, select **Questetra BPM Suite**, and then click **Complete** to add the application.</span></span>

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b296b-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b296b-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b296b-139">Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Azure AD enkel inloggning med Questetra BPM Suite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b296b-139">The objective of this section is to show you how to configure and test Azure AD single sign-on with Questetra BPM Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b296b-140">För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i Questetra BPM Suite till en användare i Azure AD är okänt.</span><span class="sxs-lookup"><span data-stu-id="b296b-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Questetra BPM Suite to an user in Azure AD is.</span></span> <span data-ttu-id="b296b-141">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i Questetra BPM Suite upprättas.</span><span class="sxs-lookup"><span data-stu-id="b296b-141">In other words, a link relationship between an Azure AD user and the related user in Questetra BPM Suite needs to be established.</span></span>  
<span data-ttu-id="b296b-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i Questetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="b296b-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Questetra BPM Suite.</span></span>

<span data-ttu-id="b296b-143">Om du vill konfigurera och testa Azure AD enkel inloggning med Questetra BPM Suite, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b296b-143">To configure and test Azure AD single sign-on with Questetra BPM Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b296b-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b296b-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b296b-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b296b-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b296b-146">**[Skapa en testanvändare Questetra BPM Suite](#creating-a-questetra-bpm-suite-test-user)**  – har en motsvarighet för Britta Simon Questetra BPM Suite som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="b296b-146">**[Creating a Questetra BPM Suite test user](#creating-a-questetra-bpm-suite-test-user)** - to have a counterpart of Britta Simon in Questetra BPM Suite that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="b296b-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b296b-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b296b-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b296b-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b296b-149">Konfigurera Azure AD-Single Sign-On</span><span class="sxs-lookup"><span data-stu-id="b296b-149">Configuring Azure AD Single Sign-On</span></span>
<span data-ttu-id="b296b-150">Syftet med det här avsnittet är att aktivera Azure AD enkel inloggning i den klassiska Azure-portalen och konfigurera enkel inloggning i ditt Questetra BPM Suite-program.</span><span class="sxs-lookup"><span data-stu-id="b296b-150">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your Questetra BPM Suite application.</span></span>

<span data-ttu-id="b296b-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Questetra BPM Suite:**</span><span class="sxs-lookup"><span data-stu-id="b296b-151">**To configure Azure AD single sign-on with Questetra BPM Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="b296b-152">I den klassiska Azure-portalen på den **Questetra BPM Suite** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b296b-152">In the Azure classic portal, on the **Questetra BPM Suite** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][8]

2. <span data-ttu-id="b296b-154">På den **hur vill du att användarna kan logga in på Questetra BPM Suite** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b296b-154">On the **How would you like users to sign on to Questetra BPM Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][9]

3. <span data-ttu-id="b296b-156">Logga in i ett annat webbläsarfönster din **Questetra BPM Suite** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b296b-156">In a different web browser window, log into your **Questetra BPM Suite** company site as an administrator.</span></span>

4. <span data-ttu-id="b296b-157">Klicka på menyn högst upp **systeminställningar**.</span><span class="sxs-lookup"><span data-stu-id="b296b-157">In the menu on the top, click **System Settings**.</span></span> 
   
    ![Azure AD-Single Sign-On][10]

5. <span data-ttu-id="b296b-159">Öppna den **SingleSignOnSAML** klickar du på **SSO (SAML)**.</span><span class="sxs-lookup"><span data-stu-id="b296b-159">To open the **SingleSignOnSAML** page, click **SSO (SAML)**.</span></span> 
   
    ![Azure AD-Single Sign-On][11]

6. <span data-ttu-id="b296b-161">I den klassiska Azure-portalen på den **konfigurera Appinställningar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b296b-161">In the Azure classic portal, on the **Configure App Settings** dialog page, perform the following steps:</span></span> 
   
    ![Konfigurera Appinställningar för][13]
   
    <span data-ttu-id="b296b-163">a.</span><span class="sxs-lookup"><span data-stu-id="b296b-163">a.</span></span> <span data-ttu-id="b296b-164">Om du **Questetra BPM Suite** företagets webbplats, i avsnittet SP Information, kopiera den **ACS URL**, och klistrar in det i den **logga URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b296b-164">On you **Questetra BPM Suite** company site, in the SP Information section, copy the **ACS URL**, and then paste it into the **Sign On URL** textbox.</span></span>
   
    <span data-ttu-id="b296b-165">b.</span><span class="sxs-lookup"><span data-stu-id="b296b-165">b.</span></span> <span data-ttu-id="b296b-166">Om du **Questetra BPM Suite** företagets webbplats, i avsnittet SP Information, kopiera den **enhets-ID**, och klistrar in det i den **utfärdar-URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b296b-166">On you **Questetra BPM Suite** company site, in the SP Information section, copy the **Entity ID**, and then paste it into the **Issuer URL** textbox.</span></span>
   
    <span data-ttu-id="b296b-167">c.</span><span class="sxs-lookup"><span data-stu-id="b296b-167">c.</span></span> <span data-ttu-id="b296b-168">Om du **Questetra BPM Suite** företagets webbplats, i avsnittet SP Information, kopiera den **ACS URL**, och klistrar in det i den **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b296b-168">On you **Questetra BPM Suite** company site, in the SP Information section, copy the **ACS URL**, and then paste it into the **Reply URL** textbox.</span></span>
   
    <span data-ttu-id="b296b-169">d.</span><span class="sxs-lookup"><span data-stu-id="b296b-169">d.</span></span> <span data-ttu-id="b296b-170">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b296b-170">Click **Next**.</span></span>

7. <span data-ttu-id="b296b-171">På den **Konfigurera enkel inloggning på Questetra BPM Suite** klickar du på **hämta certifikat**, och sedan spara certifikatfilen lokalt på datorn.</span><span class="sxs-lookup"><span data-stu-id="b296b-171">On the **Configure single sign-on at Questetra BPM Suite** page, click **Download certificate**, and then save the certificate file locally on your computer.</span></span>
   
    ![Konfigurera enkel inloggning][14]

8. <span data-ttu-id="b296b-173">Om du **Questetra BPM Suite** företagets webbplats, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b296b-173">On you **Questetra BPM Suite** company site, perform the following steps:</span></span> 
   
    ![Konfigurera enkel inloggning][15]
   
    <span data-ttu-id="b296b-175">a.</span><span class="sxs-lookup"><span data-stu-id="b296b-175">a.</span></span> <span data-ttu-id="b296b-176">Välj **aktivera enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="b296b-176">Select **Enable Single Sign-On**.</span></span>
   
    <span data-ttu-id="b296b-177">b.</span><span class="sxs-lookup"><span data-stu-id="b296b-177">b.</span></span> <span data-ttu-id="b296b-178">På den klassiska Azure-portalen, kopiera den **utfärdar-URL** värdet och klistrar in det i den **enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="b296b-178">On the Azure classic portal, copy the **Issuer URL** value, and then paste it into the **Entity ID** textbox.</span></span>
   
    <span data-ttu-id="b296b-179">c.</span><span class="sxs-lookup"><span data-stu-id="b296b-179">c.</span></span> <span data-ttu-id="b296b-180">På den klassiska Azure-portalen, kopiera den **inloggning tjänst-URL för enkel** värdet och klistrar in det i den **inloggning Sidadress** textruta.</span><span class="sxs-lookup"><span data-stu-id="b296b-180">On the Azure classic portal, copy the **Single Sign-On Service URL** value, and then paste it into the **Sign-in page URL** textbox.</span></span>
   
    <span data-ttu-id="b296b-181">d.</span><span class="sxs-lookup"><span data-stu-id="b296b-181">d.</span></span> <span data-ttu-id="b296b-182">På den klassiska Azure-portalen, kopiera den **tjänst-URL för enkel Sign-Out** värdet och klistrar in det i den **URL för utloggning** textruta.</span><span class="sxs-lookup"><span data-stu-id="b296b-182">On the Azure classic portal, copy the **Single Sign-Out Service URL** value, and then paste it into the **Sign-out page URL** textbox.</span></span>
   
    <span data-ttu-id="b296b-183">e.</span><span class="sxs-lookup"><span data-stu-id="b296b-183">e.</span></span> <span data-ttu-id="b296b-184">I den **NameID format** textruta typen **urn: oasis: namn: tc: SAML:1.1:nameid-format: e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="b296b-184">In the **NameID format** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="b296b-185">f.</span><span class="sxs-lookup"><span data-stu-id="b296b-185">f.</span></span> <span data-ttu-id="b296b-186">Skapa en Base64-kodade filen från din hämtat certifikat.</span><span class="sxs-lookup"><span data-stu-id="b296b-186">Create a base-64 encoded file from your downloaded certificate.</span></span> 

    >[!TIP] 
    ><span data-ttu-id="b296b-187">Mer information finns i [hur du konverterar ett binärt certifikat till en textfil](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="b296b-187">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>

    <span data-ttu-id="b296b-188">g.</span><span class="sxs-lookup"><span data-stu-id="b296b-188">g.</span></span> <span data-ttu-id="b296b-189">Öppna din Base64-kodade certifikatet i anteckningar, kopiera innehållet i den till Urklipp och klistrar in det i den **validering certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="b296b-189">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it into the **Validation certificate** textbox.</span></span> 

    <span data-ttu-id="b296b-190">h.</span><span class="sxs-lookup"><span data-stu-id="b296b-190">h.</span></span> <span data-ttu-id="b296b-191">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b296b-191">Click **Save**.</span></span>

1. <span data-ttu-id="b296b-192">Välj bekräftelsen konfiguration för enkel inloggning på den klassiska Azure-portalen och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b296b-192">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    ![Vad är Azure AD Connect?][17]

2. <span data-ttu-id="b296b-194">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="b296b-194">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Vad är Azure AD Connect?][18]


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b296b-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b296b-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="b296b-197">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b296b-197">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="b296b-198">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="b296b-198">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b296b-199">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b296b-199">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa testanvändare i Azure AD][100] 

2. <span data-ttu-id="b296b-201">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="b296b-201">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="b296b-202">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="b296b-202">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Skapa testanvändare i Azure AD][101] 

4. <span data-ttu-id="b296b-204">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="b296b-204">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Skapa testanvändare i Azure AD][102] 

5. <span data-ttu-id="b296b-206">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b296b-206">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Skapa testanvändare i Azure AD][103]
   
    <span data-ttu-id="b296b-208">a.</span><span class="sxs-lookup"><span data-stu-id="b296b-208">a.</span></span> <span data-ttu-id="b296b-209">Som **typ av användare**väljer **ny användare i din organisation**.</span><span class="sxs-lookup"><span data-stu-id="b296b-209">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="b296b-210">b.</span><span class="sxs-lookup"><span data-stu-id="b296b-210">b.</span></span> <span data-ttu-id="b296b-211">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b296b-211">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="b296b-212">c.</span><span class="sxs-lookup"><span data-stu-id="b296b-212">c.</span></span> <span data-ttu-id="b296b-213">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="b296b-213">Click Next.</span></span>

6. <span data-ttu-id="b296b-214">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b296b-214">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Skapa testanvändare i Azure AD][104] 
   
    <span data-ttu-id="b296b-216">a.</span><span class="sxs-lookup"><span data-stu-id="b296b-216">a.</span></span> <span data-ttu-id="b296b-217">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="b296b-217">In the **First Name** textbox, type **Britta**.</span></span> 
   
    <span data-ttu-id="b296b-218">b.</span><span class="sxs-lookup"><span data-stu-id="b296b-218">b.</span></span> <span data-ttu-id="b296b-219">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="b296b-219">In the **Last Name** textbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="b296b-220">c.</span><span class="sxs-lookup"><span data-stu-id="b296b-220">c.</span></span> <span data-ttu-id="b296b-221">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b296b-221">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="b296b-222">d.</span><span class="sxs-lookup"><span data-stu-id="b296b-222">d.</span></span> <span data-ttu-id="b296b-223">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="b296b-223">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="b296b-224">e.</span><span class="sxs-lookup"><span data-stu-id="b296b-224">e.</span></span> <span data-ttu-id="b296b-225">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b296b-225">Click **Next**.</span></span>

7. <span data-ttu-id="b296b-226">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b296b-226">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa testanvändare i Azure AD][105]  

8. <span data-ttu-id="b296b-228">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b296b-228">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Skapa testanvändare i Azure AD][106]   
   
    <span data-ttu-id="b296b-230">a.</span><span class="sxs-lookup"><span data-stu-id="b296b-230">a.</span></span> <span data-ttu-id="b296b-231">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b296b-231">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="b296b-232">b.</span><span class="sxs-lookup"><span data-stu-id="b296b-232">b.</span></span> <span data-ttu-id="b296b-233">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="b296b-233">Click **Complete**.</span></span>   

### <a name="creating-a-questetra-bpm-suite-test-user"></a><span data-ttu-id="b296b-234">Skapa en testanvändare Questetra BPM Suite</span><span class="sxs-lookup"><span data-stu-id="b296b-234">Creating a Questetra BPM Suite test user</span></span>
<span data-ttu-id="b296b-235">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i Questetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="b296b-235">The objective of this section is to create a user called Britta Simon in Questetra BPM Suite.</span></span>

<span data-ttu-id="b296b-236">**Utför följande steg för att skapa en användare som kallas Britta Simon i Questetra BPM Suite:**</span><span class="sxs-lookup"><span data-stu-id="b296b-236">**To create a user called Britta Simon in Questetra BPM Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="b296b-237">Inloggning på webbplatsen Questetra BPM Suite företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="b296b-237">Sign-on to your Questetra BPM Suite company site as an administrator.</span></span>
2. <span data-ttu-id="b296b-238">Gå till **systeminställningar > användarlistan > Ny användare**.</span><span class="sxs-lookup"><span data-stu-id="b296b-238">Go to **System Settings > User List > New User**.</span></span> 
3. <span data-ttu-id="b296b-239">I dialogrutan Ny användare utför du följande steg:</span><span class="sxs-lookup"><span data-stu-id="b296b-239">On the New User dialog, perform the following steps:</span></span> 
   
    ![Skapa testanvändare][300] 
   
    <span data-ttu-id="b296b-241">a.</span><span class="sxs-lookup"><span data-stu-id="b296b-241">a.</span></span> <span data-ttu-id="b296b-242">I den **namn** textruta anger Brittas användarnamn i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b296b-242">In the **Name** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="b296b-243">b.</span><span class="sxs-lookup"><span data-stu-id="b296b-243">b.</span></span> <span data-ttu-id="b296b-244">I den **e-post** textruta anger Brittas användarnamn i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b296b-244">In the **Email** textbox, type Britta's user name in Azure AD.</span></span>
   
    <span data-ttu-id="b296b-245">c.</span><span class="sxs-lookup"><span data-stu-id="b296b-245">c.</span></span> <span data-ttu-id="b296b-246">I den **lösenord** textruta, ange ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="b296b-246">In the **Password** textbox, type a password.</span></span>

4. <span data-ttu-id="b296b-247">Klicka på **Lägg till nya användare**.</span><span class="sxs-lookup"><span data-stu-id="b296b-247">Click **Add new user**.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b296b-248">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="b296b-248">Assigning the Azure AD test user</span></span>
<span data-ttu-id="b296b-249">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja sin åtkomst till Questetra BPM Suite.</span><span class="sxs-lookup"><span data-stu-id="b296b-249">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to Questetra BPM Suite.</span></span>

![Vad är Azure AD Connect?][200]

<span data-ttu-id="b296b-251">**Om du vill tilldela Questetra BPM Suite Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b296b-251">**To assign Britta Simon to Questetra BPM Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="b296b-252">På den klassiska Azure-portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="b296b-252">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Vad är Azure AD Connect?][201]
2. <span data-ttu-id="b296b-254">Välj i listan med program **Questetra BPM Suite**.</span><span class="sxs-lookup"><span data-stu-id="b296b-254">In the applications list, select **Questetra BPM Suite**.</span></span>
   
    ![Vad är Azure AD Connect?][205]
3. <span data-ttu-id="b296b-256">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="b296b-256">In the menu on the top, click **Users**.</span></span>
   
    ![Vad är Azure AD Connect?][202]
4. <span data-ttu-id="b296b-258">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b296b-258">In the Users list, select **Britta Simon**.</span></span>
   
    ![Vad är Azure AD Connect?][203]
5. <span data-ttu-id="b296b-260">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="b296b-260">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Vad är Azure AD Connect?][204]

### <a name="testing-single-sign-on"></a><span data-ttu-id="b296b-262">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b296b-262">Testing Single Sign-On</span></span>
<span data-ttu-id="b296b-263">Syftet med det här avsnittet är att testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b296b-263">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="b296b-264">När du klickar på panelen Questetra BPM Suite på åtkomstpanelen du bör få automatiskt loggat in på ditt Questetra BPM Suite-program.</span><span class="sxs-lookup"><span data-stu-id="b296b-264">When you click the Questetra BPM Suite tile in the Access Panel, you should get automatically signed-on to your Questetra BPM Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b296b-265">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b296b-265">Additional Resources</span></span>
* [<span data-ttu-id="b296b-266">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b296b-266">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b296b-267">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b296b-267">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 
