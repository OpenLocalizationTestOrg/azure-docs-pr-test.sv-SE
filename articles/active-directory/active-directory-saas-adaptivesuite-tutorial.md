---
title: "Självstudier: Azure Active Directory-integrering med anpassningsbar Suite | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och anpassningsbar Suite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 5d7ba2f4c7d814e3aaa1bf804ddc5030380ccb2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a><span data-ttu-id="64c79-103">Självstudier: Azure Active Directory-integrering med anpassningsbar Suite</span><span class="sxs-lookup"><span data-stu-id="64c79-103">Tutorial: Azure Active Directory integration with Adaptive Suite</span></span>

<span data-ttu-id="64c79-104">I kursen får lära du att integrera anpassningsbar Suite med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="64c79-104">In this tutorial, you learn how to integrate Adaptive Suite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="64c79-105">Integrera anpassningsbar Suite med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="64c79-105">Integrating Adaptive Suite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="64c79-106">Du kan styra i Azure AD som har tillgång till anpassningsbara Suite</span><span class="sxs-lookup"><span data-stu-id="64c79-106">You can control in Azure AD who has access to Adaptive Suite</span></span>
- <span data-ttu-id="64c79-107">Du kan aktivera användarna att automatiskt hämta loggat in på anpassningsbar Suite (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="64c79-107">You can enable your users to automatically get signed-on to Adaptive Suite (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="64c79-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="64c79-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="64c79-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="64c79-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64c79-110">Krav</span><span class="sxs-lookup"><span data-stu-id="64c79-110">Prerequisites</span></span>

<span data-ttu-id="64c79-111">För att konfigurera Azure AD-integrering med anpassningsbar Suite, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="64c79-111">To configure Azure AD integration with Adaptive Suite, you need the following items:</span></span>

- <span data-ttu-id="64c79-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="64c79-112">An Azure AD subscription</span></span>
- <span data-ttu-id="64c79-113">En anpassningsbar Suite enkel inloggning på aktiverade prenumeration</span><span class="sxs-lookup"><span data-stu-id="64c79-113">An Adaptive Suite single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="64c79-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="64c79-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="64c79-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="64c79-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="64c79-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="64c79-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="64c79-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64c79-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="64c79-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="64c79-118">Scenario description</span></span>
<span data-ttu-id="64c79-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="64c79-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="64c79-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="64c79-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="64c79-121">Att lägga till anpassad Suite från galleriet</span><span class="sxs-lookup"><span data-stu-id="64c79-121">Adding Adaptive Suite from the gallery</span></span>
2. <span data-ttu-id="64c79-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64c79-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-adaptive-suite-from-the-gallery"></a><span data-ttu-id="64c79-123">Att lägga till anpassad Suite från galleriet</span><span class="sxs-lookup"><span data-stu-id="64c79-123">Adding Adaptive Suite from the gallery</span></span>
<span data-ttu-id="64c79-124">För att konfigurera integrering av anpassningsbar Suite i Azure AD, som du behöver lägga till anpassad Suite från galleriet i listan över hanterade SaaS-appar.</span><span class="sxs-lookup"><span data-stu-id="64c79-124">To configure the integration of Adaptive Suite into Azure AD, you need to add Adaptive Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="64c79-125">**Utför följande steg för att lägga till anpassad Suite från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="64c79-125">**To add Adaptive Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="64c79-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="64c79-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="64c79-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="64c79-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="64c79-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="64c79-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="64c79-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64c79-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="64c79-133">I sökrutan skriver **anpassningsbar Suite**.</span><span class="sxs-lookup"><span data-stu-id="64c79-133">In the search box, type **Adaptive Suite**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_search.png)

5. <span data-ttu-id="64c79-135">Välj i resultatpanelen **anpassningsbar Suite**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="64c79-135">In the results panel, select **Adaptive Suite**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="64c79-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64c79-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="64c79-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med anpassningsbar Suite baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="64c79-138">In this section, you configure and test Azure AD single sign-on with Adaptive Suite based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="64c79-139">Azure AD måste du känna till användaren i anpassningsbar Suite motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="64c79-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Adaptive Suite is to a user in Azure AD.</span></span> <span data-ttu-id="64c79-140">Med andra ord måste en länk mellan en Azure AD-användare och relaterade användaren i anpassningsbar Suite upprättas.</span><span class="sxs-lookup"><span data-stu-id="64c79-140">In other words, a link relationship between an Azure AD user and the related user in Adaptive Suite needs to be established.</span></span>

<span data-ttu-id="64c79-141">I anpassningsbar Suite tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="64c79-141">In Adaptive Suite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="64c79-142">Om du vill konfigurera och testa Azure AD enkel inloggning med anpassningsbar Suite, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="64c79-142">To configure and test Azure AD single sign-on with Adaptive Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="64c79-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="64c79-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="64c79-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64c79-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="64c79-145">**[Skapa en anpassningsbar Suite testanvändare](#creating-an-adaptive-suite-test-user)**  – du har en motsvarighet för Britta Simon i anpassningsbar Suite som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="64c79-145">**[Creating an Adaptive Suite test user](#creating-an-adaptive-suite-test-user)** - to have a counterpart of Britta Simon in Adaptive Suite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="64c79-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="64c79-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="64c79-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="64c79-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="64c79-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64c79-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="64c79-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i anpassningsbar Suite-program.</span><span class="sxs-lookup"><span data-stu-id="64c79-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Adaptive Suite application.</span></span>

<span data-ttu-id="64c79-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med anpassningsbar Suite:**</span><span class="sxs-lookup"><span data-stu-id="64c79-150">**To configure Azure AD single sign-on with Adaptive Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="64c79-151">I Azure-portalen på den **anpassningsbar Suite** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="64c79-151">In the Azure portal, on the **Adaptive Suite** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="64c79-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="64c79-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_samlbase.png)

3. <span data-ttu-id="64c79-155">På den **anpassningsbar Suite domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="64c79-155">On the **Adaptive Suite Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_url.png)

    <span data-ttu-id="64c79-157">I den **Reply URL** textruta Skriv en URL med följande mönster:`https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span><span class="sxs-lookup"><span data-stu-id="64c79-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://login.adaptiveinsights.com:443/samlsso/<unique-id>`</span></span>

    >[!NOTE]
    > <span data-ttu-id="64c79-158">Du får det här värdet från det anpassade paketet **SAML SSO inställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="64c79-158">You can get this value from the Adaptive Suite’s **SAML SSO Settings** page.</span></span>
    >  

4. <span data-ttu-id="64c79-159">På den **SAML-signeringscertifikat** klickar du på **certifikat (Base64)** och spara certifikatfilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="64c79-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_certificate.png) 

5. <span data-ttu-id="64c79-161">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="64c79-161">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="64c79-163">På den **anpassningsbar Suite Configuration** klickar du på **konfigurera anpassningsbar Suite** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="64c79-163">On the **Adaptive Suite Configuration** section, click **Configure Adaptive Suite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="64c79-164">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="64c79-164">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_configure.png) 

