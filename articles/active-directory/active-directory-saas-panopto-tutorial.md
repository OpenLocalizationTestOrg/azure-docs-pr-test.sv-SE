---
title: "Självstudier: Azure Active Directory-integrering med Panopto | Microsoft Docs"
description: "Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och Panopto."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 89c88e23-93ce-4970-9baa-1104c4e8fe4a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 725fba1227cfc9c4850f9e2d6fd0b13e88eafa20
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-panopto"></a><span data-ttu-id="43003-103">Självstudier: Azure Active Directory-integrering med Panopto</span><span class="sxs-lookup"><span data-stu-id="43003-103">Tutorial: Azure Active Directory integration with Panopto</span></span>

<span data-ttu-id="43003-104">I kursen får lära du att integrera Panopto med Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="43003-104">In this tutorial, you learn how to integrate Panopto with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="43003-105">Integrera Panopto med Azure AD ger dig följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="43003-105">Integrating Panopto with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="43003-106">Du kan styra i Azure AD som har åtkomst till Panopto</span><span class="sxs-lookup"><span data-stu-id="43003-106">You can control in Azure AD who has access to Panopto</span></span>
- <span data-ttu-id="43003-107">Du kan aktivera användarna att automatiskt hämta loggat in på Panopto (Single Sign-On) med sina Azure AD-konton</span><span class="sxs-lookup"><span data-stu-id="43003-107">You can enable your users to automatically get signed-on to Panopto (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="43003-108">Du kan hantera dina konton i en central plats - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="43003-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="43003-109">Om du vill veta mer information om integrering av SaaS-app med Azure AD finns [vad är programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="43003-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43003-110">Krav</span><span class="sxs-lookup"><span data-stu-id="43003-110">Prerequisites</span></span>

<span data-ttu-id="43003-111">För att konfigurera Azure AD-integrering med Panopto, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="43003-111">To configure Azure AD integration with Panopto, you need the following items:</span></span>

- <span data-ttu-id="43003-112">En Azure AD-prenumeration</span><span class="sxs-lookup"><span data-stu-id="43003-112">An Azure AD subscription</span></span>
- <span data-ttu-id="43003-113">En Panopto enkel inloggning aktiverad prenumeration</span><span class="sxs-lookup"><span data-stu-id="43003-113">A Panopto single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="43003-114">Om du vill testa stegen i den här kursen rekommenderar vi inte med hjälp av en produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="43003-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="43003-115">Om du vill testa stegen i den här självstudiekursen, bör du följa dessa rekommendationer:</span><span class="sxs-lookup"><span data-stu-id="43003-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="43003-116">Använd inte i produktionsmiljön, om det är nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="43003-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="43003-117">Om du inte har en utvärderingsversion Azure AD-miljö kan du hämta en utvärderingsversion för en månad [här](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="43003-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="43003-118">Scenariobeskrivning</span><span class="sxs-lookup"><span data-stu-id="43003-118">Scenario description</span></span>
<span data-ttu-id="43003-119">I kursen får testa du Azure AD enkel inloggning i en testmiljö.</span><span class="sxs-lookup"><span data-stu-id="43003-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="43003-120">Det scenario som beskrivs i den här kursen består av två huvudsakliga byggblock:</span><span class="sxs-lookup"><span data-stu-id="43003-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="43003-121">Att lägga till Panopto från galleriet</span><span class="sxs-lookup"><span data-stu-id="43003-121">Adding Panopto from the gallery</span></span>
2. <span data-ttu-id="43003-122">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43003-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-panopto-from-the-gallery"></a><span data-ttu-id="43003-123">Att lägga till Panopto från galleriet</span><span class="sxs-lookup"><span data-stu-id="43003-123">Adding Panopto from the gallery</span></span>
<span data-ttu-id="43003-124">Du måste lägga till Panopto från galleriet i listan över hanterade SaaS-appar för att konfigurera integrering av Panopto i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="43003-124">To configure the integration of Panopto into Azure AD, you need to add Panopto from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="43003-125">**Utför följande steg för att lägga till Panopto från galleriet:**</span><span class="sxs-lookup"><span data-stu-id="43003-125">**To add Panopto from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="43003-126">I den  **[Azure-portalen](https://portal.azure.com)**, klicka på den vänstra navigeringspanelen **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43003-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="43003-128">Gå till **företagsprogram**.</span><span class="sxs-lookup"><span data-stu-id="43003-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="43003-129">Gå till **alla program**.</span><span class="sxs-lookup"><span data-stu-id="43003-129">Then go to **All applications**.</span></span>

    ![Program][2]
    
3. <span data-ttu-id="43003-131">Om du vill lägga till nya programmet, klickar du på **nytt program** knappen överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43003-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Program][3]

4. <span data-ttu-id="43003-133">I sökrutan skriver **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="43003-133">In the search box, type **Panopto**.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_search.png)

