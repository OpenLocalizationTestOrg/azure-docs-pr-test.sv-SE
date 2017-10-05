---
title: "Självstudier: Azure Active Directory-integrering med SilkRoad livslängd Suite | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SilkRoad livslängd Suite."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a><span data-ttu-id="df7d0-103">Självstudier: Azure Active Directory-integrering med SilkRoad livslängd Suite</span><span class="sxs-lookup"><span data-stu-id="df7d0-103">Tutorial: Azure Active Directory integration with SilkRoad Life Suite</span></span>
<span data-ttu-id="df7d0-104">Syftet med den här kursen är att visa dig hur du integrerar SilkRoad livslängd Suite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="df7d0-104">The objective of this tutorial is to show you how to integrate SilkRoad Life Suite with Azure Active Directory (Azure AD).</span></span> 

<span data-ttu-id="df7d0-105">Integrera SilkRoad livslängd Suite med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="df7d0-105">Integrating SilkRoad Life Suite with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="df7d0-106">Du kan styra i Azure AD som har åtkomst till SilkRoad livslängd Suite</span><span class="sxs-lookup"><span data-stu-id="df7d0-106">You can control in Azure AD who has access to SilkRoad Life Suite</span></span> 
* <span data-ttu-id="df7d0-107">Du kan aktivera användarna att automatiskt hämta loggat in på SilkRoad livslängd Suite enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="df7d0-107">You can enable your users to automatically get signed-on to SilkRoad Life Suite single sign-on (SSO) with their Azure AD accounts</span></span>

<span data-ttu-id="df7d0-108">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="df7d0-108">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df7d0-109">Krav</span><span class="sxs-lookup"><span data-stu-id="df7d0-109">Prerequisites</span></span>
<span data-ttu-id="df7d0-110">För att konfigurera Azure AD-integrering med SilkRoad livslängd Suite, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="df7d0-110">To configure Azure AD integration with SilkRoad Life Suite, you need the following items:</span></span>

* <span data-ttu-id="df7d0-111">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="df7d0-111">An Azure AD subscription</span></span>
* <span data-ttu-id="df7d0-112">En prenumeration SilkRoad livslängd Suite SSO aktiverad</span><span class="sxs-lookup"><span data-stu-id="df7d0-112">A SilkRoad Life Suite SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="df7d0-113">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="df7d0-113">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="df7d0-114">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="df7d0-114">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="df7d0-115">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="df7d0-115">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="df7d0-116">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="df7d0-116">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="df7d0-117">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="df7d0-117">Scenario Description</span></span>
<span data-ttu-id="df7d0-118">Syftet med den här kursen är att du ska testa Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="df7d0-118">The objective of this tutorial is to enable you to test Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="df7d0-119">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="df7d0-119">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="df7d0-120">Att lägga till SilkRoad livslängd Suite från galleriet</span><span class="sxs-lookup"><span data-stu-id="df7d0-120">Adding SilkRoad Life Suite from the gallery</span></span> 
2. <span data-ttu-id="df7d0-121">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="df7d0-121">Configuring and testing Azure AD SSO</span></span>

## <a name="add-silkroad-life-suite-from-the-gallery"></a><span data-ttu-id="df7d0-122">Lägg till SilkRoad livslängd Suite från galleriet</span><span class="sxs-lookup"><span data-stu-id="df7d0-122">Add SilkRoad Life Suite from the gallery</span></span>
<span data-ttu-id="df7d0-123">Du måste lägga till SilkRoad livslängd Suite från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av SilkRoad livslängd Suite i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df7d0-123">To configure the integration of SilkRoad Life Suite into Azure AD, you need to add SilkRoad Life Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="df7d0-124">**Utför följande steg för att lägga till SilkRoad livslängd Suite från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="df7d0-124">**To add SilkRoad Life Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="df7d0-125">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-125">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="df7d0-127">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="df7d0-127">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="df7d0-128">Klicka för att öppna vyn program i vyn directory **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="df7d0-128">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Program][2]

4. <span data-ttu-id="df7d0-130">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="df7d0-130">Click **Add** at the bottom of the page.</span></span>
   
    ![Program][3]