7. <span data-ttu-id="64c79-166">I en annan webbläsarfönster loggar du in på webbplatsen anpassningsbar Suite företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="64c79-166">In a different web browser window, log in to your Adaptive Suite company site as an administrator.</span></span>

8. <span data-ttu-id="64c79-167">Gå till **Admin**.</span><span class="sxs-lookup"><span data-stu-id="64c79-167">Go to **Admin**.</span></span>
   
    <span data-ttu-id="64c79-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="64c79-168">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>

9. <span data-ttu-id="64c79-169">I den **användare och roller** klickar du på **hantera inställningar för SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="64c79-169">In the **Users and Roles** section, click **Manage SAML SSO Settings**.</span></span>
   
    <span data-ttu-id="64c79-170">![Hantera inställningar för SAML SSO](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "hantera SAML SSO-inställningar")</span><span class="sxs-lookup"><span data-stu-id="64c79-170">![Manage SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805645.png "Manage SAML SSO Settings")</span></span>

10. <span data-ttu-id="64c79-171">På den **SAML SSO inställningar** utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="64c79-171">On the **SAML SSO Settings** page, perform the following steps:</span></span>
   
    <span data-ttu-id="64c79-172">![Inställningar för SAML SSO](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO-inställningar")</span><span class="sxs-lookup"><span data-stu-id="64c79-172">![SAML SSO Settings](./media/active-directory-saas-adaptivesuite-tutorial/IC805646.png "SAML SSO Settings")</span></span>

    <span data-ttu-id="64c79-173">a.</span><span class="sxs-lookup"><span data-stu-id="64c79-173">a.</span></span> <span data-ttu-id="64c79-174">I den **identitet providernamn** textruta, ange ett namn för din konfiguration.</span><span class="sxs-lookup"><span data-stu-id="64c79-174">In the **Identity provider name** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="64c79-175">b.</span><span class="sxs-lookup"><span data-stu-id="64c79-175">b.</span></span> <span data-ttu-id="64c79-176">Klistra in den **SAML enhets-ID** kopieras värdet från Azure-portalen i den **identitetsleverantör enhets-ID** textruta.</span><span class="sxs-lookup"><span data-stu-id="64c79-176">Paste the **SAML Entity ID** value copied from Azure portal into the **Identity provider Entity ID** textbox.</span></span>
  
    <span data-ttu-id="64c79-177">c.</span><span class="sxs-lookup"><span data-stu-id="64c79-177">c.</span></span> <span data-ttu-id="64c79-178">Klistra in den **SAML inloggning tjänst-URL för enkel** kopieras värdet från Azure-portalen i den **identitetsleverantör SSO URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="64c79-178">Paste the **SAML Single Sign-On Service URL** value copied from Azure portal into the **Identity provider SSO URL** textbox.</span></span>
  
    <span data-ttu-id="64c79-179">d.</span><span class="sxs-lookup"><span data-stu-id="64c79-179">d.</span></span> <span data-ttu-id="64c79-180">Klistra in den **SAML enkel inloggning Tjänstwebbadress** kopieras värdet från Azure-portalen i den **anpassad logga ut URL** textruta.</span><span class="sxs-lookup"><span data-stu-id="64c79-180">Paste the **SAML Single Sign-On Service URL** value copied from Azure portal into the **Custom logout URL** textbox.</span></span>
  
    <span data-ttu-id="64c79-181">e.</span><span class="sxs-lookup"><span data-stu-id="64c79-181">e.</span></span> <span data-ttu-id="64c79-182">Om du vill överföra din hämtat certifikat klickar du på **Välj fil**.</span><span class="sxs-lookup"><span data-stu-id="64c79-182">To upload your downloaded certificate, click **Choose file**.</span></span>
  
    <span data-ttu-id="64c79-183">f.</span><span class="sxs-lookup"><span data-stu-id="64c79-183">f.</span></span> <span data-ttu-id="64c79-184">Välj följande, för:</span><span class="sxs-lookup"><span data-stu-id="64c79-184">Select the following, for:</span></span>
    * <span data-ttu-id="64c79-185">**Användar-id för SAML**väljer **användarens användarnamn för anpassningsbar insikter**.</span><span class="sxs-lookup"><span data-stu-id="64c79-185">**SAML user id**, select **User’s Adaptive Insights user name**.</span></span>
    * <span data-ttu-id="64c79-186">**Id för SAML Användarplats**väljer **användar-id i NameID ämne**.</span><span class="sxs-lookup"><span data-stu-id="64c79-186">**SAML user id location**, select **User id in NameID of Subject**.</span></span>
    * <span data-ttu-id="64c79-187">**Format för SAML-NameID**väljer **e-postadress**.</span><span class="sxs-lookup"><span data-stu-id="64c79-187">**SAML NameID format**, select **Email address**.</span></span>
    * <span data-ttu-id="64c79-188">**Aktivera SAML**väljer **Tillåt SAML SSO och anpassningsbar insikter direktinloggning**.</span><span class="sxs-lookup"><span data-stu-id="64c79-188">**Enable SAML**, select **Allow SAML SSO and direct Adaptive Insights login**.</span></span>
    
    <span data-ttu-id="64c79-189">g.</span><span class="sxs-lookup"><span data-stu-id="64c79-189">g.</span></span> <span data-ttu-id="64c79-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="64c79-190">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="64c79-191">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="64c79-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="64c79-192">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="64c79-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="64c79-193">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="64c79-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="64c79-194">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="64c79-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="64c79-195">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="64c79-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="64c79-197">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="64c79-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="64c79-198">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="64c79-198">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="64c79-200">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="64c79-200">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="64c79-202">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64c79-202">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="64c79-204">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="64c79-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-adaptivesuite-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="64c79-206">a.</span><span class="sxs-lookup"><span data-stu-id="64c79-206">a.</span></span> <span data-ttu-id="64c79-207">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="64c79-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="64c79-208">b.</span><span class="sxs-lookup"><span data-stu-id="64c79-208">b.</span></span> <span data-ttu-id="64c79-209">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="64c79-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="64c79-210">c.</span><span class="sxs-lookup"><span data-stu-id="64c79-210">c.</span></span> <span data-ttu-id="64c79-211">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="64c79-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="64c79-212">d.</span><span class="sxs-lookup"><span data-stu-id="64c79-212">d.</span></span> <span data-ttu-id="64c79-213">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="64c79-213">Click **Create**.</span></span>
 
### <a name="creating-an-adaptive-suite-test-user"></a><span data-ttu-id="64c79-214">Skapa en anpassningsbar Suite testanvändare</span><span class="sxs-lookup"><span data-stu-id="64c79-214">Creating an Adaptive Suite test user</span></span>

<span data-ttu-id="64c79-215">Om du vill aktivera Azure AD-användare kan logga in på anpassningsbar Suite etableras de i anpassningsbar Suite.</span><span class="sxs-lookup"><span data-stu-id="64c79-215">To enable Azure AD users to log in to Adaptive Suite, they must be provisioned into Adaptive Suite.</span></span>  

* <span data-ttu-id="64c79-216">Anpassningsbar Suite är etablering en manuell aktivitet.</span><span class="sxs-lookup"><span data-stu-id="64c79-216">In the case of Adaptive Suite, provisioning is a manual task.</span></span>

<span data-ttu-id="64c79-217">**Utför följande steg för att konfigurera användaretablering:**</span><span class="sxs-lookup"><span data-stu-id="64c79-217">**To configure user provisioning, perform the following steps:**</span></span> 

1. <span data-ttu-id="64c79-218">Logga in på ditt **anpassningsbar Suite** företagets webbplats som administratör.</span><span class="sxs-lookup"><span data-stu-id="64c79-218">Log in to your **Adaptive Suite** company site as an administrator.</span></span>
2. <span data-ttu-id="64c79-219">Gå till **Admin**.</span><span class="sxs-lookup"><span data-stu-id="64c79-219">Go to **Admin**.</span></span>
   
   <span data-ttu-id="64c79-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span><span class="sxs-lookup"><span data-stu-id="64c79-220">![Admin](./media/active-directory-saas-adaptivesuite-tutorial/IC805644.png "Admin")</span></span>
3. <span data-ttu-id="64c79-221">I den **användare och roller** klickar du på **Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="64c79-221">In the **Users and Roles** section, click **Add User**.</span></span>
   
   <span data-ttu-id="64c79-222">![Lägg till användare](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "lägga till användare")</span><span class="sxs-lookup"><span data-stu-id="64c79-222">![Add User](./media/active-directory-saas-adaptivesuite-tutorial/IC805648.png "Add User")</span></span>
4. <span data-ttu-id="64c79-223">I den **ny användare** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="64c79-223">In the **New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="64c79-224">![Skicka](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "skicka")</span><span class="sxs-lookup"><span data-stu-id="64c79-224">![Submit](./media/active-directory-saas-adaptivesuite-tutorial/IC805649.png "Submit")</span></span>   

   <span data-ttu-id="64c79-225">a.</span><span class="sxs-lookup"><span data-stu-id="64c79-225">a.</span></span> <span data-ttu-id="64c79-226">Typ av **namn**, **inloggning**, **e-post**, **lösenord** av en giltig Azure Active Directory-användare som du vill etablera i relaterade textrutor.</span><span class="sxs-lookup"><span data-stu-id="64c79-226">Type the **Name**, **Login**, **Email**, **Password** of a valid Azure Active Directory user you want to provision into the related textboxes.</span></span>
  
   <span data-ttu-id="64c79-227">b.</span><span class="sxs-lookup"><span data-stu-id="64c79-227">b.</span></span> <span data-ttu-id="64c79-228">Välj en **rollen**.</span><span class="sxs-lookup"><span data-stu-id="64c79-228">Select a **Role**.</span></span>
  
   <span data-ttu-id="64c79-229">c.</span><span class="sxs-lookup"><span data-stu-id="64c79-229">c.</span></span> <span data-ttu-id="64c79-230">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="64c79-230">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="64c79-231">Du kan använda något annat anpassningsbar Suite användarens konto skapas verktyg eller API: er som tillhandahålls av anpassningsbar Suite etablera AAD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="64c79-231">You can use any other Adaptive Suite user account creation tools or APIs provided by Adaptive Suite to provision AAD user accounts.</span></span>
>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="64c79-232">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="64c79-232">Assigning the Azure AD test user</span></span>

<span data-ttu-id="64c79-233">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till anpassningsbar Suite.</span><span class="sxs-lookup"><span data-stu-id="64c79-233">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Adaptive Suite.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="64c79-235">**Om du vill tilldela anpassningsbar Suite Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="64c79-235">**To assign Britta Simon to Adaptive Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="64c79-236">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="64c79-236">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="64c79-238">Välj i listan med program **anpassningsbar Suite**.</span><span class="sxs-lookup"><span data-stu-id="64c79-238">In the applications list, select **Adaptive Suite**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-adaptivesuite-tutorial/tutorial_adaptivesuite_app.png) 

3. <span data-ttu-id="64c79-240">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="64c79-240">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="64c79-242">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="64c79-242">Click **Add** button.</span></span> <span data-ttu-id="64c79-243">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64c79-243">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="64c79-245">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="64c79-245">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="64c79-246">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64c79-246">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="64c79-247">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="64c79-247">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="64c79-248">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="64c79-248">Testing single sign-on</span></span>

<span data-ttu-id="64c79-249">Syftet med det här avsnittet är att testa Microsoft Azure AD Single Sign-On-konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="64c79-249">The objective of this section is to test your Microsoft Azure AD Single Sign-On configuration using the Access Panel.</span></span>

<span data-ttu-id="64c79-250">När du klickar på panelen anpassningsbar Suite på åtkomstpanelen du bör få automatiskt loggat in på ditt anpassningsbar Suite-program.</span><span class="sxs-lookup"><span data-stu-id="64c79-250">When you click the Adaptive Suite tile in the Access Panel, you should get automatically signed-on to your Adaptive Suite application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="64c79-251">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="64c79-251">Additional resources</span></span>

* [<span data-ttu-id="64c79-252">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="64c79-252">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="64c79-253">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="64c79-253">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adaptivesuite-tutorial/tutorial_general_203.png