5. <span data-ttu-id="43003-135">Välj i resultatpanelen **Panopto**, och klicka sedan på **Lägg till** för att lägga till programmet.</span><span class="sxs-lookup"><span data-stu-id="43003-135">In the results panel, select **Panopto**, and then click **Add** button to add the application.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="43003-137">Konfigurera och testa Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43003-137">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="43003-138">I det här avsnittet kan du konfigurera och testa Azure AD enkel inloggning med Panopto baserat på en testanvändare som kallas ”Britta Simon”.</span><span class="sxs-lookup"><span data-stu-id="43003-138">In this section, you configure and test Azure AD single sign-on with Panopto based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="43003-139">Azure AD måste du känna till användaren i Panopto motsvarighet till en användare i Azure AD för enkel inloggning ska fungera.</span><span class="sxs-lookup"><span data-stu-id="43003-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Panopto is to a user in Azure AD.</span></span> <span data-ttu-id="43003-140">Med andra ord måste en länk förhållandet mellan en Azure AD-användare och relaterade användaren i Panopto upprättas.</span><span class="sxs-lookup"><span data-stu-id="43003-140">In other words, a link relationship between an Azure AD user and the related user in Panopto needs to be established.</span></span>

<span data-ttu-id="43003-141">I Panopto, tilldela värdet för den **användarnamn** i Azure AD som värde för den **användarnamn** etablera länken relationen.</span><span class="sxs-lookup"><span data-stu-id="43003-141">In Panopto, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="43003-142">Om du vill konfigurera och testa Azure AD enkel inloggning med Panopto, måste du utföra följande byggblock:</span><span class="sxs-lookup"><span data-stu-id="43003-142">To configure and test Azure AD single sign-on with Panopto, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="43003-143">**[Konfigurera Azure AD enkel inloggning](#configuring-azure-ad-single-sign-on)**  - om du vill att användarna kan använda den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="43003-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="43003-144">**[Skapa en Azure AD-testanvändare](#creating-an-azure-ad-test-user)**  - om du vill testa Azure AD enkel inloggning med Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43003-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="43003-145">**[Skapa en testanvändare Panopto](#creating-a-panopto-test-user)**  – du har en motsvarighet för Britta Simon i Panopto som är kopplad till Azure AD-representation av användaren.</span><span class="sxs-lookup"><span data-stu-id="43003-145">**[Creating a Panopto test user](#creating-a-panopto-test-user)** - to have a counterpart of Britta Simon in Panopto that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="43003-146">**[Tilldela Azure AD-testanvändare](#assigning-the-azure-ad-test-user)**  - om du vill aktivera Britta Simon att använda Azure AD enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43003-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="43003-147">**[Testa enkel inloggning](#testing-single-sign-on)**  - om du vill kontrollera om konfigurationen fungerar.</span><span class="sxs-lookup"><span data-stu-id="43003-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="43003-148">Konfigurera Azure AD enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43003-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="43003-149">I det här avsnittet Aktivera Azure AD enkel inloggning i Azure-portalen och konfigurera enkel inloggning i ditt Panopto program.</span><span class="sxs-lookup"><span data-stu-id="43003-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Panopto application.</span></span>

<span data-ttu-id="43003-150">**Utför följande steg för att konfigurera Azure AD enkel inloggning med Panopto:**</span><span class="sxs-lookup"><span data-stu-id="43003-150">**To configure Azure AD single sign-on with Panopto, perform the following steps:**</span></span>

1. <span data-ttu-id="43003-151">I Azure-portalen på den **Panopto** integreringssidan för programmet, klickar du på **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="43003-151">In the Azure portal, on the **Panopto** application integration page, click **Single sign-on**.</span></span>

    ![Konfigurera enkel inloggning][4]

2. <span data-ttu-id="43003-153">På den **enkel inloggning** markerar **läge** som **SAML-baserade inloggning** att aktivera enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="43003-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Konfigurera enkel inloggning](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_samlbase.png)

3. <span data-ttu-id="43003-155">På den **Panopto domän och URL: er** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43003-155">On the **Panopto Domain and URLs** section, perform the following steps:</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_url.png)

    <span data-ttu-id="43003-157">I den **inloggnings-URL** textruta Skriv en URL med följande mönster:`https://<tenant-name>.panopto.com`</span><span class="sxs-lookup"><span data-stu-id="43003-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenant-name>.panopto.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="43003-158">Det här värdet är inte verkliga.</span><span class="sxs-lookup"><span data-stu-id="43003-158">This value is not real.</span></span> <span data-ttu-id="43003-159">Uppdatera det här värdet med det faktiska inloggnings-URL.</span><span class="sxs-lookup"><span data-stu-id="43003-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="43003-160">Kontakta [Panopto klienten supportteamet](mailto:support@panopto.com‎) att hämta det här värdet.</span><span class="sxs-lookup"><span data-stu-id="43003-160">Contact [Panopto Client support team](mailto:support@panopto.com‎) to get this value.</span></span> 
 
4. <span data-ttu-id="43003-161">På den **SAML-signeringscertifikat** klickar du på **XML-Metadata för** och spara sedan metadatafilen på datorn.</span><span class="sxs-lookup"><span data-stu-id="43003-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_certificate.png) 

5. <span data-ttu-id="43003-163">Klicka på **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="43003-163">Click **Save** button.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panopto-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="43003-165">På den **Panopto Configuration** klickar du på **konfigurera Panopto** att öppna **konfigurera inloggning** fönster.</span><span class="sxs-lookup"><span data-stu-id="43003-165">On the **Panopto Configuration** section, click **Configure Panopto** to open **Configure sign-on** window.</span></span> <span data-ttu-id="43003-166">Kopiera den **SAML enhets-ID och SAML enkel inloggning Tjänstwebbadress** från den **Snabbreferens avsnitt.**</span><span class="sxs-lookup"><span data-stu-id="43003-166">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_configure.png) 

