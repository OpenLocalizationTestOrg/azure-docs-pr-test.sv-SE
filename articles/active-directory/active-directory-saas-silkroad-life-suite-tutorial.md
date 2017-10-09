---
title: "Självstudier: Azure Active Directory-integrering med SilkRoad livslängd Suite | Microsoft Docs"
description: "Lär dig hur tooconfigure enkel inloggning mellan Azure Active Directory och SilkRoad livslängd Suite."
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
ms.openlocfilehash: 07367282ab42b7332f166d64743b4b447aec4935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a><span data-ttu-id="b54c5-103">Självstudier: Azure Active Directory-integrering med SilkRoad livslängd Suite</span><span class="sxs-lookup"><span data-stu-id="b54c5-103">Tutorial: Azure Active Directory integration with SilkRoad Life Suite</span></span>
<span data-ttu-id="b54c5-104">hello syftet med den här kursen är tooshow du hur toointegrate SilkRoad livslängd Suite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="b54c5-104">hello objective of this tutorial is tooshow you how toointegrate SilkRoad Life Suite with Azure Active Directory (Azure AD).</span></span> 

<span data-ttu-id="b54c5-105">Integrera SilkRoad livslängd Suite med Azure AD ger dig hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="b54c5-105">Integrating SilkRoad Life Suite with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="b54c5-106">Du kan styra i Azure AD som har åtkomst tooSilkRoad livslängd Suite</span><span class="sxs-lookup"><span data-stu-id="b54c5-106">You can control in Azure AD who has access tooSilkRoad Life Suite</span></span> 
* <span data-ttu-id="b54c5-107">Du kan aktivera din användare tooautomatically get inloggade tooSilkRoad livslängd Suite enkel inloggning (SSO) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="b54c5-107">You can enable your users tooautomatically get signed-on tooSilkRoad Life Suite single sign-on (SSO) with their Azure AD accounts</span></span>

<span data-ttu-id="b54c5-108">Om du vill tooknow mer information om integrering av SaaS-app med Azure AD, se [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b54c5-108">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b54c5-109">Krav</span><span class="sxs-lookup"><span data-stu-id="b54c5-109">Prerequisites</span></span>
<span data-ttu-id="b54c5-110">tooconfigure Azure AD-integrering med SilkRoad livslängd Suite måste hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="b54c5-110">tooconfigure Azure AD integration with SilkRoad Life Suite, you need hello following items:</span></span>

* <span data-ttu-id="b54c5-111">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="b54c5-111">An Azure AD subscription</span></span>
* <span data-ttu-id="b54c5-112">En prenumeration SilkRoad livslängd Suite SSO aktiverad</span><span class="sxs-lookup"><span data-stu-id="b54c5-112">A SilkRoad Life Suite SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="b54c5-113">tootest hello stegen i den här självstudiekursen, rekommenderas inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="b54c5-113">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="b54c5-114">tootest hello steg i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="b54c5-114">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="b54c5-115">Du bör inte använda produktionsmiljön, om det inte är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b54c5-115">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="b54c5-116">Om du inte har en utvärderingsversion Azure AD-miljö kan du få en [utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b54c5-116">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="b54c5-117">Scenario-beskrivning</span><span class="sxs-lookup"><span data-stu-id="b54c5-117">Scenario Description</span></span>
<span data-ttu-id="b54c5-118">hello syftet med den här kursen är tooenable du tootest Azure AD SSO i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="b54c5-118">hello objective of this tutorial is tooenable you tootest Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="b54c5-119">hello-scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="b54c5-119">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b54c5-120">Att lägga till SilkRoad livslängd Suite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b54c5-120">Adding SilkRoad Life Suite from hello gallery</span></span> 
2. <span data-ttu-id="b54c5-121">Konfigurera och testa Azure AD-SSO</span><span class="sxs-lookup"><span data-stu-id="b54c5-121">Configuring and testing Azure AD SSO</span></span>

## <a name="add-silkroad-life-suite-from-hello-gallery"></a><span data-ttu-id="b54c5-122">Lägg till SilkRoad livslängd Suite från hello-galleriet</span><span class="sxs-lookup"><span data-stu-id="b54c5-122">Add SilkRoad Life Suite from hello gallery</span></span>
<span data-ttu-id="b54c5-123">tooconfigure hello integrering av SilkRoad livslängd Suite i Azure AD, behöver du tooadd SilkRoad livslängd Suite hello galleriet tooyour listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="b54c5-123">tooconfigure hello integration of SilkRoad Life Suite into Azure AD, you need tooadd SilkRoad Life Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b54c5-124">**tooadd SilkRoad livslängd Suite från galleriet hello utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b54c5-124">**tooadd SilkRoad Life Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b54c5-125">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-125">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="b54c5-127">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="b54c5-127">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="b54c5-128">tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="b54c5-128">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Program][2]