5. <span data-ttu-id="df7d0-132">På den **vad vill du göra** dialogrutan klickar du på **lägga till ett program från galleriet**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-132">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Program][4]

6. <span data-ttu-id="df7d0-134">I sökrutan skriver **SilkRoad livslängd Suite**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-134">In the search box, type **SilkRoad Life Suite**.</span></span>
   
    ![Program][5]

7. <span data-ttu-id="df7d0-136">I resultatfönstret, Välj **SilkRoad livslängd Suite**, och klicka sedan på **Slutför** lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="df7d0-136">In the results pane, select **SilkRoad Life Suite**, and then click **Complete** to add the application.</span></span>
   
    ![Program][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="df7d0-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df7d0-138">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="df7d0-139">Syftet med det här avsnittet är att visa dig hur du konfigurerar och testa Azure AD SSO med SilkRoad livslängd Suite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="df7d0-139">The objective of this section is to show you how to configure and test Azure AD SSO with SilkRoad Life Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="df7d0-140">För enkel inloggning ska fungera, måste Azure AD motsvarighet användaren i SilkRoad livslängd Suite till en användare i Azure AD är okänt.</span><span class="sxs-lookup"><span data-stu-id="df7d0-140">For SSO to work, Azure AD needs to know what the counterpart user in SilkRoad Life Suite to an user in Azure AD is.</span></span> <span data-ttu-id="df7d0-141">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i SilkRoad livslängd Suite upprättas.</span><span class="sxs-lookup"><span data-stu-id="df7d0-141">In other words, a link relationship between an Azure AD user and the related user in SilkRoad Life Suite needs to be established.</span></span>

<span data-ttu-id="df7d0-142">Den här länken relationen upprättas genom att tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** i SilkRoad livslängd Suite.</span><span class="sxs-lookup"><span data-stu-id="df7d0-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SilkRoad Life Suite.</span></span>

<span data-ttu-id="df7d0-143">Om du vill konfigurera och testa Azure AD enkel inloggning med SilkRoad livslängd Suite, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="df7d0-143">To configure and test Azure AD single sign-on with SilkRoad Life Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="df7d0-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="df7d0-144">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="df7d0-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df7d0-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="df7d0-146">**[Skapa en testanvändare SilkRoad livslängd Suite](#creating-a-silkroad-life-suite-test-user)**  – har en motsvarighet för Britta Simon SilkRoad livslängd Suite som är kopplad till Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="df7d0-146">**[Creating a SilkRoad Life Suite test user](#creating-a-silkroad-life-suite-test-user)** - to have a counterpart of Britta Simon in SilkRoad Life Suite that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="df7d0-147">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="df7d0-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="df7d0-148">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="df7d0-148">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="df7d0-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df7d0-149">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="df7d0-150">Syftet med det här avsnittet är att aktivera Azure AD SSO i den klassiska Azure-portalen och konfigurera enkel inloggning i SilkRoad livslängd Suite-program.</span><span class="sxs-lookup"><span data-stu-id="df7d0-150">The objective of this section is to enable Azure AD SSO in the Azure classic portal and to configure SSO in your SilkRoad Life Suite application.</span></span>

<span data-ttu-id="df7d0-151">**Utför följande steg för att konfigurera Azure AD enkel inloggning med SilkRoad livslängd Suite:**</span><span class="sxs-lookup"><span data-stu-id="df7d0-151">**To configure Azure AD single sign-on with SilkRoad Life Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="df7d0-152">Inloggning på webbplatsen SilkRoad företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="df7d0-152">Sign-on to your SilkRoad company site as administrator.</span></span> 

  >[!NOTE] 
  > <span data-ttu-id="df7d0-153">För att få åtkomst till programmet SilkRoad livslängd Suite autentisering för att konfigurera federation med Microsoft Azure AD kan kontakta SilkRoad Support eller genom ditt ombud SilkRoad tjänster.</span><span class="sxs-lookup"><span data-stu-id="df7d0-153">To obtain access to the SilkRoad Life Suite Authentication application for configuring federation with Microsoft Azure AD, please contact SilkRoad Support or your SilkRoad Services representative.</span></span>
  > 

2. <span data-ttu-id="df7d0-154">Gå till **tjänstleverantör**, och klicka sedan på **Federation information**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-154">Go to **Service Provider**, and then click **Federation Details**.</span></span> 
   
    ![Azure AD-Single Sign-On][10] 

3. <span data-ttu-id="df7d0-156">Klicka på **hämta Federationsmetadata**, och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="df7d0-156">Click **Download Federation Metadata**, and then save the metadata file on your computer.</span></span>
   
    ![Azure AD-Single Sign-On][11] 

4. <span data-ttu-id="df7d0-158">I den klassiska Azure-portalen på den **SilkRoad livslängd Suite** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** att öppna den **Konfigurera enkel inloggning** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="df7d0-158">In the Azure classic portal, on the **SilkRoad Life Suite** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 

5. <span data-ttu-id="df7d0-160">På den **hur vill du att användarna kan logga in på SilkRoad livslängd Suite** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-160">On the **How would you like users to sign on to SilkRoad Life Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][7] 

6. <span data-ttu-id="df7d0-162">På den **konfigurera Appinställningar** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7d0-162">On the **Configure App Settings** dialog page, perform the following steps:</span></span>
   
    ![Azure AD-Single Sign-On][8]   
 1. <span data-ttu-id="df7d0-164">I den **logga URL** textruta, Skriv URL: en som används av dina användare logga in på webbplatsen SilkRoad livslängd Suite (t.ex.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span><span class="sxs-lookup"><span data-stu-id="df7d0-164">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your SilkRoad Life Suite site (e.g.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span></span>  
 2. <span data-ttu-id="df7d0-165">Öppna den hämtade **Silkroad** metadatafil.</span><span class="sxs-lookup"><span data-stu-id="df7d0-165">Open the downloaded **Silkroad** metadata file.</span></span> 
 3. <span data-ttu-id="df7d0-166">Leta upp den **AssertionConsumerService** tagg och sedan kopiera den **plats** attribut.</span><span class="sxs-lookup"><span data-stu-id="df7d0-166">Locate the **AssertionConsumerService** tag, and then copy the **Location** attribute.</span></span>         
   
    ![Azure AD-Single Sign-On][21] 
 4. <span data-ttu-id="df7d0-168">Klistra in värdet till den **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="df7d0-168">Paste the value into the **Reply URL** textbox.</span></span>  
 5. <span data-ttu-id="df7d0-169">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-169">Click **Next**.</span></span>

6. <span data-ttu-id="df7d0-170">På den **Konfigurera enkel inloggning på SilkRoad livslängd Suite** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7d0-170">On the **Configure single sign-on at SilkRoad Life Suite** page, perform the following steps:</span></span>
   
    ![Azure AD-Single Sign-On][9]  
 1. <span data-ttu-id="df7d0-172">Klicka på hämta certifikatet och spara filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="df7d0-172">Click Download certificate, and then save the file on your computer.</span></span>  
 2. <span data-ttu-id="df7d0-173">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-173">Click **Next**.</span></span>

7. <span data-ttu-id="df7d0-174">I din **SilkRoad** programmet, klickar du på **autentisering källor**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-174">In your **SilkRoad** application, click **Authentication Sources**.</span></span>
   
    ![Azure AD-Single Sign-On][12] 

8. <span data-ttu-id="df7d0-176">Klicka på **Lägg till autentiseringskälla**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-176">Click **Add Authentication Source**.</span></span> 
   
    ![Azure AD-Single Sign-On][13] 

9. <span data-ttu-id="df7d0-178">I den **Lägg till autentiseringskälla** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7d0-178">In the **Add Authentication Source** section, perform the following steps:</span></span> 
   
    ![Azure AD-Single Sign-On][14]  
 1. <span data-ttu-id="df7d0-180">Under **alternativ 2 - metadatafil**, klickar du på **Bläddra** att överföra metadatafilen hämtade.</span><span class="sxs-lookup"><span data-stu-id="df7d0-180">Under **Option 2 - Metadata File**, click **Browse** to upload the downloaded metadata file.</span></span>  
 2. <span data-ttu-id="df7d0-181">Klicka på **skapa identitetsleverantör med hjälp av fildata**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-181">Click **Create Identity Provider using File Data**.</span></span>

10. <span data-ttu-id="df7d0-182">I den **autentisering källor** klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-182">In the **Authentication Sources** section, click **Edit**.</span></span> 
    
     ![Azure AD-Single Sign-On][15] 

11. <span data-ttu-id="df7d0-184">På den **redigera autentiseringskälla** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7d0-184">On the **Edit Authentication Source** dialog, perform the following steps:</span></span> 
    
     ![Azure AD-Single Sign-On][16] 
 1. <span data-ttu-id="df7d0-186">Som **aktiverad**väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-186">As **Enabled**, select **Yes**.</span></span>   
 2. <span data-ttu-id="df7d0-187">I den **IdP beskrivning** textruta skriver du en beskrivning för konfigurationen (t.ex.: *Azure AD SSO*).</span><span class="sxs-lookup"><span data-stu-id="df7d0-187">In the **IdP Description** textbox, type a description for your configuration (e.g.: *Azure AD SSO*).</span></span>  
 3. <span data-ttu-id="df7d0-188">I den **IdP namnet** textruta, ange ett namn som är specifik för din konfiguration (t.ex.: *Azure SP*).</span><span class="sxs-lookup"><span data-stu-id="df7d0-188">In the **IdP Name** textbox, type a name that is specific to your configuration (e.g.: *Azure SP*).</span></span>  
 4. <span data-ttu-id="df7d0-189">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-189">Click **Save**.</span></span>

12. <span data-ttu-id="df7d0-190">Inaktivera alla autentisering källor.</span><span class="sxs-lookup"><span data-stu-id="df7d0-190">Disable all other authentication sources.</span></span> 
    
     ![Azure AD-Single Sign-On][17]

13. <span data-ttu-id="df7d0-192">I den klassiska Azure-portalen på den **enkel inloggning bekräftelse** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-192">In the Azure classic portal, on the **Single sign-on confirmation** page, click **Next**.</span></span>  
    
     ![Azure AD-Single Sign-On][18]

14. <span data-ttu-id="df7d0-194">På den **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-194">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
    
     ![Azure AD-Single Sign-On][19]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="df7d0-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="df7d0-196">Create an Azure AD test user</span></span>
<span data-ttu-id="df7d0-197">Syftet med det här avsnittet är att skapa en testanvändare i den klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="df7d0-197">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="df7d0-199">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="df7d0-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="df7d0-200">I den **klassiska Azure-portalen**, klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-200">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. <span data-ttu-id="df7d0-202">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="df7d0-202">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="df7d0-203">Klicka för att visa en lista över användare, på menyn upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-203">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="df7d0-205">Öppna den **Lägg till användare** i verktygsfältet längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-205">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="df7d0-207">På den **berätta om den här användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7d0-207">On the **Tell us about this user** dialog page, perform the following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. <span data-ttu-id="df7d0-209">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="df7d0-209">As Type Of User, select New user in your organization.</span></span>  
 2. <span data-ttu-id="df7d0-210">I användarnamnet **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-210">In the User Name **textbox**, type **BrittaSimon**.</span></span> 
 3. <span data-ttu-id="df7d0-211">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-211">Click **Next**.</span></span>

6. <span data-ttu-id="df7d0-212">På den **användarprofil** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7d0-212">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. <span data-ttu-id="df7d0-214">I den **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-214">In the **First Name** textbox, type **Britta**.</span></span>    
 2. <span data-ttu-id="df7d0-215">I den **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-215">In the **Last Name** textbox, type, **Simon**.</span></span> 
 3. <span data-ttu-id="df7d0-216">I den **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-216">In the **Display Name** textbox, type **Britta Simon**.</span></span> 
 4. <span data-ttu-id="df7d0-217">I den **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-217">In the **Role** list, select **User**.</span></span>
 5. <span data-ttu-id="df7d0-218">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-218">Click **Next**.</span></span>

7. <span data-ttu-id="df7d0-219">På den **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-219">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="df7d0-221">På den **skaffa tillfälligt lösenord** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="df7d0-221">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. <span data-ttu-id="df7d0-223">Anteckna värdet för den **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-223">Write down the value of the **New Password**.</span></span> 
 2. <span data-ttu-id="df7d0-224">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="df7d0-224">Click **Complete**.</span></span>   

### <a name="create-a-silkroad-life-suite-test-user"></a><span data-ttu-id="df7d0-225">Skapa en testanvändare SilkRoad livslängd Suite</span><span class="sxs-lookup"><span data-stu-id="df7d0-225">Create a SilkRoad Life Suite test user</span></span>
<span data-ttu-id="df7d0-226">Syftet med det här avsnittet är att skapa en användare som kallas Britta Simon i SilkRoad livslängd Suite.</span><span class="sxs-lookup"><span data-stu-id="df7d0-226">The objective of this section is to create a user called Britta Simon in SilkRoad Life Suite.</span></span> <span data-ttu-id="df7d0-227">Brittas måste ha ett ID för enkel inloggning (kallas ibland ett *AuthParam*) som matchar Britta's **e-postadress** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df7d0-227">Britta's must have an SSO ID (sometimes referred to as an *AuthParam*) that matches Britta's **emailaddress** in Azure AD.</span></span>

<span data-ttu-id="df7d0-228">**Utför följande steg för att skapa en användare som kallas Britta Simon i SilkRoad livslängd Suite:**</span><span class="sxs-lookup"><span data-stu-id="df7d0-228">**To create a user called Britta Simon in SilkRoad Life Suite, perform the following steps:**</span></span>

- <span data-ttu-id="df7d0-229">Be din SilkRoad livslängd Suite supportteamet för att skapa en användare som har som **SSO ID** attributet samma värde som den **e-postadress** av Britta Simon i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="df7d0-229">Ask your SilkRoad Life Suite support team to create a user that has as **SSO ID** attribute the same value as the **emailaddress** of Britta Simon in Azure AD.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="df7d0-230">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="df7d0-230">Assign the Azure AD test user</span></span>
<span data-ttu-id="df7d0-231">Syftet med det här avsnittet är att aktivera Britta Simon att använda Azure SSO med sin åtkomst beviljas till SilkRoad livslängd Suite.</span><span class="sxs-lookup"><span data-stu-id="df7d0-231">The objective of this section is to enable Britta Simon to use Azure SSO by granting her access to SilkRoad Life Suite.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="df7d0-233">**Om du vill tilldela SilkRoad livslängd Suite Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="df7d0-233">**To assign Britta Simon to SilkRoad Life Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="df7d0-234">På den klassiska Azure-portalen för att öppna vyn program i katalogen vyn klickar du på **program** på huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="df7d0-234">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Tilldela användare][201] 

2. <span data-ttu-id="df7d0-236">Välj i listan med program **SilkRoad livslängd Suite**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-236">In the applications list, select **SilkRoad Life Suite**.</span></span>
   
    ![Tilldela användare][202] 

3. <span data-ttu-id="df7d0-238">Klicka på menyn högst upp **användare**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-238">In the menu on the top, click **Users**.</span></span>
   
    ![Tilldela användare][203] 

4. <span data-ttu-id="df7d0-240">Välj i listan användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-240">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="df7d0-241">Klicka på i verktygsfältet längst ned i **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="df7d0-241">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="df7d0-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="df7d0-243">Test single sign-on</span></span>
<span data-ttu-id="df7d0-244">Syftet med det här avsnittet är att testa Azure AD SSO-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="df7d0-244">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="df7d0-245">När du klickar på panelen SilkRoad livslängd Suite på åtkomstpanelen du bör få automatiskt loggat in på ditt SilkRoad livslängd Suite-program.</span><span class="sxs-lookup"><span data-stu-id="df7d0-245">When you click the SilkRoad Life Suite tile in the Access Panel, you should get automatically signed-on to your SilkRoad Life Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="df7d0-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="df7d0-246">Additional Resources</span></span>
* [<span data-ttu-id="df7d0-247">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="df7d0-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="df7d0-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="df7d0-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