7. <span data-ttu-id="43003-168">I en annan webbläsarfönster loggar du in på webbplatsen Panopto företag som administratör.</span><span class="sxs-lookup"><span data-stu-id="43003-168">In a different web browser window, log in to your Panopto company site as an administrator.</span></span>

8. <span data-ttu-id="43003-169">Klicka på i verktygsfältet på vänster **System**, och klicka sedan på **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="43003-169">In the toolbar on the left, click **System**, and then click **Identity Providers**.</span></span>
   
   <span data-ttu-id="43003-170">![System](./media/active-directory-saas-panopto-tutorial/ic777670.png "System")</span><span class="sxs-lookup"><span data-stu-id="43003-170">![System](./media/active-directory-saas-panopto-tutorial/ic777670.png "System")</span></span>
9. <span data-ttu-id="43003-171">Klicka på **Lägg till Provider**.</span><span class="sxs-lookup"><span data-stu-id="43003-171">Click **Add Provider**.</span></span>
   
   <span data-ttu-id="43003-172">![Identitetsleverantörer](./media/active-directory-saas-panopto-tutorial/ic777671.png "identitetsleverantörer")</span><span class="sxs-lookup"><span data-stu-id="43003-172">![Identity Providers](./media/active-directory-saas-panopto-tutorial/ic777671.png "Identity Providers")</span></span>
   