4. <span data-ttu-id="b54c5-130">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="b54c5-130">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Program][3]

5. <span data-ttu-id="b54c5-132">På hello **vad vill du vill toodo** dialogrutan klickar du på **lägga till ett program från galleriet hello**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-132">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Program][4]

6. <span data-ttu-id="b54c5-134">Skriv i sökrutan hello **SilkRoad livslängd Suite**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-134">In hello search box, type **SilkRoad Life Suite**.</span></span>
   
    ![Program][5]

7. <span data-ttu-id="b54c5-136">I resultatfönstret hello väljer **SilkRoad livslängd Suite**, och klicka sedan på **Slutför** tooadd hello program.</span><span class="sxs-lookup"><span data-stu-id="b54c5-136">In hello results pane, select **SilkRoad Life Suite**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Program][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="b54c5-138">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b54c5-138">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="b54c5-139">hello syftet med det här avsnittet är tooshow hur tooconfigure och testa Azure AD SSO med SilkRoad livslängd Suite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="b54c5-139">hello objective of this section is tooshow you how tooconfigure and test Azure AD SSO with SilkRoad Life Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b54c5-140">För enkel inloggning toowork måste Azure AD tooknow vilka hello motsvarighet användaren i SilkRoad livslängd Suite tooan användare i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b54c5-140">For SSO toowork, Azure AD needs tooknow what hello counterpart user in SilkRoad Life Suite tooan user in Azure AD is.</span></span> <span data-ttu-id="b54c5-141">Med andra ord måste en länk mellan en Azure AD-användare och hello relaterade användare i SilkRoad livslängd Suite toobe upprättas.</span><span class="sxs-lookup"><span data-stu-id="b54c5-141">In other words, a link relationship between an Azure AD user and hello related user in SilkRoad Life Suite needs toobe established.</span></span>

<span data-ttu-id="b54c5-142">Den här länken relationen upprättas genom att tilldela hello värdet för hello **användarnamn** i Azure AD som hello värde för hello **användarnamn** i SilkRoad livslängd Suite.</span><span class="sxs-lookup"><span data-stu-id="b54c5-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SilkRoad Life Suite.</span></span>

<span data-ttu-id="b54c5-143">tooconfigure och testa Azure AD enkel inloggning med SilkRoad livslängd Suite, behöver du toocomplete hello följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="b54c5-143">tooconfigure and test Azure AD single sign-on with SilkRoad Life Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b54c5-144">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-single-sign-on)**  -tooenable användare-toouse den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="b54c5-144">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b54c5-145">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  -tootest Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b54c5-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b54c5-146">**[Skapa en testanvändare SilkRoad livslängd Suite](#creating-a-silkroad-life-suite-test-user)**  -toohave en motsvarighet för Britta Simon i SilkRoad livslängd Suite som är länkade toohello Azure AD-representation av henne.</span><span class="sxs-lookup"><span data-stu-id="b54c5-146">**[Creating a SilkRoad Life Suite test user](#creating-a-silkroad-life-suite-test-user)** - toohave a counterpart of Britta Simon in SilkRoad Life Suite that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="b54c5-147">**[Tilldela hello Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="b54c5-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b54c5-148">**[Testa enkel inloggning](#testing-single-sign-on)**  -tooverify hello om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="b54c5-148">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="b54c5-149">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b54c5-149">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="b54c5-150">hello syftet med det här avsnittet är tooenable Azure AD SSO i hello klassiska Azure-portalen och tooconfigure SSO i SilkRoad livslängd Suite-program.</span><span class="sxs-lookup"><span data-stu-id="b54c5-150">hello objective of this section is tooenable Azure AD SSO in hello Azure classic portal and tooconfigure SSO in your SilkRoad Life Suite application.</span></span>

<span data-ttu-id="b54c5-151">**tooconfigure Azure AD enkel inloggning med SilkRoad livslängd Suite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b54c5-151">**tooconfigure Azure AD single sign-on with SilkRoad Life Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b54c5-152">Inloggning tooyour SilkRoad företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="b54c5-152">Sign-on tooyour SilkRoad company site as administrator.</span></span> 

  >[!NOTE] 
  > <span data-ttu-id="b54c5-153">tooobtain åtkomst toohello SilkRoad livslängd Suite autentisering program för att konfigurera federation med Microsoft Azure AD kontakta SilkRoad Support eller genom ditt ombud SilkRoad tjänster.</span><span class="sxs-lookup"><span data-stu-id="b54c5-153">tooobtain access toohello SilkRoad Life Suite Authentication application for configuring federation with Microsoft Azure AD, please contact SilkRoad Support or your SilkRoad Services representative.</span></span>
  > 

2. <span data-ttu-id="b54c5-154">Gå för**tjänstleverantör**, och klicka sedan på **Federation information**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-154">Go too**Service Provider**, and then click **Federation Details**.</span></span> 
   
    ![Azure AD-Single Sign-On][10] 

3. <span data-ttu-id="b54c5-156">Klicka på **hämta Federationsmetadata**, och sedan spara hello metadatafil på datorn.</span><span class="sxs-lookup"><span data-stu-id="b54c5-156">Click **Download Federation Metadata**, and then save hello metadata file on your computer.</span></span>
   
    ![Azure AD-Single Sign-On][11] 

4. <span data-ttu-id="b54c5-158">I hello klassiska Azure-portalen på hello **SilkRoad livslängd Suite** integreringssidan för programmet, klickar du på **Konfigurera enkel inloggning** tooopen hello **Konfigurera enkel inloggning**  dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="b54c5-158">In hello Azure classic portal, on hello **SilkRoad Life Suite** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Konfigurera enkel inloggning][6] 