10. <span data-ttu-id="43003-173">Utför följande steg i avsnittet SAML-provider:</span><span class="sxs-lookup"><span data-stu-id="43003-173">In the SAML provider section, perform the following steps:</span></span>
   
    <span data-ttu-id="43003-174">![Konfiguration av SaaS](./media/active-directory-saas-panopto-tutorial/ic777672.png "SaaS-konfiguration")</span><span class="sxs-lookup"><span data-stu-id="43003-174">![SaaS configuration](./media/active-directory-saas-panopto-tutorial/ic777672.png "SaaS configuration")</span></span>
    
    <span data-ttu-id="43003-175">a.</span><span class="sxs-lookup"><span data-stu-id="43003-175">a.</span></span> <span data-ttu-id="43003-176">Från den **providertyp** väljer **SAML20**.</span><span class="sxs-lookup"><span data-stu-id="43003-176">From the **Provider Type** list, select **SAML20**.</span></span>    
    
    <span data-ttu-id="43003-177">b.</span><span class="sxs-lookup"><span data-stu-id="43003-177">b.</span></span> <span data-ttu-id="43003-178">I den **instansnamn** textruta, ange ett namn för instansen.</span><span class="sxs-lookup"><span data-stu-id="43003-178">In the **Instance Name** textbox, type a name for the instance.</span></span>

    <span data-ttu-id="43003-179">c.</span><span class="sxs-lookup"><span data-stu-id="43003-179">c.</span></span> <span data-ttu-id="43003-180">I den **beskrivning** textruta Skriv en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="43003-180">In the **Friendly Description** textbox, type a friendly description.</span></span>
    
    <span data-ttu-id="43003-181">d.</span><span class="sxs-lookup"><span data-stu-id="43003-181">d.</span></span> <span data-ttu-id="43003-182">I **studs sidadress** textruta klistra in värdet för **SAML inloggning tjänst-URL för enkel**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="43003-182">In **Bounce Page Url** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="43003-183">e.</span><span class="sxs-lookup"><span data-stu-id="43003-183">e.</span></span> <span data-ttu-id="43003-184">I den **utfärdaren** textruta klistra in värdet för **SAML enhets-ID**, som du har kopierat från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="43003-184">In the **Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="43003-185">f.</span><span class="sxs-lookup"><span data-stu-id="43003-185">f.</span></span> <span data-ttu-id="43003-186">Öppna Base64-kodade certifikatet, som du har hämtat från Azure-portalen, kopiera innehållet i den till Urklipp och klistra in den till den **offentliga nyckel** textruta.</span><span class="sxs-lookup"><span data-stu-id="43003-186">Open your base-64 encoded certificate, which you have downloaded from Azure portal, copy the content of it in to your clipboard, and then paste it to the **Public Key**  textbox.</span></span>

11. <span data-ttu-id="43003-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="43003-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="43003-188">Du kan nu läsa en kortare version av instruktionerna i den [Azure-portalen](https://portal.azure.com), medan du installerar appen!</span><span class="sxs-lookup"><span data-stu-id="43003-188">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="43003-189">När du lägger till den här appen från den **Active Directory > företagsprogram** avsnittet, klickar du på den **enkel inloggning** fliken och få åtkomst till den inbäddade dokumentationen via den **Configuration** avsnittet längst ned.</span><span class="sxs-lookup"><span data-stu-id="43003-189">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="43003-190">Du kan läsa mer om funktionen inbäddade dokumentationen här: [inbäddade dokumentation för Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="43003-190">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="43003-191">Skapa en testanvändare i Azure AD</span><span class="sxs-lookup"><span data-stu-id="43003-191">Creating an Azure AD test user</span></span>

<span data-ttu-id="43003-192">Syftet med det här avsnittet är att skapa en testanvändare i Azure-portalen kallas Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="43003-192">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Skapa Azure AD-användare][100]

<span data-ttu-id="43003-194">**Utför följande steg för att skapa en testanvändare i Azure AD:**</span><span class="sxs-lookup"><span data-stu-id="43003-194">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="43003-195">I den **Azure-portalen**, klicka på det vänstra navigeringsfönstret **Azure Active Directory** ikon.</span><span class="sxs-lookup"><span data-stu-id="43003-195">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="43003-197">Om du vill visa en lista över användare, gå till **användare och grupper** och på **alla användare**.</span><span class="sxs-lookup"><span data-stu-id="43003-197">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="43003-199">Öppna den **användare** dialogrutan klickar du på **Lägg till** överst i dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43003-199">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="43003-201">På den **användaren** dialogrutan utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="43003-201">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Skapa en testanvändare i Azure AD](./media/active-directory-saas-panopto-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="43003-203">a.</span><span class="sxs-lookup"><span data-stu-id="43003-203">a.</span></span> <span data-ttu-id="43003-204">I den **namn** textruta typen **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="43003-204">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="43003-205">b.</span><span class="sxs-lookup"><span data-stu-id="43003-205">b.</span></span> <span data-ttu-id="43003-206">I den **användarnamn** textruta typ av **e-postadress** av BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="43003-206">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="43003-207">c.</span><span class="sxs-lookup"><span data-stu-id="43003-207">c.</span></span> <span data-ttu-id="43003-208">Välj **visa lösenordet** och anteckna värdet för den **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="43003-208">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="43003-209">d.</span><span class="sxs-lookup"><span data-stu-id="43003-209">d.</span></span> <span data-ttu-id="43003-210">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="43003-210">Click **Create**.</span></span>
 
### <a name="creating-a-panopto-test-user"></a><span data-ttu-id="43003-211">Skapa en testanvändare Panopto</span><span class="sxs-lookup"><span data-stu-id="43003-211">Creating a Panopto test user</span></span>

<span data-ttu-id="43003-212">Det finns inga uppgiften som du kan konfigurera användaretablering till Panopto.</span><span class="sxs-lookup"><span data-stu-id="43003-212">There is no action item for you to configure user provisioning to Panopto.</span></span>  
<span data-ttu-id="43003-213">När en tilldelad användare försöker logga in med hjälp av åtkomstpanelen Panopto kontrollerar Panopto om användaren finns.</span><span class="sxs-lookup"><span data-stu-id="43003-213">When an assigned user tries to log in to Panopto using the access panel, Panopto checks whether the user exists.</span></span>  

<span data-ttu-id="43003-214">Om det finns inget användarkonto ännu, skapas den automatiskt av Panopto.</span><span class="sxs-lookup"><span data-stu-id="43003-214">If there is no user account available yet, it is automatically created by Panopto.</span></span>

>[!NOTE]
><span data-ttu-id="43003-215">Du kan använda något annat Panopto användarens konto skapas verktyg eller API: er som tillhandahålls av Panopto att etablera Azure AD-användarkonton.</span><span class="sxs-lookup"><span data-stu-id="43003-215">You can use any other Panopto user account creation tools or APIs provided by Panopto to provision Azure AD user accounts.</span></span>
>
>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="43003-216">Tilldela Azure AD-testanvändare</span><span class="sxs-lookup"><span data-stu-id="43003-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="43003-217">I det här avsnittet kan du aktivera Britta Simon att använda Azure enkel inloggning genom att bevilja åtkomst till Panopto.</span><span class="sxs-lookup"><span data-stu-id="43003-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Panopto.</span></span>

![Tilldela användare][200] 

<span data-ttu-id="43003-219">**Om du vill tilldela Panopto Britta Simon utför du följande steg:**</span><span class="sxs-lookup"><span data-stu-id="43003-219">**To assign Britta Simon to Panopto, perform the following steps:**</span></span>

1. <span data-ttu-id="43003-220">Öppna vyn program i Azure-portalen och gå till vyn directory och gå till **företagsprogram** Klicka **alla program**.</span><span class="sxs-lookup"><span data-stu-id="43003-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Tilldela användare][201] 

2. <span data-ttu-id="43003-222">Välj i listan med program **Panopto**.</span><span class="sxs-lookup"><span data-stu-id="43003-222">In the applications list, select **Panopto**.</span></span>

    ![Konfigurera enkel inloggning](./media/active-directory-saas-panopto-tutorial/tutorial_panopto_app.png) 

3. <span data-ttu-id="43003-224">Klicka på menyn till vänster **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="43003-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Tilldela användare][202] 

4. <span data-ttu-id="43003-226">Klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="43003-226">Click **Add** button.</span></span> <span data-ttu-id="43003-227">Välj sedan **användare och grupper** på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43003-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Tilldela användare][203]

5. <span data-ttu-id="43003-229">På **användare och grupper** markerar **Britta Simon** på listan användare.</span><span class="sxs-lookup"><span data-stu-id="43003-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="43003-230">Klicka på **Välj** knappen på **användare och grupper** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43003-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="43003-231">Klicka på **tilldela** knappen på **Lägg uppdrag** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="43003-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="43003-232">Testa enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="43003-232">Testing single sign-on</span></span>

<span data-ttu-id="43003-233">I det här avsnittet kan du testa Azure AD enkel inloggning konfigurationen med hjälp av panelen åtkomst.</span><span class="sxs-lookup"><span data-stu-id="43003-233">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="43003-234">När du klickar på panelen Panopto på åtkomstpanelen ska du automatiskt hämta inloggningssidan för Panopto program.</span><span class="sxs-lookup"><span data-stu-id="43003-234">When you click the Panopto tile in the Access Panel, you should get automatically login page of Panopto application.</span></span>
<span data-ttu-id="43003-235">Läs mer om åtkomstpanelen [introduktion till åtkomstpanelen](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="43003-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43003-236">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="43003-236">Additional resources</span></span>

* [<span data-ttu-id="43003-237">Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="43003-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="43003-238">Vad är programåtkomst och enkel inloggning med Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="43003-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-panopto-tutorial/tutorial_general_203.png