5. <span data-ttu-id="b54c5-160">På hello **hur skulle du som användare toosign på tooSilkRoad livslängd Suite** väljer **Azure AD enkel inloggning**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-160">On hello **How would you like users toosign on tooSilkRoad Life Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Azure AD-Single Sign-On][7] 

6. <span data-ttu-id="b54c5-162">På hello **konfigurera Appinställningar** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b54c5-162">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>
   
    ![Azure AD-Single Sign-On][8]   
 1. <span data-ttu-id="b54c5-164">I hello **logga URL** textruta typen hello URL som används av din användare toosign på tooyour SilkRoad livslängd Suite plats (t.ex.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span><span class="sxs-lookup"><span data-stu-id="b54c5-164">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour SilkRoad Life Suite site (e.g.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span></span>  
 2. <span data-ttu-id="b54c5-165">Öppna hello hämtas **Silkroad** metadatafil.</span><span class="sxs-lookup"><span data-stu-id="b54c5-165">Open hello downloaded **Silkroad** metadata file.</span></span> 
 3. <span data-ttu-id="b54c5-166">Leta upp hello **AssertionConsumerService** tagg och sedan kopiera hello **plats** attribut.</span><span class="sxs-lookup"><span data-stu-id="b54c5-166">Locate hello **AssertionConsumerService** tag, and then copy hello **Location** attribute.</span></span>         
   
    ![Azure AD-Single Sign-On][21] 
 4. <span data-ttu-id="b54c5-168">Klistra in hello värdet i hello **Reply URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="b54c5-168">Paste hello value into hello **Reply URL** textbox.</span></span>  
 5. <span data-ttu-id="b54c5-169">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-169">Click **Next**.</span></span>

6. <span data-ttu-id="b54c5-170">På hello **Konfigurera enkel inloggning på SilkRoad livslängd Suite** utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b54c5-170">On hello **Configure single sign-on at SilkRoad Life Suite** page, perform hello following steps:</span></span>
   
    ![Azure AD-Single Sign-On][9]  
 1. <span data-ttu-id="b54c5-172">Klicka på hämta certifikatet och spara hello-filen på datorn.</span><span class="sxs-lookup"><span data-stu-id="b54c5-172">Click Download certificate, and then save hello file on your computer.</span></span>  
 2. <span data-ttu-id="b54c5-173">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-173">Click **Next**.</span></span>

7. <span data-ttu-id="b54c5-174">I din **SilkRoad** programmet, klickar du på **autentisering källor**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-174">In your **SilkRoad** application, click **Authentication Sources**.</span></span>
   
    ![Azure AD-Single Sign-On][12] 

8. <span data-ttu-id="b54c5-176">Klicka på **Lägg till autentiseringskälla**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-176">Click **Add Authentication Source**.</span></span> 
   
    ![Azure AD-Single Sign-On][13] 

9. <span data-ttu-id="b54c5-178">I hello **Lägg till autentiseringskälla** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b54c5-178">In hello **Add Authentication Source** section, perform hello following steps:</span></span> 
   
    ![Azure AD-Single Sign-On][14]  
 1. <span data-ttu-id="b54c5-180">Under **alternativ 2 - metadatafil**, klickar du på **Bläddra** tooupload hello hämtas metadatafil.</span><span class="sxs-lookup"><span data-stu-id="b54c5-180">Under **Option 2 - Metadata File**, click **Browse** tooupload hello downloaded metadata file.</span></span>  
 2. <span data-ttu-id="b54c5-181">Klicka på **skapa identitetsleverantör med hjälp av fildata**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-181">Click **Create Identity Provider using File Data**.</span></span>

10. <span data-ttu-id="b54c5-182">I hello **autentisering källor** klickar du på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-182">In hello **Authentication Sources** section, click **Edit**.</span></span> 
    
     ![Azure AD-Single Sign-On][15] 

11. <span data-ttu-id="b54c5-184">På hello **redigera autentiseringskälla** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b54c5-184">On hello **Edit Authentication Source** dialog, perform hello following steps:</span></span> 
    
     ![Azure AD-Single Sign-On][16] 
 1. <span data-ttu-id="b54c5-186">Som **aktiverad**väljer **Ja**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-186">As **Enabled**, select **Yes**.</span></span>   
 2. <span data-ttu-id="b54c5-187">I hello **IdP beskrivning** textruta skriver du en beskrivning för konfigurationen (t.ex.: *Azure AD SSO*).</span><span class="sxs-lookup"><span data-stu-id="b54c5-187">In hello **IdP Description** textbox, type a description for your configuration (e.g.: *Azure AD SSO*).</span></span>  
 3. <span data-ttu-id="b54c5-188">I hello **IdP namnet** textruta, ange ett namn som är specifika tooyour konfiguration (t.ex.: *Azure SP*).</span><span class="sxs-lookup"><span data-stu-id="b54c5-188">In hello **IdP Name** textbox, type a name that is specific tooyour configuration (e.g.: *Azure SP*).</span></span>  
 4. <span data-ttu-id="b54c5-189">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-189">Click **Save**.</span></span>

12. <span data-ttu-id="b54c5-190">Inaktivera alla autentisering källor.</span><span class="sxs-lookup"><span data-stu-id="b54c5-190">Disable all other authentication sources.</span></span> 
    
     ![Azure AD-Single Sign-On][17]

13. <span data-ttu-id="b54c5-192">I hello klassiska Azure-portalen på hello **enkel inloggning bekräftelse** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-192">In hello Azure classic portal, on hello **Single sign-on confirmation** page, click **Next**.</span></span>  
    
     ![Azure AD-Single Sign-On][18]

14. <span data-ttu-id="b54c5-194">På hello **enkel inloggning bekräftelse** klickar du på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-194">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
    
     ![Azure AD-Single Sign-On][19]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="b54c5-196">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="b54c5-196">Create an Azure AD test user</span></span>
<span data-ttu-id="b54c5-197">hello syftet med det här avsnittet är toocreate en testanvändare i hello klassiska Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b54c5-197">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][20]

<span data-ttu-id="b54c5-199">**toocreate en testanvändare i Azure AD kan utföra hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b54c5-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b54c5-200">I hello **klassiska Azure-portalen**, på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-200">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. <span data-ttu-id="b54c5-202">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="b54c5-202">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="b54c5-203">toodisplay hello lista över användare i hello menyn hello överst, klickar du på **användare**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-203">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b54c5-205">tooopen hello **Lägg till användare** i hello verktygsfältet på hello längst ned i dialogrutan klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-205">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="b54c5-207">På hello **berätta om den här användaren** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b54c5-207">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. <span data-ttu-id="b54c5-209">Välj ny användare i din organisation som typ av användare.</span><span class="sxs-lookup"><span data-stu-id="b54c5-209">As Type Of User, select New user in your organization.</span></span>  
 2. <span data-ttu-id="b54c5-210">I hello användarnamn **textruta**, typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-210">In hello User Name **textbox**, type **BrittaSimon**.</span></span> 
 3. <span data-ttu-id="b54c5-211">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-211">Click **Next**.</span></span>

6. <span data-ttu-id="b54c5-212">På hello **användarprofil** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b54c5-212">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. <span data-ttu-id="b54c5-214">I hello **Förnamn** textruta typen **Britta**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-214">In hello **First Name** textbox, type **Britta**.</span></span>    
 2. <span data-ttu-id="b54c5-215">I hello **efternamn** textruta typ, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-215">In hello **Last Name** textbox, type, **Simon**.</span></span> 
 3. <span data-ttu-id="b54c5-216">I hello **visningsnamn** textruta typen **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-216">In hello **Display Name** textbox, type **Britta Simon**.</span></span> 
 4. <span data-ttu-id="b54c5-217">I hello **rollen** väljer **användaren**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-217">In hello **Role** list, select **User**.</span></span>
 5. <span data-ttu-id="b54c5-218">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-218">Click **Next**.</span></span>

7. <span data-ttu-id="b54c5-219">På hello **skaffa tillfälligt lösenord** dialogrutan sidan, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-219">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="b54c5-221">På hello **skaffa tillfälligt lösenord** dialogrutan utför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="b54c5-221">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. <span data-ttu-id="b54c5-223">Skriv ned hello värdet för hello **nytt lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-223">Write down hello value of hello **New Password**.</span></span> 
 2. <span data-ttu-id="b54c5-224">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="b54c5-224">Click **Complete**.</span></span>   

### <a name="create-a-silkroad-life-suite-test-user"></a><span data-ttu-id="b54c5-225">Skapa en testanvändare SilkRoad livslängd Suite</span><span class="sxs-lookup"><span data-stu-id="b54c5-225">Create a SilkRoad Life Suite test user</span></span>
<span data-ttu-id="b54c5-226">hello syftet med det här avsnittet är toocreate en användare som kallas Britta Simon i SilkRoad livslängd Suite.</span><span class="sxs-lookup"><span data-stu-id="b54c5-226">hello objective of this section is toocreate a user called Britta Simon in SilkRoad Life Suite.</span></span> <span data-ttu-id="b54c5-227">Brittas måste ha ett ID för enkel inloggning (kallas ibland tooas en *AuthParam*) som matchar Britta's **e-postadress** i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b54c5-227">Britta's must have an SSO ID (sometimes referred tooas an *AuthParam*) that matches Britta's **emailaddress** in Azure AD.</span></span>

<span data-ttu-id="b54c5-228">**toocreate en användare som kallas Britta Simon i SilkRoad livslängd Suite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b54c5-228">**toocreate a user called Britta Simon in SilkRoad Life Suite, perform hello following steps:**</span></span>

- <span data-ttu-id="b54c5-229">Be din SilkRoad livslängd Suite support-teamet toocreate en användare som har som **SSO ID** attributet hello samma värde som hello **e-postadress** av Britta Simon i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b54c5-229">Ask your SilkRoad Life Suite support team toocreate a user that has as **SSO ID** attribute hello same value as hello **emailaddress** of Britta Simon in Azure AD.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="b54c5-230">Tilldela användare hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="b54c5-230">Assign hello Azure AD test user</span></span>
<span data-ttu-id="b54c5-231">hello syftet med det här avsnittet är tooenable Britta Simon toouse Azure SSO genom att ge sina access tooSilkRoad livslängd Suite.</span><span class="sxs-lookup"><span data-stu-id="b54c5-231">hello objective of this section is tooenable Britta Simon toouse Azure SSO by granting her access tooSilkRoad Life Suite.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="b54c5-233">**tooassign Britta Simon tooSilkRoad livslängd Suite utför hello följande steg:**</span><span class="sxs-lookup"><span data-stu-id="b54c5-233">**tooassign Britta Simon tooSilkRoad Life Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="b54c5-234">På hello Azure klassiska portal, tooopen hello program i vyn hello directory vyn klickar du på **program** i hello huvudmenyn.</span><span class="sxs-lookup"><span data-stu-id="b54c5-234">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Tilldela användare][201] 

2. <span data-ttu-id="b54c5-236">Välj i listan med program hello **SilkRoad livslängd Suite**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-236">In hello applications list, select **SilkRoad Life Suite**.</span></span>
   
    ![Tilldela användare][202] 

3. <span data-ttu-id="b54c5-238">Hello-menyn överst hello **användare**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-238">In hello menu on hello top, click **Users**.</span></span>
   
    ![Tilldela användare][203] 

4. <span data-ttu-id="b54c5-240">Markera i hello användare **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-240">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="b54c5-241">Klicka i hello verktygsfältet hello längst ned **tilldela**.</span><span class="sxs-lookup"><span data-stu-id="b54c5-241">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Tilldela användare][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="b54c5-243">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="b54c5-243">Test single sign-on</span></span>
<span data-ttu-id="b54c5-244">hello syftet med det här avsnittet är tootest din Azure AD SSO konfiguration av hello åtkomstpanelen.</span><span class="sxs-lookup"><span data-stu-id="b54c5-244">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="b54c5-245">När du klickar på hello SilkRoad livslängd Suite-panelen i hello åtkomstpanelen får automatiskt inloggade tooyour SilkRoad livslängd Suite-program.</span><span class="sxs-lookup"><span data-stu-id="b54c5-245">When you click hello SilkRoad Life Suite tile in hello Access Panel, you should get automatically signed-on tooyour SilkRoad Life Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b54c5-246">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="b54c5-246">Additional Resources</span></span>
* [<span data-ttu-id="b54c5-247">Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b54c5-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b54c5-248">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="b54c5-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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





